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
- Ketikkan `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s [Prefix IP].0.0/16` pada root router Foosha
- Melihat IP DNS pada foosha dengan mengetikkan `cat /etc/resolv.conf`
- Pada setiap node, menambahkan IP server foosha dalam `/etc/resolv.conf` dengan command `echo nameserver [IP DNS] > /etc/resolv.conf`
- Tes dengan ping google.com pada client server
- ![image](https://user-images.githubusercontent.com/72771774/139524776-0f3a9eb0-ad38-4a50-a444-9e56a50383b0.png)
- Isi `setup.sh`
- ![image](https://user-images.githubusercontent.com/72771774/139524874-9c550fd1-91c2-4e7c-9750-dbf08c9e273e.png)
- ![image](https://user-images.githubusercontent.com/72771774/139524944-f4464162-349e-4916-a750-49ece9535ddd.png)

## No 2
### Soal
Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku
### Penjelasan Jawaban
- Menambahkan konfigurasi di bawah kepada file /etc/bind/named.conf.local
`zone "franky.D03.com" {
	type master;
	file "/etc/bind/kaizoku/franky.D03.com";
};`
-  Membuat folder kaizoku dengan command `mkdir /etc/bind/kaizoku`
-  Copy file db.local ke franky.D03.com dengan command `cp /etc/bind/db.local /etc/bind/kaizoku/franky.D03.com`. Namun, dalam pengerjaan dapat langsung diletakkan ke dalam `setup.sh` pada root
-  Edit file `/etc/bind/kaizoku/franky.D03.com`
- ![image](https://user-images.githubusercontent.com/72771774/139524551-6c3e00b7-b875-46e4-a354-e1df37f7d18e.png)
-  Restart dengan command `service bind9 restart`
-  Ping franky.D03.com pada client server. Keterangan : nameserver pada `/etc/resolv.conf` diganti dengan IP EniesLobby sebagai DNS Masternya
- ![image](https://user-images.githubusercontent.com/72771774/139524816-dd93384e-5535-4d09-9abd-da274a253fb4.png)

## No 3
### Soal
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie
### Penjelasan Jawaban
- Edit file `/etc/bind/kaizoku/franky.D03.com` seperti berikut
- ![image](https://user-images.githubusercontent.com/72771774/139525022-e0e9d616-cf67-45e8-9a60-39bd2366f634.png)
- Restart service `service bind9 restart`
- Testing pada client server
- ![image](https://user-images.githubusercontent.com/72771774/139525991-082d843b-553f-4320-8a24-2321426b7fa4.png)

## No 4
### Soal
Buat juga reverse domain untuk domain utama
### Penjelasan Jawaban
- Tambahkan konfigurasi di bawah ini ke dalam `/etc/bind/named.conf.local`
`zone "2.193.192.in-addr.arpa" {
    type master;
    file "/etc/bind/kaizoku/2.193.192.in-addr.arpa";
};`
- Copy file db.local ke 2.40.10.in-addr.arpa dengan command `cp /etc/bind/db.local /etc/bind/kaizoku/2.40.10.in-addr.arpa` atau bisa langsung di setup. Keterangan 2.193.192 adalah 3 byte pertama IP EniesLobby yang dibalik urutan penulisannya
- Lalu edit file `/etc/bind/kaizoku/2.40.10.in-addr.arpa`
- ![image](https://user-images.githubusercontent.com/72771774/139525239-5825cfb1-afa0-444e-983c-af4353fc24bd.png)
- Restart service `service bind9 restart`
- Testing pada client server
`// Install package dnsutils
// Pastikan nameserver di /etc/resolv.conf telah dikembalikan sama dengan nameserver dari Foosha
apt-get update
apt-get install dnsutils

//Kembalikan nameserver agar tersambung dengan EniesLobby
host -t PTR "IP EniesLobby"`
- ![image](https://user-images.githubusercontent.com/72771774/139525259-721e0fe0-d8ec-41c0-b6d7-99ce6efb6edb.png)

## No 5
### Soal
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama
### Penjelasan Jawaban
#### EniesLobby
- Edit file `/etc/bind/named.conf.local`
`zone "franky.D03.com" {
    type master;
    notify yes;
    also-notify { 192.193.2.3; }; // Masukan IP Water7 
    allow-transfer { 192.193.2.3; }; // Masukan IP Water7 
    file "/etc/bind/kaizoku/franky.D03.com";
};`
![image](https://user-images.githubusercontent.com/72771774/139525341-6c045ef6-cd74-4b35-9e6c-5aa07ef06994.png)
- Restart service `service bind9 restart`
#### Water7
- Edit file `/etc/bind/named.conf.local`. Keterangan : jangan lupa install bin9 dengan `apt-get install bind9 -y`.
`zone "franky.D03.com" {
    type slave;
    masters { 192.193.2.2; }; // Masukan IP EniesLobby 
    file "/var/lib/bind/franky.D03.com";
};`
- ![image](https://user-images.githubusercontent.com/72771774/139525423-abe7a9d6-2195-4990-b0e9-1edce0b05d41.png)
- Restart service `service bind9 restart`
### Testing 
- stop service bin9 pada EniesLobby `service bind9 stop`
- Pastikan pengaturan nameserver pada `/etc/resolv.conf` mengarah ke IP EniesLobby dan IP Water7 di dalam client servernya
- Ping franky.D03.com 
- ![image](https://user-images.githubusercontent.com/72771774/139525544-bbd34ad1-7c09-47f4-9f40-4ab09ccbe56e.png)

## No 6
### Soal
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo
### Penjelasan Jawaban
#### EniesLobby
- Edit `/etc/bind/kaizoku/franky.D03.com`
`
ns1     IN      A       192.193.2.3 ;IP Water7
mecha   IN      NS      ns1
`
- ![image](https://user-images.githubusercontent.com/72771774/139525756-d581ea19-1644-4688-94fe-6c2d5e7a3d9b.png)
- Edit `/etc/bind/named.conf.options` dengan **comment** `dnssec-validation auto;` dan menambahkan `allow-query{any;};`
- Edit `/etc/bind/named.conf.local`
`zone "franky.D03.com" {
    type master;
    file "/etc/bind/kaizoku/franky.D03.com";
    allow-transfer { 192.193.2.3; }; // Masukan IP Water7
};`
- ![image](https://user-images.githubusercontent.com/72771774/139525648-bf8e95ab-3b66-437e-959a-fb9260e14220.png)
- Restart service `service bind9 restart`
#### Water7
- Edit `/etc/bind/named.conf.options` dengan **comment** `dnssec-validation auto;` dan menambahkan `allow-query{any;};`
- Edit `/etc/bind/named.conf.local`
`zone "mecha.franky.D03.com" {
        type master;
        file "/etc/bind/sunnygo/mecha.franky.D03.com";
};`
- ![image](https://user-images.githubusercontent.com/72771774/139525690-19fecb55-c88f-4561-8fd1-57fd8909f59c.png)
- Membuat folder sunnygo dengan command `mkdir /etc/bind/sunnygo`
-  Copy file db.local ke mecha.franky.D03.com dengan command `cp /etc/bind/db.local /etc/bind/sunnygo/mecha.franky.D03.com`. Namun, dalam pengerjaan dapat langsung diletakkan ke dalam `setup.sh` pada root
-  Edit `mecha.franky.D03.com`
- ![image](https://user-images.githubusercontent.com/72771774/139525853-d4add5e7-74c6-4f5d-a3f7-72b5c5829d60.png)
-  Restart service `service bind9 restart`
#### Testing
![image](https://user-images.githubusercontent.com/72771774/139525909-4321c099-3312-43e4-91f0-c35f308a176c.png)

## No 7
### Soal
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie
### Penjelasan Jawaban
- Edit `mecha.franky.D03.com` pada Water7
- ![image](https://user-images.githubusercontent.com/72771774/139525923-db9d797a-186e-42ff-98eb-dc34da872544.png)
- Restart service `service bind9 restart`
- Testing pada client server
- ![image](https://user-images.githubusercontent.com/72771774/139526218-b8774384-a808-4171-a09d-1ab6cb706ec4.png)

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
Buka kembali file `/etc/apache2/sites-available/super.franky.D03.com.conf` dan tambahkan:
```bash
Alias "/js" "/var/www/super.franky.D03.com/public/js"
```
menjadi seperti berikut:
![image](https://user-images.githubusercontent.com/29938033/139524024-e2d3d0b5-399d-4c62-b3b6-c50bd8b9f57b.png)
Kemudian restart webserver dengan command `service apache2 restart`.

Tampilan website ketika dibuka di client Loguetown dengan `lynx www.super.franky.D03.com/js`:
![image](https://user-images.githubusercontent.com/29938033/139524131-26ddefbc-6c02-4fd9-9570-525daf2ce7a8.png)

## No 14
### Soal
Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500
### Penjelasan Jawaban
di folder `/etc/apache2/sites-available/`, copy file `000-default.conf` menjadi `general.mecha.franky.D03.com.conf` lalu ubah tag pembuka `VirtualHost` menjadi:
```bash
<VirtualHost *:15000 *:15500>
```
Ubah juga nilai `DocumentRoot` dan tambahkan nilai `ServerName` dan `ServerAlias` menjadi:
```bash
ServerAdmin webmaster@localhost
DocumentRoot /var/www/general.mecha.franky.D03.com
ServerName general.mecha.franky.D03.com
ServerAlias www.general.mecha.franky.D03.com
```
Sehingga file confignya menjadi seperti berikut:
![image](https://user-images.githubusercontent.com/29938033/139524406-57bc8fa7-788b-4ab4-8e8d-ba0852ca187a.png)

Kemudian buka file `/etc/apache2/ports.conf` dan tambahkan:
```bash
Listen 15000
Listen 15500
```
sehingga filenya menjadi seperti berikut:
![image](https://user-images.githubusercontent.com/29938033/139524549-96d010c1-ef1d-4ed7-8c50-44b964aed29a.png)
Aktifkan website dengan menjalankan `a2ensite general.mecha.franky.D03.com` dan kemudian restart webserver dengan command `service apache2 restart`.

Tampilan website ketika dibuka di client Loguetown dengan `lynx www.general.mecha.franky.D03.com:15000`:
![image](https://user-images.githubusercontent.com/29938033/139524510-dcbcaad4-7689-4098-850b-bbb7962b6a3d.png)

## No 15
### Soal
dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy
### Penjelasan Jawaban
Buatlah file `.htpasswd` yang akan menyimpan username dan password dengan command `htpasswd -b -c "/etc/apache2/.htpasswd" luffy onepiece`
Kemudian buka kembali file `/etc/apache2/sites-available/general.mecha.franky.D03.com.conf` dan tambahkan:
```bash
<Directory "/var/www/general.mecha.franky.D03.com">
    AuthType Basic
    AuthName "Restricted Content"
    AuthUserFile /etc/apache2/.htpasswd
    Require valid-user
</Directory>
```
sehingga filenya menjadi seperti berikut:
![image](https://user-images.githubusercontent.com/29938033/139524683-3d3af9e7-1209-4f5f-a4ff-7639998dcd71.png)
Kemudian restart kembali webserver dengan `service apache2 restart`

Tampilan website ketika dibuka di client Loguetown dengan `lynx www.general.mecha.franky.D03.com:15000`:
![image](https://user-images.githubusercontent.com/29938033/139524702-d74b5449-b15f-4003-8ede-d67c9d0eeb27.png)
![image](https://user-images.githubusercontent.com/29938033/139524721-ac301a6c-9adf-442c-b8f5-332bcd38b7b2.png)
![image](https://user-images.githubusercontent.com/29938033/139524728-c30fb6d4-7b4f-4ce8-be9a-57ea70ce7ec9.png)
website akan meminta username dan password sebelum membuka directory

## No 16
### Soal
Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com
### Penjelasan Jawaban
Buka file `/etc/apache2/sites-available/000-default.conf` dan tambahkan:
```Bash
Redirect 301 / http://franky.D03.com/
```
sehingga filenya menjadi seperti:
![image](https://user-images.githubusercontent.com/29938033/139524843-7aa63a04-b1d1-4347-b8b1-4272072c7204.png)
Kemudian restart server dengan command `service apache2 restart`

Tampilan website ketika dibuka di client Loguetown dengan `lynx 192.193.2.4`:
![image](https://user-images.githubusercontent.com/29938033/139524870-97a1e3df-127a-4e50-bfff-4d9488fee434.png)

## No 17
### Soal
Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png
### Penjelasan Jawaban
Buat file .htaccess di dalam `/var/www/super.franky.D03.com` kemudian isikan dengan:
```Bash
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !franky.png
RewriteRule ^public/images/.*franky.*$ /public/images/franky.png [NC,L]
```
Seperti di gambar berikut:
![image](https://user-images.githubusercontent.com/29938033/139525003-adfac9d2-e7c4-42f5-9f57-8525ad533341.png)
Jika belum mengaktifkan modul rewrite, aktifkan dengan command `a2enmod rewrite`

Buka file `/etc/apache2/sites-available/super.franky.D03.com.conf` dan tambahkan:
```Bash
<Directory /var/www/super.franky.D03.com>
    Options +FollowSymLinks -Multiviews
    AllowOverride All
</Directory>
```
sehingga isi filenya menjadi seperti berikut:
![image](https://user-images.githubusercontent.com/29938033/139525063-26f7dbf2-0b29-49a6-9f57-fe5d2ae55e65.png)
Terakhir, restart kembali webserver dengan command `service apache2 restart`

Mendownload gambar `franky.png` di client Loguetown dengan `lynx www.super.franky.D03.com/public/images/franky.png`:
![image](https://user-images.githubusercontent.com/29938033/139525154-3066a5d9-12bd-4647-b6d0-ac1267c0fcc4.png)
Mendownload gambar `not-franky.jpg` di client Loguetown dengan `lynx www.super.franky.D03.com/public/images/not-franky.jpg`:
![image](https://user-images.githubusercontent.com/29938033/139525192-32cfa574-6d7c-44a0-9604-eac4aca61205.png)
Membandingkan kedua gambar dengan command `diff franky.png not-franky.jpg && echo gambar sama`
![image](https://user-images.githubusercontent.com/29938033/139525282-239eea24-5162-490e-a134-6c4dc6eafa64.png)
Kedua gambar tersebut sama sehingga berarti dua url tadi sudah mengarah ke gambar yang sama, yaitu `franky.png`
