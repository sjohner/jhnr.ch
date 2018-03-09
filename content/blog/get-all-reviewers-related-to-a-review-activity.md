---
author: sjohner
comments: true
date: 2016-09-14 21:51:11+00:00
layout: post
slug: get-all-reviewers-related-to-a-review-activity
title: Get all Reviewers related to a Review Activity
wordpress_id: 1663
categories:
- Service Manager
tags:
- PowerShell
- Service Manager
- Service Manager 2012 R2
- System Center
---

In the Service Manager world, review activities (RA) and corresponding reviewers are an important part of most deployments. A review activity is kind of an approval item which is used to control execution of a business process. Often when providing items for end users in the service catalog it is necessary to somehow review input of submitted requests. This is where you specify the actual users that must approve a request. When automating Service Manager workloads (for example by using Service Management Automation or Orchestrator), it may sometimes be necessary to get those user objects by using a PowerShell script.

The following script is also published on my [GitHub Service Manager script repository](https://github.com/sjohner/SCSM-ScriptRepository). Feel free to extend and modify it to fit your needs. You may want to combine it with one of my previously published scripts which will [get you all review activities related to a given Service Request](https://blog.jhnr.ch/2015/03/29/recursively-get-all-review-activities-related-to-a-workitem/). You can also easily adapt it to get details about the actual _System.Reviewer_ object. Like if a specific reviewer has veto configured. If you find any bugs and typos, please let me know :-)

    
    <#
     .Notes
     NAME: Get-SCSMReviewer.ps1
     AUTHOR: Stefan Johner
     Website: http://blog.jhnr.ch
     Twitter: http://twitter.com/JohnerStefan
     Version: 1.0
     CREATED: 07/09/2016
     LASTEDIT:
     07/09/2016 1.0
     Initial Release
     
     .Synopsis
     This script gets all Reviewers (Users) for a given review activity.
     
     .Description
     When run on a Service Manager Management Server, the script will get all Reviewers (Users) which are related to a given review activity.
     The script has to be run on a Service Manager Management server and SMLets cmdlets have to be available.
     
     .Parameter ReviewActivityId
     Specify the Id of the Review Activity you want to find Reviewers for
     
     .Outputs
     Reviewers (Users) which are related to the given Review Activity
     
     .Example
     ./Get-SCSMReviewers -ReviewActicityId "RA12345"
     
     .Link
     http://github.com/sjohner/SCSM-ScriptRepository
     
    #>
    
    [cmdletbinding()]
    Param(
    	[Parameter(Mandatory=$True)][string]$ReviewActivityId
    )
    
    # Run the script and check for errors
    try
    {	
    	# Create datatable to return values
    	$ReviewerObjArray = @()
    	
    	# Load SMLets module
    	If ( ! (Get-module SMLets ))
    	{
    		Import-Module SMLets
    	}
    	
    	# Get the service request class
    	$ActivityClass = Get-SCSMClass -Name ^System.WorkItem.Activity$
    	if($ActivityClass -eq $null)
    	{
    		Throw "Can't get activity class"
    	}
    	
    	# Get activity object
    	if ([string]::IsNullOrEmpty($ReviewActivityId))
    	{
    		Throw "Parameter ReviewActivityId is null or empty."
    	}
    	else
    	{
    		$ActivityObj = Get-SCSMObject -Class $ActivityClass -Filter "Name -eq $ReviewActivityId"
    		if($ActivityObj -eq $null)
    		{
    			Throw "Can't get activity object"
    		}
    	}
    	
    	# Get the relationship class System.ReviewActivityHasReviewer$
    	$RelationReviewActivityHasReviewer = Get-SCSMRelationshipClass ^System.ReviewActivityHasReviewer$
    	if($RelationReviewActivityHasReviewer -eq $null)
    	{
    		Throw "Can't get System.RelationReviewActivityHasReviewer relationship"
    	}
    	
    	# Get the relationship class System.ReviewerIsUser$
    	$RelationReviewerIsUser = Get-SCSMRelationshipClass ^System.ReviewerIsUser$
    	if($RelationReviewerIsUser -eq $null)
    	{
    		Throw "Can't get System.System.ReviewerIsUser relationship"
    	}
    	
    	#Get related Reviewer objects
    	$Reviewers = @()
    	$Reviewers = Get-SCSMRelatedObject -SMObject $ActivityObj -Relationship $RelationReviewActivityHasReviewer
    	
    	# Get related object for System.ReviewerIsUser rel for each of the given Reviewer objects
    	foreach ($Reviewer in $Reviewers)
    	{
    		$ReviewerUser = Get-SCSMRelatedObject -SMObject $Reviewer -Relationship $RelationReviewerIsUser
    		$ReviewerObjArray += $ReviewerUser
    	}
    	
    	# Remove SMLets module
    	Remove-Module SMLets -Force
    	
    	# Output Reviewer
    	$ReviewerObjArray
    }
    catch
    {
    	#Throw error
        Throw "@   
    	$error[0]
        @"  
    }
