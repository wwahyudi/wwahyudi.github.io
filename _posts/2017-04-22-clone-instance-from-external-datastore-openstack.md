---
title:      Clone Instance from External Datastore on Openstack
date:       2017-04-22
summary:   	Clone Instance from external datastore on Openstack 
category:   Openstack
---

So I was worked for some Openstack which is branded as FusionSphere Openstack 6.0. It's an Openstack that made by Huawei. On this environment Openstack version is Juno (Nova version `2014.2`). My objective on this post is creating an instance clone where the instance was created on external datastore.

1. First one, we need to look for the attached volume ID from the instance.

    ```bash
    nova show $instance-ID
    ```

    for example:

    ```bash
    nova show abb8709e-702d-4b0c-9e51-672ce419cebf
    ```

2. Then create a Cinder Volume based on the attached volume ID. Then wait until the volume created.

    ```bash
    cinder create --source-volid $attached-volume-id --name $new-clone-volume-name $new-clone-volume-size
    ```

    for example:

    ```bash
    cinder create --source-volid 95b9c0bf-22d2-43cb-b241-4a964f988f57 --name cirrosClone 1
    ```

    You can also add parameter `--progress` to show the progress of creating the volume.

3. Set the volume that has been create with flag bootable.

    ```bash
    cinder set-bootable $new-clone-volume-id true
    ```

    for example:

    ```bash
    cinder set-bootable 7d7211fd-6404-4a89-b7d0-2e85bd534c5f true
    ```

4. Upload the volume that has been create to be an image.

    ```bash
    cinder upload-to-image --disk-format qcow2 --container-format bare $new-clone-volume-id $new-clone-image-name
    ```

    for example:

    ```bash
    cinder upload-to-image --disk-format qcow2 --container-format bare 7d7211fd-6404-4a89-b7d0-2e85bd534c5f CirrosClone
    ```

    You can also add parameter `--progress` to show the progress of upload the volume.

5. Make the new image as a public, so other users may use the new cloned image.

    ```bash
    glance image-update $new-clone-image-id --is-public=true
    ```

    for example:

    ```bash
    glance image-update 7d7211fd-6404-4a89-b7d0-2e85bd534c5f --is-public=true
    ```

6. Then create new instance based on new cloned image that you have been made.
