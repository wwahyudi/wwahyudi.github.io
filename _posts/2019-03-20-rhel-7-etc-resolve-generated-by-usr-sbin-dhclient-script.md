---
title:      RHEL 7 file /etc/resolv.conf generated by /usr/sbin/dhclient-script
date:       2019-05-21
summary:    RHEL 7 file /etc/resolv.conf generated by /usr/sbin/dhclient-script and each time reboot the configuration gone
category: 	Linux
---

For manual configuration on RHEL 7 DNS address on `/etc/resolv.conf`, the configuration always reverts back after reboot because dhclient script controlling it, and it's having label `';generated by /usr/sbin/dhclient-script'`.

{% include image.html url="/images/2019-03-20-rhel-7-etc-resolve-generated-by-usr-sbin-dhclient-script/etcresolvconf-is-having-generated-by-usr-sbin-dhclient-script.png" description="/etc/resolv.conf is having generated by usr sbin dhclient-script" %}

To solve this issue, edit file `/etc/dhclient-enter-hooks`

```bash
vi /etc/dhclient-enter-hooks
```

And append code

```bash
make_resolv_conf(){
    :
}
```

Save and close the file.
