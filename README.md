# Membuat Aplikasi dengan Laravel dan HHVM #

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


Fitur Aplikasi :

* Umum

    * Login
    * Logout
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

