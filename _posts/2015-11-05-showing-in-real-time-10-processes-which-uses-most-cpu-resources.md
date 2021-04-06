---
title:      Showing in Real-time 10 Processes which Uses Most CPU Resources
date:       2015-11-05
summary:    Showing 10 processes that use resources most cpu
category:   Linux
---

How to know processes are consuming the most CPU resources on Ubuntu system? (and this also works on another Linux distro).
By knowing the process of consuming too many CPU resources, you can minimize or kill unneeded programs on the system. So, It would be helpful for you to give stability for the system that you are using:

```bash
# watch -n 1 'ps aux | sort -nrk 4 | head'
```

or just simply use:

```bash
# top
```

The command will showing 10 processes that use resources most cpu every 1 second.

{% include image.html url="/images/2015-11-05-showing-in-real-time-10-processes-which-uses-most-cpu-resources/result-top-command.png" description="Result top command" %}

another way to show real time processes, you can use `htop`. `htop` provide more friendly user interface. It can be execute with command

```bash
htop
```

{% include image.html url="/images/2015-11-05-showing-in-real-time-10-processes-which-uses-most-cpu-resources/result-htop-command.png" description="Result top command" %}
