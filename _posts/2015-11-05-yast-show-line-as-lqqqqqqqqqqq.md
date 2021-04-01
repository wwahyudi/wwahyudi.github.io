---
layout:     post
title:      Why does YaST now show lines as lqqqqqqq?
date:       2015-11-05
summary:    YaST show line as lqqqqqqqqqq
categories: suse
---

So before, I've installed new fresh SuSE 11 on my laptop as a VM. Then when I was ssh it through putty, it showed lines as lqqqqqqqqqqqqqqq and so on, like this:

![desk](//sapikuda.com/images/posts/2015-11-05-yast-show-line-as-lqqqqqqqqqqq/why-does-yast-now-show-lines-as-lqqqqqqqqqqqqqqq-1.png)

lqqqqqqqqqqqqqqq on ncurses

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