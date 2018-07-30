---
author: sjohner
comments: true
date: 2014-04-14 17:47:02+00:00
slug: create-custom-scsm-console-workspace-with-high-res-folder-image-on-overview-page
title: Create custom SCSM Console workspace with High-Res folder image on overview
  page
categories:
- Service Manager
tags:
- Folder
- Hi-Res
- Image
- Management Pack
- Overview
- SCSM Console
- View
- Workspace
- Wunderbar
---

If you are working with Service Manager you definitively know the default workspaces aka wunderbars which separate for example Work Item views from Config Item views. Sometimes it would be very handy to be able to create your own workspaces which could for example separate your custom CI classes and views from the default Service Manager views. However, creating a new workspace in Service Manager Console is not possible. But guess what: This can be done by manually adding some XML code to one of your Management Packs.

[![Custom SCSM Cars Workspace](/images/customcarsworkspace.png?w=604)](/images/customcarsworkspace.png)

Since a workspace is basically nothing other than a normal folder which has _RootFolder_ as parent folder you first have to create an additional folder in a Management Pack by adding the following XML snippet.

```xml
    <Folders>
         <Folder ID="workspace.cars" Accessibility="Public" ParentFolder="Console!ServiceManager.Console.RootFolder" />
    </Folders>
```

Make sure you reference the _Microsoft.EnterpriseManagement.ServiceManager.UI.Console.mp_ Management Pack which contains some tasks we'll need later on. Also make sure, a reference to the _System.Library_ Management Pack is present.

```xml
    <Reference Alias="Console">
         <ID>Microsoft.EnterpriseManagement.ServiceManager.UI.Console</ID>
         <Version>7.5.3079.0</Version>
         <PublicKeyToken>31bf3856ad364e35</PublicKeyToken>
    </Reference>
    <Reference Alias="System">
         <ID>System.Library</ID>
         <Version>7.5.8501.0</Version>
         <PublicKeyToken>31bf3856ad364e35</PublicKeyToken>
    </Reference>
```

Add the appropriate _DisplayString_ element to name the new wunderbar and you can also add an _ImageReference_ if you want to give it a shiny icon. The wunderbar icon size is 16x16px for collapsed and 24x24px for expanded wunderbars. Since we are going to use different image sizes depending on whether the workspace is expanded or collapsed, we need to define an _ImageReference_ element for each one of them.

```xml
    <DisplayString ElementID="workspace.cars">
         <Name>Cars</Name>
    </DisplayString>
```

```xml
    <ImageReferences>
         <ImageReference ElementID="workspace.cars" ImageID="workspace.cars.image24" />
         <ImageReference ElementID="workspace.cars" ImageID="workspace.cars.image16" />
    </ImageReferences>
```

Also you need to add a _Resource_ element which contains your image files.

```xml
    <Resources>
         <Image ID="workspace.cars.image24" Accessibility="Public" FileName="workspace.cars.image24.png" />
         <Image ID="workspace.cars.image16" Accessibility="Public" FileName="workspace.cars.image16.png" />
    </Resources>
```

Besides the _ImageReferences_ defined above, we have to tell Service Manager when to use which image. So we need to add a _Category_ element for each of the newly added image. Each _Category_ element references an enumeration that tells Service Manager if this image is going to be used as a 16xp or 32px icon.
What? I thought you told me that expanded workspaces are using icons of 24px in size? Yes, and that's still true :-) But there exist only enumeration values for 16 and 32px so we have to use the 32px one in this case. You will see later on in this post that there are other applications where we need to use the 32px enumeration for images of another size

```xml
    <Categories>
        <Category ID="Category.workspace.cars.image24x24" Target="cars.image24" Value="System!System.Internal.ManagementPack.Images.u32x32Icon" />
        <Category ID="Category.workspace.cars.image16x16" Target="cars.image16" Value="System!System.Internal.ManagementPack.Images.u16x16Icon" />
    </Categories>
```

If you want to be able to create new folders and views within your new workspace in SCSM console, it is necessary to add a _FolderItem_ for the workspace folder

```xml
    <FolderItem ElementID="Console!Microsoft.EnterpriseManagement.ServiceManager.UI.Console.Task.CreateFolder" ID="workspace.cars.folderitem.createfolder" Folder="workspace.cars" />
    <FolderItem ElementID="Console!Microsoft.EnterpriseManagement.ServiceManager.UI.Console.Task.CreateGridView" ID="workspace.cars.folderitem.createview" Folder="workspace.cars" />
```


Instead of providing the ability to create folders and views directly in the workspace, it may be more appropriate to create a static folder and to enable folder and view creation for this folder. You can sure do this by adding a new folder to your Management Pack which has the newly created wunderbar as _ParentFolder_

```xml
    <Folder ID="folder.cars" Accessibility="Public" ParentFolder="workspace.cars" />
```

Additionally to display "Create View" and "Create Folder" tasks for this new parent folder, the following _FolderItem_ have to be added for the newly created folder.

```xml
    <FolderItem ElementID="Console!Microsoft.EnterpriseManagement.ServiceManager.UI.Console.Task.CreateFolder" ID="folder.cars.folderitem.createfolder" Folder="folder.cars" />
    <FolderItem ElementID="Console!Microsoft.EnterpriseManagement.ServiceManager.UI.Console.Task.CreateGridView" ID="folder.cars.folderitem.createview" Folder="folder.cars" />
```

Create a Management Pack Bundle which contains the MP XML together with the images. If you need further information on how to create a Management Pack Bundle file, you can refer to [this excellent blog post](http://www.concurrency.com/blog/management-pack-bundling-in-system-center-service-manager-2012/)written by [Lee Berg ](https://twitter.com/LeeAlanBerg)to get additional information on this topic.
Import the MPB file, et voilà: a new workspace which contains your new folder is visible in the bottom left corner of your SCSM Console. When you collapse the workspace you should see a smaller version of the assigned image.

[![Custom SCSM Workspace](/images/customworkspace.png?w=604)](/images/customworkspace.png)

So far so good, but wouldn't it be nice to have a suitable image also for the new root folder in your workspace? Sure, let's do this! All you have to do is to add the following _ImageReference_ right?

```xml
    <ImageReference ElementID="folder.cars" ImageID="cars.image16" />
```

Almost ;-) This will assign the image to your new folder but what about the folder preview like seen in the screenshot below? Pretty blurry no? The image shown in the folder preview should have a size of 64x64px. However Service Manager just takes the 16x16px image used for the folder icon and scales it.

[![SCSM Folder Preview Blurry](/images/folderpreviewblurry.png)](/images/folderpreviewblurry.png)

So to change this we are going to need another image, a corresponding _Category_ element and an _ImageReference_ element. Add another image for the folder preview as you did for the folder and the workspace but this time it should have the size of 64x64px.
Add a second _ImageReference_ element to assign the new image to your root folder in addition to the existing 16px image.

```xml
    <ImageReference ElementID="folder.cars" ImageID="cars.image64" />
```

As your already did for collapsed and expanded workspace icons, you have to tell Service Manager when to use which image. This can be done by adding another category targeting your new 64x64px image. Since there only exist enumeration values for 16 and 32px, you again have to use the 32px one in this case.

```xml
    <Category ID="Category.workspace.cars.image64x64" Target="cars.image64" Value="System!System.Internal.ManagementPack.Images.u32x32Icon" />
```

Import the updated Management Pack and you will see that the folder preview shows you a much nicer image than before.

[![SCSM Folder Preview Sharp](/images/folderpreviewsharp.png)](/images/folderpreviewsharp.png)
