---
title:      My Lazy Way to Configure SSH Client for Hyperscaler Setup
date:       2023-07-03
summary:    My lazy way to deal with a lot of servers
category:   Linux Personal
---

As someone who SSH to servers, I am quite bothered with accepting SSH fingerprints for my local production servers. Anyway, automatically accepting the SSH fingerprint effectively by-passes the security put in place by SSH. You should be careful using this, especially on untrusted networks, including the public internet.

To automatically accept the SSH servers fingerprint and add it to the known hosts file we can pass the `StrictHostKeyChecking no` option to SSH.

Other than this, I also bothered with known host warnings. I handled a lot of servers that reinstalled a lot of times, so known host key will be changed as well. The way I like to do to handle this kind of thing is to not save the known host keys, and directly throw it way to `/dev/null`. Also, I set the log level to ERROR only so there will be no warning related to known host keys.

Again, these methods not really recommended. It is against security guidelines. 

Myself, I like to configure this at `~/.ssh/config`. The usual way I define it will be:

```
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
  LogLevel ERROR
```

`StrictHostKeyChecking no` will be automatically accepting fingerprint.
`UserKnownHostsFile /dev/null` will be save known host keys to `/dev/null`. I like this way because I reinstalled a lot of servers, and known host keys will changes over times.
`LogLevel ERROR` will be shown errors only on error level, I not really like warnings.

All of these are personal preference, and it may not suit for your enviroment, and again, these settings are not recommended and may against security guidelines.
