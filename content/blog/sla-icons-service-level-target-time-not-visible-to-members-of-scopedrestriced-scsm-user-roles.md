---
author: sjohner
comments: true
date: 2013-08-22 16:04:05+00:00
layout: post
slug: sla-icons-service-level-target-time-not-visible-to-members-of-scopedrestriced-scsm-user-roles
title: SLA icons & Service Level Target time not visible to members of scoped/restricted
  SCSM user roles
wordpress_id: 682
categories:
- Service Manager
tags:
- Service Level Target
- SLO
- User Role
- View
---

I recently wrote [a post about how to create custom views with SLA icons and Service Level Target column](http://scsmlab.com/2013/07/05/create-custom-view-with-sla-icon-and-service-level-target-column/). After writing this post I realized, that if a user is member of a scoped/restricted user role, he would not be able to see the SLA icons and Service Level Target times. I noticed that not only custom views are affected by this issue. This happens too for default SCSM views which display SLA icons and Service Level Target column, like the "Assigned To Me" view for Incidents as well as for Service Requests.

It seems pretty strange, but I was able to reproduce the issue multiple times by restricting_ Incident Resolvers_, _Service Request Analysts_ as well as _Advanced Operators_ user roles. As long as there are no restrictions on a user role, everything works fine. But if for example the user role is restricted to only allow access to a specific Catalog Group, icons and Service Level Target times will no more be visible for members of this user role.
<!-- more -->

As a [reply to my question on Technet forums](http://social.technet.microsoft.com/Forums/systemcenter/en-US/e0f4169d-52cc-49a1-af77-3bf15ff9a81f/service-level-target-and-sla-icon-do-not-show-up-for-members-of-custom-incident-resolvers-user-role), Patrick Massie mentioned that this issue can be resolved by creating a _All Service Level Configuration items _group and assigning that group to the scoped user role. Thanks for pointing me to the right direction Patrick, that does the trick!

As Patrick mentions in his forum reply, within the Incidents, on the Service Level tab, the _Time before SLO Breached_ just shows a hyphen instead of the remaining time. As described below, this can be resolved too by creating another configuration item group and selecting _ServiceManager.Calendar.Projection _combination class. The following steps will describe how to create the necessary groups.

Within Service Manager you can use groups to logically group and manage configuration items. Groups can contain either CIs of the same class or mixed classes. They can be either static by manually adding certain CIs or dynamic by specifying rules to include CIs. We are going to use the latter one to create our groups.

First of all, create a new Config Item group which includes all _Service Level Configuration_ items

[![Service Level Configuration Items Group](/images/servicelevelconfigurationitemsgroup.png?w=696)](/images/servicelevelconfigurationitemsgroup.png)

As Patrick mentions in his forum reply, within the Incidents, on the Service Level tab, the _Time before SLO Breached_ just shows a hyphen instead of the remaining time. This behavior is normal for _Service Level Instance Time Information_ objects which have their status not equal to _Active. _However in this case the status **is** _Active_ and you should see the time remaining before the SLO breaches.

[![Incident Time Before SLO Breached](/images/incidenttimebeforeslobreached.png?w=696)](/images/incidenttimebeforeslobreached.png)

To resolve the above problem, create another new Config Item group where you specify the _ServiceManager.Calendar.Projection _combination class.

[![Service Manager Calendar Projection Items Group](/images/servicemanagercalendarprojectionitemsgroup.png?w=696)](/images/servicemanagercalendarprojectionitemsgroup.png)

Assign both groups to your scoped user role and members of this role should finally be able to see SLA icons, Service Level Target time as well as time remaining before the SLA is breached. Make sure you do not uncheck your other CI groups to which you want to provide access.

[![Edit User Role Config Item Groups](/images/edituserroleconfigitemgroups.png?w=696)](/images/edituserroleconfigitemgroups.png)
