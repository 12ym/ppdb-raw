# Panduan Menyambungkan Backend Gratis (Firebase) — RA Al Washliyah PPDB

Tanpa langkah ini, situs tetap bisa dipakai untuk **lihat tampilan & alur** (cocok untuk demo cepat ke klien),
tapi data formulir, cek status, dan panel admin **tidak akan tersimpan permanen** begitu di-hosting di luar Claude.

Ikuti langkah ini sekali saja (sekitar 10–15 menit) supaya situs **beneran menyimpan data**, gratis, dan jalan di hosting mana pun (Netlify, Cloudflare Pages, dll).

---

## 1. Buat Project Firebase

1. Buka **https://console.firebase.google.com** (login pakai akun Google/Gmail apa saja).
2. Klik **"Add project"** / **"Tambahkan project"**.
3. Beri nama, misalnya `ra-alwashliyah-ppdb` → Lanjut.
4. Saat ditanya Google Analytics, boleh **dimatikan saja** (tidak perlu untuk kebutuhan ini) → **Create project**.
5. Tunggu sebentar sampai project selesai dibuat → klik **Continue**.

## 2. Aktifkan Firestore Database

1. Di menu sebelah kiri, klik **Build → Firestore Database**.
2. Klik **Create database**.
3. Pilih lokasi server terdekat, misalnya **asia-southeast2 (Jakarta)**.
4. Pilih mode **Start in production mode** → Next → Enable.

## 3. Pasang Aturan Keamanan (Security Rules)

1. Di halaman Firestore, buka tab **Rules**.
2. Hapus semua isi yang ada, lalu **copy-paste** isi file `firestore.rules` (satu folder dengan panduan ini) ke sana.
3. Klik **Publish**.

> Catatan jujur soal keamanan: rules ini membuat data pendaftar bisa **dibaca** oleh siapa pun yang tahu nomor pendaftarannya (dipakai fitur Cek Status — sama seperti cek resi), dan **diupdate** oleh siapa saja yang tahu cara memanggil Firestore langsung (bukan cuma lewat tombol Panel Admin di situs). Untuk demo dan tahap awal ini wajar dan cukup. Kalau sudah live dengan data sungguhan dalam jumlah besar, sebaiknya tambahkan **Firebase Authentication** khusus untuk admin — bisa saya bantu nanti kalau sudah siap ke tahap itu.

## 4. Ambil Kode Konfigurasi (Firebase Config)

1. Klik ikon **gerigi (⚙️)** di sebelah "Project Overview" → **Project settings**.
2. Scroll ke bagian **"Your apps"** → klik ikon web **`</>`**.
3. Beri nama app, misalnya `ppdb-web` → **Register app** (tidak perlu centang Firebase Hosting).
4. Akan muncul kode seperti ini:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "ra-alwashliyah-ppdb.firebaseapp.com",
     projectId: "ra-alwashliyah-ppdb",
     storageBucket: "ra-alwashliyah-ppdb.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
5. **Copy semua nilainya.**

## 5. Tempel ke `index.html`

1. Buka file `index.html` pakai text editor apa saja (Notepad, VS Code, dll).
2. Cari tulisan **`KONFIGURASI FIREBASE — GANTI BAGIAN INI`** (gunakan Ctrl+F / Cmd+F).
3. Ganti tiap nilai `"GANTI_..."` dengan nilai asli dari langkah 4. Contoh sebelum/sesudah:
   ```js
   // Sebelum
   apiKey: "GANTI_DENGAN_API_KEY_ANDA",
   // Sesudah
   apiKey: "AIzaSy...isi_asli_anda",
   ```
4. Simpan file-nya.

## 6. Upload Ulang ke Hosting

Upload ulang seluruh folder (yang sudah berisi `index.html` terbaru) ke Netlify / hosting kakak — bisa drag & drop lagi seperti sebelumnya.

## 7. Cek Hasilnya

1. Buka situsnya, isi formulir pendaftaran sampai selesai.
2. Buka menu **Cek Status Pendaftaran**, masukkan nomor pendaftaran & no. WA yang tadi dipakai → harus muncul datanya.
3. Buka **Panel Admin Sekolah** (link kecil di footer) → masukkan kode akses → lihat label kecil di atas tabel:
   - 🟢 **"Tersambung ke Firestore (data permanen)"** → berhasil!
   - 🔴 **"Mode Demo Claude (belum tersambung Firestore)"** → config belum terisi dengan benar, ulangi langkah 5.

---

## Soal Biaya

Firebase **gratis** untuk skala sekolah kecil-menengah (paket "Spark"). Batas gratisnya per hari kurang lebih:
50.000 pembacaan data, 20.000 penulisan data — jauh lebih dari cukup untuk PPDB tahunan. Tidak perlu kartu kredit untuk paket gratis ini.

## Jangan Lupa

- Ganti `ADMIN_PASS` di `index.html` (cari kata `ADMIN_PASS`) dengan kode akses baru sebelum situs dipakai sungguhan — jangan pakai kode default lagi.
