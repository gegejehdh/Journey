# Journey Inspera — Portal Pers Sekolah (Firebase)
Dua situs, satu backend: `public/` (portal berita) dan `admin/` (dashboard). Keduanya memakai Auth + Firestore + Storage yang sama.

## Setup (sekali saja)
1. console.firebase.google.com → buat proyek → aktifkan **Authentication (Email/Password)**, **Firestore**, **Storage**.
2. Project settings → tambah Web app → salin config ke `public/config.js` dan `admin/config.js`.
3. `npm i -g firebase-tools && firebase login && firebase use --add`
4. Dua situs hosting: `firebase hosting:sites:create NAMA-publik` dan `...NAMA-admin`, lalu
   `firebase target:apply hosting public NAMA-publik` dan `firebase target:apply hosting admin NAMA-admin`.
5. `firebase deploy` (menerapkan rules, index, dan kedua situs). Domain utama: sambungkan ke situs publik; subdomain `admin.` ke situs admin (Hosting → Add custom domain).
6. **Super Admin pertama**: Authentication → Add user (email+password). Salin UID-nya, lalu di Firestore buat dokumen `users/<UID>` dengan field `name`, `email`, `role` = `superadmin`. Setelah itu semua akun lain dibuat dari menu *Pengurus*.
7. Login admin → Kategori → "Isi kategori awal".

## Cara kerja publikasi
Status `draft` tidak terbaca publik (dijaga Security Rules). `published` + `publishAt` di masa depan = terjadwal; baru terbaca setelah waktunya tiba (publik memuat ulang tiap 5 menit). Edit/hapus/batalkan langsung tercermin lewat listener real-time.
