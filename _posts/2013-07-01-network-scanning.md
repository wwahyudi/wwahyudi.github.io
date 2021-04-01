---
layout:     post
title:      Network Scanning
date:       2013-07-01
summary:    Network Scanning menggunakan nmap di Lab
categories: network
---

### Network scan

##### Jalankan nmap dengan menggunakan option : -sL

Cek jumlah host yang beroperasi

![nmap –sL](//sapikuda.com/images/posts/2013-07-01-network-scanning/1.png)

Perintah nmap –sL ke 10.252.108.1. –sL digunakan untuk menampilkan semua alamat dalam cakupan target yang ditentukan, dan tidak mengecek apakah host up atau sedang down namun perintah ini me-lookup reverse DNS. Jumlah host yang beroperasi sebanyak 28 host.

##### Jalankan nmap dengan menggunakan option : -sP

Menampilkan host dalam range IP tertentu dalam merespon ping

![nmap –sP](//sapikuda.com/images/posts/2013-07-01-network-scanning/2.png)

Perintah nmap –sP ke 10.252.108.1. –sP digunakan untuk menampilkan host dalam range IP tertentu dalam merespon ping. –sP mendeteksi computer yang online bukan port yang terbuka

Host yang beroperasi 0 host, dimungkinkan ada kesalahan saat praktek dikarenakan subnet pada ip yang discan tidak tercantum. Terdapat perbedaan perbedaan pada hasil option –sL. Perbedaaanya terdapat pada, -sL men-scan semua alamat dalam cakupan target yang ditentukan, dan tidak mengecek apakah host up atau sedang down namun perintah ini me-lookup reverse DNS sedangkan –sP menampilkan host dalam range IP tertentu dalam merespon ping. –sP mendeteksi computer yang online bukan port yang terbuka.

##### Tambahkan option “-v” 

–v memberikan keterangan keadaan host itu sekarang berdasarkan ping yang dikirimkan

![nmap –sP –v](//sapikuda.com/images/posts/2013-07-01-network-scanning/3.png)

Perintah nmap –sP –v ke 10.252.108.1. –sP –v memberikan keterangan keadaan host itu sekarang berdasarkan ping yang dikirimkan

Perbedaaannya adalah pada –sP memberikan hasil bahwa ip tersebut tidak discan karena tidak membeikan reply ping, sedang –sP –v memberikan keterangan keadaan host itu sekarang berdasarkan ping yang dikirimkan.



### Hosts scan

##### pemindaian ke alamat host: scanmap.eepis-its.edu dengan memakai teknik pemindaian -F dan -sV

![Hasil perintah nmap –F ke scanmap.eepis-its.edu](//sapikuda.com/images/posts/2013-07-01-network-scanning/4.png)

Hasil perintah nmap –F ke scanmap.eepis-its.edu

![Hasil perintah nmap –sV ke scanmap.eepis-its.edu](//sapikuda.com/images/posts/2013-07-01-network-scanning/5.png)

Hasil perintah nmap –sV ke scanmap.eepis-its.edu

Opsi –F pada nmap lebih cepat menscan port yang terbuka dikarenakan opsi –F men-scan well-known port yang sudah terlist pada nmap_sevices. Hasil opsi –F memberitahukan bahwa scanmap.eepis-its.edu merupakan suatu virtual OS dengan beberapa port yang terbuka.

Opsi –sV digunakan untuk Version Detection yang artinya opsi –sV mengumpulkan informasi tentang layanan tertentu yang berjalan pada port terbuka, termasuk nama produk dan nomor versi. Informasi ini dapat menjadi penting dalam menentukan titik masuk untuk serangan. . Hasil opsi –F memberitahukan bahwa scanmap.eepis-its.edu merupakan suatu virtual OS dengan OS linux beberapa port dengan versi dan service yang terbuka.

### Hosts scan dengan menggunakan script yang telah disediakan oleh nmap

##### Dengan menggunakan  manual dari nmap 

Scripts nmaps disimpan di `/usr/share/nmap/scripts`. 

![Direktori tersimpannya scrip nmap dalam system](//sapikuda.com/images/posts/2013-07-01-network-scanning/6.png)

Penggunaan dari scripts (-sC)

![Penggunaan dari scripts (-sC)](//sapikuda.com/images/posts/2013-07-01-network-scanning/7.png)

Direktori tersimpannya nmap ada pada /usr/share/nmap/script.

Opsi -sC hanya menjalankan nmap scan menggunakan default script saja atau setara dengan parameter –script=default. Untuk menjalankan script lain dapat menggunakan banyak parameter lain seperti:

–script <filename>|<category>|<directory>|<expression>[,…] untuk menjalankan script tertentu

–script-args <args> untuk memberikan argument pada script tersebut

–script-args-file <filename> untuk memberikan argument pada file script tertentu

–script-help <filename>|<category>|<directory>|<expression>|all[,…] untuk menjalankan help script tertentu

–script-trace parameter ini mirip dengan –packet-trace, namun bekerja pada level aplikasi bukan pada  packet by packet

–script-updatedb digunakan mengupdate database script ditemukan di script / script.db yang digunakan oleh Nmap untuk menentukan default scripts and categories yang tersedia

##### Pemindaian ke alamat host: scanmap.eepis-its.edu dengan menggunakan opsi –sC

Hasil nmap –sC ke scanmap.eepis-its.edu

![Hasil nmap –sC ke scanmap.eepis-its.edu](//sapikuda.com/images/posts/2013-07-01-network-scanning/8.png)

Memiliki karakteristik yang hampir sama dengan opsi –F atau –sV namun –sC menjalankan nmap dengan menscan semua port (opsi –F dan –sV tidak menscan semua port, hanya well-known port saja). Opsi –sC menscan menggunakan default script yang ada sehingga opsi ini akan menghasilkan hasil yang lebih detail (dan juga lebih lama) semua OS detail, hingga keterangan-keterangan service yang berjalan akan ditampilkan sesuai dengan script default yang dipasang. Pada port 80 pun dapat mendeteksi apakah ada interface HTML yang diset disana. Bahkan sshhotkey dari sebuah service SSH ditampilkan pula. Juga pada imap, capabilities dapat dilihat dari situ.

### Lain-lain

Nmap dengan menggunakan teknik TCP half open atau dengan menggunakan opsi –sS.

Hasil nmap –sS ke scanmap.eepis-its.edu

![Hasil nmap –sS ke scanmap.eepis-its.edu](//sapikuda.com/images/posts/2013-07-01-network-scanning/9.png)

Opsi –sS sering disebut juga dengan SYN scanning (juga dikenal dengan nama half-open, atau stealth scanning). Ketika sebuah koneksi TCP yang dibuat antara dua sistem, sebuah proses yang dikenal sebagai three way handshake terjadi. Ini melibatkan pertukaran tiga paket, dan menyinkronkan sistem satu sama lain (yang diperlukan untuk koreksi kesalahan dibangun ke TCP. Sistem memulai koneksi mengirimkan sebuah paket ke sistem yang ingin terhubung. Paket TCP memiliki bagian header yang berisi flag (penanda). Flags menyatakan jenis paket yang akan dikirim, sehingga akan mendapatkan respon yang sesuai. Flag yang dimaksud adalah Ini adalah SYN (Synchronise), ACK (Acknowledge), FIN (Finished) dan RST (Reset). Paket SYN termasuk nomor urutan TCP, yang memungkinkan sistem remote tahu apa urutan yang diharapkan dalam komunikasi berikutnya. ACK memberikan tanda bahwa paket atau set paket sudah diterima, FIN dikirim saat komunikasi selesai, meminta bahwa koneksi ditutup, dan RST akan dikirim saat sambungan direset (ditutup segera).

Untuk memulai koneksi TCP, sistem memulai mengirimkan paket SYN ke tujuan, yang akan merespon dengan SYN sendiri, dan ACK, memberitahu bahwa paket pertama sudah diterima (kemudian digabungkan menjadi sebuah paket SYN / ACK tunggal) . Sistem pertama kemudian mengirimkan sebuah paket ACK untuk menandai penerimaan SYN / ACK, dan kemudian transfer data dapat dimulai. SYN atau Stealth pemindaian memanfaatkan prosedur ini dengan mengirimkan paket SYN dan melihat respon. Jika SYN / ACK dikirim kembali, port terbuka dan ujung remote mencoba untuk membuka koneksi TCP. Nmap kemudian mengirimkan RST untuk men-down-kan koneksi sebelum terbentuk koneksi; sehingga mencegah koneksi muncul di log aplikasi. Jika port ditutup, RST akan dikirim. Jika difilter, paket SYN akan di-drop dan tidak akan mengirim response. Dengan cara ini, Nmap dapat mendeteksi tiga status port, open, closed and filtered.

Hasil Listen paket antara host anda dan host target dengan menggunakan wireshark

![Hasil Listen paket antara host anda dan host target dengan menggunakan wireshark](//sapikuda.com/images/posts/2013-07-01-network-scanning/10.png)

Half Open Connection adalah koneksi TCP yang tidak dibuka secara penuh (SYN-ACK-FIN), namun koneksi TCP berupa (SYN-ACK-RST). Contohnya, Anda mengirim paket SYN, seolah-olah Anda akan membuka koneksi sesungguhnya dan kemudian menunggu tanggapan. SYN / ACK menandakan port sedang listening (open), kemudian koneksi akan berstatus RST (reset) dimana tidak akan men-down-kan koneksi sebelum terbentuk koneksi.

Penggambaran diagram TCP menggunakan Wireshark

![Penggambaran diagram TCP menggunakan Wireshark](//sapikuda.com/images/posts/2013-07-01-network-scanning/11.png)

### Kesimpulan

Yang harus dilakukan oleh hacker untuk mengetahui informasi dari hosts target yang akan diserang adalah mencari informasi korban atau foot printing dilakukan dengan menentukan ruang lingkup pencarian informasi, enumerasi jaringan, mengintrogasi DNS korban, melakukan pengintaian terhadap aktifitas jaringan korban.

Nmap adalah sebuah tool open source untuk eksplorasi dan audit keamanan jaringan. Nmap menggunakan paket IP raw untuk mendeteksi host yang terhubung dengan jaringan dilengkapi dengan layanan (nama aplikasi dan versi) yang diberikan, sistem operasi (dan versi), apa jenis firewall/filter paket yang digunakan, dan sejumlah karakteristik lainnya.

Output Nmap adalah sebuah daftar target host yang diperiksa dan informasi tambahan sesuai dengan opsi yang digunakan. Berikut adalah beberapa informasi tambahan yang menarik untuk ditelaah :

- nomor port
- service
- status port : terbuka (open), difilter (filtered), tertutup (closed), atau tidak difilter (Unfiltered).
- nama reverse DNS
- prakiraan sistem operasi
- jenis device, dan
- alamat MAC.
- Terdapat banyak parameter yang disediakan oleh nmap untuk melakukan footprinting yaitu dengan pencarian informasi, enumerasi jaringan, mengintrogasi DNS korban, melakukan pengintaian terhadap aktifitas jaringan korban.