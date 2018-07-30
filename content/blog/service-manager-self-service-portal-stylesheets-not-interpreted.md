---
author: sjohner
comments: true
date: 2016-05-17 17:01:23+00:00
layout: post
slug: service-manager-self-service-portal-stylesheets-not-interpreted
title: Service Manager Self-Service Portal Stylesheets not interpreted
categories:
- Service Manager
tags:
- CSS
- HTML
- IIS
- SCSM Portal
- Self-Service Portal
- Service Manager
- Technical Preview
---

When I recently installed Service Manager 2016 Technical Preview 5, I also installed the new out of the box Self-Service portal. However after installation finished successfully, I experienced an odd behavior: The portal did no show up in it's usual design.

As you can see below, the CSS stylesheets were transferred with the wrong MIME type _text/plain_ which caused the portal to be unusable for users.

[![SCSM HTML5 Portal without CSS](/images/PortalError-1024x497.png)](/images/PortalError.png)

Luckily the error pretty much tells you everything you need to know to resolve the issue :-)

In IIS, the _Static Content_ feature is responsible for serving the static contents of a website like images, scripts , style sheets, etc. The portal installer unfortunately does not seem to check whether the IIS HTTP Feature _Static Content_ is installed or not. Of course it was not...

[![Add Feature IIS Static Content](/images/AddFeatures_StaticContent.png)](/images/AddFeatures_StaticContent.png)

Installing the feature _Web Server (IIS) - Web Server - Common HTTP Features - Static Content _did the trick and the portal turned on the full power
of its personality :-)
