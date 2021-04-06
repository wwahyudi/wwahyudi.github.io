---
title:      Kriptografi
date:       2013-07-01
summary:    Tentang kriptografi dan aplikasi sederhananya
category:   Security
---

Kriptografi muncul didasari atas berkomunikasi dan saling bertukar informasi/data secara jarak jauh.komunikasi dan pertukaran data antar wilayah dan negara ataupun benua bukan lagi menjadi suatu kendala yang berarti. Seiring dengan itu tuntutan akan keamanan terhadap kerahasiaan informasi yang saling dipertukarkan tersebut semakin meningkat. Begitu banyak pengguna seperti departemen pertahanan, suatu perusahaan atau bahkan individu-individu tidak ingin informasi yang disampaikannya diketahui oleh orang lain atau kompetitornya atau negara lain.oleh karena itu munculah cabang ilmu yang mempelajari tentang cara-cara pengamanan data atau dikenal dengan istilah Kriptografi.

Dalam kriptografi terdapat dua konsep utama yakni enkripsi dan dekripsi. Enkripsi adalah proses dimana informasi/data yang hendak dikirim diubah menjadi bentuk yang hampir tidak dikenali sebagai informasi awalnya dengan menggunakan algoritma tertentu. Dekripsi adalah kebalikan dari enkripsi yaitu mengubah kembali bentuk tersamar tersebut menjadi informasi awal.

Ada empat tujuan dari ilmu kriptografi, yaitu:

- Kerahasiaan. Pesan (plaintext) hanya dapat dibaca oleh pihak yang memliki kewenangan.
- Autentikasi. Pengirim pesan harus dapat diidentifikasi dengan pasti, penyusup harus dipastikan tidak bisa berpura-pura menjadi orang lain.
- Integritas. Penerima pesan harus dapat memastikan bahwa pesan yang dia terima tidak dimodifikasi ketika sedang dalam proses transmisi data.
- Non-Repudiation. Pengirim pesan harus tidak bisa menyangkal pesan yang dia kirimkan

## Percobaan Kriptografi

Dalam percobaan ini, kriptografi digunakan pada program chat klien-server sederhana, dimana teks yang akan dikirim yang akan dienkripsi. Pada percobaan ini bahasa pemrograman yang digunakan adalah Java SE sehingga pada program ini akan digunakan Java Encryption Architecture. JCA adalah bagian utama dari platform, dan berisi arsitektur “provider” dan satu set API untuk digital signatures, message digests (hashs), certificates dan certificate validation, encryption (symmetric/asymmetric block/stream ciphers), key generation dan manajemennya, dan secure random number generation,dan lain-lain. API ini memungkinkan pengembang untuk dengan mudah mengintegrasikan keamanan ke dalam kode aplikasi mereka. Arsitektur ini dirancang di sekitar prinsip-prinsip berikut:

### Implementation independence

Aplikasi tidak perlu untuk mengimplementasikan algoritma keamanan. Sebaliknya, mereka dapat meminta jasa keamanan dari platform Java. Jasa keamanan diimplementasikan dalam penyedia (lihat di bawah), yang terhubung ke platform Java melalui antarmuka standar. Sebuah aplikasi dapat mengandalkan beberapa penyedia independen untuk fungsi keamanan.

### Implementation interoperability

Penyedia yang dioperasikan di seluruh aplikasi. Secara khusus, aplikasi yang tidak terikat ke operator tertentu, dan penyedia tidak terikat aplikasi tertentu.

### Algorithm extensibility

Platform Java mencakup sejumlah penyedia built-in yang menerapkan satu set dasar pelayanan keamanan yang banyak digunakan saat ini. Namun, beberapa aplikasi mungkin mengandalkan muncul standar belum diimplementasikan, atau layanan eksklusif. Platform Java mendukung instalasi penyedia kustom yang mengimplementasikan layanan tersebut.

## Hasil percobaannya

Untuk mengenkripsi digunakan class berikut ini:

```java
public static String sapi(String key, String msg) throws Throwable {
        byte[] textEncrypt = null;
        DESKeySpec dks = new DESKeySpec(key.getBytes());
        SecretKeyFactory skf = SecretKeyFactory.getInstance("DES");
        SecretKey desKey = skf.generateSecret(dks);
        Cipher cipher = Cipher.getInstance("DES/ECB/PKCS5Padding");

        cipher.init(Cipher.ENCRYPT_MODE, desKey);
        byte[] text = msg.getBytes();
        textEncrypt = cipher.doFinal(text);
        String encryptedValue = new BASE64Encoder().encode(textEncrypt);
        return encryptedValue;
    }
```

Untuk mengdekripsi digunakan class berikut ini:

```java
public static String kuda(String key, String msg) throws Throwable {
        DESKeySpec dks = new DESKeySpec(key.getBytes());
        SecretKeyFactory skf = SecretKeyFactory.getInstance("DES");
        SecretKey desKey = skf.generateSecret(dks);
        Cipher cipher = Cipher.getInstance("DES/ECB/PKCS5Padding");

        cipher.init(Cipher.DECRYPT_MODE, desKey);
        byte[] decordedValue = new BASE64Decoder().decodeBuffer(msg);
        byte[] textEncrypt = cipher.doFinal(decordedValue);
        String decryptedValue = new String(textEncrypt);
        return decryptedValue;
    }
```

Berikut hasil eksekusinya:

{% include image.html url="/images/2013-07-01-kriptografi/1.png" description="Hasil running Chat Server" %}

{% include image.html url="/images/2013-07-01-kriptografi/2.png" description="Hasil running Chat Client 1" %}

{% include image.html url="/images/2013-07-01-kriptografi/3.png" description="Hasil running Chat Client 2" %}

Enkripsi yang digunakan pada percobaan diatas adalah des/ecb/pkcs5padding yag artinya:

- DES = Data Encryption Standard.
- ECB = Electronic Codebook mode.
- PKCS5Padding = PKCS #5-style padding

Dalam hal ini, percobaan dibuat menggunakan DES (Data Encryption Standard) cipher dalam mode codebook elektronik, dengan PKCS # 5 gaya bantalan.

Pada percobaan diatas digunakan skema enkripsi asimetrik, dimana tiap pengguna (klien) memiliki key sendiri. Tiap klient diminta untuk memiliki key tersendiri.

{% include image.html url="/images/2013-07-01-kriptografi/4.gif" description="Gambaran skema enkripsi asimetrik" %}

Penggunaan enkripsi dapat dilihat pada hasil running diatas, pada server terlihat bahwa semua text yang terkirim pada server tidak dapat dibaca dengan mudah. Sedangkan pada klien dapat dibaca dengan mudah dikarenakan klien memiliki key dan chipper-nya, sehingga teks yang dikirim langsung diterjemahkan kembali.

Berikut hasil perbandingan Follow Stream dari paket yang dikirim dengan klien dengan enkripsi dan klien tanpa enkripsi:

{% include image.html url="/images/2013-07-01-kriptografi/5.png" description="Hasil Follow Stream paket yang dikirim dengan klien yang mengenkripsi teksnya" %}

Hasil Follow Stream paket yang dikirim dengan klien yang mengenkripsi teksnya

{% include image.html url="/images/2013-07-01-kriptografi/6.png" description="Hasil Follow Stream paket yang dikirim dengan klien yang tidak mengenkripsi teksnya" %}

Hasil Follow Stream paket yang dikirim dengan klien yang tidak mengenkripsi teksnya

Terlihat diatas bahwa Hasil Follow Stream paket yang dikirim dengan klien yang **tidak** mengenkripsi teksnya dapat dibaca dengan mudah sehingga dapat dianggap bahwa pesan yang dikirim tidak aman, sedangkan Hasil Follow Stream paket yang dikirim dengan klien yang mengenkripsi teksnya, hasil pesan tidak mudah dibaca.
