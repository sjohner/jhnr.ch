---
author: sjohner
comments: false
date: 2013-11-13 16:02:15+00:00
layout: post
slug: quick-post-how-to-find-service-manager-database-server-name-in-registry
title: 'Quick Post: How to find Service Manager database server name in registry'
wordpress_id: 756
categories:
- Service Manager
tags:
- Data Warehouse
- Database
- Registry
- Reporting
---

Have you ever wondered how you can find the Service Manager database server which is used in your Service Manager environment? This can be done easily by querying the registry on one of your SCSM management servers.




The registry key _HKEY_LOCAL_MACHINESOFTWAREMicrosoftSystem Center2010CommonDatabase _leads you to the corresponding values _DatabaseName _and _DatabaseServerName_




[![Registry SCSM Database](/images/registryscsmdatabase.png?w=604)](/images/registryscsmdatabase.png)




The same registry key found on the **Data Warehouse Management Server** contains the values for the corresponding Data Warehouse databases together with the DWH database server. Furthermore, the key _HKEY_LOCAL_MACHINESOFTWAREMicrosoftSystem Center2010CommonReporting _contains the reporting server name as well as the web service URL.



