# Jarkom_Modul3_Lapres_D06

 - **Rafid Ferdianto - 05111840000032**
 - **Abdur Rohman - 05111840000100**

## Soal 1
 **Membuat Topologi jaringan dengan; Surabaya sebagai router, Malang sebagai DNS Server, Tuban sebagai DHCP server, Mojokerto sebagai Proxy server, serta Sidoarjo, Gresik, Banyuwangi, dan Madiun sebagai Client**
  
  - Pertama pada `topologi.sh` dibuatkan setting sebagai berikut :
  
  ```
  # Switch
  uml_switch -unix switch1 > /dev/null < /dev/null &
  uml_switch -unix switch2 > /dev/null < /dev/null &
  uml_switch -unix switch3 > /dev/null < /dev/null &

  # Router
  xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.78.29 eth1=daemon,,,switch1 eth2=daemon,,,switch3 eth3=daemon,,,switch2 mem=256M &

  # Server
  xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=160M &
  xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
  xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

  # Klien
  xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &
  xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &
  xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI,jarkom umid=BANYUWANGI eth0=daemon,,,switch3 mem=64M &
  xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &
  ```   

   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.1.png)
  
  - Kedua, pada UML **Surabaya** di-enable packet forwarding for IPv4 dengan cara di-**uncomment**, pada file `/etc/sysctl.conf`
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.2.png)
  
  - Ketiga, pada **setiap** UML dibuatkan interface baru pada file `/etc/network/interfaces` sebagai berikut:
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.3.png)
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.4.png)
 
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.5.png)
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.6.png)
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.7.png)
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.8.png)
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.9.png)
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.10.png)
  
  - Keempat, pada UML **Surabaya** dibuatkan file `iptables.sh`, sebagai berikut:
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.11.png)
  
  - Kelima, jalankan **iptables.sh** yang baru saja dibuat dengan `bash iptables.sh`
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.12.png)
  
  - Terakhir, pada **setiap** UML dibuatkan file `export.sh` sebagai berikut, dan dijalankan dengan ` . export.sh `
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/1.13.png)
  
## Soal 2
 **Membuat Surabaya menjadi sebagai perantara (DHCP Relay) antara DHCP Server dan client**
 
 - Pertama, pada UML **Tuban** diinstall **DHCP Server** denggan perintah `apt-get install isc-dhcp-server`
 - Kedua, setelah selesai proses instalasi **DHCP Server** disetting file `/etc/default/isc-dhcp-server`
  
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.1.png)
 
 - Ketiga, pada UML **Surabaya** diinstall **DHCP Relay** denggan perintah `apt-get install isc-dhcp-relay`
 - Keempat, setelah selesai proses instalasi **DHCP Relay** disetting file `/etc/default/isc-dhcp-relay`, dengan dimasukkannya IP Tuban dan interface yang digunakan
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.2.png)
  
 - Kelima, pada UML **Tuban**, ditambahkan **subnet NID_DMZ** pada file `/etc/dhcp/dhcp.conf`, seperti berikut:
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.3.png)
 
 - Keenam, pada setiap UML **Client**, dirubah semua interface yang static menjadi menggunakan **DHCP**, seperti berikut:
  
    ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.4.png)
  
 - Berikut ini adalah langkah **OPTIONAL**, apabila ingin membuat server merequest ip menggunakan *DHCP*, 
   - Pertama pada UML **Server**, cari **hardware access** dengan perintah `ifconfig` dan melihat pada bagian **HWaddr**
   
     ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.5.png)
   
     ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.6.png)
   
   - Kedua pada setiap UML **Server**, ditambahkan setting **interface** pada file `/etc/network/interfaces` sesuai **hardware address** masing-masing, sebagai berikut:
   
     ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.7.png)
   
     ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.8.png)
   
     ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/2.9.png)
   
   - Terakhir pada UML **Tuban**, di-Restart **DHCP Server** dengan perintah `service isc-dhcp-server restart`

## Soal 3
 **Membuat Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200**
 
 - Caranya adalah pada UML **Tuban** disetting pada file `/etc/dhcp/dhcpd.conf` dengan menambah range yang sesuai untuk semua Client dengan subnet 1.
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/3.1.png)
   
 - Untuk mengetest, pada UML Client dengan subnet 1, yaitu **Gresik** dan **Sidoarjo**, jalankan perintah `ifconfig`
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/3.2.png)
   
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/3.3.png)
 
## Soal 4
 **Membuat Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70**
 
 - Caranya adalah pada UML **Tuban** disetting pada file `/etc/dhcp/dhcpd.conf` dengan menambah range yang sesuai untuk semua Client dengan subnet 3.
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/4.1.png)

 - Untuk mengetest, pada UML Client dengan subnet 3, yaitu **Banyuwangi** dan **Madiun**, jalankan perintah `ifconfig`
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/3.2.png)
   
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/3.3.png)
   
## Soal 5
 **Membuat Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP**
 
 - Caranya adalah pada UML **Tuban** disetting pada file `/etc/dhcp/dhcpd.conf` dengan menambahkan `option domain-name-serves` sesuai rangenya pada subnet 1 dan 3
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/5.1.png)
   
 - Untuk mengetest, pada setiap UML **Client** , jalankan perintah `ifconfig`
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/5.2.png)
   
## Soal 6
 **Membuat Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, dan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit**
 
 - Caranya adalah pada UML **Tuban** disetting pada file `/etc/dhcp/dhcpd.conf` dengan menambahkan `default-lease-time` dan `max-lease-time` sesuai waktu yang diinginkan pada setiap subnet
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/6.1.png)
   
 - Untuk mengetest, pada setiap UML **Client** , restart semua networking dengan perintah `service networking restart`
 # Jarkom_Modul3_Lapres_D06


## Nomor 7

Install squid dan apache utils di Mojokerto

```dotnetcli
apt-get install squid

apt-get install apache2-utils
```

Buat users userta_d06 dengan apache2-utils htpasswd

```dotnetcli
htpasswd -c /etc/squid/passwd userta_d06
```
Ketika htpasswd dijalankan maka akan ada promtp untuk memasukan password. Masukan password untuk userta_d06 yaitu inipassw0rdta_d06

Flag -c untuk mengcreate file hanya dibutuhkan saat pertama kali htpasswd dijalankan.

Kemudian setting squid. Tapi pertama tama back up terlebih dahulu file squid.conf.
```dotnetcli
mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
```
Buka file squid.conf
```dotnetcli
nano /etc/squid/squid.conf
```
Tambahkan konfigurasi berikut pada squid.conf

```
acl all src 0.0.0.0/0.0.0.0
http_port 8080
visible_hostname mojokerto

# Nomor 7
auth_param basic program /usr/lib/squid/ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
# End of Nomor 7
```

## Nomor 8
Buat file untuk konfigurasi waktu disini kita beri nama acl.conf

```dotnetcli
nano /etc/squid/acl.conf
```

Tambahkan akses control untuk waktu

```dotnetcli
acl KERJATA time TW 13:00-18:00
```

Kemudian untuk membuat akses kontrol bekerja tambahkan pada squid.conf line berikut

```dotnetcli
include /etc/squid/acl.conf
http_access allow USERS KERJATA
http_access deny all
```




## Soal 9
 **Membuat proxy hanya dapat diakses pada hari selasa sampai kamis jam 21.00-09.00(besok harinya)**
 
 - Pertama, pada UML **Mojokerto**, ditambahkan setting waktu sebagai berikut pada file `/etc/squid/acl.conf`
   ```
   acl BIMBINGAN time TWH 21:00-23:59
   acl BIMBINGANN time WHF 00:00-09:00
   ```
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/9.1.png)

 - Kedua, pada UML **Mojokerto**, ditambahkan setting sebagai berikut pada file `/etc/squid/squid.conf`
   ```
   http_access allow USERS BIMBINGAN
   http_access allow USERS BIMBINGANN
   ```
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/9.2.png)
   
## Soal 10
 **Membuat apabila mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id**
 
 - Pertama, pada UML **Mojokerto**, tambahkan `google.com` pada file `/etc/squid/ban.acl`
   
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/10.1.png)
 
 - Kedua, pada UML **Mojokerto**, tambahkan config sebagai berikut pada file `/etc/squid/squid.conf`
   ```
   acl RED url_regex "/etc/squid/ban.acl"
   deny_info http://monta.if.its.ac.id/ RED
   http_access deny RED
   ```
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/10.2.png)
   
## Soal 11
 **Mengubah error page default squid**
 
 - Pertama, pada UML **Mojokerto** ditambahkan **error_directory** dengan menambahkan setting `error_directory /usr/share/squid/errors/revisi` pada file `/etc/squid/squid.conf`
 - Kedua, Buat **directory** yang sudah dimasukkan tadi dengan perintah `mkdir /usr/share/squid/errors/revisi`
 - Ketiga, **pindah** ke directory **squid error** dengan perintah `cd /usr/share/squid/errors` dan **copy** semua file dari template ke revisi dengan perintah `cp -r templates/* revisi/`
 - Terakhir, **pindah** ke directory revisi dengan **perintah** cd `revisi/` dan **download** custom error page dengan perintah `wget -N 10.151.36.202/ERR_ACCESS_DENIED`
 - Testing
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/11.1.png)

## Soal 12
 **Membuat apabila ingin menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.d06.pw dan memasukkan port 8080.**
 
 - Pertama, pada UML **Malang** diinstall Bind9 dengan perintah `apt-get install bind9 -y`
 - Kedua, ditambahkan zone baru pada file `/etc/bind/named.conf.local`, dengan menambahkan
   ```
   zone "janganlupa-ta.d06.pw"{
           type master;
           file "/etc/bind/praktikum/janganlupa-ta.d06.pw";
   };
   ```
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/12.1.png)
   
 - Terakhir, buat konfigurasi untuk janganlupa-ta.d06.pw, pada `etc/bind/praktikum//janganlupa-ta.d06.pw`, dengan konfig sebagai berikut:
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/12.2.png)
   
 - Hasil Test:
 
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/12.3.png)
   
   ![p](https://github.com/Raferto/Jarkom_Modul3_Lapres_D06/blob/main/images/12.4.png)
