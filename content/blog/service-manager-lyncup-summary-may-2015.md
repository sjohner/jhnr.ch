---
author: sjohner
comments: true
date: 2015-05-20 22:14:09+00:00
layout: post
slug: service-manager-lyncup-summary-may-2015
title: Service Manager LyncUp Summary May 2015
categories:
- Service Manager
tags:
- Connect
- LyncUp
- Road map
- SCSM Portal
- Self-Service Portal
- Summary
- Update Rollup 6
- Update Rollup 7
- UR6
- UR7
---

Unfortunately I missed April LyncUp call. Luckily the Service Manager team already did a great job on summarizing pretty much all the important information which came up in the April call on the[ official Service Manager blog](http://blogs.technet.com/b/servicemanager/archive/2015/04/22/it-39-s-time-for-ur6.aspx). Overall it was all about Update Rollup 6 which was released April 27th.

As always you can [download the recording and the slides from here](http://1drv.ms/1r5Czkf). That's also where the recording of the May LyncUp call is available. **T****he next call is scheduled to take place on June 16th, 9 am PST. **


#### Update Rollup 6 Hotfix




Unfortunately some customer reported issues with Active Directory and & SCCM connectors after installation of Update Rollup 6.




[![Update Rollup 6 Warning](/images/ur6warning.png?w=604)](/images/ur6warning.png)


<!-- more -->


These issues are currently under investigation and **a hotfix for UR6 will be released in the next few days**. There won't be a new release of UR6, instead you will need to install Update Rollup 6 together with the hotfix. However the hotfix should be included in **Update Rollup 7 which is scheduled to be released 28th of July.**** **




When dealing with connector issues after installing UR6, be aware that the first connector sync after installation could take longer than expected since it does a full sync.




Further more, a fix for AD connector performing full sync after Domain Controller failover **will be withdrawn with the coming hotfix** because of issues originating from changes made in Update Rollup 6.





#### Self service portal Wishlist




As announced in April LyncUp call, the product team thinks about putting some **investments in Service Manager Self-Service portal**. However focus is only about use cases for end user needs. As it seems, there are no intentions to add analyst portal functions.




There is no decision yet if the portal would be running only on premise, only on Azure or maybe both. But it will be **definitively be on HTML5**. Great news I'd say!




Possible investments in portal area **could** be the following. These items should show up on connect in some days that you can vote on them to help the product team to prioritize.
**Update:** links to Connect items are now available on [official Service Manager blog](http://blogs.technet.com/b/servicemanager/archive/2015/05/21/may-service-manager-lyncup-recording-is-now-available.aspx).






	
  * AD User picker as form field in request offerings, user picker as a form element

	
  * Dynamic forms in request offerings

	
    * Updating enums as per the input of other field

	
    * Show/Hide form elements a per the input of other field




	
  * Rich experience with help articles

	
    * Viewing help articles in portal itself

	
    * Rating help articles

	
    * Sharing help articles




	
  * Escalate filed requests

	
  * Bookmarking favorite request offerings and knowledge articles for easy navigation

	
  * Language preferences for each user


Other suggestions from attendees were the following:

	
  * Portal access for non-Active Directory users aka guest access

	
  * Portal language should be the same as Operating System language by default but of course be changeable by user

	
  * Visualize (be able to find and sort) work items users created

	
  * Better access to branding/themes/layouts of the portal

	
  * Built-In user satisfaction survey


If you have any suggestions for a better portal experience, please submit a feature request on [Connect](https://connect.microsoft.com/WindowsServer/feedback/CreateFeedbackForm.aspx?FeedbackFormConfigurationID=6117&FeedbackType=3). As announced in [March LyncUp call](http://scsmlab.com/2015/04/01/service-manager-lyncup-summary-march-2015/), there is now also a feature request section available for Service Manager.


#### **Connect issues**




There is actually a chance that one of the most voted Connect issue will be resolved in Update Rollup 7. According to the product team the **data collision exception issue is in deep progress and on track for UR7** which is planned to be released on July 28th. So is a fix for the scrolling problem on the Incident form. Good news!




[![SCSM Top Voted Connect Issues May 2015](/images/connectissues.png?w=604)](/images/connectissues.png)


If you are not already a Member of connect:



	
  1. Register for Connect, you will need a Microsoft Account
[https://connect.microsoft.com/directory/](https://connect.microsoft.com/directory/)



	
  2. Join the Windows Server System Center Public Connect[
https://connect.microsoft.com/WindowsServer/SC_Public](https://connect.microsoft.com/WindowsServer/SC_Public)



	
  3. Vote up or down the existing reported issues at
[https://connect.microsoft.com/WindowsServer/Feedback](https://connect.microsoft.com/WindowsServer/Feedback)




For all of you which can't wait until UR7 for a solution to the data collision exception issue, you definitively have to take a look at the “**Property Level Optimistic Concurrency control for console forms**” solution by [Aaron Croasmun](https://social.technet.microsoft.com/profile/aaron%20croasmun/) which is available on [TechNet Gallery](https://gallery.technet.microsoft.com/System-Center-Service-fca7af29).





#### Service Manager road map




Well, everyone is interested in this one. This topic pops up in quite every LyncUp call again and again. No exception on this call :-) **At the moment there is no Service Manager road map available**. As stated at Ignite conference, details are still work in progress. We will have to wait a little to get some insights on long term planning.


In the meantime I recommend watching the recording of [Platform Vision & Strategy (6 of 7): What’s New in System Center for Management](http://channel9.msdn.com/Events/Ignite/2015/BRK2459) session hosted by [Jeremy Winter](https://channel9.msdn.com/Events/Speakers/Jeremy-Winter) which gives you some insights on overall System Center road map.


### **General discussion/questions**


Other interesting facts/questions which came up during the call are the following:

There was a question raised regarding** announcements on OOB Service Manager portal**. There is actually a way to enable announcements on the portal and it is described in chapter 7 of the [System Center Service Manager 2012 Unleashed book](http://blogs.technet.com/b/servicemanager/archive/2014/10/07/system-center-service-manager-2012-unleashed-book-now-available.aspx).


If some of you are searching for **non-IT use cases for Service Manager**, please refer to chapter 2 of the free [MS Press book Optimizing System Center Service Manager](http://blogs.msdn.com/b/microsoft_press/archive/2013/12/18/free-ebook-microsoft-system-center-optimizing-service-manager.aspx) where there are some non-IT uses for Service Manager described




According to the product group, there exist no concrete plans for an **Azure Automation connector** at this moment.
