---
author: sjohner
comments: true
date: 2017-06-09 13:24:27+00:00
layout: post
slug: scsm-2016-ur3-breaks-html-portal-hamburger-menu-links
title: SCSM 2016 UR3 breaks HTML portal Hamburger menu hyperlinks
wordpress_id: 2149
categories:
- Service Manager
tags:
- HTML5
- SCSM
- SCSM Portal
- Self-Service Portal
---

**Update:** This has been fixed with [Update Rollup 4 for System Center 2016 Service Manager](https://support.microsoft.com/en-us/help/4024038/update-rollup-4-for-system-center-2016-service-manager)

This issue was initially discovered by [Jure Jeram](https://twitter.com/JureJeram) and all credits for the workaround below goes to him. His findings are also documented in [Service Manger forums](https://social.technet.microsoft.com/Forums/systemcenter/en-US/c31ede33-be6f-4365-99f7-9f7b1137ba7a/2016-html-portal-broken-links-after-the-ur3?forum=portals). Thanks for sharing your findings!

With [update rollup 3 for Service Manager 2016](https://support.microsoft.com/en-us/help/4019979/update-rollup-3-for-system-center-2016-service-manager) released by the end of May 2017, a small issue seems to pop up after applying it to the out of the box HTML portal of Service Manager 2016.

The installation of UR3 applies some changes to the files _sidenavigator.js_ and _Sidebar.cshtml_ of the HTML portal. However it seems as if this breaks the hyperlinks in the expanded Hamburger menu on the portal.

Basically several lines in _menuActions_ function in _sidenavigator.js_ are commented out after applying update rollup 3.

[![Sidenavigator.js after installing UR3](/images/SideNavigatorWithUR3-1024x576.png)](/images/SideNavigatorWithUR3.png)

Furthermore UR3 adds some hyperlink HTML tags to Sidebar.cshtml. It seems that some functionality affecting the Hamburger menu was moved from _sidenavigator.js_ to _Sidbar.cshtml _with upate rollup 3.

[![Sidebar CSHTML Minimized Menu](/images/SidbarLinks-1024x575.png)](/images/SidbarLinks.png)

But apparently these these changes are only added for the minimized version of the Hamburger menu, they are missing for the expanded menu.

[![Sidebar CSHTML Expanded Menu](/images/SidbarLinkMissing-1024x577.png)](/images/SidbarLinkMissing.png)

As Jure outlines in Technet Forums, as a workaround we can add the missing HTML tags manually in Sidebar.cshtml file. All we have to do is to add the following code to each menu item

    
    <div class="row side_nav_home">
    	<a class="side_nav_bar_icon" href="/home">
               <span class="icon-Dictionary icon-medium icon icon-pos"></span>
               <span class="icon-text icon-text-pos">@Resources.SelfServicePortalResources.ServiceCatalog</span>
    	</a>
        </div>
        <div class="row side_nav_request">
    	<a class="side_nav_bar_icon" href="/MyRequests">
               <span class="icon-ContactInfo icon-medium icon icon-pos"></span>
               <span class="icon-text icon-text-pos">@Resources.SelfServicePortalResources.MyRequests</span>
    	</a>
        </div>
        <div class="row side_nav_activities">
    	<a class="side_nav_bar_icon" href="/MyActivities">
               <span class="icon-MultiSelectMirrored icon-medium icon icon-pos"></span>
               @if (ViewBag.Notications > 0)
               {
                   <div class="notification"><span class="activities_count">@ViewBag.Notications</span></div>
               }
               <span class="icon-text icon-text-pos">@Resources.SelfServicePortalResources.MyActvities</span>
    	</a>
        </div>
        <div class="row side_nav_help">
    	<a class="side_nav_bar_icon" href="/KnowledgeBase">
               <span class="icon-WhatsThis icon-medium icon icon-pos"></span>
               <span class="icon-text icon-text-pos">@Resources.SelfServicePortalResources.HelpArticles </span>
    	</a>
        </div>


[![Sidebar CSHTML Expanded Menu Added Links](/images/SidebarAddedLinks.png)](/images/SidebarAddedLinks.png)

Hope this helps you work around the issue for the time being. I [reported this on Microsoft Connect](https://connect.microsoft.com/WindowsServer/feedbackdetail/view/3135432/ur3-breaks-hyperlinks-in-hamburger-menu-of-html-portal), feel free to vote on it if you are affected.
