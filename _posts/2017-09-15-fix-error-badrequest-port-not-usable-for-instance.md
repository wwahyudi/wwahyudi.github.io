---
layout:     post
title:      Fix ERROR (BadRequest): Port [port-id] not usable for instance [instance-id]
date:       2017-09-19
summary:   	How to fix ERROR (BadRequest): Port [port-id] not usable for instance [instance-id]. (HTTP 400) (Request-ID: req-[request-id])
categories: openstack
---

So I was got an error which was said:

> ERROR (BadRequest): Port c97cbdcf-9aab-4043-a6d7-19d8c69962cd not usable for instance beb834cf-7bde-40d7-8097-92e26777f380. (HTTP 400) (Request-ID: req-35a31098-8fd3-49d1-8625-454ea73e1ecb)

Or the pattern look like this:
> ERROR (BadRequest): Port [port-id] not usable for instance [instance-id]. (HTTP 400) (Request-ID: req-[request-id])

Symptom
-------------------

I was want to attached a port to an instance that have been created before. But then it show that error when execute command `nova interface-attach`

```
HOSTNAME:~ # nova interface-attach --port-id c97cbdcf-9aab-4043-a6d7-19d8c69962cd beb834cf-7bde-40d7-8097-92e26777f380
ERROR (BadRequest): Port c97cbdcf-9aab-4043-a6d7-19d8c69962cd not usable for instance beb834cf-7bde-40d7-8097-92e26777f380. (HTTP 400) (Request-ID: req-35a31098-8fd3-49d1-8625-454ea73e1ecb  
```

Root Cause
-------------------

It was caused by instance and port created on different tenant. Here are the detail:

#### Instance detail
```
HOSTNAME:~ # nova show beb834cf-7bde-40d7-8097-92e26777f380
+--------------------------------------+---------------------------------------------------------------+
| Property                             | Value                                                         |
+--------------------------------------+---------------------------------------------------------------+
| created                              | 2017-07-12T13:41:22Z                                          |
| flavor                               | some-instance-flavour (2bb2e97f-4b6c-40c5-8178-4c7608117808)  |
| hostId                               | CBTCNBICTCT01                                                 |
| id                                   | beb834cf-7bde-40d7-8097-92e26777f380                          |
| image                                | windows_server_2012_r2 (d6804d69-c212-48c4-b32f-2718d3b18857) |
| name                                 | instance-name                                                 |
| os-extended-volumes:volumes_attached | [{"id": "813ad7f6-3262-475c-82ee-9ccd75649b75"}]              |
| status                               | SHUTOFF                                                       |
| tags                                 | []                                                            |
| tenant_id                            | a858a61d283f400ca2c63682b86da203                              |
| updated                              | 2017-09-04T08:46:14Z                                          |
| user_id                              | e7e6e2f3a7ca4ae5bb1c91c40cae4d76                              |
+--------------------------------------+---------------------------------------------------------------+
```
Tenant ID: <span style="background-color: red;">a858a61d283f400ca2c63682b86da203</span> where tenant name is admin

#### Port detail
```
HOSTNAME:~ # neutron port-show c97cbdcf-9aab-4043-a6d7-19d8c69962cd
+-----------------------+-------------------------------------------------------------------------------------+
| Field                 | Value                                                                               |
+-----------------------+-------------------------------------------------------------------------------------+
| fixed_ips             | {"subnet_id": "7f0c8c1b-efd6-4437-9f03-6661fd868e4d", "ip_address": "10.10.100.10"} |
| id                    | c97cbdcf-9aab-4043-a6d7-19d8c69962cd                                                |
| mac_address           | fa:16:7e:9b:dc:1f                                                                   |
| name                  | instance-port-name                                                                  |
| network_id            | 42fccefd-874e-43aa-a5cd-f66c1d05fd9f                                                |
| security_groups       | 35b43d53-762a-4f0e-9d96-c7fc860d943a                                                |
| status                | DOWN                                                                                |
| tenant_id             | a93f5da3afb142ea955a4c8651db8c3a                                                    |
+-----------------------+-------------------------------------------------------------------------------------+
```
Tenant ID: <span style="background-color: red;">a93f5da3afb142ea955a4c8651db8c3a</span> where tenant name is dc_system_tenant1

Solution
-------------------

Use same tenant to create port and instance. You can export it correct enviroment variable for keystone.

Enviroment variable used before

> export OS_AUTH_URL=https://keystone.url/identity-admin/v2.0 <br>
> <span style="background-color: yellow;">export OS_USERNAME=tenant1_admin</span> <br>
> <span style="background-color: yellow;">export OS_TENANT_NAME=dc_system_tenant1</span> <br>
> export OS_REGION_NAME=az1.tenant1 <br>
> export NOVA_ENDPOINT_TYPE=internalURL <br>
> export OS_ENDPOINT_TYPE=internalURL <br>
> export CINDER_ENDPOINT_TYPE=internalURL <br>
> export OS_VOLUME_API_VERSION=2 <br>
> export BASE_BOND=brcps

The correct enviroment variable

> export OS_AUTH_URL=https://keystone.url/identity/v2.0 <br>
> <span style="background-color: yellow;">export OS_USERNAME=tenant2_admin</span> <br>
> <span style="background-color: yellow;">export OS_TENANT_NAME=admin</span> <br>
> export OS_REGION_NAME=az1.tenant1 <br>
> export NOVA_ENDPOINT_TYPE=internalURL <br>
> export OS_ENDPOINT_TYPE=internalURL <br>
> export CINDER_ENDPOINT_TYPE=internalURL <br>
> export OS_VOLUME_API_VERSION=2 <br>
> export BASE_BOND=brcps <br>

> **Tip:** You can use `env | grep "OS_"` to check keystone enviroment variable that has been set.  

