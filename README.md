# Vlog Sat-set Install Framework Frappe dengan Docker

[![DevOps Komodo](https://img.youtube.com/vi/kdjChBZIMyQ/0.jpg)](https://www.youtube.com/watch?v=kdjChBZIMyQ)

Dokumen ini digunakan sebagai pendukung video yang diterbitkan di channel Orang_IT. 

Dokumen ini menjelaskan cara install framework frappe dengan docker. Framework Frappe bisa diinstall langsung di Host tanpa memerlukan Docker. Tapi bahasan tersebut di luar cakupan dokumen ini.

Docker bisa berjalan di sistem operasi Linux dan MacOS. Untuk pengguna MS Windows bisa menggunakan Docker Desktop (via WSL2).

Selain langsung di-install di host, kita bisa juga menginstall Docker di Virtual Machine, misalnya dengan VirtualBox, VMWare, Proxmox, atau Multipass.

Dalam video saya menggunakan MacOS dan menginstall docker & frappe di VM Multipass supaya tidak mengganggu host. Dan sewaktu-waktu bisa di-copy/backup atau dihapus dengan mudah.

Namun cara instalasi di host apa saja bisa menggunakan cara ini asal sudah menggunakan docker.

## Tetapi Mengapa Panduan Ini Dibuat?

Dokumentasi instalasi framework frappe & aplikasinya sudah ada di website resminya & cukup detail. Tapi ada beberapa alasan penting mengapa dokumen ini perlu dibuat:

1. Menyediakan panduan dalam bahasa Indonesia sehingga mudah dipahami & diikuti. Panduan disajikan dengan runut sehingga mengurangi kemungkinan step yang terlewat.
2. Ada beberapa bugs di dokumentasi resminya, dan dokumen ini mencoba untuk mengkoreksinya sehingga tingkat keberhasilan instalasi lebih tinggi.
3. Ada beberapa tambahan detail sehingga dapat mempermudah instalasi dan akses.
4. Ada beberapa tips/trick sehingga dapat membantu Anda, sekarang dan kelak.

## Cara Install Docker

Untuk instalasi docker di berbagai sistem operasi, silakan kunjungi website resminya di: https://docs.docker.com/engine/install/

Jika Anda menggunakan OS Linux (atau dari VM Linux) atau dari WSL2 (Ubuntu/Debian), maka bisa menggunakan cara praktis berikut:

```
sudo snap install docker
```

Setelah itu daftarkan user yang Anda pakai ke group docker. Berikut perintahnya:
```
sudo adduser namauser docker
```
Ubah 'namauser' dengan user yg Anda gunakan.

Keluar atau logout, kemudian masuk lagi ke terminal supaya group docker aktif untuk user Anda.

## Cara Install Framework Frappe

Masuk ke host dan clone proyek frappe_docker. Berikut perintahnya:

```
git clone https://github.com/frappe/frappe_docker.git frappe
```

Setelah itu masuk ke folder frappe dengan perintah berikut:

```
cd frappe
```

Di sinilah basis instalasi framework Frappe.

Sebenarnya ada script python bernama ```easy-install.py```, namun pada host dengan arsitektur ARM64 atau Apple Silicon mungkin akan gagal. Jadi kita tidak menggunakannya, karena sebenarnya ada cara yang lebih praktis.

## Custom pwd.yml

Ada file bernama ```pwd.yml``` yang bisa kita jalankan langsung. Tapi kita akan melakukan sedikit kustomisasi.

Kustomisasi ini diperlukan supaya kita bisa mengakses folder sites & database mariadb untuk bisa kita pergunakan kelak di kemudian hari.

Edit file ```pwd.yml``` dan temukan bagian ```volumes```. Berikut adalah bagian tersebut orisinalnya:

```
...

volumes:
  db-data:
  redis-queue-data:
  sites:
  logs:

...
```

Kita ubah menjadi:

```
...

volumes:
  db-data:
    driver: local
    driver_opts:
      o: bind
      type: none
      device: ./_db_data
  redis-queue-data:
  sites:
    driver: local
    driver_opts:
      o: bind
      type: none
      device: ./_sites
  logs:

...
```

Sebelum menjalankan docker-compose, kita perlu membuat folder ```_db_data``` dan ```_sites```. Berikut perintahnya:

```
mkdir _db_data
mkdir _sites
```

## Docker Compose

Saatnya kita menjalankannya! Jalankan perintah berikut ini:

```
docker compose -f pwd.yml up
```

Oh iya, jika ingin supaya instance baru ini tidak menggunakan terminal, maka bisa menambahkan argumen ```-d``` sehingga bisa langsung detach (atau meninggalkan terminal dan berjalan di background).

Untuk pengguna macOS mungkin memerlukan sudo di awal perintah.

Tunggu prosesnya, mungkin akan memerlukan beberapa menit, karena akan mendownload beberapa image (backend, frontend, reddis, mariadb, traefik, configurator, dll).

Konfigurasinya juga butuh waktu beberapa menit. Tunggu sampai ada pesan log yang menuliskan kalau sistem sudah LISTEN 0.0.0.0:8080. 

Jika sudah muncul, maka Anda bisa mengaksesnya di IP Host/VM yang menjalankan docker dengan port 8080.

Untuk mengetahui IP-nya, buka terminal baru di host/VM dan jalankan perintah berikut:

```
hostname -I
```

Jika menggunakan multipass, maka bisa mendapatkan IP dari VM dengan menjalankan perintah berikut di host-nya:

```
multipass info
```

## Akses dari Browser

Buka browser dan akses ke IP dan port dari install framework frappe. Dalam contoh saya menggunakan IP multipass di 192.168.64.23. Jadi berikut adalah URL-nya:

```
https://192.168.64.23:8080
```

Untuk login bisa menggunakan username ```Administrator``` dan password default ```admin```.

Nanti akan ada prompt untuk membuat user & password baru.

# Xenara Cafe and Coworking Space

Vlog ini disponsori oleh Xenara Cafe and Coworking Space.

Alamat:

Ruko Citra Grand, Blok London C-08, Semarang, Jawa Tengah, Indonesia
