---
author: sjohner
comments: true
date: 2013-01-17 22:22:53+00:00
slug: how-to-embed-files-in-management-pack-bundle-with-visual-studio-authoring-extensions-vsae
title: How to embed Files in Management Pack Bundle with Visual Studio Authoring Extensions
  (VSAE)
categories:
- Service Manager
tags:
- Management Pack
- Management Pack Bundle
- Visual Studio
- VSEA
---

Recently I wrote a [post](http://scsmlab.com/2013/01/03/include-files-in-management-pack-bundle-with-vsae/) about adding dll resource files to a Management Pack Bundle with Visual Studio Authoring Extensions (VSAE). Now this only works for .mp and .dll files. If you want to include for example an image file to your Management Pack Bundle which is going to be displayed as a Folder Icon, you first have to add the file to your Management Pack Project.

[![Add Existing Item](/images/addexistingitem.png?w=460)](/images/addexistingitem.png)

Now if you build your Management Pack Project the file is not yet included in the Management Pack Bundle. You have to change the _Build Action_ property of the newly added file from _Content _to _Embedded Resource_ to achieve this.

[![Embedded Resource Property](/images/embeddedresource.png?w=426)](/images/embeddedresource.png)

Hope this helps!
