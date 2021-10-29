# Jarkom-Modul-1-D03-2021

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

## No 9
### Soal
Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home
### Penjelasan Jawaban

## No 10
### Soal
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com
### Penjelasan Jawaban

## No 11
### Soal
Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja
### Penjelasan Jawaban

## No 12
### Soal
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache
### Penjelasan Jawaban

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
