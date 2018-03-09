---
author: sjohner
comments: true
date: 2015-07-20 21:26:30+00:00
layout: post
slug: dynamically-update-service-manager-enum-values-from-mysql-database-by-using-powershell
title: Dynamically update Service Manager enum values from MySQL database by using
  PowerShell
wordpress_id: 1063
categories:
- Service Manager
tags:
- Enumeration
- MySQL
- Performance
- PowerShell
- Service Manager
- Service Manager 2012 R2
- SQL
---










Lists in SCSM can sometimes be pretty dynamic. As an example it may be the case that you want to publish job titles for an employee joiner offering to the end users. Job titles may change over time and if your company is using kind of an external system to manage these titles together with other employee data, it would be nice to have your list updated dynamically right?




Besides many other commands, SMLets module provides you also with the following cmdlets to manage enumerations.






	
  * Get-SCSMEnumeration

	
  * Get-SCSMChildEnumeration

	
  * Remove-SCSMEnumeration

	
  * Add-SCSMEnumeration

	
  * Get-SCSMTopLevelEnumeration




With these cmdlets in mind it is pretty straight forward to add new and remove existing enumerations. The only challenge when adding new enums can be the definition of the appropriate ordinal. Easiest way of solving this is to always add new enums to the very bottom of an existing list. With this approach, one only has to get the largest ordinal of all child enums and increment this for the newly added one.


<!-- more -->


As always with Service Manager and PowerShell, there are many ways of skinning a cat :-) But some of them are faster :-)




For example when getting the largest ordinal number for all child enums of a particular enumeration list this can be achieved in (at least) two different ways:




One is getting all child enumerations, sort them by Ordinal value and select Ordinal of the last object in the sorted list.




    
    ($ChildEnums | Sort-Object Ordinal | select -Last 1).Ordinal
    




Another way would be to use the Measure-Object cmdlet on the Ordinal property and to measure for max value.




    
    ($ChildEnums | Measure-Object -Property Ordinal -Maximum).Maximum
    




However when comparing these two methods, getting the largest ordinal value using Measure-Object seems to be much faster than sorting an getting last objects ordinal value




    
    PS C:> (Measure-Command {($ChildEnums | Measure-Object -Property Ordinal -Maximum).Maximum}).TotalMilliseconds
    0,892
    PS C:> (Measure-Command {($ChildEnums | Sort-Object Ordinal | select -Last 1).Ordinal}).TotalMilliseconds
    4,0093
    




When working with a large number of child enums, this can slow down your script remarkably.




Below you can find some snippets which are used to update an existing enum list in Service Manager. I also uploaded the complete PowerShell script to [TechNet Gallery here](https://gallery.technet.microsoft.com/Service-Manager-Update-bc571449). Furthermore the script checks whether a enum does not anymore exist in MySQL database and removes it from Service Manager if applicable. **Be aware that this would have impact on CIs or WorkItems where the enum to be removed is selected**.




For the MySQL part,[ Thomas Maurer wrote a great post](http://www.thomasmaurer.ch/2011/04/powershell-run-mysql-querys-with-powershell/) about querying MySQL with PowerShell.  I recommend reading his post if you need more details about this step. When querying MS SQL databases one could use [Invoke-Sqlcmd cmdlet ](//msdn.microsoft.com/en-us/library/cc281720.aspx)instead.




    
    #Define MySQL query to get job titles
    $MySqlQuery = "select title from jobtitles where deleted = 0";
    
    #Define MySQL credentials and connection string
    $MySQLUserName = 'mysqluser'
    $MySQLPassword = 'p@ssword'
    $MySQLDatabase = 'databasename'
    $MySQLHost = 'mysql.jhnr.ch'
    $ConnectionString = "server=" + $MySQLHost + "; port=3306; uid=" + $MySQLUserName + "; pwd=" + $MySQLPassword + "; database=" +$MySQLDatabase
    
        Function Invoke-MySQL {
            Param
                (
                [Parameter(Mandatory = $true,ParameterSetName = '',ValueFromPipeline = $true)][string]$Query
                )
    
            #Load assembly. MySQL Connector/Net ADO.NET driver has to be installed http://dev.mysql.com/downloads/connector/net/
            [void][System.Reflection.Assembly]::LoadWithPartialName("MySql.Data")
    
            #Create new connection by using previousely defined connection string
            $Connection = New-Object MySql.Data.MySqlClient.MySqlConnection
            $Connection.ConnectionString = $ConnectionString
            $Connection.Open()
    
            #Excute query and close connection
            $Command = New-Object MySql.Data.MySqlClient.MySqlCommand($Query, $Connection)
            $DataAdapter = New-Object MySql.Data.MySqlClient.MySqlDataAdapter($Command)
            $DataSet = New-Object System.Data.DataSet
            $RecordCount = $dataAdapter.Fill($dataSet, "data")
            $Connection.Close()
    
            #Return query results
            Return $DataSet.Tables[0]
        }
    
        #Get Job Titles from Tippps MySQL database
        $Result = Invoke-MySQL -Query $MySqlQuery
    




Below you can find a snippet which get job titles from a MySQL database and creates an enumeration for each of the received titles if not already existing in Service Manager.




    
        #Import SMLets
        Import-Module smlets
    
        #Get Management Pack to save enumerations and appropriate parent enum
        $EnumManagementPack = Get-SCSMManagementPack -Name ^jhnr.listelements.library$
        $ParentEnum = Get-SCSMEnumeration -Name ^jhnrenumemploymentstatus$
    
        #Get child enumsn using Get-SCSMChildEnumeration which is much faster
        #PS C:> (Measure-Command {Get-SCSMChildEnumeration -Enumeration $ParentEnum}).TotalMilliseconds
        #1,3816
        #PS C:> (Measure-Command {(Get-SCSMEnumeration | where {$_.Parent -match $($ParentEnum.Id)})}).TotalMilliseconds
        #242,5261
        $ChildEnums = Get-SCSMChildEnumeration -Enumeration $ParentEnum
    
        #Add new titles to enum list if not already in enum list
        Foreach($Title in $Result.Title) {
            if($($ChildEnums.DisplayName) -notcontains $Title) {
    
                #Get highest ordinal value using Measure-Object. Seems to be much faster than sorting an getting last objects ordinal value
                $Ordinal = (($ChildEnums | Measure-Object -Property Ordinal -Maximum).Maximum) + 1
    
                #Create child enum internal name (convert to lower case and remove spaces)
                $ChildEnumName = "$($ParentEnum.Name)$Title".ToLower() -replace 's',''
    
                #Add new child enum as last element in list (increment hightest ordinal)
                Add-SCSMEnumeration -Parent $ParentEnum -Name $ChildEnumName -DisplayName $Title -Ordinal $Ordinal -ManagementPack $EnumManagementPack
            }
        }
    




The script checks whether a enum does not anymore exist in MySQL database and removes it from Service Manager if applicable.




    
        #Remove obsolete titles from enum list if not anymore in database
        Foreach($ChildEnum in $ChildEnums) {
            if($($Result.Title) -notcontains $($ChildEnum.DisplayName)) {
                Remove-SCSMEnumeration -Enumeration $ChildEnum
                Write-Verbose "Removed $($ChildEnum.DisplayName) from Enum List"
            }
        }
    









