---
title:      Virtual Private Server Sebagai Proxy Server dan Web Server
date:       2013-07-02
summary:    Konfigurasi proxy Squid dan apache di Lab
category:   Linux
---

Pembahasan kali ini merupakan pengaplikasian konfigurasi jaringan komputer yang menggunakan sebuah virtual OS sebagai server yang memiliki 2 network line. Target pratikum adalah membuat Proxy Server dan Webserver (http dan https). Modul pratikum yang digunakan adalah Virtual Server dengan OS Debian 6 dan router MikroTik RouterBoard 1100.

## Skema Jaringan

![Skema jaringan yang diterapkan](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/1.png)

Skema jaringan yang diterapkan

## Persiapan

### Assign IP Server

Untuk men-assign static IP address untuk eth0 dan eth1 edit file pada /etc/network/interfaces. Sesuai skema eth0 akan di-assign IP 10.252.108.85 sedangkan eth1 akan di-assign IP 192.168.1.85. eth0 di-assign dengan tujuan sebagai remote address untuk SSH dan penghubungan dengan [debian mirror kebo](http://kebo.pens.ac.id/) sedangkan eth1 menggunakan gateway untuk mengkaitkan server ke node router 192.168.1.x. Berikut hasil konfigurasi file interfaces:

![konfigurasi file interfaces](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/2.png)

Setelah di-set, restart service networking.

Untuk tetap terkoneksi dengan debian mirror kebo, set nameserver /etc/resolv.conf ke DNS IPv6 PENS yaitu 2001:df0:a8:85::3 dan 2001:df0:a8:85::4. Berikut hasil konfigurasi file resolv.conf:

![konfigurasi file resolv.conf](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/3.png)

Ketika server dapat menghubungi server debian mirror kebo sehingga dapat meng-update atau menginstal paket yang dibutuhkan, maka assign ip server berhasil dikonfigurasi.

### Menginstal ssh server

Untuk mempermudah akses ke server, install ssh server dengan perintah 

```bash
apt-get install ssh
```

Berikut hasil instalasi:

![Hasil instalasi ssh](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/4.png)

Untuk melakukan remote ssh, lakukan dengan perintah #ssh ip_address_server, contohnya 

```bash
ssh 10.252.108.85
```

Ketika server dapat di-remote ssh, maka instalasi ssh server berhasil dikonfigurasi.

## Mengkonfigurasi Webserver Server

Pada kesempatan ini, target webserver ini adalah webserver yang dilengkapi fitur ssl. Hal yang pelu dilakukan pertama adalah menginstal apache2 dan openssl. Menginstal dapat menggunakan perintah #apt-get install apache2 openssl.

![Hasil instalasi apache2 dan openssl](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/5.png)

Kemudian untuk membuat web dengan koneksi ssl maka diperlukan sebuah sertifikat ssl. Untuk membuatnya gunakan openssl untuk membuat self-signed ssl certificate. Berikut hasil pembuatan sertifikat ssl:

Membuat rsa key:

![Membuat rsa key](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/6.png)

Membuat certificate identity:

![Membuat file certificate](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/8.png)

Untuk membuat localhost memakai ssl certificate maka edit file /etc/apache2/sites-available/default-ssl. Pastikan bahwa SSLEngine On dan ubah SSLCertificateFile dan SSLCertificateKeyFile ke sertifikat dan key yang telah dibuat sebelumnya. Berikut hasil pengubahan SSLCertificateFile dan SSLCertificateKeyFile pada file default-ssl:

![Hasil pengubahan SSLCertificateFile dan SSLCertificateKeyFile pada file default-ssl](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/9.png)

Hasil pengubahan SSLCertificateFile dan SSLCertificateKeyFile pada file default-ssl

Kemudian enable-kan modul ssl dengan perintah:

```bash
a2enmod ssl
```

![enable ssl](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/10.png)

Kemudian masukan default-ssl sebagai site yang di-enable-kan dengan perintah:

```bash
a2ensite default-ssl
```

![Mengaktifkan default-ssl](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/11.png)

Terakhir, restart services apache2 mengunakan:

```bash
/etc/init.d/apache2
```

![Hasil restart apache2 service berhasil](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/12.png)

Dan hasilnya:

![http://localhost diakses](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/13.png)

http diakses

![https://localhost diakses](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/14.png)

https diakses

Ketika kedua halaman tersebut dapat diakses, maka konfigurasi webserver dengan ssl berhasil dikonfigurasi.

## Mengkonfigurasi Proxy Server

Konfigurasi selanjutnya adalah Proxy Server. Proxy software yang digunakan adalah squid. Untuk memulai konfigurasi Proxy server, install paket squid dengan perintah #apt-get install squid. Setelah itu, untuk membuat squid secara otomatis mendeteksi sistem hostname server maka set visible_hostname pada /etc/squid/squid.conf. Tujuan menambahkan hostname pada visible_hostname adalah membantu squid untuk memberikan direksi pada squid kemana squid harus memberikan error message, dan lain sebagainya. Berikut hasil pengisian hostname pada /etc/squid/squid.conf:

![pengisian hostname pada /etc/squid/squid.conf](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/15.png)

Kemudian jalankan perintah #squid –z. Perintah tersebut digunakan untuk membangun atau rebuild direktori cache dan swap directories untuk squid. Berikut hasil eksekusi perintah #squid –z:

![hasil eksekusi perintah #squid –z](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/16.png)

Kemudian tambahkan ACL (Access Control List) sesuai dengan aturan yang sudah ditentukan, dimana segmen 10.252.108.x tidak dapat mengakses internet, segmen 192.168.1.x dan 192.168.24.x dapat mengakses internet dengan autentifikasi user, dan sisanya tidak dapat mengakses internet.  Berikut hasil pembuatan ACL pada /etc/squid/squid.conf:

![hasil pembuatan ACL pada /etc/squid/squid.conf](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/17.png)

Setelah itu restart service squid untuk menerapkan hasil konfigurasi pada /etc/squid/squid.conf.

![restart service squid](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/18.png)

Kemudian buat user auth list dengan perintah #htpasswd -c /etc/squid/password nama_user. Berikut contoh pembuatan user autentifikasi untuk squid:

![contoh pembuatan user autentifikasi untuk squid](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/19.png)

Dan hasilnya

![browser yang sudah diset proxy-nya akan mngeluarkan autentifikasi seperti gambar diatas](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/20.png)

Browser yang sudah diset proxy-nya akan mngeluarkan autentifikasi seperti gambar diatas

Ketika proxy dapat berjalan pada jaringan lokal yang diuji, dan pada jaringan yang diblok tidak dapat mengakses interner, maka squid proxy server sudah dapat dikonfigurasi dengan benar. Kerja proxy juga dapat dicek pada /var/log/squid/access.log. Berikut gambar hasil akses proxy:

![Hasil akses proxy klien pada /var/log/squid/access.log](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/21.png)

Hasil akses proxy klien pada /var/log/squid/access.log

## Mengkonfigurasi DNS server

Untuk mengatur aliasing nama ip dan name resolution diperlukan sebuah DNS server. DNS server yang diinstal adalah BIND. Untuk menginstal DNS server, jalankan perintah #apt-get install bind9. Kemudian file /etc/resolv.conf perlu dikonfigurasi untuk menseting komputer yang digunakan sebagai DNS server, maka option nameserver diisi dengan nomor IP address komputer tersebut. DNS server ang akan diseting berdomain kecut.com. Berikut hasil seting file /etc/resolv.conf:

![hasil seting file /etc/resolv.conf](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/22.png)

Setelah itu copy template database ke /var/cache/bind gunakan perintah #cp /etc/bind/db.local /var/cache/bind/kecut.com.db dan #cp /etc/bind/db.255 /var/cache/bind/kecut.com.rev. meng-copy selesai, edit database file menggunakan perintah #nano /var/cache/bind/kecut.com.db. Berikut hasil edit dari file database:

![hasil edit dari file database](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/23.png)

Edit juga file reverse menggunakan perintah #nano /var/cache/bind/kecut.com.rev. Berikut hasil edit dari file reverse:

![hasil edit dari file reverse](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/24.png)

kemudian edit file konfigurasi named.conf.local dengan perintah #nano /etc/bind/named.conf.local arahkan database dan reverse file ke file yang sudah dibuat di /var/cache/bind. Juga tambahkan allow-update untuk dhcp sehingga server DHCP dapat secara dinamis memperbarui catatan DNS record yang dibuat. Berikut hasilnya:

![Hasil edit file /etc/bind/named.conf.local](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/25.png)

Juga edit file konfigurasi named.conf.option dengan perintah #nano /etc/bind/named.conf.option arah dns forwarder ke 180.131.144.144 dan 180.131.145.145.

![Hasil edit file /etc/bind/named.conf.option](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/26.png)

Terakhir, restart services bind menggunakan perintah #/etc/init.d/bind9 restart.

![Restart service bind](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/27.png)

Hasil set up DNS dapat dicheck menggunakan dig

![nslookup ke www.kecut.com dan nslookup ke ip 192.168.1.85](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/28.png)

nslookup ke www.kecut.com dan nslookup ke ip 192.168.1.85

Ketika nslookup mengembalikan return nilai sesuai dengan setting yang dilakukan sebelumnya, maka dns server sudah dapat dikonfigurasi dengan benar.

## Mengkonfigurasi Router

Router yang digunakan adalah router MikroTik RouterBoard 1100. Pada router ini akan digunakan sebagai nat router. Tersedia 13 ethernet. eth1 digunakan sebagai penyambung ke jaringan 192.168.1.x, selanjutnya eth2-eht12 digunakan untuk klien (eth13 untuk pxe boot console). Berikut konfigurasi yang dilakukan:

Pertama assign ip 192.168.1.233 ke eth1, dapat dilakukan menjadi static 1 dengan perintah ip address add address=192.168.1.233 atau dengan mengassignnya sebagai dhcp client jaringan 192.168.1.x.

Kemudian, tambahakan ip static juga ke eth2, dengan alamat 192.168.24.1/24.

![Hasil assign ip ke eth2](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/29.png)

Setelah itu tambahkan route gateway address 192.168.1.1.

![Hasil menambahkan route gateway address 192.168.1.1](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/30.png)

Langkah selanjutnya set dns router ke dns yang telah dibuat sebelumnya yaitu 192.168.1.85 dan beri kemampuan untuk mengijikan remote request.

![set dns router ke dns yang telah dibuat dan mengijinkan remote request](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/31.png)

Untuk memudahkan pengalamatan ip ke klien buat dhcp server pada mikrotik.

![set dhcp server pada mikrotik](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/32.png)

Terakhir buat scr nat dengan masquerade pada eth1

![set scr nat dengan masquerade pada eth1](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/33.png)

## Uji Coba dan Hasil

Berikut hasil uji coba

### Uji DHCP mikrotik untuk klien

![Hasil assign IP DHCP dari mikrotik](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/34.png)

### Uji DNS server

![Set resolver](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/35.png)

Karena allow-update untuk dhcp diijikan sehingga server DHCP dapat secara dinamis memperbarui catatan DNS record pada /etc/resolv.conf pada klien

![nslookup ke www.kecut.com dan nslookup ke ip 192.168.1.85](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/36.png)

### Uji webserver dan dns pada browser

![Hasil uji memanggil site pada webserver](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/37.png)

Coba buka web

![konfigurasi proxy pada client](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/39.png)

Konfigurasi proxy pada client

![browser yang sudah diset proxy-nya akan mngeluarkan autentifikasi seperti gambar diatas](/images/2013-07-02-virtual-private-server-sebagai-proxy-server-dan-web-server/40.png)

Browser yang sudah diset proxy-nya akan mngeluarkan autentifikasi seperti gambar diatas.
