---
title:      Intrusion Detection System Menggunakan Snort
date:       2013-07-05
summary:    Konfigurasi IDS Snort di lab
category:   Network
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

## Snort Setup

Pada pembahasan kali ini digunakan MikroTik RouterBoard 1100.

![MikroTik RouterBoard 1100](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/1.jpg)

Pada pembahasan ini digunakan 3 NIC dengan detail penggunaan sebagai berikut:

- eth1 dengan 10.252.108.14, IP address yang berasal dari EEPIS server (dalam hal ini, IP address ini dianggap IP publik), didapat dari DHCP EEPIS Server .
- eth3 dengan 192.168.1.1 private IP address, jaringan privat digunakan untuk client.
- eth4 dengan master port eth3 sehingga menggunakan private IP address.

Berikut penggambaran skema pada pembahasan:

![penggambaran skema pada pembahasan](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/2.jpg)

Setelah konfigurasi router dan clientsudah dibuat, lingkungan dapat digunakan untuk melakukan percobaan menggunakan Snort.

## Instalasi Snort (Community Version)

Lakukan instalasi snort pada Client1. Pada percobaan ini Client1 digunakan sebagai PC server temapat Snort diinstal.

```bash
apt-get install snort
```

Kemudian masukkan range network yang akan dianalisa. Berikut hasil input range network untuk instalasi snort:

![input range network untuk instalasi snort](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/3.png)

## Menjalankan Snort dalam Sniffer mode

Seperti sudah dibahas diatas bahwa Snort memiliki 3 mode pengoperasian, pada pembahasan ini akan dibahas mode pengoperasian tersebut. Mode yang pertama adalah Sniffer mode. Untuk menjalankan snort pada sniffer mode tidaklah sukar, beberapa contoh parameter yang dapat digunakan di bawah ini:

```bash
snort –v
```

Berikut hasil perintah #snort –v:

![hasil perintah #snort –v](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/4.png)

Setelah startup, Snort menampilkan mode, logging directory dan interface Snort langsung me-listen pada port. Kemudian Snort mulai men-dump paket.  Snort akan meng-generate ringkasan paket yang diambil, termasuk protokol dan statistik lainnya, seperti fragmentasi paket. Opsi -d akan menampikan data aplikasi. Opsi ini menyediakan output bahkan lebih rinci. Data aplikasi terlihat jelas dan dapat melihat teks biasa dalam paket.

```bash
snort –vd
```

Berikut hasil perintah #snort –vd:
![hasil perintah #snort –vd](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/5.png)

```bash
snort –vde
```

Berikut hasil perintah #snort –vde:
![Berikut hasil perintah #snort –vde:](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/6.png)

Kebanyakan jaringan akan menghasilkan load off traffic dan output sniffer Snort ke layar akan bergulir dengan sangat cepat. Jadi lebih baik untuk mengarahkan output ke sebuah file log gantinya.

```bash
snort –v –d –e
```

Berikut hasil perintah #snort –v –d –e:

![hasil perintah #snort –v –d –e](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/7.png)

dengan menambahkan beberapa switch –v, -d, -e akan menghasilkan beberapa keluaran yang berbeda, yaitu:

- -v, untuk melihat header TCP/IP paket yang lewat.
- -d, untuk melihat isi paket.
- -e, untuk melihat header link layer paket seperti Ethernet header.

## Menjalankan Snort dalam Packet Logger mode

Karena output sniffer Snort ke layar akan bergulir dengan sangat cepat, untuk mempermudah pembacaan masukkan hasil snort ke dalam file, jalankan perintah berikut:

```bash
snort –dev –i eth0 –L /var/log/snort/snort.log
```

Berikut hasil me-log-kan hasil snort pada snort.log

![hasil me-log-kan hasil snort pada snort.log](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/8.png)

Akan menghasilkan sebuah file di folder /var/log/snort. Berikut hasil perintah # ls dari folder /var/log/snort:

![hasil perintah # ls dari folder /var/log/snort](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/9.png)

Untuk membaca file snort (misal: snort.log.1234) berikan option –r pada snort

```bash
snort -dev -r /var/log/snort/snort.log.1234
```

Berikut hasil read dari snort.log yang sudah digenerate

![hasil read dari snort.log yang sudah digenerate](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/10.png)

Log yang di-generate oleh Snort adalah log yang dienkripsi sehingga harus dibaca dengan pertintah diatas.

## Menjalankan Snort dalam NIDS (Network Intrusion Detection System) mode

Mode operasi snort yang paling rumit adalah sebagai pendeteksi penyusup (intrusion detection) di jaringan yang kita gunakan. Ciri khas mode operasi untuk pendeteksi penyusup adaah dengan menambahkan perintah ke snort untuk membaca file konfigurasi –c nama-file-konfigurasi.conf. Isi file konfigurasi ini lumayan banyak, tapi sebagian besar telah di set secara baik dalam contoh snort.conf yang dibawa oleh source snort.

Untuk menjalankan snort dengan mode NIDS, opsi e dihilangkan karena kita tidak perlu mengetahui link layer MAC. Opsi v dihilangkan juga, jalankan menggunakan option sebagai berikut:

```bash
snort -d -h 192.168.1.0/24 -l /var/log/snort -c /etc/snort/snort.conf
```

Berikut hasil eksekusi perintah dari snort running In IDS mode:

![hasil eksekusi perintah dari snort running In IDS mode](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/11.png)

Untuk mengetahui kerja Snort dalam NIDS mode, jalankan scanning dari komputer lain (PC Client) dengan nmap menuju komputer yang anda pasangi snort (PC Server). Terlebih dulu jalankan snort dengan mode NIDS, kemudian lakukan scanning dengan perintah:

```bash
snort -d -h 192.168.1.0/24 host <no_ip_snort> -l /var/log/snort –c /etc/snort/snort.conf
```

Berikut merupakan hasil nmap dari client2 ke client 1:

![hasil nmap dari client2 ke client 1](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/12.jpg)

Setelah di scan menggunakan nmap. Untuk melihat apa yang dicatat oleh snort, dapat di hasil log snort, atau untuk mempermudahnya, dapat dilihat di alert log snort. Berikut hasil yang di tangkap oleh alert log snort pada port 53.

```bash
[**] [1:257:9] DNS named version attempt [**]
[Classification: Attempted Information Leak] [Priority: 2]
04/08-03:31:08.565026 192.168.1.253:62292 -> 192.168.1.252:53
TCP TTL:128 TOS:0x0 ID:22366 IpLen:20 DgmLen:72 DF
***AP*** Seq: 0x55293CD9  Ack: 0x190DE26D  Win: 0x100  TcpLen: 20
[Xref => http://cgi.nessus.org/plugins/dump.php3?id=10028][Xref => http://www.whitehats.com/info/IDS278]
```

Untuk percobaan berikutya adalah percobaan membuat rule baru. rule baru simpan di /etc/snort/rules. Rule yang dibuat kurang lebih adalah seperti berikut:

```bash
alert tcp any any -> any any (content:"10.252.108.99"; msg:"Someone is visiting Webfig";sid:1000001;rev:1;)
alert tcp any any -> any any (msg:"TCP Traffic";sid:1000002;rev:0;)
```

Rule diatas bermaksud untuk memberikan alert ketika ada transaksi dat TCP dari Ip berapa saja dan port berapa saja kemudia ke IP berapa saja ke port berapa saja dengan konten 10.252.108.99 akan memberikan pesan seperti diatas.

![membuat rule](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/13.png)

Kemudian tambahkan include $RULE_PATH/alltcp.rules pada /etc/snort/snort.conf. kemudian restart service snort.

![restart service snort](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/14.png)

Hasilnya adalah

![Hasil penerapan rule](/images/2013-07-04-intrusion-detection-system-menggunakan-snort/15.png)
