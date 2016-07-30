---
layout:     post
title:      Konfigurasi MikroTik DMZ
date:       2013-04-06
summary:    Konfigurasi Demilitarized Zone di lab
categories: network
---

DMZ adalah singkatan untuk Demilitarized Zone, istilah berasal dari penggunaan militer, yang berarti daerah penyangga antara dua musuh. Menerapkannya pada lingkup TI, itu berarti komputer atau subnetwork kecil yang berada di antara jaringan internal yang terpercaya, seperti LAN swasta perusahaan, dan jaringan eksternal terpercaya, seperti Internet publik.

Biasanya, DMZ berisi perangkat diakses untuk lalu lintas internet, seperti Web (HTTP) server, server FTP, SMTP (e-mail) server dan DNS server.

Demilitarized zone digunakan untuk mengamankan jaringan internal dari akses eksternal. DMZ dapat dibuat menggunakan MikroTik Router. Ada banyak cara yang berbeda untuk merancang jaringan dengan DMZ. Metode dasar adalah dengan menggunakan single Linux firewall dengan 3 Ethernet Card. Berikut meruapakan pembahasan pengaturan DMZ dan forwarding public traffic untuk server internal.

### DMZ Setup

Pada pembahasan kali ini digunakan MikroTik RouterBoard 1100.

![MikroTik RouterBoard 1100](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/1.jpg)

Pada pembahasan ini digunakan 3 NIC dengan detail penggunaan sebagai berikut:

1. eth1 dengan 10.252.108.14, IP address yang berasal dari EEPIS server (dalam hal ini, IP address ini dianggap IP publik), didapat dari DHCP EEPIS Server .
2. eth2  dengan 192.168.2.1, private IP address, DMZ connected ke Web server.
3. eth3 dengan 192.168.1.1 private IP address, jaringan privat digunakan untuk client.

![Network topologi](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/2.png)

Untuk mempermudah konfigurasi, digunakan Winbox, utility untuk melakukan remote GUI ke Router Mikrotik. For windows. Berikut pengkonfigurasian yang telah dicoba:

**1.** Login pada MikroTik. Pertama buat koneksi antara MikroTik dengan EEPIS server, buat eth1 untuk mendapatkan hasil DHCP dari EEPIS server. Berikut hasil DHCP client pada eth1.

![Login ke mikrotik](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/3.jpg)

**2.** Jika diperlukan beri komentar pada Ethernet port yang dipakai, berikut hasil pemberian komentar pada eth port sesuai dengan ketentuan diatas.

![Tambah komen](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/4.jpg)

**3.** Buka terminal MikroTik, pertama lihat hasil pada semua interface

![Lihat interface](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/5.jpg)

**4.** Assign ip address 192.168.1.1 pada eth3. Kemudian cek perubahannya

![Assign IP ke eth3](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/6.jpg)

**5.** Tambahkan ip address 10.252.108.1 (gateway dari server EEPIS) sebagai gateway dari ip yang dianggap sebagai public IP. Cek hasil penambahannya.

![Tambah gateway](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/7.jpg)

**6.** Untuk membuat DHCP server, assign DNS servernya. DNS server yang digunakan adalah DNS server dari EEPIS yaitu 202.9.85.3. Set remote remote request menjadi yes untuk menentukan apakah akan server DNS memperbolehkan network requests. Ketika remote remote request diaktifkan, router MikroTik menanggapi TCP dan UDP DNS permintaan pada port 53. Kemudian cek hasil penambahannya.

![Set DNS](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/8.jpg)

**7.** Kemudian buat DHCP service untuk eth3 dengan network space 192.168.1.0/24 dengan gateway 192.168.1.1 pada DNS server EEPIS yang udah diset sebelumnya. Konfigurasi lebih lanjut terlihat pada gambar dibawah ini.

![Set DHCP](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/9.jpg)

**8.** Kemudian untuk eth2 lakukan assign IP yaitu 192.168.2.1.

![Assign IP ke eth2](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/10.jpg)

**9.** Tambahkan source NAT atau srcnat dengan action masquerade. Scrnat adalah jenis NAT yang mentranslasi paket yang berasal dari jaringan yang telah di-NAT. Router NAT akan mentranslasikan paket yang berasal dari IP address private ke IP address publik ketika melalui router. Pengembalian paket balasan akan dilakukan dengan me-reverse operasi srcnat. Masquerading akan mengubah alamat IP sumber dan port dari paket berasal dari jaringan 192.168.1.0/24 ke alamat 10.255.108.114 dari router ketika paket yang diarahkan melalui router tersebut.

![Tambah source NAT](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/11.jpg)

**10.** Kemudian,juga tambahkan destination NAT atau dstnat dari semua source pada IP tujuan 10.252.108.14 pada protocol TCP kearah 192.168.2.2 ke port 80 dimana pada IP 192.168.2.2:80  sudah disiapkan sebuah web service . Dstnat adalah jenis NAT yang mentranslasi paket yang berasal dari jaringan yang mengakses IP public (10.252.108.14) ke jaringan local (private) dan mem-forward-nya ke IP yang sudah di-assign yaitu 192.168.2.2:80.

![Tambah destination NAT](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/12.jpg)

### Hasil percobaan pada Client

Hasil DHCP service yang diberikan oleh DHCP server MikroTik.

![DHCP yang didapat client](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/13.png)

Hasil ping client (private network ke 192.168.1.1 dan ke IP public 10.252.108.14)

![Ping dari client](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/14.png)

Hasil akses client ke website luar – website EEPIS

![Akses ke website luar](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/15.png)

Ketika IP public diakses, maka langsung di forward ke 192.168.2.2 bukan mengakses 192.168.1.1/webconfig

![DMZ portal](images/posts/2013-04-06-konfigurasi-mikrotik-dmz-demilitarized-zone/16.png)

### Kesimpulan

DMZ adalah suatu area  bagi hackers yang digunakan untuk melindungi system internal yang berhubungan dengan serangan hacker (hack attack). DMZ bekerja pada seluruh dasar pelayanan jaringan yang membutuhkan akses terhadap jaringan “Internet atau dunia luar” ke bagian jaringan yang lainnya. Dengan begitu, seluruh “open port” yang berhubungan dengan dunia luar akan berada pada jaringan, sehingga jika seorang hacker melakukan serangan dan melakukan crack pada server yang menggunakan sistem DMZ, hacker tersebut hanya akan dapat mengakses hostnya saja, tidak pada jaringan internal. Secara umum DMZ dibangun berdasarkan tiga buah konsep, yaitu: NAT (Network Address Translation), PAT (Port Addressable Translation), dan Access List. NAT berfungsi untuk menunjukkan kembali paket-paket yang datang dari “real address” ke alamat internal. Misal : jika kita memiliki “real address” 10.252.108.14, kita dapat membentuk suatu NAT langsung secara otomatis pada data-data yang datang ke 192.168.1.1 (sebuah alamat jaringan internal). Kemudian PAT berfungsi untuk menunjukan data yang datang pada particular port, atau range sebuah port dan protocol (TCP/UDP atau lainnya) dan alamat IP ke sebuah particular port atau range sebuah port ke sebuah alamat internal IP. Sedangkan access list berfungsi untuk mengontrol secara tepat apa yang datang dan keluar dari jaringan dalam suatu pertanyaan. Misal : kita dapat menolak atau memperbolehkan semua ICMP yang datang ke seluruh alamat IP kecuali untuk sebuah ICMP yang tidak diinginkan.

Network Address Translation(NAT) berfungsi untuk mengarahkan alamat riil, seperti alamat internet, ke bentuk alamat internal. Misalnya alamat riil 10.252.108.14 dapat diarahkan ke bentuk alamat jaringan internal 192.168.2.1 secara otomatis dengan menggunakan NAT. Namun jika semua informasi secara otomatis ditranslasi ke bentuk alamat internal, maka tidak ada lagi kendali  terhadap informasi yang masuk. Oleh karena itu maka muncullah PAT.

Port Address Translation(PAT) berfungsi untuk mengarahkan data yang masuk melalui port, sekumpulan port dan protokol, serta alamat IP pada port atau sekumpulan post. Sehingga dapat dilakukan kendali ketat pada setiap data yang mengalir dari dan ke jaringan.

Access List melakukan layanan pada pengguna agar dapat mengendalikan data jaringan. Daftar Akses dapat menolak atau menerima akses dengan berdasar pada alamat IP, alamat IP tujuan, dan tipe protokol.

Pada percobaan, tidak dilakukan access list, namun percobaan ini dapat dikatakan sebagai DMZ sederhana yang diimplementasikan pada sebuat MikroTik Router.