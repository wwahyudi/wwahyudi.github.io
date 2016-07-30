---
layout:     post
title:      Host Based Intrussion Detection System menggunakan Tripwire
date:       2013-07-05
summary:    Konfigurasi IDS Tripwire di lab
categories: network
---

IDS merupakan sistem yang bersifat pasif, mengingat tugasnya hanyalah mendeteksi intrusi yang terjadi dan memberikan peringatan kepada administrator jaringan bahwa mungkin ada serangan atau gangguan terhadap jaringan.  IDS merupakan sistem yang bersifat pasif, mengingat tugasnya hanyalah mendeteksi intrusi yang terjadi dan memberikan peringatan kepada administrator jaringan bahwa mungkin ada serangan atau gangguan terhadap jaringan. Cara kerjanya adalah dengan menggunakan pendeteksian berbasis *signature* (seperti halnya yang dilakukan oleh beberapa antivirus), yang melibatkan pencocokan lalu lintas jaringan dengan basis data yang berisi cara-cara serangan dan penyusupan yang sering dilakukan oleh penyerang. Sama seperti halnya antivirus, jenis ini membutuhkan pembaruan terhadap basis data *signature* IDS yang bersangkutan.

Tripwire adalah termasuk kategori Host Intrusion Detection System (IDS). Yang mendeteksi perubahan file di mesin yang mungkin dilakukan oleh penyerang. Logika bekerja tripwire adalah dengan membuat baseline database dari file yang ada di system. Jika file tersebut berubah maka tripwire akan mencatat dan / atau memberitahukan administrator mesin. Integrity checker pada Tripwire bekerja dengan cara menghitung checksum (menghitung integritas) dari daftar-daftar direktory yang sudah dienkripsi dan menyimpannya di dalam sebuah database. Kemudian secara periodik atau ketika user memerintahkan untuk melakukan pengecekan, checksum dari program-program tersebut akan dihitung ulang dan dibandingkan dengan database checksum tersebut.

Berikut proses instalasi Tripwire sebagai Host Base IDS:

Step awal dalam instalasi tripwire adalah membuka terminal dan menginstalnya dengan #apt-get install tripwire. Pada awal instalasi ada petunjuk bahwa peringatan tentang apa yang akan dibutuhkan dalam instalasi Tripwire. Untuk melanjutkan, pilih OK.

![Awal instalasi Tripwire](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/1.png)

Kemudian muncul dialog untuk meminta user membuat key passphrase, pilih yes.

![Konfirmasi untuk membuat paraphase](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/2.png)

Kemudian akan ada peringatan kembali, persis seperti awal instalasi, peringatan ini, mengingatkan untuk membuat key yang aman.

![Warning kedua](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/3.png)

Kemudian ada konfirmasi apakah ingin membuat local paraphase atau tidak. Pilih yes karena paraphase akan digenerate pada local machine sendiri.

![Konfirmasi apakah key akan dibuat sebagai local key](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/4.png)

Kemudian ada permintaan konfirmasi apakah ingin untuk menyimpan hasil konfigurasi pada /etc/tripwire/twcfg.txt. Pilih yes untuk me-rebuild file configurasi.

![Konfirmasi untuk me-rebuild file konfigurasi](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/5.png)

Setelah itu, ada permintaan konfirmasi apakah ingin untuk menyimpan hasil policy pada /etc/tripwire/twpol.txt. Pilih yes untuk me-rebuild file policy.

![Konfirmasi me-rebuild policy file](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/6.png)

Setelah itu, masukan key yang diinginkan dan ulangi pada langkah selanjutnya setelah memilih OK.

![Memasukan site-key passphrase](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/7.png)

Dan instalasi akan selesai ketika passphrase dimasukan dua kali. Kemudian akan ada dialog yang menyatakan bahwa instasi Tripwire selesai.

![hasil tripwire sudah diinstal](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/8.png)

Dan berikut langkah-langkah untuk memodifikasi file Policy & file Konfigurasi Tripwire agar lebih aman.

Pertama modifikasi file /etc/tripwire/twcfg.txt jika diperlukan kemudian enkripsi file dengan menggunakan perintah #twadmin –create-cfgfile –cfgfile ./tw.cfg –site-keyfile ./site.key ./twcfg.txt

![Mengenkriptsi file twcfg.txt](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/9.png)

Lakukan juga pada file /etc/tripwire/twpol.txt, modifikasi jika diperlukan dan kemudian enkripsi file tersebut.

Setelah melakukan proses modifikasi dan enkripsi file twpol.txt dan twcfg.txt, lakukan inisialisasi database dari Tripwire. Inisialisasi database dengan menggunakan perintah:

{% highlight bash %}
# tripwire --init --cfgfile /etc/tripwire/tw.cfg  --polfile /etc/tripwire/tw.pol --site-keyfile /etc/tripwire/site.key  --local-keyfile /etc/tripwire/debian-local.key
{% endhighlight %}

untuk perintah debian-local key sesuaikan dengan **hostname** PC yang digunakan. Masukan key site passphrase. Pada awal inisialisasi, akan terdapat banyak warning dan error karena Tripwire tidak mempunyai data yang sama pada databasenya.

![Proses inisialisasi database Tripwire](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/10.png)

Untuk mencheck pada sistem terhadap adanya perubahan pada file-file pada host, gunakan perintah:

{% highlight bash %}
# tripwire –check
{% endhighlight %}

![Berikut hasil pengecekan](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/11.png)

Untuk melakukan update file policy Tripwire lakukan dengan perintah:

{% highlight bash %}
# tripwire --update-policy --cfgfile ./tw.cfg --polfile ./tw.pol  --site-keyfile ./site.key --local-keyfile ./HOSTNAME-local.key ./twpol.txt
{% endhighlight %}

Update ini difungsikan apabila ada perubahan pada file twpol.txt, misalnya menambahkan atau mengurangi folder yang dimonitor.

Sedangkan untuk melakukan update database file system Tripwire lakukan dengan perintah:

{% highlight bash %}
# tripwire –update -Z low –twrfile /var/lib/tripwire/report/debian-20130311-134129.twr
{% endhighlight %}

debian-20130311-134129 merupakan host dan date (yyyymmdd-time) perintah tersebut berarti bahwa tripwire akan membandingkan antara database yang ada dengan file yang ada di system, kemudian akan menjalankan editor untuk memilih perubahan di database.

![Mengupdate database tripwire](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/12.png)

Untuk mencoba sedikit kerja Tripware dilakukan percobaan sebagai berikut:

Pertama lakukan Tripwire check.

![Hasilnya](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/13.png)

Kemudian ubah file policy twpol.txt. Tambahkan beberapa tambahan berikut ini di baris paling bawah:

{% highlight bash %}
(
rulename = “Kirim Notifikasi ke email”,  severity = $(SIG_HI),  emailto = root@localhost
){}
{% endhighlight %}

Email akan dikirimkan ke akun email dari root dari system yang anda monitor. Biasanya, email akan ditujukan kea kun user yang dapat bertindak sebagai root.

![Perubuhan yang dimaksudkan](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/14.png)

Kemudian enkripsi kembali menggunakan perintah

{% highlight bash %}
# twadmin --create-polfile --cfgfile ./tw.cfg  --site-keyfile ./site.key ./twpol.txt
{% endhighlight %}

Juga ubah file konfigurasi untuk memasukkan informasi smtp

{% highlight bash %}
…
MAILMETHOD =SMTP
SMTPHOST =localhost
SMTPPORT =25
…
{% endhighlight %}

![Perubahan konfigurasi smpt Tripwire](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/15.png)

Juga enkripsi kembali fil twcfg.txt menggunakan perintah:

{% highlight bash %}
# twadmin --create-cfgfile --cfgfile ./tw.cfg --site-keyfile ./site.key ./twcfg.txt
{% endhighlight %}

Jalankan test pengiriman email dengan menggunakan perintah:

{% highlight bash %}
# tripwire –test –email root@localhost
{% endhighlight %}

![Test email](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/16.png)

Kemudian check email user

![Terdapat banyak email test karena email test yang dikirim berulang-ulang](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/17.png)

Terdapat banyak email test karena email test yang dikirim berulang-ulang

Untuk pengetesan Buat sebuah file kosong . Kemudian salinlah ke dalam direktori /bin. Kemudian lakukan cek konsistensi.

![Hasil pengecekan](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/18.png)

Hasilnya terlihat pada log tripwire yang menyatakan penambahan file dan pemodifikasian pada direktori root, kemudian dibeberapa keterangan dibawahnya menyebutkan warning terhadap penambahan file pada direktori root. Terdapat email pula yang memberikan warning kepada user. Email pada user terlihat pada email no 4 pada gambar sebelumnya. Yang berbeda dari hasil check tripwire adalah adanya tambahan pada keterangan apa yang bertambah dan dimodifikasi

![Hasil perubahan](http://sapikuda.com/images/posts/2013-07-05-host-based-intrussion-detection-system/19.jpg)