---
layout:     post
title:      Create a Bootable USB Installer Windows
date:       2013-01-19
summary:    Creating a Bootable USB Installer Windows 7 and Windows 8 without 3rd Party Tools 
categories: windows
---

USB flash drive is also often used to install Windows. In fact, installing Windows via USB proven faster than using DVDs. Bootable USB can be created without 3rd party software, it’s a build-in feature, just only with Windows command prompt. Not only Windows 7 and Windows 8 that could be made using this method, Windows server 2008 and Windows Server 2012 can be. Here's how to make a USB Bootable Windows 7 and Windows 8 without Tools:

1.	The first step that you need to do, of course, is to plug a USB stick into your port.

2.	Next, go to your command prompt by typing 'cmd' from 'Start Menu'. Right-click and select 'Run as administrator'. Remember to be 'Run as administrator' because if cmd not be run as administrator, it may has some commands that cannot be executed. This step is used if the USB drive you've never used as a bootable USB installer. If you've ever used a USB flash drive as a USB installer you may directly go to step 6 to record reformat your USB flash drive.
![open cmd](https://bzjr3a-ch3302.files.1drv.com/y3p7VJD0jSg4_bqf5REY_GUGTbJypAc-Rnb2yd8ElZiT3gQn5EOdDhz1QpS87tBX9CMHRxyLOg2MBg-9KK_R2byqIwiT8t-Pa_m_E392dn9gVQBPaBNW6aozeEkehTRB2U6/1.png)

3.	Type 'diskpart' command, the prompt will change to 'DISKPART>'
![diskpart](https://bzjr3a-ch3301.files.1drv.com/y3pjGjibUyjWvrhsmEVc2lyd3eAz7aWqDEj-X7ctW1D5To9AFNqb42XFnSsWd8bwARFTiLURBg4x-Ux2Dqvs1RcQFD82_kWAi21BDTXhgvlF0radJpVIQ2MZIfWodadfFHd/2.png)

4.	Next type 'list disk' to display disks available in our computers now.
![list disk](https://bzjr3a-ch3302.files.1drv.com/y3pioaBkWDdigkrJGwgqB5V3gKsjAgQXgxF5RVkkla_a8ekdywJkgQVZaU0w58s__xYqBT5gl2h5qGNgnC0BTmm55_8rmL6FREEgJoZmbMaSGbp1HEGNzv137rNJ8Kvf3T0/3.png)

5.	Clear USB first, also backup the existing data in the USB if available. Then format the USB. Type the following command below:

{% highlight bash %}
select disk 2
{% endhighlight %}
![select disk 2](https://bzjr3a.dm2301.livefilestore.com/y2pJo4nrTWzv726iPlny36Qpd-UNCsdPjpvhT8bWOfcpadlLIKRno2lLAJxCwKkujkinBc_91sCPl3tQMg4k3YlAahp4ywSPl2TvfSX6w_fmDo/4.png)
Number 2 is for USB disk placed.

{% highlight bash %}
clean
{% endhighlight %}
![clean](https://bzjr3a-ch3302.files.1drv.com/y3pawiBSzhDKsvbOOTzym0F6tWZ6mu1LBpmtnNhNWK8h2Ug41MPLwKQsB5K5zDBCSBY1112TGJN-kitx2wHVV8bPEPZ8qv1hpAJVxzoaOpLKc_G_6TRokGhXv21-I7-77hU/5.png)

{% highlight bash %}
create partition primary
{% endhighlight %}
![create partition primary](https://bzjr3a.by3302.livefilestore.com/y3pJQB6vKQGxuKcYz1XM_TjaMauwUCp3PyBii_iZMbYkM9Nlcr3B1Wpbcvjrqz72IZjiag_gzGzlxrPomNCmT8OEApn7VI5YXRNlxxnJDnckzOD3kOG-jg89obL173T10se/6.png)

{% highlight bash %}
select partition 1
{% endhighlight %}
![select partition 1](https://bzjr3a-ch3302.files.1drv.com/y3ptnFSmpyT6lFlcevMAmOGVZc7hciCpf61_tdR2mgBltCfaMslxuU62Qxh7qBNonENGkhNzF-tadOkPj1ndCUMOCLy8rHZ1JrCZ-H9SzeWeMVXQIgimOyDHUnnP1erQxXL/7.png)

{% highlight bash %}
active
{% endhighlight %}
![active](https://bzjr3a-ch3302.files.1drv.com/y3pWGxMXqffDUyLHguSNB37xmvtw2QDmUoqAn80ffiGf_Z0MHqUrXfFgAzOLs97vOPCreZWJTnGH2Lj7N46hl_JnwwPJePzVuODddetJ-RUFasXOpbcIADn7L8RCzwN3tOn/8.png)

{% highlight bash %}
format fs=NTFS
{% endhighlight %}
![format fs=NTFS](https://bzjr3a-ch3302.files.1drv.com/y3pVnuKNBWc34PA_2A_Gvy_W2RKERaCo_f_m6spJR_batP8NB88RgsgMdh-xFfaadn2RWJOWAqy_-jNyh3T4QO5VI_N4hmd94yyEdEqNvAblHX9UsUrqSw8LXagsQHq8Apr/9.png)

{% highlight bash %}
assign
{% endhighlight %}
![assign](https://bzjr3a-ch3301.files.1drv.com/y3p8gZu0EOFgaMwJ-jgPab1q_eZqr8NUfft5_HWh3RbSRYBkaKcRyZlGIpR3er04QmL6z90q06imDg0tJzORDdQfa8DXFT-2JGHeC5G1s9iuJwauD17rHKr2BMOkPewcS69/10.png)

{% highlight bash %}
exit
{% endhighlight %}
![exit](https://bzjr3a-ch3301.files.1drv.com/y3pKcPLGj9rUFa88ZsNrJq0E54gQNGF7QRFcAHrO-I6MZmno8W8fMqCgvFXUDOSyg5Y7li48YUtcr9x4UhBJg27SlBFMzYaCoSFMhOYWhiUrijk9vMnS61rp2HWwlOw12UW/11.png)

6.	After successfully at format, then we will make our USB bootable using utility assistance 'bootsect' which is found in Windows Vista and Windows 7 (also in Windows 8). Insert the Windows 7 DVD (or you may mount ISO of Windows 7), then change to the directory where the DVD is located, with the command:
{% highlight bash %}
h:
cd boot
{% endhighlight %}
![cd boot](https://bzjr3a-ch3301.files.1drv.com/y3pVRO65joA0H36z4WIlumUw9E9OSEqab4mJEcll_KdThyXWqd-G3MGbZdipjlgo4Gzic4sl4_tliF3VUdJMt5APvhnJUVmy7eHWiomS9urWWIs2HdBrCERdy9gjH1rZ41S/12.png)

where h: is my Windows 7 DVD drive letter
7.	Use bootsect to make us into a bootable USB. The trick is to type the command:
{% highlight bash %}
bootsect /nt60 n:
{% endhighlight %}
![open cmd](https://bzjr3a-ch3301.files.1drv.com/y3pOH5p-LOm6Myb02xfOiIxCQDjeraEONkeXIb9lIlgr4KfP2lOlxIZ-IX5UOdypZl4140tVBS_9EIswyt2-bs-xuvdgLyWk2GadiempV8OXeLh9W4ZAYLCGkl0tsAIhifX/13.png)
where n: is my USB drive letter

8.	Now you can close your windows command prompt. Copy the one all the installation files in your DVD drive into your flash. If you have finished, you have been created bootable USB Windows installer!