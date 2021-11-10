# Jarkom-Modul-3-TI6-2021
## Anggota Kelompok TI6:

- Syakhisk Al-Azmi 05311940000003
- Muhammad Rizqi Wijaya 05311940000014
- Shafira Firdausi 05311940000040

## Soal

### 1. Buat peta dengan kriteria **EniesLobby** sebagai DNS Server, **Jipangu** sebagai DHCP Server, **Water7** sebagai Proxy Server.

Pertama dibuat topologi seperti gambar berikut

![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled.png)

Pertama dilakukan konfigurasi pada Jipangu sebagai DHCP Server. Dilakukan instalasi dhcp server dengan command `apt install isc-dhcp-server -y` dan dilakukan konfigurasi interfaces pada `/etc/default/isc-dhcp-server` dengan mengisi `INTERFACES="eth0"`. 

***Hasil:***

- EniesLobby sebagai DNS Server
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%201.png)
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%202.png)
    
- Jipangu sebagai DHCP Server
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%203.png)
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%204.png)
    
- Water7 sebagai Proxy Server
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%205.png)
    

### 2. Buat **Foosha** sebagai DHCP Relay

Untuk menjadikan Foosha sebagai DHCP Relay, pertama lakukan instalasi dhcp relay pada Foosha dengan command `apt install isc-dhcp-relay -y` lalu konfigurasikan server dan interfaces pada `/etc/default/isc-dhcp-relay` seperti berikut.

```bash
SERVERS="192.214.2.4"
INTERFACES="eth3 eth1 eth2"
OPTIONS=""
```

Setelah itu restart dengan command `service isc-dhcp-relay restart`.

***Hasil:*** 

![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%206.png)

### 3. Semua client yang ada **HARUS** menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari 192.214.1.20 - 192.214.1.99 dan 192.214.1.150 - 192.214.1.169

Dilakukan konfigurasi pada Jipangu yakni di `/etc/dhcp/dhcpd.conf`. Tambahkan subnet untuk switch2 terlebih dahulu agar subnet dapat berjalan. Lalu tambahkan juga subnet untuk switch1 dengan dua range seperti berikut.

```bash
subnet 192.214.2.0 netmask 255.255.255.0 {}

subnet 192.214.1.0 netmask 255.255.255.0 {
    range 192.214.1.20 192.214.1.99;
    range 192.214.1.150 192.214.1.169;
    option routers 192.214.1.1;
    option broadcast-address 192.214.1.255;
    default-lease-time 600;
    max-lease-time 7200;
}
```

***Hasil:***

- IP Loguetown
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%207.png)
    
- IP Alabasta
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%208.png)
    

### 4. Client yang melalui Switch3 mendapatkan range IP dari 192.214.3.30 - 192.214.3.50

Pada file yang sama seperti No.3, tambahkan satu subnet lagi untuk switch3 seperti berikut.

```bash
subnet 192.214.3.0 netmask 255.255.255.0 {
    range 192.214.3.30 192.214.3.50;
    option routers 192.214.3.1;
    option broadcast-address 192.214.1.255;
    default-lease-time 600;
    max-lease-time 7200;
}
```

***Hasil:***

- IP Skypie
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%209.png)
    
- IP TottoLand
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2010.png)
    

### 5. Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut

Tambahkan konfigurasi option DNS dengan IP EniesLobby pada masing-masing subnet switch1 dan switch 3 yakni `option domain-name-servers 192.214.2.2;` sehingga menjadi seperti berikut.

```bash
subnet 192.214.1.0 netmask 255.255.255.0 {
    range 192.214.1.20 192.214.1.99;
    range 192.214.1.150 192.214.1.169;
    option routers 192.214.1.1;
    option broadcast-address 192.214.1.255;
    option domain-name-servers 192.214.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 192.214.3.0 netmask 255.255.255.0 {
    range 192.214.3.30 192.214.3.50;
    option routers 192.214.3.1;
    option broadcast-address 192.214.1.255;
    option domain-name-servers 192.214.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}
```

***Hasil:***

- Loguetown
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2011.png)
    
- Alabasta
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2012.png)
    
- Skypie
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2013.png)
    
- TottoLand
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2014.png)
    

### 6. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit

Ubah default dan max lease time masing-masing subnet switch:

- Switch1
    
    ```bash
    default-lease-time 360;
    max-lease-time 7200;
    ```
    
- Switch2
    
    ```bash
    default-lease-time 720;
    max-lease-time 7200;
    ```
    

***Hasil:***

### 7. Jadikan **Skypie** sebagai server menggunakan **alamat IP yang tetap** dengan IP 192.214.3.69

Untuk memberikan Skypie sebuah fixed-address, pertama pada konfigurasi dhcp Jipangu tambahkan satu host Skypie di `/etc/dhcp/dhcpd.conf` seperti berikut, lalu restart.

```bash
host Skypie {
    hardware ethernet aa:6e:ae:6a:df:e7; // [hwaddress skypie]
    fixed-address 192.214.3.69;
}
```

Setelah itu pada Skypie, lakukan konfigurasi network interface pada  `/etc/network/interfaces` dengan menambahkan baris berikut.

```bash
hwaddress ether aa:6e:ae:6a:df:e7 // [hwaddress skypie]
```

***Hasil:***

![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2015.png)

### 8. Pada Loguetown, proxy **harus bisa diakses** dengan nama **jualbelikapal.ti6.com** dengan **port** yang digunakan adalah **5000**

Di EniesLobby, lakukan `apt install bind9 -y`. Setelah itu konfigurasikan forwarder pada `/etc/bind/named.conf.options` seperti berikut.

```bash
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        allow-query { any; };

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

Siapkan folder untuk [jualbelikapal.ti6.com](http://jualbelikapal.ti6.com) dengan command `mkdir -p /etc/bind/jarkom/`

Buat zone [jualbelikapal.ti6.com](http://jualbelikapal.ti6.com) pada `/etc/bind/named.conf.local` seperti berikut.

```bash
zone "jualbelikapal.ti6.com" {
  type master;
  file "/etc/bind/jarkom/jualbelikapal.ti6.com";
};
```

Konfigurasikan `/etc/bind/jarkom/jualbelikapal.ti6.com` seperti berikut.

```bash
@       IN      SOA     jualbelikapal.ti6.com. root.jualbelikapal.ti6.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      jualbelikapal.ti6.com.
@       IN      A       192.214.2.3
```

Lalu restart bind9 dengan command `service bind9 restart`.

Selanjutnya pada Water7, lakukan instalasi squid dan apache2-utils terlebih dahulu dengan command `apt install squid apache2-utils -y`. Setelah itu pada `/etc/squid/squid.conf` konfigurasikan port dan hostnamenya seperti berikut.

```bash
http_port 5000
visible_hostname Water7
```

Jika sudah, pergi ke Loguetown untuk mengaktifkan proxy dengan command `export http_proxy="http://jualbelikapal.ti6.com:5000"` .

***Hasil:***

![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2016.png)

### 9. Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy ****dipasang **autentikasi user proxy dengan enkripsi MD5** dengan **dua username,** yaitu luffybelikapalti6 dengan password ****luffy_ti6 **dan** zorobelikapalti6 dengan password zoro_ti6

Pada Water7, install package `apache2-utils` untuk dapat menggunakan package `htpasswd`. Lalu buat file passwd dengan command `touch /etc/squid/passwd` dan tambahkan dua user beserta password dengan command berikut

```bash
touch /etc/squid/passwd
htpasswd -mb /etc/squid/passwd luffybelikapalti6 luffy_ti6
htpasswd -mb /etc/squid/passwd zorobelikapalti6 zoro_ti6
```

Lalu tambahkan konfigurasi di `/etc/squid/squid.conf` seperti berikut

```bash
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

http_access allow USERS
```

Lalu restart

***Hasil:***

Jika melakukan lynx tanpa authorization

![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2017.png)

- Luffy
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2018.png)
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2019.png)
    
- Zoro
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2020.png)
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2022.png)
    

### 10. Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari **Senin-Kamis pukul 07.00-11.00** dan setiap hari **Selasa-Jum’at pukul 17.00-03.00** keesokan harinya **(sampai Sabtu pukul 03.00)**

Pada Water7, buat file acl.conf pada `/etc/squid/acl.conf` dan tambahkan beberapa baris code sesuai acltime yang ditentukan seperti berikut.

```bash
acl acltime1 time MTWH 07:00-11:00
acl acltime2 time TWHF 17:00-23:59
acl acltime3 time WHFA 00:00-03:00
```

Setelah itu tambahkan konfigurasi http_access allow time beserta user pada `/etc/squid/squid.conf` sehingga menjadi seperti berikut.

```bash
http_port 5000

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on

acl USERS proxy_auth REQUIRED

http_access allow acltime1 USERS
http_access allow acltime2 USERS
http_access allow acltime3 USERS

http_access deny all

visible_hostname Water7
```

***Hasil:***

- Saat mengkases pada jam yang tersedia
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2023.png)
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2024.png)
    
- Saat mengakses ***diluar*** jam yang tersedia
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2025.png)
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2026.png)
    

### 11. Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap **mengakses google.com, akan diredirect menuju super.franky.ti6.com** dengan website yang sama pada soal shift modul 2. Web server super.franky.ti6.com berada pada node **Skypie**

Pertama, perlu dibuat zone untuk [super.franky.ti6.com](http://super.franky.ti6.com) dan dikonfigurasikan terlebih dahulu di EniesLobby. Tambahkan code berikut pada `/etc/bind/named.conf.local`.

```bash
zone "super.franky.ti6.com" {
  type master;
  file "/etc/bind/jarkom/super.franky.ti6.com";
};
```

Setelah itu konfigurasikan `/etc/bind/jarkom/super.franky.ti6.com` seperti berikut.

```bash
\$TTL    604800
@       IN      SOA     super.franky.ti6.com. root.super.franky.ti6.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      super.franky.ti6.com.
@       IN      A       192.214.3.69
```

Lakukan restart bind.

Selanjutnya pada Skypie, lakukan terlebih dahulu beberapa instalasi yakni apache2, unzip, dan php. Lalu unduh file super.franky.zip dan pindahkan ke `/var/www/super.franky.ti6.com`.

```bash
curl -Lk https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/super.franky.zip -o super.franky.zip
unzip super.franky.zip
mv super.franky /var/www/super.franky.ti6.com
```

Buat VirtualHost dan konfigurasikan super.franky pada `/etc/apache2/sites-available/000-default.conf`.

```bash
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        ServerName super.franky.ti6.com
        ServerAlias www.super.franky.ti6.com
        DocumentRoot /var/www/super.franky.ti6.com

         <Directory /var/www/super.franky.ti6.com>
             Options +FollowSymLinks -Multiviews
             AllowOverride All
         </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Lalu restart apache2.

Untuk bisa melakukan redirect [google.com](http://google.com) menuju super.franky.ti6.com, tambahkan beberapa baris berikut di file `/etc/squid/squid.conf` pada Water7.

```bash
acl lan src 192.214.1.0/24 192.214.3.0/24
acl badsites dstdomain .google.com
deny_info http://super.franky.ti6.com/ lan
http_reply_access deny badsites lan

dns_nameservers 192.214.2.2
```

***Hasil:***

- Ping super.franky.ti6.com
    
    ![Untitled](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/Untitled%2027.png)
    
- Redirect
    
    ![keren.gif](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/keren.gif)
    

### 12. Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk **mencari harta karun di super.franky.ti6.com**. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk **mendapatkan gambar (.png, .jpg)**, sedangkan Zoro **mendapatkan sisanya**. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan **10 kbps**

Di file `/etc/squid/squid.conf` pada Water7, akan dikonfigurasikan untuk pengunduhan gambar bagi Luffy dan Zoro. Dibuat terlebih dahulu acl proxy_auth dan acl urlpath_regex untuk masing-masing user seperti berikut 

```bash
acl luffy proxy_auth luffybelikapalti6
acl zoro proxy_auth zorobelikapalti6
acl downloadluffy urlpath_regex -i \.jpg$ \.png$
acl downloadzoro urlpath_regex -i !\.jpg$ !\.png$
```

Setelah itu dilakukan deny download dengan http_reply_access untuk Luffy dan Zoro seperti berikut

```bash
http_reply_access deny downloadzoro luffy
http_reply_access deny downloadluffy zoro
```

Karena kecepatan Luffy dibatasi sebesar 10kbps, maka diberikan pembatasan bandwith dengan menambahkan code berikut

```bash
delay_pools 1
delay_class 1 1
delay_parameters 1 1250/1250
delay_access 1 allow downloadluffy luffy
delay_access 1 deny all
```

***Hasil:***

*Note: Untuk hasil download Zoro ada di soal berikutnya*

- Download gambar sebagai Luffy
    
    ![download_luffy.gif](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/download_luffy.gif)
    

### 13. Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya

Karena kecepatan Zoro tidak dibatasi, maka tidak diberikan pembatasan bandwith untuk Zoro.

***Hasil:***

- Download gambar sebagai Zoro
    
    ![download_zoro.gif](Laporan%20Soal%20Shift%203%20d7b50ee772084ceea0e1927ad7acf740/download_zoro.gif)
