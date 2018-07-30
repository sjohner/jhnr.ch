---
author: sjohner
comments: true
date: 2014-11-12 21:30:45+00:00
layout: post
slug: updated-service-manager-prettyprint-app-released
title: Updated Service Manager PrettyPrint app published on Technet Gallery
categories:
- Service Manager
tags:
- PrettyPrint
- Printer
- Printing
- Technet Gallery
---

It's been a while since my last blog post. I've been pretty busy this summer :-) As some of you may have noticed, I published a Service Manager app called [PrettyPrint on Technet Gallery](https://gallery.technet.microsoft.com/Service-Manager-PrettyPrint-0e1808b1). I announced the first release in September at [System Center Universe Europe](http://www.systemcenteruniverse.ch/) in my session with [Marcel Zehner](http://twitter.com/marcelzehner) about Service Manager customizations. The SCU session recording can be found on [Channel9](http://channel9.msdn.com/Series/SCUE2014/Customer-requirements-first-Service-Manager-customizations-without-limits).

The now released version fixes a bug where the PrettyPrint task failed with the following message when starting from a form and if the settings were configured **not to** show Word print preview. This was actually a pretty annoying bug and to fix this I had to use a custom print dialog. This is because the WPF PrintDialog is missing an owner property which [makes it unusable from UI threads other than the main UI thread](https://connect.microsoft.com/VisualStudio/feedback/details/462054/wpf-printdialog-is-missing-owner-property-making-it-unusable-from-ui-threads-other-than-main-ui-thread) (the Service Manager console main window in this case).

[![PrettyPrintErrorThread](/images/prettyprinterrorthread.png)](/images/prettyprinterrorthread.png)

Besides this bugfix, **PrettyPrint now also supports Change Requests.**

For all of you who don't know yet about PrettyPrint, I will try to explain in short what it does. If you have any suggestions on how to improve this app, please let me know :-)

The PrettyPrint app allows console users to print specific Incident, Service Request and Change Request information using a custom Microsoft Word template. The solution uses Microsoft Office Interop assemblies to create a new Microsoft Word document from a given template and extends the document with values of a selected Incident, Service Request or Change Request. This newly generated document is afterwards shown to the user in the Word print preview mode.

[![PrettyPrint Word Print Preview](/images/prettyprintwordpreview.png?w=604)](/images/prettyprintwordpreview.png)

To design your templates, you can make use of bookmarks to add placeholders for Incident, Service Request and Change Request properties you want to show in your document. PrettyPrint will work with out of the box class properties as well as with extended properties.

[![PrettyPrint Incident](/images/prettyprintincident.png?w=604)](/images/prettyprintincident.png)

The app also provides a Service Manager Administration Setting, which can be used to configure separate template paths for Incidents, Service Requests and Change Requests respectively. Furthermore, PrettyPrint can be configured to show Word Print Preview or just a simple Print Dialog.

[![PrettyPrint Settings](/images/prettyprintsettings.png?w=604)](/images/prettyprintsettings.png)

Also, if you are unsure about what properties can be used as Bookmarks or how they are named, you will find all the necessary information in PrettyPrint Settings when clicking one of the appropriate buttons. This will give you an overview of all the property names as well as component aliases which are available for a specific work item. Make sure you use the _Name_ attribute for properties and _Alias _for components as your bookmarks in the Word template.

![PrettyPrint Incident Properties](/images/prettyprintincidentproperties.png)

The [download on Technet Gallery](https://gallery.technet.microsoft.com/Service-Manager-PrettyPrint-0e1808b1) contains a documentation file where all the necessary information concerning installation and usage of the app can be found. If you find any bugs or have any suggestions on how to improve this app, please let me know :-)