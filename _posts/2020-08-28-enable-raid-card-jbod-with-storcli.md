---
layout:     post
title:      Enable RAID Card JBOD with storcli 
date:       2020-08-28
summary:    How to enable JBOD from LSI RAID Card with storcli command line tools
categories: linux
---

JBOD, which stands for Just a Bunch of Disks or Just a Bunch of Drives, is a storage architecture consisting of numerous disk drives inside of a single storage enclosure.
JBOD enclosures are usually not configured to act as a RAID, but they can be.
By their very nature, JBOD enclosures are storage capacity monoliths.

This command for enable JBOD from LSI RAID Card with storcli command line tools.
```bash
storcli64 /c0 set jbod=on
```
Where /c0 is location of controller number

Example:
Before execution, we can notice that disks with state UGood (Unconfigure Good):
```bash
PD LIST :
=======

-------------------------------------------------------------------------
EID:Slt DID State DG       Size Intf Med SED PI SeSz Model            Sp
-------------------------------------------------------------------------
24:0     10 Onln   0 557.861 GB SAS  HDD N   N  512B HUC156060CSS200  U
24:1      0 Onln   0 557.861 GB SAS  HDD N   N  512B HUC156060CSS200  U
24:2     12 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:3      6 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:4     13 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:5      2 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:6     21 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:7     16 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:8     20 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:9     18 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:10     3 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:11    15 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:12    19 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:13    11 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:14     5 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:15     7 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:16     1 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:17    25 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:18    23 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:19     4 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:20    17 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:21    22 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:22     9 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:23     8 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:24    26 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:25    14 UGood  -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
-------------------------------------------------------------------------
```

Execute command to enable JBOD mode:
```bash
[root@localhost storcli]# ./storcli64 /c0 set jbod=on
Controller = 0
Status = Success
Description = None


Controller Properties :
=====================

----------------
Ctrl_Prop Value
----------------
JBOD      ON
----------------

PD LIST :
=======

-------------------------------------------------------------------------
EID:Slt DID State DG       Size Intf Med SED PI SeSz Model            Sp
-------------------------------------------------------------------------
24:0     10 Onln   0 557.861 GB SAS  HDD N   N  512B HUC156060CSS200  U
24:1      0 Onln   0 557.861 GB SAS  HDD N   N  512B HUC156060CSS200  U
24:2     12 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:3      6 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:4     13 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:5      2 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:6     21 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:7     16 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:8     20 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:9     18 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:10     3 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:11    15 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:12    19 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:13    11 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:14     5 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:15     7 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:16     1 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:17    25 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:18    23 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:19     4 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:20    17 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:21    22 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:22     9 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:23     8 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:24    26 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
24:25    14 JBOD   -   1.635 TB SAS  HDD N   N  512B HUC101818CS4200  U
-------------------------------------------------------------------------
[root@localhost storcli]
```