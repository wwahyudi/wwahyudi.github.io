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

![MikroTik RouterBoard 1100](https://ajjjaq-ch3302.files.1drv.com/y3p2lnghOb6ARpZtMv0XYM0mt2uLoXQPgnhG_kjt-PX8qr4z4X-J2KFnwFpoGDboWiYz-fOvF2bvD7ClucR62UObgEjINH6yoVy-d0l_wj8fSAGRN6HndgYcit8d3gAKjf6/1.jpg?rdrts=144157009)

Pada pembahasan ini digunakan 3 NIC dengan detail penggunaan sebagai berikut:

1. eth1 dengan 10.252.108.14, IP address yang berasal dari EEPIS server (dalam hal ini, IP address ini dianggap IP publik), didapat dari DHCP EEPIS Server .
2. eth2  dengan 192.168.2.1, private IP address, DMZ connected ke Web server.
3. eth3 dengan 192.168.1.1 private IP address, jaringan privat digunakan untuk client.

![Network topologi](https://ajjjaq-ch3301.files.1drv.com/y3pSX5L4jXl3e5leckpRxCBMfYgrXtHwDpxBfgC8uJU0VbOKntlmlQD6DNYsVU8hFIxaaKU-1G3TThcliNhljvwYrTWWJg0oAjlbJIHS4y8OsOYCs-jJ7i1s5fch6QQUK3m/2.png?rdrts=144157066)

Untuk mempermudah konfigurasi, digunakan Winbox, utility untuk melakukan remote GUI ke Router Mikrotik. For windows. Berikut pengkonfigurasian yang telah dicoba:

**1.** Login pada MikroTik. Pertama buat koneksi antara MikroTik dengan EEPIS server, buat eth1 untuk mendapatkan hasil DHCP dari EEPIS server. Berikut hasil DHCP client pada eth1.

![Login ke mikrotik](https://ajjjaq-ch3302.files.1drv.com/y3pvvaoX_aT5ThMMUdYKT3IEk5IT-mE0yTHaCN7XkB_BDYPz13PqEE7MtP9PwraVMS8E_8GRq3zeBe8TcBKCLfTH4tdSlR26NSSut_yygbgfbjc69fZT8WLJDGB79VYLLM8/3.jpg?rdrts=144157389)

**2.** Jika diperlukan beri komentar pada Ethernet port yang dipakai, berikut hasil pemberian komentar pada eth port sesuai dengan ketentuan diatas.

![Tambah komen](https://ajjjaq.bay.livefilestore.com/y1ppYvFWaW-nYeq0vZyoELui9dQEJRzmo-lGi67mdoWTOXs4PWqfSNO9yA78dzMJF-8EGw8MoBINzx2hJP4kYSOqnB3_S3f5kCK/4.jpg)

**3.** Buka terminal MikroTik, pertama lihat hasil pada semua interface

![Lihat interface](https://ajjjaq-ch3301.files.1drv.com/y3pbYus886usuYspl0zSjWoxgS7H1GEXzHIVOQtfL9G0pFjssC8a00D8ZvF9Ap6RUsUZE20mBXZiBKmMZb1Cv1o8alrjuWf3rvqH5u3-VZmqvjWvlh1A5eXSI8Kj9WwwYP0/5.jpg?rdrts=144157466)

**4.** Assign ip address 192.168.1.1 pada eth3. Kemudian cek perubahannya

![Assign IP ke eth3](https://ajjjaq-ch3302.files.1drv.com/y3p6DHZbdHGN2Z43W11MaKRTzEd4yGvmOSNig0stA6LqYlTCSNotuTl6LQuJXr2LnlGkIw2Ax2oil6xdM4icnYWy1p3KwrYWJ3VHDSdnJ9cf7Af4vwnEavl8ZWHAO2lglMx/6.jpg?rdrts=144157489)

**5.** Tambahkan ip address 10.252.108.1 (gateway dari server EEPIS) sebagai gateway dari ip yang dianggap sebagai public IP. Cek hasil penambahannya.

![Tambah gateway](https://ajjjaq-ch3302.files.1drv.com/y3pmMyP2q_W0Bx9yumWQEB7kqr6iABBHkIi-lctZlo37XAF_bAoXmvpuIvJiEDhHOq6V7c2fEob8MFov5Sa3aw7878Cj-s9zGBZRRd4CcPKVZ4KFlitj1LcTxyMoWdXpaXJ/7.jpg?rdrts=144156694)

**6.** Untuk membuat DHCP server, assign DNS servernya. DNS server yang digunakan adalah DNS server dari EEPIS yaitu 202.9.85.3. Set remote remote request menjadi yes untuk menentukan apakah akan server DNS memperbolehkan network requests. Ketika remote remote request diaktifkan, router MikroTik menanggapi TCP dan UDP DNS permintaan pada port 53. Kemudian cek hasil penambahannya.

![Set DNS](https://ajjjaq-ch3302.files.1drv.com/y3pyPFSAEL727pB2MrCBS7wTBXpDJ8bV15eqPO2tL5BhgIGKc5tcYLpilBHp54MNnCuHgDWcTg90ixOrRXWUh1oxYx4sVXYTHrQixoFH0hQAkpUFB9OZRvxGSeJtzkABT_n/8.jpg?rdrts=144156694)

**7.** Kemudian buat DHCP service untuk eth3 dengan network space 192.168.1.0/24 dengan gateway 192.168.1.1 pada DNS server EEPIS yang udah diset sebelumnya. Konfigurasi lebih lanjut terlihat pada gambar dibawah ini.

![Set DHCP](https://ajjjaq-ch3302.files.1drv.com/y3pTcfLNYocEuZyx4XOpv7An5olQSPm1S23SftcnSktic9fjVbBSgWs27SW8_0mS19_w11CHHrej_c-d8Z0V9Eq7bRSE2igYTnKkRWJJceqn_KUBtBua2sb9M3F1o55ys8F/9.jpg?rdrts=144156694)

**8.** Kemudian untuk eth2 lakukan assign IP yaitu 192.168.2.1.

![Assign IP ke eth2](https://ajjjaq-ch3302.files.1drv.com/y3pauOckNTl1XWTr2Ud0Q0mSG8qJSvijNyOEf3sDZ-F0yW_eHGpGhyJ0qeIZinogy3jmR06ZxmBsXhVTeyK-i-nBfG2pXXzXXZA_f0eP73unYaO2ykgwLG5D3NTJy5qtWAU/10.jpg?rdrts=144156694)

**9.** Tambahkan source NAT atau srcnat dengan action masquerade. Scrnat adalah jenis NAT yang mentranslasi paket yang berasal dari jaringan yang telah di-NAT. Router NAT akan mentranslasikan paket yang berasal dari IP address private ke IP address publik ketika melalui router. Pengembalian paket balasan akan dilakukan dengan me-reverse operasi srcnat. Masquerading akan mengubah alamat IP sumber dan port dari paket berasal dari jaringan 192.168.1.0/24 ke alamat 10.255.108.114 dari router ketika paket yang diarahkan melalui router tersebut.

![Tambah source NAT](https://ajjjaq-ch3301.files.1drv.com/y3phlkOm_iqlmm1kaOdr0lnATtYyH3lykP-TrZMStUtRg6j8VIk_zvESyx32yOMM3edWK152rgm8DHJLzS84vSsLW-YnZWhJ3FopeL1vdl_9PHNnSiPQXAwCS3o2ftidCMj/11.jpg?rdrts=144156694)

**10.** Kemudian,juga tambahkan destination NAT atau dstnat dari semua source pada IP tujuan 10.252.108.14 pada protocol TCP kearah 192.168.2.2 ke port 80 dimana pada IP 192.168.2.2:80  sudah disiapkan sebuah web service . Dstnat adalah jenis NAT yang mentranslasi paket yang berasal dari jaringan yang mengakses IP public (10.252.108.14) ke jaringan local (private) dan mem-forward-nya ke IP yang sudah di-assign yaitu 192.168.2.2:80.

![Tambah destination NAT](https://ajjjaq-ch3301.files.1drv.com/y3p_1hfgmYEsYnO1r80Ertu9OWena2Ige0Bc06wYNt0n8djlv0YV-iI59VlamNa5zQnkWb8aTbb1c-Y7bXe_oo_SfgVpoiPGVFy6e0Yqbmp4ycd7iqhkxTYSyJQIQ3_r-C1/12.jpg?rdrts=144156694)

### Hasil percobaan pada Client

Hasil DHCP service yang diberikan oleh DHCP server MikroTik.

![DHCP yang didapat client](https://ajjjaq-ch3301.files.1drv.com/y3ptCkoE6k9AUTG-k_yDpuDfQor7uYcfdbtA81fa2rrsJzQ0srRSpIqbHPCTglfoG4KrH-uLn4sz30ni6uRv4_sQ8lbRRvjZzSkvkSVWlfnRd2sDPguqZiNPSv_ChN7t6vL/13.png?rdrts=144156699)

Hasil ping client (private network ke 192.168.1.1 dan ke IP public 10.252.108.14)

![Ping dari client](https://ajjjaq-ch3301.files.1drv.com/y3pq95iRXkHguiplYGHH3ibmsPbXqqlnbQKFLN-gFRe-uwAtdKM_htetDmCBqP0HINdNOOXRSFPy9uY0AKzgR6mTz7Jp4q8iQQEPX9WvTtgRnTSBd8F-Bi-wgQoHcrzi_M9/14.png?rdrts=144156699)

Hasil akses client ke website luar – website EEPIS

![Akses ke website luar](https://ajjjaq-ch3302.files.1drv.com/y3p2WnVzAA0kGZroES6BJeuTz5N0lJpwDrnwj0XcInWclTy_f6yMWVhMYUIQLtEfVXJbui5pITNRZ0_AV2f7B0JuNTzoP1tcz9lRnexbR_b_EAAghBowM7sNhIMkKFfThlg/15.png?rdrts=144156699)

Ketika IP public diakses, maka langsung di forward ke 192.168.2.2 bukan mengakses 192.168.1.1/webconfig

![DMZ portal](https://ajjjaq-ch3301.files.1drv.com/y3pqfVmXMGNK_ftHDnAo88DYz0xzPJrWCZ-ueDhLEc0-JEXW-Pn0PAcmqBA8V62-usLR-hV0rAzb4EZVZwvcmVgRqcoHcYT8i8apExbawb28OUp0FCRyeZBS56Ntmj1pdgN/16.png?rdrts=144156699)

### Kesimpulan

DMZ adalah suatu area  bagi hackers yang digunakan untuk melindungi system internal yang berhubungan dengan serangan hacker (hack attack). DMZ bekerja pada seluruh dasar pelayanan jaringan yang membutuhkan akses terhadap jaringan “Internet atau dunia luar” ke bagian jaringan yang lainnya. Dengan begitu, seluruh “open port” yang berhubungan dengan dunia luar akan berada pada jaringan, sehingga jika seorang hacker melakukan serangan dan melakukan crack pada server yang menggunakan sistem DMZ, hacker tersebut hanya akan dapat mengakses hostnya saja, tidak pada jaringan internal. Secara umum DMZ dibangun berdasarkan tiga buah konsep, yaitu: NAT (Network Address Translation), PAT (Port Addressable Translation), dan Access List. NAT berfungsi untuk menunjukkan kembali paket-paket yang datang dari “real address” ke alamat internal. Misal : jika kita memiliki “real address” 10.252.108.14, kita dapat membentuk suatu NAT langsung secara otomatis pada data-data yang datang ke 192.168.1.1 (sebuah alamat jaringan internal). Kemudian PAT berfungsi untuk menunjukan data yang datang pada particular port, atau range sebuah port dan protocol (TCP/UDP atau lainnya) dan alamat IP ke sebuah particular port atau range sebuah port ke sebuah alamat internal IP. Sedangkan access list berfungsi untuk mengontrol secara tepat apa yang datang dan keluar dari jaringan dalam suatu pertanyaan. Misal : kita dapat menolak atau memperbolehkan semua ICMP yang datang ke seluruh alamat IP kecuali untuk sebuah ICMP yang tidak diinginkan.

Network Address Translation(NAT) berfungsi untuk mengarahkan alamat riil, seperti alamat internet, ke bentuk alamat internal. Misalnya alamat riil 10.252.108.14 dapat diarahkan ke bentuk alamat jaringan internal 192.168.2.1 secara otomatis dengan menggunakan NAT. Namun jika semua informasi secara otomatis ditranslasi ke bentuk alamat internal, maka tidak ada lagi kendali  terhadap informasi yang masuk. Oleh karena itu maka muncullah PAT.

Port Address Translation(PAT) berfungsi untuk mengarahkan data yang masuk melalui port, sekumpulan port dan protokol, serta alamat IP pada port atau sekumpulan post. Sehingga dapat dilakukan kendali ketat pada setiap data yang mengalir dari dan ke jaringan.

Access List melakukan layanan pada pengguna agar dapat mengendalikan data jaringan. Daftar Akses dapat menolak atau menerima akses dengan berdasar pada alamat IP, alamat IP tujuan, dan tipe protokol.

Pada percobaan, tidak dilakukan access list, namun percobaan ini dapat dikatakan sebagai DMZ sederhana yang diimplementasikan pada sebuat MikroTik Router.