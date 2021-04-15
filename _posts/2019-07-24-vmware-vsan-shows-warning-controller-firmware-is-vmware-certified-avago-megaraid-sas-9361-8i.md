---
title:      VMWare VSAN Shows Warning Controller Firmware is VMWare certified Avago MegaRAID SAS 9361-8i
date:       2019-07-24
summary:    VMWare VSAN Shows Warning Controller Firmware is VMWare certified on Avago MegaRAID SAS 9361-8i RAID Card
category:   VMWare
---

I was in a POC which need to show it performance, feature, and especially compatibility with VMWare VSAN. On testing I found an alarm shows warning on item Avago (LSI) MegaRAID SAS Invader Controller `Controller firmware is VMWare certified`.

{% include image.html url="/images/2019-07-24-vmware-vsan-shows-warning-controller-firmware-is-vmware-certified-avago-megaraid-sas-9361-8i/warning-controller-firmware-not-certified.png" description="Warning controller firmware not certified" %}

I think it will not only works on Huawei Servers, but generaly it applied on any servers with Avago MegaRAID SAS 9361-8i RAID Card.

Basically you need to enable JBOD mode and then install the driver of Avago MegaRAID SAS 9361-8i RAID Card which can be downloaded on [VMWare portal](https://my.vmware.com/web/vmware/downloads/details?downloadGroup=DT-ESXI65-AVAGO-LSI-MR3-69121200-1OEM&productId=614). Install with command:

```bash
esxcli software vib install –v /tmp/lsi-mr3-6.912.12.00-1OEM.650.0.0.4240417.x86_64.vib-no-sig-check
```

Also you need to install storcli driver on [Broadcom page](https://www.broadcom.com/support/download-search?pf=RAID+Controller+Cards). Install with command:

```bash
esxcli software vib install –v /tmp/vmware-storcli-007.0409.0000.0000.vib -no-sig-check
```

{% include image.html url="/images/2019-07-24-vmware-vsan-shows-warning-controller-firmware-is-vmware-certified-avago-megaraid-sas-9361-8i/result-after-fix-the-warning-controller-firmware-not-certified.png" description="Result after fix warning controller firmware not certified" %}

The detail operation documented on Huawei Support website. Read on [support.huawei.com](https://support.huawei.com/carrier/docview!docview?nid=KB1000638785).
