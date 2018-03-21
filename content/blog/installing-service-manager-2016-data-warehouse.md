---
author: sjohner
comments: true
date: 2017-01-19 22:47:47+00:00
layout: post
slug: installing-service-manager-2016-data-warehouse
title: Installing Service Manager 2016 Data Warehouse Management Server
wordpress_id: 1862
categories:
- Service Manager
tags:
- Analysis Services
- Data Warehouse
- Debugging
- Error
- Installation
- Kerberos
- Management Group
- Management Pack
- Management Server
- PowerShell
- Report Server
- Reporting
- Reporting Services
- Service Manager
- Service Manager 2016
- SQL
- SQL Server 2016
- System Center
- Windows Server 2016
---

This is the fourth part of a blog post series called “Installing Service Manager 2016 Data Warehouse” and will cover how to install a Service Manger 2016 Data Warehouse server on Windows Server 2016 with Desktop Experience.



 	
  * [Installing Service Manager 2016 Part 1: What’s new](https://blog.jhnr.ch/2016/10/12/whats-new-in-system-center-service-manager-2016/)

 	
  * [Installing Service Manager 2016 Part 2: Primary Management Server](https://blog.jhnr.ch/2016/10/14/installing-service-manager-2016-with-sql-2016-on-windows-server-2016/)

 	
  * [Installing Service Manager 2016 Part 3: Self Service Web Portal](https://blog.jhnr.ch/2016/10/25/installing-service-manger-2016-self-service-portal/)

 	
  * Installing Service Manager 2016 Part 4: Data Warehouse Management Server (this post)

 	
  * [Installing Service Manager 2016 Part 5: Scripting the installation](https://blog.jhnr.ch/2017/08/24/service-manager-install-script/)


In this blog post series I will describe the installation of Service Manager 2016 based on a pretty simple 3 computer scenario. The first computer hosts the Service Manager management server as well as the Service Manager database. The second computer hosts the Self Service Web Portal and the third one the data warehouse management server and the Data Warehouse databases.

[![Install Scenario](/images/BlogScenario3-1024x315.png)](/images/BlogScenario3.png)

The Service Manager data warehouse is an optimized component for reporting purposes. It can be extended via management packs. The data warehouse and the reporting infrastructure can be used in other System Center products like Configuration Manager as well as Operations Manager.

For more detailed information about the Service Manager data warehouse, check out [this Technet article](https://social.technet.microsoft.com/wiki/contents/articles/15608.system-center-service-manager-data-warehouse-and-reporting.aspx) written by Travis Wright back in 2009. As stated in this article,  the data warehouse in Service Manager provides three primary functions:



 	
  1. Offload data from the main Service Manager database to improve performance of the Service Manager database

 	
  2. Long-term data storage

 	
  3. Provide data for reports


The data warehouse can sometimes be a bit fragile. There are [numerous people out in Technet forums](https://social.technet.microsoft.com/Forums/systemcenter/en-US/home?forum=dwreportingdashboards) complaining that their data warehouse is broken and sometimes the only solution is to reinstall the DWH. People also tend to think that the data warehouse does contain all the data from Service Manager but this is not the case unfortunately. For example, attachments and the work item history are not synced to the DWH by default.

To avoid problems with your data warehouse, make sure that you wait at least 1 hour before importing a new management pack if you deleted a previous version of the same MP. The data warehouse MPSync job runs every hour and if there exists an MP in the data warehouse which is not (anymore) compatible with the one in Service Manager you will get into troubles. Check [this Technet article](https://technet.microsoft.com/en-us/library/ff461007.aspx) for more details about data warehouse jobs.

For a lab environment, I do not recommend installing the data warehouse component. Simply because it is very likely to break after some time testing, importing and deleting management packs

Enough with data warehouse basics, lets start with the installation. Service Manager data warehouse setup depends on some prerequisites. So before starting with Service Manger 2016 data warehouse installation, we need to make sure that these are installed. The following prerequisites are needed to successfully install Service Manager 2016 data warehouse management server:



 	
  * .NET Framework 3.5

 	
  * SQL Server 2014 Analysis Management Objects

 	
  * SQL Server 2012 Native Client


Besides installing the above mentioned prerequisites, you will need five service accounts for this setup, where the Service Manager Service Account has to be member of the local administrators group of the server where you are installing the data warehouse management server:

 	
  * SQL Server Service Account

 	
  * Service Manager Service Account

 	
  * Reporting Services Account

 	
  * Analysis Services Account


**Important:** *Make sure that you run Service Manager setup with a domain user which has appropriate permissions on the corresponding SQL server instance (sysadmin) as well as local administrator permissions for the server where you are installing Service Manager.*


##### Install SQL Server 2016


As with the [installation of the primary management server](https://blog.jhnr.ch/2016/10/14/installing-service-manager-2016-with-sql-2016-on-windows-server-2016/), we will get started by installing SQL Server 2016. Basically Service Manager is doing pretty good with an ordinary SQL instance. However, Service Manager is heavily dependent on SQL Server performance. I am no SQL expert and will not go into any details about SQL Server tuning. I recommend reviewing your production SQL installation for best practices and performance bottlenecks with a SQL expert to get the best out of your Service Manager environment.

Make sure you check out the general SQL recommendations for System Center components found [here. This was initially written for System Center 2012 but still applies for 2016. ](https://technet.microsoft.com/en-us/library/dn281933(v=sc.12).aspx)

There are nevertheless some things which have to be mentioned in terms of SQL requirements. First of all install SQL Server 2016 with the following features enabled:



 	
  * Full-Text and Semantic Extractions for Search

 	
  * Analysis Services

 	
  * Reporting Services


![SQL_DWH_Features](/images/SQL_DWH_Features.png)

Service Manager is somewhat touchy when it comes to SQL collations. If you haven’t done yet, I recommend reading [Clarification on SQL Server Collation Requirements for System Center 2012](https://blogs.technet.microsoft.com/servicemanager/2012/05/24/clarification-on-sql-server-collation-requirements-for-system-center-2012/) and [this article on serverfault.com](http://serverfault.com/questions/498421/sql-server-collation-settings-for-system-center-service-manager-2012) regarding collation settings.

Most people probably will be fine with _Latin1_General_100_CI_AS _which is recommended for Latin based languages like English, German, Italian, Portuguese and Dutch. However be sure to check [Language Support for System Center 2012 – Service Manager](https://technet.microsoft.com/en-us/library/hh495583%28v=sc.12%29.aspx) for the appropriate collation used with your language.

**Important:** *Support for multiple languages in Service Manager is not possible when you are using the default collation (SQL_Latin1_General_CP1_CI_AS). If later you decide to support multiple languages using a different collation, you have to reinstall SQL Server.
Also make sure that you use the same collation for your data warehouse database as you did for the Service Manager database:*


> The collation used must be the same for the computers hosting the Service Manager database, data warehouse database, analysis services database, and Reporting Services database.


[![DWH SQL Collation](/images/SQL_DWH_Collation.png)](/images/SQL_DWH_Collation.png)

After dealing with collations, make sure that you specify a group of SQL server administrators which will have unrestricted access to the database engine.

[![SQL DB Engine Configuration](/images/SQL_DWH_DBEngine.png)](/images/SQL_DWH_DBEngine.png)

Since we are installing Reporting and Analysis Services, some additional configuration is needed compared to the Service Manager management server installation. First of all make sure that you add an appropriate group of Analysis Services administrators which will have unrestricted access to Analysis Services.

[![Analysis Services Configuration](/images/SQL_DWH_AnalysisServices.png)](/images/SQL_DWH_AnalysisServices.png)

When it comes to the configuration of the reporting services, you are good to go with the _Install and configure_ option. This will give you a fully operational report server after SQL installation is finished.

[![Install and configure Reporting Services](/images/SQL_DWH_ReportingServices.png)](/images/SQL_DWH_ReportingServices.png)


### Install Prerequisites


When installing the Service Manager data warehouse server, you will need exactly the same prerequisites as with the primary management server. Although Service Manager 2016 now supports .NET Framework 4.5.1, you will need to install version 3.5. Setup wizard will tell you if .NET Framework 3.5 is missing and you will not be able to continue the setup process.

[![Install .NET Framework 3.5](https://i2.wp.com/blog.jhnr.ch/wp-content/uploads/2016/10/Install_DotNetFramework35.png?resize=700%2C499&ssl=1)](https://i2.wp.com/blog.jhnr.ch/wp-content/uploads/2016/10/Install_DotNetFramework35.png?ssl=1)

Besides .NET Framework, you will need to install **Microsoft SQL Server 2014 Analysis Management Objects (AMO) which you can get from [here](http://www.microsoft.com/en-us/download/details.aspx?id=42295)** (watch out for _SQL_AS_AMO.msi_). You may need to install SQL Server 2012 Native Client as well if your SQL Server is installed on another computer (which is not the case in this scenario). Don’t worry because the setup wizard will tell you to do so when installing Service Manager on a separate server. If you need SQL Server 2012 Native Client you can get it from [here](https://www.microsoft.com/en-us/download/details.aspx?id=29065) (watch out for _sqlncli.msi_)


### Manual Steps to Configure the Remote SQL Server Reporting Services


If you specify a remote server as SQL Report Server you need to perform some manual steps to configure the remote SSRS server.

**Important:** *This is **not** necessary if you are installing Service Manager data warehouse server on the same machine as SQL Report Server is installed (as outlined in this scenario). In this case Service Manager setup will add necessary dll file as well as code segment and extension tag.*

Assuming you are using SQL Server 2016, you will need to copy _Microsoft.EnterpriseManagement.Reporting.Code.dll_ (which is located in the Prerequisites folder on your Service Manager installation media) to the folder _C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin_ on the computer that is hosting SSRS.

[![Remote Report Server Copy DLL](/images/SCSM_DWH_CopyDLLs-1024x576.png)](/images/SCSM_DWH_CopyDLLs.png)

Furthermore you will need to manually add the following code segment to _Rssrvpolicy.config_ in the folder _C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer_

    
    <CodeGroup   
       class="UnionCodeGroup"   
       version="1"   
       PermissionSetName="FullTrust"   
       Name="Microsoft System Center Service Manager Reporting Code Assembly"   
       Description="Grants the SCSM Reporting Code assembly full trust permission.">   
       <IMembershipCondition   
          class="StrongNameMembershipCondition"     
          version="1"  
          PublicKeyBlob="0024000004800000940000000602000000240000525341310004000001000100B5FC90E7027F67871E773A8FDE8938C81DD402BA65B9201D60593E96C492651E889CC13F1415EBB53FAC1131AE0BD333C5EE6021672D9718EA31A8AEBD0DA0072F25D87DBA6FC90FFD598ED4DA35E44C398C454307E8E33B8426143DAEC9F596836F97C8F74750E5975C64E2189F45DEF46B2A2B1247ADC3652BF5C308055DA9"   
    />  
    </CodeGroup>


[![rssrvolicy.config Code Segment](/images/SCSM_DWH_rssrvolicy-1024x642.png)](/images/SCSM_DWH_rssrvolicy.png)


Besides that you need to add the following Extension tag to the Data segment in the _rsreportserver.conf_ file located in _C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer_




    
    <Extension Name="SCDWMultiMartDataProcessor" Type="Microsoft.EnterpriseManagement.Reporting.MultiMartConnection, Microsoft.EnterpriseManagement.Reporting.Code" />


[![reportserver.config Extension tag](/images/SCSM_DWH_reportserver-1024x641.png)](/images/SCSM_DWH_reportserver.png)

For further details check out the [Technet article](https://technet.microsoft.com/en-us/library/ff461215.aspx) outlining the steps above for Service Manager 2012 and SQL 2008. It is still valid for Service Manager 2016.


### Install Service Manager Data Warehouse


Now let’s get started with Service Manager Data Warehouse setup! If you installed all the necessary prerequisites, you should see a similar screen after entering product key and continuing in the setup wizard.

[![Data Warehouse Prerequisites](/images/SCSM_DWH_Prerequisites.png)](/images/SCSM_DWH_Prerequisites.png)

Next specify SQL Server and Instance name where Service Manager should create its data warehouse databases. Make sure that corresponding ports are open when using a separate SQL Server with firewall enabled. This should not be a problem in this scenario since SQL server is installed on the same computer. Note that since Service Manager 2016, setup also supports SQL AlwaysOn installations.

*Note that if you are using the default collation (SQL_Latin1_General_CP1_CI_AS), a warning message appears which you should read carefully. As stated in the beginning of this post, support for multiple languages in Service Manager is not possible when using the default collation.*

[![SCSM Data Warehouse Databases](/images/SCSM_DWH_Databases1.png)](/images/SCSM_DWH_Databases1.png)

We will not create Operations Manager and Configuration Manager data warehouse databases in this scenario.

[![SCSM Data Warehouse Databases](/images/SCSM_DWH_Databases2.png)](/images/SCSM_DWH_Databases2.png)

Next up we will define the name of the new Service Manager management group. Management group names must be unique.

**Important:** *Do not use the same management group name even when you are deploying a Service Manager management server and a Service Manager data warehouse management server. Furthermore, do not use the management group name that is used for Operations Manager.*

Besides the management group name, you also want to specify a management group administrators group.

[![Data Warehouse Management Group](/images/SCSM_DWH_ManagementGroup.png)](/images/SCSM_DWH_ManagementGroup.png)

Now specify the SQL Reporting Services server used for your Service Manager reports. If you are working with a remote server you will need to confirm that you have take the manual steps to configure SSRS server as outlined above before being able to continue with the installation.

[![Configure Reporting Services](/images/SCSM_DWH_ReportingServices.png)](/images/SCSM_DWH_ReportingServices.png)

Now enter the credentials of the Service Manager Service Account. It will be assigned to the logon account for the System Center Data Access Service as well as for System Center Management Configuration service. *Note that the Service Account has to be member of the computers local administrators group.*

[![Service Manager Credentials](/images/SCSM_DWH_Credentials.png)](/images/SCSM_DWH_Credentials.png)

Besides the Service Manager account you will need a reporting account which is used to read data warehouse reporting data sources and to generate reports.

**Important:** *This account will be used to publish OOB reports when registering the data warehouse in Service Manager. The account needs to be member of "Content manager" role for the newly created "ServiceManager" folder on your specified Report Server.*

[![Report Server Credentials](/images/SCSM_DWH_Credentials2.png)](/images/SCSM_DWH_Credentials2.png)

And last but not least you will need to specify Analysis Services server used for OLAP cubes.

[![Configure Analysis Services](/images/SCSM_DWH_OLAPCubes.png)](/images/SCSM_DWH_OLAPCubes.png)

Of course you will need a analysis services account which is used to connect to the data warehouse datamart databases. Setup will create a corresponding login for the affected databases.

[![Analysis Services Credentials](/images/SCSM_DWH_Credentials3.png)](/images/SCSM_DWH_Credentials3.png)

That’s pretty much it for the installation of a data warehouse management server. You do not necessarily need to backup your encryption key since we already did that when installing our primary management server.

To be able to view reports from Service Manager console, the newly created Report Server has to be accessible over port 80. Check if an appropriate firewall rule exists on your Report Server otherwise you will see a message telling you that _the data warehouse SQL Reporting Services server is currently unavailable_ when starting your Service Manager console

[![Report Server unavailable error](/images/SCSM_DWH_ReportServerUnavailable.png)](/images/SCSM_DWH_ReportServerUnavailable.png)

An appropriate rule which allows inbound traffic on port 80 can be created as follows:

    
    New-NetFirewallRule -Name Allow_HTTP -DisplayName "Allow HTTP" -Description "Allow access to Report Server for SCSM" -Protocol TCP -LocalPort 80 -Direction Inbound -Enabled True -Profile Any -Action Allow


There is one final step we need to perform and this is setting the SPN’s (Service Principal Names) for Service Manager as we already did for our primary and secondary management servers. Refer to [Configure the Kerberos for SCSM 2012 (SPN and delegation)](http://blog.scsmsolutions.com/2012/11/configure-the-kerberos-for-scsm-2012-spn-and-delegation/) published by [Anton Gritsenko](https://twitter.com/scsmru) for more detailed information about Service Manager and SPN’s. The post was originally written for Service Manager 2012 but its content is still valid for Service Manager 2016.

Assuming that you configured an Active Directory account as Service Account, create necessary SPN’s like the following for each Service Manager management server:

    
    setspn.exe -S MSOMSdkSvc/LabVM05 Lab\svc-scsm
    setspn.exe -S MSOMSdkSvc/LabVM05.lab.jhnr.ch Lab\svc-scsm
    setspn.exe -L Lab\svc-scsm
    Registered ServicePrincipalNames for CN=SCSM Service Account,CN=Users,DC=lab,DC=jhnr,DC=ch:
            MSOMSdkSvc/LabVM05.lab.jhnr.ch
            MSOMSdkSvc/LabVM05
            MSOMSdkSvc/LabVM04.lab.jhnr.ch
            MSOMSdkSvc/LabVM04
            MSOMSdkSvc/LabVM03.lab.jhnr.ch
            MSOMSdkSvc/LabVM03


You should now have SPN’s configured for all of your Service Manger management servers (two in this scenario) as well as for your data warehouse management server.

When your installation fails and you see the below error message, check the setup log (which can be found in ‘C:\Users\<User>\AppData\Local\Temp’) for the following error:


    AssignSsrsRole Error: System.Web.Services.Protocols.SoapException : System.Web.Services.Protocols.SoapException: The user or group name ‘BUILTIN\BUILTIN’ is not recognized. —> Microsoft.ReportingServices.Diagnostics.Utilities.UnknownUserNameException: The user or group name ‘BUILTIN\BUILTIN’ is not recognized


[![DWH Installation Error](/images/SCSM_DWH_ReportingError.png)](/images/SCSM_DWH_ReportingError.png)

If this is the case, check out [my blog post ](https://blog.jhnr.ch/2016/11/08/error-when-installing-service-manager-data-warehouse-management-server-the-user-or-group-name-builtinbuiltin-is-not-recognized/)on how this might be resolved :-)


### Register Data Warehouse


Now that you have successfully installed your data warehouse management server, it is time to register the data warehouse to your existing Service Manager environment. This can be easily done by using the Service Manager console.

[![Register Data Warehouse Server](/images/SCSM_Register_DWH.png)](/images/SCSM_Register_DWH.png)

Specify your newly created data warehouse management server and test the connection.

[![Register Data Warehouse Server](/images/SCSM_Register_DWH1.png)](/images/SCSM_Register_DWH1.png)

You will need to provide credentials for your Service Manager service account to complete the registration.

[![Register Data Warehouse Server](/images/SCSM_Register_DWH3.png)](/images/SCSM_Register_DWH3.png)

[![Register Data Warehouse Server](/images/SCSM_Register_DWH4.png)](/images/SCSM_Register_DWH4.png)

After registering the data warehouse, you should see a new workspace named _Data Warehouse_ in your Service Manager console. All existing management packs will be synced, reports will be deployed on your SQL Report Server and new SQL Analysis cubes will be created and processed. This takes quite some time. So register the data warehouse and call it a day :-)

When you start working the day after, you should see another workspace named _Reporting._ Also check if all management packs are synced successfully to the data warehouse. If this is not the case - especially if report management packs are failed - check the Opearations Manager event log on the data warehouse server for errors.

[![Service Manager Reporting](/images/SCSM_DWH_Reports-1024x742.png)](/images/SCSM_DWH_Reports.png)

Stay tuned for Part 5 of the blog post series “Installing Service Manager 2016” where I will describe how to get started with your newly created Service Manager environment and what to consider after Service Manager is up and running.
