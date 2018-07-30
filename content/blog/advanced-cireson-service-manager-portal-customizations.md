---
author: sjohner
comments: true
date: 2016-07-04 18:21:00+00:00
slug: advanced-cireson-service-manager-portal-customizations
title: Advanced Cireson Service Manager Portal Customizations
categories:
- Service Manager
tags:
- Cireson
- Java Script
- Self-Service Portal
- Service Catalog
- Service Manager
- Service Manager 2012 R2
- Sharepoint
- Shop
---

You probably know that Service Manager has some drawbacks when it comes to the Self-Service portal. Microsoft recently released a new HTML 5 Self-Service portal which is actually a big step forward compared to the Silverlight based portal delivered out of the box with Service Manager 2012. However many customers evaluating ITSM tools are still surprised that there is no better out of the box portal available. Compared to the other ITSM product vendors, the Service Manager portal is definitively not what you are expecting from a state of the art ITSM software.

But since Service Manager really is a flexible and customizable product, it offers you an API which can be used to interact with 3rd party products. This is what some Microsoft Partners are using when creating their very own version of a Service Manager portal.

This really gives you the flexibility to choose from a variety of portal solutions. However you will have to decide for your self what solution suits your needs best. For this post I will concentrate on the [Cireson Self-Service Portal](http://cireson.com/apps/self-service-portal/). But Cireson is not the only company offering a replacement for the Service Manager portal. Other Self-Service portal solutions are offered by [itnetX](http://itnetx.ch/products/itnetx-itsm-portal/) as well as [Cased Dimensions](http://www.caseddimensions.com/scsm_self_service_portal/).

One of our customers decided to go with the Cireson portal as portal solution for his Service Manager environment. However, this portal still did not fit all their needs unfortunately. By default, the portal for example does not provide a shop-like experience. There is no shopping basket and also not images can be shown for items.

However, the Cireson portal provides a neat way to customize quite a lot of things. You can customize the style of the portal using CSS. These customization changes are saved in a dedicated folder and applied on top of the default style. They will always override what is provided out of the box even if the out of the box CSS code changes. Details can be found on the [Cireson support page](https://support.cireson.com/KnowledgeBase/View/43#/).

Luckily a talented web developer works for the above mentioned customer :-) With his permission, I want to share some of the customizations which he made to boost their Cireson portal experience. All credits for the following portal customizations go to [Manuel Kammermann](https://twitter.com/kimamil). If you need to know more details about the below customizations, reach out to Manuel. I am sure he is happy to help.

### Service Catalog

Normally the first thing a user sees when connecting to the Cireson portal is the Service Catalog. Service Catalog experience can be customized pretty heavily by changing colors, fonts and also some parts of the layout. With this in mind you can create your own custom portal experience according to your company's corporate identity. As you see below, colors, fonts were changed and the search bar was resized. Also, in addition to the Request Offering's name, users can now also click the Request Offering's image to access the corresponding form.

[![Cireson Portal Service Catalog](/images/CiresonPortalCatalog-1024x566.png)](/images/CiresonPortalCatalog.png)

Furthermore, Knowledge Base articles are hidden by default to save some space. Users can toggle them by clicking _Show How To's_ below the categories navigation pane.

[![Cireson Portal Knowledge Base](/images/CiresonPortalKnowledgeBase-1024x562.png)](/images/CiresonPortalKnowledgeBase.png)

### Images for Request Offerings

When it comes to Request Offerings where users can order hardware and applications, it is often requested to show some images representing the corresponding item. As already mentioned, the Cireson portal out of the box does not support images in Request Offerings. However, one can achieve this by using some Java Script magic which shows appropriate image located on the file system based on the filename.

Along with images,

[![Cireson Portal Images](/images/CiresonPortalImages-1024x546.png)](/images/CiresonPortalImages.png)

### Order Summary

Unfortunately the Cireson portal lacks some functionality when it comes to ordering items with a shop like experience, where users have kind of a shopping basket to keep track of their order. This can be (partly) solved by customizing the portal. As you see below, users can now see exactly what items they selected for a given request. What's still missing unfortunately, is the possibility to remove items directly from the basket.

[![Cireson Portal Order Summary](/images/CiresonPortalOrderSummary-1024x563.png)](/images/CiresonPortalOrderSummary.png)

### Activity Status Highlighting

Users and Analysts can now see at first sight what activities have which status.

[![Cireson Portal Activity Status](/images/CiresonPortalActivityStatus-1024x661.png)](/images/CiresonPortalActivityStatus.png)