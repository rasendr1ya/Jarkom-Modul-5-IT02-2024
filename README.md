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
|A1|192.234.1.224|255.255.255.252|192.234.1.227|192.234.1.225 - 192.234.1.226|
|A2|192.234.1.208|255.255.255.248|192.234.1.215|192.234.1.209 - 192.234.1.214|
|A3|192.234.1.200|255.255.255.248|192.234.1.207|192.234.1.201 - 192.234.1.206|
|A4|192.234.1.128|255.255.255.192|192.234.1.191|192.234.1.129 - 192.234.1.190|
|A5|192.234.1.228|255.255.255.252|192.234.1.231|192.234.1.229 - 192.234.1.230|
|A6|192.234.1.232|255.255.255.252|192.234.1.235|192.234.1.233 - 192.234.1.234|
|A7|192.234.0.0|255.255.255.0|192.234.0.255|192.234.0.1 - 192.234.0.254|
|A8|192.234.1.192|255.255.255.248|192.234.1.199|192.234.1.193 - 192.234.1.198|
|A9|192.234.1.0|255.255.255.128|192.234.1.127|192.234.1.1 - 192.234.1.126|

> Soal 2: Setelah membagi alamat IP menggunakan VLSM, gambarkan pohon subnet yang menunjukkan hierarki pembagian IP di jaringan New Eridu. Lingkari subnet-subnet yang akan dilewati dalam jaringan.

![modul 5](https://github.com/user-attachments/assets/894e985a-8cd0-43b5-a4c7-3c720f4c9785)


> Soal 3: Setelah pembagian IP selesai, buatlah konfigurasi rute untuk menghubungkan semua subnet dengan benar di jaringan New Eridu. Pastikan perangkat dapat saling terhubung.

#### Konfigurasi Subnet
1. Router: NewEridu 
```bash
# NAT
auto eth0
iface eth0 inet dhcp

# A1
auto eth1
iface eth1 inet static
  address 192.234.1.217
  netmask 255.255.255.252

# A6
auto eth2
iface eth2 inet static
  address 192.234.1.221
  netmask 255.255.255.252
```

2. Router: SixStreet (A1, A2, A3)
```bash
# A1
auto eth0
iface eth0 inet static
  address 192.234.1.218
  netmask 255.255.255.252
  gateway 192.234.1.217

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
  address 192.234.1.222
  netmask 255.255.255.252
  gateway 192.234.1.221

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
auto eth0
iface eth0 inet static
  address 192.234.1.195
  netmask 255.255.255.248
  gateway 192.234.1.193
```

8. Webserver: HollowZero (A5)
```bash
auto eth0
iface eth0 inet static
  address 192.234.1.226
  netmask 255.255.255.252
  gateway 192.234.1.225
```

9. DNS Server: HDD (A2)
```bash
auto eth0
iface eth0 inet static
  address 192.234.1.202
  netmask 255.255.255.248
  gateway 192.234.1.201
```

10. DHCP Server: Fairy (A2)
```bash
auto eth0
iface eth0 inet static
  address 192.234.1.203
  netmask 255.255.255.248
  gateway 192.234.1.201
```

11. Clients: Jane (A7)
```bash
auto eth0
iface eth0 inet dhcp
```

12. Clients: Ellen (A9)
```bash
auto eth0
iface eth0 inet dhcp
```

#### Konfigurasi Routing
1. Router: NewEridu
```bash
# A2
route add -net 192.234.1.192 netmask 255.255.255.248 gw 192.234.1.217

# A3
route add -net 192.234.1.200 netmask 255.255.255.248 gw 192.234.1.217

# A4
route add -net 192.234.1.220 netmask 255.255.255.252 gw 192.234.1.217

# A5
route add -net 192.234.1.128 netmask 255.255.255.192 gw 192.234.1.217

# A7
route add -net 192.234.0.0 netmask 255.255.255.0 gw 192.234.1.225

# A8
route add -net 192.234.1.208 netmask 255.255.255.248 gw 192.234.1.225

# A9
route add -net 192.234.1.0 netmask 255.255.255.128 gw 192.234.1.225

# iptables untuk NAT
IP_ETH0=$(ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $IP_ETH0

echo 'Routing berhasil diterapkan pada NewEridu'
```

2. Router: SixStreet

Buka Web Console pada SixStreet, lalu masukkan konfigurasi di bawah ini.
```bash
# A4
route add -net 192.234.1.220 netmask 255.255.255.252 gw 192.234.1.205

# A5
route add -net 192.234.1.128 netmask 255.255.255.192 gw 192.234.1.205

# A7
route add -net 192.234.0.0 netmask 255.255.255.0 gw 192.234.1.218

# A8
route add -net 192.234.1.208 netmask 255.255.255.248 gw 192.234.1.218

# A9
route add -net 192.234.1.0 netmask 255.255.255.128 gw 192.234.1.218

# DHCP relay setup
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt install isc-dhcp-relay -y

echo 'SERVERS="192.234.1.202"
INTERFACES="eth0 eth1 eth2 eth3"
OPTIONS=""
' > /etc/default/isc-dhcp-relay

# Enable IP forwarding
echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf

# Restart DHCP relay service
service isc-dhcp-relay restart

echo 'Routing berhasil diterapkan pada SixStreet'
```

### Misi 2: Menemukan Jejak Sang Peretas
