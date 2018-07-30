---
author: sjohner
comments: true
date: 2013-04-30 15:58:03+00:00
slug: custom-task-for-printing-labels-with-brother-p-touch-device-directly-from-scsm-console
title: Custom Task for printing labels with Brother P-Touch device directly from SCSM
  Console
categories:
- Service Manager
tags:
- Brother P-Touch
- Custom Form
- dll file
- Management Pack
- Printer
- SCSM Console
- Task
- Visual Studio
---

Do you know these small [Brother P-Touch ](http://www.p-touch.com)devices with which you can print labels in quite every manner? These devices are pretty widespread in Switzerland and many IT departments use them to print labels which they stick on a device and contain serial number, IP address and/or other key facts of this specific device. A customer for which I am developing a small asset management extension for Service Manager is also using such a device.

You can imagine that customers would like a solution where the employees could print such labels directly from Service Manager without having to use any other 3rd party solution. This would save the IT department a lot of time compared to the solution they work with now, where all assets reside in a Microsoft Access database which was connected to the Brother P-Touch Editor software. So I tried to figure out how this could be done and the solution I came up with looks like the following:

[![P-Touch Print SCSM Form](/images/form.png?w=696)](/images/form.png)

Pretty unspectacular :) I discovered that Brother provides a SDK to communicate with the P-Touch devices programmatically. So creating a new Task which implements this functionality did the trick. But of course I still had to develop a custom assembly file which does all the printing magic and which I could reference by my new label printing task.

Brother provides some pretty good code samples together with their SDK. After installing [b-PAC SDK for developers](http://www.brother.com/product/dev/label/bpac/download/index.htm)you can print to your P-Touch device from SCSM console by using the following C# code.

Note that b-PAC client component is not yet compatible with Windows 8. So you are not able to run any of the sample projects downloaded with the SDK. Also the application which is implementing your printing code has to be compiled for x86. x64 applications are not yet supported by Brother and in case of SCSM you therefore have to do a x86 installation of your console.

```csharp
    private const String templatePath = @"C:SourceBrotherTest.lbx";
    private void buttonPrintLabel_Click(object sender, RoutedEventArgs e)
    {
    bpac.DocumentClass doc = new DocumentClass();
    if (doc.Open(templatePath) != false)
    {
    //Set content of label fields by using current instance
    doc.GetObject("DisplayName").Text = (String)instance["DisplayName"];
    doc.GetObject("SerialNumber").Text = (String)instance["SerialNumber"];
    doc.GetObject("WarrantyUntil").Text = String.Format("{0:dd.MM.yyyy}", (DateTime)instance["WarrantyUntil"]);
    <p>doc.StartPrint("", PrintOptionConstants.bpoDefault);<
    doc.PrintOut(1, PrintOptionConstants.bpoDefault);
    doc.EndPrint();
    doc.Close();
    }
    else
    {
    MessageBox.Show("Open() Error: " + doc.ErrorCode);
    }
    }
```

As you see, there is no printer defined in this snippet. All the information you need for printing a label is stored in the template which you have to create with P-Touch Editor software before. This makes it pretty easy to change printer and other label definitions without changing the code. As you see in the code listing above, it is possible to define an ID for each field within a label template. This is needed to map the object properties to the fields of the label for printing.

[![P-Touch Editor](/images/labeltemplate.png)](/images/labeltemplate.png)

However it was not that simple to get it working with SCSM. To make use of the SDK you have to reference a external DLL file provided by Brother which contains all the magic to be able to directly print to your P-Touch device.

Unfortunately it was not enough to just reference the external DLL in my new assembly and to include the DLL in the management pack bundle. I got the following exception in SCSM console when I tried to print a label

_System.IO.FileNotFoundException: Could not load file or assembly 'Interop.bpac, Version=1.1.0.0, Culture=neutral, PublicKeyToken=null' or one of its dependencies. The system cannot find the file specified.
__File name: 'Interop.bpac, Version=1.1.0.0, Culture=neutral, PublicKeyToken=null_

After a while of trial and error I tried to embed the external DLL in my assembly rather than referencing it. And guess what, it worked. There is a very nice tutorial found on [codeproject.com](http://www.codeproject.com/Articles/528178/Load-DLL-From-Embedded-Resource)which describes how to embed managed and unmanaged DLL files into your own assembly.

So once I had a working assembly, I had to create a task to reference it. Do not forget to add a category for your Management Pack, otherwise your new task will never be visible to the users.
The third argument within the task definition is used to check what label has to be printed by the assembly. In my case this could be Printer or Scanner since the labels for these two assets differ.

```xml
    <categories>
    <category id="Category.itnetx.library" value="Console!Microsoft.EnterpriseManagement.ServiceManager.ManagementPack">
    <managementpackname>itnetx.library</managementpackname>
    <managementpackversion>1.0.0.0</managementpackversion>
    <managementpackpublickeytoken>8265f1180deadfe8</managementpackpublickeytoken>
    </category>
    </categories>
    <presentation>
    <consoletasks>
    <consoletask target="Printer" requireoutput="false" enabled="true" id="itnetx.library.task.ptouchprint" accessibility="Public">
    <assembly>Console!SdkDataAccessAssembly</assembly>
    <handler>Microsoft.EnterpriseManagement.UI.SdkDataAccess.ConsoleTaskHandler</handler>
    <parameters><argument name="Assembly">itnetx.ptouchprint</argument>
    <argument name="Type">itnetx.ptouchprint.PTouchPrintTaskHandler</argument>
    <argument>Asset</argument>
    </parameters>
    </consoletask>
    </consoletasks>
```

Of course the newly created assembly has to be referenced in the Management Pack and we want to give the task a shiny new icon :)

```xml
    <resources>
    <assembly hasnullstream="false" filename="itnetx.ptouchprint.dll" qualifiedname="itnetx.ptouchprint, Version=1.0.0.0, Culture=neutral, PublicKeyToken=8265f1180deadfe8" id="itnetx.ptouchprint" accessibility="Public"></assembly>
    <image hasnullstream="false" filename="Label16.png" id="Label16x16" accessibility="Public"></image>
    </resources>
    <presentation>
    <imagereferences>
    <imagereference elementid="itnetx.library.task.ptouchprint" imageid="Label16x16"></imagereference>
    </imagereferences>
    </presentation>
```

Finally, to be able to print a label directly from SCSM console, the following requirements must be fulfilled on the client computer:

* x86 SCSM Console has to be installed (no x64 applications are supported by the Brother b-pac SDK)
* Install Brother P-Touch Printer
* Install Brother b-PAC Client Component

Et voilà, printing labels for your devices is just one click away :)

[![WP_20130426_006](/images/wp_20130426_006.jpg?w=696)](/images/wp_20130426_006.jpg)
