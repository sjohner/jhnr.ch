---
author: sjohner
comments: true
date: 2015-02-24 20:00:00+00:00
slug: get-installed-scsm-version-for-multiple-computers-by-using-powershell
title: Get installed SCSM Version for multiple computers by using PowerShell
categories:
- Service Manager
tags:
- CSV
- PowerShell
- Remote
- Service Manager
- Version
---

I recently worked in a project where people are working with several Service Manager environments. First there is like a development environment where everyone does first steps and sees how it goes. If everything works as expected Management Packs and configuration are imported into the integration environment. Last but no least, there is the productive environment where end users are using Service Manager to choose offerings from a pretty big service catalog :-)

Well, when working with several environments, one has to make sure that all Service Manager servers are on the same patch level. Especially integration and production should be on the same level.

[![Screenshot Script Check File Version](/images/checkfileversion.png?w=660)](/images/checkfileversion.png)

To check for Service Manager versions on multiple servers I wrote a small PowerShell script which checks file version of _Microsoft.EnterpriseManagement.DataAccessLayer.dll_ on every computer passed in a CSV file. To check files, a remote PowerShell session is established so the computers specified must be configured to receive Windows PowerShell remote commands that are sent by using the WS-Management technology

The CSV file should have the following structure (where computername is the header to be included):

**_Computername_**
_SM01_
_SCSMSERVER01_
_MS01_

It is kind of a first draft and may be improved in some ways. Feel free to comment if you have any suggestions on how to enhance the script!

```powershell
    <#
     
    .SYNOPSIS
    This script checks what version of a file exists on multiple computers.
     
    .DESCRIPTION
    The script establishes a remote session to every computer specified in the CSV file and checks version of a given file. By default it checks
    for C:Program FilesMicrosoft System Center 2012 R2Service ManagerMicrosoft.EnterpriseManagement.DataAccessLayer.dll
     
    .EXAMPLE
    .Check-ScsmVersion.ps1 -PathToCsv .scsmserver.csv -Verbose
    
    .NOTES
    The user which runs the script needs to be able to establish a powershell remote session to the specified computers. The CSV file needs to have the following structure
    
    Computername
    SM01
    SCSMSERVER01
    MS01
     
    .LINK
    http://scsmlab.com
     
    #>
    
    [cmdletbinding()]
    Param(
        [Parameter(Mandatory=$True)][string]$PathToCsv,
        [Parameter(Mandatory=$False)][string]$PathToFile = "C:Program FilesMicrosoft System Center 2012 R2Service ManagerMicrosoft.EnterpriseManagement.DataAccessLayer.dll"
    )
    
    #Get servers from CSV
    if(Test-Path $PathToCsv) {
        $Servers = Import-Csv $PathToCsv
    }
    else {
        Write-Error -Category ObjectNotFound -Message "Could not find specified CSV file"
    }
    
    Foreach ($Server in $Servers) {
        #Create remote session and check file version
        $FileVersion = $null
        $FileVersion = Invoke-Command $Server.Computername -ArgumentList $PathToFile {
            Param ($PathToFile)
            if (Test-Path $PathToFile) {
                return (Get-Item $PathToFile | Select -ExpandProperty VersionInfo).FileVersion
            } 
        }
        
        if ($FileVersion -ne $null) {
            Write-Verbose "File version on computer $($Server.Computername) is $FileVersion"
        }
        else {
            Write-Warning "Specified file $PathToFile on computer $($Server.Computername) not found"
        }
    }
```