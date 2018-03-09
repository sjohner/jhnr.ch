---
author: sjohner
comments: true
date: 2014-02-17 21:08:20+00:00
layout: post
slug: customize-service-manager-user-profiles-by-using-c-and-sdk
title: Customize Service Manager User Profiles by using C# and SDK
wordpress_id: 847
categories:
- Service Manager
tags:
- C#
- Incident
- Management Group
- PowerShell
- Relationship
- SDK
- User Profile
- User Role
- Visual Studio
---

As you may know, Service Manager uses so called _user roles_ to allow access to the application. A user which is member of a specific user role is able to perform tasks associated to this user role. As [Kurt van Hoecke](http://twitter.com/BunkCo) describes in [one of his posts](http://scug.be/scsm/2010/03/21/service-manager-role-based-security-scoping/), each user role contains a specific _user profile_ which defines which operations on an object are allowed. Existing work item classes like Incidents and Service Requests do have a default user profile already defined. As long as you use the out of the box Service Manager work item classes you will probably never have to mess around with user profiles. However, when you are going to customize one of the existing classes or if you create new work item classes and want to have tight control over access to objects of this class, you will have to deal with user profiles.

For instance, whenever you create a custom relationship for the default Incident class, you must grant permissions to use this relationship if the user is not member of the Advanced Operators or Administrators user role. You will have to grant _Object__Set_ (edit) permissions to the user profiles that need to be able to save information to your newly created relationship. In Service Manager 2012, this can be done by using the Service Manager SDK<!-- more -->

[Marcel Zehner](http://twitter.com/marcelzehner) wrote a great [post](http://marcelzehner.ch/2014/01/07/scsm-user-role-customization-using-powershell-and-sdk-part-1/) about how to extend a user profile to cover a custom work item class by using PowerShell and the Service Manager SDK. In a [second part of this post](http://marcelzehner.ch/2014/01/12/scsm-user-role-customization-using-powershell-and-sdk-part-2/) he also covers how to extend the user profile to allow editing of relationships.

What I would like to show you now is how to extend a user profile to allow editing of custom relationships by using some C# magic :-)

First of all you have to include the following references in your Visual Studio project:

    
    //Contains RegistryKey
    using Microsoft.Win32;
    
    //Requires Microsoft.EnterpriseManagement.Core reference
    //Contains EnterpriseManagementGroup
    using Microsoft.EnterpriseManagement;
    //EnterpriseManagementObject
    using Microsoft.EnterpriseManagement.Common;
    //ManagementPack, ManagementPackClass
    using Microsoft.EnterpriseManagement.Configuration;
    
    //Requires Microsoft.EnterpriseManagement.UI.Foundation reference
    //Contains IDataItem
    using Microsoft.EnterpriseManagement.UI.DataModel;
    
    //Requires Microsoft.EnterpriseManagement.UI.Extensions reference
    //ConsoleContextHelper
    using Microsoft.EnterpriseManagement.UI.Extensions.Shared;
    using Microsoft.EnterpriseManagement.Security;


The assemblies referenced above can be found in Service Manager installation directory. Afterwards a connection to your Service Manager Management Group has to established. For example you can use the following code to create a EnterpriseManagementGroup object.

    
    EnterpriseManagementGroup emg = null;
    
    //Get SDK Server from Registry and create connection to Management Group based on this value
    try
    {
        RegistryKey scsmRegistryKey = Registry.CurrentUser.OpenSubKey(@&quot;SoftwareMicrosoftSystem Center2010Service ManagerConsoleUser Settings&quot;);
        string scsmSDKServer = &quot;&quot;;
    
        if (scsmRegistryKey != null)
        {
            scsmSDKServer = scsmRegistryKey.GetValue(&quot;SDKServiceMachine&quot;).ToString();
        }
        else
        {
            scsmSDKServer = &quot;localhost&quot;;
        }
        EnterpriseManagementConnectionSettings connectionSettings = new EnterpriseManagementConnectionSettings(scsmSDKServer);
        emg = new EnterpriseManagementGroup(connectionSettings);
    }
    catch (ServiceNotRunningException)
    {
        Console.WriteLine(&quot;n**** ERROR: Cannot connect to System Center Data Access Service ****&quot;);
        //Wait for user to exit
        Console.Read();
        Environment.Exit(0);
    }


Now you have to get the profile for which you want to give access to edit (Object__Set operation) the relationship. In this example I am going to edit the _Incident Resolver_ profile. You will need the class and relationship GUID’s affected by your change to the user profile. In case you added a custom relationship to the default Incident class, you will have to get the Incident class GUID as well as the GUID of your custom relationship.

    
    Profile profile = emg.Security.GetProfile(Profile.IncidentResolverProfileId);
    Guid classGuid = new Guid(&quot;a604b942-4c7b-2fb2-28dc-61dc6f465c68&quot;);
    Guid relGuid = new Guid(&quot;5ece919d-f09f-41ae-a090-1456b010d1b2&quot;);
    //Locate the operation in the profile that will be enhanced
    Operation opObjectSet = profile.Operations.Where(op =&gt; op.Name == &quot;Object__Set&quot;).FirstOrDefault();
    if (opObjectSet != null)
    {
            try
            {
                Console.WriteLine(&quot;nEnhancing Operation {0}&quot;, opObjectSet.Name);
    
                //Extend the operation’s type restrictions.
                OperationImplicitScope relationshipScope = new OperationImplicitScope(classGuid, relGuid, RelationshipEndpoint.Unset);
                opObjectSet.ImplicitScopes.Add(relationshipScope);
                //Update profile
                profile.Update();
            }
            catch (ProfileOperationImplicitScopeAlreadyExistsException)
            {
                Console.WriteLine(&quot;n**** ERROR: Profile operation scopes already exist with the same broader rights. ****&quot;);
            }
    }


You can find more information about the _OperationImplicitScope_ class on [MSDN](http://msdn.microsoft.com/en-us/library/hh964378.aspx). One useful property of this class is _IsCustomized._ This property allows you to easily find all custom scopes for a user profile. For instance you can use the following code sample to find all custom scopes for the Incident Resolver user profile.

    
    //Locate the operation in the profile that will be queried
    Operation opObjectSet = profile.Operations.Where(op =&gt; op.Name == &quot;Object__Set&quot;).FirstOrDefault();
    if (opObjectSet != null)
    {
        List&lt;OperationImplicitScope&gt; opImplicitScopesCustomized = new List&lt;OperationImplicitScope&gt;();
    
        //Get scopes which have IsCustomized flag set
        foreach (OperationImplicitScope opImplicitScope in opObjectSet.ImplicitScopes)
        {
            if (opImplicitScope.IsCustomized == true)
            {
                opImplicitScopesCustomized.Add(opImplicitScope);
            }
        }
    
        if (opImplicitScopesCustomized.Count() == 0)
        {
            Console.WriteLine(&quot;n**** No customized Scopes found ****&quot;);
        }
        else
        {
            foreach (OperationImplicitScope opImplicitScope in opImplicitScopesCustomized)
            {
                Console.WriteLine(&quot;nFound customized Scope:nProperty: {0}nRelationship: {1}nEndpoint: {2}n&quot;, opImplicitScope.Class.ToString(), opImplicitScope.Relationship.ToString(), opImplicitScope.RelationshipEndpoint.ToString());
            }
        }
    }


Please keep in mind that a am not a developer so the code samples above may be improved :-) If you have any feedback regarding these samples, feel free to write a comment or reach out to me on [Twitter](http://twitter.com/JohnerStefan). I also have to mention that the samples above are based on [this Technet forum answer](http://social.technet.microsoft.com/Forums/de-DE/98808622-1617-4e9d-8ca6-06a3555139cf/the-end-users-cannot-submit-a-service-request-after-extending-the-class?forum=portals), so credits for this post go to Rob Ford.
