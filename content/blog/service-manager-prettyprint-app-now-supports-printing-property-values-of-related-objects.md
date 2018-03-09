---
author: sjohner
comments: true
date: 2015-05-05 17:00:14+00:00
layout: post
slug: service-manager-prettyprint-app-now-supports-printing-property-values-of-related-objects
title: Service Manager PrettyPrint app now supports printing property values of related
  objects
wordpress_id: 1059
categories:
- Service Manager
tags:
- PrettyPrint
- SCSM Console
- SCSM Portal
- Service Manager
- Service Manager 2012 R2
- System Center
- Task
- Technet Gallery
---

I just uploaded a new version of [PrettyPrint app for Service Manager. ](https://gallery.technet.microsoft.com/Service-Manager-PrettyPrint-0e1808b1)I got some feedback from people using PrettyPrint to print out their Service Desk tickets. Of course when using PrettyPrint for such purpose it would be very handy to be able to print contact information of the affected user. This is now possible :-)

![PrettyPrint Affected User Phone Number](/images/prettyprintaffecteduserphonenumber.png?w=604)

<!-- more -->

If you want to print values of properties of a related item, this is now possible by defining a bookmark containing component _Alias _and corresponding property _Name_ separated by underscore. As an example you can define a bookmark called _AffectedUser_BusinessPhone _which prints the affected users business phone number. Check it out and let me know what you think about it.

Remember that for bookmarks, the name must start with a word character (but not a digit), then any Unicode word character may follow up to an overall length of 40 characters. Word characters explicitly exclude white space and punctuation of any kind.

For those of you who don’t know about [PrettyPrint](https://gallery.technet.microsoft.com/Service-Manager-PrettyPrint-0e1808b1) yet, read [one of my previous blog posts](http://scsmlab.com/2014/11/12/updated-service-manager-prettyprint-app-released/) which explains the functionality and requirements of this free Service Manager extension.
