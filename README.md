# Form Print Out Slip Gaji Pegawai
![Dataset Preview](https://github.com/Arfi3/slip-gaji/blob/main/Screenshot%202026-07-02%20111934.png)


![Dataset Preview](https://github.com/Arfi3/slip-gaji/blob/main/Screenshot%202026-07-02%20111949.png)

Halaman web sederhana untuk mencari data gaji pegawai berdasarkan NIP, menampilkannya dalam format slip gaji resmi, lalu mencetaknya langsung dari browser.

**Live demo:** https://arfi3.github.io/slip-gaji/

## Tampilan

Pengguna memasukkan NIP pada kolom pencarian (lengkap dengan saran otomatis saat mengetik), lalu sistem menampilkan:

- NIP, Nama, Divisi, Posisi, dan Tanggal cetak
- Rincian **Diterima**: Gaji Pokok, Lembur, THR, BPJS Ketenagakerjaan & Kesehatan (porsi perusahaan), Bonus Transport
- Rincian **Dipotong**: BPJS Ketenagakerjaan & Kesehatan (porsi pegawai), Cicilan, PPh21
- Total Penerimaan, Total Potongan, Gaji Bersih, dan **Terbilang** (otomatis dikonversi ke teks)
- Kolom tanda tangan Penerima (otomatis nama pegawai) dan Diserahkan Oleh

## Fitur

- 🔍 Pencarian NIP dengan saran otomatis (autocomplete), mirip combobox filter di Excel
- 🖨️ Tombol Print — mencetak hanya bagian slip, kolom pencarian & tombol otomatis disembunyikan saat mode print
- 🔤 Terbilang otomatis — Gaji Bersih dikonversi ke teks Bahasa Indonesia
- ⚡ Tidak butuh backend atau database — seluruh data dan logika berjalan di sisi klien (browser)
- 📦 Data ditanam langsung (embedded) di dalam file — sekali buka langsung jalan, tanpa koneksi internet

## Tentang Data

Data pada proyek ini adalah data internal perusahaan (700 pegawai) yang diekspor dari `Data_Gaji_Karyawan.xlsx`, mencakup NIP, nama, departemen, level jabatan, dan seluruh komponen gaji (Diterima & Dipotong).

Struktur data mengikuti format berikut:

| No | NIP | Nama Pegawai | Posisi | Divisi | Tahun Masuk | Gaji Pokok | Total Jam Lembur | Lembur | Lama Bekerja | THR | BPJS Ketenagakerjaan (Perusahaan/Pegawai) | BPJS Kesehatan (Perusahaan/Pegawai) | Perjalanan Dinas | Bonus Transport | Cicilan | PPh21 |
|----|-----|--------------|--------|--------|-------------|------------|-------------------|--------|----------------|-----|---------------------------------------------|----------------------------------------|-------------------|-------------------|---------|-------|

> Catatan penamaan kolom: `Posisi` pada data sumber sebenarnya berisi **nama departemen** (misal "Finance"), sedangkan `Divisi` berisi **level jabatan** (misal "Supervisor"). Di tampilan slip, label sudah disesuaikan supaya tidak membingungkan: **Divisi** = departemen, **Posisi** = level jabatan.

**Kode Departemen**

| Kode | Departemen |
|:----:|------------|
| H | Human Resources (HR) |
| M | Marketing |
| S | Sales |
| F | Finance |
| L | Logistics |
| O | Operations |
| I | Information Technology (IT) |

**Kode Level Jabatan**

| Kode | Level |
|:----:|-------|
| P1 | Staff |
| P2 | Senior Staff |
| P3 | Supervisor |
| P4 | Assistant Manager |
| P5 | Manager |

Format NIP: `[Tahun Masuk]-[Kode Departemen]-[Kode Level]-[Nomor Urut]`
Contoh: `2025-H-P1-0001` → masuk tahun 2025, departemen HR (H), level Staff (P1), nomor urut 0001.

Seluruh data asli dapat dilihat pada file `Data_Gaji_Karyawan.xlsx`.

## Ketentuan Perhitungan

| Komponen | Ketentuan |
|---|---|
| Lembur | Rp 100.000 / jam |
| BPJS Ketenagakerjaan — Perusahaan | 3.70% dari Gaji Pokok |
| BPJS Ketenagakerjaan — Pegawai | 2% dari Gaji Pokok |
| BPJS Kesehatan — Perusahaan | 4% dari Gaji Pokok |
| BPJS Kesehatan — Pegawai | 1% dari Gaji Pokok |
| THR — masa kerja ≥ 12 bulan | 1× Gaji Pokok |
| THR — masa kerja < 12 bulan | (masa kerja bulan / 12) × Gaji Pokok |
| Bonus Transport | Rp 200.000 × jumlah perjalanan dinas |

## Logika THR

THR dihitung berdasarkan tanggal cetak dan lama bekerja (rumus dari file Excel):

```
=IF(MONTH(TODAY())=12, IF(J3>=12,G3,G3*(J3/12)), 0)
```

- THR diberikan setiap akhir tahun.
- Pegawai dengan masa kerja ≥ 12 bulan mendapat THR penuh (1× Gaji Pokok).
- Pegawai dengan masa kerja < 12 bulan mendapat THR proporsional (Gaji Pokok × masa kerja/12).

Nilai `thr` pada data JS bersifat statis (hasil perhitungan Excel saat data diekspor), bukan dihitung ulang secara real-time oleh halaman web.

## Tech Stack

- HTML5, CSS3 (custom, tanpa framework/template generik)
- Vanilla JavaScript (tanpa library eksternal)
- Data ditanam langsung di dalam file — tidak memerlukan server atau koneksi internet untuk berjalan

## Menjalankan Secara Lokal

1. Clone atau download repository ini
2. Buka file `slip-gaji.html` langsung di browser (double-click), tidak perlu instalasi apa pun

## Deploy ke GitHub Pages

1. Pastikan file HTML berada di root repository (bisa direname jadi `index.html` agar otomatis terbuka di root URL)
2. Buka **Settings → Pages**
3. Pilih branch `main` dan folder `/ (root)` sebagai source
4. Simpan — halaman akan aktif di `https://<username>.github.io/<nama-repo>/` dalam 1–2 menit

## Catatan

Proyek ini dikembangkan dengan bantuan AI (Claude, Anthropic) — mulai dari perancangan UI, penulisan kode, konversi data dari Excel, hingga penyesuaian struktur dan logika sesuai kebutuhan.

## Lisensi

Bebas digunakan dan dimodifikasi untuk keperluan internal perusahaan.
