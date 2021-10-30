# Jarkom-Modul-2-D03-2021

- Zahrotul Adillah (05111940000139)
- Muhammad Nur Abdurrauf (05111940000140)
- Aflah Hilmy (05111940000177)

## Daftar Isi

- [No 1](#no-1)
- [No 2](#no-2)
- [No 3](#no-3)
- [No 4](#no-4)
- [No 5](#no-5)
- [No 6](#no-6)
- [No 7](#no-7)
- [No 8](#no-8)
- [No 9](#no-9)
- [No 10](#no-10)
- [No 11](#no-11)
- [No 12](#no-12)
- [No 13](#no-13)
- [No 14](#no-14)
- [No 15](#no-15)
- [No 16](#no-16)
- [No 17](#no-17)

## No 1
### Soal
EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet
### Penjelasan Jawaban

## No 2
### Soal
Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku
### Penjelasan Jawaban

## No 3
### Soal
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie
### Penjelasan Jawaban

## No 4
### Soal
Buat juga reverse domain untuk domain utama
### Penjelasan Jawaban

## No 5
### Soal
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama
### Penjelasan Jawaban

## No 6
### Soal
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo
### Penjelasan Jawaban

## No 7
### Soal
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie
### Penjelasan Jawaban

## No 8
### Soal
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com
### Penjelasan Jawaban
Di Skypie, menjalankan `apt-get update` dan `apt-get install apache2 php libapache2-mod-php7.0 -y`. 
![Screenshot (483)](https://user-images.githubusercontent.com/74708771/139522227-a2568b9b-b8b8-4294-9c83-eff1aa6daf34.png)
Setelah itu mendownload file yang dibutuhkan dengan command `wget --no-check-certificate https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/franky.zip`, `wget --no-check-certificate https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip`, dan `wget --no-check-certificate https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/general.mecha.franky.zip`, lalu di-unzip
![Screenshot (484)](https://user-images.githubusercontent.com/74708771/139522233-da7724e3-f8b6-4624-9a08-1a149e4509d1.png)
Di dalam folder /etc/apache2/sites-available/, copy file 000-default.conf menjadi franky.D03.com.conf. 
![Screenshot (485)](https://user-images.githubusercontent.com/74708771/139522253-3970fc28-88d1-4ee3-a2a2-fac4165aca1d.png)
Di dalam file franky.D03.com.conf, ubah isinya menjadi seperti berikut:
![Screenshot (487)](https://user-images.githubusercontent.com/74708771/139522292-d5d8acfb-df16-4992-bf58-7451687b5f16.png)
Copy folder franky yang telah diunzip di root ke /var/www/franky.D03.com `cp -r /root/franky /var/www/franky.D03.com`.
![Screenshot (492)](https://user-images.githubusercontent.com/74708771/139522359-0bfcd78a-291c-484c-a683-40b6505b64ba.png)
Start site tersebut `a2ensite franky.D03.com` lalu restart service apache 2 `service apache2 restart`.
![Screenshot (494)](https://user-images.githubusercontent.com/74708771/139522368-b16a8ac8-0313-4c8a-84e0-9a6b2bea7f58.png)

Hasil website dibuka di client Loguetown dengan `lynx www.franky.D03.com`.
![Screenshot (478)](https://user-images.githubusercontent.com/74708771/139522186-1b1e0e03-a109-45be-9494-a20dabcea97b.png)


## No 9
### Soal
Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home
### Penjelasan Jawaban
Di dalam Skypie, jalankan command `a2enmod rewrite`.
![Screenshot (497)](https://user-images.githubusercontent.com/74708771/139522385-eb970766-3e62-49bb-acb4-ed8656bc4cf4.png)
Restart service apache 2 `service apache2 restart`.
![Screenshot (496)](https://user-images.githubusercontent.com/74708771/139522399-464ee2f0-2e40-48c8-8a62-64944540c4a8.png)
Buat file .htacces di dalam folder /var/www/franky.D03.com dengan isi seperti berikut:
![Screenshot (498)](https://user-images.githubusercontent.com/74708771/139522393-9454639d-08e1-479b-ae8e-4bcb68925b6a.png)
Di dalam file /etc/apache2/sites-available/franky.D03.com.conf tambahkan menjadi seperti berikut:
![Screenshot (488)](https://user-images.githubusercontent.com/74708771/139522302-52596f0f-885e-4c19-8e8c-dbb7ca3fee15.png)
Restart service apache 2 `service apache2 restart`.
![Screenshot (496)](https://user-images.githubusercontent.com/74708771/139522399-464ee2f0-2e40-48c8-8a62-64944540c4a8.png)

Hasil website dibuka di client Loguetown dengan `lynx www.franky.D03.com/home`.
![Screenshot (479)](https://user-images.githubusercontent.com/74708771/139522188-f4de71dd-c44d-412b-b6b1-7ecccea3b8ee.png)

## No 10
### Soal
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com
### Penjelasan Jawaban
Di dalam Skypie, di dalam folder /etc/apache2/sites-available/, copy file 000-default.conf menjadi super.franky.D03.com.conf.
![Screenshot (486)](https://user-images.githubusercontent.com/74708771/139522279-46117bd3-f4de-4c10-bd31-730eacf6c438.png)
Di dalam file super.franky.D03.com.conf, ubah isinya menjadi seperti berikut:
![Screenshot (489)](https://user-images.githubusercontent.com/74708771/139522336-245f5936-9a79-4d4d-b682-44ea9917d728.png)
Copy folder super.franky yang telah diunzip di root ke /var/www/super.franky.D03.com `cp -r /root/super.franky /var/www/super.franky.D03.com`.
![Screenshot (493)](https://user-images.githubusercontent.com/74708771/139522363-1871e99d-4be1-4fd4-80fb-062eb50c0836.png)
Start site tersebut `a2ensite super.franky.D03.com` lalu restart service apache 2 `service apache2 restart`.
![Screenshot (495)](https://user-images.githubusercontent.com/74708771/139522375-bababec0-06b7-4c7d-80f6-e024d2d100a6.png)

Hasil website dibuka di client Loguetown dengan `lynx www.super.franky.D03.com`.
![Screenshot (480)](https://user-images.githubusercontent.com/74708771/139522199-59e1c2d4-11ef-498c-a97d-cb1dc5529680.png)

## No 11
### Soal
Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja
### Penjelasan Jawaban
Di dalam file /etc/apache2/sites-available/super.franky.D03.com.conf tambahkan menjadi seperti berikut:
![Screenshot (490)](https://user-images.githubusercontent.com/74708771/139522343-2466443f-caac-43a7-a7da-feb7e28aa993.png)
Restart service apache 2 `service apache2 restart`.
![Screenshot (496)](https://user-images.githubusercontent.com/74708771/139522399-464ee2f0-2e40-48c8-8a62-64944540c4a8.png)

Hasil website dibuka di client Loguetown dengan `lynx www.super.franky.D03.com/public`.
![Screenshot (481)](https://user-images.githubusercontent.com/74708771/139522202-0815a8bb-9359-494a-b48c-42093c632bcc.png)

## No 12
### Soal
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache
### Penjelasan Jawaban
Di dalam file /etc/apache2/sites-available/super.franky.D03.com.conf tambahkan menjadi seperti berikut:
![Screenshot (491)](https://user-images.githubusercontent.com/74708771/139522345-6969c10d-d47e-4d89-b4cf-27b3981ef000.png)
Restart service apache 2 `service apache2 restart`.
![Screenshot (496)](https://user-images.githubusercontent.com/74708771/139522399-464ee2f0-2e40-48c8-8a62-64944540c4a8.png)

Hasil website dibuka di client Loguetown dengan `lynx www.super.franky.D03.com/terserah`.
![Screenshot (482)](https://user-images.githubusercontent.com/74708771/139522215-bc7302af-83f3-4f7d-a66a-f250cee5fe5e.png)

## No 13
### Soal
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js
### Penjelasan Jawaban

## No 14
### Soal
Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500
### Penjelasan Jawaban

## No 15
### Soal
dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy
### Penjelasan Jawaban

## No 16
### Soal
Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com
### Penjelasan Jawaban

## No 17
### Soal
Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png
### Penjelasan Jawaban
