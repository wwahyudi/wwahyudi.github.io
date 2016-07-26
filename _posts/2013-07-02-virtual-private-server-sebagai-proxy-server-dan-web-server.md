---
layout:     post
title:      Virtual Private Server Sebagai Proxy Server dan Web Server
date:       2013-07-02
summary:    Konfigurasi proxy Squid dan apache di Lab
categories: linux
---

Pembahasan kali ini merupakan pengaplikasian konfigurasi jaringan komputer yang menggunakan sebuah virtual OS sebagai server yang memiliki 2 network line. Target pratikum adalah membuat Proxy Server dan Webserver (http dan https). Modul pratikum yang digunakan adalah Virtual Server dengan OS Debian 6 dan router MikroTik RouterBoard 1100.

### Skema Jaringan

![Skema jaringan yang diterapkan](https://9tehha-ch3301.files.1drv.com/y3p6olFOc2wDh0EOBw7jFhDfq1rLuXjiM7BlM7h0qf8RkIygaVQSryBrkm_5ymXPJJwWKmPDiZfiMBptIhAr5MRT9mGbnn1h4D-HvkgLCuVdWsDCC0EYa5dRVMephPMK2j0/1.png?psid=1&rdrts=144162803)

Skema jaringan yang diterapkan

### Persiapan

#### Assign IP Server

Untuk men-assign static IP address untuk eth0 dan eth1 edit file pada /etc/network/interfaces. Sesuai skema eth0 akan di-assign IP 10.252.108.85 sedangkan eth1 akan di-assign IP 192.168.1.85. eth0 di-assign dengan tujuan sebagai remote address untuk SSH dan penghubungan dengan [debian mirror kebo](http://kebo.pens.ac.id/) sedangkan eth1 menggunakan gateway untuk mengkaitkan server ke node router 192.168.1.x. Berikut hasil konfigurasi file interfaces:

![konfigurasi file interfaces](https://9tehha-ch3302.files.1drv.com/y3pYL4CYI69vhquI41W070JCPpfZyMPIbmVE9NluNc67avEqhxFyXXaiWhKEFDV33Rgs7vkGPY0LZSpYmaZ6IYmgW6XkwkF7gx-oY2ZmyFCitRIbxDwFZov7y_rlZ-ZWxO-/2.png?psid=1&rdrts=144162803)

Setelah di-set, restart service networking.

Untuk tetap terkoneksi dengan debian mirror kebo, set nameserver /etc/resolv.conf ke DNS IPv6 PENS yaitu 2001:df0:a8:85::3 dan 2001:df0:a8:85::4. Berikut hasil konfigurasi file resolv.conf:

![konfigurasi file resolv.conf](https://9tehha-ch3301.files.1drv.com/y3pNP_30Turo8I6aHQlVGDB_hT4vhEciQV8kZwH7fwdcQsGiRrWSneFCI7Fg05IN0f0Hkp4emr3vtfnCa3CqyhppkFUWVV2SHffo4oSiPi1mPCEyhMMTJy3YRWc8a8bRikl/3.png?psid=1&rdrts=144162803)

Ketika server dapat menghubungi server debian mirror kebo sehingga dapat meng-update atau menginstal paket yang dibutuhkan, maka assign ip server berhasil dikonfigurasi.

#### Menginstal ssh server

Untuk mempermudah akses ke server, install ssh server dengan perintah 

{% highlight bash %}
# apt-get install ssh
{% endhighlight %}

Berikut hasil instalasi:

![Hasil instalasi ssh](https://9tehha-ch3302.files.1drv.com/y3prmnJpmVtLJ0MCRurrdJayZjHE7EhXw05jBAfEGJGWn_vZMoPRxO_GkOKsnrDi-JLG2bwgB_bzQLrt5YCPf1FU7W2qmMKSCql4TtGcO2-m1gsIC77v0C0nwtk1Sk3oEyg/4.png?psid=1&rdrts=144162803)

Untuk melakukan remote ssh, lakukan dengan perintah #ssh ip_address_server, contohnya 

{% highlight bash %}
# ssh 10.252.108.85
{% endhighlight %}

Ketika server dapat di-remote ssh, maka instalasi ssh server berhasil dikonfigurasi.

### Mengkonfigurasi Webserver Server

Pada kesempatan ini, target webserver ini adalah webserver yang dilengkapi fitur ssl. Hal yang pelu dilakukan pertama adalah menginstal apache2 dan openssl. Menginstal dapat menggunakan perintah #apt-get install apache2 openssl.

![Hasil instalasi apache2 dan openssl](https://9tehha-ch3301.files.1drv.com/y3p4v997S_z5Lcy4tnuFnf7knzkHo1QgQU3hqEkiZR0xf71LJaLKKYY8-7Gdfn8D3BRstvLJsK6qxuSaesiFi6SCcPjzAt9kJ1F_HO4HW-BtsBzHW1qqXKi05kXLh-VaHi0/5.png?psid=1&rdrts=144162803)

Kemudian untuk membuat web dengan koneksi ssl maka diperlukan sebuah sertifikat ssl. Untuk membuatnya gunakan openssl untuk membuat self-signed ssl certificate. Berikut hasil pembuatan sertifikat ssl:

Membuat rsa key:

![Membuat rsa key](https://9tehha-ch3302.files.1drv.com/y3p7jUUYJJ2Ik7dH4_2uyRb9QsgvD35LDbsgeCGTvymiM_yxWPeAqTMY628IwZoxAlcHL-S-a8m4AH0fpXIUVAAzXIC3joe5ocfn0-_XOr736Rr1GS2_GDXrFIVQqdRkyb4/6.png?psid=1&rdrts=144162803)

Membuat certificate identity:

![Membuat file certificate](https://9tehha-ch3302.files.1drv.com/y3p3hUdIC3XMZJV40N9TU8IJkjOcAjlC2OvSVl71fPZPGioXTjKgANmqx45K5lHxOcUaqs_p57gc9JeUgXCrQPrU5T7mD8K6lCvPGdVvJrXs0FEIXlRDsL-eUAFl29Ax-eh/8.png?psid=1&rdrts=144162250)

Untuk membuat localhost memakai ssl certificate maka edit file /etc/apache2/sites-available/default-ssl. Pastikan bahwa SSLEngine On dan ubah SSLCertificateFile dan SSLCertificateKeyFile ke sertifikat dan key yang telah dibuat sebelumnya. Berikut hasil pengubahan SSLCertificateFile dan SSLCertificateKeyFile pada file default-ssl:

![Hasil pengubahan SSLCertificateFile dan SSLCertificateKeyFile pada file default-ssl](https://9tehha-ch3301.files.1drv.com/y3pTvNsr7tKFZyQJFBsZPs4Wq_R9O5Oqn_41LO1-mzspm-KYGVeIoc2BTyoFsNGHOcF3HxggYnfkFth40upof7yba_ScLBb-Te65k2wFfuryHOUU9hQQO9wq3aPdjBwOnhT/9.png?psid=1&rdrts=144162250)

Hasil pengubahan SSLCertificateFile dan SSLCertificateKeyFile pada file default-ssl

Kemudian enable-kan modul ssl dengan perintah 

{% highlight bash %}
# a2enmod ssl
{% endhighlight %}

![enable ssl](https://9tehha-ch3302.files.1drv.com/y3pmFasurEWPwKZxujs1NuQjfixytjfDDd-rBRlO_hsGFo5K_wi6DL40PJLmLICXlgtQR3H9iEIBITCeJreleHaEuJXvLTyWkKoBreE9pq2D_Uy_AOR7XDKL-WlbFE3d-Qw/10.png?psid=1&rdrts=144162250)

Kemudian masukan default-ssl sebagai site yang di-enable-kan dengan perintah 

{% highlight bash %}
# a2ensite default-ssl
{% endhighlight %}

![Mengaktifkan default-ssl](https://9tehha-ch3302.files.1drv.com/y3pxoTuxAZon8pxvyj0QL3sjCMtwQVzd-jfWMp4M47-eYHUDp75gp_9T-PxOTka4fAMhZo6mTkByhMpcYIEzgYvAayrxwMdY-x1B4kQUQipdOngvvrzTBaR-OnubEjyExti/11.png?psid=1&rdrts=144162250)

Terakhir, restart services apache2 mengunakan 

{% highlight bash %}
# /etc/init.d/apache2
{% endhighlight %}

![Hasil restart apache2 service berhasil](https://9tehha-ch3302.files.1drv.com/y3pgTt1oZ3myKyGSfhmJ_1-mCbLzf70oJPrsvFOqV3opZ915u3CoOYqCEIGCG8wm96WzDzV1H3DXdza3THF4SRXn0UD5goIo661QCAMZicFBwfDrFQjS2X7XrtOMPmU5VBs/12.png?psid=1&rdrts=144162250)

Dan hasilnya:

![http://localhost diakses](https://9tehha-ch3301.files.1drv.com/y3pu2QUj2d41p9A9KZwvWWcyQoWU6gnmwVFT4kXR-woaJW7z2HZRqp8O8e8_5lSGumrOrNXTo2-M8ApbL0dw6D3b0T8rJSQxN8TSG2W6h4dLen-PZKq3tkKcVORdf5Zhhh4/13.png?psid=1&rdrts=144162251)

http diakses

![https://localhost diakses](https://9tehha-ch3302.files.1drv.com/y3pQ9Gl2D5yiKCVGnKFXCkTzvLihoyY0RPBT9Z8TKz1ZSr0vntHHuKFPSoxPO23q7aq0-k6lQPrhqRqC4ixKakIwQedRcoQ_hJMutUy9u9cexZYNBlg18SSIx7VWp-U2zEu/14.png?psid=1&rdrts=144162251)

https diakses

Ketika kedua halaman tersebut dapat diakses, maka konfigurasi webserver dengan ssl berhasil dikonfigurasi.

### Mengkonfigurasi Proxy Server

Konfigurasi selanjutnya adalah Proxy Server. Proxy software yang digunakan adalah squid. Untuk memulai konfigurasi Proxy server, install paket squid dengan perintah #apt-get install squid. Setelah itu, untuk membuat squid secara otomatis mendeteksi sistem hostname server maka set visible_hostname pada /etc/squid/squid.conf. Tujuan menambahkan hostname pada visible_hostname adalah membantu squid untuk memberikan direksi pada squid kemana squid harus memberikan error message, dan lain sebagainya. Berikut hasil pengisian hostname pada /etc/squid/squid.conf:

![pengisian hostname pada /etc/squid/squid.conf](https://9tehha-ch3302.files.1drv.com/y3pY2PTFg1Mj0EEG4y9HdVCjkjog-TpMsnWQp5ql5UXCi4AgpqYQok1Je9PyjAa9Fl8MgarH5_YOAEOb_cOFcGE7BZwaA2K5Jrp2wRjzZIDfD5W3ebF8zn3XHn2h5HIuPjC/15.png?psid=1&rdrts=144162251)

Kemudian jalankan perintah #squid –z. Perintah tersebut digunakan untuk membangun atau rebuild direktori cache dan swap directories untuk squid. Berikut hasil eksekusi perintah #squid –z:

![hasil eksekusi perintah #squid –z](https://9tehha-ch3301.files.1drv.com/y3p3pjcEY6sZdgnYjYgDZ4k1snc_tHs6mTfmNH9lARU8_8clStp5XnD7iqFw8VbYKqVX_WwTZ4ZpaXnlrBuYBuPBFOV8P83PDj6fQEqbdz33Ax7vXLDxj47Ny_jqlGg6Jhi/16.png?psid=1&rdrts=144162251)

Kemudian tambahkan ACL (Access Control List) sesuai dengan aturan yang sudah ditentukan, dimana segmen 10.252.108.x tidak dapat mengakses internet, segmen 192.168.1.x dan 192.168.24.x dapat mengakses internet dengan autentifikasi user, dan sisanya tidak dapat mengakses internet.  Berikut hasil pembuatan ACL pada /etc/squid/squid.conf:

![hasil pembuatan ACL pada /etc/squid/squid.conf](https://9tehha-ch3301.files.1drv.com/y3pYwHYiBTwSfXf6S3kwYMtc9ui-84KXf0sJ5tuqgnFSyEUlQBhaPzgyetLXEtTJFYEiSJeFUsPUUY2AdBYhNz2dSbfqPb24S3uRn1XqcMJFAC36nDh59xaDoeCypLdNct1/17.png?psid=1&rdrts=144162251)

Setelah itu restart service squid untuk menerapkan hasil konfigurasi pada /etc/squid/squid.conf.

![restart service squid](https://9tehha-ch3302.files.1drv.com/y3pI1NnXmLyC9BiMMm_-10jhVC3Wa4Q1Zhzxvmlzb1JBCRa-x_nQunPUhXE9Q70xvcE51zNIjpqJpDOt7YrUIOpK_BOmi39i-ogsHcn9EIDpcHffg6PM0S3lMS6vrjGAC1m/18.png?psid=1&rdrts=144162251)

Kemudian buat user auth list dengan perintah #htpasswd -c /etc/squid/password nama_user. Berikut contoh pembuatan user autentifikasi untuk squid:

![contoh pembuatan user autentifikasi untuk squid](https://9tehha-ch3301.files.1drv.com/y3pQq2XWoDozSh1ut4IDKyQLpqy-IzzNlWRIRwShayqCh2SvMhIKnwy2PiO3acZFVL5cFTHGK1fK2mEJiSe0nFsfXjVdOg9JV2ZNuRACbGWMcjnIZ-UUhx2Q95J0rphh0qt/19.png?psid=1&rdrts=144162251)

Dan hasilnya

![browser yang sudah diset proxy-nya akan mngeluarkan autentifikasi seperti gambar diatas](https://9tehha-ch3302.files.1drv.com/y3pc0cTtqq_0QzHuDEeFv6aIe0tOksP674KI-CIfvmyRA0wTaH_kCy1xdGhbV9I1CQ4OGHi56Lqc-SBjB7kfwLwJ1o468w6AhGT3hcYEo2iGMIiYwkz2Hdwzg58uZp-uRfY/20.png?psid=1&rdrts=144162251)

Browser yang sudah diset proxy-nya akan mngeluarkan autentifikasi seperti gambar diatas

Ketika proxy dapat berjalan pada jaringan lokal yang diuji, dan pada jaringan yang diblok tidak dapat mengakses interner, maka squid proxy server sudah dapat dikonfigurasi dengan benar. Kerja proxy juga dapat dicek pada /var/log/squid/access.log. Berikut gambar hasil akses proxy:

![Hasil akses proxy klien pada /var/log/squid/access.log](https://9tehha-ch3301.files.1drv.com/y3p56GjIyLrAw-aJIglEmtCdq5G4gsqeHc2FpL8J10-w3SeDJuHdf57yOID9hBCDOZBuNKwO0N-YPmf1g2qoJyCl05-5GxYTa2ajcqi45EbtIXTBrJIzkAHN2ga6d2c8qEM/21.png?psid=1&rdrts=144162251)

Hasil akses proxy klien pada /var/log/squid/access.log

### Mengkonfigurasi DNS server

Untuk mengatur aliasing nama ip dan name resolution diperlukan sebuah DNS server. DNS server yang diinstal adalah BIND. Untuk menginstal DNS server, jalankan perintah #apt-get install bind9. Kemudian file /etc/resolv.conf perlu dikonfigurasi untuk menseting komputer yang digunakan sebagai DNS server, maka option nameserver diisi dengan nomor IP address komputer tersebut. DNS server ang akan diseting berdomain kecut.com. Berikut hasil seting file /etc/resolv.conf:

![hasil seting file /etc/resolv.conf](https://9tehha-ch3301.files.1drv.com/y3pCbGiOwFUzLz8TeAD6ijS3d155exd4BqbjJ4UDk0EjKI4t78szc9Y8X0vZS2calj7KHNKSSJjyurHiRKfBUwLXNQFhLtNdSmoyVGn-kFo44Z48tnRPmg86b-qFjHRFT_T/22.png?psid=1&rdrts=144162252)

Setelah itu copy template database ke /var/cache/bind gunakan perintah #cp /etc/bind/db.local /var/cache/bind/kecut.com.db dan #cp /etc/bind/db.255 /var/cache/bind/kecut.com.rev. meng-copy selesai, edit database file menggunakan perintah #nano /var/cache/bind/kecut.com.db. Berikut hasil edit dari file database:

![hasil edit dari file database](https://9tehha-ch3301.files.1drv.com/y3pDZ107xdXEtox8Wph67jBulRADrTMfzf0qLh-uaJEC81stXds6ki-1PFotwEemgqG3sQ5UoxNybkVzfYm61OLuNtbre1oSNs0qAPYIMthuO4MpynMrujPUm9imYMnR3bh/23.png?psid=1&rdrts=144162252)

Edit juga file reverse menggunakan perintah #nano /var/cache/bind/kecut.com.rev. Berikut hasil edit dari file reverse:

![hasil edit dari file reverse](https://9tehha-ch3302.files.1drv.com/y3pZxjS2wNvKDB--F-GSSPB0b7dtZjNg9wLQdS4SN45TgmPNcxjP75_CD3TxKwNN2cMznpKsnkXQJqfOCR3u-LuUkD8EpVqd-zSWS3jpaSqcKl3jMChwcW8fLl4Zvv063Lq/24.png?psid=1&rdrts=144162253)

kemudian edit file konfigurasi named.conf.local dengan perintah #nano /etc/bind/named.conf.local arahkan database dan reverse file ke file yang sudah dibuat di /var/cache/bind. Juga tambahkan allow-update untuk dhcp sehingga server DHCP dapat secara dinamis memperbarui catatan DNS record yang dibuat. Berikut hasilnya:

![Hasil edit file /etc/bind/named.conf.local](https://9tehha-ch3301.files.1drv.com/y3p4wA3MPvnKq2Jz6w3WzIgoWdGG8Opu3M3dm8JuWbLwkOj0-11RwgvtrkWhbaR7yhcyBMLoTsu0d4RHuwByz92_zAlB9-EV5rRAvmvVmbV75Rr3hMg-Y3WVeZNKrcJdC7V/25.png?psid=1&rdrts=144162252)

Juga edit file konfigurasi named.conf.option dengan perintah #nano /etc/bind/named.conf.option arah dns forwarder ke 180.131.144.144 dan 180.131.145.145.

![Hasil edit file /etc/bind/named.conf.option](https://9tehha-ch3302.files.1drv.com/y3pvHpUm1PkuOPBH8oK25RiVUChdgWU6SVtQMRHwVuezy44UY1BvUApmcc9S7o6Set-sFMXJImTyvr8szTRt5zE6epw3GXO_Pi0fANL_ca-fS7hf1mLD34_Ka6kLU_3a3C1/26.png?psid=1&rdrts=144162252)

Terakhir, restart services bind menggunakan perintah #/etc/init.d/bind9 restart.

![Restart service bind](https://9tehha-ch3301.files.1drv.com/y3pnUeXvYaPlcXDP55eulTNdYIJbWGhXLXdfy1mvjWWHbVNJwphBcU3W5XNt-GfexJhRHv9O9zUIjo1KX5JyJD8Ry-GWmyE_dLBfJrhfy6nmH-VawuKDD7Hc9QV68STFx_H/27.png?psid=1&rdrts=144162252)

Hasil set up DNS dapat dicheck menggunakan dig

![nslookup ke www.kecut.com dan nslookup ke ip 192.168.1.85](https://9tehha-ch3301.files.1drv.com/y3pYwVYKBD_GH6y3olHhznXcMtCYunh6JYb639VXhUHebJISlVLxVejI9nj00b7-mb5SAZzWOzsk_hE0LB6fdUurwbnniiLGeZEyXmS4j1u1gQ4KlJDLOfqGsV0TZ4Q1AZ1/28.png?psid=1&rdrts=144162252)

nslookup ke www.kecut.com dan nslookup ke ip 192.168.1.85

Ketika nslookup mengembalikan return nilai sesuai dengan setting yang dilakukan sebelumnya, maka dns server sudah dapat dikonfigurasi dengan benar.

### Mengkonfigurasi Router

Router yang digunakan adalah router MikroTik RouterBoard 1100. Pada router ini akan digunakan sebagai nat router. Tersedia 13 ethernet. eth1 digunakan sebagai penyambung ke jaringan 192.168.1.x, selanjutnya eth2-eht12 digunakan untuk klien (eth13 untuk pxe boot console). Berikut konfigurasi yang dilakukan:

Pertama assign ip 192.168.1.233 ke eth1, dapat dilakukan menjadi static 1 dengan perintah ip address add address=192.168.1.233 atau dengan mengassignnya sebagai dhcp client jaringan 192.168.1.x.

Kemudian, tambahakan ip static juga ke eth2, dengan alamat 192.168.24.1/24.

![Hasil assign ip ke eth2](https://9tehha-ch3301.files.1drv.com/y3pVsJ5feAlfbFNsh0Wn9dzJqLrpArOsb2P7OG1VJElg3JOzWnLssZUZjuvbaaweA7Z61Bq3PXoYMYd4BVyvJn4uDEueFr4JqHnQCtx-CyvZsR6F8aNlUeAMIlGH7BhbpWO/29.png?psid=1&rdrts=144162253)

Setelah itu tambahkan route gateway address 192.168.1.1.

![Hasil menambahkan route gateway address 192.168.1.1](https://9tehha-ch3302.files.1drv.com/y3pmaO96hfy6PUbX--O7qiKdq567FXgR1QdvIJbtTIoYSw6RW99geXMqOQB7qgX11Bqggemj6yFEmqaRBVvJEKmYbm9Gjq01FHZWb8wjKEm5dEO0lUh4QflTN3JzBthT8kS/30.png?psid=1&rdrts=144162253)

Langkah selanjutnya set dns router ke dns yang telah dibuat sebelumnya yaitu 192.168.1.85 dan beri kemampuan untuk mengijikan remote request.

![set dns router ke dns yang telah dibuat dan mengijinkan remote request](https://9tehha-ch3301.files.1drv.com/y3p5NM2NSpDV518G0YdqHJdSvTsOgFo1Go-BmoNp-VmKbB904SHewh6Oq4WOylisfbZqc6HMgmAt8qXRbl7R_qSsKD2P9FzpqQRwsMWccgzSVOdcgF-ctEo-LN9qVV97c-3/31.png?psid=1&rdrts=144162253)

Untuk memudahkan pengalamatan ip ke klien buat dhcp server pada mikrotik.

![set dhcp server pada mikrotik](https://9tehha-ch3301.files.1drv.com/y3pYj-47CtroDCQZkEkBMsl9Fuh_3Z7iVsK4Ngf4D59pB8nzIEcaNAkse-6MtP7-fr0YhsYB-I-dGc-UEGbVi9fvI-O4H8AnFzlORAhzdQJLwXJrSJ_vTAtXovHiIYEDByJ/32.png?psid=1&rdrts=144162253)

Terakhir buat scr nat dengan masquerade pada eth1

![set scr nat dengan masquerade pada eth1](https://9tehha-ch3301.files.1drv.com/y3pHBAgcsWbyrG4r1QPQ_CoUdc88JDHTXs3Y9DuRZzTuqHYu0fVLgoWXkVCNuCFmTpiLj5qHwUY4L-xGjHpDKJzMSs_qkSPIi23NJ4gChT-E-BvL01tJTFKgdktB2WiHdIH/33.png?psid=1&rdrts=144162253)

### Uji Coba dan Hasil

Berikut hasil uji coba

#### Uji DHCP mikrotik untuk klien

![Hasil assign IP DHCP dari mikrotik](https://9tehha-ch3301.files.1drv.com/y3p7kcjufEvnxnJJ-MyMBpNDA7TQzOBleOMCDMuV4Mh9bQU5uWF9l76WzgFrfautSHGqjhtkVApexvCsh5R9AiUqyiKE0t8xqe14s5UD759IXInMOSVUuFoRL03UHM0C8oB/34.png?psid=1&rdrts=144162253)

#### Uji DNS server

![Set resolver](https://9tehha-ch3302.files.1drv.com/y3pUGIwfKm0vAAL_E4gQgsoqMcN6KR8W1FBCdu0FkeUJzJvO9i-QGwZxaJHZTEqG32JZi1DfaTUihymsW7N6qL9p4Kizeglf5VZS8n5k9qp7nxYOb-N_aG2wcr3QpyAcB1R/35.png?psid=1&rdrts=144162253)

Karena allow-update untuk dhcp diijikan sehingga server DHCP dapat secara dinamis memperbarui catatan DNS record pada /etc/resolv.conf pada klien

![nslookup ke www.kecut.com dan nslookup ke ip 192.168.1.85](https://9tehha-ch3302.files.1drv.com/y3poQzu8c3DVPIio1tqDrnukFq_VATLVpknmaeqnAbtCV64TYQkrifK1InG0Zbhhp9yfdZL8T8P8XcdF9VEJMz1ajyZ3EpRwJm-wk89fjgbvmyMBcaWIHsoIOJkpossRF7Y/36.png?psid=1&rdrts=144162253)

#### Uji webserver dan dns pada browser

![Hasil uji memanggil site pada webserver](https://9tehha-ch3301.files.1drv.com/y3pEcY08d8UIgfHCuygCTZtIoDOqmSVSaTBEbXmTOlECe-Skdj0upS-QEcR4S03aoCUOOXbgNcNvCiqMx-K_mR0_LLfEKiW0L13y8JBajnU6hgm0Z_TyXqytH0v7nuqoozW/37.png?psid=1&rdrts=144162253)

Coba buka web

![konfigurasi proxy pada client](https://9tehha-ch3302.files.1drv.com/y3pihzE7FXjmqIKfK1RU5wV7pO1CxW7J_KvTwC-DZAZAcA-86rVLIAsxM4HPfcu7yscqJVxqRZ38IJlNAxyxPUx609XsbXJd_abpD4NRIhd15XC7ShyE9dM_XCJ5KaskHwE/39.png?psid=1&rdrts=144162254)

Konfigurasi proxy pada client

![browser yang sudah diset proxy-nya akan mngeluarkan autentifikasi seperti gambar diatas](https://9tehha-ch3301.files.1drv.com/y3pviC69d7gRKBew7_M4clO2y7cnxniSODKOIp3C7NaLyAswnjSB0YJJb8kE4qcgllnnIAmfQWZgSzl61D8rWHzJ5bwHLdr8mI-dzptMFbNbORsLgm_qg1LDxvXBjn3HOsf/40.png?psid=1&rdrts=144162253)

Browser yang sudah diset proxy-nya akan mngeluarkan autentifikasi seperti gambar diatas
