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

![Instalasi free radius](https://v8pruw-ch3301.files.1drv.com/y3pAFzhnVzTZfVjD5mePIDj6JgAdW1nuaBYsn0_2ArMg0CIo45U1rOTfdeQcaWqLHpCnUFAKSjX8hamIqk5UmXL_oQ9oiTPhWSQnf9Hssjhih4D6J2TA3WllNl8hrXzkX11/1.png?psid=1&rdrts=144165351)

Kemudian install database mysql. Paket yang diinstal adalah `mysql-server mysql-client`

![Konfigurasi mysql server, set root password](https://v8pruw-ch3302.files.1drv.com/y3pdaieRzHm4AqN5-4YqqqmKZPHlCHcMUTKaDk1M_LEYk_GDyT0ZZHcEnP4I5hf-u8ET9jYjHLj2fCBBp9riMiBRV1X2nbKkEOnhsvX95RMNC1t5BK-5E-uQRw8wpFQtYNh/2.png?psid=1&rdrts=144165351)

Setelah mysql terinstal, buat database Radius dan buat user radius serta memberikan privilege ke user radius. Untuk skema database radius, import skema dari `/etc/freeradius/sql/mysql/schema.sql`.

![Membangun database radius](https://v8pruw-ch3301.files.1drv.com/y3pY00PSBoLpWXoY5lhO29IEPiaHjBTILUZ431cEZrch2C7ebAD4gqcwmLOQftm31pNXV8WX2TOo1gOxVY5fm6eOc1x5oTaU_u4VbQSV24DzN0zoBg5Nivoo-6IWVewh5_g/3.png?psid=1&rdrts=144165351)

Seting konfigurasi koneksi radius dengan database. Buka file `/etc/freeradius/sql.conf` dan masukkan konfigurasi database mysql.

![Hasil konfigurasi koneksi database](https://v8pruw-ch3301.files.1drv.com/y3p7LmvQ5BmCRQ_hu3-PqTvLAWaJjss7uDanZ48sQULJ1ZGAW46RZ0OudGZaxvu_vmmkXBcvleT9JWwHrP66eiLHweIuXeU0gZSPRzkreB4Ak8mRXZ8xY0wr5O-Rxsd4vtL/4.png?psid=1&rdrts=144165351)

Kemudian include-kan hasil konfigurasi database ke konfigurasi services Radius. Buka file `/etc/freeradius/radiusd.conf`, hilangkan tanda `#` pada baris `$INCLUDE sql.conf`.

![meng- include-kan hasil konfigurasi database ke konfigurasi services Radius](https://v8pruw-ch3302.files.1drv.com/y3ppLMdU2q6uS3rCytuM9SnNkfW4MUWCfGhg5AqS_Rccww84mmbOhZEaA987X6sKJ5OGjmbtWPwmaycpUOp50Mblr3ro6DYSxgZ1sQfzlrbP4ESwIN8WmtQUf6Mhh0ERzHJ/5.png?psid=1&rdrts=144165351)

Untuk memberikan akses penyimpanan data pada mysql untuk meng-authorize, meng- accounting dan mengijikan session pada default enabled site, edit `/etc/freeradius/sites-available/default` dan hilangkan tanda comment pada baris yang mengandung `sql` di bagian `authorize{}` dan`sql` di bagian  `accounting{}`, dan `sql` di bagian `session{}`.

![Menghilangkan hilangkan tanda comment pada baris yang mengandung ‘sql’ di bagian authorize{}](https://v8pruw-ch3302.files.1drv.com/y3piLUY24hlakgQzXnBU_kYX68Gd3sjaDeKJD__W7j33umywiE1ko-SGUFmaXAFmoWVnZqKwORnxLTG-_pTUWPaG5sEzPDBjq5spFYmMgrv855P7_-p3YN3nKfKIbfVrQCm/6.png?psid=1&rdrts=144165351)

Kemudian untuk mengijinkan virtual server inner-tunnel (virtual server yang hanya mengijinkan request untuk melakukan tunneling dalam server radius sendiri) untuk me-request bertipe `EAP-TTLS` dan `PEAP`, edit `/etc/freeradius/sites-available/inner-tunnel`, dan hilangkan tanda comment yang mengandung `sql` di dalam bagian `authorize {}` dan di bagian `session {}`.

!Menghilangkan hilangkan tanda comment pada baris yang mengandung ‘sql’ di bagian authorize{} pada inner-tunnel](https://v8pruw-ch3301.files.1drv.com/y3p9gJLDZntLQdzirbEwpqmcZeGnabnO7JGZ0HjGuJB3WHKqbptww98ommuInuOzUTlt6gvoDRBAWPinj4Kq11kS59bzTOzL44h6KadAQFg9yUsSKe4aHfl_lBBy8W8ok1q/7.png?psid=1&rdrts=144162537)

Untuk meng-apply hasil konfigurasi dan mengecek apakah radius berjalan dengan baik restart service Freeradius dan kemudian stop dan start kembali service tersebut.

![Meng-apply hasil konfigurasi dan mengecek apakah radius berjalan dengan baik](https://v8pruw-ch3301.files.1drv.com/y3paBEpEbOQbPoQ3WoX3cnv3EI19zPMA-n9BJgfvb2iAYFAz56v6_BW-CRXIj6YrXgYfevZQjmoixF1XSRV8nsrnKXtVSD8qGorWzE4O-lEeo37G3RdMQyfAURUOyPJzXzp/8.png?psid=1Meng-apply%20hasil%20konfigurasi%20dan%20mengecek%20apakah%20radius%20berjalan%20dengan%20baik&rdrts=144162537)

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

![Hasil konfigurasi squid.conf](https://v8pruw-ch3302.files.1drv.com/y3pWBTHAR4R7DwAaS4sh1uSSBvRzRpkoozyeiaC3Akw_M84tk6lJgFnzdNJJu2e6u2oRb2HFpVSn_QHWhUA8fWVGv_dPKEabvbjN2azb-whfKfcTdU_Zjt7bFt57VQX1ez8/9.png?psid=1&rdrts=144162537)

Download plugin radius, kemudian ekstrak.

![Download plugin radius, kemudian ekstrak](https://v8pruw-ch3301.files.1drv.com/y3peWKDAc6WB-PWpd0QyyRcy68HptGseP3tjyMhI47mQgCpH8x-jrbXcv4SU_KldBStiXRKFma--LqawyFbEJ_iXz7j1WehDqOuFZiwArDOps8U_zUvFZzjIVQsfdBXCFlU/10.png?psid=1&rdrts=144162537)

Pindah ke hasil direktori hasil ekstraksi `squid_radius_auth-1.10`, kemudian install plugin:

![Hasil pindah direktori](https://v8pruw-ch3301.files.1drv.com/y3pGV7KSEZUAgUrqzkCX15ChoXhcwCGDZLVeQ9qaFKkx9d865rFgr3mMy-jy_BH4BZF2Lpxh_Wke31HrbkKFL-H2Ou37lhAjj9c2-DH5Zcko1fbEM8mjSG7MZy5nV7IoA12/11.png?psid=1&rdrts=144162544)

![Hasil install plugin](https://v8pruw-ch3302.files.1drv.com/y3pVcDFvdumC7Z9FkF-HNdah3o1ppDOgr12oJGRaB5bfirSNTpCbILxF0j2wHhF0_81eyP4NV7aFsqLsn48FeAE7p-df1miyQoQKCxd3nnM0vDjK7or6cm-z20A4Aj0C_sT/12.png?psid=1&rdrts=144162544)

Kemudian Copy file binari plugin radius auth ke `/usr/local/squid/libexec/squid_radius_auth`. Kemudian buat file konfigurasi plugin radius 

{% highlight bash %}
# touch /etc/squid/squid_rad_auth.conf.
{% endhighlight %}

![Meng-kopi binary plugin radius ke /usr/local/squid/libexec/squid_radius_auth. Hasil membuat file konfigurasi plugin radius](https://v8pruw-ch3302.files.1drv.com/y3pZigdUsAFSaAldBpXyY39GhNjbpuykfe1KSQZIpbDKtut0pkhZwJxDdv1xw7lY6FkvtFJBe8EFNxQlZPMltSvsv-ZfoOyXEhx4zkyUZqmTwza8RpZrCOiQdaf2Jru9_tc/13.png?psid=1&rdrts=144162544)

Masukkan konfigurasi berikut ke file squid_rad_auth.conf

![Hasil memasukan konfigurasi berikut ke file squid_rad_auth.conf](https://v8pruw-ch3302.files.1drv.com/y3puu2yGFqHZcuSlvdJiSLLkTHCkHr19fHcm-G4eflzG0BCCT3GbeEPTwG9QppuMiKnzMWlIE3yFcVjKB0xlbMvI2BFIBIE6eOq5ElhcdACIB8VuZ_PnJtHXtdGD5vSDGa7/14.png?psid=1&rdrts=144162544)

Kemudian restart service squid untuk meng-apply hasil konfigurasi.

![Restart service squid](https://v8pruw-ch3302.files.1drv.com/y3pNbzl0wZgBeFkAE6VZpq6kI-nmpaFNjMag8XC9rdgAZovjua9iHAprHxJG-RlW-cqxE__ih7_Ea_faYw341hsf26zl135JmNuNdFW59NfpzmWXwYOr9qbTY911rGPAgen/15.png?psid=1&rdrts=144162544)

Setup browser client dengan proxy alamat IP proxy server dan port 3128. Ketika Anda mulai browsing dengan browser akan muncul form login sebagai berikut:

![Hasil pengaturan proxy dan radius](https://v8pruw-ch3301.files.1drv.com/y3p9e9ky3kUVWLashoP_o7bMW9X_UUED4XGq-HLLbSXILLDotH3D5d9RsvgJq9uFOWGCZrDhpb9p_KBm9ZscmUEHIDNUdhaS3qnRCxD3FbYBYcydTkRwPUYCy1CRNO6WmCL/16.png?psid=1&rdrts=144162545)

Untuk mempermudah manajemen Freeradius, install daloradius sebagai user interface manajemen

Ekstrak daloradius kemudian pindahkan ke `var/www/`. Kemudian masuk ke direktori `../daloradius/contrib/db`. Kemudian tambahkan tabel-tabel daloradius untuk menambahkan beberapa tabel dalam database radius.

![Menambahkan tabel-tabel daloradius dalam database radius](https://v8pruw-ch3302.files.1drv.com/y3pp5TrbtzIR5-9k8dr2_desiH3_-w2T2Yk1Lko4juqtZ3mEUhAG193WdZlQ3RcyI03SVqKYKUl00M55kYLKNHwRZTnujGtwof4PoKXjPmXVEiCAx4Z8eEGKb4TNDaa0wkH/17.png?psid=1&rdrts=144162545)

Kemudian konfigurasi koneksi database ke daloradius ke `../daloradius/library/daloradius.conf.php`.

![Hasil konfigurasi koneksi database daloradius](https://v8pruw-ch3301.files.1drv.com/y3pwl2LqIHeWyQ6BPqHfm2sdK0O3EqC6lRXaF97rIxFVT2YoB3hoRUcpgeu7W-YvqaU5RpEFgJfnE6Kryv8BwZISPADmmYOITPg81qJwIILc0qIUSpCQ_W7yhmry9rrdPEw/18.png?psid=1&rdrts=144162545)

Jika mengalami error dalam konfigurasi install paket `php5-gd`, `php-pear` dan `php-db`. Berikut hasil konfigurasi daloradius:

![Hasil penambahan user](https://v8pruw-ch3301.files.1drv.com/y3pzYc-ucc1Mzd7h026BIlJmAdTbmjtAQ2fayLZlcBTX5CIm8Vdba-G4M4JYX1rScbeBS-N1_Vs-QajNy6_op73SLfGGqC4vs8LsAj4qQZ_Hz0yNgMWjIJNUArLHnJ67dPN/19.png?psid=1&rdrts=144162546)

Hasil penambahan user

Dan hasilnya adalah:

![hasilnya](https://v8pruw-ch3301.files.1drv.com/y3pRO6_vwLaaYILXND15CuyKBt7bbecMoZq1rHTTbYMJSBKuzNMlWAtazjAIqdjP929qRkuBttZ27paFhY1ZDZooAwwJMBsiuHBza2qEr0coA2UfaMnnn1BHmWAViISAoox/20.png?psid=1&rdrts=144162547)