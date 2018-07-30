---
author: sjohner
comments: true
date: 2014-07-15 16:23:21+00:00
slug: how-to-check-if-a-service-manager-notification-template-is-associated-to-a-subscription
title: How to check if a Service Manager Notification Template is associated to a
  Workflow or Notification Subscription
categories:
- Service Manager
tags:
- Notification Subscription
- PowerShell
- SMlets
- Template
---

**Update:** As Trang mentioned in the comments, the first version of the script only found templates which are assigned to a Notification Subscription. I updated the script to look for templates which are assigned either to a Workflow or Notification Subscription.

Sometimes I come across Service Manager environments which have a whole bunch of notification subscriptions and corresponding templates configured. Normally these environments are productive for some time and several changes have been made to processes and notifications.

Well, when changing and deleting workflow and notification subscriptions the corresponding template is mostly left behind. Maybe for good reason because people just don’t know which of their notification templates are referenced by other workflow or notification subscriptions. Since it is pretty awful to open the properties of every subscription in the Service Manager console to check whether it references a given template, I searched for a better way to find out whether a template is assigned to a workflow or notification subscription or not.

[![Find Notification Template Dependencies](/images/findnotificationtemplatedependencies.png?w=604)](/images/findnotificationtemplatedependencies.png)

Like in many situations, PowerShell together with SMlets is your friend ![Smile](/images/wlemoticon-smile.png) Since Notification Subscriptions are in fact workflows, we can check if a template is referenced in the _WriteAction_ of a workflow. So what I do is to get all the workflow objects from Service Manager and check if their _WriteAction _contains a specific pattern which includes the given template GUID.

You can get the GUID of a template by using the template’s DisplayName and the following query:

```powershell
    (Get-SCSMObjectTemplate -DisplayName "Escalation").Id
```

Et voilà, you can find the PowerShell script I created below. You will need SMLet installed to be able to execute correctly. It is kind of a first draft and may be improved in some ways. Feel free to comment if you have any suggestions on how to enhance the script! Hope this helps!
    
```powershell    
    <#
     .Notes
     NAME: Check-NotificationTemplateUsage.ps1
     AUTHOR: Stefan Johner
     Website: http://blog.jhnr.ch
     Twitter: http://twitter.com/JohnerStefan
     Version: 1.1
     CREATED: 15/07/2014
     LASTEDIT:
     14/08/2014 1.1
     Added support for Workflow Subscriptions
     15/07/2014 1.0
     Initial Release
    
     .Synopsis
     Checks for a given notification template if it is used in any notification subscription.
    
     .Description
     Retrieves all workflows and notification subscriptions that a given notification template is directly referenced to.
    
     .Parameter TemplateId
     Specify the GUID of the template you want to check usage for
    
     .Outputs
     [ref]
     DisplayName, Name and Id properties of the workflow or notification subscriptions which use the given template
    
     .Example
     .Check-NotificationTemplateUsage.ps1 -TemplateId "5120d091-efef-b47a-f942-6905250f577f"
    
    #>
    
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]$TemplateId
    )
    
    # Run the script and check for errors
    try {
    
        # Load SMLets
        if(!(get-module smlets)){import-module smlets -Force -ErrorAction Stop}
    
        $NotificationSubscriptionPattern = '<workflowarrayparameter type="string" name="TemplateIds"><item>' + $TemplateId + '</item></workflowarrayparameter>'
        $WorkflowSubscriptionPattern = '<workflowarrayparameter type="guid" name="NotificationTemplates"><item>' + $TemplateId + '</item></workflowarrayparameter>'
    
        # Get Notification Template
        $TemplateObj = Get-SCSMObjectTemplate -Id $TemplateId
    
        if($TemplateObj) {
    
            Write-Host " "
            Write-Host "Validating..."
            Write-Host " "
    
            $Workflows = @()
    
            # Get Workflows which use the defined template in WriteAction
            Get-SCSMWorkflow.ps1 | ForEach-Object {
                $wf = $_
                $wf.WriteActionCollection | ForEach-Object {
                    if ($_.Configuration.Contains($NotificationSubscriptionPattern) -or $_.Configuration.Contains($WorkflowSubscriptionPattern)) {
                        $Workflows += $wf
                    }
                }
            }
    
            # Output results
            If($Workflows.Count -gt 0) {
                Write-Host "Template used by $($Workflows.Count) workflow(s) or notification subscription(s):" -ForegroundColor Green
                Write-Host " "
                $Workflows | ForEach-Object {
                    $_ | Select-Object DisplayName,Name,Id
                }
            }
            Else {
                Write-Host "Template not used in any workflow or notification subscription!" -ForegroundColor Red
                $TemplateObj | Select-Object DisplayName,Name,Id
            }
        }
        Else {
            Write-Host " "
            Write-Host "Template with ID `"$($TemplateId)`" not found!" -ForegroundColor Red
            Write-Host " "
        }
    
    # Remove SMLets
    remove-module smlets -force
    }
    
    # Catch any errors
    catch
    {
    # Return the error details
    Throw $_.Exception
    }
```
