---
author: sjohner
comments: true
date: 2017-08-24 11:02:59+00:00
layout: post
slug: service-manager-install-script
title: Service Manager Installation by using a PowerShell Script
categories:
- Service Manager
tags:
- Connect
- Database
- Management Group
- PowerShell
- Service Manager
- Service Manager 2016
- SQL
- System Center
- Technet Gallery
---

This is the fifth part of a blog post series called "Installing Service Manager 2016" and will cover how to install a secondary Service Manger 2016 management server and necessary prerequisites by using a PowerShell script.



 	
  * [Installing Service Manager 2016 Part 1: What's new](https://blog.jhnr.ch/2016/10/12/whats-new-in-system-center-service-manager-2016/)

 	
  * [Installing Service Manager 2016 Part 2: Primary Management Server](https://blog.jhnr.ch/2016/10/14/installing-service-manager-2016-with-sql-2016-on-windows-server-2016/)

 	
  * [Installing Service Manager 2016 Part 3: Self Service Web Portal](https://blog.jhnr.ch/2016/10/25/installing-service-manger-2016-self-service-portal/)

 	
  * [Installing Service Manager 2016 Part 4: Data Warehouse Management Server](https://blog.jhnr.ch/2017/01/19/installing-service-manager-2016-data-warehouse-management-server/)

 	
  * Installing Service Manager 2016 Part 5: Scripting the installation (this post)


When it comes to Service Manager installations, it sometimes feels like one has to solve a big puzzle until finally having a working installation. Besides that the installations is somewhat time consuming, there are also multiple prerequisites which have to be installed. Also see my [previous post](https://blog.jhnr.ch/2016/10/14/installing-service-manager-2016-with-sql-2016-on-windows-server-2016/) about installing Service Manager 2016 for more information about necessary prerequisites.

To facilitate this process I created a PowerShell script which automatically downloads and installs all the necessary prerequisites as well as Service Manager itself. The benefit of having an install script is of course that you don’t have to click through all these manual steps with the installer but also that every installation which is performed with this script looks the same. As a nice side-effect, the installation is already documented by the script.

The script is not perfect and it will not do all the work for you. You still have to install SQL server and create service user accounts as well as administrator security group. But since most of the time the database and Active Directory users are provided by a separate team, this should not bother you too much.

Basically the script performs the following steps:



 	
  1. Check if it was executed with administrative privileges

 	
  2. Check if specified service and workflow accounts are available

 	
  3. Check if the specified service account has local admin permissions

 	
  4. Check if the SQL connection is available

 	
  5. Install .Net framework (sxs folder has to be present)

 	
  6. Install prerequisites (download them if not already present)

 	
  7. Install Service Manager 2016 (either as primary or additional management server)

 	
  8. Add DAL registry settings

 	
  9. Restart computer


Make sure the script is started with a user account that has administrative privileges on the Service Manager server as well as appropriate permissions on the SQL database which is used for installation.

The following files have to be present on the machine you want to install Service Manager 2016:

 	
  * Service Manager source files

 	
  * Windows Server 2016 sxs folder for installation of .NET framework 3.5


If you do not have internet connection on that machine, you can copy the prerequisites executables to the corresponding machine. The script will check if prerequisites are already available and won't try to download them in this case.

I recommend using the following directory structure but you are free to pass other values for _sxs_, _source_ and _prerequisites_ folders.

    
    PS C:\Install> ls
    
        Directory: C:\Install
    
    Mode                LastWriteTime         Length Name
    ----                -------------         ------ ----
    d-----        8/23/2017   2:51 PM                prereq
    d-----        8/23/2017   2:17 PM                source
    d-----        8/23/2017   2:48 PM                sxs
    -a----        8/23/2017   2:29 PM          14295 Install-SCSM2016.ps1


When running the script you can specify if you are installing a primary or additional management server. Furthermore (at least for now) you need to set several variables at the beginning of the script. For example you will need to specify service account usernames and passwords as well as database server and license key. This could be improved by importing the necessary variables from a csv file for example or by passing the appropriate values to corresponding parameters.

I recommend running the script by using the _-Verbose_ switch which gives you some log output about what's going on in the background. You can also check the Service Manager installation log file which is located in temp directory (‘C:\Users\<User>\AppData\Local\Temp’)

The script has been tested with Service Manager 2016 installed on Windows Server 2016 with SQL Server 2016. It is available for download on my [Service Manager GitHub repository](https://github.com/sjohner/scsm-repository/) and [Technet Gallery](https://gallery.technet.microsoft.com/Service-Manager-Installatio-23f3c784). Please feel free to get it from there and contribute to the project. If you have any suggestions/issues concerning the script, please report them on the [corresponding GitHub issue page](https://github.com/sjohner/scsm-repository/issues) or even better implement changes by yourself and push back to the repository so that others can benefit from your ideas as well :-)




