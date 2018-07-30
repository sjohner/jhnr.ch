---
author: sjohner
comments: true
date: 2016-02-18 22:00:57+00:00
layout: post
slug: service-manager-lync-up-summary-february-2016-ur9-and-technical-preview-5
title: Service Manager Lync Up Summary February 2016 – UR9 and Technical Preview 5
categories:
- Service Manager
tags:
- Connect
- Data Warehouse
- Exchange Connector
- HTML
- LyncUp
- Performance
- SCSM Console
- SCSM Portal
- Self-Service Portal
- Service Manager
- Service Manager 2012 R2
- SQL
- Summary
- System Center
- Technical Preview
---

For this February LyncUp call, the product team shared some interesting facts about upcoming improvements in Technical Preview 5. Seems as if we are going to see some pretty cool performance improvements in next version of Service Manager :-)

As always you can [download the recording and the slides from here](http://1drv.ms/1r5Czkf). **Next meeting will take place on March 15th.**


## Update Rollup 9


As you may already know, Update Rollup 9 has been pulled since it was resetting the customizations because it replaces some unsealed Management Packs and was causing console crashes while opening forms in non-English Service Manager console.



	
  * Hotfix for scroll wheel issue in some Service Manager console forms has been pulled out of UR9

	
  * New installer package (7.5.3079.607)  is available here https://support.microsoft.com/en-us/kb/3129780




## HTML5 Self-Service Portal





	
  * More than 10k unique users (data coming from machines with telemetry enabled in last 30 days)

	
  * Keep sharing your feedback


![LyncUpHTML5PortalData](/images/LyncUpHTML5PortalData.png)


## Technical Preview 5 Highlights


TP5 will be available in coming weeks and there are actually some interesting new features improving both performance in general as well as reporting:



	
  * **Improved data processing through optimized SM-DB transactions**. This feature makes full utilization of SQL processing power and results in impressive performance improvements throughout Service Manager:

	
    * Higher work item per minute capacity

	
    * Improved console performance (work item create and update)
![LyncUpTP5ImprovedDataProcessingWorkItems](/images/LyncUpTP5ImprovedDataProcessingWorkItems.png)

	
    * Improved workflow processing

	
      * Fewer or no workflow lags

	
      * Faster workflow catch up in case of lags

	
      * Workflow catch up time improved 1.5 times in internal testing
![LyncUpTP5ImprovedDataProcessingWorkflows](/images/LyncUpTP5ImprovedDataProcessingWorkflows.png)







	
  * **Reporting improvements** (and that's a good one :-)

	
    * Out of box date dimensions for Service Manager OLAP cubes
![LyncUpTP5ImprovedReporting](/images/LyncUpTP5ImprovedReporting.png)







### **General discussion/questions**




Check out the following Connect issues which were mentioned during the call. It might be worth voting for them :-)






	
  * [Support Multi-Tenancy OOB](https://connect.microsoft.com/WindowsServer/Feedback/details/1793767)

	
  * [Relationship subscription cannot be combined with property validation](https://connect.microsoft.com/WindowsServer/Feedback/details/1741399)

	
  * [Change Request support for SCSM Portal](https://connect.microsoft.com/WindowsServer/Feedback/details/1582580)

	
  * [View all team requests in portal](https://connect.microsoft.com/WindowsServer/Feedback/details/1684176)


The product team also mentioned that the Data Warehouse is meant to be a reporting DB and is not designed for archiving data. This might be interesting for some of you which have their DWH retention settings increased from the default three years to a couple of years.

For those of you interested in syncing attachments to the Data Warehouse, the following community solution might be interesting: [http://www.systemcentercentral.com/extract-attachments-from-scsm-workitems/](http://www.systemcentercentral.com/extract-attachments-from-scsm-workitems/)


