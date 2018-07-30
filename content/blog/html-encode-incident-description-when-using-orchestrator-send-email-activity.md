---
author: sjohner
comments: true
date: 2014-11-25 17:16:48+00:00
layout: post
slug: html-encode-incident-description-when-using-orchestrator-send-email-activity
title: HTML encode Incident description when using Orchestrator Send Email Activity
categories:
- Orchestrator
tags:
- E-Mail
- HTML
- PowerShell
- Runbook
- System Center
---

When using the Send Email activity in System Center Orchestrator, one can choose between sending E-mails in "normal" text format or sending them HTML formatted. Well, since HTML offers much more options to format a message this is normally be the way to go.

If you would like Orchestrator to use HTML format when sending E-mails, you can simply change the message format in Advanced properties of the corresponding Send Email activity.

[![Send Email Message Options](/images/sendemailmessageoptions.png)](/images/sendemailmessageoptions.png)
<!-- more -->
However sending E-mails in HTML format has a drawback when using published data in your E-mail body which can contain line breaks and special characters. For example the description field of Incidents is very likely to contain line brakes. But these line breaks are simply not showing in your HTML message since there is no tag specifying them.

One solution for this is to HTML encode the description's published data before using it in a HTML formatted message. A sample runbook which gets an Incident and afterwards sends an E-mail message to a specified address could look like the following.

[![Runbook Screenshot](/images/runbook.png)](/images/runbook.png)

The custom script activity uses the .Net _System.Web _assembly to HTML encode the Incident description and replaces line brakes with _<br />._

[![RunScript Activity Screenshot](/images/runscript.png?w=604)](/images/runscript.png)

After encoding the Incident description text, the encoded text has to be published on the data bus. With this done it can be consumed by the _Send E-Mail _activity. For example it can be used in the e-mail body.

[![SendMail](/images/sendmail.png?w=604)](/images/sendmail.png)

You can find the code snippet to encode the description text right below. Of course you can use this snippet also to encode other fields than Incident description. Have fun! :-)

    
    $DescriptionText = @'
    //Published data from Get Object (Incident) activity
    '@
    
    Add-Type -AssemblyName System.Web
    $EncodedDescriptionText = [System.Web.HttpUtility]::HtmlEncode($DescriptionText).Replace("`r`n", "<br />")
    
