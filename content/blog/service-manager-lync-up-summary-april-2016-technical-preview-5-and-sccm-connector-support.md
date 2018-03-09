---
author: sjohner
comments: true
date: 2016-04-20 19:04:49+00:00
layout: post
slug: service-manager-lync-up-summary-april-2016-technical-preview-5-and-sccm-connector-support
title: Service Manager Lync Up Summary April 2016 – Technical Preview 5 and SCCM connector
  support
wordpress_id: 1573
categories:
- Service Manager
tags:
- HTML
- LyncUp
- Performance
- SCCM Connector
- SCSM Console
- SCSM Portal
- Self-Service Portal
- Service Manager
- Service Manager 2012 R2
- Summary
- System Center
---

For this April LyncUp call, the product team shared some interesting facts about upcoming improvements in Technical Preview 5 as well as SCCM support. Actually it was mostly the same as already presented in [February LyncUp call](https://blog.jhnr.ch/2016/02/18/service-manager-lync-up-summary-february-2016-ur9-and-technical-preview-5/). But still interesting to see some pretty cool performance improvements in next version of Service Manager 🙂

As always you can [download the recording and the slides from here](http://1drv.ms/1r5Czkf). **Next meeting will take place on May 17th.**


## Technical Preview 5 Highlights


As mentioned, there were no real news about TP5 which have not yet been covered in [February LyncUp call](https://blog.jhnr.ch/2016/02/18/service-manager-lync-up-summary-february-2016-ur9-and-technical-preview-5/). Basically the product team focused a lot on performance. And as some early adopters mentioned on the call the Console as well as SDK performance seems to be much, much better in TP5. **Service Manager Technical Preview 5 will be released on 27th of April. **Upgrade from 2012 R2 to TP5 will be supported.**
**

One thing which that was not mentioned during last calls, is that average incident creation time in TP5 is considerably improved. According to the product team, Service Manager can now easily handle a large inflow of 45 work items per minute.

[![TP5 Processing Capacity](/images/TP5_ProcessingCapacity-1024x117.png)](/images/TP5_ProcessingCapacity.png)

For those of you interested in details about the upcoming performance improvements, I recommend reading my [February LyncUp call summary](https://blog.jhnr.ch/2016/02/18/service-manager-lync-up-summary-february-2016-ur9-and-technical-preview-5/) and checking out the recording as well as slides from April's LyncUp call.


## HTML5 Self-Service Portal


**Update: **The[ product team announced on April 26th](https://twitter.com/ServiceManager/status/724843879275614208) that Update Rollup 3 for HTML5 Self-Service portal is re-scheduled to be released on May 10th instead of April 27th.

There will be an **Update Rollup for HTML5 Self-Service portal released on <del>April 27th</del> May 10th**. Key improvements will be the following:



 	
  * "Must vote" and "Has veto" details added for reviewers in review activities

 	
  * Multiple selection across pages in Query UI are now working

 	
  * Enum lists now appear in the same order as shown in the Console

 	
  * Fixed the last item scrolling issues in IE10

 	
  * Portal showing incorrect time with server having 12 hour (am/pm) time format

 	
  * Request and Service offerings are now shown alphabetically

 	
  * Attachments are now listed in "My Requests" and can also be downloaded

 	
  * Support for "Display Only" fields added




## SCCM Support


Good news for all of you waiting to upgrade your Configuration Manager environment to current branch!

SCCM connector is now officially supported with SCCM Build 1511 and 1602 with the following Service Manager versions:



 	
  * Service Manager 2012 R2 UR9

 	
  * Service Manager 2016 TP5




## **General discussion/questions**


Interesting question was raised if  the workflow triggering technique remains the same in TP5 where triggered workflows still poll for new/changed objects? There will be no changes in workflow triggering technique for version 2016.

Also there was a question about showback and chargeback which was delivered by the Cloud Process Pack for 2012. That is not supported in 2016. According to the product team, there are no enhancements for Cloud Process Pack planned and there will be no chargeback in version 2016.

The product team started a discussion about a new SMA or Azure Automation connector. They were checking on how many people would be interested in what kind of connector. SMA got most of the votes (23), Azure Automation (19) and Orchestrator (20) where close behind. Seems that most of the attendees are waiting for both SMA and Azure Automation connector. Maybe there will be released an out of the box connector for Azure Automation and/or SMA eventually.
