---
layout:     post
title:      Change DNS in DHCP Server
date:       2016-07-25
summary:   	Change DNS on Windows Server DHCP Server
categories: windows
---

So I just configure DHCP Server on Windows Server 2008 R2, and accidently I configured DNS address incorrectly. So steps to change DNS on DHCP scope are:

1. Open the DHCP Console.
2. Open your Server.
3. If applicable open the IPv4 or IPv6.
4. Open the relevant scope.
5. Click on the Scope Options.
6. Click on the 006 DNS Servers option, right click and select properties.
7. Higlight the unwanted IP address and click remove.
8. Insert the new IP address and click add.
9. Click Apply then OK.

![Just picture guide](http://sapikuda.com/images/posts/2016-07-25-change-dns-in-dhcp-server/2016-07-25_190200.png)

And done.
