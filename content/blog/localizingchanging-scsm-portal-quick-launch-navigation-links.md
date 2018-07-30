---
author: sjohner
comments: true
date: 2013-03-25 22:25:25+00:00
slug: localizingchanging-scsm-portal-quick-launch-navigation-links
title: Localizing/Changing SCSM Portal quick launch navigation links
categories:
- Service Manager
tags:
- Language Pack
- Localization
- SCSM Portal
- Sharepoint
---

In Switzerland a total of four languages are spoken. Beside German, Italian and French there is also a fourth language called Romansh (see also [http://en.wikipedia.org/wiki/Romansh_language](http://en.wikipedia.org/wiki/Romansh_language)) which only a few people speak. However, because there are multiple languages spoken in Switzerland it is common that we have to localize System Center Service Manager portal. There are already many blog posts (including this one from Travis Wright [http://blogs.technet.com/b/servicemanager/archive/2012/02/14/how-to-select-the-portal-language-in-scsm-2012.aspx](http://blogs.technet.com/b/servicemanager/archive/2012/02/14/how-to-select-the-portal-language-in-scsm-2012.aspx)) about localizing Sharepoint by installing the appropriate language packs. However, installing the language packs does not localize the navigation links like _Help Articles_, _My Request_ and _My Activities_ located in the quick launch section of the SCSM portal.

Before changing the text of such a navigation link, select language you want to change the quick launch link text for.

[![Change Language in SCSM Portal](/images/changelanguage1.png?w=560)](/images/changelanguage1.png)

Go to Site Actions - Site Settings on your Sharepoint Portal

[![Go to Sharepoint Site settings](/images/gotositesettings.png)](/images/gotositesettings.png)

Under the settings for customizing the look and feel of the Sharepoint site, choose Quick launch.

[![Sharepoint Site settings](/images/sitesettings.png?w=560)](/images/sitesettings.png)

Now you'll see your existing navigation links which you can edit by clicking the image right beside the link.

[![Quick Launch Sidebar](/images/quicklaunch.png?w=300)](/images/quicklaunch.png)

Change the description of the link to the text you want to see for the language you selected before on the portal.

[![Edit Navigation Link](/images/editnavigationlink.png?w=560)](/images/editnavigationlink.png)

That's it, your navigation links on the quick launch sidebar should now show the new description.

[![SCSM Portal localized](/images/homelocalized.png?w=560)](/images/homelocalized.png)
