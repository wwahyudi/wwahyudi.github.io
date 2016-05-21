---
layout:     post
title:      PowerShell says “Execution of scripts is disabled on this system.”
date:       2016-03-24
summary:    Fix execution policy for powerShell scripts cannot be loaded because the execution of scripts is disabled on this system. 
categories: windows
---
When I am trying to run a powershell scripts before, I got this message:
> script_name.ps1 cannot be loaded because the execution of scripts is disabled on this system.

If you’re using Windows 2008 R2 then there is an x64 and x86 version of PowerShell both of which have to have their execution policies set. You can set the execution policy by typing this into your PowerShell window:
{% highlight bash %}
Set-ExecutionPolicy RemoteSigned
{% endhighlight %}

After that you can run your powershell scripts smoothly.

For more information see on [Microsoft TechNet post](https://technet.microsoft.com/en-us/library/ee176961.aspx).


