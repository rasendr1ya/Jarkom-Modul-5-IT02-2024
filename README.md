# Praktikum Modul 05 Komunikasi Data dan Jaringan Komputer
## Kelompok IT02
### Anggota Kelompok :
|             Nama              |     NRP    |
|-------------------------------|------------|
| Gallant Damas Hayuaji         | 5027231037 |
| Danar Bagus Rasendriya        | 5027231055 |

***

### Misi 1: Memetakan Kota New Eridu
> Soal 1: Sebuah topologi sederhana menggambarkan jaringan New Eridu

![Screenshot 2024-11-30 080625](https://github.com/user-attachments/assets/a00a0656-c705-40fe-8bff-e63e448efc42)


Keterangan: 
- HDD: Sebagai DNS Server
- Fairy: Sebagai DHCP Server
- Web Servers: HIA, HollowZero
- Client:
  - Burnice: 5 host
  - Lycaon: 20 host
  - Policeboo: 30 host
  - Caesar: 50 host
  - Ellen: 100 host
  - Jane: 200 host

Berikut ini adalah pembagian subnet pada topologi:

#### Routing Table
|Nama Subnet|Rute|Jumlah IP|Netmask|
|--|--|--|--|
|A1|NewEridu ↔ SixStreet|2|/30|
|A2|SixStreet ↔ Switch RandomPlay ↔ Fairy dan HDD|6|/29|
|A3|SixStreet ↔ Switch Metro ↔ ScootOutpost ↔ OuterRing|6|/29|
|A4|OuterRing ↔ Switch SoC ↔ Caesar dan Burnice|56|/26|
|A5|ScootOutpost ↔ HollowZero|2|/30|
|A6|NewEridu ↔ LuminaSquare|2|/30|
|A7|LuminaSquare ↔ Switch PubSec ↔ Jane dan Policeboo|231|/24|
|A8|LuminaSquare ↔ Switch Ofc.Mewmew ↔ HIA dan BalletTwins|6|/29|
|A9|BalletTwins ↔ Switch Victoria ↔ Ellen dan Lycaon|121|/25|
|Total||432|/23|

#### Alokasi IP
|Subnet|Network ID|Netmask|Broadcast|Range IP|
|-|-|-|-|-|
|A1|192.234.1.220|255.255.255.252|192.234.1.223|192.234.1.221 - 192.234.1.222|
|A2|192.234.1.200|255.255.255.248|192.234.1.207|192.234.1.201 - 192.234.1.206|
|A3|192.234.1.208|255.255.255.248|192.234.1.215|192.234.1.209 - 192.234.1.214|
|A4|192.234.1.128|255.255.255.192|192.234.1.191|192.234.1.129 - 192.234.1.190|
|A5|192.234.1.224|255.255.255.252|192.234.1.227|192.234.1.225 - 192.234.1.226|
|A6|192.234.1.216|255.255.255.252|192.234.1.219|192.234.1.217 - 192.234.1.218|
|A7|192.234.0.0|255.255.255.0|192.234.0.255|192.234.0.1 - 192.234.0.254|
|A8|192.234.1.192|255.255.255.248|192.234.1.199|192.234.1.193 - 192.234.1.198|
|A9|192.234.1.0|255.255.255.128|192.234.1.127|192.234.1.1 - 192.234.1.126|

> Soal 2: Setelah membagi alamat IP menggunakan VLSM, gambarkan pohon subnet yang menunjukkan hierarki pembagian IP di jaringan New Eridu. Lingkari subnet-subnet yang akan dilewati dalam jaringan.

![modul 5](https://github.com/user-attachments/assets/894e985a-8cd0-43b5-a4c7-3c720f4c9785)

#### Tree VLSM
![tree vlsm it02_REV drawio](https://github.com/user-attachments/assets/413adda0-d21e-4bd4-af4f-af87621b61f7)

> Soal 3: Setelah pembagian IP selesai, buatlah konfigurasi rute untuk menghubungkan semua subnet dengan benar di jaringan New Eridu. Pastikan perangkat dapat saling terhubung.

#### Konfigurasi Subnet
Konfigurasi ini dimasukkan ke dalam setiap node
1. Router: NewEridu 
```bash
auto eth0
iface eth0 inet dhcp

# A1
auto eth1
iface eth1 inet static
	address 192.234.1.221
	netmask 255.255.255.252

# A6
auto eth2
iface eth2 inet static
	address 192.234.1.217
	netmask 255.255.255.252
```

2. Router: SixStreet (A1, A2, A3)
```bash
# A1
auto eth0
iface eth0 inet static
	address 192.234.1.222
	netmask 255.255.255.252
	gateway 192.234.1.221

# A2
auto eth1
iface eth1 inet static
	address 192.234.1.201
	netmask 255.255.255.248

# A3
auto eth2
iface eth2 inet static
	address 192.234.1.209
	netmask 255.255.255.248
```

3. Router: OuterRing (A3, A4)
```bash
# A3
auto eth0
iface eth0 inet static
	address 192.234.1.210
	netmask 255.255.255.248
	gateway 192.234.1.209

# A4
auto eth1
iface eth1 inet static
	address 192.234.1.129
	netmask 255.255.255.192
```

4. Router: ScootOutpost (A3, A5)
```bash
# A3
auto eth0
iface eth0 inet static
	address 192.234.1.211
	netmask 255.255.255.248
	gateway 192.234.1.209

# A5
auto eth1
iface eth1 inet static
	address 192.234.1.225
	netmask 255.255.255.252
```

5. Router: LuminaSquare (A6, A7, A8)
```bash
# A6
auto eth0
iface eth0 inet static
	address 192.234.1.218
	netmask 255.255.255.252
	gateway 192.234.1.217

# A7
auto eth1
iface eth1 inet static
	address 192.234.0.1
	netmask 255.255.255.0

# A8
auto eth2
iface eth2 inet static
	address 192.234.1.193
	netmask 255.255.255.248
```

6. Router: BalletTwins (A8, A9)
```bash
# A8
auto eth0
iface eth0 inet static
	address 192.234.1.194
	netmask 255.255.255.248
  	gateway 192.234.1.193

# A9
auto eth1
iface eth1 inet static
  	address 192.234.1.1
	netmask 255.255.255.128
```

7. Webserver: HIA (A7)
```bash
# A8
auto eth0
iface eth0 inet static
	address 192.234.1.195
	netmask 255.255.255.248
  	gateway 192.234.1.193
```

8. Webserver: HollowZero (A5)
```bash
# A5
auto eth0
iface eth0 inet static
	address 192.234.1.226
	netmask 255.255.255.252
	gateway 192.234.1.225
```

9. DNS Server: HDD (A2)
```bash
# A2
auto eth0
iface eth0 inet static
	address 192.234.1.203
	netmask 255.255.255.248
	gateway 192.234.1.201
```

10. DHCP Server: Fairy (A2)
```bash
# A2
auto eth0
iface eth0 inet static
	address 192.234.1.202
	netmask 255.255.255.248
	gateway 192.234.1.201
```

11. Clients: Jane (A7/ 200 Host)
```bash
auto eth0
iface eth0 inet dhcp
```

12. Clients: Ellen (A9/ 100 Host)
```bash
auto eth0
iface eth0 inet dhcp
```

13. Client: Burnice (5 Host)
```bash
auto eth0
iface eth0 inet dhcp
```

14. Client: Caesar (50 Host)
```bash
auto eth0
iface eth0 inet dhcp
```

15. Client: PoliceBoo (30 Host)
```bash
auto eth0
iface eth0 inet dhcp
```

16. Client: Lycaon (20 Host)
```bash
auto eth0
iface eth0 inet dhcp
```

#### Konfigurasi Routing
1. Router: NewEridu
```bash
auto eth0
iface eth0 inet dhcp

# A1
auto eth1
iface eth1 inet static
	address 192.234.1.221
	netmask 255.255.255.252

# A6
auto eth2
iface eth2 inet static
	address 192.234.1.217
	netmask 255.255.255.252

# menambahkan konfigurasi di bawah ini
# Kanan
post-up route add -net 192.234.0.0 netmask 255.255.255.0 gw 192.234.1.218
post-up route add -net 192.234.1.192 netmask 255.255.255.248 gw 192.234.1.218
post-up route add -net 192.234.1.0 netmask 255.255.255.128 gw 192.234.1.218

# Kiri
post-up route add -net 192.234.1.200 netmask 255.255.255.248 gw 192.234.1.222
post-up route add -net 192.234.1.208 netmask 255.255.255.248 gw 192.234.1.222
post-up route add -net 192.234.1.128 netmask 255.255.255.192 gw 192.234.1.222
post-up route add -net 192.234.1.224 netmask 255.255.255.252 gw 192.234.1.222
```

2. Router: SixStreet

```bash
# A1
auto eth0
iface eth0 inet static
	address 192.234.1.222
	netmask 255.255.255.252
	gateway 192.234.1.221

# A2
auto eth1
iface eth1 inet static
	address 192.234.1.201
	netmask 255.255.255.248

# A3
auto eth2
iface eth2 inet static
	address 192.234.1.209
	netmask 255.255.255.248

# menambahkan konfigurasi di bawah ini
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.234.1.221
post-up route add -net 192.234.1.128 netmask 255.255.255.192 gw 192.234.1.210
post-up route add -net 192.234.1.224 netmask 255.255.255.252 gw 192.234.1.211
```

3. Router: OuterRing
```bash
# A3
auto eth0
iface eth0 inet static
	address 192.234.1.210
	netmask 255.255.255.248
	gateway 192.234.1.209

# A4
auto eth1
iface eth1 inet static
	address 192.234.1.129
	netmask 255.255.255.192

# menambahkan konfigurasi di bawah ini
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.234.1.209
post-up route add -net 192.234.1.224 netmask 255.255.255.252 gw 192.234.1.211
```

4. Router: ScootOutpost
```bash
# A3
auto eth0
iface eth0 inet static
	address 192.234.1.211
	netmask 255.255.255.248
	gateway 192.234.1.209

# A5
auto eth1
iface eth1 inet static
	address 192.234.1.225
	netmask 255.255.255.252

# menambahkan konfigurasi di bawah ini
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.234.1.209
post-up route add -net 192.234.1.128 netmask 255.255.255.192 gw 192.234.1.210
```

5. Router: LuminaSquare
```bash
# A6
auto eth0
iface eth0 inet static
	address 192.234.1.218
	netmask 255.255.255.252
	gateway 192.234.1.217

# A7
auto eth1
iface eth1 inet static
	address 192.234.0.1
	netmask 255.255.255.0

# A8
auto eth2
iface eth2 inet static
	address 192.234.1.193
	netmask 255.255.255.248

# menambahkan konfigurasi
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.234.1.217
post-up route add -net 192.234.1.0 netmask 255.255.255.128 gw 192.234.1.194
```

6. Router: BalletTwins
```bash
# A8
auto eth0
iface eth0 inet static
	address 192.234.1.194
	netmask 255.255.255.248
 	gateway 192.234.1.193

# A9
auto eth1
iface eth1 inet static
  	address 192.234.1.1
	netmask 255.255.255.128

# menambahkan konfigurasi
post-up route add -net 0.0.0.0 netmask 0.0.0.0 gw 192.234.1.192
```

### Soal 4: Konfigurasi → dikerjakan setelah misi 2 nomor 1
> - Fairy sebagai DHCP Server agar perangkat yang berada dalam Burnice, Caesar, Ellen, Jane, Lycaon, dan Policeboo dapat menerima alamat IP secara otomatis.
> - OuterRing, Ballet Twins, Sixstreet dan LuminaSquare Sebagai DHCP Relay
> - HDD sebagai DNS server
> - HIA dan HollowZero Sebagai Web server (gunakan apache)

#### DHCP Server (dhcps.sh)
Masuk ke terminal Fairy (DHCP Server) untuk install `DHCP Server` lalu masukkan konfigurasi
```bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y

echo '
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

echo '
# Jane & Policeboo
subnet 192.234.0.0 netmask 255.255.255.0 {
  range 192.234.0.2 192.234.0.254;
  option routers 192.234.0.1;
  option broadcast-address 192.234.0.255;
  option domain-name-servers 192.234.1.203;
}

# Ellen & Lycaon
subnet 192.234.1.0 netmask 255.255.255.128 {
  range 192.234.1.2 192.234.1.126;
  option routers 192.234.1.1;
  option broadcast-address 192.234.1.127;
  option domain-name-servers 192.234.1.203;
}

# Caesar & Burnice
subnet 192.234.1.128 netmask 255.255.255.192 {
  range 192.234.1.130 192.234.1.190;
  option routers 192.234.1.129;
  option broadcast-address 192.234.1.191;
  option domain-name-servers 192.234.1.203;
}

subnet 192.234.1.200 netmask 255.255.255.248 {
  range 192.234.1.202 192.234.1.206;
  option routers 192.234.1.201;
  option broadcast-address 192.234.1.207;
  option domain-name-servers 192.234.1.203;
}
' > /etc/dhcp/dhcpd.conf
```

Jika sudah, lakukan restart DHCP Server.
```bash
service isc-dhcp-server restart
```

![Screenshot 2024-12-08 202352](https://github.com/user-attachments/assets/ac7d729a-3f2f-4ffb-a9c6-b1fc3bff3f8a)


#### DHCP Relay (dhcpr.sh)
Masuk ke node `SixStreet` untuk install `DHCP Relay` lalu masukkan konfigurasi
```bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-relay -y

echo '
SERVERS="192.234.1.202" #fairy
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

echo '
net.ipv4.ip_forward=1
' > /etc/sysctl.conf
```

Lakukan restart DHCP Relay
```bash
service isc-dhcp-relay restart
```

![Screenshot 2024-12-08 202326](https://github.com/user-attachments/assets/a1d38d28-9b65-44d7-a16a-681e03a919ba)

#### DNS Server (dnss.sh)
Install `bind9` terlebih dahulu
```bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y

echo 'options {
    directory "/var/cache/bind";

    forwarders {
        192.168.122.1;
    };

    // dnssec-validation auto;

    allow-query { any; };
    auth-nxdomain no;
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```

Lakukan restart bind9
```bash
service bind9 restart
```

![Screenshot 2024-12-08 202625](https://github.com/user-attachments/assets/71267e16-5f54-4a07-989b-01bcb6ab3675)

#### WebServer (webs.sh)
Buka console WebServer `HIA` dan `HollowZero` untuk install `apache` dan masukkan konfigurasi
```bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install apache2 -y

echo "Welcome to {hostname}" > /var/www/html/index.html
```

Restart apache
```bash
service apache2 restart
```

HIA

![Screenshot 2024-12-08 203005](https://github.com/user-attachments/assets/e95d510a-0203-4d64-975e-abfb7f4218ab)

HollowZero

![Screenshot 2024-12-08 203146](https://github.com/user-attachments/assets/3f363076-c9fd-4c9f-afc7-47bec16c1389)

### Misi 2: Menemukan Jejak Sang Peretas
> Soal 1: Agar jaringan di New Eridu bisa terhubung ke luar (internet), kalian perlu mengkonfigurasi routing menggunakan iptables. Namun, kalian tidak diperbolehkan menggunakan MASQUERADE.

Membuka terminal web console NewEridu dan 
```bash
ETH0_IP=$(ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $ETH0_IP
```

![Screenshot 2024-12-07 171722](https://github.com/user-attachments/assets/08e9b84a-24bb-46da-a924-f27d2f407e50)

> Soal 2: Karena Fairy adalah Al yang sangat berharga, kalian perlu memastikan bahwa tidak ada perangkat lain yang bisa melakukan ping ke Fairy. Tapi Fairy tetap dapat mengakses seluruh perangkat.

Masuk ke console Fairy dan jalankan command berikut
```bash
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
iptables -A OUTPUT -p icmp --icmp-type echo-request -j ACCEPT
```

Perintah `-j DROP` akan memblokir ping yang masuk ke Fairy. Lalu `-j ACCEPT` adalah perintah untuk mengizinkan Fairy ping ke perangkat lain.

![Screenshot 2024-12-08 204343](https://github.com/user-attachments/assets/53e460fb-3e86-4536-8705-a17ef67ebaad)

Untuk mengembalikan Fairy ke pengaturan awal, jalankan command berikut
```bash
iptables -D INPUT -p icmp --icmp-type echo-request -j DROP
iptables -D OUTPUT -p icmp --icmp-type echo-request -j ACCEPT
```

***
