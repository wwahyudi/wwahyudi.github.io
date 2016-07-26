---
layout:     post
title:      Intrusion Detection System Menggunakan Snort
date:       2013-07-05
summary:    Konfigurasi IDS Snort di lab
categories: network
---

Keamanan jaringan menjadi fokus penting dalam melindungi serangan terhadap informasi atau aset yang berharga bagi sebuah organisasi. Salah satu metode pengamanan yang baik adalah membangun sistem pendeteksi penyusupan.

Intrusion Detection System berfungsi melakukan pengamatan (monitoring) kegiatan-kegiatan yang tidak lazim pada jaringan, sehingga langkah awal dari para penyerang dapat diketahui. Dengan demikian administrator jaringan bisa melakukan tindakan pencegahan dan bersiap atas kemungkinan yang akan terjadi kemudian. Ada beberapa alasan untuk menggunakan IDS, diantaranya adalah :

- Mendeteksi serangan dan pelanggaran keamanan sistem jaringan yang tidak bisa dicegah oleh firewall, dan juga
- Mencegah resiko keamanan yang terus meningkat agar serangan tidak terulang kembali.

Untuk membangun IDS tentu ada banyak hal yang perlu dipertimbangkan entah dengan biaya yang mahal dengan memanfaatkan beberapa device yang memiliki modul IDS, adapula yang tanpa biaya dengan memanfaatkan sebuah open source yang telah ada. Melihat ada sesuatu yang dapat dibangun dengan gratis tentulah banyak sekali orang yang memanfaatkannya, karena cukup mudah memanfaatkan tools tersebut. Tools umum yang banyak sekali digunakan adalah Snort IDS.

Dalam pengoperasiannya, Snort memiliki 3 mode yaitu:

- Sniffer mode, untuk melihat paket yang lewat di jaringan.
- Packet logger mode, untuk mencatat semua paket yang lewat di jaringan untuk di analisa di kemudian hari.
- Intrusion Detection mode, pada mode ini snort akan berfungsi untuk mendeteksi serangan yang dilakukan melalui jaringan komputer. Untuk menggunakan mode IDS ini di perlukan setup dari berbagai rules / aturan yang akan membedakan sebuah paket normal dengan paket yang membawa serangan.

### Snort Setup

Pada pembahasan kali ini digunakan MikroTik RouterBoard 1100.

![MikroTik RouterBoard 1100](https://7zet9g.bay.livefilestore.com/y2pmi0fMZ0Ev2ZAgHXUv5Ve087RZP_ZiEt9woGLnzpGo40xGhAoFm3EJMAgr9L4MU9OQl1b7bP4AkKhEqXa8f7R_k1M8hoVEvp_9H9plNZ28qH5s2n0o4zExYp7Jp5CP4mU/1.jpg?psid=1)

Pada pembahasan ini digunakan 3 NIC dengan detail penggunaan sebagai berikut:

- eth1 dengan 10.252.108.14, IP address yang berasal dari EEPIS server (dalam hal ini, IP address ini dianggap IP publik), didapat dari DHCP EEPIS Server .
- eth3 dengan 192.168.1.1 private IP address, jaringan privat digunakan untuk client.
- eth4 dengan master port eth3 sehingga menggunakan private IP address.

Berikut penggambaran skema pada pembahasan:

![penggambaran skema pada pembahasan](https://7zet9g.bay.livefilestore.com/y2pmQS7ugCVxtuRlpwGTSKcnpC-BMmlSmiuffGS6O80FLoaF2Zsm9DeNdsepHQswa3Xd5jmphRE5MjiX8WSHc5Eut4Ma2aVM1xaxUHAHPus0nflwGBDJ35qbF-WP754Vxjh/2.jpg?psid=1)

Setelah konfigurasi router dan clientsudah dibuat, lingkungan dapat digunakan untuk melakukan percobaan menggunakan Snort.

### Instalasi Snort (Community Version)

Lakukan instalasi snort pada Client1. Pada percobaan ini Client1 digunakan sebagai PC server temapat Snort diinstal.

{% highlight bash %}
# apt-get install snort
{% endhighlight %}

Kemudian masukkan range network yang akan dianalisa. Berikut hasil input range network untuk instalasi snort:

![input range network untuk instalasi snort](https://7zet9g.bay.livefilestore.com/y2pQKz-dN0f4tzXc5BdauwkEwkxDJk_9EjeaE-8Do79khY-7rrNnGKwXllUTto7TdjTL3AUc1eJ0SaPTAMOdX-P3nFvkMAaSMkX6wZuG8Q3jKz-CH1-ybju5m_VcTeOqcNt/3.png?psid=1)

### Menjalankan Snort dalam Sniffer mode

Seperti sudah dibahas diatas bahwa Snort memiliki 3 mode pengoperasian, pada pembahasan ini akan dibahas mode pengoperasian tersebut. Mode yang pertama adalah Sniffer mode. Untuk menjalankan snort pada sniffer mode tidaklah sukar, beberapa contoh parameter yang dapat digunakan di bawah ini:

{% highlight bash %}
#snort –v
{% endhighlight %}

Berikut hasil perintah #snort –v:

![hasil perintah #snort –v](https://7zet9g.bay.livefilestore.com/y2pC8o19V1_YewytogTgPQeOu5QHV6ZaS4i3g_idgsqYlCDqcfV8hY9XdBxzvOjcTLqMgiqE5qePDD-uCBZSR-8ibJQ_aovtay4sWQokjIFKPu9kx-wV9C8j9SmAV2CWPR6/4.png?psid=1)

Setelah startup, Snort menampilkan mode, logging directory dan interface Snort langsung me-listen pada port. Kemudian Snort mulai men-dump paket.  Snort akan meng-generate ringkasan paket yang diambil, termasuk protokol dan statistik lainnya, seperti fragmentasi paket. Opsi -d akan menampikan data aplikasi. Opsi ini menyediakan output bahkan lebih rinci. Data aplikasi terlihat jelas dan dapat melihat teks biasa dalam paket.

{% highlight bash %}
#snort –vd
{% endhighlight %}

Berikut hasil perintah #snort –vd:
![hasil perintah #snort –vd](https://7zet9g.bay.livefilestore.com/y2pK7S-5B1yvWfkKR-tqVE44LDReOc0Q_tb9jb5d5r2QW_DqMO_W3nE6dqkrUGT-3E1YvTRg38HguI9cI2mrxTA6QYlnoCfq1u3zjQz1NZqXiAcE35VURo9kSO5CFobcHra/5.png?psid=1)

{% highlight bash %}
#snort –vde
{% endhighlight %}

Berikut hasil perintah #snort –vde:
![Berikut hasil perintah #snort –vde:](https://7zet9g.bay.livefilestore.com/y2pH30NFAPIvtaEHjG6uyGWLSp-LbNY2xFtyYTO0Q8PDdWUJ84_ubHXSb7bmbaz-Owg-BunFSbaWHuZEjCAsLGuPka-ToAMzF9lWBocvb6sud-AFwFXnWD3SoNWtrxCb0HW/6.png?psid=1)

Kebanyakan jaringan akan menghasilkan load off traffic dan output sniffer Snort ke layar akan bergulir dengan sangat cepat. Jadi lebih baik untuk mengarahkan output ke sebuah file log gantinya.

{% highlight bash %}
# snort –v –d –e
{% endhighlight %}

Berikut hasil perintah #snort –v –d –e:

![hasil perintah #snort –v –d –e](https://7zet9g-ch3301.files.1drv.com/y3pD1CbsYm8qaS_GmBtoq6BuGSsff224Sudrj2fV9UaHTE7p7BTqwtOlAyQEg4_SVrjUCrS2wjac99P0vAXJxB_CEfswo1VcFWR7M24fVPxL9QTPfn2TTDJz0MyE86jbR9H/7.png?psid=1&rdrts=144168776)

dengan menambahkan beberapa switch –v, -d, -e akan menghasilkan beberapa keluaran yang berbeda, yaitu:

- -v, untuk melihat header TCP/IP paket yang lewat.
- -d, untuk melihat isi paket.
- -e, untuk melihat header link layer paket seperti Ethernet header.

### Menjalankan Snort dalam Packet Logger mode

Karena output sniffer Snort ke layar akan bergulir dengan sangat cepat, untuk mempermudah pembacaan masukkan hasil snort ke dalam file, jalankan perintah berikut:

{% highlight bash %}
#snort –dev –i eth0 –L /var/log/snort/snort.log
{% endhighlight %}

Berikut hasil me-log-kan hasil snort pada snort.log

![hasil me-log-kan hasil snort pada snort.log](https://7zet9g-ch3301.files.1drv.com/y3pZKAzfosdcA43qEtghztjiA48q6aa2mhua0hxxvBjK6mn7HLqy5yRnHm3D2Rj2pj6C5XPjVn2mnnPrapS3-qeBRyIAj2eBOcBLGBRa9o0rw3tMneBKhX-BU5fg8DbyOfo/8.png?psid=1&rdrts=144168776)

Akan menghasilkan sebuah file di folder /var/log/snort. Berikut hasil perintah # ls dari folder /var/log/snort:

![hasil perintah # ls dari folder /var/log/snort](https://7zet9g-ch3301.files.1drv.com/y3pvCnCJmXvpRXR22RSkrpgwy2nl0hn4eYLtvyg_qKqGG8TM4iposCFKeE9zey5xrRWcAMegW2HO-kEyFIlAKZAS73xItTIZEP-RxjTLnAA1kx5QDmTz4wUGN_cTtq_uJ72/9.png?psid=1&rdrts=144168776)

Untuk membaca file snort (misal: snort.log.1234) berikan option –r pada snort

{% highlight bash %}
# snort -dev -r /var/log/snort/snort.log.1234
{% endhighlight %}

Berikut hasil read dari snort.log yang sudah digenerate

![hasil read dari snort.log yang sudah digenerate](https://7zet9g-ch3301.files.1drv.com/y3p0UVvSlDXloQxfHz6hfTwRqowpeZdvW4LbgbE3dw4hlQ20IruVW3BNWzhJ4qCWes0n0hzbWZqRiBkv7prLb5fm5iuCHrIF2DFFIYdq2tKOzhjkQ3dHAKtRh72NAS3PYrL/10.png?psid=1&rdrts=144168776)

Log yang di-generate oleh Snort adalah log yang dienkripsi sehingga harus dibaca dengan pertintah diatas.

### Menjalankan Snort dalam NIDS (Network Intrusion Detection System) mode

Mode operasi snort yang paling rumit adalah sebagai pendeteksi penyusup (intrusion detection) di jaringan yang kita gunakan. Ciri khas mode operasi untuk pendeteksi penyusup adaah dengan menambahkan perintah ke snort untuk membaca file konfigurasi –c nama-file-konfigurasi.conf. Isi file konfigurasi ini lumayan banyak, tapi sebagian besar telah di set secara baik dalam contoh snort.conf yang dibawa oleh source snort.

Untuk menjalankan snort dengan mode NIDS, opsi e dihilangkan karena kita tidak perlu mengetahui link layer MAC. Opsi v dihilangkan juga, jalankan menggunakan option sebagai berikut:

{% highlight bash %}
#snort -d -h 192.168.1.0/24 -l /var/log/snort -c /etc/snort/snort.conf
{% endhighlight %}

Berikut hasil eksekusi perintah dari snort running In IDS mode:

![hasil eksekusi perintah dari snort running In IDS mode](https://7zet9g-ch3302.files.1drv.com/y3p0xFGQwy3uhWXAmrVFqMPfI7u3RsKcspPNvHV8rOm0AnP5lJ4yPHjTo_lt3bjWHU95A5BYoIf5RbQzVKMa1onOry7_cwTTGLLbvCyKro9Pw5UaroMeo0_rdwR3kFtrv3I/11.png?psid=1&rdrts=144168779)

Untuk mengetahui kerja Snort dalam NIDS mode, jalankan scanning dari komputer lain (PC Client) dengan nmap menuju komputer yang anda pasangi snort (PC Server). Terlebih dulu jalankan snort dengan mode NIDS, kemudian lakukan scanning dengan perintah:

{% highlight bash %}
#snort -d -h 192.168.1.0/24 host <no_ip_snort> -l /var/log/snort –c /etc/snort/snort.conf
{% endhighlight %}

Berikut merupakan hasil nmap dari client2 ke client 1:

![hasil nmap dari client2 ke client 1](https://7zet9g-ch3301.files.1drv.com/y3p3tSYJf2WfPz5oGq0ii1cqePevaScYSnhSFKkuVKCkHwDPS3GR463WWYPsMAO2hjTMYPAMtMr1QdhR0YtFwuf2gLPht9TOy-YVnd-PThTzzqUGpd3qgeu5EK3bethNyKC/12.jpg?psid=1&rdrts=144168777)

Setelah di scan menggunakan nmap. Untuk melihat apa yang dicatat oleh snort, dapat di hasil log snort, atau untuk mempermudahnya, dapat dilihat di alert log snort. Berikut hasil yang di tangkap oleh alert log snort pada port 53.

{% highlight bash %}
[**] [1:257:9] DNS named version attempt [**]
[Classification: Attempted Information Leak] [Priority: 2]
04/08-03:31:08.565026 192.168.1.253:62292 -> 192.168.1.252:53
TCP TTL:128 TOS:0x0 ID:22366 IpLen:20 DgmLen:72 DF
***AP*** Seq: 0x55293CD9  Ack: 0x190DE26D  Win: 0x100  TcpLen: 20
[Xref => http://cgi.nessus.org/plugins/dump.php3?id=10028][Xref => http://www.whitehats.com/info/IDS278]
{% endhighlight %}

Untuk percobaan berikutya adalah percobaan membuat rule baru. rule baru simpan di /etc/snort/rules. Rule yang dibuat kurang lebih adalah seperti berikut:

{% highlight bash %}
alert tcp any any -> any any (content:”10.252.108.99”; msg:”Someone is visiting Webfig”;sid:1000001;rev:1;)
alert tcp any any -> any any (msg:"TCP Traffic";sid:1000002;rev:0;)
{% endhighlight %}

Rule diatas bermaksud untuk memberikan alert ketika ada transaksi dat TCP dari Ip berapa saja dan port berapa saja kemudia ke IP berapa saja ke port berapa saja dengan konten 10.252.108.99 akan memberikan pesan seperti diatas.

![membuat rule](https://7zet9g-ch3302.files.1drv.com/y3pUiPz5AB5PeFGtPOpe7paZ2OwK76rfm8tUyWpKdQoljbINb6Ua4IHh89xdzlVjsTLW_SgIQFIKosQzvqLySZ4H-dzG1_SN1o5ZISXV5yYKXkPygEzbgW042AJpjM6SuS-/13.png?psid=1&rdrts=144168777)

Kemudian tambahkan include $RULE_PATH/alltcp.rules pada /etc/snort/snort.conf. kemudian restart service snort.

![restart service snort](https://7zet9g-ch3301.files.1drv.com/y3p_yLSi2JoNs78uTPHyIPfzgE4H5DNbAqQKuwxphGq_LZ2OBZdeyoXQEpesMB8-2lDQSu_3WdsdfmJzd1SSRBs6LCcOG695hL2fWWxBNdQVR6jLfzVb9LZqRr3zfz5hnIa/14.png?psid=1&rdrts=144168778)

Hasilnya adalah

![Hasil penerapan rule](https://7zet9g-ch3302.files.1drv.com/y3p3a7yqlhik_vLxMnnBkp_5s5KzJqsKiNsXn6ZjHouGoehL0s2ba_8xmuQuatE1R8iQ1rBZgMkrdld0BCeh14zkz3WaIFGxS9VA0wgu8PqIXyWg34w1QkYvgTLpgUVUpO3/15.png?psid=1&rdrts=144168779)