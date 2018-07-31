---
author: sjohner
date: 2016-08-26 09:44:18+00:00
title: Adding your CollectD metrics to Operations Management Suite
categories:
- Linux
- Operations Managment Suite
tags:
- Cloud
- CollectD
- Dashboard
- Linux
- OMS
- Operations Management Suite
---

As of version [v1.1.0-217](http://github.com/Microsoft/OMS-Agent-for-Linux/releases) OMS Agent for Linux now has support for CollectD. [CollectD](https://collectd.org/) is an open source Linux daemon that periodically collects data from applications and system level information. CollectD actually brings [a lot of very useful plugins](https://collectd.org/wiki/index.php/Table_of_Plugins). Example applications CollectD can collect metrics from include the Java Virtual Machine, MySQL Server, Nginx, etc. CollectD is often used in combination with [Grafana](https://github.com/grafana/grafana) which enables you to create awesome dashboards to visualize the collected metrics.

If you are using OMS you might want to consider to forward the data from CollectD into Operations Management Suite. This is actually pretty easy to configure and once the data is present in OMS you can benefit from [alerting and automation](https://azure.microsoft.com/en-us/documentation/articles/log-analytics-alerts/) functionality OMS provides you with.

When installing the OMS Agent for Linux by using the _--collectd_ switch, the agent listens on port 26000 for CollectD metrics and then converts them to OMS schema metrics. CollectD uses the default _write_http_ plugin to forward metric data in JSON format over port 26000 to OMS Agent for Linux.

[![OMS CollectD Architecture](/images/OMS_CollectD_Architecture.png)](/images/OMS_CollectD_Architecture.png)

The following example was actually demoed at [System Center Universe 2016](http://www.systemcenteruniverse.ch). It uses CollectD and OMS Agent for Linux to collect ping, uptime and user metrics from different Linux boxes. These information are afterwards sent to OMS and visualized in a custom dashboard. There is also a [post available on Microsoft Technet](https://blogs.technet.microsoft.com/msoms/2016/07/14/bring-your-custom-collectd-metrics-into-the-oms-log-analytics-platform/) which describes the set up for metric retrieval from MySQL Server.

First of all you will need to install CollectD on your Ubuntu box. This can be done by using the following command. Make sure you use _no-install-recommends_ because otherwise a lot of packages are installed which are not necessarily needed in this scenario

```bash
apt-get install collectd --no-install-recommends collectd
```

If OMS Agent for Linux is already installed on your machine, you can run the following command to set up CollectD to forward metrics to OMS Agent for Linux

```bash    
sudo /opt/microsoft/omsagent/bin/omsadmin.sh –c
```

Now modify your CollectD configuration to make sure _uptime_, _ping_ and _users_ plugins are enabled. The Users plugin simply measures how many users are logged in on a host. This could be a good metric to create OMS alerts from. An unusual amount of logged in users on a host may be a sign of an ongoing attack or other suspicious activity.

The [**Ping plugin**](https://collectd.org/documentation/manpages/collectd.conf.5.shtml#plugin_ping) measures network latency using ICMP "echo requests", usually known as "ping". Network latency is measured as a round-trip time. So by monitoring the ping connectivity to your hosts you are able get some basic insights about your network performance.

```bash
vim /etc/collectd/collectd.conf

LoadPlugin uptime
LoadPlugin users
LoadPlugin ping
<Plugin "ping">
   Host "labvm01.lab.jhnr.ch"
   Host "labvm02.lab.jhnr.ch"
   Host "labvm03.lab.jhnr.ch"
</Plugin>
```

When working with the _Ping_ plugin, make sure you install _liboping0 _package because this is required for proper functioning of the plugin. Unfortunately _liboping0_ is not included as dependency when installing collectd package.

```bash
sudo apt-get install liboping0
```

If you did not install _liboping0_ you will see the following error messages appearing in _/var/log/syslog_ as soon as you restart collectd with _Ping_ module installed:

```bash
Aug  7 10:49:01 LabVM03 collectd[4119]: lt_dlopen ("/usr/lib/collectd/ping.so") failed: file not found. The most common cause for this problem is missing dependencies. Use ldd(1) to check the dependencies of the plugin / shared object.
Aug  7 10:49:01 LabVM03 collectd[4119]: ERROR: lt_dlopen ("/usr/lib/collectd/ping.so") failed: file not found. The most common cause for this problem is missing dependencies. Use ldd(1) to check the dependencies of the plugin / shared object.
Aug  7 10:49:01 LabVM03 collectd[4119]: plugin_load: Load plugin "ping" failed with status 1.
Aug  7 10:49:01 LabVM03 collectd[4119]: Found a configuration for the `ping' plugin, but the plugin isn't loaded or didn't register a configuration callback.
```

After configuring CollectD, it is now time to install the OMS agent for Linux. Make sure you are using the _--collectd_ switch

```bash 
sudo sh ./omsagent-1.2.0-25.universal.x64.sh --upgrade --collectd -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

When using _--collectd _switch, the OMS install routine will actually add a config file _oms.conf_ to _/etc/collectd/collectd.conf.d. _This conf file will make sure that CollectD is forwarding all the metrics in JSON format to OMS Agent for Linux using the _write_http_ plugin.

```bash 
vim /etc/collectd/collectd.conf.d/oms.conf
    
LoadPlugin write_http
<Plugin write_http>
     <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
     </Node>
</Plugin>
```

The OMS Agent for Linux listens on port 26000 for CollectD metrics and then converts them to OMS schema metrics. To have a common model between infrastructure metrics that are already collected by the OMS Agent for Linux and the additional metrics that CollectD gathers, the following schema mapping is used:

[![OMS CollectD Schema](/images/OMS_CollectD_Schema-e1472052330883.png)](/images/OMS_CollectD_Schema.png)

If everything went well and , you should soon see some CollectD metrics data populated in your OMS workspace. If not, you probably have to restart CollectD service because it did not yet pick off the configuration changes made by OMS Agent for Linux installation routine.

```bash 
service collectd restart
```

As you notice, the CollectD type (users in this case) is mapped to the ObjectName field in OMS.

[![OMS_CollectD_Results](/images/OMS_CollectD_Results.png)](/images/OMS_CollectD_Results.png)

Now that the data is available in OMS, you can pretty easily create some [custom dashboards](https://azure.microsoft.com/en-us/documentation/articles/log-analytics-dashboards/) based on your CollectD metrics. You can download the below dashboard from [here](https://github.com/sjohner/OMS-Dashboards).

[![OMS CollectD Dashboard](/images/OMS_CollectD_Dashboard-1024x405.png)](/images/OMS_CollectD_Dashboard.png)

For example, use a query like the following to get logged in users aggregated by computer.

```bash
Type=Perf ObjectName=users | measure max(CounterValue) by Computer interval 5minutes
```

When working with the _Ping_ metrics you want to aggregate on CounterName field because that is where the host name of the pinged computer is found. The following should give you the average RTT for a given host.

```bash
Type=Perf ObjectName=ping | measure avg(CounterValue) by CounterName interval 5minutes
```
