# Os Bone Coffee — GitHub Pages + Google Sheets

Paket ini mempertahankan template/desain aplikasi yang sudah ada. GitHub Pages dipakai sebagai hosting frontend, sedangkan Google Sheets + Google Apps Script dipakai sebagai database/API.

## Struktur

- `index.html` — halaman utama GitHub Pages (salinan dari template utama).
- `1-Website-Utama.html` — template utama asli, dipertahankan.
- `2-Website-Builder.html` — editor konten.
- `3-Website-Pemesanan-QR.html` — pemesanan pelanggan.
- `4-Sistem-Kasir.html` — sistem kasir.
- `5-Google-Apps-Script.gs` — API Google Sheets.
- `shared.js` — fungsi bersama dan fallback localStorage.
- `app-config.js` — URL Web App Apps Script.
- `.nojekyll` — agar GitHub Pages tidak menjalankan pemrosesan Jekyll.

## 1. Google Sheets

1. Buat Google Spreadsheet baru.
2. Buka **Extensions → Apps Script**.
3. Hapus kode contoh.
4. Salin seluruh isi `5-Google-Apps-Script.gs` ke Apps Script.
5. Simpan.
6. Jalankan fungsi `setup` sekali dari editor Apps Script jika diminta otorisasi.
7. Pastikan sheet `Orders` dan `Content` dibuat.

## 2. Deploy Apps Script sebagai Web App

Di Apps Script:

1. **Deploy → New deployment**.
2. Pilih **Web app**.
3. **Execute as:** Me.
4. **Who has access:** Anyone.
5. Klik **Deploy**.
6. Salin URL Web App yang berakhiran `/exec`.

Jangan menggunakan URL `/dev` untuk website produksi.

## 3. Hubungkan website

Ada dua cara. Cara yang direkomendasikan untuk GitHub Pages adalah mengisi `app-config.js` sebelum upload:

```js
window.APP_SCRIPT_URL = 'https://script.google.com/macros/s/XXXXXXXXXXXX/exec';
```

Ganti URL contoh dengan URL Web App Anda.

Alternatifnya, buka `2-Website-Builder.html` setelah website online, masukkan URL yang sama pada **Umum & Teks → Google Apps Script Web App URL**, lalu klik **Simpan**. Nilai tersebut juga disimpan di Google Sheets dan akan dibaca halaman lain.

## 4. Upload ke GitHub

Buat repository baru, misalnya `os-bone-coffee`, lalu upload seluruh isi folder paket ini ke branch `main`.

Pastikan `index.html` berada di root repository.

## 5. Aktifkan GitHub Pages

Di repository GitHub:

**Settings → Pages → Build and deployment**

- Source: **Deploy from a branch**
- Branch: **main**
- Folder: **/ (root)**

Klik **Save**.

Alamat website akan menjadi kira-kira:

`https://USERNAME.github.io/os-bone-coffee/`

## 6. Urutan pengujian

1. Buka website GitHub Pages.
2. Buka `2-Website-Builder.html` melalui URL GitHub Pages.
3. Pastikan status sinkronisasi menunjukkan online setelah URL Apps Script tersedia.
4. Klik **Simpan** sekali untuk membuat record `osbone_content` di sheet `Content`.
5. Buka halaman pemesanan dan buat satu pesanan percobaan.
6. Buka kasir dan pastikan pesanan muncul.
7. Uji pembayaran tunai, diskon, dan status transaksi.
8. Periksa sheet `Orders` untuk memastikan data masuk.

## Catatan keamanan

- Jangan menaruh URL spreadsheet atau kredensial Google di frontend.
- Yang dibutuhkan frontend hanya URL Web App Apps Script.
- Google Apps Script menjalankan akses spreadsheet di sisi server.
- URL Web App bersifat publik. Untuk produksi dengan data sensitif, tambahkan autentikasi/otorisasi pada Apps Script sebelum digunakan secara luas.
- GitHub Pages hanya meng-host file frontend; data transaksi tetap berada di Google Sheets.
