---
author: sjohner
comments: true
date: 2015-01-21 20:00:00+00:00
layout: post
slug: scsm-dwh-installation-unable-to-connect-to-sql-reporting-services
title: 'SCSM DWH Installation: Unable to connect to SQL Reporting Services'
categories:
- Service Manager
tags:
- Authentication
- Data Warehouse
- Database
- Debugging
- Error
- Reporting
- Reporting Services
---

Today I experienced a kind of Reporting Services configuration problem when installing a new Service Manager Data Warehouse Management Server. When clicking through the install wizard I reached the point where I had to specify SQL Report Server and the corresponding Web Service URL. That was the point where I hit the following error: _The request failed with HTTP status 401: Unauthorized._

[![Service Manager Install Error](/images/installerror.png?w=660)](/images/installerror.png)

Hmm... The error message seems to be pretty clear. My user account does not seem to be authorized for SQL Reporting Services. Ok let's see if I am able to connect to Report Server by using a browser.

[![IElogin](/images/ielogin.png?w=660)](/images/ielogin.png)

Uups, that doesn't work either. Entering username and password does not help. But when opening the reports website directly on the server running Report Server, it worked. So it seems my user actually is allowed to use Reporting Services but it somehow isn't authenticated correctly.

After digging into SQL Reporting Services a bit more, I noticed that although the Report Service runs under a domain user account.

[![ServiceAccount](/images/serviceaccount.png?w=660)](/images/serviceaccount.png)

After reading some facts about Reporting Services authentication options, I took a look at the _rsreportserver.config_ file to check if there are some authentication options configured which may cause issues in this configuration. And in point of fact I found that authentication settings contain _<RSWindowsNegotiate/>_ as an authentication option.

[![rsreportserver.config-wrong](/images/rsreportserver-config-wrong.png?w=660)](/images/rsreportserver-config-wrong.png)

As explained in MSDN articles [here ](https://msdn.microsoft.com/en-us/library/cc281253.aspx)and [here](https://msdn.microsoft.com/en-us/library/cc281253.aspx#proxyfirewallRSWindowsNegotiate), using _RSWindowsNegotiate_ will result in a Kerberos authentication error if you configured the Report Server service to run under a domain user account and you did not register a Service Principal Name (SPN) for the account. If you don't want to use to use NTLM, remove RSWindowsNegotiate from the _RSReportServer.config_ file and verify that only RSWindowsNTLM is specified. If you choose this approach, you can continue to use a domain user account for the Report Server service even if you do not define an SPN for it.

So when experiencing this error you can choose whether you want to create an SPN for the domain user account which runs Report Server or to modify authentication options not to include _RSWindowsNegotiate._

Since normally when installing Reporting Services with a domain user account, this _RSWindowsNegotiate_ authentication option is not included in _RSReportServer.config_ I assume that Report Services were initially installed using a local user account. So when switching from local account to a domain user account for running Report Server service, think about either changing authentication options or creating SPNs :-)


