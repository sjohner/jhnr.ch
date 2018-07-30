---
author: sjohner
comments: true
date: 2014-12-23 16:27:58+00:00
slug: resolve-sql-server-service-not-running-when-installing-service-manager-database-on-sql-alwayson-availability-group
title: Resolve "SQL Server service not running" when installing Service Manager database
  on SQL AlwaysOn Availability Group
categories:
- Service Manager
tags:
- AlwaysOn
- Availability Group
- Database
- Error
- Service Manager 2012 R2
- SQL
---

As you may know, there are some things to remember when installing Service Manager database in a SQL AlwaysOn cluster environment. For example SQL Server AlwaysOn Availability Groups functionality is supported by all versions of Service Manager, but only for the default server instance. Also those of you who tried to install Service Manager in a SQL AlwaysOn environment may have experienced the following error when it comes to configuring the database part in the Service Manager install wizard

**A required SQL Server service is not running on <YourSqlServer> MSSQLServer**

![ServiceNotRunningError](/images/servicenotrunningerror.png)

Of course you have SQL server up and running on your SQL cluster nodes and the necessary services are running fine. Also you may notice that if you don't use your SQL Availability Group listener but the IP address of one of your SQL cluster nodes, the install wizard will let you continue with Service Manager setup.

Some posts out there are suggesting installing Service Manager database to one of the cluster nodes and switching database server name in registry after a successful installation. And although this perfectly works, there exists a more comfortable way to work around this issue.

To be able to continue with Service Manager setup you have to add the following key to the registries of your SQL AlwaysOn cluster nodes. According to a [Microsoft article](http://support2.microsoft.com/default.aspx?scid=kb;EN-US;306985)  the _NoRemapPipes_ registry key controls how named pipes are opened on the network that is managed by the Network Name resource. If RemapPipeNames is set to TRUE, named pipes are always opened by using local pipe names.

**Key:** *HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesLanmanServerParameters*
**Name:** *NoRemapPipes*
**Type:** *REG_MULTI_SZ*
**Data:** *winreg __svcctl __eventlog*

![RegChange](/images/regchange.png?w=604)

Hope this helps :-) Merry Christmas and a happy New Year to all of you!
