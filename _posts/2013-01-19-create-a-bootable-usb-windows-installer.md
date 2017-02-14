---
layout:     post
title:      Create a Bootable USB Installer Windows
date:       2013-01-19
summary:    Creating a Bootable USB Installer Windows 7 and Windows 8 without 3rd Party Tools
categories: windows
---

USB flash drive is also often used to install Windows. In fact, installing Windows via USB proven faster than using DVDs. Bootable USB can be created without 3rd party software, it’s a build-in feature, just only with Windows command prompt. Not only Windows 7 and Windows 8 that could be made using this method, Windows server 2008 and Windows Server 2012 can be. Here's how to make a USB Bootable Windows 7 and Windows 8 without Tools:

### Format the USB Flash Drive

The first step that you need to do, of course, is to plug a USB stick into your port.

Next, go to your command prompt by typing `cmd` from `Start Menu`. Right-click and select `Run as administrator`. Remember to be run as administrator because if cmd not be run as administrator, some commands  may cannot be executed. **This step is used if the USB drive you've never used as a bootable USB installer. If you've ever used a USB flash drive as a USB installer you may directly go to step 6 to record reformat your USB flash drive.**

![open cmd](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/1.png)

Type `diskpart` command, the prompt will change to `DISKPART>`
![diskpart](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/2.png)

Next type `list disk` to display disks available in our computers now.
![list disk](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/3.png)

Clear USB first, also backup the existing data in the USB if available. Then format the USB. Type the following command below:

{% highlight bash %}
select disk 2
{% endhighlight %}

![select disk 2](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/4.png)

Number 2 is for USB disk placed.

{% highlight bash %}
clean
{% endhighlight %}

![clean](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/5.png)

{% highlight bash %}
create partition primary
{% endhighlight %}

![create partition primary](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/6.png)

{% highlight bash %}
select partition 1
{% endhighlight %}

![select partition 1](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/7.png)

{% highlight bash %}
active
{% endhighlight %}

![active](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/8.png)

{% highlight bash %}
format fs=NTFS quick
{% endhighlight %}

![format fs=NTFS quick](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/9.png)

{% highlight bash %}
assign
{% endhighlight %}

![assign](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/10.png)

{% highlight bash %}
exit
{% endhighlight %}

![exit](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/11.png)

### Make USB Flash Drive Bootable

After successfully at format, then we will make our USB bootable using utility assistance 'bootsect' which is found in Windows Vista and Windows 7 (also in Windows 8). Insert the Windows 7 DVD (or you may mount ISO of Windows 7), then change to the directory where the DVD is located, with the command:
{% highlight bash %}
h:
cd boot
{% endhighlight %}

![cd boot](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/12.png)

where h: is my Windows 7 DVD drive letter

Use bootsect to make us into a bootable USB. The trick is to type the command:
{% highlight bash %}
bootsect /nt60 n:
{% endhighlight %}

![open cmd](http://sapikuda.com/images/posts/2013-01-19-create-a-bootable-usb-windows-installer/13.png)

where n: is my USB drive letter

Now you can close your windows command prompt. Copy the one all the installation files in your DVD drive into your flash. If you have finished, you have been created bootable USB Windows installer!
