---
title:      Multipath Configuration for Huawei OceanStor & Dorado Series Storage
date:       2021-03-21
summary:    Configure multipath on Huawei Storage (OceanStor or Dorado Series) on RHEL
category:   Linux
---

To configure multipath on RedHat Enterprise Linux (RHEL) 7.x. I recommend you to use this configuration on `/etc/multipath.conf`:

```bash
devices {
         device {
                   vendor                  "HUAWEI"
                   product                 "XSG1"
                   path_grouping_policy    multibus
                   path_checker            tur
                   prio                    const
                   path_selector           "service-time 0"
                   failback                immediate
                   no_path_retry           15
        }
}
defaults {
        user_friendly_names yes
        find_multipaths yes
}
```

And then you can do reload service of multipathd with command:

```bash
systemctl reload multipathd
```

This configuration will define huawei storage, both OceanStor and Dorado series, on multipathd. As note, this configuration will activate ALUA. [Read more about Huawei ALUA](https://support.huawei.com/enterprise/en/doc/EDOC1000162776/3c09c8ef/alua-working-principles). By default Huawei Storage will set ALUA enabled.

And to detect new LUNs with command:

```bash
/usr/bin/rescan-scsi-bus.sh
```

And there is a [suggestion from RedHat about multipath configuration on Huawei Storage](https://access.redhat.com/solutions/3544911) as follows:

* Till RHEL 7.4 the default multipath configuration is only for "NON-ALUA" mode of HUAWEI XSG1 Storage. Hence misses ALUA configuration.
* In RHEL 7.5 the default multipath is only for "ALUA" mode of HUAWEI XSG1 Storage. Hence misses NON-ALUA configuration.
