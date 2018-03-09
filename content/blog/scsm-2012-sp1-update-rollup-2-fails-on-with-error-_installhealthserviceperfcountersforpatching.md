---
author: sjohner
comments: true
date: 2013-07-25 21:43:54+00:00
layout: post
slug: scsm-2012-sp1-update-rollup-2-fails-on-with-error-_installhealthserviceperfcountersforpatching
title: Installation of SCSM 2012 SP1 Update Rollup 2 fails
wordpress_id: 657
categories:
- Service Manager
tags:
- dll file
- Error
- Registry
- Update Rollup 2
---

Since I always have some troubles installing [Service Manager SP1 UR2 ](http://www.microsoft.com/en-us/download/details.aspx?id=38409)I decided to write a post on how to install Update Rollup 2 in case the installation fails with  _error occured while executing custom action:_Installhealthserviceperfcountersforpatching_




There already exist several posts on how to solve the problem, like [the one from Thomas Ellerman](http://blogs.technet.com/b/thomase/archive/2013/04/16/failed-to-apply-ur2-to-scsm-2012-sp1-performance-counters-not-found.aspx). However, I wanted to document the solution once again for my personal use so I don't have to look for it time and time again… ;-)


<!-- more -->


First of all, the error message you get when installing UR2 looks like the following:




[![KB2802159 Install Error](/images/scsm2012sp1_kb2802159_install_error.png?w=696)](/images/scsm2012sp1_kb2802159_install_error.png)




After the installation failed, you will notice that the System Center Management Service is stopped. If you are installing UR2 on your workflow server, this will probably prevent your workflows from running.




[![SC Management Service Stopped](/images/scsm2012sp1_kb2802159_servicesstopped.png)](/images/scsm2012sp1_kb2802159_servicesstopped.png)




As documented by Thomas Ellerman, this error is caused by a missing Registry Key and sometimes missing DLLs and binaries. Add the necessary key to the registry like shown below. Thomas Ellerman provides a Reg-File to import to registry in his blog post. You just have to make sure that the path of where SCSM is installed is correct.




[![MOMConnector Registry](/images/scsm2012sp1_kb2802159_registry.png?w=696)](/images/scsm2012sp1_kb2802159_registry.png)




Make sure the following files, especially MOMConnectorPerformance.dll, exist on your Management Server before adding the registry key. They reside in _C:Program FilesMicrosoft System Center 2012Service Manager_ or _C:Program FilesMicrosoft System CenterService Manager 2010_ depending on whether you directly installed Service Manager 2012 or upgraded from Service Manager 2010. If some DLL files are missing, you'll best get them from another system.




[![SCSM 2012 SP1 MOM Files](/images/scsm2012sp1_kb2802159_momfiles.png?w=696)](/images/scsm2012sp1_kb2802159_momfiles.png)




Afterwards the installation should complete successfully




[![KB2802159 Install Successfull](/images/scsm2012sp1_kb2802159_install_success.png?w=696)](/images/scsm2012sp1_kb2802159_install_success.png)




Note that also the System Center Management service is up and running again.




[![SC Management Service Started](/images/scsm2012sp1_kb2802159_servicesstarted.png?w=696)](/images/scsm2012sp1_kb2802159_servicesstarted.png)
