---
title:      Enable SSH Huawei Quidway
date:       2015-01-04
summary:    Enable SSH on Huawei Quidway switch
category:   Huawei
---

Secure Shell (SSH) is a protocol which provides a secure remote access connection to network devices. This post discusses how to configure SSH on Huawei Quidway. For short story, Huawei have switches and routers products and those products called as Huawei Quidway series.

## Login to Switch or Router

First, login to switch or router first, usually we login using COMM port, set the Bits per second 9600, data bits to 8, parity none, stop bits 1, and flow controll none. Then you will login as user mode, showed as:

```bash
<Quidway>
```

## Create RSA Local Public Key Pair

Then go to system view, and create RSA local public key pair first (you may skipp this if your switch or router RSA local public key pair has been created before).

```bash
<Quidway> system-view
 Enter system view, return user view with Ctrl+Z.
[Quidway]rsa local-key-pair create
 The key name will be: Quidway_Host
 The range of public key size is (512 ~ 2048).
 NOTES: If the key modulus is greater than 512,
 It will take a few minutes.
 Input the bits in the modulus[default = 512]:1024
 Generating keys...
 ......................................++++++
 .....++++++
 ..++++++++
 .........++++++++
[Quidway]
```

## Create Username and Password for SSH

Then create a username and password for SSH authentication:

```bash
[Quidway]aaa
[Quidway-aaa]local-user root password cipher YourPassword
[Quidway-aaa]local-user root level 15
[Quidway-aaa]local-user root service-type ssh
[Quidway-aaa]quit
[Quidway]
 [Quidway]display current-configuration configuration aaa
 #
 aaa
  local-user root password cipher XXXXXXXXX
  local-user root service-type terminal ssh
  authentication-scheme default
  #
  authorization-scheme default
  #
  accounting-scheme default
  #
  domain default
  #
 #
[Quidway]
```

## Configure SSH Server

Then configure the SSH server:

```bash
[Quidway]stelnet server enable
Info:Start STELNET server
[Quidway]ssh user root authentication-type password
Info:A new ssh user added
[Quidway]ssh user root service-type stelnet
[Quidway]
```

## Configure Virtual User Terminal

And the last, configure virtual user terminal interface:

```bash
[Quidway]user-interface vty 0 4
[Quidway-ui-vty0-4]authentication-mode aaa
[Quidway-ui-vty0-4]protocol inbound ssh
[Quidway-ui-vty0-4]
```

And done. You can try to SSH your Huawei Quidway switch or router. Also don't forget to save your configuration.
More about SSH configuration on Quidway S5300 visit [Huawei support page Overview of Authentication Modes and User Privilege Levels](https://support.huawei.com/hedex/hdx.do?lib=DOC1100750696AEJ1221U&docid=DOC1100750696&lang=en&v=02&tocLib=DOC1100750696AEJ1221U&tocV=02&id=EN-US_CONCEPT_0177100107&tocURL=resources%2525252Fdc%2525252Fdc_cfg_login_1004.html&p=t&fe=1&ui=3&keyword=User%25252Bprivilege%25252Blevel).
