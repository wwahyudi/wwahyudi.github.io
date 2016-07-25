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

![Just picture guide](https://7dcogw.dm2301.livefilestore.com/y3pRjyR1k5DUyI3X-DIyn6InyTxppgonZILs7VLjPpzvT6QjddOt9FXTtYljC3vXQ1mNKrvae0rvgG0zrG79yQNVI8B_P7ZJCSG3FCO7TG-_hvt6oIMFwqH-iJLStR0qjlgH5b2NFvvigJ9fYbRgGYTMZQL04wPWb1sQqN51zHABUw/2016-07-25_190200.png)

And done.
