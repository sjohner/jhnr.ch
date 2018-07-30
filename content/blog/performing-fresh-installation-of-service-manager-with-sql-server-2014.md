---
author: sjohner
comments: true
date: 2016-01-15 00:24:46+00:00
slug: performing-fresh-installation-of-service-manager-with-sql-server-2014
title: Performing fresh installation of  Service Manager with SQL Server 2014
categories:
- Service Manager
tags:
- PowerShell
- Service Manager
- Service Manager 2012 R2
- SQL
- SQL Server 2014
- System Center
---

Since the release of Service Manager 2012 R2 Update Rollup 6, the use of SQL Server 2014 is officially supported by Microsoft. This probably seems like a pretty good thing because for new installations you probably want to use the newest SQL Server version. And since SQL Server 2014 was released back in April 2015 more and more customers are using latest version for their System Center deployments.

Well, this all sounds pretty good in theory. However if you really want to use SQL Server 2014 together with Service Manager 2012 R2 you may run into some issues. There exists [an official blog post describing how to install a secondary management server after an upgrade to SQL 2014](http://blogs.technet.com/b/servicemanager/archive/2015/07/30/deploying-secondary-management-server-after-sql-2104-upgrade.aspx). However installation is quite a bit of a pain because the graphical installer does not yet support installation of a management server together with SQL 2014. As soon as you get to select the database server you will be prompted with a message saying that “SQL Server <Servername> is not running SQL Server 2008R2 or SQL Server2012. Please update a newest version and retry”.

![SQL Server Version Error](/images/SqlVersionError.png)

Well in order to get around this problem you have to use the command line to install the secondary management server. This is perfectly fine, but still installation only works if an in-place upgrade of SQL Server 2012 to SQL Server 2014 was done and fails for the fresh installation of SQL Server 2014.
Unfortunately the installer checks for the SQL Server 2012 Namespace in the SQL server machine. If the namespace _Root\Microsoft\SqlServer\ComputerManagement11_ is not present in SQL server machine then the command line installation fails.

In order to get past the above mentioned issue, you could install a dummy instance of SQL Server 2012 on the SQL server machine. A much simpler solution however would be to create the mentioned namespace by using PowerShell (thanks to Aaron Croasmun for mentioning this in the comment section of the [Deploying Secondary Management Server after SQL 2014 upgrade](https://blogs.technet.microsoft.com/servicemanager/2015/07/30/deploying-secondary-management-server-after-sql-2014-upgrade/) blog post)

Add the namespace to WMI of your SQL server by using the following commands:

```powershell
    $nameSpace = [wmiclass]”root\Microsoft\SQLServer:__Namespace”
    $newNameSpace=$nameSpace.CreateInstance()
    $newNameSpace.Name=”ComputerManagement11”
    $newNameSpace.Put()
```

With the above commands we can in fact trick the Service Manager installer to think that SQL Server 2012 is present on our database server.

What works for the installation of a secondary management server does also for a fresh installation of Service Manager 2012 R2. We just have to slightly adjust the command to start a fresh Service Manager installation instead of adding a secondary management server:

```powershell
    .\Setup.exe /Install:Server /AcceptEula /ProductKey:'' /CreateNewDatabase /SqlServerInstance:'' /ManagementGroupName:'SM_Lab' /AdminRoleGroup:'Lab\SCSM-Admins' /ServiceRunUnderAccount:'lab\svc-scsm\password' /WorkflowAccount:'lab\svc-scsm\password' /CustomerExperienceImprovementProgram:NO /EnableErrorReporting:NO /Silent
```

I have to mention, that this installation procedure may not be completely supported by Microsoft as in fact SQL Server 2014 is only supported with Update Rollup 6 installed. But there is no process to manually slipstream UR6 into existing Service Manager installation media and you would basically be forced to do a fresh installation of Service Manager 2012 R2 with SQL Server 2012 and upgrade SQL Server after applying UR6.

Using the above command line installation completes successfully even without UR6 installed. You can check installation status by looking into the installer log files located in _C:\Users\<Username>\AppData\Local\Temp\2.
_Of course you will have to install the latest Update Rollup to make sure SQL Server 2014 is properly supported. You can then safely remove the previously created _ComputerManagement11_ namespace from your SQL server:

```powershell
    Get-WMIObject –query “Select * from __namespace where name=’ComputerManagement11’” –namespace “root\Microsoft\SQLServer” | Remove-WMIObject
```

Voilà, your Service Manager environment should now be up and running with SQL Server 2014 without the hassle of performing a SQL upgrade. Just remember to use command line again if you want to install additional management servers. An example can be found on the [official Service Manager blog](http://blogs.technet.com/b/servicemanager/archive/2015/07/30/deploying-secondary-management-server-after-sql-2104-upgrade.aspx).
