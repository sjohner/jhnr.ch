---
author: sjohner
comments: false
date: 2013-11-13 16:02:15+00:00
slug: quick-post-how-to-find-service-manager-database-server-name-in-registry
title: 'Quick Post: How to find Service Manager database server name in registry'
categories:
- Service Manager
tags:
- Data Warehouse
- Database
- Registry
- Reporting
---

Have you ever wondered how you can find the Service Manager database server which is used in your Service Manager environment? This can be done easily by querying the registry on one of your SCSM management servers.

The registry key `HKEY_LOCAL_MACHINE/SOFTWARE/Microsoft/System Center/2010/Common/Database` leads you to the corresponding values `DatabaseName` and `DatabaseServerName`

[![Registry SCSM Database](/images/registryscsmdatabase.png?w=604)](/images/registryscsmdatabase.png)

The same registry key found on the **Data Warehouse Management Server** contains the values for the corresponding Data Warehouse databases together with the DWH database server. Furthermore, the key `HKEY_LOCAL_MACHINESOFTWARE/MicrosoftSystem Center2010/Common/Reporting` contains the reporting server name as well as the web service URL.