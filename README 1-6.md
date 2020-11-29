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
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.2.png)
  
  - Ketiga, pada **setiap** UML dibuatkan interface baru pada file `/etc/network/interfaces` sebagai berikut:
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.3.png)
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.4.png)
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.5.png)
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.6.png)
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.7.png)
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.8.png)
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.9.png)
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.10.png)
  
  - Keempat, pada UML **Surabaya** dibuatkan file `iptables.sh`, sebagai berikut:
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.11.png)
  
  - Kelima, jalankan **iptables.sh** yang baru saja dibuat dengan `bash iptables.sh`
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.12.png)
  
  - Terakhir, pada **setiap** UML dibuatkan file `export.sh` sebagai berikut, dan dijalankan dengan ` . export.sh `
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/1.13.png)
  
## Soal 2
 **Membuat Surabaya menjadi sebagai perantara (DHCP Relay) antara DHCP Server dan client**
 
 - Pertama, pada UML **Tuban** diinstall **DHCP Server** denggan perintah `apt-get install isc-dhcp-server`
 - Kedua, setelah selesai proses instalasi **DHCP Server** disetting file `/etc/default/isc-dhcp-server`
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.1.png)
 
 - Ketiga, pada UML **Surabaya** diinstall **DHCP Relay** denggan perintah `apt-get install isc-dhcp-relay`
 - Keempat, setelah selesai proses instalasi **DHCP Relay** disetting file `/etc/default/isc-dhcp-relay`, dengan dimasukkannya IP Tuban dan interface yang digunakan
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.2.png)
  
 - Kelima, pada UML **Tuban**, ditambahkan **subnet NID_DMZ** pada file `/etc/dhcp/dhcp.conf`, seperti berikut:
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.3.png)
 
 - Keenam, pada setiap UML **Client**, dirubah semua interface yang static menjadi menggunakan **DHCP**, seperti berikut:
  
  ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.4.png)
  
 - Berikut ini adalah langkah **OPTIONAL**, apabila ingin membuat server merequest ip menggunakan *DHCP*, 
   - Pertama pada UML **Server**, cari **hardware access** dengan perintah `ifconfig` dan melihat pada bagian **HWaddr**
   
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.5.png)
   
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.6.png)
   
  - Kedua pada setiap UML **Server**, ditambahkan setting **interface** pada file `/etc/network/interfaces` sesuai **hardware address** masing-masing, sebagai berikut:
   
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.7.png)
   
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.8.png)
   
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/2.9.png)
   
  - Terakhir pada UML **Tuban**, di-Restart **DHCP Server** dengan perintah `service isc-dhcp-server restart`

## Soal 3
 **Membuat Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200**
 
 - Caranya adalah pada UML **Tuban** disetting pada file `/etc/dhcp/dhcpd.conf` dengan menambah range yang sesuai untuk semua Client dengan subnet 1.
 
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/3.1.png)
   
 - Untuk mengetest, pada UML Client dengan subnet 1, yaitu **Gresik** dan **Sidoarjo**, jalankan perintah `ifconfig`
 
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/3.2.png)
   
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/3.3.png)
 
## Soal 4
 **Membuat Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70**
 
 - Caranya adalah pada UML **Tuban** disetting pada file `/etc/dhcp/dhcpd.conf` dengan menambah range yang sesuai untuk semua Client dengan subnet 3.
 
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/4.1.png)

 - Untuk mengetest, pada UML Client dengan subnet 3, yaitu **Banyuwangi** dan **Madiun**, jalankan perintah `ifconfig`
 
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/3.2.png)
   
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/3.3.png)
   
## Soal 5
 **Membuat Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP**
 
 - Caranya adalah pada UML **Tuban** disetting pada file `/etc/dhcp/dhcpd.conf` dengan menambahkan `option domain-name-serves` sesuai rangenya pada subnet 1 dan 3
 
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/5.1.png)
   
 - Untuk mengetest, pada setiap UML **Client** , jalankan perintah `ifconfig`
 
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/5.2.png)
   
## Soal 6
 **Membuat Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, dan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit**
 
 - Caranya adalah pada UML **Tuban** disetting pada file `/etc/dhcp/dhcpd.conf` dengan menambahkan `default-lease-time` dan `max-lease-time` sesuai waktu yang diinginkan pada setiap subnet
 
   ![p](https://github.com/Raferto/Jarkom_Modul2_Lapres_D06/blob/main/images/6.1.png)
   
 - Untuk mengetest, pada setiap UML **Client** , restart semua networking dengan perintah `service networking restart`
