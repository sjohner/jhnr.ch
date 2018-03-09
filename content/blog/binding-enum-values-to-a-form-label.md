---
author: sjohner
comments: true
date: 2015-11-25 21:06:52+00:00
layout: post
slug: binding-enum-values-to-a-form-label
title: Binding Enum Values to a form label
wordpress_id: 1196
categories:
- Service Manager
tags:
- Custom Form
- Enumeration
- Management Pack
- SCSM Console
- Service Manager
- Service Manager 2012 R2
- System Center
---

There are several blog posts out there which describe how to extend a default form with other information. One popular example is the indication of VIP users by either binding to a [text](http://blogs.technet.com/b/servicemanager/archive/2011/04/06/indicating-vips-through-form-customization.aspx) or [boolean](https://systemcenternoise.wordpress.com/2015/02/18/scsm-vip-user-solution/) value available on the selected user object.

If you read through the above noted posts you will notice that extending Service Manager forms is not a big thing and can be pretty handy to put something to attention of the analyst. Instead of binding a label to a property of a related object one can also bind it to a property of the same object of course.<!-- more -->

With this you can easily present information on the _General _tab of an Incident which is normally hidden on an other tab.

To illustrate this I created kind of a sample Management Pack which contains an Incident form extension which shows the selected Incident tier queue enum right below the Affected User.

If you are interested, you can [download the Management Pack on Technet Gallery ](https://gallery.technet.microsoft.com/Enum-Label-Binding-Example-dba245d1)or [GitHub](https://github.com/sjohner/scsm-repository/blob/master/managementpacks/jhnr.IncidentTierQueueLabel.xml). Feel free to extend and change it so that it fits your needs :-)

![738607](/images/738607.png)
