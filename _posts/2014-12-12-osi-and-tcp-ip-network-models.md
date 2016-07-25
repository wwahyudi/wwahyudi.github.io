---
layout:     post
title:      OSI and TCP-IP Network Models
date:       2014-12-12
summary:    Cheat sheet of OSI 7 Layers and TCP-IP
categories: network
---

Just a note for myself to remember about OSI 7 Layers and TCP/IP. 

### OSI 7 Layers

<table align="center" style="font-size: 0.75rem !important;">
    <tbody>
        <tr>
            <th style="text-align: center;">Layer #</th>
            <th style="text-align: center;">Name</th>
            <th style="text-align: center;">Mnemonic</th>
            <th style="text-align: center;">Encapsulation Units</th>
            <th style="text-align: center;">Devices or Components</th>
            <th style="text-align: center;">Keywords/Description</th>
        </tr>
        <tr>
            <td style="text-align: center;">7</td>
            <td>Application</td>
            <td>All</td>
            <td>data</td>
            <td>PC</td>
            <td>Network services for application processes, such as file, print, messaging, database services</td>
        </tr>
        <tr>
            <td style="text-align: center;">6</td>
            <td>Presentation</td>
            <td>People</td>
            <td>data</td>
            <td></td>
            <td>Standard interface to data for the application layer. MIME encoding, data encryption, conversion, formatting, compression</td>
        </tr>
        <tr>
            <td style="text-align: center;">5</td>
            <td>Session</td>
            <td>Seem</td>
            <td>data</td>
            <td></td>
            <td>Interhost communication. Establishes, manages and terminates connection between applications</td>
        </tr>
        <tr>
            <td style="text-align: center;">4</td>
            <td>Transport</td>
            <td>To</td>
            <td>segments</td>
            <td></td>
            <td>End-to-end connections and reliability. Segmentation/desegmentation of data in proper sequence. Flow control</td>
        </tr>
        <tr>
            <td style="text-align: center;">3</td>
            <td>Network</td>
            <td>Need</td>
            <td>packets</td>
            <td>router</td>
            <td>Logical addressing and path determination. Routing. Reporting delivery errors</td>
        </tr>
        <tr>
            <td style="text-align: center;">2</td>
            <td>Data Link</td>
            <td>Data</td>
            <td>frames</td>
            <td>bridge, switch, NIC</td>
            <td>Physical addressing and access to media. Two sublayers: Logical Link Control (LLC) and Media Access Control (MAC)</td>
        </tr>
        <tr>
            <td style="text-align: center;">1</td>
            <td>Physical</td>
            <td>Processing</td>
            <td>bits</td>
            <td>repeater, hub, transciever</td>
            <td>Binary transmission signals and encoding. Layout of pins, voltages, cable specifications, modulation</td>
        </tr>
    </tbody>
</table>

### OSI comparision with TCP/IP Protocol Stack


<table align="center" style="font-size: 0.75rem !important;">
    <tbody>
        <tr>
            <th style="text-align: center;">OSI #</th>
            <th style="text-align: center;">OSI Layer Name</th>
            <th style="text-align: center;">TCP/IP #</th>
            <th style="text-align: center;">TCP/IP Layer Name</th>
            <th style="text-align: center;">Encapsulation Units</th>
            <th style="text-align: center;">TCP/IP Protocols</th>
        </tr>
        <tr>
            <td style="text-align: center;">7</td>
            <td>Application</td>
            <td style="text-align: center;" rowspan="3">4</td>
            <td rowspan="3">Application</td>
            <td>data</td>
            <td>FTP, HTTP, POP3, IMAP, telnet, SMTP, DNS, TFTP</td>
        </tr>
        <tr>
            <td style="text-align: center;">6</td>
            <td>Presentation</td>
            <td>data</td>
            <td></td>
        </tr>
        <tr>
            <td style="text-align: center;">5</td>
            <td>Session</td>
            <td>data</td>
            <td></td>
        </tr>
        <tr>
            <td style="text-align: center;">4</td>
            <td>Transport</td>
            <td style="text-align: center;">3</td>
            <td>Transport</td>
            <td>segments</td>
            <td>TCP, UDP</td>
        </tr>
        <tr>
            <td style="text-align: center;">3</td>
            <td>Network</td>
            <td style="text-align: center;">2</td>
            <td>Internet</td>
            <td>packets</td>
            <td>IP</td>
        </tr>
        <tr>
            <td style="text-align: center;">2</td>
            <td>Data Link</td>
            <td style="text-align: center;" rowspan="2">1</td>
            <td rowspan="2">Network Access</td>
            <td>frames</td>
            <td></td>
        </tr>
        <tr>
            <td style="text-align: center;">1</td>
            <td>Physical</td>
            <td>bits</td>
            <td></td>
        </tr>
    </tbody>
</table>