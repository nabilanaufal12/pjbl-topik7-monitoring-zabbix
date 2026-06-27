# Sistem Monitoring Jaringan Berbasis Zabbix

Repositori ini berisi dokumentasi dan file konfigurasi untuk Project Based Learning (PjBL) mata kuliah Jaringan Komputer, Program Studi Teknik Informatika, Universitas Maritim Raja Ali Haji (UMRAH).

**Kelompok 4:**
- Jian Safitri Wibowo (2401020056)
- Nabil (2401020073)
- Kevin Brian Rizky (2401020085)
- Devanovita Chelsea Prasojo (2401020086)
- Annisa Darmawan (2401020087)

**Dosen Pengampu:**
- Ferdi Chahyadi, S.Kom., M.Cs
- Feri Irawan, S.Kom., M.Kom

---

## Akses File Lingkungan GNS3
Dikarenakan batasan ukuran file pada GitHub (file proyek dan *image* sistem operasi berukuran lebih dari 1.9 GB), maka file `.gns3project` beserta konfigurasi mesin virtual disimpan di Google Drive. 

**Tautan Unduhan GNS3:** [Google Drive - Proyek Zabbix Kelompok 4]([https://drive.google.com/drive/folders/1ObpODHjbXSI5ga0m7t3cz_pyRVc9lQPK](https://drive.google.com/drive/folders/1MTL6OBLVvUENw33q-a4CJvKmM-0IU-jg?usp=sharing))

---

## Deskripsi Proyek
Proyek ini bertujuan untuk merancang, membangun, dan menguji coba infrastruktur jaringan multi-subnet di dalam simulator GNS3, yang kemudian diintegrasikan dengan sistem monitoring **Zabbix**. Pengawasan (monitoring) mencakup analisis performa *Router* menggunakan protokol SNMP dan pemantauan ketersediaan *End-Device* (Client) menggunakan metode ICMP Ping.

## Arsitektur dan Topologi Jaringan
Jaringan dibagi menjadi tiga segmen utama yang saling terhubung menggunakan teknik *Static Routing*:
1. **Subnet Server (192.168.10.0/24)**: Berfungsi sebagai *management network* tempat beroperasinya Ubuntu Server (IP: 192.168.10.10) yang menjalankan sistem Zabbix.
2. **Subnet Client (192.168.20.0/24)**: Berfungsi sebagai lingkungan simulasi pengguna akhir dengan dua PC Virtual (Client1: 192.168.20.10 & Client2: 192.168.20.20).
3. **Link Point-to-Point (10.10.10.0/30)**: Jalur interkoneksi langsung antara Router R1 dan Router R2.

## Fitur Utama Sistem
1. **Aksesibilitas Eksternal via NAT**: Router R1 dikonfigurasi menggunakan NAT dan *Port Forwarding* untuk menjembatani jaringan virtual GNS3 dengan perangkat *host* fisik. Hal ini memungkinkan antarmuka *web* Zabbix diakses langsung melalui peramban pada mesin fisik.
2. **Monitoring SNMP (Simple Network Management Protocol)**: Sistem menggunakan protokol SNMPv2c untuk menarik data utilisasi CPU, trafik jaringan (*inbound/outbound*), dan status operasional dari Router Cisco c7200 secara *real-time*.
3. **Monitoring ICMP Ping**: Melakukan *polling* data ICMP setiap 60 detik untuk memantau ketersediaan, waktu respons (*latency*), dan persentase kehilangan paket (*packet loss*) pada PC Client (VPCS).
4. **Sistem Peringatan (Alerting) Otomatis**: Integrasi *trigger* pada Zabbix yang dikonfigurasi untuk secara otomatis mendeteksi anomali jaringan dan menampilkan peringatan saat perangkat klien tidak dapat dijangkau (*unreachable*).

## Panduan Penggunaan
Untuk menjalankan simulasi ini secara lokal, ikuti langkah-langkah berikut:

1. **Persiapan Simulator**:
   - Unduh seluruh direktori dari tautan Google Drive yang disediakan di atas.
   - Buka file proyek `.gns3project` menggunakan aplikasi GNS3.
   - Klik tombol **Start All Nodes** dan tunggu hingga Ubuntu Server selesai melakukan proses *booting*.

2. **Verifikasi Konektivitas Jaringan**:
   - Buka *console* pada `Client1`.
   - Lakukan uji coba konektivitas lintas subnet dengan menjalankan perintah: `ping 192.168.10.10`.
   - Pastikan balasan paket berstatus *reply*.

3. **Akses Dashboard Zabbix**:
   - Buka *web browser* pada mesin fisik (*host*).
   - Akses URL hasil translasi *Port Forwarding*: `http://192.168.122.87:8080/zabbix/`
   - Masuk menggunakan kredensial standar (Username: `Admin` | Password: `zabbix`).

4. **Navigasi Monitoring**:
   - Menu **Monitoring -> Hosts**: Memverifikasi ketersediaan agen SNMP pada Router R1 dan R2.
   - Menu **Monitoring -> Graphs**: Menganalisis visualisasi grafik utilisasi CPU dan lalu lintas *interface* Router.
   - Menu **Monitoring -> Latest Data**: Memantau hasil *polling* ICMP Ping dari perangkat Client secara *real-time*.
