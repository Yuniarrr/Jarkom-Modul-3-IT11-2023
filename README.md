# Jarkom-Modul-3-IT11-2023

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Dyas Amorita Radhwa Nashirah (5027211009)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Midyanisa Yuniar (5027211025)

&nbsp;&nbsp;0. [Soal 0](#soal-0)

&nbsp;&nbsp;1. [Soal 1](#soal-1)

&nbsp;&nbsp;2. [Soal 2](#soal-2)

&nbsp;&nbsp;3. [Soal 3](#soal-3)

&nbsp;&nbsp;4. [Soal 4](#soal-4)

&nbsp;&nbsp;5. [Soal 5](#soal-5)

&nbsp;&nbsp;6. [Soal 6](#soal-6)

&nbsp;&nbsp;7. [Soal 7](#soal-7)

&nbsp;&nbsp;8. [Soal 8](#soal-8)

&nbsp;&nbsp;9. [Soal 9](#soal-9)

&nbsp;&nbsp;10. [Soal 10](#soal-10)

&nbsp;&nbsp;11. [Soal 11](#soal-11)

&nbsp;&nbsp;12. [Soal 12](#soal-12)

&nbsp;&nbsp;13. [Soal 13](#soal-13)

&nbsp;&nbsp;14. [Soal 14](#soal-14)

&nbsp;&nbsp;15. [Soal 15](#soal-15)

&nbsp;&nbsp;16. [Soal 16](#soal-16)

&nbsp;&nbsp;16. [Soal 16](#soal-16)

&nbsp;&nbsp;17. [Soal 17](#soal-17)

&nbsp;&nbsp;18. [Soal 18](#soal-18)

&nbsp;&nbsp;19. [Soal 19](#soal-19)

&nbsp;&nbsp;20. [Soal 20](#soal-20)

## Soal 0

Melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.

script untuk setup heiter

```sh
#!/bin/bash

apt update
apt install bind9 dnsutils -y

# Konfigurasi yang akan dimasukkan ke dalam file
echo "zone \"riegel.canyon.it11.com\" {
 	type master;
 	file \"/etc/bind/zones/riegel.canyon.it11.com\";
};

zone \"granz.channel.it11.com\" {
	type master;
	file \"/etc/bind/zones/granz.channel.it11.com\";
};" >/etc/bind/named.conf.local

zones_folder="/etc/bind/zones"

# Mengecek apakah folder sudah ada
if [ ! -d "$zones_folder" ]; then
  # Jika folder tidak ada, maka membuatnya
  mkdir -p "$zones_folder"
  echo "Folder $zones_folder telah dibuat."
fi

# Konfigurasi yang akan dimasukkan ke dalam file
echo ";
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     riegel.canyon.it11.com. root.riegel.canyon.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@           IN      NS      riegel.canyon.it11.com.
@           IN      A       10.69.4.2 ; ip frieren laravel worker
www         IN      CNAME   riegel.canyon.it11.com.
" >/etc/bind/zones/riegel.canyon.it11.com

# Backup existing file
cp /etc/bind/named.conf.options /etc/bind/named.conf.options/bak

# Configuration data
echo 'options {
    listen-on-v6 { none; };
    directory "/var/cache/bind";

    # Forwarders
    forwarders {
        192.168.122.1;
    };

    # If there is no answer from the forwarders, dont attempt to resolve recursively
    forward only;

    dnssec-validation no;

    auth-nxdomain no;    # conform to RFC1035
    allow-query { any; };
};' >/etc/bind/named.conf.options

# Konfigurasi yang akan dimasukkan ke dalam file
echo ";
; BIND data file for local loopback interface
;
\$TTL    604800
@       IN      SOA     granz.channel.it11.com. root.granz.channel.it11.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@           IN      NS      granz.channel.it11.com.
@           IN      A       10.69.3.1 ; ip lawine larafel worker
www         IN      CNAME   granz.channel.it11.com.
" >/etc/bind/zones/granz.channel.it11.com

service bind9 restart
service bind9 status
```

## Soal 1

Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

- Aura (DHCP Relay)

```sh
# config for eth0
auto eth0
iface eth0 inet dhcp

# config for eth1
auto eth1
iface eth1 inet static
	address 10.69.1.254
	netmask 255.255.255.0

# config for eth2
auto eth2
iface eth2 inet static
	address 10.69.2.254
	netmask 255.255.255.0

# config for eth3
auto eth3
iface eth3 inet static
	address 10.69.3.254
	netmask 255.255.255.0

# config for eth4
auto eth4
iface eth4 inet static
	address 10.69.4.254
	netmask 255.255.255.0
```

- Himmel (DHCP Server)

```sh
auto eth0
iface eth0 inet static
	address 10.69.1.2
	netmask 255.255.255.0
	gateway 10.69.1.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Heiter (DNS Server)

```sh
auto eth0
iface eth0 inet static
	address 10.69.1.3
	netmask 255.255.255.0
	gateway 10.69.1.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Denken (Database Server)

```sh
auto eth0
iface eth0 inet static
	address 10.69.2.2
	netmask 255.255.255.0
	gateway 10.69.2.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Eisen (Load Balancer)

```sh
auto eth0
iface eth0 inet static
	address 10.69.2.3
	netmask 255.255.255.0
	gateway 10.69.2.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Frieren (Laravel Worker)

```sh
auto eth0
iface eth0 inet static
	address 10.69.4.2
	netmask 255.255.255.0
	gateway 10.69.4.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Flamme (Laravel Worker)

```sh
auto eth0
iface eth0 inet static
	address 10.69.4.3
	netmask 255.255.255.0
	gateway 10.69.4.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Fern (Laravel Worker)

```sh
auto eth0
iface eth0 inet static
	address 10.69.4.4
	netmask 255.255.255.0
	gateway 10.69.4.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Lawine (PHP Worker)

```sh
auto eth0
iface eth0 inet static
	address 10.69.3.1
	netmask 255.255.255.0
	gateway 10.69.3.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Linie (PHP Worker)

```sh
auto eth0
iface eth0 inet static
	address 10.69.3.3
	netmask 255.255.255.0
	gateway 10.69.3.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Lugner (PHP Worker)

```sh
auto eth0
iface eth0 inet static
	address 10.69.3.2
	netmask 255.255.255.0
	gateway 10.69.3.254
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Revolte (Client)

```sh
auto eth0
iface eth0 inet dhcp
```

- Richter (Client)

```sh
auto eth0
iface eth0 inet dhcp
```

- Sein (Client)

```sh
auto eth0
iface eth0 inet dhcp
```

- Stark (Client)

```sh
auto eth0
iface eth0 inet dhcp
```

![soal 1](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/88996914/646bde83-5158-4969-858f-26c8bbb93819)

## Soal 2

Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

Setup Aura sebagai DHCP Relay

```sh
#!/bin/bash

apt update

apt-get install isc-dhcp-relay -y

# Konfigurasi yang akan dimasukkan ke dalam file
echo "# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS=\"10.69.1.2\"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES=\"eth1 eth2 eth3 eth4\"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=\"\"" >/etc/default/isc-dhcp-relay

sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/g' /etc/sysctl.conf

iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.69.0.0/16

service isc-dhcp-relay restart
service isc-dhcp-relay status
```

Setup Himmel sebagai DHCP Server

```sh
#!/bin/bash

apt update
apt install isc-dhcp-server -y

# Konfigurasi yang akan dimasukkan ke dalam file
echo "# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)
# Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
#DHCPDv4_CONF=/etc/dhcp/dhcpd.conf
#DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf
# Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
#DHCPDv4_PID=/var/run/dhcpd.pid
#DHCPDv6_PID=/var/run/dhcpd6.pid
# Additional options to start dhcpd with.
#       Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=\"\"
# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#       Separate multiple interfaces with spaces, e.g. \"eth0 eth1\".
INTERFACESv4=\"eth0\"
INTERFACESv6=\"\"" >/etc/default/isc-dhcp-server

echo "subnet 10.69.1.0 netmask 255.255.255.0 {
    option routers 10.69.1.254;
    option broadcast-address 10.69.1.255;
}

subnet 10.69.2.0 netmask 255.255.255.0 {
    option routers 10.69.2.254;
    option broadcast-address 10.69.2.255;
}

subnet 10.69.3.0 netmask 255.255.255.0 {
    range 10.69.3.16 10.69.3.32;
    range 10.69.3.64 10.69.3.80;
    option routers 10.69.3.254;
    option broadcast-address 10.69.3.255;
    option domain-name-servers 192.168.122.1;
}" >/etc/dhcp/dhcpd.conf

rm /var/run/dhcpd.pid

service isc-dhcp-server restart

service isc-dhcp-server status
```

## Soal 3

Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

Tambahkan pengaturan pada Himmel sebagai DHCP Server

```sh
subnet 10.69.4.0 netmask 255.255.255.0 {
    range 10.69.4.12 10.69.4.20;
    range 10.69.4.160 10.69.4.168;
    option routers 10.69.4.254;
    option broadcast-address 10.69.4.255;
    option domain-name-servers 192.168.122.1;
}
```

## Soal 4

Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut.

Ubah pengaturan domain name servers pada Himmel sebagai DHCP Server

```sh
subnet 10.69.3.0 netmask 255.255.255.0 {
    range 10.69.3.16 10.69.3.32;
    range 10.69.3.64 10.69.3.80;
    option routers 10.69.3.254;
    option broadcast-address 10.69.3.255;
    option domain-name-servers 10.69.1.3;
}

subnet 10.69.4.0 netmask 255.255.255.0 {
    range 10.69.4.12 10.69.4.20;
    range 10.69.4.160 10.69.4.168;
    option routers 10.69.4.254;
    option broadcast-address 10.69.4.255;
    option domain-name-servers 10.69.1.3;
}
```

## Soal 5

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit 

Ubah pengaturan domain name servers pada Himmel sebagai DHCP Server

```sh
subnet 10.69.3.0 netmask 255.255.255.0 {
    range 10.69.3.16 10.69.3.32;
    range 10.69.3.64 10.69.3.80;
    option routers 10.69.3.254;
    option broadcast-address 10.69.3.255;
    option domain-name-servers 10.69.1.3;
    default-lease-time 180;
    max-lease-time 5760; # 96 minutes
}

subnet 10.69.4.0 netmask 255.255.255.0 {
    range 10.69.4.12 10.69.4.20;
    range 10.69.4.160 10.69.4.168;
    option routers 10.69.4.254;
    option broadcast-address 10.69.4.255;
    option domain-name-servers 10.69.1.3;
    default-lease-time 720; # 12 minutes
    max-lease-time 5760; # 96 minutes
}
```

## Soal 6

Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website [berikut](https://drive.google.com/file/d/1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1/view?usp=sharing) dengan menggunakan php 7.3. 

Masukkan script berikut pada tiap PHP Worker, yaitu Lawine, Linie, dan Lugner

```sh
#!/bin/bash

apt update
apt install nginx php-fpm php7.3 apache2 unzip lynx -y

mkdir /var/www/jarkom

curl -L --insecure "https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1" -o granz.zip

unzip granz.zip -d /var/www
rm granz.zip

mv /var/www/modul-3/* /var/www/jarkom/
rm -rf /var/www/modul-3

echo 'server {
    listen 80;
    root /var/www/jarkom;
    index index.php index.html index.htm;
    server_name _;
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
    }
    error_log /var/log/nginx/jarkom_error.log;
    access_log /var/log/nginx/jarkom_access.log;
}' >/etc/nginx/sites-available/jarkom.conf

ln -s /etc/nginx/sites-available/jarkom.conf /etc/nginx/sites-enabled/

rm /etc/nginx/sites-enabled/default

service nginx restart
service nginx status

service php7.3-fpm start
service php7.3-fpm status
```

## Soal 7

Kepala suku dari Bredt Region memberikan resource server sebagai berikut:

- Lawine, 4GB, 2vCPU, dan 80 GB SSD.
- Linie, 2GB, 2vCPU, dan 50 GB SSD
- Lugner 1GB, 1vCPU, dan 25 GB SSD.
    
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

Masukkan script berikut pada Eisen sebagai Load Balancer

```sh
#!/bin/bash

apt update
apt install nginx php php-fpm lynx apache2-utils -y

echo 'upstream weight_round_robin  {
    server 10.69.3.1 weight=4; #IP Lawine
    server 10.69.3.3 weight=2; #IP linie
    server 10.69.3.2 weight=1; #IP Lugner
}

server {
    listen 86;
        location /its {
            rewrite ^/its(.*)$ https://www.its.ac.id$1 permanent;
        }

        location / {
            proxy_pass http://weight_round_robin;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/weight-round-robin

unlink /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/weight-round-robin /etc/nginx/sites-enabled/weight-round-robin

service nginx restart
```

## Soal 8

Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
- Nama Algoritma Load Balancer
- Report hasil testing pada Apache Benchmark
- Grafik request per second untuk masing masing algoritma. 
- Analisis

Tambahkan script berikut pada Eisen sebagai Load Balancer

```sh
echo 'upstream round_robin  {
    server 10.69.3.1; #IP Lawine
    server 10.69.3.3; #IP linie
    server 10.69.3.2; #IP Lugner
}

server {
    listen 81;

        location /its {
            rewrite ^/its(.*)$ https://www.its.ac.id$1 permanent;
        }

        location / {
            proxy_pass http://round_robin;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/round-robin

echo 'upstream generic_hash  {
    hash $request_uri consistent;
    server 10.69.3.1; #IP Lawine
    server 10.69.3.3; #IP linie
    server 10.69.3.2; #IP Lugner
}

server {
    listen 83;
        location /its {
            rewrite ^/its(.*)$ https://www.its.ac.id$1 permanent;
        }

        location / {
            proxy_pass http://generic_hash;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/generic-hash

echo 'upstream ip_hash  {
    ip_hash;
    server 10.69.3.1; #IP Lawine
    server 10.69.3.3; #IP linie
    server 10.69.3.2; #IP Lugner
}

server {
    listen 84;
        location /its {
            rewrite ^/its(.*)$ https://www.its.ac.id$1 permanent;
        }

        location / {
            proxy_pass http://ip_hash;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/ip-hash

echo 'upstream least_conn  {
    least_conn;
    server 10.69.3.1; #IP Lawine
    server 10.69.3.3; #IP linie
    server 10.69.3.2; #IP Lugner
}

server {
    listen 85;
        location /its {
            rewrite ^/its(.*)$ https://www.its.ac.id$1 permanent;
        }

        location / {
            proxy_pass http://least_conn;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/least-conn

unlink /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/round-robin /etc/nginx/sites-enabled/round-robin
ln -s /etc/nginx/sites-available/generic-hash /etc/nginx/sites-enabled/generic-hash
ln -s /etc/nginx/sites-available/ip-hash /etc/nginx/sites-enabled/ip-hash
ln -s /etc/nginx/sites-available/least-conn /etc/nginx/sites-enabled/least-conn

service nginx restart
```

## Soal 9

Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

## Soal 10

Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

Tambahkan script berikut pada Eisen sebagai Load Balancer

```sh
mkdir /etc/nginx/rahasisakita

htpasswd -bc /etc/nginx/rahasisakita/htpasswd netics ajkit11

echo 'upstream round_robin_auth  {
    server 10.69.3.1; #IP Lawine
    server 10.69.3.3; #IP linie
    server 10.69.3.2; #IP Lugner
}

server {
    listen 82;
        location / {
            auth_basic "Restricted Content";
            auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
            proxy_pass http://round_robin_auth;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;
        }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/round-robin-auth

ln -s /etc/nginx/sites-available/round-robin-auth /etc/nginx/sites-enabled/round-robin-auth

service nginx restart
```

## Soal 11

Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id.

Tambahkan script berikut pada Eisen sebagai Load Balancer

```sh
location /its {
    rewrite ^/its(.*)$ https://www.its.ac.id$1 permanent;
}
```

## Soal 12

Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.

Tambahkan script berikut pada Eisen sebagai Load Balancer

```sh
location / {
    allow 127.0.0.1;
    allow 10.69.3.69;
    allow 10.69.3.70;
    allow 10.69.4.167;
    allow 10.69.4.168;
    deny all;

    auth_basic "Restricted Content";
    auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
    proxy_pass http://round_robin_auth;
    proxy_set_header    X-Real-IP $remote_addr;
    proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header    Host $http_host;
}
```

Tambahkan script berikut pada Himmel sebagai DHCP Server pada **/etc/dhcp/dhcpd.conf**

```sh
host Revolte {
    hardware ethernet 86:28:2c:ef:7a:81;
    fixed-address 10.69.3.70;
    option host-name \"Revolte\";
}
```

Kemudian jalankan **service isc-dhcp-server restart**. Selanjutnya ubah konfigurasi pada Revolte

```sh
auto eth0
iface eth0 inet dhcp
hwaddress ether 86:28:2c:ef:7a:81
```

## Soal 13

Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.

Tambahkan script berikut pada Denken sebagai Database Server

```sh
#!/bin/bash

apt update
apt install mariadb-server -y

service mysql start

mysql <<EOF
CREATE USER 'kelompokit11'@'%' IDENTIFIED BY 'passwordit11';
CREATE USER 'kelompokit11'@'localhost' IDENTIFIED BY 'passwordit11';
CREATE DATABASE dbkelompokit11;
GRANT ALL PRIVILEGES ON *.* TO 'kelompokit11'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompokit11'@'localhost';
FLUSH PRIVILEGES;
quit
EOF

mysql -u kelompokit11 -p'passwordit11' <<EOF
SHOW DATABASES;
quit
EOF

echo '[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
skip-networking=0
skip-bind-address' >/etc/mysql/my.cnf

service mysql restart
```

## Soal 14

Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan [quest guide](https://github.com/martuafernando/laravel-praktikum-jarkom) berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

Lakukan install depedensi dasar yang digunakan
```
apt-get update
apt-get install lynx -y
apt-get install mariadb-client -y # dipakai untuk mengecek apakah bisa mengakses database yang dibuat di Denken
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get update
```
Kemudian lakukan instalasi kebutuhan ``PHP 8.0``

```
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y
service nginx start
service php8.0-fpm start
```
install ``composer``
```
# Install composer
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer
```
Lakukan git clone terhadap repository https://github.com/martuafernando/laravel-praktikum-jarkom serta jalankan ``composer update``

```
# Install git 
apt-get install git -y
cd /var/www 
git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom 
composer update
```

Setelah selesai jalankan kode berikut untuk melakukan konfigurasi pada ``worker``

```
cd /var/www/laravel-praktikum-jarkom 
cp .env.example .env
echo '
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=10.69.2.2
DB_PORT=3306
DB_DATABASE=dbkelompokit11
DB_USERNAME=kelompokit11
DB_PASSWORD=passwordit11

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
' > /var/www/laravel-praktikum-jarkom/.env
cd /var/www/laravel-praktikum-jarkom
php artisan key:generate
php artisan config:cache
php artisan migrate
php artisan db:seed
php artisan storage:link
php artisan jwt:secret
php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage


echo '
 server {
 	listen 8001;

 	root /var/www/laravel-praktikum-jarkom/public;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
 	}

    location ~ /\.ht {
 			deny all;
 	}

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
 }
' > /etc/nginx/sites-available/implementasi
ln -s /etc/nginx/sites-available/implementasi /etc/nginx/sites-enabled/
unlink /etc/nginx/sites-enabled/default

chown -R www-data.www-data /var/www/laravel-praktikum-jarkom-main/storage

service php8.0-fpm start
service nginx restart
```
Untuk mengecek apakah kode sudah benar bisa menggunakan ``nginx -t``. Pada bagian:

```
server {
 	listen 8001; ...... }
```
portnya diganti sesuai dengan port masing masing worker. Untuk konfigurasi pada kelompok kami, kami membagi port sebagai berikut:

- ``8001``: Frieren
- ``8002``: Fern
- ``8003``: Flamme

Kemudian setelah melakukan konfigurasi, cek dengan menjalankan ``lynx localhost:<port worker>`` (contoh lynx localhost:8001)

Jika sudah benar maka tampilan setelah lynx localhost:<port worker> adalah sebagai berikut
  ![lynx_14](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/b7daae70-3d92-42c5-8cea-5f846fd02b88)

## Soal 15

Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

  POST /auth/register

Buat file ``login.json`` yang nantinya akan dikirimkan dalam perintah ``/POST`` pada ``client``
```
echo '
{
    "username": "kelompokit11",
    "password": "passwordit11"
}
' > /var/www/laravel-praktikum-jarkom/login.json
```
jalankan perintah berikut pada ``client``
``` ab  -n 100 -c 10 -p login.json -T application/json http://10.69.4.2:8001/api/auth/register ```
Untuk bagian ``` http://10.69.4.2:8001 ``` disesuaikan dengan ``ip worker:portnya`` yang ingin diuji. Disini sebagai contoh digunakan ``ip:port`` dari worker ``Frieren``.

hasil

![check_regis](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/b9fc7f75-ed95-481d-a58d-287fed049648)


## Soal 16

Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

  POST /auth/login

Jalankan perintah berikut pada ``client``

``` ab  -n 100 -c 10 -p login.json -T application/json http://10.69.4.2:8001/api/auth/login ```

File ``.JSON`` yang digunakan sama seperti no 15

Hasil testing dengan htop

![htop_login_8001](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/17187d7a-4145-4566-b941-b118b8ab29bb)

![ab_login_8001](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/163591d7-762e-4723-a7e4-43ed393d429e)


## Soal 17

Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

  GET /me

```
curl -X POST -H "Content-Type: application/json" -d @regis.json http://10.69.4.2:8001/api/auth/login > login_token.txt
```
Kode tersebut akan menyimpan respon JSON kedalam ``login_token.txt``

Hasil htop saat load testing dijalankan

![17_htop](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/ce93af9c-4236-4230-b872-c6aabf7b8552)

hasil load testing sesudahnya

![load_test_17_8001](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/5f322900-0c61-4633-b9f0-137b54597bdd)

## Soal 18

Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari ``Frieren``, ``Flamme``, dan ``Fern``.

jalankan kode berikut pada ``Eisen``

```
echo 'upstream laravel_least_conn  {
    server 10.69.4.2:8001; #IP Frieren
    server 10.69.4.3:8003; #IP Flamme
    server 10.69.4.4:8002; #IP Fern
}

server {
    listen 87;

    location /app1 {
        proxy_pass http://10.69.4.2/;
        proxy_bind 10.69.4.2; #IP Frieren
        rewrite ^/app1(.*)$ http://10.69.4.2/$1 permanent;
    }

    location /app2 {
        proxy_pass http://10.69.4.3/;
        proxy_bind 10.69.4.3; #IP Flamme
        rewrite ^/app2(.*)$ http://10.69.4.3/$1 permanent;
    }

    location /app3 {
        proxy_pass http://10.69.4.4/;
        proxy_bind 10.69.4.4; #IP Fern
        rewrite ^/app3(.*)$ http://10.69.4.4/$1 permanent;
    }

    location / {
        proxy_pass http://laravel_least_conn;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;
    }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/proxy-bind

unlink /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/proxy-bind /etc/nginx/sites-enabled/proxy-bind

service nginx restart
```

untuk mengetest jalankan kode berikut pada ``client``:

``` ab  -n 100 -c 10 -p login.json -T application/json http://10.69.2.3:87/api/auth/register ```

`` http://10.69.2.3`` merupakan IP dari ``Eisen`` dan ``87`` adalah port yang digunakan. Setelah dijalankan pergi ke ``worker`` laravel untuk menentukan apakah berhasil melakukan proxy bind dengan mengecek lognya. 

Perintah yang digunakan untuk mengecek log adalah dengan

``` cat /var/log/nginx/implementasi_access.log ```

Log pada ``Frieren`` 

![image](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/b67bd93f-ca33-4a51-8624-36e8cb6a9e2f)


log pada ``Fern`` 

![image](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/1dc8f726-e82b-4e27-b91f-9139f751f941)


log pada ``Flamme ``

![image](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/3577ee4d-419c-42ee-820f-912295a16d39)



## Soal 19

Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 

- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers

sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.

Edit file konfigurasi berikut pada ``eisen``

```
#!/bin/bash
echo '[lb_site]
user = lb_user
group = lb_user
listen = /var/run/php8.0-fpm-lb-site.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = <edit here>
pm.start_servers = <edit here>
pm.min_spare_servers = <edit here>
pm.max_spare_servers = <edit here>
pm.process_idle_timeout = 10s

;' >/etc/php/8.0/fpm/pool.d/lb.conf

groupadd lb_user
useradd -g lb_user lb_user

/etc/init.d/php8.0-fpm restart
/etc/init.d/php8.0-fpm status
```
kemudian jalankan testing pada tiap perubahan script. Disini sebagai contoh dilakukan percobaan pada worker Frieren dengan menjalankan kode

```
ab  -n 100 -c 10 -p login.json -T application/json http://10.69.4.3:8003/api/auth/register
```

script 1:

```
pm = dynamic
pm.max_children = 25
pm.start_servers = 5
pm.min_spare_servers = 3
pm.max_spare_servers = 10
pm.process_idle_timeout = 10s
```

![image](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/035723aa-3334-4e53-bf6f-8e7068bd455a)

script 2:

```
pm = dynamic
pm.max_children = 50
pm.start_servers = 8
pm.min_spare_servers = 5
pm.max_spare_servers = 15
pm.process_idle_timeout = 10s
```

![image](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/74582f55-9a6c-4320-a227-b46ea39b5aef)

Script 3:

```
pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.process_idle_timeout = 10s
```

![image](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/71d628b3-6853-4681-b67b-3351e8980a37)

Penjelasan:

``pm.max_children``: Menentukan jumlah maksimum proses anak (child processes) yang PHP-FPM dapat buat untuk melayani permintaan.

``pm.start_servers``: Menentukan jumlah proses anak yang akan dibuat saat PHP-FPM pertama kali dijalankan.

``pm.min_spare_servers``: Menentukan jumlah minimum proses anak yang akan dijaga hidup oleh PHP-FPM saat tidak ada permintaan yang diterima.

``pm.max_spare_servers``: Menentukan jumlah maksimum proses anak yang diizinkan tetap hidup oleh PHP-FPM saat tidak ada permintaan yang diterima.

pada hasil testing, dapat dilihat bahwa rata-rata waktu processing semakin meningkat tiap script:

- pada script 1: 279 s
- pada script 2: 286 s
- pada script 3: 295 s

Peningkatan rata-rata waktu pemrosesan pada skenario 2 dan 3 mungkin disebabkan oleh penggunaan sumber daya yang lebih tinggi akibat peningkatan jumlah proses anak. Karena pada script terlihat bahwa terjadi peningkatan kapasitas child processes dari tiap script.

## Soal 20

Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.

Lakukan modifikasi pada kode berikut

```
echo 'upstream laravel_least_conn  {
    server 10.69.4.2:8001; #IP Frieren
    server 10.69.4.3:8003; #IP Flamme
    server 10.69.4.4:8002; #IP Fern
}

server {
    listen 87;

    location /app1 {
        proxy_pass http://10.69.4.2/;
        proxy_bind 10.69.4.2; #IP Lawine
        rewrite ^/app1(.*)$ http://10.69.4.2/$1 permanent;
    }

    location /app2 {
        proxy_pass http://10.69.4.3/;
        proxy_bind 10.69.4.3; #IP linie
        rewrite ^/app2(.*)$ http://10.69.4.3/$1 permanent;
    }

    location /app3 {
        proxy_pass http://10.69.4.4/;
        proxy_bind 10.69.4.4; #IP Lugner
        rewrite ^/app3(.*)$ http://10.69.4.4/$1 permanent;
    }

    location / {
        proxy_pass http://laravel_least_conn;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    Host $http_host;
    }

    error_log /var/log/nginx/lb_error.log;
    access_log /var/log/nginx/lb_access.log;
}' >/etc/nginx/sites-available/proxy-bind

unlink /etc/nginx/sites-enabled/default

ln -s /etc/nginx/sites-available/proxy-bind /etc/nginx/sites-enabled/proxy-bind

service nginx restart
```
dengan menambahkan ``Least_conn;`` seperti ini
```
echo 'upstream laravel_least_conn  {
    least_conn;
    server 10.69.4.2:8001;
    server 10.69.4.3:8003;
    server 10.69.4.4:8002;
}
```
Hasilnya adalah sebagai berikut:
sebelum least con

![image](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/a9245a01-1127-463d-ad14-546c3ec6affe)

setelah least con

![image](https://github.com/Yuniarrr/Jarkom-Modul-3-IT11-2023/assets/107184933/ed71fb72-c2c4-4c07-b31f-cb8b701f8159)

dari data terlihat bahwa waktu yang digunakan setelah menggunakan `` Least_conn;`` lebih sedikit dibandingkan yang digunakan daripada yang tidak.

