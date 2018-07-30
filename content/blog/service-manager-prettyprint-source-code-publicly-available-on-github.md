---
author: sjohner
comments: true
date: 2016-06-03 15:34:24+00:00
layout: post
slug: service-manager-prettyprint-source-code-publicly-available-on-github
title: Service Manager PrettyPrint Source Code publicly available on GitHub
categories:
- Service Manager
tags:
- Custom Form
- Management Pack
- PrettyPrint
- Printer
- SCSM Console
- Service Manager
- Service Manager 2012 R2
- Visual Studio
---

As you may know, I published PrettyPrint for Service Manager back in 2014. Since then, I got a lot of positive feedback for this Service Manager extension and I thought that it would be a good idea to publish PrettyPrint source code, so everyone can profit from and contribute to the project.

You can find the [source code on GitHub](https://github.com/sjohner/SCSM-PrettyPrint/). It is published under [GNU General Public License](http://choosealicense.com/licenses/gpl-3.0/). Please feel free to check out and extend the code with your own ideas. If you would like to contribute to the project, let me know. I would be happy to add you as contributor. If you find any bugs or would like to create a feature request, please report it on the [GitHub issues page](https://github.com/sjohner/SCSM-PrettyPrint/issues). Please remember that I am not a developer, so there may be room for improvement regarding code quality ;-)

For those of you who don’t know yet about PrettyPrint, I will try to explain in short what it does: The PrettyPrint app allows console users to print specific Incident, Service Request and Change Request information using a custom Microsoft Word template. The solution uses Microsoft Office Interop assemblies to create a new Microsoft Word document from a given template and extends the document with values of a selected Incident, Service Request or Change Request. This newly generated document can then be shown to the user in the Word print preview mode.

[![PrettyPrint Word Print Preview](/images/prettyprintwordpreview.png)](/images/prettyprintwordpreview.png)
