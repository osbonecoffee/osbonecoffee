# Os Bone Coffee — versi terintegrasi & diperbaiki

## File
- `1-Website-Utama.html` — template utama; area kanan header dan kartu "Pesan langsung melalui website" dihapus, logo responsif diperbesar, harga menu tidak ditampilkan.
- `2-Website-Builder.html` — semua teks utama dapat diedit, detail menu/badge/gambar dapat diedit, logo dapat diganti, navigasi dapat diedit, serta kontrol tata letak dasar (lebar kontainer, ukuran logo, tinggi header, jarak section, jumlah kolom menu, radius card).
- `3-Website-Pemesanan-QR.html` — nomor meja dihapus, pilihan Dine In/Take Away dipindahkan ke tahap konfirmasi sebelum pesanan dikirim, opsi Hot/Ice dan gula dibuat ringkas/profesional, tombol kembali ke beranda dihapus.
- `4-Sistem-Kasir.html` — pembayaran tunai memakai popup nominal diterima + kembalian real-time, diskon persen/nominal, riwayat selalu tampil di sisi kanan dan bisa dibuka untuk detail, tombol batal dihilangkan, jumlah item terjual dan ringkasan menu terjual ditampilkan, pencarian tidak lagi kehilangan fokus setiap satu karakter, pesanan manual tetap tersedia.
- `5-Google-Apps-Script.gs` — API Google Sheets + endpoint website utama online.
- `shared.js` — sinkronisasi/fallback localStorage dan normalisasi data lama.
- `app-config.js` — konfigurasi URL Web App Apps Script.

## Setup online
1. Buat Google Spreadsheet.
2. Extensions → Apps Script.
3. Tempel isi `5-Google-Apps-Script.gs`.
4. Deploy → New deployment → Web app.
5. Execute as: Me.
6. Who has access: Anyone.
7. Salin URL `/exec`.
8. Buka `2-Website-Builder.html` → Umum & Teks → Google Apps Script Web App URL → tempel URL → Simpan.
9. Setelah berhasil tersimpan, status Builder harus menunjukkan **Tersimpan & online**.
10. URL `/exec` Apps Script juga dapat dibuka sebagai **website utama online**. Jika Content belum pernah disimpan, buka Builder dan Simpan sekali terlebih dahulu.

## Catatan sinkronisasi
Semua halaman mencoba membaca/menulis Google Sheets melalui URL Web App. Jika URL belum diisi atau deployment gagal, aplikasi tetap bekerja memakai localStorage perangkat. Builder sekarang membedakan kondisi **offline**, **online**, dan **sinkron online gagal**, sehingga pesan tidak lagi menyesatkan.

## Struktur Google Sheets
`Orders` menyimpan:
- identitas pesanan, waktu, tipe Dine In/Take Away
- item JSON
- status pembayaran
- uang diterima dan kembalian
- diskon tipe/nilai/potongan
- total bayar

`Content` menyimpan satu record `osbone_content` berisi seluruh konfigurasi website/builder.

## Pemeriksaan bug
Sebelum paket dibuat, seluruh JavaScript pada 4 halaman HTML telah diperiksa dengan syntax checker Node.js dan tidak ditemukan error sintaks. Apps Script juga ditulis dengan pemetaan kolom yang konsisten untuk diskon dan pembayaran.
