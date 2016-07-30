---
layout:     post
title:      Please wait for the System Event Notification Service
date:       2015-01-15
summary:    Windows server shows "Please wait for the System Event Notification Service"
categories: windows
---

This afternoon, when I restarted Windows Server 2008 R2 via remote desktop, I got a message “Please wait for the System Event Notification Service…” and the loading was too long.

System Event Notification Service (SENS) ia a service which provides automatic distribution of events to subscribing Component Object Model (COM) components. In the `services.msc` it listed as `COM+ Event System`.

![desk](http://sapikuda.com/images/posts/2015-01-15-please-wait-for-the-system-event-notification-service/Please-wait-for-the-System-Event-Notification-Service-Ebuggi.png)

I got confused how as soon as Windows Server is rebooted and running normally as usual. After Googling and searching for solutions, it turns out the way is as follows:

1. Remote Desktop another account on server.
2. Open cmd, right-click Run as Administrator.
3. Type `sc queryex SENS` to know the PID of SENS.
4. When you already know the PID of SENS, type `taskkill /PID xxxx /F` where xxxx is the PID of the SENS.

OK and finish then reboot the server.

Update:

You may disable System Event Notification Service if you want. SENS is one of the support files that you'll probably never have any use for, but if you disable it, the warning notices you receive are worse than leaving it enabled. To disable it just go to `services.msc`, look for `COM+ Event System`, then just change from `Automatic` to `Manual`, and done.