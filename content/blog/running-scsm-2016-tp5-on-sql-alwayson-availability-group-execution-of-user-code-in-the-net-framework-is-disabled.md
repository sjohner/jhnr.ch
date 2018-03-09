---
author: sjohner
comments: true
date: 2016-08-03 20:05:47+00:00
layout: post
slug: running-scsm-2016-tp5-on-sql-alwayson-availability-group-execution-of-user-code-in-the-net-framework-is-disabled
title: Running SCSM 2016 TP5 on SQL AlwaysOn Availability Group - "Execution of user
  code in the .Net Framework is disabled"
wordpress_id: 1626
categories:
- Service Manager
tags:
- dll file
- Error
- SCSM Console
- Service Manager
- Service Manager 2016 TP5
- SQL
- Technical Preview
---

Now that the new Service Manager release is just around the corner, more and more customers are asking to deploy Service Manager 2016 TP5 in production. Benefits of deploying Service Manager 2016 TP5 are listed below. For a complete list of new features in Service Manager 2016 TP5, please see [https://technet.microsoft.com/en-us/system-center-docs/sm/get-started/what-s-new-in-service-manager](https://msdn.microsoft.com/en-us/library/ms131048(v=sql.110).aspx)



 	
  * New Self-Service Portal

 	
  * Performance improvements

 	
  * Support for Lync 2013 and Skype for Business

 	
  * Changes to Setup supporting SQL AlwaysOn Availability Groups installation

 	
  * Support for .NET Framework 4.5.1


One cool thing is that Setup now supports SQL AlwaysOn Availability Groups installation. No more hacks and workarounds when [Performing a fresh installation of Service Manager with SQL Server 2014.](https://blog.jhnr.ch/2016/01/15/performing-fresh-installation-of-service-manager-with-sql-server-2014/) :-)

First installation of SCSM TP5 on SQL AlwaysOn Availability Groups was done quickly and without any issues. However as soon as the Service Manager DB switched from the initial SQL AlwaysOn node to the other one, all my custom classes and forms stopped working. All I got was the following error message when creating a new object from SCSM console:

_Execution of user code in .NET Framework is disabled. Enable "clr enabled" configuration option._

[![CLR Error](/images/CLR_Error.png)](/images/CLR_Error.png)

The error message pretty much shows the solution to the problem. I checked both SQL Always On nodes to see if "clr enabled" configuration option is actually enabled.

[![CLR Configuration Option Disabled](/images/CLR_Configuration_Option_Disabled.png)](/images/CLR_Configuration_Option_Disabled.png)

As suspected, the option was only enabled on the one node which was the primary one at the time of Service Manager installation. It seems as if the SCSM 2016 TP5 installer does not configure "clr enabled" configuration option on all nodes of a SQL Always On cluster.

Luckily, enabling this configuration option is a piece of cake :-) The following SQL commands will enable "CLR Enabled". Check out [https://msdn.microsoft.com/en-us/library/ms131048(v=sql.110).aspx](https://msdn.microsoft.com/en-us/library/ms131048(v=sql.110).aspx) for more information

    
    sp_configure 'show advanced options', 1;
    GO
    RECONFIGURE;
    GO
    sp_configure 'clr enabled', 1;
    GO
    RECONFIGURE;
    GO


[![CLR Configuration Option Enabled](/images/CLR_Configuration_Option_Enabled.png)](/images/CLR_Configuration_Option_Enabled.png)

I will try to reach out to the product team to check with them if this is an isolated case or if this may happen for all installations on SQL Server 2014 AlwaysOn Availability Groups.
