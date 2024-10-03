# Laporan Resmi Praktikum Modul 2 Jaringan Komputer

### GNS 3

### Kelompok IT43 -

| Name                              |     NRP    |
| ----------------------------------|------------|
| Rafael Jonathan Arnoldus          | 5027231006 | 
| Gandhi Ert Julio                  | 5027231081 |


---
---
---
Script 
- install.sh (Sriwijaya dan Majapahit)
```
apt-get update
apt-get install bind9 -y
```

- configm.sh (Sriwijaya)
```
echo "Konfigurasi Sudarsana..."
  cat <<EOL > /etc/bind/named.conf.local
zone "sudarsana.it43.com" {
    type master;
    notify yes;
    also-notify { 192.238.2.2; };
    allow-transfer { 192.238.2.2; };
    file "/etc/bind/it43/sudarsana.it43.com";
};

zone "pasopati.it43.com" {
    type master;
    notify yes;
    also-notify { 192.238.2.2; };
    allow-transfer { 192.238.2.2; };
    file "/etc/bind/it43/pasopati.it43.com";
};

zone "rujapala.it43.com" {
    type master;
    notify yes;
    also-notify { 192.238.2.2; };
    allow-transfer { 192.238.2.2; };
    file "/etc/bind/it43/rujapala.it43.com";
};

zone "1.238.192.in-addr.arpa" {
    type master;
    file "/etc/bind/it43/1.238.192.in-addr.arpa";
};
EOL

  echo "Restarting BIND service on Sudarsana..."
  service bind9 restart
```

- configs.sh (Majapahit)
```
echo "Konfigurasi Majapahit..."
  cat <<EOL > /etc/bind/named.conf.local
zone "sudarsana.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/sudarsana.it43.com";
};

zone "pasopati.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/pasopati.it43.com";
};

zone "rujapala.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/rujapala.it43.com";
};

zone "panah.pasopati.it43.com" {
    type master;
    file "/etc/bind/panah/panah.pasopati.it43.com";
};
EOL

  echo "Restarting BIND service on Majapahit..."
  service bind9 restart
```

- dnsconf.sh (sriwijaya)
```
#!/bin/bash

# Script untuk konfigurasi file zone pada BIND

# Buat direktori /etc/bind/it43 jika belum ada
mkdir -p /etc/bind/it43

# Konfigurasi sudarsana.it43.com
echo "Mengonfigurasi sudarsana.it43.com..."
cp /etc/bind/db.local /etc/bind/it43/sudarsana.it43.com
cat <<EOL > /etc/bind/it43/sudarsana.it43.com
;
; BIND data file for sudarsana.it43.com
;
\$TTL    604800
@       IN      SOA     sudarsana.it43.com. root.sudarsana.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it43.com.
@       IN      A       192.238.2.3
@       IN      AAAA    ::1
www     IN      CNAME   sudarsana.it43.com.
cakra   IN      A       192.238.1.5
EOL

# Konfigurasi pasopati.it43.com
echo "Mengonfigurasi pasopati.it43.com..."
cp /etc/bind/db.local /etc/bind/it43/pasopati.it43.com
cat <<EOL > /etc/bind/it43/pasopati.it43.com
;
; BIND data file for pasopati.it43.com
;
\$TTL    604800
@       IN      SOA     pasopati.it43.com. root.pasopati.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it43.com.
@       IN      A       192.238.1.6
@       IN      AAAA    ::1
www     IN      CNAME   pasopati.it43.com.
ns1     IN      A       192.238.2.2
panah   IN      NS      ns1
EOL

# Konfigurasi rujapala.it43.com
echo "Mengonfigurasi rujapala.it43.com..."
cp /etc/bind/db.local /etc/bind/it43/rujapala.it43.com
cat <<EOL > /etc/bind/it43/rujapala.it43.com
;
; BIND data file for rujapala.it43.com
;
\$TTL    604800
@       IN      SOA     rujapala.it43.com. root.rujapala.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it43.com.
@       IN      A       192.238.1.4
@       IN      AAAA    ::1
www     IN      CNAME   rujapala.it43.com.
EOL

# Konfigurasi 1.238.192.in-addr.arpa
echo "Mengonfigurasi 1.238.192.in-addr.arpa..."
cp /etc/bind/db.local /etc/bind/it43/1.238.192.in-addr.arpa
cat <<EOL > /etc/bind/it43/1.238.192.in-addr.arpa
;
; BIND data file for 1.238.192.in-addr.arpa
;
\$TTL    604800
@       IN      SOA     pasopati.it43.com. root.pasopati.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
1.238.192.in-addr.arpa.   IN  NS      pasopati.it43.com.
6                       IN  PTR     pasopati.it43.com.
EOL
service bind9 restart
echo "Konfigurasi selesai!"
```

- dnsconf (Majapahit)
```
#!/bin/bash

# Script untuk konfigurasi file zone pada BIND untuk panah.pasopati.it43.com

# Buat direktori /etc/bind/panah jika belum ada
mkdir -p /etc/bind/panah

# Konfigurasi panah.pasopati.it43.com
echo "Mengonfigurasi panah.pasopati.it43.com..."
cp /etc/bind/db.local /etc/bind/panah/panah.pasopati.it43.com
cat <<EOL > /etc/bind/panah/panah.pasopati.it43.com
;
; BIND data file for panah.pasopati.it43.com
;
\$TTL    604800
@       IN      SOA     panah.pasopati.it43.com. root.panah.pasopati.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it43.com.
@       IN      A       192.238.1.6
@       IN      AAAA    ::1
www     IN      CNAME   panah.pasopati.it43.com.
log     IN      A       192.238.1.6
www.log IN      CNAME   panah.pasopati.it43.com.
EOL

# Restart layanan BIND
echo "Restarting BIND service..."
service bind9 restart

echo "Konfigurasi selesai!"
```

- configop.sh (Sriwijaya dan Majapahit)
```
#!/bin/bash

# Script untuk konfigurasi file named.conf.options pada BIND

# Konfigurasi named.conf.options
echo "Mengonfigurasi named.conf.options..."
cat <<EOL > /etc/bind/named.conf.options
options {
    directory "/var/cache/bind";

    forwarders { 192.168.122.1; };

    //dnssec-validation auto;
    allow-query{any;};

    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};
EOL

# Restart layanan BIND
echo "Restarting BIND service..."
service bind9 restart

echo "Konfigurasi selesai!"
```

1. Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave. 

![image](https://github.com/user-attachments/assets/e1d90912-9ea0-4c01-a03e-5950c4c28fef)

Konfigurasi Masing-masing node 
```
Nusantara
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.238.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.238.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.238.3.1
	netmask 255.255.255.0

Majapahit 
auto eth0
iface eth0 inet static
	address 192.238.2.2
	netmask 255.255.255.0
	gateway 192.238.2.1
Solok
auto eth0
iface eth0 inet static
	address 192.238.2.3
	netmask 255.255.255.0
	gateway 192.238.2.1 
Sriwijaya
auto eth0
iface eth0 inet static
	address 192.238.1.2
	netmask 255.255.255.0
	gateway 192.238.1.1
HayamWuruk
auto eth0
iface eth0 inet static
	address 192.238.1.3
	netmask 255.255.255.0
	gateway 192.238.1.1
GajahMada
auto eth0
iface eth0 inet static
	address 192.238.3.2
	netmask 255.255.255.0
	gateway 192.238.3.1
ThomasAlfaEdison
auto eth0
iface eth0 inet static
	address 192.238.3.3
	netmask 255.255.255.0
	gateway 192.238.3.1
Tanjungkulai
auto eth0
iface eth0 inet static
	address 192.238.1.4
	netmask 255.255.255.0
	gateway 192.238.1.1
Bedahulu
auto eth0
iface eth0 inet static
	address 192.238.1.5
	netmask 255.255.255.0
	gateway 192.238.1.1
Kotalingga
auto eth0
iface eth0 inet static
	address 192.238.1.6
	netmask 255.255.255.0
	gateway 192.238.1.1
```
Lalu di Nusantara :

    1. nano /root/.bashrc
    2. iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.238.0.0/16
Di Node lain semuanya:

    1. nano /root/.bashrc
    2. echo nameserver 192.168.122.1 >> /etc/resolv.conf
    3. echo nameserver 192.238.1.2 >> /etc/resolv.conf
    4. echo nameserver 192.238.2.2 >> /etc/resolv.conf 

2. Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

Di Sriwijaya :

    1. apt-get update
    2. apt-get install bind9 -y
    3. nano /etc/bind/named.conf.local
```
zone "sudarsana.it43.com" {
    type master;
    file "/etc/bind/it43/sudarsana.it43.com";
};
```
Lalu :

    4. mkdir /etc/bind/it43
    5. cp /etc/bind/db.local /etc/bind/it43/sudarsana.it43.com
    6. nano /etc/bind/it43/sudarsana.it43.com
    
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it43.com. root.sudarsana.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it43.com.
@       IN      A       192.238.2.3
@       IN      AAAA    ::1
www     IN      CNAME   sudarsana.it43.com.
```

3. Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

Di Sriwijaya :

    1. apt-get update
    2. apt-get install bind9 -y
    3. nano /etc/bind/named.conf.local
```
zone "pasopati.it43.com" {
    type master;
    file "/etc/bind/it43/pasopati.it43.com";
};
```
Lalu :

    4. mkdir /etc/bind/it43
    5. cp /etc/bind/db.local /etc/bind/it43/pasopati.it43.com
    6. nano /etc/bind/it43/pasopati.it43.com
    
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it43.com. root.pasopati.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it43.com.
@       IN      A       192.238.1.6
@       IN      AAAA    ::1
www     IN      CNAME   pasopati.it43.com.
```
4. Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com. 

Di Sriwijaya :

    1. apt-get update
    2. apt-get install bind9 -y
    3. nano /etc/bind/named.conf.local
```
zone "rujapala.it43.com" {
    type master;
    file "/etc/bind/it43/rujapala.it43.com";
};
```
Lalu :

    4. mkdir /etc/bind/it43

    5. cp /etc/bind/db.local /etc/bind/it43/rujapala.it43.com

    6. nano /etc/bind/it43/rujapala.it43.com
    
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it43.com. root.rujapala.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it43.com.
@       IN      A       192.238.1.4
@       IN      AAAA    ::1
www     IN      CNAME   rujapala.it43.com.
```

5. Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.
Pastikan : nano /etc/resolv.conf 
```
    nameserver 192.238.1.2
```
---
1. ping rujapala.it43.com -c 3
2. ping sudarsana.it43.com -c 3
3. ping pasopati.it43.com -c 3
4. ping www.rujapala.it43.com -c 2
5. ping www.sudarsana.it43.com -c 2
6. ping www.pasopati.it43.com -c 2
---
Hasil Ping Pada HayamWuruk :

![image](https://github.com/user-attachments/assets/b64ffda2-8f7a-454c-8c7c-45f054ed343e)
![image](https://github.com/user-attachments/assets/a6ca8f81-3a07-40ea-8974-d7704254a37b)

6. Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

DI Sriwijaya: 

    1. nano /etc/bind/named.conf.local
```
zone "1.238.192.in-addr.arpa" {
    type master;
    file "/etc/bind/it43/1.238.192.in-addr.arpa";
};
```
    2. cp /etc/bind/db.local /etc/bind/it43/1.238.192.in-addr.arpa
    3. nano /etc/bind/it43/1.238.192.in-addr.arpa
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it43.com. root.pasopati.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
1.238.192.in-addr.arpa.   IN  NS          pasopati.it43.com.
6                       IN  PTR         pasopati.it43.com.
```
    4. service bind9 restart 
    5. Coba di client : 

![image](https://github.com/user-attachments/assets/d0c1fd9c-6493-4a4b-b5b8-8edfd283a344)


7. Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya.

Di Sriwijaya :

    1. nano /etc/bind/named.conf.local
```
zone "sudarsana.it43.com" {
    type master;
    notify yes;
    also-notify { 192.238.2.2; };
    allow-transfer { 192.238.2.2; };
    file "/etc/bind/it43/sudarsana.it43.com";
};

zone "pasopati.it43.com" {
    type master;
    notify yes;
    also-notify { 192.238.2.2; };
    allow-transfer { 192.238.2.2; };
    file "/etc/bind/it43/pasopati.it43.com";
};

zone "rujapala.it43.com" {
    type master;
    notify yes;
    also-notify { 192.238.2.2; };
    allow-transfer { 192.238.2.2; };
    file "/etc/bind/it43/rujapala.it43.com";
};

zone "1.238.192.in-addr.arpa" {
    type master;
    file "/etc/bind/it43/1.238.192.in-addr.arpa";
};
```
    2. service bind9 restart 

Di Majapahit 

    1. nano /etc/bind/named.conf.local 
```
zone "sudarsana.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/sudarsana.it43.com";
};

zone "pasopati.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/pasopati.it43.com";
};

zone "rujapala.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/rujapala.it43.com";
};
```
    2. service bind9 restart

Client Server :

    1. nano etc/resolv.conf
```
nameserver 192.238.1.2
nameserver 192.238.2.2 
```
Coba mematikan bind9 di sriwijaya

![image](https://github.com/user-attachments/assets/b800c1e3-f075-44fe-a0c0-d63909aad5d3)

Mencoba ping di client setelah bind9 sriwijaya mati

![image](https://github.com/user-attachments/assets/53fb22d5-76d1-4a7f-9ea9-53a0ae3c8ccc)

8. Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

Di Sriwijaya :

    1. nano /etc/bind/it43/sudarsana.it43.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it43.com. root.sudarsana.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it43.com.
@       IN      A       192.238.2.3
@       IN      AAAA    ::1
www     IN      CNAME   sudarsana.it43.com.
cakra   IN      A       192.238.1.5
```
    2. service bind9 restart (di majapahit juga) 

Di client:

    1. ping cakra.sudarsana.it43.com

![image](https://github.com/user-attachments/assets/722733d6-33cc-40c7-8a86-fea82d28a4bc)

9. Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Majapahit dengan alamat IP menuju radar di Kotalingga.

Di Sriwijaya: 

    1. nano /etc/bind/it43/pasopati.it43.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it43.com. root.pasopati.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it43.com.
@       IN      A       192.238.1.6
@       IN      AAAA    ::1
www     IN      CNAME   pasopati.it43.com.
ns1     IN      A       192.238.2.2
panah   IN      NS      ns1
```
    2. nano /etc/bind/named.conf.options
```
options {
    directory "/var/cache/bind";
    //dnssec-validation auto;
    allow-query{any;};

    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};
```
    3. service bind9 restart 
Di Majapahit :

    1. nano /etc/bind/named.conf.options
```
options {
    directory "/var/cache/bind";
    //dnssec-validation auto;
    allow-query{any;};

    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};
```
    2. nano /etc/bind/named.conf.local
```
zone "sudarsana.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/sudarsana.it43.com";
};

zone "pasopati.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/pasopati.it43.com";
};

zone "rujapala.it43.com" {
    type slave;
    masters { 192.238.1.2; };
    file "/var/lib/bind/rujapala.it43.com";
};

zone "panah.pasopati.it43.com" {
    type master;
    file "/etc/bind/panah/panah.pasopati.it43.com";
};
```
    3. mkdir /etc/bind/panah
    4. cp /etc/bind/db.local /etc/bind/panah/panah.pasopati.it43.com
    5. nano /etc/bind/panah/panah.pasopati.it43.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it43.com. root.panah.pasopati.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it43.com.
@       IN      A       192.238.1.6
@       IN      AAAA    ::1
www     IN      CNAME   panah.pasopati.it43.com.
```
    6. service bind9 restart 

Di client server :

    1. ping panah.pasopati.it43.com

![image](https://github.com/user-attachments/assets/04ce72d1-6c51-47e6-9a36-4ac49cf21806)

10. Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.

Di Majapahit :

    1. nano /etc/bind/panah/panah.pasopati.it43.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it43.com. root.panah.pasopati.it43.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it43.com.
@       IN      A       192.238.1.6
@       IN      AAAA    ::1
www     IN      CNAME   panah.pasopati.it43.com.
log     IN      A       192.238.1.6
www.log IN      CNAME   panah.pasopati.it43.com.
```
    2. service bind restart 

Di client Server :

    1. ping log.panah.pasopati.it43.com
![image](https://github.com/user-attachments/assets/8175cd92-8708-414e-ad05-fbda09f2efd2)

    2. ping www.log.panah.pasopati.it43.com
![image](https://github.com/user-attachments/assets/1a622523-e684-4e8a-ba1a-6d06873fb1a9)

11. Setelah pertempuran mereda, warga IT dapat kembali mengakses jaringan luar dan menikmati meme brainrot terbaru, tetapi hanya warga Majapahit saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga IT yang berada diluar Majapahit dapat mengakses jaringan luar melalui DNS Server Majapahit.

Di Majapahit/Sriwijaya :

    1. nano /etc/bind/named.conf.options
```
options {
    directory "/var/cache/bind";

    forwarders { 192.168.122.1; };

    //dnssec-validation auto;
    allow-query{any;};

    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};
```
    2. service bind9 restart 
DI Client :

    1. ping google.com 

![image](https://github.com/user-attachments/assets/cf160245-5bf7-434b-ab4c-28aed42aa6f9)

12. Karena pusat ingin sebuah laman web yang ingin digunakan untuk memantau kondisi kota lainnya maka deploy laman web ini (cek resource yg lb) pada Kotalingga menggunakan apache.

Di kotalingga :

    1. buat install.sh (Kotalingga)
```
apt-get update
apt-get install lynx apache2 php libapache2-mod-php7.0 nginx -y
apt-get install apache2 -y
apt-get install libapache2-mod-php7.0 -y
apt-get install php -y
apt install wget
apt install zip
```
    2. cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/pasopati.it43.com.conf
    3. nano /etc/apache2/sites-available/pasopati.it43.com.conf
```
<VirtualHost *:80> 

ServerAdmin webmaster@localhost
DocumentRoot /var/www/pasopati.it43.com #ubah document rootnya
ServerName pasopati.it43.com
ServerAlias www.pasopati.it43.com

```
    4. nano /etc/apache2/ports.conf
```
listen 80
```
    5. mkdir /var/www/pasopati.it43.com
    6. service apache2 reload && service apache2 start
    7. a2ensite pasopati.it43.com.conf
    8. wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7' -O lb.zip
    9. unzip lb.zip  -d  lb
    10. mv lb/* /var/www/pasopati.it43.com
    11. cp /var/www/pasopati.it43.com/worker/index.php /var/www/pasopati.it43.com/index.php
    12. cp /var/www/pasopati.it43.com/index.php /var/www/html/index.php
    13. rm /var/www/html/index.html
    14. service apache2 restart

confapa.sh
```
#!/bin/bash

cat <<EOL > /etc/apache2/ports.conf
listen 80
EOL

mkdir -p /var/www/pasopati.it43.com

service apache2 reload && service apache2 start

a2ensite pasopati.it43.com.conf

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7' -O lb.zip

unzip lb.zip -d lb

mv lb/* /var/www/pasopati.it43.com

cp /var/www/pasopati.it43.com/worker/index.php /var/www/pasopati.it43.com/index.php

cp /var/www/pasopati.it43.com/index.php /var/www/html/index.php

rm /var/www/html/index.html

service apache2 restart

echo "Konfigurasi selesai!"

```
Di client :

    1. apt install lynx
    2. lynx pasopati.it43.com atau lynx 192.238.1.6 

![image](https://github.com/user-attachments/assets/57445bb3-c09c-4191-9edd-4e8f97fab5a6)

13. Karena Sriwijaya dan Majapahit memenangkan pertempuran ini dan memiliki banyak uang dari hasil penjarahan (sebanyak 35 juta, belum dipotong pajak) maka pusat meminta kita memasang load balancer untuk membagikan uangnya pada web nya, dengan Kotalingga, Bedahulu, Tanjungkulai sebagai worker dan Solok sebagai Load Balancer menggunakan apache sebagai web server nya dan load balancer nya.

Di Solok :
    
    1. nano install.sh 
```
apt-get update && apt-get install apache2 -y

a2enmod proxy
a2enmod proxy_balancer
a2enmod proxy_http
a2enmod lbmethod_byrequests
```
    2. nano /etc/apache2/sites-available/000-default.conf
```
<VirtualHost *:80>
    <Proxy balancer://mycluster>
        BalancerMember http://192.238.1.4
        BalancerMember http://192.238.1.5
        BalancerMember http://192.238.1.6
        ProxySet lbmethod=byrequests
    </Proxy>

    ProxyPass / balancer://mycluster/
    ProxyPassReverse / balancer://mycluster/

</VirtualHost>
```
Di Webserver :

Bendahulu: 

    1. cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/sudarsana.it43.com.conf
    2. nano /etc/apache2/sites-available/sudarsana.it43.com.conf
```
<VirtualHost *:80> 

ServerAdmin webmaster@localhost
DocumentRoot /var/www/sudarsana.it43.com #ubah document rootnya
ServerName sudarsana.it43.com
ServerAlias www.sudarsana.it43.com

```
    3. Buat dan jalankan configapa.sh dengan mengganti domainnya menjadi sudarsana

Tanjungkulai :

    1. cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/rujapala.it43.com.conf
    2. nano /etc/apache2/sites-available/rujapala.it43.com.conf
```
<VirtualHost *:80> 

ServerAdmin webmaster@localhost
DocumentRoot /var/www/rujapala.it43.com #ubah document rootnya
ServerName rujapala.it43.com
ServerAlias www.rujapala.it43.com
```
    3. Buat dan jalankan configapa.sh dengan mengganti domainnya menjadi rujapala

Di Client :

    1. Test Bendahulu

![image](https://github.com/user-attachments/assets/9452927f-7160-433d-876e-50ebf42a4fdf)

    2. Test Tanjungkulai

![image](https://github.com/user-attachments/assets/befccfb5-5229-4cf3-be29-df60eedcec62)


14. Selama melakukan penjarahan mereka melihat bagaimana web server luar negeri, hal ini membuat mereka iri, dengki, sirik dan ingin flexing sehingga meminta agar web server dan load balancer nya diubah menjadi nginx.


Di Webserver :

    1. apt-get install nginx php-fpm -y

Di Kotalingga :

    1. nano /etc/nginx/sites-available/pasopati.it43.com
```
server {
    listen 80;

    root /var/www/pasopati.it43.com;

    index index.php index.html index.htm;
    server_name _ pasopati.it43.com www.pasopati.it43.com;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
    2. ln -s /etc/nginx/sites-available/pasopati.it43.com /etc/nginx/sites-enabled/pasopati.it43.com
    3. rm /etc/nginx/sites-enabled/default
    4. service nginx restart
    5. service php7.0-fpm start

Di Bedahulu :
    1. nano /etc/nginx/sites-available/sudarsana.it43.com
```
server {
    listen 80;

    root /var/www/sudarsana.it43.com;

    index index.php index.html index.htm;
    server_name _ sudarsana.it43.com www.sudarsana.it43.com;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
    2. ln -s /etc/nginx/sites-available/sudarsana.it43.com /etc/nginx/sites-enabled/sudarsana.it43.com
    3. rm /etc/nginx/sites-enabled/default
    4. service nginx restart
    5. service php7.0-fpm start

Di Tanjungkulai :
    1. nano /etc/nginx/sites-available/rujapala.it43.com
```
server {
    listen 80;

    root /var/www/rujapala.it43.com;

    index index.php index.html index.htm;
    server_name _ rujapala.it43.com www.rujapala.it43.com;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
    2. ln -s /etc/nginx/sites-available/rujapala.it43.com /etc/nginx/sites-enabled/rujapala.it43.com
    3. rm /etc/nginx/sites-enabled/default
    4. service nginx restart
    5. service php7.0-fpm start

Di Solok :
    1. apt-get install nginx -y
    2. nano /etc/nginx/sites-available/solok
```
upstream webserver  {
    server 192.238.1.4;
    server 192.238.1.5;
    server 192.238.1.6;
}

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://webserver;
    }
}
```
    3. ln -s /etc/nginx/sites-available/solok /etc/nginx/sites-enabled/solok
    4. rm /etc/nginx/sites-enabled/default
    5. service nginx restart

DI Client :

    1. Test lynx 192.238.2.3 (Solok)

![image](https://github.com/user-attachments/assets/9b47d20f-ca0c-4c14-92ff-d518b427140a)

![image](https://github.com/user-attachments/assets/9b9f0a7f-7c8b-43e7-8f07-ef2b8fef8f06)

![image](https://github.com/user-attachments/assets/988fc807-b8ea-4cc3-934e-2370fb5aed82)
