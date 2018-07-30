---
author: sjohner
comments: true
date: 2013-03-17 15:29:34+00:00
layout: post
slug: how-to-create-a-new-scsm-object-from-custom-form
title: How to create a new SCSM object from custom form
categories:
- Service Manager
tags:
- Code-Behind
- Custom Form
- Management Pack
- Management Pack Bundle
- Visual Studio
- XAML
---

I recently had to create a custom form on which I had to place a button to create a new SCSM object. The goal of the form was that one could create a new Phone object directly by clicking a button on a custom Person object form. This may be not a common requirement but is pretty handy if you want to link an object with another one which does not yet exist.

First of all I had to create a button on my custom XAML form and a related Click Event Handler in the code-behind of the form.
 

    
    <Button Grid.Row="2" Grid.Column="1" Margin="10,20,0,0" VerticalAlignment="Top" HorizontalAlignment="Left" Name="buttonIdentityAdd" Height="20" Width="100" Click="buttonIdentityNew_Click">New Phone</Button>


 

    
    private void buttonNewPhone_Click(object sender, RoutedEventArgs e)
    {
     createNewPhone();
    }



I also added a SingleInstancePicker dialog to be able to link my new Phone object to a Person after being created by my _New Phone_ button.
 

    
    <smcontrols:SingleInstancePicker Grid.Row="2" Grid.Column="0" HorizontalAlignment="Stretch" Margin="10,20,0,0" Name="singleInstancePickerPhone" BaseClassId="3438d2e8-ec37-7a6f-ced5-bca95aa9d5e8" Instance="{Binding Path=PersonHasPhone, UpdateSourceTrigger=PropertyChanged, Mode=TwoWay}" />



This should then look something like this

[![New Phone Button](/images/newphonebutton1.png?w=300)](/images/newphonebutton1.png)
<!-- more -->
Ok, I got a button on my custom XAML form right beside the SingleInstancePicker dialog. Next I had to add some code for the method _createNewPhone()_ to bring this button to life :)

The GUID of the Phone class which is needed to create the necessary _ManagementPackClass_ object can be found by using Powershell
 

    
    Get-SCSMClass –name itnetx.class.phone$ | select Id



Also the _ManagementPackPublicKeyToken _is needed to create a _ManagementPack_ object. The PublicKeyToken can be found by using _sn.exe_
 

    
    sn.exe -T FileNameOfYourSealedManagementPack



As you see, it is also possible to already add some property values to the newly created Phone object.
 

    
    internal static void createNewPhone()
    {
            // Create EnterpriseManagementGroup
            EnterpriseManagementGroup emg = new EnterpriseManagementGroup("localhost");
    
            //Get Phone Library Management Pack)
            ManagementPack mpItnetxLibrary = emg.ManagementPacks.GetManagementPack("itnetx.library", "494dc115bcf3c575", new Version(1, 0, 0, 0));
    
            //Get Phone Class
            ManagementPackClass classPhone = emg.EntityTypes.GetClass(new Guid("3438d2e8-ec37-7a6f-ced5-bca95aa9d5e8"));
    
            //Create and commit Phone object. Object has to be commited before the console form can be opened
            CreatableEnterpriseManagementObject cemoPhone = new CreatableEnterpriseManagementObject(emg, classPhone);
    
            //Add some property values
            cemoPhone[classPhone, "DisplayName"].Value = "TestPhone";
            cemoPhone.Commit();
    
            //Convert EnterpriseManagementObject to IDataItem
            EnterpriseManagementObjectDataType dataType = new EnterpriseManagementObjectDataType(classPhone);
            IDataItem itemIdentity = dataType.CreateProxyInstance(cemoPhone);
    
            //Open Console form for created object
            ConsoleContextHelper.Instance.PopoutForm(itemIdentity);
    }



Build your new form assembly and add it to your Management Pack Bundle to import to Service Manager. See [my previous post ](http://scsmlab.com/2013/01/03/include-files-in-management-pack-bundle-with-vsae/)on how to add a DLL file to a Management Pack Bundle in Visual Studio.

Et voilà, a click on the new button opens the form for the newly created Phone object.

Hope that helps :)
