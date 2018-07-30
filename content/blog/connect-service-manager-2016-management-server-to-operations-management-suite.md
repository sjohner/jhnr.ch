---
author: sjohner
comments: true
date: 2016-11-10 00:50:54+00:00
slug: connect-service-manager-2016-management-server-to-operations-management-suite
title: Connect Service Manager 2016 management server to Operations Management Suite
categories:
- Operations Managment Suite
- Service Manager
tags:
- Azure Log Analytics
- Cloud
- Microsoft Monitoring Agent
- OMS
- Operations Management Suite
- Operations Manager
- SCOM
- SCSM
- Service Manager
- Service Manager 2012 R2
- Service Manager 2016
- System Center
---

You are probably aware that since System Center 2012 R2, Service Manager comes with a pre-installed Microsoft Monitoring Agent (MMA) which is perfectly compatible to System Center 2012 Operations Manager. After completing Service Manger setup, you just have to manually configure the agent to communicate with your Operations Manager management server.

However in Service Manager, the Microsoft Monitoring Agent does not just calculate the health state of the monitored computer and reports back to the Operations Manager management server. Instead it's primary role is managing all the Service Manager workflows, including workflow subscriptions, group calculations, connectors, etc.

With this in mind, Microsoft published [a blog post](https://blogs.technet.microsoft.com/servicemanager/2016/09/20/monitoring-service-manager-with-microsoft-system-center-operations-manager/) recently which covers how to monitor Service Manager with Operations Manager and why it may be a good idea to go for agent-less monitoring for Service Manager management servers.

So that's basically it on the Operations Manager story. However with the general availability of Microsoft Operations Management Suite (OMS), the Microsoft Monitoring Agent was now able to connect directly to OMS. Unfortunately this was not the case for Service Manager 2012 R2 since this release was delivered with MMA version 6.x

Luckily this changed. It seems that the fact, that the Microsoft Monitoring Agent which comes pre-installed with Service Manger 2016 now supports connection to Operations Management Suite, was not actively communicated by Microsoft. At least it is not found in System Center 2016 release notes.

With Service Manager 2016, the Microsoft Monitoring Agent is now delivered in version 7.5.7487.0 which brings a new 'Azure Log Analytics (OMS)' tab in MMA properties.

[![Microsoft Monitoring Agent Version](/images/MMA_Version.png)](/images/MMA_Version.png)

So when using Service Manager 2016 you can easily connect your management servers to Microsoft Operations Management Suite.

[![Connect to Log Analytics](/images/MMA_ConnectOMS.png)](/images/MMA_ConnectOMS.png)

Simply enter your OMS workspace id and key and you should see your management server popping up in Operations Management Suite.

[![Monitoring Agent Log Search](/images/MMA_LogSearch-1024x376.png)](/images/MMA_LogSearch.png)
