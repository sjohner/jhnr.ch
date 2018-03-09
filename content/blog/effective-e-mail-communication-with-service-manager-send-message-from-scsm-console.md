---
author: sjohner
comments: true
date: 2013-07-15 21:32:51+00:00
layout: post
slug: effective-e-mail-communication-with-service-manager-send-message-from-scsm-console
title: Effective E-Mail Communication with Service Manager - Send Message from SCSM
  Console
wordpress_id: 643
categories:
- Service Manager
tags:
- Custom Form
- E-Mail
- Exchange Connector
- SCSM Console
- Task
- Visual Studio
---

Analysts working with Service Manager are sometimes facing problems when it comes down to communication with customers or endusers. Although [Service Manager Exchange Connector](http://www.microsoft.com/en-us/download/details.aspx?id=38791) does provide a simple solution for creating and updating tickets by e-mail, this does barley solve the problem from an analysts perspective.

Analysts often have to check with someone (mostly the affected user in case of incidents) to get all the information they need or to send instructions. It would be nice if they could do this directly from Service Manager console because switching between Service Manager console and Outlook every few minutes can be pretty painful...

Another important aspect is that all the communication initiated by the analyst using an external application like Microsoft Outlook is not known to Service Manager and is therefore not logged in the incident's action log.

Unfortunately there is no out of the box "messaging" solution in Service Manager. So how to deal with that?<!-- more -->

One possible solution is to use the public available [SendEmail Management Pack ](http://gallery.technet.microsoft.com/Service-Manager-SendEmail-8171ba70)published by Travis Wright. Although this is a great solution to address messaging in Service Manager, it is limited in some ways. For example there is no way to send attachments like manuals as well as other documents and it is not possible to send a message to multiple recipients.


Based on some customer requirements we tried to address these limitations to make things easier for Service Desk employees. Together with [Marcel Zehner ](http://marcelzehner.ch/)and some of our customers we developed the idea of a SCSM messaging solution which basically has the following features:






	
  * Send e-mail to affected user by default, but be able to add other e-mail addresses or to delete the affected user's address.

	
  * Possibility to add e-mail addresses in CC

	
  * Message subject contains the incident's title but can be altered.

	
  * Predefined e-mail templates have to be available to the analyst

	
  * There is the possibility to add attachments to the message which are also linked to the corresponding incident

	
  * The analyst is free to choose whether the message should be logged in the incident's action log or not


The newly developed messaging form can be started from the task pane.

[![ConsoleMail Incident View Task](/images/consolemailincidentview.png?w=696)](/images/consolemailincidentview.png)


Running the above task opens the form.




[![ConsoleMail Form](/images/consolemailform.png)](/images/consolemailform.png)




The form automatically adds the e-mail address of the affected user of the selected incident as TO address. More e-mail addresses can be added by separating them by semicolon. The same way multiple CC addresses can be added.




User input for TO and CC fields is validated to only allow valid e-mail address formats.




The subject field is populated with the incident's title but can be modified by the analyst. As the message is sent, the incident's ID added to the subject too, so that the receiving person is able to reply to the message using Service Manager Exchange connector.




The message field can contain up to 4000 characters and predefined templates can be chosen by the analyst. Templates shown in the dropdown are default Service Manager Notification Templates and are added dynamically to the dropdown based on a configurable prefix in their DisplayName.




Once the message is sent, the attachment is added to the corresponding incident.




[![Console Mail Attachment](/images/consolemailattachment.png?w=696)](/images/consolemailattachment.png)




The message body along with TO and CC addresses is written to the action log.


[![ConsoleMail Action Log](/images/consolemailactionlog.png?w=696)](/images/consolemailactionlog.png)

To configure SMTP server and related details, we created a very basic Service Manager Administration Setting.

[![ConsoleMail Settings View](/images/consolemailsettingsview.png?w=696)](/images/consolemailsettingsview.png)

[![ConsoleMail Admin Form](/images/consolemailadminform.png?w=696)](/images/consolemailadminform.png)

As outlined above, this is a pretty basic solution to enable analysts to send e-mails directly from SCSM console but we hope it makes things a little easier for Service Desk employees :-)

Thanks to Travis Wright for providing some great input on this topic.
