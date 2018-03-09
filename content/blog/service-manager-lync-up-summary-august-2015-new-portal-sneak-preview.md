---
author: sjohner
comments: true
date: 2015-08-22 11:38:45+00:00
layout: post
slug: service-manager-lync-up-summary-august-2015-new-portal-sneak-preview
title: Service Manager Lync Up Summary August 2015 - New portal sneak preview
wordpress_id: 1149
categories:
- Service Manager
tags:
- Connect
- HTML5
- LyncUp
- SCSM Portal
- Sharepoint
- Summary
---

Unfortunately I missed the July Lync Up call. But I am back with the Service Manager Lync Up call summary for August. And that is a good one ;-)  The call mainly focused on demoing the upcoming new Service Manager portal. Good stuff! As always you can [download the recording and the slides from here](http://1drv.ms/1r5Czkf). **Next meeting will take place on September 15th.**





## Sneak preview of new Self-Service Portal




Service Manager team is actually working on a **new Self-Service Portal**. This portal **will not have any dependencies on Silverlight** and will therefore also **support multiple browsers and screens** (Desktop, Mobile, Tables, etc.). Another huge improvement will be that there is **no requirement for SharePoint** anymore. Besides that, there will be a **new knowledge articles experience** where it won't be necessary to download KB articles as a RTF anymore as well as a new caching infrastructure for faster performance. Sounds great!

<!-- more -->


If you are interested in seeing a demo of the portal, check out the [recording of the August Lync Up call](http://1drv.ms/1r5Czkf). **Remember that the portal is still work in progress and a lot of things may change until final release.**




As you can see in the screenshot below, the portal is built in HTML5 and will support announcements as well as favorites




[![New SCSM Portal Home Page](/images/newscsmportal_home.png?w=660)](/images/newscsmportal_home.png)




Another nice feature will be that the generic Create Request button can now be mapped to any request.




Knowledge articles are now searchable and will be displayed right in the portal as HTML5 page instead of having to download an RTF file. Users are also able to mark KB articles as a favorite and most popular KB articles will be shown on the knowledge base home page.




[![New SCSM Portal Knowledge Base](/images/newscsmportal_kb.png?w=660)](/images/newscsmportal_kb.png)




There will be a conversion RTF to HTML conversion in the back end to display KB articles from SCSM CMDB in the browser.




[![New SCSM Portal KB Article](/images/newscsmportal_kbarticle.png?w=660)](/images/newscsmportal_kbarticle.png)




Another cool feature is the _Generic Search_ where users can search for both knowledge articles as well as request offerings which contain the given keyword in either title or description.




[![New SCSM Portal Generic Search](/images/newscsmportal_genericsearch.png?w=660)](/images/newscsmportal_genericsearch.png)




My Requests and My Activities pages are quite similar to the actual portal, but seem to load much faster now




[![New SCSM Portal My Requests](/images/newscsmportal_myrequests.png?w=660)](/images/newscsmportal_myrequests.png)




Currently the portal is **targeting end users only** but one has to mention that the **portal is still work in progress and a lot of things may change until final release**. The portal should be **available somewhere around Update Rollup 8 which should be released in October 2015** and the portal will be **available for Service Manager 2012 R2**. The portal itself will not be part of Update Rollup 8 but available with a separate installer. For System Center vNext, new portal will be shipped with the default installer.





## Disabling ECL (Entity Change Logs) for Config Items brought in by connectors




Entity Change Logs are written for every transaction in Service Manager. They are used to display history of Config Items and Work Items as well. Furthermore they are used by workflows to trigger user defined actions on these items. These logs are kept for one year.




However when connectors bring in objects to the CMDB a large amount of transactions are involved in this process and for every transaction, an entry in the ECL table is created. This results in ECL table size growing immensely over time, which affects Service Manager performance.




The product team states that disabling the ECL improves connector sync time by around 65% for SCCM and 50% for AD connector. The option to disable ECL will be an optional feature which can be activated by a registry key and will be disabled by default.




One has to be aware that enabling this feature, will have impact on Service Manager functionality:






	
  * DCM (Desired Configuration Management) Incidents will stop working

	
  * Workflows configured on Config Items brought in by a connector will not work

	
  * History data for Config Items won't be shown for Config Items




Most mentioned problem for customers when enabling this feature will probably be that history data for Config Items wont' be shown anymore. This can have some impact on scenarios like auditing and troubleshooting where one needs to know which item was changed by whom and when.




Also some 3rd party solutions like Asset Management for Service Manager may rely heavily on workflows that do actions on Config Items.




Currently this feature is work in progress and may change until final release. Also there is no release date defined yet.





## Workloads




For current and future development, the product team is interested in getting information about workloads in different Service Manager environments. There will be a SQL script available for download which will collect workload data from your CMDB. **The script won't capture any private information**, only counts to estimate the load (e.g. work item count, number of groups/queues, etc.).




The script will generate a readable CSV file which can be reviewed/verified before sharing. The script as well as more details will be [available on the Service Manager blog](http://blogs.technet.com/b/servicemanager/). **Please share your workloads to help the product team better understand different Service Manager environments.**





## **General discussion/questions**




Other interesting facts/questions which came up during the call are the following:




For those of you who were waiting for feedback to **Update Rollup 7**; most people on the call reported that UR7 runs fine in their environments and that they are rolling out to production.




According to the product group, there is no **Azure Automation connector** planned for near future
