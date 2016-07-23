---
layout:     post
title:      Copy Big File from Remote Desktop
date:       2015-11-05
summary:    How to copy file bigger than 2GB on remote desktop
categories: personal
---

Sometimes we need copy big files (more or equal from 2GB), and on the middle of copying it cancelled accidentally. There 2 methods we can use for it:

### Method 1
Use Drive Redirection through Remote Desktop Services or a Terminal Services session if you want to transfer files larger than 2 GB.

### Method 2
Use command-line alternatives such as XCopy to copy files larger than 2 GB over a Remote Desktop Services or Terminal Services session. For example, you can use the following command:
{% highlight powershell %}
xcopy \\tsclient\c\myfiles\LargeFile d:\temp
{% endhighlight %}

Here is the help [source](https://support.microsoft.com/en-us/kb/2258090).
