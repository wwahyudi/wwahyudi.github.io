---
title:      Why does YaST now show lines as lqqqqqqq?
date:       2015-11-05
summary:    YaST show line as lqqqqqqqqqq
category:   Linux
---

So before, I've installed new fresh SuSE 11 on my laptop as a VM. Then when I was ssh it through putty, it showed lines as `lqqqqqqqqqqqqqqq` and so on, like this:

{% include image.html url="/images/2015-11-05-yast-show-line-as-lqqqqqqqqqqq/yast-show-ncurses-lines-as-lqqqqqqqqqqqqqqq.png" description="YaST show ncurses lines as lqqqqqqqqqqqqqqq" %}

It was difficult to read, and also bit annoyed me. So I googled and found on [superuser.com](http://superuser.com/questions/735269/why-does-yast-now-show-lines-as-lqqqqqqqqqqqqqqq), and they suggest to set to add this command:

```bash
export NCURSES_NO_UTF8_ACS=1
```

## Make it Permanent

That works, but when I reopen a new window for it it back to showed `lqqqqqqqqqqqqqqq` again, so to make it permanent you can do this:

- If you are as a user

```bash
echo "export NCURSES_NO_UTF8_ACS=1" >> ~/.bashrc
```

- And if you are root

```bash
echo "export NCURSES_NO_UTF8_ACS=1" >> /etc/bash.bashrc.local
```

and finally it show `lqqqqqqqqqqqqqqq` no more.
