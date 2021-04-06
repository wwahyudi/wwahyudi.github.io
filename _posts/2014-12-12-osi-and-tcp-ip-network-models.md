---
title:      OSI and TCP-IP Network Models
date:       2014-12-12
summary:    Cheat sheet of OSI 7 Layers and TCP-IP
category:   Network
---

Just a note for myself to remember about OSI 7 Layers and TCP/IP.

## OSI 7 Layers

<table class="post">
    <tbody>
        <tr>
            <th>Layer #</th>
            <th>Name</th>
            <th>Mnemonic</th>
            <th>Encapsulation Units</th>
            <th>Devices or Components</th>
            <th>Keywords/Description</th>
        </tr>
        <tr>
            <td>7</td>
            <td>Application</td>
            <td>All</td>
            <td>data</td>
            <td>PC</td>
            <td>Network services for application processes, such as file, print, messaging, database services</td>
        </tr>
        <tr>
            <td>6</td>
            <td>Presentation</td>
            <td>People</td>
            <td>data</td>
            <td></td>
            <td>Standard interface to data for the application layer. MIME encoding, data encryption, conversion, formatting, compression</td>
        </tr>
        <tr>
            <td>5</td>
            <td>Session</td>
            <td>Seem</td>
            <td>data</td>
            <td></td>
            <td>Interhost communication. Establishes, manages and terminates connection between applications</td>
        </tr>
        <tr>
            <td>4</td>
            <td>Transport</td>
            <td>To</td>
            <td>segments</td>
            <td></td>
            <td>End-to-end connections and reliability. Segmentation/desegmentation of data in proper sequence. Flow control</td>
        </tr>
        <tr>
            <td>3</td>
            <td>Network</td>
            <td>Need</td>
            <td>packets</td>
            <td>router</td>
            <td>Logical addressing and path determination. Routing. Reporting delivery errors</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Data Link</td>
            <td>Data</td>
            <td>frames</td>
            <td>bridge, switch, NIC</td>
            <td>Physical addressing and access to media. Two sublayers: Logical Link Control (LLC) and Media Access Control (MAC)</td>
        </tr>
        <tr>
            <td>1</td>
            <td>Physical</td>
            <td>Processing</td>
            <td>bits</td>
            <td>repeater, hub, transciever</td>
            <td>Binary transmission signals and encoding. Layout of pins, voltages, cable specifications, modulation</td>
        </tr>
    </tbody>
</table>

## OSI comparision with TCP/IP Protocol Stack

<table class="post">
    <tbody>
        <tr>
            <th>OSI #</th>
            <th>OSI Layer Name</th>
            <th>TCP/IP #</th>
            <th>TCP/IP Layer Name</th>
            <th>Encapsulation Units</th>
            <th>TCP/IP Protocols</th>
        </tr>
        <tr>
            <td>7</td>
            <td>Application</td>
            <td rowspan="3">4</td>
            <td rowspan="3">Application</td>
            <td>data</td>
            <td>FTP, HTTP, POP3, IMAP, telnet, SMTP, DNS, TFTP</td>
        </tr>
        <tr>
            <td>6</td>
            <td>Presentation</td>
            <td>data</td>
            <td></td>
        </tr>
        <tr>
            <td>5</td>
            <td>Session</td>
            <td>data</td>
            <td></td>
        </tr>
        <tr>
            <td>4</td>
            <td>Transport</td>
            <td>3</td>
            <td>Transport</td>
            <td>segments</td>
            <td>TCP, UDP</td>
        </tr>
        <tr>
            <td>3</td>
            <td>Network</td>
            <td>2</td>
            <td>Internet</td>
            <td>packets</td>
            <td>IP</td>
        </tr>
        <tr>
            <td>2</td>
            <td>Data Link</td>
            <td rowspan="2">1</td>
            <td rowspan="2">Network Access</td>
            <td>frames</td>
            <td></td>
        </tr>
        <tr>
            <td>1</td>
            <td>Physical</td>
            <td>bits</td>
            <td></td>
        </tr>
    </tbody>
</table>
