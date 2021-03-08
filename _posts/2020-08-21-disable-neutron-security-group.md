---
layout:     post
title:      Disable Neutron Security Group 
date:       2020-08-21
summary:    How to disable neutron security group 
categories: openstack
---

These commands use to disable neutron port which has security group. 

Update to release security group from neutron port or no security group
```bash
neutron port-update {port_id} --no-security-groups
```

Update disable port security
```bash
neutron port-update {port_id} --port-security-enable false
```