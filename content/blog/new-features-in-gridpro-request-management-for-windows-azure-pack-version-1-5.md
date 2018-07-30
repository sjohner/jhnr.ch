---
author: sjohner
comments: true
date: 2016-05-11 21:23:18+00:00
layout: post
slug: new-features-in-gridpro-request-management-for-windows-azure-pack-version-1-5
title: New features in Gridpro Request Management for Windows Azure Pack version 1.5
categories:
- Service Manager
- Windows Azure Pack
tags:
- Encryption
- Gridpro
- Password
- Request Management for Windows Azure Pack
- Service Manager
- Service Manager 2012 R2
- Summary
- Windows Azure Pack
---

Today I attended a great webinar hosted by [Patrik Sundqvist.](https://twitter.com/gridpatrik) There are some awesome features coming in new version of Request Management for Windows Azure Pack! If you have not yet seen it, I recommend [attending the second webinar on May 12th](http://www.gridprosoftware.com) to see some nice demo scenarios. And since I always forget about details which are presented in those webinars I decided to summarize content and share it here :-)

Update: Gridpro just published the recording of the webinar [here](http://videos.gridprosoftware.com/video/166334341).


#### Service Manager Request Offerings


When using the new publish option for Request Offerings called "WAP: Action Type" you can now avoid exposing a Request Offering in the New Request Wizard. However it can be still used as a Request Action


#### Password Textbox


Text prompts can be configured to be displayed as Password prompts. With this option enabled, passwords are not obfuscated in the request offering but still visible in Service Manager.

As shown in the below screenshot, it is now also possible to add tooltips for all kind of prompts.

[![Request Management for WAP Password Demo](/images/RequestManagementForWap_Demo3-1024x839.png)](/images/RequestManagementForWap_Demo3.png)


#### RSA encrypt text in password textbox


However it is also possible to encrypt/decrypt passwords by using certificates. This makes it impossible to read passwords in Service Manager but it is still possible to decrypt the passwords by using for example SMA and the given certificate.

[![Request Management for WAP Encryption emo](/images/RequestManagementForWap_Demo4-1024x484.png)](/images/RequestManagementForWap_Demo4.png)


#### Exclude from user input


Prompts can now be configured to be excluded from _User Input. _This means, that prompts are shown on the request offering as _readonly_ prompts (e.g. to display information to the user dependent on the preceding prompt) but will not be included in _User Input _XML.

[![Request Management for WAP Exclude User Input Demo](/images/RequestManagementForWap_Demo6-1024x834.png)](/images/RequestManagementForWap_Demo6.png)


#### Request Management


Knowledge articles now support displaying internal content in HTML and it is now possible to search in all request offerings using the new _All _category in the New Request Wizard.

[![Request Management for WAP Search Demo](/images/RequestManagementForWap_Demo5.png)](/images/RequestManagementForWap_Demo5.png)


#### Resource Management


Launching a Request Offering from an Action now renders the Request Offering in a new Wizard specially built for Action Requests. This makes it possible to associate resources directly to the request (e.g. pre-fill user name as shown below)

[![Request Management for WAP Password Demo](/images/RequestManagementForWap_Demo3-1024x839.png)](/images/RequestManagementForWap_Demo3.png)

Furthermore, it is now possible to provide a UPN domain to NetBIOS domain name mapping which is used when automatically creating new users in the CMDB.
