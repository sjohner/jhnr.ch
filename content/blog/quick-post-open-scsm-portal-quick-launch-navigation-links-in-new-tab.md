---
author: sjohner
comments: true
date: 2014-01-27 16:55:15+00:00
slug: quick-post-open-scsm-portal-quick-launch-navigation-links-in-new-tab
title: 'Quick Post: Open SCSM Portal Quick Launch Navigation Links in new Tab'
categories:
- Service Manager
tags:
- Browser
- JavaScript
- SCSM Portal
- Sharepoint
---

One of my customers came up with the request to add some quick launch navigation links in Service Manager portal. Quick launch navigation links are located on the navigation pane on the Service Manager portal and reference, among other things, to _My Requests_ and _Help Articles_.

[![Portal Quick Launch Navigation Nodes](/images/quicknavigation.png)](/images/quicknavigation.png)

So far no problem because these navigation links can be added pretty straightforward by editing the portal's Sharepoint site. I wrote a [blog post](http://scsmlab.com/2013/03/25/localizingchanging-scsm-portal-quick-launch-navigation-links/) some time ago about how to localize these links. If you are interested in how to add a link, check out [this article](http://technet.microsoft.com/en-us/library/hh549264.aspx) on Microsoft TechNet.
However the customer wanted these links to open in a new browser tab instead of the active one. Unfortunately it is not possible do configure this behavior for a navigation link. There is no such option as "_Open in new Window"_. All you can configure is a URL, name and parent navigation node.

The solution to this problem can be found by using some JavaScript code. First I came up with the following to be defined in the URL field:

```javascript
javascript:window.open('https://blog.jhnr.ch');
```

Worked pretty well and the URL was opened in another browser tab. But there was one drawback with this - the active browser tab was not anymore showing the Service Manager portal page but this:

```javascript
[object]_
```

After a quick internet search, it turned out that solving this issue was pretty easy though. This problem occurs because the browser is navigating to the window object returned by the Window.open file call when using the above code in a HREF HTML tag. So according to a Microsoft Knowledgebase article ([http://support.microsoft.com/kb/257321](http://support.microsoft.com/kb/257321)) one can add void(0) to the statement to give a no return value.

```javascript
javascript:window.open('https://blog.jhnr.ch');void(0)
```

Et voilà, opening quick launch navigation links in another browser tab is no longer a problem, so everyone can now add a link to [https://blog.jhnr.ch](https://blog.jhnr.ch) to their Service Manager portal page ;-)
