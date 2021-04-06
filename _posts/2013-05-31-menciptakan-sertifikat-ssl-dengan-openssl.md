---
title:      Menciptakan Sertifikat SSL dengan OpenSSL
date:       2013-05-31
summary:    Membuat self signed SSL certificate untuk Apache2
category:   Linux
---

Pada bagian ini anda akan belajar bagaimana menginstall web server yang secure dengan menggunakan SSL/TLS untuk transaksinya. Mengenai software yang dibutuhkan, anda bisa membaca pada dasar teori. Sebagai langkah awal installah software-software tersebut. Untuk mod_ssl, secara otomatis telah ter-built-in pada Apache2.

Tahap paling penting  adalah membuat public/private key, SSL sertifikat, Certificate Signing Request (CSR). Biasanya, server yang komersial, akan meminta otoritas pihak ke tiga seperti VeriSign untuk menandatangani sertifikatnya. Kita tidak akan meminta bantuan pihak ketiga, namun menjadi CA sendiri (self-signing CA). Kita juga akan membuat domain sendiri yang dihost secara local. Karena itu anda bisa memilih nama domain yang anda inginkan. Kerjakan langkah-langkah dibawah dan jawab pertanyaan-pertanyaannnya.

## Menginstal dan membuat ssl server key

**1.** Menginstall software-software yang dibutuhkan:

```bash
sudo apt-get install apache2
sudo apt-get install openssl
```

![Instalasi apache2 dan ssl](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/1.png)

**2.** Langkah berikutnya adalah membuat sertifikat SSL untuk web server sebelum kita menjalankan secure server dengan HTTPS. Selain itu , kita juga akan membuat pasangan public/private key untuk melakukan request sertifikat. Anda akan membutuhkan domain name untuk sertifikat yang anda ciptakan. Pada contoh ini digunakan nama: `www.asem.com`.

**3.** Masuk ke direktori: /etc/apache2/ssl. Direktori ssl adalah direktori dimana anda menyimpan semua private keys, certificate signing request dan sertifikat. Lihat isi direktori ini.

```bash
cd /etc/apache2/ssl.
ls -al
```

![Membuat direktori ssl dan masuk ke direktori tersebut](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/2.png)

**4.** Untuk membuat Certificate Signing Request (CSR), anda harus membuat sepasang public/privat key terlebih dahulu.

**5.** Kemudian buatlah key dengan nama server.key yang merupakan private key dengan perintah:

```bash
sudo openssl genrsa -des3 -out server.key 1024
```

- `genrsa`: menunjukkan OpenSSL bahwa anda ingin menciptakan sepasang key
- `des3`: menunjukkan bahwa private key harus dienkripsi dan dilindungi oleh passphrase
- `out`: menunjukkan filename yang akan menyimpan hasil output
- `1024`: menunjukkan jumlah bit dari key yang dibuat

![pembuatan key untuk ssl](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/3.png)

**6.** Siapkan passphrase dan isikan pada saat diminta. Hasilnya akan muncul seperti ini:

```bash
Generating RSA private key, 1024 bit long modulus
........++++++
...++++++
e is 65537 (0x10001)
Enter pass phrase for server.key:
Verifying - Enter pass phrase for server.key:
You will be prompted for a pass phrase, once you type in your initial pass phrase, you will be asked to verify this pass phrase.
```

Setelah menjalankan langkah ini, akan tercipta server.key pada direktori ssl.

Passphrase yang diset adalah: `asem`

![isi file server.key](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/4.png)

Isi file server.key

**7.** Pastikan anda maih berada pada direktori /etc/apache2/ssl. Cobalah generate file server.csr dengan menggunakan private key diatas. Gunakan perintah dibawah. Csr ini kependekan dari certificate signing request.

```bash
sudo openssl req -new -key server.key -out server.csr
```

Akan muncul tampilan berikut. Yang penting ketika memasukkan Common Name (CN) adalah CN harus sesuai dengan alamat web, nama DNS atau IP address pada konfigurasi Apache. Setelah anda memasukkan informasi tersebut, akan terbentuk file server.csr yang akan digunakan untuk meminta sertifikat.

![membentuk csr key dari openssl key](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/5.png)

Membentuk csr key dari openssl key

![Isi file server.csr](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/6.png)

Isi file server.csr

**8.** Perhatikan bahwa untuk membuat file server.crt anda membutuhkan server.key dan server.csr. Biasanya, pada langkah ini, web server komersial akan meminta CA professional seperti VeriSign untuk memberikan file crt. Pada kesempatan ini kita akan berlaku sebagai CA dan mengenerate file crt tersebut secara mandiri (self-signing certificate).

**9.** Sekarang coba generate file server.crt dengan perintah dibawah:

```bash
sudo openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```

Perintah diatas akan meminta anda memasukkan kembali passphrase untuk mengenerate server.key (yaitu 123456). Masukkan passphrase yang sesuai.

![membentuk crt key dari csr key](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/7.png)

Membentuk crt key dari csr key

![isi file server.csr](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/8.png)

**10.** Cek lagi directory ssl dan anda seharusnya memiliki 3 file, server.key,server.crt dan server.crs.

![Isi direktori ssl](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/9.png)

Isi direktori ssl

## Konfigurasi Apache untuk site www.asem.com

**1.** Cek direktori ssl untuk memastikan ada 3 file disini yaitu : server.key,server.crt dan server.crs. Hal ini penting untuk kelancaran konfigurasi kali ini.

![Isi direktori ssl](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/9.png)

**2.** Enable-kan modul mod_ssl dengan perintah:

```bash
sudo a2enmod ssl
```

![mengakftifkan modul ssl](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/10.png)

**3.** Restart apache. Tujuannya agar module ssl berjalan pada apache. Proses ini harusnya berjalan tanpa error.

```bash
sudo /etc/init.d/apache2 restart
```

![Merestart service apache2](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/11.png)

**4.** Copy file default ke file ke file www.asem.com

```bash
cd /etc/apache2/sites-available/
cp default www.asem.com
```

![Mengcopy default untuk www.asem.com](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/12.png)

**5.** Edit file www.asem.com dengan gedit atau pico

```bash
sudo gedit etc/apache2/sites-available/www.asem.com
```

Ubah port VirtualHost dari 80 ke 443. Baris pertama dari file namapk seperti dibawah.

```bash
<VirtualHost *:443>
```

Tambahkan server name dibawah baris ServerAdmin

```bash
ServerName www.asem.com:443
```

Ubah DocumentRoot untuk menunjuk ke web direktori dari website asem.

```bash
DocumentRoot /var/www/asem
```

Ubah <Directory /var/www/> untuk menunjuk ke direktori asem

```bash
<Directory /var/www/asem>
```

![Mengubah port, server name, dan directory root www.asem.com](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/13.png)

Mengubah port, server name, dan directory root www.asem.com

Masukkan baris ini sebelum baris ErrorLog /var/log/apache2/error.log. Tujuannya agar virtual host tahu dimana key dan sertifikat SSL disimpan.

```bash
SSLEngine On
SSLCertificateFile /etc/apache2/ssl/server.crt
SSLCertificateKeyFile /etc/apache2/ssl/server.key
```

![Menambahkan SSL pada default dari www.asem.com](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/14.png)

Menambahkan SSL pada default dari www.asem.com

**6.** Simpan file www.asem.com

**7.** Pada directory /var/www/, copy file index.html ke /var/www/asem/

```bash
sudo cp /var/www/index.html /var/www/asem/index.html
```

![Mengcopy html index untuk index file www.asem.com](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/15.png)

**8.** Enable-kan website www.asem.com dengan:

```bash
sudo a2ensite www.asem.com
```

![Mengaktifkan website www.asem.com](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/16.png)

**9.** Edit file /etc/hosts untuk melakukan resolusi domain www.asem.com website ke 127.0.0.1. Cari baris 127.0.0.1 lalu tambahkan baris ini:

```bash
127.0.0.1 localhost asem www.asem.com
```

![Menambahkan konfigurasi resolv domain](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/17.png)

## Menjalankan https

**1.** Restart apache2 agar efek dari sertifikat SSL mulai efektif.

```bash
sudo /etc/init.d/apache2 restart
```

Masukkan passphrase yaitu 123456. Jika cocok maka apache2 akan di-restart dan memberikan tanda [OK].

![merestart service apache2](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/18.png)

**2.** Buka http://localhost pada browser. Jika apache2 berjalan dengan baik, maka akan muncul pesan "It Works!"

**3.** Buka URL berikut https://www.asem.com. Akan muncul pesan dari browser kita karena sertikat yang diterima oleh www.asem.com bersifat *self-signed*. Kita akan menerima sertikat tersebut dan menambahkan pengecualian.

**4.** Klik tombol : `I Understand the Risks`, kemudian klik tombol: `Add Exception` dan terakhir klik `Get Certificate`. Klik `View`. Anda akan melihat sertifikat *self-signed* yang tadi anda buat. Setelah itu klik checkbox `Permanently store this exception` dan klik tombol `Confirm Security Exception`.

![Konfigurasi eksepsi https certificate](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/19.png)

**5.** Akhirnya muncul halaman web yang kita buat. Perhatikan bahwa awal dari URL adalah https, bukan http

![Hasil buka https://www.asem.com](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/20.png)

**6.** Pada pojok kiri bawah tampak tanda gembok yang menunjukkan bahwa web server tersebut menggunakan sertifikat.

**7.** Double klik tanda kunci tersebut. Klik tombol `view certificate` pada tanda kunci tersebut. Dapat diketahui bahwa sertifikat bertanggal 05/06/2014 dan akan expire dalam 1 tahun.

**8.** Pada tab `Detail`, kita dapat mengtahui siapa issuer sertifikat ini.

![Detail properties certificate issuer](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/22.png)

## Pembahasan

Teknologi SSL menggunakan konsep teknologi kriptografi kunci publik untuk bisa mencapai komunikasi yang aman ini antara server dan pengunjungnya. Saat server dan pengakses saling bertukar data yang disamarkan dan untuk membacanya digunakan sandi dan kunci yang hanya dimiliki kedua pihak yang berkomunikasi tersebut, sehingga pihak lain yang mencoba menyadap data yang dikirim tersebut tidak akan bisa membacanya karena sandi dan kunci yang dibutuhkan tersebut hanya dimiliki oleh kedua pihak yang berkomunikasi tadi. Secara sederhana, komunikasi internet dengan SSL ini bisa dilihat dengan cara akses alamat URL-nya yang diawali dengan "https://" (misalnya https://www.asem.com ), sedangkan yang tanpa SSL alamatnya hanya http biasa (misalnya http://www.asem.com).

Cara kerja ssl adalah sebagai berikut:

![Bagan SSL handshake](/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/23.gif)

Bagan SSL handshake, dijelaskan sebagai berikut:

1. SSL atau TLS klien mengirimkan "client hello" pesan yang mencantumkan informasi kriptografi seperti SSL atau TLS versi dan, dalam rangka klien preferensi, ciphersuite didukung oleh klien. Pesan tersebut juga berisi string byte acak yang digunakan dalam perhitungan berikutnya. Protokol ini memungkinkan untuk "client hello " untuk memasukkan metode kompresi data yang didukung oleh klien.
2. SSL atau TLS server merespon dengan server "server hello" pesan yang berisi ciphersuite dipilih oleh server dari daftar yang disediakan oleh klien, ID sesi, dan satu lagi byte string acak. Server juga mengirimkan sertifikat digital. Jika server memerlukan sertifikat digital untuk otentikasi klien, server akan mengirimkan "permintaan sertifikat klien" yang berisi daftar jenis sertifikat didukung dan Nama Distinguished Otoritas Sertifikasi diterima (CA).
3. SSL atau TLS klien memverifikasi sertifikat digital server.
4. SSL atau TLS klien mengirimkan byte string acak yang memungkinkan baik klien dan server untuk menghitung kunci rahasia yang digunakan untuk mengenkripsi data pesan berikutnya. Acak byte string itu sendiri dienkripsi dengan kunci publik server.
5. Jika SSL atau TLS server dikirim "client certificate request", klien mengirimkan byte string acak dienkripsi dengan kunci pribadi klien, bersama dengan sertifikat digital klien, atau "tidak ada peringatan sertifikat digital". Peringatan ini hanya peringatan, tetapi dengan beberapa implementasi jabat tangan gagal jika otentikasi klien adalah wajib.
6. SSL atau TLS Server memverifikasi sertifikat klien.
7. SSL atau TLS klien mengirimkan server pesan "finished", yang dienkripsi dengan kunci rahasia, yang menunjukkan bahwa bagian klien dari jabat tangan selesai.
8. SSL atau TLS server mengirimkan klien pesan "finished", yang dienkripsi dengan kunci rahasia, yang menunjukkan bahwa bagian server jabat tangan selesai.
9. Untuk durasi sesi SSL atau TLS, server dan klien sekarang dapat bertukar pesan yang simetris dienkripsi dengan kunci rahasia bersama.

Pada implementasi nyata, berikut hasil capture handshake menggunakan wireshark:

{% include image.html url="/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/24.png" description="Hasil protocol analysis dari ssl sebelum browser mendapat persetujuan user untuk meng-Confirm Security Exception" %}

{% include image.html url="/images/2013-05-31-menciptakan-sertifikat-ssl-dengan-openssl/25.png" description="Hasil protocol analysis dari ssl sebelum browser mendapat persetujuan user untuk meng-`Confirm Security Exception`. SSL berjalan seperti pada Bagan SSL handshake." %}
