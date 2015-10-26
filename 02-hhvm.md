# HHVM #

## Apa itu HHVM ##

HHVM adalah singkatan dari HipHop Virtual Machine. Ini adalah suatu virtual machine yang bisa digunakan untuk menjalankan kode program PHP dengan lebih cepat. Selain bahasa pemrograman PHP, HHVM juga bisa menjalankan bahasa pemrograman Hack.

HHVM ini dibuat dan dimaintain oleh Facebook.

### Sejarah HHVM ###

Pada jaman dahulu, seiring dengan semakin besarnya Facebook, maka kebutuhan terhadap performance semakin mendesak. Untuk lebih mengoptimalkan penggunaan hardware, maka Facebook membutuhkan sesuatu yang lebih baik daripada `mod_php` yang biasa kita pakai.

Awalnya, Facebook mencoba mengkompilasi kode program PHP menjadi kode program C++, sehingga bisa lebih optimal memanfaatkan resource hardware. Akan tetapi setelah dipakai beberapa saat, programmer menjadi tidak produktif karena waktu kompilasinya lama. 

Selanjutnya, Facebook mencoba pendekatan berbeda, yaitu membuat interpreter, yang bisa menjalankan kode PHP langsung tanpa harus kompilasi. Kompilasi baru dilakukan setelah proses development selesai. Interpreter ini disebut dengan istilah HPHPi. Bersamaan dengan itu, dibuat juga debuggernya yang diberi nama HPHPd.

Seiring berjalan waktu, pendekatan kompilasi ternyata dipandang kurang efektif, karena ada satu langkah lagi sebelum kode program bisa dijalankan. Facebook akhirnya mengambil pendekatan Virtual Machine yang bisa langsung menjalankan kode program apa adanya. Inilah HHVM yang kita kenal sekarang ini.

### Keunggulan HHVM ###

Keunggulan utama tentu saja ada di sisi performance. Kalau kita cari di Google dengan keyword HHVM performance, tentu kita akan menemukan banyak orang yang sudah melakukan benchmarking. Tidak perlu kita kutip di sini, karena hasilnya selalu berubah. Inovasi HHVM akan diikuti oleh inovasi Zend engine dan Zephir, sehingga dari sisi performance, selalu berkejar-kejaran.

Keunggulan yang lebih penting adalah adanya bahasa Hack yang diciptakan oleh Facebook untuk berjalan di atas HHVM. 

### Hack Language ###

Bahasa pemrograman Hack adalah pengembangan dari bahasa PHP yang dibuat oleh Facebook. Ada beberapa penambahan fitur dibanding PHP, diantaranya:

* Type Annotation : kita bisa menulis tipe data untuk variabel dan return value dari method. Contohnya, bila biasanya di PHP kita menulis `var $a;`, maka di Hack, kita menulis `string $a;`
* Generics : kita bisa memberikan parameter untuk tipe data. Mirip dengan fitur generics di Java dan C#
* Tipe data null
* Collection : Set, Vector, Pair
* Lambda : memungkinkan function dijadikan parameter ke dalam function lain
* dan masih banyak lagi, yang bisa dibaca di websitenya http://hacklang.org

> Lho, kenapa pakai deklarasi tipe data? Nanti malah capek ngetik, kayak programmer Java.

Deklarasi tipe data, walaupun terlihat merepotkan, ternyata meningkatkan produktifitas pemrograman.

> Kenapa begitu?

Karena bila kita melakukan verifikasi tipe data di tahap kompilasi, error akan lebih cepat ditemukan daripada verifikasi pada waktu aplikasi dijalankan (runtime). Semakin cepat error kita temukan, semakin murah ongkos perbaikannya, karena kita masih ingat apa yang baru saja kita ketik. Bila error ini terbawa ke runtime, bisa jadi baru ditemukan berbulan-bulan kemudian. Pada saat itu, kita sudah lupa kapan kita tulis, mengapa kita tulis, apa relasinya dengan modul-modul lain, dan berbagai informasi lain yang penting untuk bisa memperbaiki error. 

Baiklah, bahasa Hack lebih bagus, HHVM lebih kencang. Lalu bagaimana cara pakainya? Mari kita lanjutkan ke instalasi.

## Instalasi HHVM ##

Untuk menyederhanakan pembahasan, kita cuma akan membahas instalasi di Ubuntu 14.04.

Ada beberapa tahap dalam instalasi, yaitu:

* instalasi Nginx
* instalasi HHVM
* konfigurasi HHVM agar bisa dijalankan oleh Nginx
* test instalasi

### Instalasi Nginx ###

Nginx adalah aplikasi HTTP server yang nantinya bertugas menerima request HTTP dari client. Setelah request diterima, selanjutnya akan diserahkan kepada HHVM untuk dieksekusi. Hasil eksekusi dikembalikan kepada Nginx untuk diteruskan ke client.

Sebetulnya kita juga bisa pakai HTTP server lain seperti Apache atau lighttpd. Untuk menyederhanakan pembahasan, kita cuma bahas yang paling umum digunakan orang, yaitu Nginx.

Agar kita mendapatkan versi Nginx terbaru, kita tambahkan dulu PPA

```
sudo add-apt-repository -y ppa:nginx/stable
sudo apt-get update
sudo apt-get install -y nginx
```

Setelah selesai, Nginx akan bisa diakses di port 80. Coba test dengan membuka browser dan mengakses `http://alamat-ip-server/`.

### Instalasi HHVM ###

HHVM memiliki software repository sendiri, untuk itu kita perlu menambahkan public key dari repository tersebut

```
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0x5a16e7281be7a449
```

Setelah itu, kita tambahkan lokasi repository

```
sudo add-apt-repository "deb http://dl.hhvm.com/ubuntu $(lsb_release -sc) main"
```

Barulah kita lakukan instalasi

```
sudo apt-get update
sudo apt-get install hhvm
```

### Integrasi HHVM dengan Nginx ###

HHVM sudah membuatkan script untuk konfigurasi Nginx. Kita tinggal jalankan saja

```
sudo /usr/share/hhvm/install_fastcgi.sh
```

HHVM akan memeriksa apakah sudah ada Nginx atau Apache HTTPD yang sudah terinstal. Bila ditemukan, maka HHVM akan membuatkan konfigurasi untuk webserver tersebut.

### Test Instalasi ###

Untuk Ubuntu 14.04, secara default lokasi aplikasi web ada di folder `/var/www/html`. Jadi dalam folder inilah kita menaruh kode program kita. Ketikkan kode program berikut

```php
<?php phpinfo();
```

Simpan dengan nama `halo.php`. Selanjutnya, akses dari browser dengan alamat `http://ip-server/halo.php`

Bila instalasi berjalan benar, maka kita akan mendapatkan tampilan seperti ini

![PHP Info dengan HHVM](img/hhvm/phpinfo.png)

Instalasi selesai, berikutnya kita akan mempelajari dasar-dasar bahasa pemrograman Hack.

### Referensi ###

* https://blog.engineyard.com/2014/hhvm-hack
* http://fideloper.com/hhvm-nginx-laravel
* https://github.com/facebook/hhvm/wiki/Getting-Started
* https://github.com/facebook/hhvm/wiki/Prebuilt-Packages-for-HHVM

## Memahami Hack Language ##

### Statement dan Comment ###

### Variabel dan Tipe Data ###

### Conditional ###

### Looping ###

### Function dan Lambda ###

### Class dan Object ###

