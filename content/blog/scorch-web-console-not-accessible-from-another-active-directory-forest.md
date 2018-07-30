---
author: sjohner
comments: true
date: 2013-10-13 16:41:02+00:00
slug: scorch-web-console-not-accessible-from-another-active-directory-forest
title: SCORCH Web Console not accessible from another Active Directory forest
categories:
- Orchestrator
- Service Manager
tags:
- Active Directory
- Forest
- Orchestrator
- SCORCH Console
- Web Console
---

When I was working on a SCSM project for a customer, IT people were complaining that the Orchestrator Console was not accessible from their workstations. For some reasons they always got an error message from SCORCH when connecting to the Web Console. In this specific environment, Orchestrator was installed in a resource forest Active Directory environment. In a resource forest topology, one forest is dedicated to running server applications, such as Microsoft Exchange Server and in this case System Center components.

Everything was fine when Orchestrator Console was accessed from another workstation in the resource forest. However, IT Service Desk should be able to start Runbooks in SCORCH Console from another forest. Which however resulted in following error message:

`Error executing the current operation [Arg_SecurityException]`

[![SCORCH Console Error](/images/scorchconsoleerror1.png?w=696)](/images/scorchconsoleerror1.png)

The message was followed by another error message saying that either the service may be down, the service URL may be wrong or access is denied.

[![SCORCH Console Error](/images/scorchconsoleerror2.png?w=696)](/images/scorchconsoleerror2.png)

After some research I was able to track down the issue. It seems that Orchestrator Console calls the Web Service by using the Netbios name of the SCORCH server. However in this case the server was not accessible because the DNS suffix for the services forest was not configured.

So if you are having similar issues with Orchestrator, go and check your DNS suffix settings to make sure the SCORCH server is accessible by using its Netbios name.

By the way: Service Manager behaves pretty similar when a Data Warehouse is registered. If you notice a massage saying that _T__he Reporting Data Warehouse Management Server is currently unavailable _when opening the SCSM Console from a workstation, you probably have to check whether your DNS suffix configuration is correct. Check out this [blog post](http://ronnydejong.com/2011/08/23/reporting-data-warehouse-management-server-is-currently-unavailable-sysctr/) written by [Ronny de Jong](https://twitter.com/ronnydejong) back in 2011 for more details.
