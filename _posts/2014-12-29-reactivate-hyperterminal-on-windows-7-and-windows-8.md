---
layout:     post
title:      Reactivate HyperTerminal on Windows 7 and Windows 8
date:       2014-12-29
summary:    Makes HyperTerminal run "again" on Windows 7 and Windows 8. HyperTerminal download link available.
categories: windows
---

So I just got a new job earlier this month. Surprisingly, I need to use HyperTerminal. Short story about HyperTerminal, HyperTerminal used to communicates data with serial COM port and also for TCP/OP Winsock. HyperTerminal is a feature from older Windows OS, and the last Window that provide HyperTerminal is Windows XP. HyperTerminal is not available on Windows 7 or newer. But you don't need to downgrade your Windows to use it.

To reactivate HyperTerminal on Windows 7, the easiest way is just copy HyperTerminal files such as __hypertrm.exe__, __hypertrm.dll__, and __hticons.dll__. On Windows XP those files can be found on:

- C:\Program Files\Windows NT\hypertrm.exe
- C:\WINDOWS\system32\hypertrm.dll
- C:\WINDOWS\system32\hticons.dll

Then just copy it on same location or just make a folder somewhere to paste it. And to run it, just simply double click it. Here are result of HyperTerminal run on my Windows 7:

![HyperTerminal on Windows 7](https://bpid8w.dm2301.livefilestore.com/y2pCMLXXQlEeOd5Q3n-3eO7O7kpUi-6qHTM-MbhOhXk0EU3CxMSpbHpLLgqmqO5ZOF4yqV_No3HARTLghHUzYW79CMmleGaLbJqAYNgF2UkeKDeUHSBDFV34oIDbcn32NNj_KU5swKWEw65-oH0I-rGOg/2014-12-29_201738.png)

And if you doesn;t have Windows XP, you also can download it [here](http://1drv.ms/1xtJ0ps).

Update:

You can also use [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) to communicate with COM port.