---
layout:     post
title:      RADIUS Proxy pada Virtual Private Server
date:       2013-07-05
summary:    Konfigurasi RADIUS Proxy di lab
categories: network
---

Remote Authentication Dial In User Service (RADIUS) adalah protokol jaringan yang menyediakan  manajemen Authentication, Authorization, dan Accounting (AAA) terpusat untuk komputer yang terhubung dan menggunakan layanan jaringan. RADIUS dikembangkan oleh Livingston Enterprises, Inc pada tahun 1991 sebagai sebuah server otentikasi akses dan protokol akuntansi dan kemudian dibawa ke Engineering Task Force (IETF) standar Internet.

Karena dukungan yang luas dan sifat mana-mana protokol RADIUS, sering digunakan oleh ISP dan perusahaan untuk mengatur akses ke jaringan internet maupun internal, jaringan nirkabel, dan layanan e-mail terintegrasi. Jaringan ini dapat menggabungkan modem, DSL, jalur akses, VPN, port jaringan, web server, dan sebagainya.

RADIUS merupakan protokol client / server yang berjalan pada lapisan aplikasi, menggunakan UDP sebagai transport. Remote Access Server, Virtual Server Private Network, saklar Jaringan dengan otentikasi berbasis pelabuhan, dan Network Access Server (NAS), semua gateway yang mengontrol akses ke jaringan, dan semua memiliki komponen klien RADIUS yang berkomunikasi dengan RADIUS server. RADIUS server biasanya proses latar belakang berjalan pada UNIX atau Microsoft Windows Server.

RADIUS memiliki tiga fungsi.:

- untuk mengotentikasi pengguna atau perangkat sebelum memberikan mereka akses ke jaringan,
- untuk mengotorisasi para pengguna atau perangkat untuk layanan jaringan tertentu dan
- untuk menjelaskan penggunaan layanan tersebut.

Salah satu produk RADIUS yang opensource adalah Freeradius Server. Freeradius Server adalah daemon untuk unix dan unix seperti rotoc operasi yang memungkinkan seseorang untuk membuat sebuah rotocol server radius, yang dapat digunakan untuk Otentikasi dan Akuntansi berbagai jenis akses jaringan. Untuk menggunakan server, anda juga membutuhkan benar klien pengaturan yang akan berbicara dengan itu, termasuk server terminal, Ethernet Switch, Wireless Access Point atau PC dengan perangkat lunak yang sesuai yang mengemulasi itu (PortSlave, radiusclient, dan sebagainya).

### Konfigurasi FreeRadius Proxy

##### Pre konfigurasi

Dianggap sudah menginstal proxy server squid dan menginstal webserver apache2. Pre konfigurasi dapat dilihat pada [post berikut](http://sapikuda.github.io/linux/2013/07/02/virtual-private-server-sebagai-proxy-server-dan-web-server/).

##### Konfigurasi

Instalasi Freeradius. Paket yang diinstal adalah `freeradius-mysql` dan `freeradius-util`.

![Instalasi free radius](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/1.png)

Kemudian install database mysql. Paket yang diinstal adalah `mysql-server mysql-client`

![Konfigurasi mysql server, set root password](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/2.png)

Setelah mysql terinstal, buat database Radius dan buat user radius serta memberikan privilege ke user radius. Untuk skema database radius, import skema dari `/etc/freeradius/sql/mysql/schema.sql`.

![Membangun database radius](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/3.png)

Seting konfigurasi koneksi radius dengan database. Buka file `/etc/freeradius/sql.conf` dan masukkan konfigurasi database mysql.

![Hasil konfigurasi koneksi database](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/4.png)

Kemudian include-kan hasil konfigurasi database ke konfigurasi services Radius. Buka file `/etc/freeradius/radiusd.conf`, hilangkan tanda `#` pada baris `$INCLUDE sql.conf`.

![meng- include-kan hasil konfigurasi database ke konfigurasi services Radius](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/5.png)

Untuk memberikan akses penyimpanan data pada mysql untuk meng-authorize, meng- accounting dan mengijikan session pada default enabled site, edit `/etc/freeradius/sites-available/default` dan hilangkan tanda comment pada baris yang mengandung `sql` di bagian `authorize{}` dan`sql` di bagian  `accounting{}`, dan `sql` di bagian `session{}`.

![Menghilangkan hilangkan tanda comment pada baris yang mengandung ‘sql’ di bagian authorize{}](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/6.png)

Kemudian untuk mengijinkan virtual server inner-tunnel (virtual server yang hanya mengijinkan request untuk melakukan tunneling dalam server radius sendiri) untuk me-request bertipe `EAP-TTLS` dan `PEAP`, edit `/etc/freeradius/sites-available/inner-tunnel`, dan hilangkan tanda comment yang mengandung `sql` di dalam bagian `authorize {}` dan di bagian `session {}`.

!Menghilangkan hilangkan tanda comment pada baris yang mengandung ‘sql’ di bagian authorize{} pada inner-tunnel](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/7.png)

Untuk meng-apply hasil konfigurasi dan mengecek apakah radius berjalan dengan baik restart service Freeradius dan kemudian stop dan start kembali service tersebut.

![Meng-apply hasil konfigurasi dan mengecek apakah radius berjalan dengan baik](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/8.png?psid=1Meng-apply%20hasil%20konfigurasi%20dan%20mengecek%20apakah%20radius%20berjalan%20dengan%20baik)

Kemudian konfigurasi `squid.conf` pada file `/etc/squid/squid.conf` dan tambahkan konfigurasi dibawah ini:

{% highlight bash %}
# TAG: auth_param
#Authentication Radius:
auth_param basic program /usr/local/squid/libexec/squid_radius_auth –f /etc/squid/squid_rad_auth.conf
auth_param basic children 5
auth_param basic realm LAB COMPUTER NETWORKS
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive off
acl radius-auth proxy_auth REQUIRED
# TAG: http_access
# Allow authorized users:
http_access allow radius-auth
{% endhighlight %}

![Hasil konfigurasi squid.conf](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/9.png)

Download plugin radius, kemudian ekstrak.

![Download plugin radius, kemudian ekstrak](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/10.png)

Pindah ke hasil direktori hasil ekstraksi `squid_radius_auth-1.10`, kemudian install plugin:

![Hasil pindah direktori](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/11.png)

![Hasil install plugin](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/12.png)

Kemudian Copy file binari plugin radius auth ke `/usr/local/squid/libexec/squid_radius_auth`. Kemudian buat file konfigurasi plugin radius 

{% highlight bash %}
# touch /etc/squid/squid_rad_auth.conf.
{% endhighlight %}

![Meng-kopi binary plugin radius ke /usr/local/squid/libexec/squid_radius_auth. Hasil membuat file konfigurasi plugin radius](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/13.png)

Masukkan konfigurasi berikut ke file squid_rad_auth.conf

![Hasil memasukan konfigurasi berikut ke file squid_rad_auth.conf](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/14.png)

Kemudian restart service squid untuk meng-apply hasil konfigurasi.

![Restart service squid](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/15.png)

Setup browser client dengan proxy alamat IP proxy server dan port 3128. Ketika Anda mulai browsing dengan browser akan muncul form login sebagai berikut:

![Hasil pengaturan proxy dan radius](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/16.png)

Untuk mempermudah manajemen Freeradius, install daloradius sebagai user interface manajemen

Ekstrak daloradius kemudian pindahkan ke `var/www/`. Kemudian masuk ke direktori `../daloradius/contrib/db`. Kemudian tambahkan tabel-tabel daloradius untuk menambahkan beberapa tabel dalam database radius.

![Menambahkan tabel-tabel daloradius dalam database radius](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/17.png)

Kemudian konfigurasi koneksi database ke daloradius ke `../daloradius/library/daloradius.conf.php`.

![Hasil konfigurasi koneksi database daloradius](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/18.png)

Jika mengalami error dalam konfigurasi install paket `php5-gd`, `php-pear` dan `php-db`. Berikut hasil konfigurasi daloradius:

![Hasil penambahan user](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/19.png)

Hasil penambahan user

Dan hasilnya adalah:

![hasilnya](//sapikuda.com/images/posts/2013-07-05-radius-proxy-pada-virtual-private-server/20.png)