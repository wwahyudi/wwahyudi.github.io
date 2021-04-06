---
title:      Ubah Windows Server Anda Menjadi Workstation
date:       2012-04-01
summary:    Mengubah Windows Server 2008 R2 menjadi workstation
category:   Windows
---

Windows Server 2008 adalah nama sistem operasi untuk server dari perusahaan Microsoft. Sistem server ini merupakan pengembangan dari versi sebelumnya yang disebut Windows Server 2003. Pada tanggal 15 Mei 2007, Bill Gates mengatakan pada konferensi WinHEC bahwa Windows Server 2008 adalah nama baru dari Windows Server `Longhorn`.

Windows Server 2008 dibangun dari kode yang sama seperti Windows Vista; karenanya Windows Server 2008 memiliki arsitektur dan fungsionalitas yang sama dengannya tetapi ditambah beberapa fasilitas untuk memudahkan kerja administrator server.

Saya sudah beberapa bulan ini saya mulai untuk mengunakan Windows Server 2008 R2 SP 1 (tentunya asli, saya mendapatkan situs MSDNAA kampus saya). Karena saya menginstal Windows ini menggantika Windows 7 saya, untuk itulah saya mencoba untuk membuat Windows Server 2008 R2 SP1 ini menjadi workstation baru menggantikan Windows 7.

Berikut fitur yang perlu diubah:

1. Instalasi Driver: Instalasi driver Microsoft Windows Server.
2. Wireless Network: (Jika Anda tidak menggunakan perangkat wireless, mengabaikan langkah ini) Aktifkan wireless pada Windows Server 2008.
3. New User and Strong Passwords Enforcement: Membuat user baru dan menonaktifkan Strong Passwords Enforcement kompleksitas minimum untuk password.
4. Shutdown Tracker: Menonaktifkan Tracker Shutdown yang cukup menjengkelkan.
5. Ctrl + Alt + Del: Men-disable ctrl + alt + del saat startup Windows.
6. Audio dan Startup Sound: Mengaktifkan audio service dan startup sound.
7. ComputerName: Mengubah ComputerName.
8. Kinerja: Meningkatkan kinerja aplikasi di windows server, ubah Processor Schedulling dari Background Service ke Programs.
9. Internet Explorer Enhanced Security: Nonaktifkan peningkat keamanan di Internet Explorer.
10. Tema, SideBar dengan Gadget Custom, Cursors Aero dan Thumbnail: Aktifkan tema Vista Aero dan Flip 3D dengan thumbnail dan preview Sidebar di Windows Explorer.
11. SuperFetch: Aktifkan feature ini untuk mempertahankan dan meningkatkan performa sistem.

dan itulah fitur yang perlu diubah untuk menjadikan Windows server 2008 R2 menjadi Workstation. dan satu kesan saya terhadap Windows Server, `AWESOME!`.
