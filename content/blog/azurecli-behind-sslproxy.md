---
author: sjohner
comments: true
date: 2018-05-16T22:50:45+02:00
slug: working-with-azure-cli-behind-ssl-intercepting-proxy-server
title: "Working with Azure CLI behind SSL intercepting proxy server"
categories:
- Cloud
- Azure
- Linux
tags:
- SSL
- Azure
- CLI
- Proxy
- Certificate
- Error
- VM
- Verification
---

The Azure CLI is one of Azure's command-line experiences for managing Azure resources (besides Azure PowerShell). Azure CLI is open source and built on Python. The cool thing about Azure CLI is that you can use it with pretty much all known platforms like macOS, Windows and Linux. This means you do not have to learn another command line tool for each platform.


When working with Azure CLI on Linux in a corporate environment it might happen to you, that creating a VM fails with the following error message:

```
sjohner@donald:~$ az vm create -n UbuntuVM -g MyRG --image UbuntuLTS --generate-ssh-keys
az vm create: error: HTTPSConnectionPool(host='raw.githubusercontent.com', port=443): Max retries exceeded with url: /Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json (Caused by SSLError(SSLError("bad handshake: Error([('SSL routines', 'tls_process_server_certificate', 'certificate verify failed')],)",),))
```

This might be due to your ssl intercepting corporate proxy server. The proxy server might be using unsigned certificates or more likely it is using a certificate which is not trusted by your system. I am no fan of ssl interception because of various reasons but unfortunately it seems pretty common in bigger enterprises.

However if you are lucky like me and working behind a corporate proxy, easiest solution to work around the above issue this is to disable the certificate check across the CLI. This might not be a very safe option but works. It can be done by setting the environment variable *AZURE_CLI_DISABLE_CONNECTION_VERIFICATION* to any value

```
export AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=anycontent
sjohner@donald:~$ az vm create -n UbuntuVM -g MyRG --image UbuntuLTS --generate-ssh-keys
Connection verification disabled by environment variable AZURE_CLI_DISABLE_CONNECTION_VERIFICATION
```

Since Azure CLI is based on Python, another and probably better option is to set *REQUESTS_CA_BUNDLE* environment variable to make python requests use the system ca-certificates bundle
```
sjohner@donald:~$ export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
```

This requires that the corresponding certificate is present in your systems local certificate trust store. For Debian based systems this can be done by adding the certificate to `/usr/local/share/ca-certificates/` and update the store with `update-ca-certificates`

```
sjohner@donald:~$ sudo cp MyWebProxyCertificate.crt /usr/local/share/ca-certificates/
sjohner@donald:~$ sudo update-ca-certificates
Updating certificates in /etc/ssl/certs...
1 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
```

You probably want to set this environment variable permanentely by placing it into your .bashrc file

```
sjohner@donald:~$ echo "export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt" >> ~/.bashrc
```

 With one of these solutions in place you should now be able to create your VM

```
sjohner@donald:~$ az vm create -n UbuntuVM -g MyRG --image UbuntuLTS --generate-ssh-keys
SSH key files '/home/sjohner/.ssh/id_rsa' and '/home/sjohner/.ssh/id_rsa.pub' have been generated under ~/.ssh to allow SSH access to the VM. If using machines without permanent storage, back up your keys to a safe location.
{
  "fqdns": "",
  "id": "/subscriptions/08f66ad0-fa16-4b2d-b046-0a04879c3b01/resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachines/UbuntuVM",
  "location": "westeurope",
  "macAddress": "00-0D-3A-39-44-A4",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "104.214.223.69",
  "resourceGroup": "MyRG",
  "zones": ""
}
```