# Laravel #

## Apa itu Laravel ##

### Sejarah Laravel ###

### Cara Instalasi Laravel ###

Laravel diinstal dengan menggunakan Composer, yaitu package management yang ada dalam PHP. Untuk itu, kita perlu menginstal Composer terlebih dulu.

Composer sudah menyediakan script instalasi di websitenya. Kita tinggal unduh dan jalankan. Perintah unduh dan jalankan script instalasi bisa dilakukan dalam satu baris seperti ini:

```
curl -sS https://getcomposer.org/installer | php
```

Bila dijalankan, script di atas akan menginstal Composer di project tempat kita berada saja (local install). Untuk menginstal secara global di keseluruhan system, pindahkan file `composer.phar` ke folder `/usr/local/bin`. Extension `.phar` juga kita bisa hilangkan.

```
sudo mv composer.phar /usr/local/composer
```

Setelah composer terinstall, kita bisa langsung membuat project Laravel tanpa perlu melakukan langkah tambahan.


## Membuat Project dengan Laravel ##

Laravel sudah ada dalam package management Composer dengan nama `laravel/laravel`. Kita tinggal suruh Composer untuk membuat project baru.

### Membuat Project Baru ###

Project aplikasi helpdesk ini akan kita buat di dalam folder `/var/www`. Jadi, kita pindah dulu ke folder tersebut

```
cd /var/www
```

Project baru dibuat dengan perintah berikut

```
composer create project laravel/laravel aplikasiticket
```


Selanjutnya, kita konfigurasi Nginx supaya bisa menjalankan aplikasi tersebut. Buat file `aplikasiticket` di dalam folder `/etc/nginx/sites-available` dengan isi sebagai berikut

```
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/aplikasiticket/public;
	index index.php index.html index.htm index.nginx-debian.html;

	server_name _;
	location / {
		try_files $uri $uri/ /index.php?$query_string;
	}
	include hhvm.conf;
}
```

Dalam file konfigurasi di atas, ada beberapa hal yang kita lakukan:

* Konfigurasi Nginx supaya berjalan di port 80
* Folder yang akan dipublish adalah `/var/www/aplikasiticket/public`, itu adalah folder di mana kita tadi membuat projectnya
* Tambahkan `index.php` sebagai default page
* Server kita bisa diakses dengan nama apa saja (`server_name _;`)
* Untuk mengaktifkan pretty URL, tambahkan konfigurasi `/index.php?$query_string`

Setelah konfigurasi selesai, kita bisa mencoba mengakses ke `http://namaserver-atau-ip`, dan kita akan mendapatkan tampilan seperti ini

![Laravel Hello World](img/laravel/laravel-install-success.png)


### Membuat Tampilan Sederhana ###

### Menjalankan aplikasi ###

### Mendeploy aplikasi ke server ###

## Membuat Tampilan dengan Laravel ##

### Controller dan View ###

### Menubar dan Navigasi ###

### Form Input ###

### List View ###

### Login dan Logout ###

## Akses Database dengan Laravel ##


### Instalasi Database ###

Dalam buku ini, kita akan menggunakan database PostgreSQL versi 9. Untuk menginstalnya di Ubuntu, kita ketikkan perintah berikut

```
sudo apt-get install postgresql -y
```

Perintah di atas akan menginstall PostgreSQL versi terbaru, yaitu 9.3 pada saat tulisan ini dibuat.

Selanjutnya, ada sedikit konfigurasi jaringan yang perlu kita lakukan. Edit file `/etc/postgresql/9.3/main/pg_hba.conf` menjadi seperti ini

```
local   all         all                           password
host    all         all         127.0.0.1/32      password
host    all         all         ::1/128           password
```

Konfigurasi di atas memungkinkan kita login dengan menggunakan username dan password, sedangkan konfigurasi sebelumnya (bawaan Ubuntu) mengharuskan kita login menggunakan username dan password sistem operasi.

Setelah file tersebut diedit, kita perlu merestart PostgreSQL agar konfigurasi tersebut aktif.

```
sudo service postgresql restart 
```

Kita perlu membuat user database untuk aplikasi kita. Pertama, kita ganti dulu user sistem operasi dari `root` menjadi `postgres`.

```
su - postgres
```

Kemudian, gunakan perintah `createuser` dengan opsi `-P` agar kita bisa mengisi password dan opsi `--interactive` agar kita bisa menentukan ijin akses user yang akan dibuat.

```
createuser --interactive -P
Enter name of role to add: tiket
Enter password for new role:
Enter it again:
Shall the new role be a superuser? (y/n) n
Shall the new role be allowed to create databases? (y/n) y
Shall the new role be allowed to create more new roles? (y/n) n 
```

Setelah selesai, keluar dari user `postgres` dan menjadi user biasa. Lalu jalankan perintah `createdb` untuk membuat database baru. Gunakan user tiket yang baru saja kita buat dengan opsi `-U`

```
createdb -U tiket dbtiket
Password:
```

Kita sudah bisa membuka database `dbtiket` dengan user `tiket` dan password sesuai yang kita masukkan.

```
psql -U tiket dbtiket
Password for user tiket: 
psql (9.3.10)
Type "help" for help.

dbtiket=> \d
No relations found.
```

Database kita siap dipakai.


### Desain skema database ###

Aplikasi kita akan memiliki beberapa tabel, yaitu:

* users : menyimpan username dan password
* m_product : menyimpan data produk
* t_ticket : menyimpan data laporan user terhadap produk tertentu
* t_comment : menyimpan data komentar untuk ticket tertentu
* t_attachment : menyimpan file screenshot/attachment untuk ticket tertentu
* t_ticket_history : menyimpan riwayat perubahan ticket tertentu

Masing-masing tabel memiliki beberapa kolom dan tipe data sebagai berikut:

* users

    * id int primary key
    * email varchar not null unique
    * password varchar not null
    * name varchar not null

* m_product

    * id int primary key
    * code varchar not null unique
    * name varchar not null

* t_ticket

    * id int primary key
    * title varchar not null
    * description varchar not null
    * created_date datetime not null,
    * created_by int not null references user(id)
    * status varchar not null

* t_comment

    * id int primary key
    * id_ticket int not null references t_ticket(id)
    * created_by int not null references user(id)
    * created_date datetime not null
    * comment varchar not null

* t_attachment

    * id int primary key
    * id_ticket int not null references t_ticket(id)
    * created_by int not null references user(id)
    * created_date datetime not null
    * file_location varchar not null

* t_ticket_history

    * id int primary key
    * id_ticket int not null references t_ticket(id)
    * old_status varchar not null
    * new_status varchar not null
    * created_by int not null references user(id)

### Konfigurasi koneksi database ###

Koneksi database dalam laravel diatur dalam file `aplikasi/config/database.php`. Ada beberapa baris yang harus diedit, sebagai berikut:

* jenis database

```php
'default' => env('db_connection', 'pgsql'),
```

* blok konfigurasi postgresql

```php
        'pgsql' => [
            'driver'   => 'pgsql',
            'host'     => env('db_host', 'localhost'),
            'database' => env('db_database', 'dbtiket'),
            'username' => env('db_username', 'tiket'),
            'password' => env('db_password', 'belajar'),
            'charset'  => 'utf8',
            'prefix'   => '',
            'schema'   => 'public',
        ],
```

Sesuaikan konfigurasi di atas dengan konfigurasi koneksi database kita. Pada contoh di atas, saya menggunakan:

* lokasi instalasi server database : `localhost`, artinya berada di mesin yang sama dengan aplikasi laravel kita
* nama database : `dbtiket`, sesuai yang kita buat pada penjelasan sebelumnya
* username database : `tiket`
* password database : `belajar`

Setelah ini, kita akan membuat script untuk menghasilkan skema database.

### Migrasi skema database dengan artisan ###

Laravel memiliki perangkat command line untuk membantu kita dalam membuat aplikasi, disebut dengan nama artisan. Salah satu kemampuan artisan adalah membuat script migrasi database. Migrasi database adalah suatu mekanisme untuk versioning database. Dengan menggunakan tools migrasi, kita bisa:

* membuat skema database
* mengubah skema database pada waktu aplikasi naik versi
* mengubah skema database bila ingin mengembalikan aplikasi ke versi sebelumnya

Script migrasi dibuat dengan perintah berikut

```
php artisan make:migration initial_schema
```

Output dari perintah di atas terlihat seperti ini

```
Created migration: 2015_11_02_021835_initial_schema
```

Perintah di atas akan menghasilkan file `2015_11_02_021835_initial_schema.php` dalam folder `database/migration`. Berikut isi file tersebut

```php
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class InitialSchema extends Migration
{
    /**
     * run the migrations.
     *
     * @return void
     */
    public function up() {
        //
    }

    /**
     * reverse the migrations.
     *
     * @return void
     */
    public function down() {
        //
    }
}
```

dalam file di atas, kita bisa melihat struktur dasar dari script migrasi database. ada dua function utama, yaitu `up` dan `down`. function `up` dijalankan untuk mengupgrade skema database kita dari versi aplikasi sebelumnya ke versi saat ini. demikian sebaliknya, function `down` dijalankan bila kita ingin mengembalikan (rollback) skema database sesuai dengan aplikasi versi sebelumnya.

untuk membuat tabel, kita gunakan function `Schema::create`. Tambahkan baris berikut di dalam function `up`

```php
Schema::create('m_product', function(Blueprint $table){
    // daftar kolom database
    $table->increments('id');
    $table->string('code')->unique();
    $table->string('name');

    // unique constraints
    $table->unique('code');
});
```

Jangan lupa untuk mengisi script rollback dalam function `down`

```php
Schema::drop('m_product');
```

Untuk tabel yang memiliki relasi, berikut contoh definisinya

```php
Schema::create('t_ticket', function(Blueprint $table){
    // daftar kolom database
    $table->increments('id');
    $table->string('title');
    $table->string('description');
    $table->timestamp('created_date');

    // foreign key constraints
    $table->integer('created_by')->unsigned();
    $table->foreign('created_by')
          ->references('id')
          ->on('users');
});
```


### Mengisi data awal (seeding) ###

Dalam sebagian besar aplikasi, biasanya untuk bisa berjalan dengan baik, ada data-data awal yang harus ada. Contohnya antara lain:

* tingkat pendidikan
* daftar provinsi, kabupaten, kecamatan, kelurahan, kode pos
* dan sebagainya

Untuk mengisi data awal tersebut, Laravel menyediakan fitur _seeder_. Kita buat script seeder dengan perintah sebagai berikut

```
php artisan make:seeder DataAwalSeeder
```

Perintah di atas akan menghasilkan file `DataAwalSeeder.php` dalam folder `database/seeds/`. Berikut isinya

```php
<?php

use Illuminate\Database\Seeder;

class DataAwalSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        //
    }
}
```

Dalam function `run`, kita isikan data awal yang kita inginkan. Ada dua pilihan metode yang disediakan:

* Query Builder
* Eloquent Model Factory

Kita akan gunakan metode Query Builder, karena Eloquent Model belum kita buat. Masukkan kode program berikut dalam method `run`

```php
DB::table('m_product')->insert([
    'code' => 'APP-001',
    'name' => 'Aplikasi Akunting'
]);

DB::table('m_product')->insert([
    'code' => 'APP-002',
    'name' => 'Aplikasi Payroll'
]);
```

Kode program di atas akan melakukan _insert_ ke dalam tabel `m_product` sebanyak 2 record.

Sekarang kita sudah memiliki script untuk membuat skema database dan mengisi data awal ke dalam tabel-tabelnya. Kita jalankan script migrasi dengan perintah berikut

```
php artisan migrate
```

Kemudian, kita jalankan perintah untuk menjalankan seeder agar tabel kita terisi

```
php artisan db:seed
```

Database sudah siap digunakan, mari kita lanjutkan untuk membuat kode akses database menggunakan Eloquent ORM.

### Apa itu Eloquent ORM ###

### Mapping Eloquent Model ke Tabel Databaes ###

### Relasi One to Many dengan Eloquent Model ###

## Menggabungkan Tampilan dengan Akses Database ##

### Menampilkan data di tabel ###

### Menggunakan dropdown list ###

### Menyimpan data ke database ###

### Menyimpan data berelasi ke database

### Referensi ###

* https://scotch.io/tutorials/a-guide-to-using-eloquent-orm-in-laravel
* http://laravelbook.com/laravel-migrations-managing-databases/

## Upload file ##

### Membuat Form Upload ###

### Lokasi penyimpanan hasil upload ###

### Menangani form upload ###

### Menampilkan hasil upload ###

## Security ##

### Skema database ###

### Implementasi Login ###

### Implementasi Logout ###

### Implementasi Ijin Akses ###



