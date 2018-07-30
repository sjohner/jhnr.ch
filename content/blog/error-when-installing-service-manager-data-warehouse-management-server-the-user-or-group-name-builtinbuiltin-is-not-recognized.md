---
author: sjohner
comments: true
date: 2016-11-08 22:53:40+00:00
layout: post
slug: error-when-installing-service-manager-data-warehouse-management-server-the-user-or-group-name-builtinbuiltin-is-not-recognized
title: 'Error when installing Service Manager Data Warehouse management server: "The
  user or group name ''BUILTIN\BUILTIN'' is not recognized."'
categories:
- Service Manager
tags:
- Data Warehouse
- Debugging
- Error
- Report Server
- Reporting
- Service Manager
- Service Manager 2012 R2
- Service Manager 2016
---

When installing Service Manager Data Warehouse management server you might run into an issue where the installation fails when it comes to configuring SQL server report server. So far I was able to reproduce this with Service Manager 2016 Data Warehouse management server installation but it might be also applicable for Service Manager 2012 version.

[![DWH Installation Error](/images/SCSM_DWH_ReportingError.png)](/images/SCSM_DWH_ReportingError.png)

When you see the above error message, check the setup log (which can be found in 'C:\Users\<User>\AppData\Local\Temp') if you see the following error:


    AssignSsrsRole Error: System.Web.Services.Protocols.SoapException : System.Web.Services.Protocols.SoapException: The user or group name 'BUILTIN\BUILTIN' is not recognized. ---> Microsoft.ReportingServices.Diagnostics.Utilities.UnknownUserNameException: The user or group name 'BUILTIN\BUILTIN' is not recognized  .


In my case, setup had failed to add the specified Reporting Services account as _Publisher_ for the Service Manager folder in Reporting Services. When checking the Reporting Services security configuration for the newly created Service Manager folder, there was indeed an account named '_BUILTIN\BUILTIN'_ configured as Content Manager

[![Report Server BUILTIN\BUILTIN account](/images/ReportServerBuiltinAccount-1024x543.png)](/images/ReportServerBuiltinAccount.png)

It seems as if setup did somehow struggle with this account so I tried the obvious and changed folder security settings to be inherited from parent folder. I re-ran the Data Warehouse management server setup and this time it finished successfully.

However, the specified Reporting Services account was again not added to the configuration. Instead the mysterious _'BUILTIN\BUILTIN' _account showed up again. So I just deleted this one and added the Reporting Services account manually. I have no explanation on why this happens and I cant tell if it is only an issue with Service Manager 2016 and SQL Server 2016, but fortunately the fix is simple :-)
