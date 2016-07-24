---
layout:     post
title:      Huawei Quidway User Privilege Level
date:       2015-01-07
summary:    User privilege level on Huawei Quidway switch
categories: huawei
---

You need to set a privilege level when you create a `local-user` on `aaa`. And here are the privilege you can set to your `local-user`:

| User Level | Command Level  | Level Name          | Description                                                                                                                                                                                                                                                                                                                                                                           |
|------------|----------------|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 0          | 0              | Visit level         | This level gives access to commands that run network diagnostic tools (such as ping and tracert) and commands that start from a local device and visit external devices (such as Telnet client side).                                                                                                                                                                                 |
| 1          | 0 and 1        | Monitoring level    | This level gives access to commands, like the display command, that are used for system maintenance and fault diagnosis.                                                                                                                                                                                                                                                              |
| 2          | 0, 1, and 2    | Configuration level | This level gives access to commands that configure network services provided directly to users, including routing and network layer commands                                                                                                                                                                                                                                          |
| 3-15       | 0, 1, 2, and 3 | Management level    | This level gives access to commands that control basic system operations and provide support for services. These commands include file system commands, FTPcommands, TFTP commands, configuration file switching commands, power supply control commands, backup board control commands, user managementcommands, level setting commands, and debugging commands for fault diagnosis. |