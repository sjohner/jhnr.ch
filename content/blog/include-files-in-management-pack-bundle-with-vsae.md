---
date: 2013-01-03 20:35:55+00:00
slug: include-files-in-management-pack-bundle-with-vsae
title: Include DLL Files in Management Pack Bundle with Visual Studio Authoring Extensions
  (VSAE)
categories:
- Service Manager
tags:
- Service Manager
- Management Pack Bundle
- Visual Studio
---

Since I am mostly using Visual Studio with Authoring Extensions to design Management Packs, I was happy to find an easy way to add external dll resource files to a Management Pack Bundle.




Marcel Zehner wrote a nice [blog post](https://web.archive.org/web/20130114082218/http://blog.scsmfaq.ch:80/2013/01/03/visual-studio-authoring-extensions-vsae-part-1-creating-a-new-ci-class/) about creating a custom CI class with VSAE.




When I was working on a project for which I had to design some custom Service Manager Console forms I had to bundle a DLL file which contained the custom forms with the Management Pack .




This could have been done by just build the Management Pack in Visual Studio and afterwards create the Management Pack bundle with Fastseal.exe to include appropriate the DLL file.




However this is pretty annoying if you are developing forms and (in my case) constantly changing Management Packs and testing things in Service Manager Console.


<!-- more -->


To make things easier you can reference the DLL (or any other) file in your Service Manager 2012 Management Pack Visual Studio Project. Simply right-click on "References" and choose "Add Reference". You are now able to browse to the DLL file you want to bundle with your Management Pack.




[](http://blog.jhnr.ch/2013/01/03/include-files-in-management-pack-bundle-with-vsae/vsae_add_reference/)[![Add reference](/images/vsae_add_reference.png)](/images/vsae_add_reference.png)




[![Referenced DLL file](/images/vsae_referenced_dll.png)](/images/vsae_referenced_dll.png)




Go to the properties of the newly added File and change "Package To Bundle" to "True"




[![Reference properties](/images/vsae_reference_properties.png)](/images/vsae_reference_properties.png)




Now the file is going to be bundled with the Management Pack if you have chosen to "Generate sealed and signed management pack" in the properties of the project.




[![Build properties](/images/vsae_build_properties.png)](/images/vsae_build_properties.png)




Happy authoring :-)




Stefan



