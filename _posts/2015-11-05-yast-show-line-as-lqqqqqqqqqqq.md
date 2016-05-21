---
layout:     post
title:      Why does YaST now show lines as lqqqqqqqqqqqqqqq?
date:       2015-11-05
summary:    YaST show line as lqqqqqqqqqqqqqqq
categories: suse
---

So before, I've installed new fresh SuSE 11 on my laptop as a VM. Then when I was ssh it through putty, it showed lines as lqqqqqqqqqqqqqqq and so on, like this:
![desk](https://7jddba.dm2301.livefilestore.com/y3pgsBe0YSnER05JOQOmV3k-znyzXh-L3KPW0N2SZyyQRiIDT82sgtNS4UmKQm8w6U_1yPlH5QmjlNtCBt1fxmfWWZIVfOhL29msrdBsKqahg4SBtZ3Yhzhsyyzkv_oZeEd6uEkmHQMsIPXcQ5I63cOQUtQSs0k3G9Yrkv00lf-gnE/why-does-yast-now-show-lines-as-lqqqqqqqqqqqqqqq-1.png)

It was difficult to read, and also bit annoyed me. So I googled and found on [superuser.com](http://superuser.com/questions/735269/why-does-yast-now-show-lines-as-lqqqqqqqqqqqqqqq), and they suggest to set to add this command:
{% highlight sh %}
export NCURSES_NO_UTF8_ACS=1
{% endhighlight %}

# Make it Permanent 
That works, but when I reopen a new window for it it back to showed lqqqqqqqqqqqqqqq again, so to make it permanent you can do like this:

- If you are as a user
{% highlight sh %}
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
{% endhighlight %}

- And if you are root
{% highlight sh %}
echo "export NCURSES_NO_UTF8_ACS=1" >> /etc/bash.bashrc.local
{% endhighlight %}

and finally it show lqqqqqqqqqqqqqqq no more.