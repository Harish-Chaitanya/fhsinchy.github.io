---
title: "How To Boot Arch Linux in UEFI Mode on VirtualBox"
date: 2022-01-25T00:00:00-00:00
categories:
  - tutorial
tags:
  - linux
  - virtualbox
---

After publishing [The Arch Linux Handbook](https://www.freecodecamp.org/news/how-to-install-arch-linux/) on [freeCodeCamp](https://www.freecodecamp.org/news/author/farhanhasin/), a number of readers have reached out to me regarding a common issue while trying to boot Arch Linux in UEFI mode on VirtualBox. That is, instead of booting into the regular Arch Linux boot menu which looks as follows:

![Arch Linux Boot Menu](/assets/images/2022-01-25-how-to-boot-arch-linux-in-uefi-mode-on-virtualbox/VirtualBox_archlinux-2022.01.01-x86_64_12_01_2022_18_39_29.png)

The virtual machine boots into the following menu:

![EFI Shell Menu](/assets/images/2022-01-25-how-to-boot-arch-linux-in-uefi-mode-on-virtualbox/VirtualBox_archlinux-2022.01.01-x86_64.iso_25_01_2022_13_43_44.png)

Now if you choose the __EFI Shell__ option, you'll land into the UEFI interactive shell:

![UEFI Interactive Shell](/assets/images/2022-01-25-how-to-boot-arch-linux-in-uefi-mode-on-virtualbox/VirtualBox_archlinux-2022.01.01-x86_64.iso_25_01_2022_13_46_37.png)

This is nothing exclusive to Arch Linux though. I've seen similar behavior with Debian in the past. To be honest, I don't have a clear explanation about why this happens but I know how you can execute the boot file manually.

To do so, make sure you're on the UEFI Interactive Shell as show in the above screenshot. The shell should show you the mapping table of all the storage devices of your virtual machine. You can clear the screen by executing the `cls` command and reprint the mapping table by executing the `map` command. Now, as you can see there are four storage devices connected to my virtual machine.

> Fun Fact: you can change the shell screen color by executing the `cls <color_code>` command, i.e `cls 0` where 0 is Black, 1 is Blue, 2 is Green, 3 is Cyan, 4 is Red, 5 is Magenta, 6 is Yellow, and 7 is Light Gray.

Among these four devices, `FS1` is the CDROM. Now considering I've attached the Arch Linux ISO file as a CDROM to this virtual machine, the boot file should be in that device. To change the working directory to the `FS1` device, execute the following command:

```bash
FS1:
```

And the prompt will immediately change from `Shell>` to `FS1:\>` indicating that you've succesfully changed the working directory. Now execute the `ls` command to get a list of the device's contents:

![FS1 Device Content](/assets/images/2022-01-25-how-to-boot-arch-linux-in-uefi-mode-on-virtualbox/VirtualBox_archlinux-2022.01.01-x86_64.iso_25_01_2022_14_07_04.png)

The boot file is usually kept inside teh `/efi/boot` directory. So `cd` into the `EFI` directory and use the `ls` command to see it's content:

![EFI Directory Content](/assets/images/2022-01-25-how-to-boot-arch-linux-in-uefi-mode-on-virtualbox/VirtualBox_archlinux-2022.01.01-x86_64.iso_25_01_2022_14_08_56.png)

Yup, there is a `BOOT` directory. Now `cd` into that directory and use the `ls` command one last time to see it's content:

![BOOT Directory Content](/assets/images/2022-01-25-how-to-boot-arch-linux-in-uefi-mode-on-virtualbox/VirtualBox_archlinux-2022.01.01-x86_64.iso_25_01_2022_14_10_09.png)

As you can see, there is a `BOOTx64.EFI` file in there and this is what you need. To execute this file, simply write `BOOTx64.EFI` in the shell and hit enter.

![Arch Linux Boot Menu](/assets/images/2022-01-25-how-to-boot-arch-linux-in-uefi-mode-on-virtualbox/VirtualBox_archlinux-2022.01.01-x86_64_12_01_2022_18_39_29.png)

And, voila!
