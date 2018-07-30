---
author: sjohner
comments: true
date: 2013-04-09 20:30:59+00:00
layout: post
slug: debugging-custom-scsm-form-in-visual-studio
title: Debugging Custom SCSM Form in Visual Studio
categories:
- Service Manager
tags:
- Custom Form
- Debugging
- Process
- SCSM Console
- Visual Studio
- VSEA
---

Maybe some of you which sometimes have to create their custom forms for SCSM in Visual Studio were wondering too how to debug a custom form in case something is not working as it should. Which should not happen very often of course ;)
However, it is pretty easy to debug your forms with Visual Studio by attaching the SCSM console to the Visual Studio debugger. Unfortunately, since the Visual Studio Authoring Extensions are not yet ready for Visual Studio 2012, I will write this post based on Visual Studio 2010. You can find some more information about debugging with Visual Studio 2010 on [http://msdn.microsoft.com/en-us/library/sc65sadd](http://msdn.microsoft.com/en-us/library/sc65sadd)<!-- more -->

Well, let's assume you have your form code in a Visual Studio 2010 project. When starting with debugging your form you first have to confirm that you compile your form code in debug mode. You can change your build configuration in the settings of your project.

[![Build Configuration Debug](/images/builddebug.png?w=560)](/images/builddebug.png)

Now you can set one or more breakpoints in your code by clicking on the far left of your source code, where you see a narrow gray strip. Click on this strip at the level of the code line you want to debug. The red block over the code line indicates where the debugger will open.

[![Set Breakpoint](/images/setbreakpoint.png?w=560)](/images/setbreakpoint.png)

The next steps depend on if you are creating a new EnterpriseManagementObject within your code and on where you installed your Visual Studio. If you installed Visual Studio on one of your SCSM Management Servers, just skip the next step and continue with attaching your debugger to the console process.

If are developing your form on a computer which is not domain joined and therefore probably also is not one of your Management Servers ;) you have to install SCSM console on this device so that you are able to attach to the console process. There exists also the possibility to work with Visual Studio Remote debugger to inspect a process which is running on a remote computer, but I was not able to get it to work properly in case your Visual Studio installation is not on a computer which is joined in the same domain as the SCSM Management Server.

With your SCSM console and Visual Studio installed on a separate machine which is not joined the same domain as your SCSM Management Server, you maybe have to add some lines of code to connect to the SCSM Management Server if you are creating a new EnterpriseManagementGroup object within your assembly.
 

    
    EnterpriseManagementGroup emg = new EnterpriseManagementGroup("<SERVERNAME>");



Otherwise you will probably see an error message like the following when debugging your form.

![Unable To Step Into Server Error](/images/unabletostepintoservererror.png)

In this case you have to provide username and password to connect to the Management server.
 

    
    //This is used to create EnterpriseManagementGroup when debugging form from a non-domain computer
    //Username and password have to be provided to create a connection
    EnterpriseManagementConnectionSettings emcsettings = new EnterpriseManagementConnectionSettings("scsmserver.jhnr.ch");
    
    emcsettings.UserName = "scsmadmin";
    emcsettings.Password = new SecureString();
    emcsettings.Domain = "JHNR";
    emcsettings.ThreeLetterWindowsLanguageName = "ENG";
    
    char[] password = { 'P', 'a', 's', 's', 'w', 'o', 'r', 'd'};
    foreach (char c in password)
    {
        emcsettings.Password.AppendChar(c);
    }
    
    EnterpriseManagementGroup emg = new EnterpriseManagementGroup(emcsettings);



Ok, now you can attach the running console process to the Visual Studio debugger.

[![Attach To Process Menu](/images/attachtoprocessmenu.png?w=560)](/images/attachtoprocessmenu.png)

[![Attach To Process](/images/attachtoprocess.png?w=560)](/images/attachtoprocess.png)

You should now see a new _Locals_ tab below your source code window. This is where you can browse hierarchies of the locals' internals. Also it is possible to visualize data in a few ways too. Probably the most important thing is that you can see the value of all your local variables.

Now let's have a look at the step options in the Visual Studio debugger. When you click the green arrow in your Visual Studio toolbar, it will take you to your next breakpoint. When debugging, you most often want to advance just one step.



	
  * Step Into: Step into your method and stop immediately inside it.

	
  * Step Over: Go to the next step in your code, but do not go inside any method.

	
  * Step Out: Ignore the rest of the current method and step out to the calling method.


[![Debug Menu](/images/debugmenu.png?w=560)](/images/debugmenu.png)

Et voila, you are ready to open your newly created form in SCSM console and to bring the action :) As soon as you hit the first of your breakpoints, Visual Studio will be focused and you can step through your code.

[![Breakpoint](/images/breakpoint.png?w=560)](/images/breakpoint.png)

Happy debugging :)
