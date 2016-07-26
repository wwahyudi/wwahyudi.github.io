---
layout:     post
title:      Mempercepat Booting Windows 7
date:       2012-04-01
summary:    Pengalaman pribadi saya untuk mengoptimasi booting Windows 7
categories: windows
---

Anda merasa booting proses Windows 7 Anda lelet? Cobalah untuk mengoptimasi proses bootingnya. Tips ini berdasarkan pengalaman yang saya dapat saat meng-uprek Windows 7 saya saat semester satu kemarin. Berikut tindakan yang saya lakukan untuk mempercepat booting Windows 7 saya:

### Startup Application

Startup Application adalah salah satu yang memperlambat booting Windows dikarenakan aplikasi-aplikasi tersebut dieksekusi saat Windows sedang mempersiapkan Logon Screen dan Desktop Enviroment. Untuk men-setting, Buka Run, ketikkan msconfig, klik run kemudian pilih tab Startup. Nonaktifkan program-program dengan menghilangkan centang pada Startup Item. Di beberapa tutorial di internet, beberapa perberi tutorial berkata bahwa harus mematikan semua, tetapi menurut saya sebaiknya, matikan startup item yang tidak diperlukan saja, dan tinggalkan startup item seperti, driver-driver dan aplikasi yang mamang Anda perlukan saat startup. Hal tersebut diperlukan agar driver-driver tetap dieksekusi sehingga Anda tidak perlu eksekusi driver-driver tersebut.

### Paksa semua core procesor Anda bekerja saat booting

Saat booting, Windows akan menyesuaikan kebutuhan core prosesor. Jika Anda menggunakan komputer dengan multi core tidak ada salahnya memaksa Windows menggunakan semua core prosesor yang ada miliki untuk booting. Penagturan dapat dilakukan pada pada tab Boot pada Advance Option msconfig.

### Optimasi penggunaan paging file Anda

Semakin besar paging file (dan RAM, tentunya) akan mempercepat jalannya booting, dikarenakan saat booting tidak semua proses dimasukan kedalam RAM sehingga beberapa proses dimasukan ke dalam panging file. Pengaturan dapat dilakukan di Advance System Settings, kemudian pilih. Setting pada frame perfomance, kemudian pilih tab Advance dan klik Change pada Virtual memory. Pilih Custom size, isikan batas minimum berupa nilai currently Allocated, dan Maximum size berupa nilai Maximum recommended.

### Menonaktifkan Windows Visual Effect

Tindakan ini berdasarkan pengalaman teman saya, dia mematikan semua efek visual Windows sehingga Windows 7-nya botting lebih cepat.

### Menggunakan Disk Cleanup unutk membersihkan beberapa file temporary, cache, dan yang lainnya.

### Mengganti hardisk ke RPM yang lebih tinggi atau dengan SSD

Sekian dari saya tentang pengalaman saya mempercepat booting Windows. Terima kasih.