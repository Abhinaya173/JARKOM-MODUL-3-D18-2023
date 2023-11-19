# JARKOM-MODUL-3-D18-2023

#### Topologi
<img width="1439" alt="image" src="https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114478450/2cf863f4-c227-4dcf-90bc-647cdee791b6">


#### No 1 

Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

Heiter

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

Testing di Freiren:

echo nameserver 10.18.1.3 > /etc/resolv.conf
ping riegel.canyon.d18.com

<img width="712" alt="image" src="https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114478450/41102d97-8b8c-4542-83c0-121205631c5c">


Testing di Lawine:

echo nameserver 10.18.1.3 > /etc/resolv.conf
ping granz.channel.d18.com

<img width="736" alt="image" src="https://github.com/Abhinaya173/JARKOM-MODUL-3-D18-2023/assets/114478450/1f418e63-1679-480b-8fb6-8faeaff7e28f">

#### No 2
#### No 3
#### No 4
#### No 5


