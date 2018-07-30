---
author: sjohner
comments: true
date: 2016-10-25 21:28:50+00:00
slug: installing-service-manger-2016-self-service-portal
title: Installing Service Manger 2016 Self Service Portal
categories:
- Service Manager
tags:
- HTML
- PowerShell
- SCSM Portal
- Self-Service Portal
- Service Manager
- Service Manager 2016
- System Center
---

This is the third part of a blog post series called "Installing Service Manager 2016" and will cover how to install a secondary Service Manger 2016 management server and the HTML based Self Service portal on Windows Server 2016 with Desktop Experience.

* [Installing Service Manager 2016 Part 1: What's new](https://blog.jhnr.ch/2016/10/12/whats-new-in-system-center-service-manager-2016/)
* [Installing Service Manager 2016 Part 2: Primary Management Server](https://blog.jhnr.ch/2016/10/14/installing-service-manager-2016-with-sql-2016-on-windows-server-2016/)
* Installing Service Manager 2016 Part 3: Self Service Web Portal (this post)
* [Installing Service Manager 2016 Part 4: Data Warehouse Management Server](https://blog.jhnr.ch/2017/01/19/installing-service-manager-2016-data-warehouse-management-server/)
* [Installing Service Manager 2016 Part 5: Scripting the installation](https://blog.jhnr.ch/2017/08/24/service-manager-installation-by-using-a-powershell-script/)

In this blog post series I will describe the installation of Service Manager 2016 based on a pretty simple 3 computer scenario. The first computer hosts the Service Manager management server as well as the Service Manager database. The second computer hosts the Self Service Web Portal and the third one the data warehouse management server and the Data Warehouse databases.

[![Install Scenario](/images/BlogScenario2-1024x323.png)](/images/BlogScenario2-1024x323.png)

Of course there exists already a [pretty good manual](https://docs.microsoft.com/en-us/system-center/scsm/deploy-self-service-portal) on how to install the HTML based Self Service portal. However there are some points I want to highlight when installing the Service Manager portal.

For example the installation manual states that you should install the Self Service portal _ideally, on the primary or secondary server. _Well, that does not seem to be 100% correct because in some other scenarios (e.g. in [this install instructions](https://www.microsoft.com/en-us/download/details.aspx?id=54060) of a Self Service portal upgrade fix) Microsoft does explicitly advises against installing the portal on the primary management server.

**Installing the Self Service portal on the same computer as the primary management server is not recommended.**

So for the sake of completeness I will outline the installation of a secondary management server as well as the following installation of the Self Service portal.

If you are performing an in-place upgrade to Service Manager 2016 Self Service Portal, when [Service Manager 2012 R2 HTML based Self Service Portal (Update Rollup 8)](https://support.microsoft.com/en-in/kb/3096383) is installed on the same machine as Service Manager management server, please make sure you install [this Service Manager Self Service Portal upgrade fix](https://www.microsoft.com/en-us/download/details.aspx?id=54060).

# Install Service Manager Prerequisites

To be able to install a secondary Service Manager management server, we first need to make sure that some prerequisites are available. Basically the same prerequisites as for a primary management server have to be installed:

* .NET Framework 3.5
* Report Viewer 2008
* SQL Server 2014 Analysis Management Objects
* SQL Server 2012 Native Client

Check out the prerequisites section of the [Installing Service Manager 2016 with SQL 2016 on Windows Server 2016](https://blog.jhnr.ch/2016/10/14/installing-service-manager-2016-with-sql-2016-on-windows-server-2016/) blog post for more detailed information about installing necessary prerequisites.

Besides the above mentioned software perquisites, the Service Manager Service Account has to be member of the local Administrator group as we already know it from installing our primary server.

# Install Service Manager 2016

Now let's get started with Service Manager setup! If you installed all the necessary prerequisites, you should be good to go after entering product key and a successful prerequisite check.

Next specify SQL Server and Instance name where this secondary management server should connect to.

[![Select existing SQL DatabaseA](/images/SCSM2016_SecondarySQLServer.png)](/images/SCSM2016_SecondarySQLServer.png)

If the firewall on your SQL server is active, you will need to disable it or even better to create an appropriate inbound rule like the following to allow the installer to find the given SQL instance:

```powershell 
    New-NetFirewallRule -DisplayName “SQL Server” -Profile Domain -Direction Inbound –Protocol TCP –LocalPort 1433 -Action allow
```

Next up we will presented the management group name and management group administrators of the existing Service Manager environment

[![Management Group](/images/SCSM2016_SecondaryMgmtGroup.png)](/images/SCSM2016_SecondaryMgmtGroup.png)

Now enter the credentials of the Service Manager Service Account. The specified account becomes the Operational System Account. It will be assigned to the logon account for the System Center Data Access Service as well as for System Center Management Configuration service. _Note that the Service Account has to be member of the computers local administrators group._

[![Specify Service Account](/images/SCSM2016_ServiceAccount.png)](/images/SCSM2016_ServiceAccount.png)

That's pretty much it for the installation of a secondary management server. You do not necessarily need to backup your encryption key since we already did that when installing our primary management server.

There is one final step we need to perform and this is setting the SPN's (Service Principal Names) for Service Manager as we already did for our primary management server. Refer to [Configure the Kerberos for SCSM 2012 (SPN and delegation)](http://blog.scsmsolutions.com/2012/11/configure-the-kerberos-for-scsm-2012-spn-and-delegation/) published by [Anton Gritsenko](https://twitter.com/scsmru) for more detailed information about Service Manager and SPN's. The post was originally written for Service Manager 2012 but its content is still valid for Service Manager 2016.

Assuming that you configured an Active Directory account as Service Account, create necessary SPN's like the following for each Service Manager management server:

```powershell
setspn.exe -S MSOMSdkSvc/LabVM04 Lab\svc-scsm
setspn.exe -S MSOMSdkSvc/LabVM04.lab.jhnr.ch Lab\svc-scsm
setspn.exe -L Lab\svc-scsm
Registered ServicePrincipalNames for CN=SCSM Service Account,CN=Users,DC=lab,DC=jhnr,DC=ch:
        MSOMSdkSvc/LabVM04.lab.jhnr.ch
        MSOMSdkSvc/LabVM04
        MSOMSdkSvc/LabVM03.lab.jhnr.ch
        MSOMSdkSvc/LabVM03
```

You should now have SPNs configured for all of your Service Manger management servers (two in this scenario).

# Install Self Service Portal Prerequisites

To be able to install the Service Manager Self Service Portal you will first need to make sure that some prerequisites are present. Basically you need to install IIS Web Server as well as some additional IIS features. Run the below PowerShell commands to make sue that the required roles and features are present.

```powershell    
[string[]]$Features = @(
	"Web-Server",				# Web Server (IIS) 
	"Web-ASP",				# ASP
	"Web-Asp-Net45",			# ASP.NET 4.6
	"Web-Basic-Auth",			# Basic Authentication
	"Web-Windows-Auth",			# Windows Authentication
	"Web-Net-Ext45",			# .NET Extensibility 4.6
	"Web-Mgmt-Tools",			# Management Tools
	"Web-Mgmt-Console",			# IIS Management Console
	"NET-WCF-HTTP-Activation45"		# HTTP Activation
)

$Result = Add-WindowsFeature $Features
$Result.ExitCode
Success
```

# Install Service Manager Self Service Portal

As you may know, there exist various 3rd party vendors out there which make use of the Service Manager API to provide a very own portal experience for Service Manager.

This really gives you the flexibility to choose from a variety of portal solutions besides the out of the box portal. Other Self-Service portal solutions are offered by [itnetX](http://itnetx.ch/products/itnetx-itsm-portal/) as well as [Cased Dimensions](http://www.caseddimensions.com/scsm_self_service_portal/) and [Cireson](http://cireson.com/apps/self-service-portal/) which offers a free community edition of their portal. However you will have to decide for your self what solution suits your needs best. For this post I will concentrate on the Service Manager out of the box portal which was introduced with Service Manger 2012 R2 UR8.

After choosing the Self Service Portal installation option in the Service Manager installer overview, you should be able to provide the directory for the portal installation.

[![Portal Website Path](/images/SCSM2016_PortalPath.png)](/images/SCSM2016_PortalPath.png)

Now if you installed all the necessary prerequisites, you should be good to go after a successful prerequisite check.

[![Portal Prerequisites](/images/SCSM2016_PortalPrerequisites.png)](/images/SCSM2016_PortalPrerequisites.png)

Next up you will need to specify the IIS website name as well as the Service Manager server name which will be in this case the FQDN of your local server. It is recommended that you use a dedicated secondary Service Manager management server on which you install the Self Service Portal.

If you want to install new Self Service Portal on port 80, you must first move the default website in IIS to a different port, otherwise the installer will not let you specify port 80 for the Self Service Portal. However you can of course install the portal on a different port and change bindings after installation is complete.

[![Portal IIS Settings](/images/SCSM2016_PortalIis.png)](/images/SCSM2016_PortalIis.png)

Now enter the credentials of either the Service Manager Service Account or a specific Self Service Portal Service Account. The specified account. _Note that the specified account will be added to the Service Manager Administrators user role._

[![Portal Service Account](/images/SCSM2016_PortalAccount.png)](/images/SCSM2016_PortalAccount.png)

That's it for now. After the installation finishes you may start the Service Manager portal which can be found under the specified port and get your first impressions of System Center Service Manager 2016 Self Service portal.

[![Portal Overview](/images/SCSM2016_PortalOverview-1024x577.png)](/images/SCSM2016_PortalOverview.png)

It may happen that when you access the portal website by using the portal server FQDN, you are presented with an authentication dialog. Just make sure that the portal URL is added to your browsers trusted sites.

[![Portal Authentication Dialog](/images/SCSM2016_PortalAuth-1024x577.png)](/images/SCSM2016_PortalAuth.png)

A common thing to consider after installation is to create a DNS CNAME to give the portal a nice and memorable URL. That's actually achieved pretty easily by creating a CNAME in your DNS which points to the portal server.

[![Portal DNS](/images/SCSM2016_PortalDns.png)](/images/SCSM2016_PortalDns.png)

If you did install the portal on a different port than 80 you may want to change that so users do not have to enter port number when accessing the portal website. By specifying a Host Name for the portal website you are able to run other websites on this web server if necessary.

[![Portal Binding](/images/SCSM2016_PortalBinding.png)](/images/SCSM2016_PortalBinding.png)

If you are thinking about changing the look of your Self Service Portal make sure that you check out the following blogs which give a very good overview on how to customize various parts of the portal:

* [Basic UI customization in the new Self Service Portal (HTML5)](https://blogs.technet.microsoft.com/servicemanager/2016/03/15/basic-ui-customization-in-new-self-service-portal-html5/)
* [Customizing the Service Manager HTML5 Portal](http://www.systemcenter.ninja/2015/12/customising-service-manager-html5-portal.html)

Stay tuned for Part 4 of the blog post series "Installing Service Manager 2016" where I will describe how to install the Service Manager Data Warehouse.
