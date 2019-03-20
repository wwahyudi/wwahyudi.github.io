---
layout:     post
title:      RHEL 7 file /etc/resolv.conf generated by /usr/sbin/dhclient-script
date:       2016-05-21
summary:    RHEL 7 file /etc/resolv.conf generated by /usr/sbin/dhclient-script and each time reboot the configuration gone
categories: personal
---

For manual configuration on RHEL 7 DNS address on `/etc/resolv.conf`, the configuration always reverts back after reboot because dhclient script controlling it, and It's having label `';generated by /usr/sbin/dhclient-script'` 

To solve this issue, edit file `/etc/dhclient-enter-hooks`
```
vi /etc/dhclient-enter-hooks
```

And append code
```
make_resolv_conf(){
	:
}
```

Save and close the file.