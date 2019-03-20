---
layout:     post
title:      Enable UEFI in VMWare Workstation
date:       2019-03-20
summary:    Enable UEFI in VMWare Workstation
categories: VMWare Workstation
---

1. Create a new virtual machine
2. Open the .VMX file of your virtual machine in Notepad (or similar) and add the following line to it:
{% highlight %}
firmware="efi"
{% endhighlight %}
3. Save .VMX file