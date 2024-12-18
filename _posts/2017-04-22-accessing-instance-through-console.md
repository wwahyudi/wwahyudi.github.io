---
title:      Accessing an Instance through Console
date:       2017-04-22
summary:   	Accessing an Instance through Nova VNC Console
category:   Openstack
---

So I was worked for some Openstack which is branded as FusionSphere Openstack 6.0. It's an Openstack that made by Huawei. On this environment Openstack version is Juno (Nova version `2014.2`). My objective on this post is accessing an instance via console. To get to console of an instance in openstack, it will use VNC. Actually there are 3 methods commonly used in Openstack:

- novnc: An in-browser VNC client implemented using HTML5 Canvas and WebSockets
- spice: A complete in-browser client solution for interaction with virtualized instances
- xvpvnc: A Java client offering console access to an instance

In FusionSphere Openstack 6.0. The default VNC console is novnc.

To access an instance through a remote console, run the following command:

```bash
nova get-vnc-console $vm-id novnc
```

The command returns a URL from which you can access your instance:

```bash
$ nova get-vnc-console 95b9c0bf-22d2-43cb-b241-4a964f988f57 novnc
+-------+----------------------------------------------------------------------------------------------------------+ 
| Type  | Url                                                                                                      | 
+-------+----------------------------------------------------------------------------------------------------------+ 
| novnc | https://nova-novncproxy.domain.com:8002/vnc_auto.html?token=52b7e33a-1688-49da-b63f-80021fc8dd31&lang=EN | 
+-------+----------------------------------------------------------------------------------------------------------+ 
```
