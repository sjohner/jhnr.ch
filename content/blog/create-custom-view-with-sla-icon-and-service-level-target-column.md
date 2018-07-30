---
author: sjohner
comments: true
date: 2013-07-05 15:23:12+00:00
slug: create-custom-view-with-sla-icon-and-service-level-target-column
title: Create custom View with SLA icon and Service Level Target column
categories:
- Service Manager
tags:
- Management Pack
- Service Level Target
- SLO
- View
---

When working with SLA’s and Incidents it would be sometimes nice to have some visual aid to see if an Incident SLA is already breached or if everything is alright. Service Manager has some out of the box Incident as well as Service Request views which display a shiny icon if the SLA of the corresponding object is breached. In addition to the icon, there is also a column named Service Level Target, which displays date and time when the SLA will breach.

One example of such a view is the “Assigned to me” view for Incidents and Service Requests. Now when working with SLA, it would sometimes be nice to have these icons - and most of the time more important the Service Level Target column – also available for custom views. One method to achieve this is to copy the “Assigned to me” view. However, this sometimes is not enough because the “Assigned to me” view is based on the built-in _System.WorkItem.Incident.View.SLAProjectionType_ Combination Class. This Type Projection does not contain for example the Affected User relationship. Means that if you are using this Type Projection, you will not be able to add a column for “Affected user” to your custom view. To get around this we could simply create a new custom Type Projection which contains all the necessary SLA relationships as well as some others.

I will try to demonstrate in this post how you can create a custom Incident SLA Type Projection and use it to create custom views with shiny SLA icons and Service Level Target columns. Note that users which are member of a scoped/restricted user role may not see SLA icons and Service Level Target times. Check out my blog post about how to resolve the issue with [SLA icons & Service Level Target times not visible to members of scoped/restricted SCSM user roles](http://scsmlab.com/2013/08/22/sla-icons-service-level-target-time-not-visible-to-members-of-scopedrestriced-scsm-user-roles/). First of all, add the following Type Projection to an existing or new Management Pack. Feel free to add more built-in or custom relationships to the Type Projection. But remember that you should keep it as simple as possible to avoid performance issues.

[![SLA View TypeProjection](/images/SLA_View_TypeProjection-1024x138.png)](/images/SLA_View_TypeProjection.png)

You will also have to add the following references to the Manifest section of your Management Pack.

[![SLA View Manifest](/images/SLA_View_Manifest.png)](/images/SLA_View_Manifest.png)

Before importing the Management Pack, it is necessary to seal it because we will reference to the Type Projection when creating the new Incident view. You can do this by either sealing the Management Pack in Service Manager Authoring Tool or by using fastseal.exe as described by Rob Ford in [one of his blog posts](https://web.archive.org/web/20131215212456/http://scsmnz.net/sealing-a-management-pack-using-fastseal-exe). After this Type Projection is populated into Service Manager, create a new view based on this Combination class. You won’t be able to select the SLA icon and Service Level Target columns in the Console GUI. We have to add them later on manually in the Management Pack XML code.

To modify the newly created view, export the corresponding Management Pack and open it with a XML editor such as [Notepad++](http://notepad-plus-plus.org/).  Search for the name of the view and you will find a corresponding display string which has the ID of the view as an attribute defined. Search for this ID and you will find the view definition within the Presentation section of the Management Pack. Now you have to add the following two snippets to your XML code. They will add the columns for the SLA icon as well as for the Service Level Target. You can get the code by viewing the XML export of a duplicated “Assigned to Me” view.

[![SLA View Icon Column](/images/slaviewiconcolumn.png?w=696)](/images/slaviewiconcolumn.png)

[![SLA View Target Column](/images/slaviewtargetcolumn.png?w=696)](/images/slaviewtargetcolumn.png)

Furthermore, additional properties and a view string for the newly added Service Level Target column have to be added to the Management Pack:

[![SLA View Additional Properties](/images/slaviewadditionalproperties.png)](/images/slaviewadditionalproperties.png)

[![SLA View Service Level Target View String](/images/slaviewserviceleveltargetviewstring1.png?w=696)](/images/slaviewserviceleveltargetviewstring1.png)

Since we also added Assigned and Affected user display name columns to our view may want to change the header of these columns too. This has to be done in the Language Pack section of the XML.

[![SLA View Affected And Assigned User Display Name](/images/slaviewaffectedandassigneduserdisplayname.png)](/images/slaviewaffectedandassigneduserdisplayname.png)

Import the modified Management Pack back into Service Manager and don’t forget to restart Service Manager Console before using the view for the first time. Et voila - assuming that you already have some incidents with a SLA assigned - you will now see shiny SLA icons and Service Level Target in your view. I also made my Management Pack which contains the newly defined Type Projection available for [download on GitHub](https://github.com/sjohner/scsm-repository/blob/master/managementpacks/jhnr.IncidentSlaView.xml).

[![Custom SLA View](/images/slaview.png?w=696)](/images/slaview.png)
