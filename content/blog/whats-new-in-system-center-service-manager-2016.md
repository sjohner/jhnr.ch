---
author: sjohner
comments: true
date: 2016-10-12 21:52:02+00:00
layout: post
slug: whats-new-in-system-center-service-manager-2016
title: What's new in System Center Service Manager 2016
wordpress_id: 1766
categories:
- Service Manager
tags:
- Code-Behind
- Data Warehouse
- Database
- dll file
- HTML
- Operations Management Suite
- Performance
- SCSM
- SCSM Console
- SCSM Portal
- Self-Service Portal
- Service Manager
- Service Manager 2012 R2
- Service Manager 2016
- SQL
- System Center
- Upgrade
- XAML
---

Today, Microsoft announced the GA (General Availability) of[ Windows Server 2016](https://blogs.technet.microsoft.com/hybridcloud/2016/10/12/another-big-step-in-hybrid-cloud-windows-server-2016-general-availability/) and [System Center 2016](https://blogs.technet.microsoft.com/hybridcloud/2016/10/12/managing-the-software-defined-datacenter-with-system-center-2016/). Those of you interested in System Center Service Manger may wonder what's new in this long-awaited release.

Inspired by the GA of Service Manager 2016, I decided to create a blog post series called "Installing Service Manager 2016" to get you some first impressions on Service Manager 2016 and to create an up-to-date installation guide for this release. This is the first part the series and will cover what's new in Service Manager 2016.



 	
  * Installing Service Manager 2016 Part 1: What's new (this post)

 	
  * [Installing Service Manager 2016 Part 2: Primary Management Server](https://blog.jhnr.ch/2016/10/14/installing-service-manager-2016-with-sql-2016-on-windows-server-2016/)

 	
  * [Installing Service Manager 2016 Part 3: Self Service Web Portal](https://blog.jhnr.ch/2016/10/25/installing-service-manger-2016-self-service-portal/)

 	
  * [Installing Service Manager 2016 Part 4: Data Warehouse Management Server](https://blog.jhnr.ch/2017/01/19/installing-service-manager-2016-data-warehouse-management-server/)

 	
  * [Installing Service Manager 2016 Part 5: Scripting the installation](https://blog.jhnr.ch/2017/08/24/service-manager-installation-by-using-a-powershell-script/)


[![Service Manager 2016 Install](/images/SCSM2016_Install.png)](/images/SCSM2016_Install.png)

Besides support for SQL Server 2016 as well as Server 2016 (<del>of course you will need to install Windows Server with Desktop Experience</del>), most of the new features are hidden somewhat under the hood.

**Update:** As described in [System requirements for System Center 2016 - Service Manager](https://docs.microsoft.com/en-us/system-center/scsm/sm-sys-reqs), Service Manager 2016 supports both Windows Server 2016 and Windows Server 2016 with Desktop Experience.

For example data processing throughput has been improved massively since Service Manager 2016 now better utilizes SQL Server:



 	
  * Time to create and update work items was greatly reduced

 	
  * Workflows in Service Manager should have less latency and should catch up faster when you do experience latency.

 	
  * Service Manger can more easily handle a large inflow of 45 work items per minute.

 	
  * Group and queue calculations were improved significantly

 	
  * You can now disable ECL logging for faster SCCM and Active Directory connector sync

 	
  * Incident-related workflows have been optimized to reduce the amount of time to update incidents

 	
  * The AD GroupExpansion functionality is now part of the ADConnector


My first impression is that performance improvements in Service Manager 2016 really make a difference when using the console. For example switching between workspaces and views seems to be noticeably faster now.

Besides the above mentioned performance improvements, other features have been released with Service Manager 2016:

 	
  * Service Manager Data Warehouse cubes contain new date dimensions

 	
  * New HTML5 Self-Service portal (which was already available for Service Manager 2012 R2)

 	
  * Spell check is now enabled for work item forms

 	
  * A new console task _Open Activity in Progress_, was added for service requests and change requests

 	
  * Support for Lync 2013 and Skype for Business

 	
  * Setup changes to support SQL AlwaysOn installations

 	
  * Support for .NET Framework 4.5.1 (you still need to install .NET Framework 3.5 though and it has some impacts on your existing Service Manager customizations)

 	
  * [Microsoft Monitoring Agent is now included in version 7.5.x which allows direct connections to Operations Management Suite (OMS)](https://blog.jhnr.ch/2016/11/10/connect-service-manager-2016-management-server-to-operations-management-suite/)


Service Manger 2016 also ships with a new version of the Service Manager Authoring Tool. However don't get too exited about this one. Basically nothing changed. I assume it was only updated to reflect the support of .NET Framework 4.5.1.

If you are thinking about upgrading your existing Service Manager environment, make sure you check out [Upgrading System Center 2012 R2 - Service Manager to System Center 2016](https://docs.microsoft.com/en-us/system-center/scsm/upgrade-to-sm-2016). Also, the newly introduced support for .NET Framework 4.5.1 may have a massive impact if you customized your existing Service Manager environment (and who didn't?). Make sure you check out the [Upgrade steps for custom development blog post](https://blogs.technet.microsoft.com/servicemanager/2016/08/03/scsm-2016-upgrade-steps-for-custom-development/) on the official Service Manger blog.

With the 2016 release the below mentioned components have been deprecated are not supported anymore. Also see [Features Removed or Deprecated from System Center 2016](https://docs.microsoft.com/en-us/system-center/features-removed-or-deprecated) for more information.



 	
  * The Silverlight based Self Service Portal was replaced by the new HTML based portal

 	
  * Microsoft IT GRC Process Management Pack

 	
  * Service Manager Cloud Service Process Pack for which it is recommended to use Windows Azure Pack instead


For a list of useful Service Manager 2016 related links I'd recommend checking out [this post](http://www.scsm.se/?p=1527) by [Anders Asp](https://twitter.com/ScsmSe)
