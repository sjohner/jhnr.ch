---
author: sjohner
comments: true
date: 2013-05-29 15:03:01+00:00
layout: post
slug: insert-ci-property-values-into-microsoft-word-template-and-print-directly-from-service-manager-console
title: Insert CI property values into Microsoft Word Template and print directly from
  Service Manager Console
categories:
- Service Manager
tags:
- dll file
- Management Pack Bundle
- Printer
- Process
- SCSM Console
- Task
- Template
- Visual Studio
- VSEA
- Word
- XAML
---

I am always impressed how customizable SCSM can be. Lately I was working for a company who manages their users smartphones and SIM cards in SCSM. That's already pretty cool but there is always some space for improvements :-) The company process of handing out a new phone together with a SIM card to an employee works as follows:



	
  * An employee reports the need for a mobile phone to the IT department

	
  * A new contract with one of Switzerlands major telecommunications provider will be closed for this employee.

	
  * The new phone together with the SIM card is delivered to the IT department which creates a new record for them in SCSM

	
  * IT department delivers the phone and SIM card together with a printed form to the employee who has to sign this form to confirm that he received the phone.


Now this all sounds pretty straightforward but when it comes to the last step of the above process there was a small drawback when handing out a printed form to the employee. Before switching to Service Manager for managing the phone assets, the IT company maintained a Microsoft Office Excel worksheet which contained all the phones and SIM cards. Pretty old-school you would say but it had one major advantage compared to Service Manager: They could easily print a form based on the information contained in the Excel worksheet and hand it out to the employee together with the phone.
<!-- more -->
Ok - Service Manager also provides a very basic task to print information of a CI but as you may know the printout is not formatted in a manner that one could hand out this to an employee. Furthermore it is not possible to just print a small set of information or to add custom text and logos.

To improve this last step of handing out a printed form I was asked to create a custom SCSM task which prints a predefined Microsoft Word template which contains some of the phone data from SCSM. I wanted to share the solution in case anyone else has a similar demand for printing information from SCSM. Basically the result looked like the following:

[![Printout Confirmation Phone Transfer](/images/confirmationword2.png?w=212)](/images/confirmationword2.png)

A custom class for SIM cards and phones was already available in Service Manager. Furthermore I already created custom forms for this class. As described above, the goal of this custom task was to get the values of several properties of a SIM card object like below and insert them in an existing Word template.

[![SCSM SIM Card Form](/images/form.png?w=300)](/images/form.png)

First of all I had to create a task which prints a existing Word template but also updates this template with some values from SCSM before printing it.
 

    
    <Presentation>
        <ConsoleTasks>
          <!--Task simcard class-->
          <ConsoleTask ID="scsmlab.simcard.library.task.templateprint" Accessibility="Public" Enabled="true" Target="scsmlabclasssimcard" RequireOutput="false">
            <Assembly>Console!SdkDataAccessAssembly</Assembly>
            <Handler>Microsoft.EnterpriseManagement.UI.SdkDataAccess.ConsoleTaskHandler</Handler>
            <Parameters>
              <Argument Name="Assembly">scsmlab.simcard.templateprint</Argument>
              <Argument Name="Type">scsmlab.simcard.templateprint.TaskHandler</Argument>
            </Parameters>
          </ConsoleTask>
        </ConsoleTasks>
        <ImageReferences>
          <ImageReference ElementID="scsmlab.simcard.library.task.templateprint" ImageID="Word16x16" />
        </ImageReferences>
      </Presentation>
      <LanguagePacks>
        <LanguagePack ID="ENU" IsDefault="true">
          <DisplayStrings>
            <DisplayString ElementID="scsmlab.simcard.library.task.templateprint">
              <Name>Print transfer confirmation</Name>
            </DisplayString>
          </DisplayStrings>
        </LanguagePack>
      </LanguagePacks>
      <Resources>
        <Assembly ID="scsmlab.simcard.templateprint" Accessibility="Public" FileName="scsmlab.simcard.templateprint.dll" HasNullStream="false" QualifiedName="scsmlab.simcard.templateprint, Version=1.0.0.0, Culture=neutral, PublicKeyToken=8275f1190deadcc4" />
        <Image ID="Word16x16" Accessibility="Public" FileName="Word16.png" HasNullStream="false" />
      </Resources>



As you see in the code listing above, the newly created task uses a custom assembly which contains all the action necessary to print a Word template directly from SCSM. The task is displayed for all SIM card objects in SCSM.

[![SCSM Task Print Word Document](/images/task1.png)](/images/task1.png)

Now I needed to develop the custom assembly which contains all the magic for updating and printing the existing Word template. But first I created the new Word template which should be printed afterwards by using the above defined task.

Make sure the Word document is saved as template (dotx) and in a path where it is accessible by the user which will print the document. To be able to update the document with values from SCSM like phone number and PIN etc. I had to define so called _Bookmarks_ in the template. In this context, bookmarks are used as placeholders for values which are later on inserted from Service Manager.

[![Microsoft Word Bookmarks](/images/bookmarks.png)](/images/bookmarks.png)

What is missing now is the custom assembly to print the document. I developed this in Visual Studio 2010 by creating a new class which inherits _ConsoleCommand_ from _Microsoft.EnterpriseManagement.UI.SdkDataAccess.dll_ and overrides_ ExecuteCommand()_ to contain my code for printing the document.
 

    
    public class TaskHandler : ConsoleCommand
    {
    [STAThread]
    public override void ExecuteCommand(IList nodes, NavigationModelNodeTask task, ICollection parameters)
    {



Since every client in the before mentioned company which has Service Manager Console installed also has Microsoft Office 2010 installed, I decided to use the Microsoft
Office 2010 Primary Interop Assemblies to open, modify etc. the Word document programmatically. More information about Microsoft Office Primary Interop Assemblies can be found on [MSDN](http://msdn.microsoft.com/en-us/library/15s06t57.aspx). Microsoft Office 2010 Primary Interop Assemblies can be downloaded from [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=3508).

Basically my code for opening, altering and printing the predefined Word template looks like the following, where _inst _is the current instance. Since I do not change anything in SCSM it does not depend if the task is run from a form or from a view. Depending on the SCSM data type (String, Date, Enum) the values may be converted to a String value.

Since I am no C# expert, feel free to correct and adjust the following code :-)
 

    
    try
    {
    //Create new Application and open the existing Word template
    Microsoft.Office.Interop.Word.Application application = new Microsoft.Office.Interop.Word.Application ();
    application.Documents.Open(@"\scsmlab.comSCSMPhoneTransfer.dotx");
    
    //Set device owner
    application.ActiveDocument.Bookmarks["DeviceOwner"].Select();
    application.Selection.TypeText(inst["SIMCardIsOwnedByUser"].ToString());
    
    //Set phone number
    application.ActiveDocument.Bookmarks["PhoneNumber"].Select();
    application.Selection.TypeText((String)inst["DisplayName"]);
    
    //Set device type
    application.ActiveDocument.Bookmarks["DeviceType"].Select();
    application.Selection.TypeText(inst["scsmlabclasssimcarddevicetype"].ToString());
    
    //Set device IMEI
    application.ActiveDocument.Bookmarks["DeviceIMEI"].Select();
    application.Selection.TypeText((String)inst["scsmlabclasssimcarddeviceimei"]);
    
    //Set PIN
    application.ActiveDocument.Bookmarks["PIN1"].Select();
    application.Selection.TypeText((String)inst["scsmlabclasssimcardpin1"]);
    
    //Print document to default printer
    application.ActiveDocument.PrintOut(true, false, WdPrintOutRange.wdPrintAllDocument,
            Item: WdPrintOutItem.wdPrintDocumentContent, Copies: "2", Pages: "",
            PageType: WdPrintOutPages.wdPrintAllPages, PrintToFile: false, Collate: true,
            ManualDuplexPrint: false);
    }
    catch (System.Exception ex)
    {
    //Show error so it looks like regular console exception
    ConsoleContextHelper.Instance.ShowErrorDialog(ex, "Error", ConsoleJobExceptionSeverity.Error);
    }



Et voilà, I am able to fill in values of an object in SCSM to a Word document and print it directly. It is also be possible to show a print dialog rather than printing the document directly to the default printer. Many code examples and step-by-step procedures for handling Microsoft Office documents can be found on [MSDN](http://msdn.microsoft.com/en-us/library/vstudio/bb157880.aspx).

Do not forget to bundle the built DLL file with your Management Pack and import the bundle to your Service Manager installation. See one of my previous posts on [how to include DLL files in a Management Bundle with Visual Studio Authoring Extensions](http://scsmlab.com/2013/01/03/include-files-in-management-pack-bundle-with-vsae/).
