---
title:      Enable UEFI in VMWare Workstation
date:       2019-03-20
summary:    Enable UEFI in VMWare Workstation
category:   VMWare
---

What you need to do to enable UEFI in your VMWare machine?

1. Create a new virtual machine
2. Open the .VMX file of your virtual machine in Notepad (or similar) and add the following line to it:

    ```bash
    firmware="efi"
    ```

3. Save .VMX file

    In addition, order to be able to select network boot, it is advisable to add a boot delay to the startup of the virtual machine.
    This can be done by adding the line `bios.bootdelay = 5000` (time is in milliseconds).

#### Update

On VMWare Workstation 16, you can simply enable EUFI on the `Virtual Machine Settings` ➜ `Options` ➜ `Advanced` ➜ `Firmware Type` as below:

{% include image.html url="/images/2019-03-20-enable-eufi-in-vmware-workstation/enable-uefi-in-vmware-workstation-16.png" description="Enable UEFI in VMWare Workstation 16" %}
