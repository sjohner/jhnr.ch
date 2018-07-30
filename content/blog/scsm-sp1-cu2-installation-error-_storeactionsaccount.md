---
author: sjohner
comments: true
date: 2013-05-21 12:07:54+00:00
layout: post
slug: scsm-sp1-cu2-installation-error-_storeactionsaccount
title: SCSM SP1 CU2 Installation Error _StoreActionsAccount
categories:
- Service Manager
tags:
- Admin Group
- CU2
- Error
- Management Group
- Registry
- System Center Management service
---

As I was installing CU2 lately on a SCSM management server of one of our customers I experienced a strange error. Since I did not find much about this error on the internet I wanted to share the error and solution in case anyone else came across the problem. I was able to successfully update the Data Warehouse management and Self-Service portal servers, without any issues. However, on the primary management server the update failed and provided the following error.

[![SCSM CU2 Error](/images/error.png?w=696)](/images/error.png)

After some troubleshooting I realized that the System Center Management service was stopped for some reasons and could not be started. Instead it provided the following error message:

[![Error System Center Management Service Startup](/images/errorservicenotstarted.png)](/images/errorservicenotstarted.png)

The System Event Log showed the following error

[![System Event Log Service terminated](/images/eventlogserviceterminated.png?w=696)](/images/eventlogserviceterminated.png)

After I compared some Registry Keys with a running SCSM installation I recognized that there were some missing on my system. Namely the Registry Key for the Management Group was missing. I imported the key form a running system and changed the name of the key to the name of the Management Group and also the Management Group ID.

Both, name and ID can be found by querying the SCSM database. Just select top 1000 rows from _dbo.__MOManagementGroup___ table to get management group name, id and also SCSM admin group of your SCSM installation.

**EDIT: **Importing the registry keys from another system may not be enough to get the System Center Management Service up and running properly. I was told by a Microsoft Support Professional that the **WindowsAccountLockDownSD** entry, which is located in the specific management group key, contains binary data which varies from system to system. **Do not override this key if it already exists. **If it doesn't and you import it from another system you may end up by getting the following error in event log:

_The Management Group <ManagementGroupName> failed to start.  The error message is The environment is incorrect.(0x8007000A).  A previous message with more detail may have been logged._

[![Get Management Group Name from SQL DB](/images/getmgmtgroupnameandid.png?w=696)](/images/getmgmtgroupnameandid.png)

My Registry entry now looked basically like the following

[![Management Group Registry Key](/images/registrykey.png?w=696)](/images/registrykey.png)

Unfortunately importing the Registry entry for the Management Group was not enough because after another restart of the System Center Management service I got the following message in Operations Manager Event Log followed by an error that no Management Group could be started.

[![Event Log AD Integration Disabled](/images/eventlogadintegrationdisabled.png?w=696)](/images/eventlogadintegrationdisabled.png)

I remembered another Registry Key called _Approved AD Management Groups_ which I've seen when I was exporting the previous one. After checking on my other SCSM installation I recognized that there exists the same key but the value EnableADIntegration is set to 1 instead of 0.

[![AD Integration Registry Key](/images/adintegrationregistrykey1.png?w=696)](/images/adintegrationregistrykey1.png)

This finally did the trick. After setting EnableADIntegration to 1 I was able to successfully start the System Center Management service and to install Service Manager 2012 SP1 CU2.
