---
layout:     post
title:      Sedikit tentang Bastion Host
date:       2013-07-01
summary:    Kilasan tentang Bastion Host
categories: network
---

Bastion host adalah tujuan komputer khusus pada jaringan khusus dirancang dan dikonfigurasi untuk menahan serangan. Komputer umumnya host single application, misalnya server proxy, dan semua layanan lainnya akan dihapus atau dibatasi untuk mengurangi ancaman ke komputer. Bastion host di-hardened  dengan cara ini terutama karena lokasi dan tujuan, yang baik di luar firewall atau di DMZ dan biasanya melibatkan akses dari jaringan untrusted atau komputer.

Istilah bastion host umumnya dikaitkan dengan Marcus J. Ranum dalam sebuah artikel membahas firewall. Di dalamnya ia mendefinisikan bastion host sebagai:

> … Sebuah sistem yang diidentifikasi oleh administrator firewall sebagai titik kuat penting dalam keamanan jaringan. Umumnya, bastion host akan memiliki beberapa tingkat perhatian ekstra diberikan pada keamanan mereka, dapat menjalani audit reguler, dan mungkin telah memodifikasi perangkat lunak.

Ini adalah sistem yang diidentifikasi oleh administrator firewall sebagai titik kuat penting dalam keamanan jaringan. Sebuah bastion host adalah sebuah komputer yang sepenuhnya terkena serangan. Sistem ini di sisi publik dari zona demiliterisasi (DMZ), dilindungi oleh firewall atau penyaringan router. Sering peran sistem ini sangat penting untuk sistem keamanan jaringan. Memang firewall dan router dapat dianggap bastion host. Karena eksposur mereka banyak upaya yang harus dimasukkan ke dalam merancang dan mengkonfigurasi bastion host untuk meminimalkan kemungkinan penetrasi. jenis lain dari bastion host termasuk web, mail, DNS, dan server FTP.

Ada dua konfigurasi jaringan yang umum yang mencakup bastion host dan penempatan mereka. Yang pertama membutuhkan dua firewall, dengan bastion host duduk di antara yang pertama “dunia luar” firewall, dan firewall dalam, di zona demiliterisasi (DMZ). Seringkali jaringan yang lebih kecil melakukan tidak memiliki beberapa firewall, jadi jika hanya satu firewall ada dalam jaringan, bastion host biasanya ditempatkan di luar firewall.

Ini adalah beberapa contoh dari sistem atau layanan bastion host:

- DNS (Domain Name System) server
- Email server
- FTP (File Transfer Protocol) server
- Honeypot
- Proxy server
- VPN (Virtual Private Network) server
- Web server