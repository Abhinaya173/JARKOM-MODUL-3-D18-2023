# JARKOM-MODUL-3-D18-2023
Abhinaya Radiansyah Listiyanto (5025211173)
Fauzi Rizki Pratama (5025211220)

#### Topologi
<img width="1439" alt="image" src="https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114478450/2cf863f4-c227-4dcf-90bc-647cdee791b6">


#### No 1 

> Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

Heiter

````
nano /etc/bind/named.conf.local

zone "riegel.canyon.d18.com" {
	type master;
	file "/etc/bind/jarkom/riegel.canyon.d18.com";
};

zone "granz.channel.d18.com" {
	type master;
	file "/etc/bind/jarkom/granz.channel.d18.com";
};

mkdir /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.d18.com
cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.d18.com

nano /etc/bind/jarkom/riegel.canyon.d18.com
ganti nama
ganti ip (192.200.4.4) // IP Frieren

nano /etc/bind/jarkom/granz.channel.d18.com
ganti nama
ganti ip (192.200.3.4) // IP Lawine


service bind9 restart
````
Testing di Freiren:
````
echo nameserver 10.18.1.3 > /etc/resolv.conf
ping riegel.canyon.d18.com
````
<img width="712" alt="image" src="https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114478450/41102d97-8b8c-4542-83c0-121205631c5c">


Testing di Lawine:
````
echo nameserver 10.18.1.3 > /etc/resolv.conf
ping granz.channel.d18.com
````
<img width="736" alt="image" src="https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114478450/1f418e63-1679-480b-8fb6-8faeaff7e28f">

#### No 2
> Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)

Di Himmel

````
apt-get update
apt-get install isc-dhcp-server -y

nano /etc/default/isc-dhcp-server
masukan “INTERFACES=”eth0”

nano /etc/dhcp/dhcpd.conf

masukkan
subnet 192.200.1.0 netmask 255.255.255.0 {}
subnet 192.200.2.0 netmask 255.255.255.0 {}

subnet 192.200.3.0 netmask 255.255.255.0 {
    range 192.200.3.16 192.200.3.32;
    range 192.200.3.64 192.200.3.80;
    option routers 192.200.3.1;
    option broadcast-address 192.200.3.255;
    option domain-name-servers 192.200.1.3;
    default-lease-time 180;
    max-lease-time 5760;
````

#### No 3
> Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)

````
nano /etc/dhcp/dhcpd.conf

subnet 192.200.4.0 netmask 255.255.255.0 {
    range 192.200.4.12 10.17.4.20;
    range 192.200.4.160 10.17.4.168;
    option routers 192.200.4.1;
    option broadcast-address 192.200.4.255;
    option domain-name-servers 192.200.1.3;
    default-lease-time 720;
    max-lease-time 5760;
}

service isc-dhcp-server restart
service isc-dhcp-server status
````

#### No 4

> Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)


Pada dasarnya, saat mengatur subner, seluruh Client sudah diatur untuk terhubung ke Heiter
Di Himmel
````
subnet 192.200.3.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 192.200.3.255;
    option domain-name-servers 192.200.1.3;
    ...
}

subnet 192.200.4.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 192.200.4.255;
    option domain-name-servers 192.200.1.3;
    ...
}

// aura 
apt-get update
apt-get install isc-dhcp-relay -y 
service isc-dhcp-relay start

nano /etc/default/isc-dhcp-relay
SERVERS=”192.200.1.2”

nano /etc/sysctl.conf
net.ipv4.ip_forward=1

service isc-dhcp-relay restart

//heiter

echo nameserver 192.200.122.1 >> /etc/resolv.conf
apt-get update
apt-get install bind9 -y

nano /etc/bind/named.conf.options
comment // dnssec-validation auto;

tambahkan
forwarders {
    192.200.122.1;
};
allow-query{any;};

service bind9 restart
````

#### No 5

> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)
Pada dasarnya, ini sudah dilakukan penyetingan saat mengerjakan nomor 2

````
subnet 192.200.3.0 netmask 255.255.255.0 {
    ...
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.200.4.0 netmask 255.255.255.0 {
    ...
    default-lease-time 720;
    max-lease-time 5760;
}

````

#### NO 6) Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.
install apt-get update dan apt-get install php
Jangan Lupa untuk install apache juga
Kemudian lakukan konfigurasi pada virtual hostnya

![Screenshot 2023-11-16 210534](https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114990549/425816f2-0fc8-40c4-bc39-52bb8e20a806)

Kemudian buat direktori mkdir /var/www/website dan chown -R www-data:www-data /var/www/website

![Screenshot 2023-11-16 210853](https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114990549/a64ca886-92de-4f30-ad73-2064f9625068)

Wget digunakan untuk mendownload web yang diinginkan dan jangan lupa untuk diunzip unzip website.zip -d /var/www/website

![Screenshot 2023-11-16 211052](https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114990549/d582415f-0f46-4a65-9fbe-4cd9d47d779e)

Contoh jika berhasil menjalankannya dengan lynx.

#### NO 7) Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
Lawine, 4GB, 2vCPU, dan 80 GB SSD.
Linie, 2GB, 2vCPU, dan 50 GB SSD.
Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

Lakukan penginstalan seperti biasa apt-get update dan lain sebagainya
Buka nano /etc/apache2/sites-available/website.conf

![Screenshot 2023-11-16 212300](https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114990549/e369c817-69a8-4646-90f1-352be19e528e)

Lakukan apt-get install apache2-utils
Dan lakukan pengujian ab -n 1000 -c 100 https://frieren.fandom.com/wiki/Bredt_Region

![Screenshot 2023-11-16 212535](https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114990549/cb8c5119-c232-41de-b2e3-ade33d763f61)

#### NO 8) Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
Nama Algoritma Load Balancer
Report hasil testing pada Apache Benchmark
Grafik request per second untuk masing masing algoritma. 
Analisis

Install haproxy untuk LB
Buka isian nano /etc/haproxy/haproxy.cfg

![Screenshot 2023-11-16 213000](https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114990549/fd4a2853-d0e7-488d-a560-25d795b3e4e3)

Jangan lupa untuk di restart service haproxy restart

Testing :
![Screenshot 2023-11-16 213234](https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114990549/b53720b0-9cba-42a8-bf76-6d3a28358091)

#### NO 9) Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. 

Pada nomor 9 sama seperti no 8 tetapi ditambahkan grafiknya, tapi kelompok kami belum berhasil.

#### NO 10) Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

Install Apache2-utils untuk Membuat File htpasswd: 
apt-get update
apt-get install apache2-utils
apt-get install nginx

Buat File htpasswd dengan Username dan Password yang Diberikan:
mkdir /etc/nginx/rahasisakita/
htpasswd -c /etc/nginx/rahasisakita/htpasswd netics

Konfigurasi Autentikasi di Nginx (Server Yang Diakses Melalui HAProxy):
nano /etc/nginx/sites-available/your-server-config

