---
author: sjohner
comments: true
date: 2016-06-22 21:34:49+00:00
layout: post
slug: service-manager-lync-up-summary-june-2016-migration-issues-and-mitigation-plan
title: Service Manager Lync Up Summary June 2016 – Migration Issues and Mitigation
  Plan
categories:
- Service Manager
tags:
- Cloud
- Connect
- dll file
- Error
- LyncUp
- Management Pack
- Performance
- PowerShell
- SCSM Portal
- Self-Service Portal
- Service Management Automation
- Service Manager
- Service Manager 2012 R2
- Service Manager 2016
- Technical Preview
- Visual Studio
---

For this June LyncUp call, the product team shared some very interesting facts about upcoming migration of Service Manager 2012R2 to Service Manager 2016. This was a very passionate Lync Up call with great feedback from both attendees and product group, so I definitively recommend watching the recording. I probably was not able to cover all comments and feedback made during the call.

As always you can [download the recording and the slides from here](http://1drv.ms/1r5Czkf). **Next meeting will take place on July 19th.**


## Technical Preview 5


Once again some insights on what's new in Service Manager Technical Preview 5.



 	
  * Huge Performance improvements

 	
    * Improved console performance - work item create/update

 	
    * Faster Workflows with fewer or no lags

 	
    * Faster connector sync cycles

 	
    * Higher work-item per min processing capacity




 	
  * Date dimensions in Data Warehouse cubes

 	
  * New HTML5 Self-Service portal


See my [April Lync Up summary](https://blog.jhnr.ch/2016/04/20/service-manager-lync-up-summary-april-2016-technical-preview-5-and-sccm-connector-support/) or [this Technet article](https://technet.microsoft.com/en-us/library/mt346039(v=sc.16).aspx) for more information about what's new in Service Manager Technical Preview. VHDs can be downloaded from [here](https://www.microsoft.com/en-us/download/details.aspx?id=51952).


## SCSM 2016 migration issues and mitigation plan


Some folks reported issues with migration of their Service Manager extensions when moving to Service Manager 2016.


#### Issues


According to the product team, custom Service Manager extensions might break if:



 	
  * The custom solutions have target .Net Framework version below 4.5

 	
  * Existing classes in use have been moved to different assembly

 	
  * Custom solutions have "Specific Version" (7.1.1000.0) reference to Service Manager assemblies


Service Manager 2016 has been upgraded to support .NET Framework version 4.5.1. That's basically a good thing! But to enable to move, the internal tools required to move few classes across dlls. This has broken the customizations built on top these dll files.

Examples:

 	
  * Few classes from the following dlls have been moved to Microsoft.EnterpriseManagement.UI.SdkDataAccess.dll:

 	
    * Microsoft.EnterpriseManagement.UI.Core.dll,

 	
    * Microsoft.EnterpriseManagement.UI.Extensions.dll and

 	
    * Microsoft.EnterpriseManagement.UI.SMControls.dll




 	
  * Assembly Microsoft.EnterpriseManagement.UI.ReportingFramework.dll has been completely merged into Microsoft.EnterpriseManagement.UI.SdkDataAccess.dll

 	
  * Few classes from Microsoft.EnterpriseManagement.ServiceManager.UI.Administration.dll have been moved to Microsoft.EnterpriseManagement.ServiceManager.Application.Common.dll


You might encounter errors which are referencing for example namespace and missing class issues when running your custom solution with Service Manager 2016


#### Solution


The product team is already working with some MVPs and partners (namely Cased Dimensions, GridPro and Provance) to upgrade/fix their tool kit with Service Manager 2016. Also, there will be a blog post detailing the issues and the mitigation plan.

Basically the following describes how to resolve the above mentioned issues when moving a custom solution to Service Manager 2016


###### Recompile the custom solutions with .NET 4.5.1


Recompile custom solutions with .NET Framework 4.5.1 if they are using .NET Framework version 3.5


###### Add the appropriate references that were moved in Service Manager 2016


If you see references which are not resolved properly after the migration to .NET Framework 4.5.1 you might use classes which have been moved to different dlls. For this case, the product team is documenting all the classes which were moved across dlls (documentation should be available in couple of weeks). So you should be able to tell what references have to be changed in your custom solutions. So this will probably be just a matter of changing references.


###### Remove the "Version Specific" information from referenced Service Manager assembly


If you referenced "Version Specific" assemblies, you will need to remove or at least change "Version Specific" information for the referenced Service Manager assembly. This is due to the fact that in Service Manager 2012R2 few assemblies have lower version (7.0.5000.0) and few have higher version (7.1.1000.0). Since in Service Manager 2016 all the assemblies have the same version (7.0.5000.0), you will need to remove or change references to assemblies with versioning 7.1.


## Upgrading to SCSM 2016


Steps for upgrade to Service Manager 2016 will be the following:



 	
  * In place upgrade of SCSM 2012R2 to SCSM 2016

 	
  * Re-import or reinstall the upgraded custom solutions from Partners and MVPs


Keep in mind that customizations made in Authoring Tool may also break if classes which are moved to other dlls are used!


## **General discussion/questions**


There will be a new version of Authoring Tool with Service Manager 2016. However it will not contain any new functionality

There should be no need to delete Management Packs in your environments when migrating from 2012R2 to 2016. It should be just a matter of changing references and updating existing Management Packs

There are no plans to move any other classes apart from the ones which were already moved

The product team has no plans to test SMLets with Service Manager 2016 because it is a community project. **SMLets are not officially supported by Microsoft**. MVP [Kurt van Hoecke](https://twitter.com/BunkCo) mentioned that so far all SMLets cmdlets he used worked fine with Service Manager 2016. Folks on the call are mentioning that SMLets should be integrated in SCSM because everyone seems to use it.

**Update: **As Anton mentioned in the comment section, he will make sure that SMLets will be ready for Service Manager 2016. Thanks Anton!

Statement was made by an attendee, that improvements are only made in foundational areas of the product. Question was raised if the development team and capacity is big enough to bring the product further.

It was mentioned by folks that all the System Center products are seeing a move to the Cloud (like SCOM does with OMS). There are some concerns if Service Management is going to be a thing that Microsoft is committed to. There seems to be a lack of confidence that Service manager is getting the same kind of focus that all of the other System Center products are getting.

Some great new to finish this up: **There will be a Service Management Automation (SMA) connector available for Service Manager 2016**. It wont be available in RTM but according to the team, it will be released with one of the first Update Rollups available for version 2016.
