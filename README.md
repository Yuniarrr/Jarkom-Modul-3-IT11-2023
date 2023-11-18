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
    option domain-name-servers 10.69.1.3;
    default-lease-time 180;
    max-lease-time 5760; # 96 minutes
}" >/etc/dhcp/dhcpd.conf

rm /var/run/dhcpd.pid

service isc-dhcp-server restart

service isc-dhcp-server status
```

## Soal 3

Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

## Soal 4

Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut.

## Soal 5

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit 

## Soal 6

Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website [berikut](https://drive.google.com/file/d/1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1/view?usp=sharing) dengan menggunakan php 7.3. 

## Soal 7

Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
    Lawine, 4GB, 2vCPU, dan 80 GB SSD.
    Linie, 2GB, 2vCPU, dan 50 GB SSD.
    Lugner 1GB, 1vCPU, dan 25 GB SSD.
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

## Soal 8

Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
- Nama Algoritma Load Balancer
- Report hasil testing pada Apache Benchmark
- Grafik request per second untuk masing masing algoritma. 
- Analisis

## Soal 9

Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

## Soal 10

Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

## Soal 11

Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id.

## Soal 12

Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.

## Soal 13

Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.

## Soal 14

Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan [quest guide](https://github.com/martuafernando/laravel-praktikum-jarkom) berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

## Soal 15

Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

  POST /auth/register

## Soal 16

Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

  POST /auth/login

## Soal 17

Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.

  GET /me

## Soal 18

Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.

## Soal 19

Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 

- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers

sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.

## Soal 20

Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.


