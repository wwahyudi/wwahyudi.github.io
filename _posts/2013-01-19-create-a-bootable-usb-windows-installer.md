---
title:      Create a Bootable USB Installer Windows
date:       2013-01-19
summary:    Creating a Bootable USB Installer Windows 7, Windows 8, and Windows 10 without 3rd Party Tools
category:   Windows
---

USB flash drive is also often used to install Windows. In fact, installing Windows via USB proven faster than using DVDs. Bootable USB can be created without 3rd party software, it’s a build-in feature, just only with Windows command prompt. Not only Windows 7 and Windows 8 that could be made using this method, Windows server 2008 and Windows Server 2012 can be.

#### Update

It also works for Windows 10, Windows Server 2016, and Windows Server 2019.

Here's how to make a USB Bootable Windows 7 and Windows 8 without Tools:

## Format the USB Flash Drive

The first step that you need to do, of course, is to plug a USB stick into your port.

Next, go to your command prompt by typing `cmd` from `Start Menu`. Right-click and select `Run as administrator`. Remember to be run as administrator because if cmd not be run as administrator, some commands  may cannot be executed. **This step is used if the USB drive you've never used as a bootable USB installer. If you've ever used a USB flash drive as a USB installer you may directly go to step 6 to record reformat your USB flash drive.**

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/1.png" description="Open cmd" %}

Type `diskpart` command, the prompt will change to `DISKPART>`

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/2.png" description="diskpart" %}

Next type `list disk` to display disks available in our computers now.

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/3.png" description="list disk" %}

Clear USB first, also backup the existing data in the USB if available. Then format the USB. Type the following command below:

```powershell
select disk 2
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/4.png" description="select disk 2" %}

Number 2 is for USB disk placed.

```powershell
clean
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/5.png" description="clean" %}

```powershell
create partition primary
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/6.png" description="create partition primary" %}

```powershell
select partition 1
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/7.png" description="select partition 1" %}

```powershell
active
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/8.png" description="active" %}

```powershell
format fs=NTFS quick
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/9.png" description="format fs=NTFS quick" %}

```powershell
assign
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/10.png" description="assign" %}

```powershell
exit
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/11.png" description="exit" %}

## Make USB Flash Drive Bootable

After successfully at format, then we will make our USB bootable using utility assistance 'bootsect' which is found in Windows Vista and Windows 7 (also in Windows 8). Insert the Windows 7 DVD (or you may mount ISO of Windows 7), then change to the directory where the DVD is located, with the command:

```powershell
h:
cd boot
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/12.png" description="cd boot" %}

where h: is my Windows 7 DVD drive letter

Use bootsect to make us into a bootable USB. The trick is to type the command:

```powershell
bootsect /nt60 n:
```

{% include image.html url="/images/2013-01-19-create-a-bootable-usb-windows-installer/13.png" description="bootsect /nt60 n:" %}

where n: is my USB drive letter

Now you can close your windows command prompt. Copy the one all the installation files in your DVD drive into your flash. If you have finished, you have been created bootable USB Windows installer!
