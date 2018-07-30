---
author: sjohner
comments: true
date: 2017-09-24 19:22:15+00:00
slug: use-azure-cli-with-corporate-proxy-server
title: Use Azure CLI 2.0 behind corporate proxy server
categories:
- Azure
- Cloud
tags:
- Azure CLI
- Azure PowerShell
- Command Line
- Open Source
- Proxy
---

There exist different options to script control, modify and automate your Azure environment. The most popular one is probably [Azure PowerShell module](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-4.3.1). However there is another good option to consider using when managing your Azure environment: [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

Azure CLI is [open source](https://github.com/dotnet/cli) and built on Python which offers good cross-platform capabilities.The cool thing about Azure CLI is that you can use it with pretty much all known platforms like macOS, Windows and Linux. This means you do not have to learn another command line tool for each platform. And the best: You can use it in your browser with [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) (which is actually also [possible with PowerShell as announced at Ignite 2017](https://azure.microsoft.com/en-us/roadmap/powershell-in-azure-cloud-shell/))

I have been using Azure CLI for a while now and when using it behind corporate proxy servers it gets a little clumsy. For some time Azure CLI was [not supported behind corporate proxies](https://github.com/Azure/azure-xplat-cli/wiki/Supporting-Corporate-Proxy) at all. However it seems that this was fixed in [2015.03.04 Version 0.8.16](https://github.com/Azure/azure-xplat-cli/blob/5996027cdba13b7ad7abd194aa3dece65dcb15e5/ChangeLog.md#20150304-version-0816) already but is not reflected on the GitHub project wiki

Due to the authentication schematics of Azure Service, Azure CLI needs to pass an authentication payload through the HTTPS request, which will be denied at authentication time at your corporate proxy. By executing Azure login you will receive a TIMEOUT message- this is expected.

[![](/images/AZ_Login_Timeout.png)](/images/AZ_Login_Timeout.png)

    
    Please ensure you have network connection. Error detail: HTTPSConnectionPool(host='login.microsoftonline.com', 
    port=443): Max retries exceeded with url: /common/oauth2/devicecode?api-version=1.0 
    (Caused by NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection object at 0x04C655D0>: 
    Failed to establish a new connection: [WinError 10060]


This can be easily solved by setting *HTTPS_PROXY* environment variable. If you don't know what proxy you are using, check out [this superuser answer](https://superuser.com/questions/346372/how-do-i-know-what-proxy-server-im-using#346376).

    
    set HTTPS_PROXY=http://proxy.jhnr.ch:8080


After setting *HTTPS_PROXY* in your command prompt you should be able to successfully execute _az login_ and connect to your Azure environment.

[![](/images/AZ_Login_Success.png)](/images/AZ_Login_Success.png)

For a permanent solution just add the above variable to your environment variables. Note that changing the environment variables requires a reboot of your terminal to take effect.

[![HTTPS_PROXY environment variable](/images/https_proxy-environment-variable.png)](/images/https_proxy-environment-variable.png)

