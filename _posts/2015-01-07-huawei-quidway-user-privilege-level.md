---
title:      Huawei Quidway User Privilege Level
date:       2015-01-07
summary:    User privilege level on Huawei Quidway switch
category:   Huawei
---

You need to set a privilege level when you create a `local-user` on `aaa`. And here are the privilege you can set to your `local-user`:

<table class="post">
    <tbody>
        <tr>
            <th>User Level</th>
            <th>Command Level</th>
            <th>Level Name</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>0</td>
            <td>0</td>
            <td>Visit level</td>
            <td>This level gives access to commands that run network diagnostic tools (such as ping and tracert) and
                commands that start from a local device and visit external devices (such as Telnet client side).</td>
        </tr>
        <tr>
            <td>1</td>
            <td>0 and 1</td>
            <td>Monitoring level</td>
            <td>This level gives access to commands, like the display command, that are used for system maintenance and
                fault diagnosis.</td>
        </tr>
        <tr>
            <td>0</td>
            <td>0, 1, and 2</td>
            <td>Configuration level</td>
            <td>This level gives access to commands that configure network services provided directly to users,
                including routing and network layer commands.</td>
        </tr>
        <tr>
            <td>3-15</td>
            <td>0, 1, 2, and 3</td>
            <td>Management level</td>
            <td>This level gives access to commands that control basic system operations and provide support for
                services. These commands include file system commands, FTPcommands, TFTP commands, configuration file
                switching commands, power supply control commands, backup board control commands, user
                managementcommands, level setting commands, and debugging commands for fault diagnosis.</td>
        </tr>
    </tbody>
</table>

More about that, visit [Huawei support page about Quidway S5300 Configuring STelnet Login](https://support.huawei.com/hedex/hdx.do?lib=DOC1100750696AEJ1221U&docid=DOC1100750696&lang=en&v=02&tocLib=DOC1100750696AEJ1221U&tocV=02&id=EN-US_CONCEPT_0177100107&tocURL=resources%2525252Fdc%2525252Fdc_cfg_login_1004.html&p=t&fe=1&ui=3&keyword=User%25252Bprivilege%25252Blevel).
