# Vlog Sat-set Install Framework Frappe dengan Docker

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

Sebenarnya ada script python bernama ~easy-install.py~, namun pada host dengan arsitektur ARM mungkin akan gagal. Jadi kita tidak menggunakannya, karena sebenarnya ada cara yang lebih praktis.

## Custom pwd.yml

Ada file bernama 
