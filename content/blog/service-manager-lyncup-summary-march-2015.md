---
author: sjohner
comments: true
date: 2015-04-01 17:00:13+00:00
layout: post
slug: service-manager-lyncup-summary-march-2015
title: Service Manager LyncUp Summary March 2015
wordpress_id: 1042
categories:
- Service Manager
tags:
- Connect
- Hotfix
- LyncUp
- Summary
- Update Rollup 5
- Update Rollup 6
- UR5
- UR6
---

Maybe you already heard about the regular Service Manager LyncUp calls hosted by [Kathleen Wilson](http://twitter.com/mkathleenwilson) and the Service Manager product team. I join the calls whenever I can, but often I just can't make it. Fortunately the calls are recorded! And since I always forget about the stuff which was discussed in the call I decided to summarize the calls and share them here. I will try to do this for future calls too and share these summaries as well. Hopefully a bit more closely to the release date of the recording :-)

Please let me know if I forgot something important and also if I interpreted something wrong :-) You can always [download the recording and the slides from here](http://1drv.ms/1r5Czkf). Availability of the recordings is announced on the[ official Service Manager blog](http://blogs.technet.com/b/servicemanager). **T****he next call is scheduled to take place on April 21st, 9 am PST. **<!-- more -->


#### **Update Rollup 5 Hotfix**


First of all, if your data warehouse server does not use Month-Day-Year time format, then an event "The conversion of a navchar data type to a datetime datatype resulted in an out-of-range value" is logged in your Service Manager Data Warehouse, and extract job no longer runs after installing Update Rollup 5.

The Service Manager team now **released a hotfix which addresses this issue.** The hotfix should be installed after UR5 and **will also be part of Update Rollup 6**. The fix might receive additional testing. Therefore if you are not severely affected by this problem, it is recommended that you wait for the next update rollup that contains this hotfix. If you can't wait download the hotfix from [http://aka.ms/kb3045893](http://aka.ms/kb3045893)


#### **Update Rollup 6**


**Update Rollup 6 is planned to be released on April 28th, 2015**. UR6 has not yet passed test and quality checks, hence the final payload might change.

For now, the following key improvements are planned for UR6



	
  * Considerable improvement in sync time of AD and SCCM connector

	
  * **AD connector sync schedule will become configurable from console** (similar to SCCM connector)

	
    * Unfortunately LDAP queries still cannot be modified. There exists a [feature request on Connect](https://connect.microsoft.com/WindowsServer/Feedback/Details/1186508) created by Brian Shaw which addresses this issue. Feel free to vote on that one. As a workaround you can also refer to the a [blog post on how to edit LDAP Query filter of Active Directory Connectors](http://bit.ly/1FHGNs8) published by [Marat Kuanyshev](https://twitter.com/SCUtils).




	
  * In current state, the AD connector Group Expansion Workflow runs even if the corresponding AD connector is disabled. **This problem will get fixed in UR6**. Make sure that your internal workflows and other IT processes do not assume this broken scenario the be the expected behavior

	
  * If a user extended screen to open forms, the SM console continues trying toto open the form windows in the extended display that no longer exists. This problem will get fixed after UR6




#### **Connect Issues**


Like in most of the calls, the product team presented some of the top voted Connect issues. As you can see below, **Lync 2013 will not be supported in current release**. However there [exists a request on Connect](https://connect.microsoft.com/WindowsServer/Feedback/Details/1174737) to support Lync 2013 with current version. Vote on that one if you need Lync 2013 integration for current release. According to the product team, there are no plans to integrate Skype for business yet.

![Top Voted Connect Issues March 2015](/images/topvotedconnectissuesmarch2015.png?w=604)

As for Connect, there should be a feature request section for Service Manager available soon. I did not check on that one but I think this should already be available by now. Use this area for all your feature requests.

If you are not already a Member of connect:



	
  1. Register for Connect, you will need a Microsoft Account
[https://connect.microsoft.com/directory/](https://connect.microsoft.com/directory/)



	
  2. Join the Windows Server System Center Public Connect[
https://connect.microsoft.com/WindowsServer/SC_Public](https://connect.microsoft.com/WindowsServer/SC_Public)



	
  3. Vote up or down the existing reported issues at
[https://connect.microsoft.com/WindowsServer/Feedback](https://connect.microsoft.com/WindowsServer/Feedback)


Below you can find various issues which are logged on connect and have been mentioned during the call. Feel free to vote on any of those.

_If a User changes from having a manager in AD to NOT having a manager, the relationship is not removed/updated in the CMDB_
[https://connect.microsoft.com/WindowsServer/Feedback/Details/1183474](https://connect.microsoft.com/WindowsServer/Feedback/Details/1183474)

_Scroll wheel doesnt work in the incident form_
[https://connect.microsoft.com/WindowsServer/Feedback/Details/1183442](https://connect.microsoft.com/WindowsServer/Feedback/Details/1183442)

_UR5 scrambled customized Service Request Areas_
[https://connect.microsoft.com/WindowsServer/Feedback/Details/1170952](https://connect.microsoft.com/WindowsServer/Feedback/Details/1170952)

_Move Service Request Lists into Unsealed Management Packs_
[https://connect.microsoft.com/WindowsServer/Feedback/Details/1102193](https://connect.microsoft.com/WindowsServer/Feedback/Details/1102193)

_Data Collision Exception / Item has been changed by another user or process_
[https://connect.microsoft.com/WindowsServer/Feedback/Details/1062768](https://connect.microsoft.com/WindowsServer/Feedback/Details/1062768)

_AD Connector: CN=DeletedObjects query not working when scoped._
According to the product team, this one is currently under investigation. They did not start working on a fix but it is on the effort which is done for AD connector.
[https://connect.microsoft.com/WindowsServer/Feedback/Details/1115057](https://connect.microsoft.com/WindowsServer/Feedback/Details/1115057)


### **General discussion/questions**


Other interesting facts/questions which came up during the call are the following:

Does the product team take time to look at what other products such as ServiceNow/Remedy do well and look to incorporate functionality
**Yes they do**

Any plan to support In-Memory OLTP in SQL Server 2014 to speed up SCSM considerably? **SQL 2014 support is in the plan for vNext but no plans to support In-Memory OLTP **

And probably the most interesting one: Is there a separate dev team for vNext?
**No it’s the same team working together for both. These guys are pretty busy…**
