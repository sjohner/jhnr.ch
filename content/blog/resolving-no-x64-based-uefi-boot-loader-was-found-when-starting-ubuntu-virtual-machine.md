---
author: sjohner
comments: true
date: 2017-02-23 17:11:21+00:00
layout: post
slug: resolving-no-x64-based-uefi-boot-loader-was-found-when-starting-ubuntu-virtual-machine
title: Resolving "No x64-based UEFI boot loader was found" when starting Ubuntu virtual
  machine
categories:
- Linux
tags:
- Boot loader
- GRUB
- Hyper-V
- Linux
- Secure Boot
- Ubuntu
- UEFI
- Virtualization
---

I have a small, virtualized Linux lab on my notebook which I only use occasionally. Since I reinstalled my machine some weeks ago I had to recreate all Hyper-V virtual machines which lounge around somewhere on the disk. No big deal I thought, but when starting my Ubuntu virtual machines, I got the following message saying that "No x64-based UEFI boot loader was found".

Hmm... well I am no Hyper-V expert in any way so I was first a little confused when I hit this message. I started debugging by disabling Secure Boot in VM settings but this did not change the situation at all.

[![No x64-based UEFI boot loader was found](/images/Hyper-V.png)](/images/Hyper-V.png)

After some troubleshooting I noticed that for this particular Linux VM, the File boot entry created by the Ubuntu installer was missing. It seems as if for Windows VMs the appropriate File boot entry is created automatically if missing.

[![Boot Order Error File Entry missing](/images/BootOrderError.png)](/images/BootOrderError.png)

For Linux VMs (or Ubuntu at least, I can't tell about other distributions but assume it is the same) this is not the case. Fortunately solving the issue is pretty straight forward using Ubuntu rescue mode:

  1. Boot into a rescue system
  2. Select _Force GRUB installation to EFI removable media path_
  3. Reboot

Forcing GRUB installation to EFI removable media path does basically the same thing as when Ubuntu installer asks you if you want to force UEFI installation: it installs to the removable media path in the ESP (EFI System Partition). This is fine for environment where no other operating system is present. However if there is another operating system present on the device which depends on this fallback location "removable media path" it will make this system temporary unbootable (you can manually configure GRUB later to boot it if necessary though).

Windows installer for example _also_ installs to the removable media path in the ESP. All OS installers installing things to this removable media path will conflict with any other such installers and that's why in Debian (and Ubuntu) installers don't do this by default. You explicitly have to select UEFI mode during the normal installation (what I did).

[![Force GRUB Installation to the EFI removable media path](/images/ForceGrubInstallation-1024x874.png)](/images/ForceGrubInstallation.png)

Forcing GRUB installation to EFI removable media path will re-create the _shimx64.efi _File boot entry for your virtual machine. Next time instead of just backing up the vhdx files, maybe an export of the VM's might be the better option.

[![Boot Order Error File Entry restored](/images/BootOrderFile.png)](/images/BootOrderFile.png)
