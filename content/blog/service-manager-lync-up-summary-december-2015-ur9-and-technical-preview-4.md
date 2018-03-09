---
author: sjohner
comments: true
date: 2015-12-16 22:01:17+00:00
layout: post
slug: service-manager-lync-up-summary-december-2015-ur9-and-technical-preview-4
title: Service Manager Lync Up Summary December 2015 - UR9 and Technical Preview 4
wordpress_id: 1236
categories:
- Service Manager
tags:
- Exchange Connector
- LyncUp
- SCSM Portal
- Self-Service Portal
- Service Manager
- Service Manager 2012 R2
- Technical Preview
- Update Rollup 9
---

Long time since my last Service Manager Lync Up summary. Unfortunately I was unable to attend the last Lync Up sessions but this time I could make it work. And it was worth it :-) The product team shared some very interesting facts about upcoming fixes in UR9 as well as Technical Preview 4. As always you can [download the recording and the slides from here](http://1drv.ms/1r5Czkf). **Next meeting will take place on January 19th.**<!-- more -->


### Service Manager Technical Preview 4


Service Manager Technical Preview was released back in November and you can download it from [https://microsoft.com/en-us/evalcenter/evaluate-system-center-technical-preview](https://microsoft.com/en-us/evalcenter/evaluate-system-center-technical-preview)

Key improvements in Service Manager Technical Preview 4:



	
  * Finally support for Lync 2013 and Skype for Business

	
  * New Self Service portal (basically the same portal which came with UR8 which will also be the default portal in version 2016)

	
  * Performance enhancements specific to 2016 Technical Preview 4

	
    * Optionally disable ECL logging for faster SCCM and AD connectors (manual activation needed)

	
    * Grooming improvements for ECL logs

	
    * Reduction in incident workflow lag

	
    * Integrated ADGroupExpansion Workflow into AD connector





Read more about what's new [https://technet.microsoft.com/en-us/library/mt346039.aspx](https://technet.microsoft.com/en-us/library/mt346039.aspx)

Be aware that when installing Technical Preview 4 that there is no supported upgrade path to the next version or even RTM.


### New HTML portal


First portal hotfix is available now, you can download from [https://aka.ms/portalU1](https://aka.ms/portalU1)

The fix contains new features as well as fixes for critical issues in Self Service portal. See the above mentioned URL for a complete list of fixes as well as known issues.

New features included are:



	
  * Nested enumeration lists are now supported inside the Request offering forms

	
  * Portal now allows you to share and access different objects inside the portal with direct URLs.


Key issues which are fixed in this release are:

	
  * Affected user and Created by user is getting set to the service account

	
  * Query type form element is not working for the Incident and User classes

	
  * Request Offering forms are failing to load if a Query type form element is part of the form

	
  * Username token is not passing values to the mapped field

	
  * Cancelling request form does not work


There are still some known issues at the moment which might prevent you from using the portal in production:

	
  * Multiple query form elements inside same request offering form cannot add same relationship types

	
  * Portal does not filter Request Offerings based on the language

	
  * Service Request User Inputs are not populated (this is planned to be fixed in the next portal update)


Technical Preview 4 currently only supports .Net Framework 4.5.1


### What is included in Update Rollup 9


Update Rollup 9 is planned to be released at the end of January 2016 and will probably contain fixes for the following issues:



	
  * Scroll wheel does not work in the incident form

	
  * AD connector goes for a full sync after updating LDAP query via PowerShell

	
  * Orchestrator connector fails to identify if any connector has been groomed out

	
  * CoreEnterprise64 is not detected as an enterprise edition by Service Manager

	
  * Reporting panel in SM console randomly becomes blank

	
  * With UR7 installed updating a work item sometimes throws the following error:
Failed to execute Submit operation. The user <USER> does not have sufficient permission to perform the operation

	
  * DCM ticket does not contain the details about the settings which have changed and caused the non-compliance issue

	
  * With UR7 installed, AD connector fails when a DC is non-reachable




### **General discussion/questions**


Support for change requests on the new HTML portal is not on the list for near future releases. However it seems to be a highly requested feature. If you would like to see change requests available for the new portal, vote for it on [Connect](https://connect.microsoft.com/WindowsServer/Feedback/Details/1582580)

There was a discussion initiated by the product team about how to deal with Service Request area list (see Connect issue [https://connect.microsoft.com/WindowsServer/Feedback/Details/1102193](https://connect.microsoft.com/WindowsServer/Feedback/Details/1102193)). The product team thinks is better to hide existing enums than to move them to unsealed Management Packs because removing them can break a lot of things when upgrading existing environments. This will be considered for a future Update Rollup.

There are no plans to release an Analyst portal now.

Also no plans for a SMA / Azure Automation connector for 2016 release.

There will be no update for Exchange Connector in version  2016. Also it will not be shipped with Service Manager but will be available as standalone solution.

No plans to integrate community solutions like SendMail or similar and no timeline for integration of Power BI features yet.

Question which comes up every now and then: Are there any plans to improve Authoring Tool? Unfortunately not. General recommendation is to use [Visual Studio Authoring Extensions](https://www.microsoft.com/en-us/download/details.aspx?id=30169) for creating Management Packs. Visual Studio Authoring Extensions are available for Visual Studio 2015 by the way.
