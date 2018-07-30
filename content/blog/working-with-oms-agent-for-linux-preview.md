---
author: sjohner
comments: true
date: 2016-01-27 22:45:07+00:00
layout: post
slug: working-with-oms-agent-for-linux-preview
title: Working with Operations Management Suite Agent for Linux (Preview)
categories:
- Operations Management Suite
tags:
- Analytics
- Cloud
- Linux
- Log
- Operations Management Suite
- Performance
---

Since everyone is talking about [Operation Management Suite (OMS)](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx) at the moment, I wanted to take a look at this relatively new cloud service on my own. There are several good blog posts out there which explain what OMS does and what it does not.

From a personal point of view I was especially interested in the Linux agent which is available in preview since some weeks. Stefan Roth already wrote [a great post about basic architecture and installation of the Linux agent](http://stefanroth.net/2015/11/13/oms-agent-for-linux-installation-preview/). I will not go into much detail about basic installation and architecture of OMS agent for Linux.

The OMS Linux agent is in a preview stage at the moment and according to [https://github.com/Microsoft/OMS-Agent-for-Linux](https://github.com/Microsoft/OMS-Agent-for-Linux) it supports the following operating systems:

* Amazon Linux 2012.09 –> 2015.09 (x86/x64)
* CentOS Linux 5,6, and 7 (x86/x64)
* Oracle Enterprise Linux 5,6, and 7 (x86/x64)
* Red Hat Enterprise Linux Server 5,6 and 7 (x86/x64)
* Debian GNU/Linux 6, 7, and 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10 (x86/x64)
* SUSE Linux Enterprise Server 11 and 12 (x86/x64)

As of version 1.1.0-2 OMS agent for Linux now also supports current Ubuntu version 15.10 as well as Agent HTTP(s) proxy. The agent can either be downloaded directly from the project's GitHub repository or on the OMS portal (which redirects you to GitHub)

Its recommended checking SHA1 checksum of the downloaded file. You can find the [checksums for both x86 and x64 sources on the project's GitHub page](https://github.com/Microsoft/OMS-Agent-for-Linux/)

I installed the new version of the OMS agent for Linux on one of my Linux boxes (Ubuntu 15.10). Installation is pretty straight forward and well documented. 

```bash   
# Get current version from GitHub
wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.1.0-2/omsagent-1.1.0-2.universal.x64.sh

# Create checksum file
echo 'ebe3eab909c1f29476eee5ae011376d6bd14ccc3b4e9788606f542cd99320cdf omsagent-1.1.0-2.universal.x64.sh' > omsagent-1.1.0-2.universal.x64.sh.sha256sum

# Compare checksum
sha256sum -c omsagent-1.1.0-2.universal.x64.sh.sha256sum

# Install OMS agent for Linux
sudo sh ./omsagent-1.1.0-2.universal.x64.sh --upgrade -w <WorkspaceID> -s <SharedKey>
```

After installation of the OMS for Linux agent you may notice some systemd messages appearing pretty much every 5 minutes in /var/log/syslog:

```bash  
systemd[1]: Stopping Operations Management Suite agent...
systemd[1]: omsagent.service: Main process exited, code=killed, status=9/KILL
systemd[1]: Stopped Operations Management Suite agent.
systemd[1]: omsagent.service: Unit entered failed state.
systemd[1]: omsagent.service: Failed with result 'signal'.
```

These messages are, unfortunately, normal behavior. Due to a technical problem with a plugin that OMS agent for Linux uses, the agent service won't respond properly to signals. Thus, to shut down the service, a  'kill -9' is done on it. This results in the systemd messages that are described above. This does not reflect a failure of the agent; this is normal for any service that is killed via the 'kill -9' mechanism on a systemd system.

According to the OMS team, they plan on changing this mechanism in a future release, which will eliminate these messages from systemd. So for now we just have to deal with that.

If your device is not listed on OMS portal after some time, onboarding may not be complete. This is no problem since you can easily onboard the agent after installation.

```bash
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <SharedKey>
```

The above command registers the agent in OMS and now the box should show up pretty quick in OMS portal

![OMS_Agent_ServerOMS](/images/oms_agent_serveroms.png)

Well this is now a basic setup of the OMS agent for Linux. By default this gives you some out of the box performance counters like _Processor Time _or _Used Disk Space_. Performance metrics to collect are controlled by the configuration in _/etc/opt/microsoft/omsagent/conf/omsagent.conf. _A list of all available performance counters can be found in [OMS agent for Linux documentation](https://github.com/Microsoft/OMS-Agent-for-Linux/).

To configure collection of performance as well as other metrics, you can either add local configuration files to _/etc/opt/microsoft/omsagent/conf/omsagent.d _or, and that's much easier, configure the metric through OMS portal and let your box pull them from OMS. If you opt to work with local configuration files, you may also want to disable central configuration through OMS portal. This can easily done by just unchecking the _Apply below configuration to my machines_ options in _Linux Performance Counters_ and _Syslog_ under the data section in settings.

![Disable Central Configuration](/images/OMS_Agent_DisabelGlobalConfig-1024x184.png)

By default, the following performance metrics are collected after installation:

* Physical Disk
* Logical Disk
* Processor
* Memory

For the above mentioned collections, all instances are collected by default. To collect processor metrics for only the specific instances, you could configure these either in the OMS portal or you can define a regular expression for the corresponding collection in the above mentioned config file.

![OMS_Agent_OmsPerfCounterConfig](/images/oms_agent_omsperfcounterconfig.png)

Since Linux servers are widely used for LAMP (Linux Apache MySQL PHP) deployments, it would be cool to have some other performance data besides _just _CPU, memory and disk.
OMS agent for Linux brings some features to cover such scenarios:

* MySQL Performance Counters
* Apache HTTP Server Performance Counters

To enable MySQL performance counters, one has to configure the performance monitoring provider for MySQL Server which will be automatically installed when a MySQL/MariaDB installation is detected. To enable the provider to access the MySQL Server, MySQL user credentials must be configured. Reference to [OMS Agent for Linux documentation](https://github.com/Microsoft/OMS-Agent-for-Linux/) for details on object permissions required by the MySQL user to collect MySQL Server performance data. Alternatively, you can specify the required MySQL credentials in a file as described here: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/develop/docs/OMS-Agent-for-Linux.md#appendix-c-managing-mysql-monitoring-credentials-in-the-authentication-file](https://github.com/Microsoft/OMS-Agent-for-Linux/)

```bash
# Enable MySQL/MariaDB performance counters
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <Username> <Password>'
sudo /opt/omi/bin/service_control restart
```

This will start sending MySQL performance data to OMS and you will soon be able to add various MySQL Server performance counters on the OMS settings page.

![OMS_Agent_MySqlPerfCounters](/images/oms_agent_mysqlperfcounters.png)

Pretty much the same procedure is necessary to configure Apache HTTP server performance counters. To enable Apache HTTP server performance counters, one has to load an Apache module in order to access performance data. The module will be installed by the OMS agent installation when a Apache HTTP server installation is detected. The module can be loaded with the following command:

```bash
# Load Apache module
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

This will start sending Apache HTTP performance data to OMS and you will soon be able to add Apache Server performance counters on the OMS settings page.

![OMS_Agent_ApachePerfCounters](/images/oms_agent_apacheperfcounters.png)

Besides various performance counters it is also possible to collect Syslog messages. After adding the agent to your Linux box, you are able to collect Syslog messages from various facilities. A [facility](https://en.wikipedia.org/wiki/Syslog#Facility) is used to specify the type of program that is logging the message. Data to collect (this applies for all data not just Syslog messages) can be defined centrally in the Operations Management Suite portal, or on the agent directly. Selections that you define in the portal for data collection should be applied to the agents within 5 minutes.

![OMS Agent Configure Syslog Data](/images/OMS_Agent_SyslogData.png)

After you completed this basic configuration you should now be able to do your first log search on Operations Management Suite. As seen below, you can for example search for Syslog messages with severity _error_ and count them by corresponding computer. This will give you a list of how many error messages were logged on which computer.

![OMS Agent Syslog Errors](/images/OMS_Agent_SyslogErrors-1.png)

To visualize your own queries, you can now easily save your searches and add them to your own custom dashboard. You can now also configure alerting and automated remediation by using Azure Automation.

![OMS Agent Custom Dashboard](/images/OMS_Agent_CustomDashboard.png)

Hopefully this gives you kind of an idea on how to work with the OMS agent for Linux. Please be aware that the agent is still in preview. There are some open issues which need to be fixed in future versions, so consider wisely if you install the agent on a production system. For example, killing the agent instead of a proper shutdown will result in tons of zombie processes  to hang around on your system.

To start your journey with OMS, I definitively recommend reading the free [Inside the Microsoft Operations Management Suite e-book](https://gallery.technet.microsoft.com/Inside-the-Operations-2928e342) published by a group of MVPs, including [Pete Zerger](https://twitter.com/pzerger), [Stanislav Zhelyazkov ](https://twitter.com/StanZhelyazkov)and [Tao Yang](https://twitter.com/MrTaoYang), to get kind of a deep dive on OMS in general.
