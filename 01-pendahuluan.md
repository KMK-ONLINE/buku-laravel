# Pendahuluan #

## Tentang isi buku ini ##

### Apa itu HHVM ###

HHVM adalah singkatan dari HipHop Virtual Machine, yaitu compiler dan runtime untuk menjalankan kode program yang ditulis dalam bahasa PHP dan Hack. HHVM dibuat oleh Facebook. Awalnya Facebook membuat HPHPc, yaitu compiler untuk mengubah kode program PHP menjadi C, tujuannya supaya PHP bisa jalan dengan lebih cepat.

Pada perkembangannya, HPHPc ini dilengkapi dengan HPHPi (interpreter) dan HPHPd (debugger). Tapi pada akhirnya, semua ini digantikan dengan HHVM sebagai satu produk yang lengkap.

Singkat cerita, HHVM adalah runtime engine yang bisa menjalankan kode program PHP dan Hack.

### Apa itu Laravel ###

Laravel adalah framework untuk membuat aplikasi web di PHP dengan menggunakan paradigma Model-View-Controller (MVC). Laravel bisa berjalan dengan lancar dalam HHVM. 

### Tujuan buku ini ###

Buku ini ditujukan bagi mereka yang ingin membuat aplikasi web dengan menggunakan framework Laravel dan dijalankan di atas HHVM. Setelah membaca buku ini, pembaca diharapkan :

* mampu membuat struktur project Laravel
* mampu membuat aplikasi yang berfungsi lancar
* mampu menggunakan modul dan library tambahan yang dibutuhkan
* mampu menginstal dan mengkonfigurasi server HHVM
* mampu mendeploy aplikasi ke server

### Susunan Buku ###

Buku ini dibagi menjadi beberapa bab, yaitu:

1. Pendahuluan
2. HHVM dan Hack Language : membahas tentang seluk beluk HHVM, cara instalasinya, apa itu Hack Language, apa bedanya dengan PHP, serta berbagai sintaks dan teknik pemrograman lainnya.
3. Laravel : membahas tentang seluk beluk Laravel, cara instalasinya, dan cara membuat aplikasi dengan Laravel. Juga dibahas tentang cara deploymentnya.

## Tentang segmen pembaca ##

Pembaca diasumsikan sudah memahami:

* Dasar-dasar HTML dan CSS
* Konsep dasar pemrograman (variabel, kondisional, perulangan, function, class)
* Konsep dasar protokol HTTP (get, post, session, dsb)

## Software yang dibutuhkan ##

Untuk mengikuti pembahasan di buku ini, kita akan menggunakan:

* Sistem Operasi Ubuntu 14.04
* Database server : PostgreSQL 9
* Text Editor : apapun boleh, mau yang sederhana seperti Notepad++, Vim, Sublime Text, ataupun yang canggih seperti Eclipse, Netbeans, Webstorm

## Studi Kasus ##

Dalam buku ini, kita akan membuat aplikasi helpdesk. Aplikasi ini digunakan
untuk memasukkan pertanyaan, keluhan, atau saran terhadap penggunaan suatu
aplikasi.

Aplikasi helpdesk ini memiliki tiga jenis user:

* Customer : orang yang memasukkan pertanyaan, keluhan, atau saran (dalam
aplikasi ini disebut dengan istilah ticket)
* Agent : petugas helpdesk yang bertugas menanggapi customer. Helpdesk biasanya
tidak mengatasi sendiri masalahnya, tapi melemparkan ke personel support yang
sesuai.
* Support : petugas yang sebenarnya menangani masalah. Menerima penugasan dari
agent dan kemudian mengupdate status ticket.


### Fitur Aplikasi ###

* Umum

  * Login
  * Logout
  * Dashboard / Welcome Page
  * menerima notifikasi bila status ticket berubah

* Customer

  * input ticket dengan beberapa field : produk/aplikasi (combo), judul,
penjelasan, screenshot (bila ada)
  * melihat status ticket


* Agent

  * melihat daftar ticket yang baru masuk
  * memilih support yang akan menangani ticket
  * menentukan tingkat prioritas ticket (dengan melihat penjelasan)

* Support

  * melihat daftar ticket yang ditugaskan kepadanya
  * update status dan komentar ticket

Mockup Tampilan Aplikasi

* Login Screen

![Login Screen](img/mockup/login.png)

* Dashboard

![Mockup Dashboard](img/mockup/dashboard.png)

* Input Tiket

![Mockup Entri Tiket](img/mockup/laporkan_masalah.png)

* Daftar Tiket

![Mockup daftar tiket](img/mockup/daftar_problem.png)

* History Tiket

![Mockup Riwayat Masalah](img/mockup/riwayat_masalah.png)


## Lisensi ##

Buku ini berlisensi Creative Commons 4.0 CC-BY-SA. Artinya, semua orang boleh menggunakan, mengubah isinya, membuat turunannya, bahkan menggunakan untuk keperluan komersial, dengan catatan:

* nama penulis asli tetap dicantumkan dalam perubahan
* lisensi pada hasil perubahan tetap sama (yaitu CC-BY-SA)

Lebih detail tentang lisensi ini bisa dibaca di websitenya https://creativecommons.org/licenses/
