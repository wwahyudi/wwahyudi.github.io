---
layout:     post
title:      Showing in Real-time 10 Processes which Uses Most CPU Resources
date:       2015-11-05
summary:    Showing 10 processes that use resources most cpu
categories: linux
---

How to know processes are consuming the most CPU resources on Ubuntu system? (and this also works on another Linux distro).
By knowing the process of consuming too many CPU resources, you can minimize or kill unneeded programs on the system. So, It would be helpful for you to give stability for the system that you are using:
{% highlight bash %}
# watch -n 1 'ps aux | sort -nrk 4 | head'
{% endhighlight %}
or just simply use:
{% highlight bash %}
# top
{% endhighlight %}
The command will showing 10 processes that use resources most cpu every 1 second.