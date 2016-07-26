---
layout:     post
title:      Host Based Intrussion Detection System
date:       2013-07-05
summary:    Konfigurasi IDS Tripwire di lab
categories: network
---

IDS merupakan sistem yang bersifat pasif, mengingat tugasnya hanyalah mendeteksi intrusi yang terjadi dan memberikan peringatan kepada administrator jaringan bahwa mungkin ada serangan atau gangguan terhadap jaringan.  IDS merupakan sistem yang bersifat pasif, mengingat tugasnya hanyalah mendeteksi intrusi yang terjadi dan memberikan peringatan kepada administrator jaringan bahwa mungkin ada serangan atau gangguan terhadap jaringan. Cara kerjanya adalah dengan menggunakan pendeteksian berbasis *signature* (seperti halnya yang dilakukan oleh beberapa antivirus), yang melibatkan pencocokan lalu lintas jaringan dengan basis data yang berisi cara-cara serangan dan penyusupan yang sering dilakukan oleh penyerang. Sama seperti halnya antivirus, jenis ini membutuhkan pembaruan terhadap basis data *signature* IDS yang bersangkutan.

Tripwire adalah termasuk kategori Host Intrusion Detection System (IDS). Yang mendeteksi perubahan file di mesin yang mungkin dilakukan oleh penyerang. Logika bekerja tripwire adalah dengan membuat baseline database dari file yang ada di system. Jika file tersebut berubah maka tripwire akan mencatat dan / atau memberitahukan administrator mesin. Integrity checker pada Tripwire bekerja dengan cara menghitung checksum (menghitung integritas) dari daftar-daftar direktory yang sudah dienkripsi dan menyimpannya di dalam sebuah database. Kemudian secara periodik atau ketika user memerintahkan untuk melakukan pengecekan, checksum dari program-program tersebut akan dihitung ulang dan dibandingkan dengan database checksum tersebut.

Berikut proses instalasi Tripwire sebagai Host Base IDS:

Step awal dalam instalasi tripwire adalah membuka terminal dan menginstalnya dengan #apt-get install tripwire. Pada awal instalasi ada petunjuk bahwa peringatan tentang apa yang akan dibutuhkan dalam instalasi Tripwire. Untuk melanjutkan, pilih OK.

![Awal instalasi Tripwire](https://wmo1ag.bay.livefilestore.com/y2pYPXUE1rpzIJViNrvXUSJ8yoFbJ9SZj2s4WZSOfW1nUT1lunOhv1JNdjtBZmoqADeqy1WwjzY2ssdySfHvVjb9BnihFnFANIh6568Eac1jFpcVNyusIB1lFE_AE93V7oc/1.png?psid=1)

Kemudian muncul dialog untuk meminta user membuat key passphrase, pilih yes.

![Konfirmasi untuk membuat paraphase](https://wmo1ag-ch3301.files.1drv.com/y3pfZw758HjZfcGLqux4CxWXghTf1jhucpyOLi_3I9YfuHNCuoImfRIYMeBFlUMcFAPjV2SYcBqqXViyby6rNgIHxZbVI-gDuMlHtRp5yjxQWItmk71uAK9neSrLtkVlEcI/2.png?psid=1&rdrts=144161217)

Kemudian akan ada peringatan kembali, persis seperti awal instalasi, peringatan ini, mengingatkan untuk membuat key yang aman.

![Warning kedua](https://wmo1ag-ch3301.files.1drv.com/y3pwq5MBkpz7W6bNsy0Uh2tEVMTLFg4O1Q8euqkHiDB2fEzTIIYelVRPdpBujZ2s6OyhbTMXYROy__WsQg4lQDGPg_ItTMJS3Woq61FK2xd5IxybEGFbLVFVr2kvdMCSr0W/3.png?psid=1&rdrts=144161242)

Kemudian ada konfirmasi apakah ingin membuat local paraphase atau tidak. Pilih yes karena paraphase akan digenerate pada local machine sendiri.

![Konfirmasi apakah key akan dibuat sebagai local key](https://wmo1ag.bay.livefilestore.com/y2pS9SQaGMc9w5zrQv9tYJ4qBBkpoLFqtKxj_OCws0euSZl5JFhskrfJML59aRyjUqsgU1PGEdKg06YEyek6syaHl5Onx7OPmjO6Pt04eWJsI0QsSZ-SeHhDtmnMsfFRc0Q/4.png?psid=1)

Kemudian ada permintaan konfirmasi apakah ingin untuk menyimpan hasil konfigurasi pada /etc/tripwire/twcfg.txt. Pilih yes untuk me-rebuild file configurasi.

![Konfirmasi untuk me-rebuild file konfigurasi](https://wmo1ag.bay.livefilestore.com/y2pLmtskzVHIBYUSiyq6BlrCqtkj_a5ORT9Op5D4YhYdJZCOo4_WJMew66eymWD4lQVr0lv7VkgVnuWp3SWehtSwnmE15iKAoIFxGWWViEoZctN1weZ9oH8LRYm6EYNcspG/5.png?psid=1)

Setelah itu, ada permintaan konfirmasi apakah ingin untuk menyimpan hasil policy pada /etc/tripwire/twpol.txt. Pilih yes untuk me-rebuild file policy.

![Konfirmasi me-rebuild policy file](https://wmo1ag-ch3301.files.1drv.com/y3pG_-BBrzorcVKaPIOCOXpbd2GnIUMbpXtbkNCOz1_loVdfMAzaC8FyplC-FXk_KFAaOGEUfbArJJlOCCeUEh94HNuaODJqLItlGp8O031OrZUe9Fc2tUMV_B0ytP4Wjzc/6.png?psid=1&rdrts=144161415)

Setelah itu, masukan key yang diinginkan dan ulangi pada langkah selanjutnya setelah memilih OK.

![Memasukan site-key passphrase](https://wmo1ag-ch3301.files.1drv.com/y3pFTqjhQQWzk-KjFong-JI1vc1ShKHGhpHMI-_LxLngT6PB7b76_A79Fo7UE_fMitIBodRZdwec8kaF62aDoC8ElVSNo8-b5dNoyhEN0WAmxNAgBSCs9MhbeJuZJ2R24q2/7.png?psid=1&rdrts=144160871)

Dan instalasi akan selesai ketika passphrase dimasukan dua kali. Kemudian akan ada dialog yang menyatakan bahwa instasi Tripwire selesai.

![hasil tripwire sudah diinstal](https://wmo1ag-ch3302.files.1drv.com/y3p3BAi_-YUyAOO6Uk0-adP2ByRwYJPe6bJOQlAgt68Ac02N-xOvlzohsHPBabyHi2VXpy6syvlb-6QGaYP6XdAIPgTFOEHFG3pkKHpJ5yYfDDJYDNFbUI9omWxtLyaQyEC/8.png?psid=1&rdrts=144160871)

Dan berikut langkah-langkah untuk memodifikasi file Policy & file Konfigurasi Tripwire agar lebih aman.

Pertama modifikasi file /etc/tripwire/twcfg.txt jika diperlukan kemudian enkripsi file dengan menggunakan perintah #twadmin –create-cfgfile –cfgfile ./tw.cfg –site-keyfile ./site.key ./twcfg.txt

![Mengenkriptsi file twcfg.txt](https://wmo1ag-ch3301.files.1drv.com/y3pgX9k5XaXO4kXHcFo2ZPRF4OyhO3tRtlWtIr12S70wIt69tVqgKKn6aQRWMJtkT3fmLGnbXRaIE0PVCPMOFZlXIG1fsaE-vGBVyIH0RVKVGweChQHZiV56r5BAunWJEld/9.png?psid=1&rdrts=144160871)

Lakukan juga pada file /etc/tripwire/twpol.txt, modifikasi jika diperlukan dan kemudian enkripsi file tersebut.

Setelah melakukan proses modifikasi dan enkripsi file twpol.txt dan twcfg.txt, lakukan inisialisasi database dari Tripwire. Inisialisasi database dengan menggunakan perintah:

{% highlight bash %}
# tripwire --init --cfgfile /etc/tripwire/tw.cfg  --polfile /etc/tripwire/tw.pol --site-keyfile /etc/tripwire/site.key  --local-keyfile /etc/tripwire/debian-local.key
{% endhighlight %}

untuk perintah debian-local key sesuaikan dengan **hostname** PC yang digunakan. Masukan key site passphrase. Pada awal inisialisasi, akan terdapat banyak warning dan error karena Tripwire tidak mempunyai data yang sama pada databasenya.

![Proses inisialisasi database Tripwire](https://wmo1ag-ch3301.files.1drv.com/y3pe8b17CPVBuvFwuUEH5C3oLJbvhULzEwFAk5VvcEWYFKDlDWAoeDgGKk_wpr_hntOC8L-td6i2sJx3Uy_YnepemSf-bAmLpdQg58n8Wt7gSwaQCXiC-xWC0YQQur4Rm_S/10.png?psid=1&rdrts=144160871)

Untuk mencheck pada sistem terhadap adanya perubahan pada file-file pada host, gunakan perintah:

{% highlight bash %}
# tripwire –check
{% endhighlight %}

![Berikut hasil pengecekan](https://wmo1ag-ch3302.files.1drv.com/y3ph1tUaj6bVHJr6zPtLhMN2-wD-jefgAfu9ua75IRDqy6NmnL7xt1-9Jq1ifSAGIgDz8UyC402e2a6FUNk-k2KUqbeQQTsflcHgGbq_OUsDPokZaaCyi06XFxLh7P4eptE/11.png?psid=1&rdrts=144160872)

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

![Mengupdate database tripwire](https://wmo1ag-ch3301.files.1drv.com/y3pgYX1ZwOEyIPBQIQI0iZr7FndejvAtZEdyiQFs-y9L4ErxCFOB5IftvYA74VMczLXf0Pi0TMEsgF-D10Cw4f3MZh0HxNjJf__MXRX-2g-9nxQda37XKRXucHmDzMKoaXU/12.png?psid=1&rdrts=144160872)

Untuk mencoba sedikit kerja Tripware dilakukan percobaan sebagai berikut:

Pertama lakukan Tripwire check.

![Hasilnya](https://wmo1ag-ch3301.files.1drv.com/y3pt7dU04XH5SmxYR-wjy-_o7L2wVzInb9Sc1SfqSTZGsJz-WK2C3NS3LRuJ-AoJZxJ_piByrCU-Ae7tPZO2c49Spg8MA8F73ylRVe4t9YzIstuvXBlRba9w6JY6JjgF02R/13.png?psid=1&rdrts=144160872)

Kemudian ubah file policy twpol.txt. Tambahkan beberapa tambahan berikut ini di baris paling bawah:

{% highlight bash %}
(
rulename = “Kirim Notifikasi ke email”,  severity = $(SIG_HI),  emailto = root@localhost
){}
{% endhighlight %}

Email akan dikirimkan ke akun email dari root dari system yang anda monitor. Biasanya, email akan ditujukan kea kun user yang dapat bertindak sebagai root.

![Perubuhan yang dimaksudkan](https://wmo1ag-ch3301.files.1drv.com/y3pEn81T_X5bId834P6qPjF6O7Rz_24IIe8jszTp1kWluKVOxWGqUylcpwWfrAlRr5iHDEAebjQIe1QIjplBF-_iLSmkys13cHIwalm5mQzBr-hXJYeqBt5iDNpHClmUN4N/14.png?psid=1&rdrts=144160872)

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

![Perubahan konfigurasi smpt Tripwire](https://wmo1ag-ch3302.files.1drv.com/y3pc3UXF8VBdw26HtSfSECyv2swpI-zua8P6ZbBBSCjeEviJ-uPBU9DprXdjyCHMd3kEWMDxY8qWQSxk-Y_6vdYmLF8U80dBHGkUIEEmuedXMfruGKdwabLTAbPnn-nzGIz/15.png?psid=1&rdrts=144160872)

Juga enkripsi kembali fil twcfg.txt menggunakan perintah:

{% highlight bash %}
# twadmin --create-cfgfile --cfgfile ./tw.cfg --site-keyfile ./site.key ./twcfg.txt
{% endhighlight %}

Jalankan test pengiriman email dengan menggunakan perintah:

{% highlight bash %}
# tripwire –test –email root@localhost
{% endhighlight %}

![Test email](https://wmo1ag-ch3302.files.1drv.com/y3pBTptwwhoVZuXh77pwxkvnfMtnaQtpanSxSxmdk2-Un_x6QpmZxvrKluX9-4AyNqboDhzDjhrXyQ7cS1YP95B_qsWotGcXszlb5H-fh3a1RO-VpvC2On4hD7FmZ9vJLkY/16.png?psid=1&rdrts=144160872)

Kemudian check email user

![Terdapat banyak email test karena email test yang dikirim berulang-ulang](https://wmo1ag-ch3302.files.1drv.com/y3pwluHZJydP6VE3juNlJrnYlseKTHwot5Koxt0jsSwh0C6yqD6lICwB7ngfPELtG-VoniuJNH2YRgsbEchwoYbKcwLIohjy1z7C5cjXxx0z-bWK8C-mkrdbz6rER4OJFa9/17.png?psid=1&rdrts=144160872)

Terdapat banyak email test karena email test yang dikirim berulang-ulang

Untuk pengetesan Buat sebuah file kosong . Kemudian salinlah ke dalam direktori /bin. Kemudian lakukan cek konsistensi.

![Hasil pengecekan](https://wmo1ag-ch3302.files.1drv.com/y3pYTtMuyLNlHWenOB4Mr6v5-YvbKGSmLw92APLgtb1Fob9pKA1J7PZxJguo2AT4ezYtUuJ-kTkIHT4ybKQUBeuEH4Oi-yP9hL9wLmLD4epnZVto8HPxdkPJUThG8Owch4C/18.png?psid=1&rdrts=144160872)

Hasilnya terlihat pada log tripwire yang menyatakan penambahan file dan pemodifikasian pada direktori root, kemudian dibeberapa keterangan dibawahnya menyebutkan warning terhadap penambahan file pada direktori root. Terdapat email pula yang memberikan warning kepada user. Email pada user terlihat pada email no 4 pada gambar sebelumnya. Yang berbeda dari hasil check tripwire adalah adanya tambahan pada keterangan apa yang bertambah dan dimodifikasi

![Hasil perubahan](https://wmo1ag-ch3302.files.1drv.com/y3pvixPeHLOGCUmCny1ekZa5mRyuCollLqHDo7vn0zNK_qYdUrC1HqRdGDcYYRDoFO4oZgF9eR_mlOy4OqoT6bqDNwLLZ-c1Vr8NoZPPAt-JlgwRc-rUAfkU6bgZ2rvRK7u/19.jpg?psid=1&rdrts=144160872)