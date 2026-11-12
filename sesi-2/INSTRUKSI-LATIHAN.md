# Sesi 2 — Latihan: Dari Excel ke Alat Rekap (Input dan Angka Kontrol)

**Estimasi waktu:** 90–120 menit  **Tingkat:** pemula (tanpa pengalaman coding)  **Alat:** VS Code (atau editor teks lain), Chrome/Edge, Excel

---

## 1. Apa yang kamu kerjakan hari ini

Alat rekap yang kamu bangun **tidak membuka file `.xlsx`**. Kamu menyalin (Copy) data dari Excel, menempelkannya (Paste) ke kotak di halaman, lalu kode JavaScript memecah teks itu menjadi baris dan kolom, membersihkan angkanya, dan menampilkan **angka kontrol**: jumlah baris dan total Nilai Sisa Stok.

Di akhir sesi ini, alat kamu harus bisa:

1. membaca hasil paste dari Excel (kolom dipisah Tab, baris dipisah Enter),
2. mengubah teks seperti `1.250.000`, `1,5`, dan `(500)` menjadi angka,
3. mencari baris judul dan kolom berdasarkan **nama judulnya** (bukan urutan kolom),
4. memberi **peringatan** (bukan error) saat ada angka yang tidak terbaca,
5. menghasilkan angka kontrol yang **sama persis** dengan hitungan Excel.

> **Aturan emas:** hasil alat **harus dibuktikan** dengan Excel. Tidak cocok berarti ada yang salah dan harus dicari sebabnya.

---

## 2. Isi folder `sesi-2`

| File / folder | Fungsinya |
|---|---|
| `INSTRUKSI-LATIHAN.md` | Dokumen yang sedang kamu baca. |
| `starter/rekap.html` | **Titik awal**: hasil akhir Sesi 1. Kerangka halaman sudah ada, kode Sesi 2 belum. |
| `data/dummy-gudang-100-baris.xlsx` | Data dummy 100 baris dalam format Excel (angka asli). |
| `data/dummy-gudang-100-baris.tsv` | Data yang sama dalam bentuk teks siap paste (tanpa Excel). |
| `data/dummy-gudang-20-baris.tsv` | 20 baris pertama saja, untuk percobaan awal. |
| `data/dummy-gudang-judul-bergeser.tsv` | Data yang sama, tetapi ada judul laporan di atas tabel (untuk Tantangan). |
| `data/data-contoh-8-baris.tsv` | Data contoh kecil (8 baris) yang dipakai tombol **Isi data contoh**. |
| `hasil/` | **Tempat kamu menyimpan pekerjaanmu** (`rekap.html` dan `catatan-sesi-2.md`). |

Kode untuk **Sesi 2A** dan **Sesi 2B** ada di materi `materi-alat-rekap_v3.html` (dari mentor). Salin dari sana, **sambil membaca penjelasan per barisnya**.

> ⚠️ **Semua data di folder ini adalah data karangan.** Jangan pernah mengunggah data asli perusahaan ke GitHub.

---

## 3. Kamus data (16 kolom)

Judul kolom di Excel dicari **menurut namanya**, jadi urutan kolom boleh bergeser.

| Kolom | Judul | Contoh | Keterangan |
|---|---|---|---|
| A | Tgl Opname | `46304` | Tanggal opname sebagai nomor seri Excel (46304 = 09/10/2026). |
| B | Kode Batch | `B-24017` | Kode unik tiap batch. Dipakai menghitung **jumlah baris**. |
| C | Nama Produk | `Teh Melati 350 ml` | |
| D | Kode Gudang | `G01` | |
| E | Nama Gudang | `Gudang Kenanga` | |
| F | Area | `Area Timur` | |
| G | Nama PIC | `Andi` | Penanggung jawab batch. |
| H | Nilai Sisa Stok (Rp) | `12.500.000` | Dipakai menghitung **total**. Bisa berbentuk `1.250.000`, `1,5`, atau `(500)` (negatif). |
| I | Target Keluar per Minggu (Rp) | `850.000` | |
| J | Minggu Ke | `4` | |
| K | Ket Kedaluwarsa | `Belum Lewat ED` / `Sudah Lewat ED` | |
| L | Ket Pengeluaran | `Keluar < 50% Target` / `Keluar ≥ 50% Target` | |
| M | Kategori Produk | `Minuman` | |
| N | Saluran Jual | `Grosir` | |
| O | Ket Klaim Retur | `Ada Klaim` / `Tdk Ada Klaim` | |
| P | Nominal Klaim (Rp) | `150.000` | |

Data dummy 100 baris **sengaja memuat beberapa “kasus aneh”** seperti yang sering ada di data sungguhan. Salah satu tugasmu adalah **menemukannya sendiri** (Tugas 7).

---

## 4. Persiapan

### 4.1 Ambil folder ini

Pilih salah satu:

- **Dengan Git:**
  ```bash
  git clone https://github.com/<akun-mentor>/bangun-alat-rekap.git
  cd bangun-alat-rekap
  git checkout -b sesi-2-<namamu>
  ```
- **Tanpa Git:** di halaman GitHub klik **Code → Download ZIP**, lalu ekstrak.

### 4.2 Buka alat bantu

- Buka folder `bangun-alat-rekap` di **VS Code** (File → Open Folder).
- Siapkan **Chrome atau Edge**. Tekan **F12** lalu pilih tab **Console**. Di sinilah kamu melihat pesan kesalahan dan mengetik perintah uji.
- Buka materi `materi-alat-rekap_v3.html` di tab browser terpisah (bagian **Sesi 2**).

### 4.3 Cek format angka di Excel kamu (penting)

Buka `data/dummy-gudang-100-baris.xlsx`, blok **H2:H6**, Copy, lalu Paste ke Notepad.

- Terbaca seperti `12.500.000` (titik): **aman**, lanjutkan.
- Terbaca seperti `12,500,000` (koma): pengaturan wilayah Excel/Windows kamu bukan Indonesia. **Pakai file `.tsv`** sebagai gantinya (buka di VS Code, Ctrl+A, Ctrl+C), atau ubah pengaturan wilayah ke Indonesia.

---

## 5. Tugas

Kerjakan **berurutan**. Setiap tugas punya **“Selesai bila”**; jangan lanjut sebelum terpenuhi.

### Tugas 1 — Siapkan titik awal

1. Salin `starter/rekap.html` menjadi `hasil/rekap.html`. **Kerjakan di salinan ini**, jangan di `starter`.
2. Buka `hasil/rekap.html` dengan klik dua kali (akan terbuka di browser). Tekan F12 → Console.
3. Di VS Code, cari baris:
   ```
   // ====== SESI 2, 3, 4 DITEMPEL DI BAWAH GARIS INI ======
   ```
   Kode yang kamu tulis nanti diletakkan **di bawah baris ini**, masih di dalam `<script>`.

**Selesai bila:** halaman tampil (kartu “1. Tempel data”), dan Console **tidak** menampilkan tulisan merah. Klik **Proses** sekarang: belum terjadi apa-apa, itu normal.

### Tugas 2 — Tempel dan pahami kode 2A (input)

1. Dari materi **Sesi 2A**, salin kode ke bawah baris penanda.
2. Baca **“Penjelasan per baris”** di materi sambil menunjuk barisnya di VS Code. Jangan hanya menempel.
3. Simpan (**Ctrl+S**), lalu muat ulang browser (**F5**).

**Selesai bila:** Console tetap tanpa tulisan merah setelah F5.

### Tugas 3 — Uji fungsi satu per satu di Console

Ketik di Console, tekan Enter setelah tiap baris, dan **catat hasilnya di `catatan-sesi-2.md`**:

```js
pecahTSV('a\tb\n1\t2\n')
keAngka('1.250.000')
keAngka('1,5')
keAngka('(500)')
keAngka('abc', [])
keTanggal('46304')
```

Lalu **tebak dulu**, baru jalankan: berapa hasil `keAngka('Rp 2.500.000')` dan `keAngka('-1.000')`? Tuliskan tebakan dan hasil aslinya.

**Selesai bila:** hasil keenam perintah pertama sama dengan komentar di materi, dan kamu bisa menjelaskan (dengan kata sendiri) mengapa `keAngka('abc', [])` menghasilkan `0` dan bukan error.

### Tugas 4 — Tempel dan pahami kode 2B (output + angka kontrol)

1. Dari materi **Sesi 2B**, salin kode ke **bawah** kode 2A.
2. Baca penjelasannya (`async/await`, `Blob`, `onclick`, `classList`).
3. Simpan, F5.

**Selesai bila:** klik **Isi data contoh** mengisi kotak paste, dan klik **Proses** memunculkan kartu **“2. Angka kontrol”** tanpa error di Console.

### Tugas 5 — Data contoh 8 baris

1. Klik **Isi data contoh → Proses**.
2. Bandingkan dengan **“Hasil antara pada data contoh”** di materi: jumlah baris, total, dan pesan peringatan.
3. Buka `data/data-contoh-8-baris.tsv` dan **cari sendiri** baris mana yang: (a) kosong, (b) nilainya negatif, (c) nilainya bukan angka, (d) tidak punya nama gudang. Tulis nomor barisnya di catatan.
4. Jawab di catatan: **mengapa** baris Shampoo (tanpa gudang) ikut terhitung di versi sederhana ini?

**Selesai bila:** hasil sama dengan materi dan keempat pertanyaan terjawab.

### Tugas 6 — Latihan inti: 20 baris + bukti Excel ✅

1. Buka `data/dummy-gudang-100-baris.xlsx`, blok **A1:P21** (judul + 20 baris data), **Ctrl+C**.
   *(Tanpa Excel: buka `data/dummy-gudang-20-baris.tsv` di VS Code, Ctrl+A, Ctrl+C.)*
2. Di halaman `hasil/rekap.html`, **kosongkan** kotak paste, klik di dalamnya, **Ctrl+V**. Pastikan angkanya tampil dengan titik ribuan (`12.500.000`).
3. Klik **Proses**. Catat **jumlah baris**, **total Nilai Sisa Stok**, dan **pesan peringatan**.
4. Di Excel, hitung dengan rumus (isi sel kosong di sheet yang sama, mis. R1 dan R2):
   ```
   =COUNTA(B2:B21)
   =SUM(H2:H21)
   ```
   *(Bila Excel memakai pemisah titik koma, rumus di atas tetap sama karena tidak ada pemisah argumen.)*
5. Bandingkan. Tulis **COCOK** atau **TIDAK COCOK** di catatan.

**Selesai bila:** hasil COCOK. Bila tidak, lihat bagian 7 (Jika macet).

### Tugas 7 — Seluruh 100 baris

1. Ulangi Tugas 6 untuk seluruh data: blok **A1:P101**, rumus `=COUNTA(B2:B101)` dan `=SUM(H2:H101)`.
2. Data ini memuat kasus aneh. Cari **sendiri** (pakai Filter Excel, atau gulir datanya) dan catat: berapa **baris kosong**, berapa **nilai negatif**, berapa **sel Nilai yang bukan angka**, berapa **baris tanpa nama gudang**. Tulis juga nomor barisnya.
3. Perhatikan: apakah alat menganggap baris kosong sebagai data? Mengapa tidak?

**Selesai bila:** hasil COCOK, dan catatan kasus anehmu terisi lengkap.

### Tugas 8 — Geser urutan kolom

1. Di Excel, **potong** kolom H (Nilai Sisa Stok) → klik kanan kolom B → **Insert Cut Cells**. Sekarang Kode Batch pindah ke kolom C.
2. Blok seluruh tabel, Copy, Paste ke alat, Proses.
3. Hitung ulang di Excel dengan rumus yang **disesuaikan** (`=COUNTA(C2:C101)`, `=SUM(B2:B101)`).

**Selesai bila:** hasil alat **sama persis** dengan Tugas 7, dan kamu bisa menjelaskan mengapa urutan kolom tidak berpengaruh (petunjuk: fungsi `cariJudul` dan objek `SINONIM`).

### Tugas 9 — Membaca peringatan

1. Di Excel, hitung jumlah sel Nilai yang bukan angka (data asli, sebelum kolom digeser):
   ```
   =COUNTA(H2:H101)-COUNT(H2:H101)
   ```
2. Bandingkan dengan **jumlah peringatan** di alat (“N peringatan: …”).

**Selesai bila:** keduanya sama, dan kamu bisa menjawab: **mengapa alat memberi peringatan, bukan menghentikan program?**

### Tugas 10 — Tantangan (opsional, nilai tambah)

**a. Judul laporan di atas tabel.** Paste isi `data/dummy-gudang-judul-bergeser.tsv`, Proses. Apakah terbaca? Berapa nomor baris judul yang ditemukan? (Petunjuk: lihat Console, ada `console.log` di kode 2B.) Mengapa bisa?

**b. Ubah judul kolom di Excel, tebak dulu hasilnya, lalu uji:**

| Ubah judul `Nilai Sisa Stok (Rp)` menjadi | Tebakanmu | Hasil sebenarnya |
|---|---|---|
| `Nilai Stok` | | |
| `NILAI SISA STOK (RP)` (huruf kapital) | | |
| `Harga Sisa` | | |

**c.** Untuk `Harga Sisa`, alat menampilkan pesan kesalahan. **Ubah kodenya** (tambahkan satu sinonim di tempat yang tepat) agar `Harga Sisa` terbaca. Catat baris yang kamu ubah.

**d. Ketik ulang** fungsi `keTanggal` **tanpa melihat** materi, lalu uji dengan `keTanggal('46304')` di Console. Lihat materi hanya untuk memeriksa.

---

## 6. Ringkasan cara cek

| Yang dicek | Rumus Excel | Harus sama dengan |
|---|---|---|
| Jumlah baris | `=COUNTA(B2:B101)` (kolom Kode Batch) | “Jumlah baris” di alat |
| Total nilai | `=SUM(H2:H101)` (kolom Nilai Sisa Stok) | “Total Nilai Sisa Stok” di alat |
| Jumlah peringatan | `=COUNTA(H2:H101)-COUNT(H2:H101)` | “N peringatan” di alat |

Alasan rumusnya benar: `COUNTA` menghitung sel yang tidak kosong (baris kosong tidak terhitung), dan `SUM` hanya menjumlah angka (teks seperti `abc` diabaikan, sama seperti alat yang menghitungnya 0).

---

## 7. Jika macet

| Gejala | Kemungkinan penyebab | Coba ini |
|---|---|---|
| Klik **Proses** tidak terjadi apa-apa | Ada kesalahan sintaks, sehingga seluruh script berhenti | F12 → Console, baca tulisan merah beserta nomor barisnya. Biasanya kurang `}`, `)`, atau tanda kutip. |
| `ReferenceError: xxx is not defined` | Kode 2A belum ditempel, atau salah urutan | Cari nama itu dengan Ctrl+F di VS Code. 2A harus **di atas** 2B. |
| `Cannot set properties of null (setting 'onclick')` | Kode ditempel **di luar** `<script>`, atau id tombol salah ketik | Pastikan semua kode berada di antara `<script>` dan `</script>`. |
| Kode sudah diubah tetapi hasilnya sama | Belum disimpan atau belum dimuat ulang | **Ctrl+S**, lalu **F5**. |
| “Baris judul tidak ditemukan” | Judul kolom tidak ada dalam daftar `SINONIM`, atau judul bukan di 15 baris pertama | Periksa ejaan judul; lihat Tugas 10. |
| “Kolom tidak ditemukan: nilai” | Judul kolom nilai tidak dikenali | Lihat Tugas 10b dan 10c. |
| Angka besar tampil `12,500,000` saat paste | Pengaturan wilayah Excel bukan Indonesia | Pakai file `.tsv`, atau ubah pengaturan wilayah. |
| Total tidak cocok dengan Excel | Rentang rumus Excel salah, atau data yang disalin kurang | Pastikan memblok **A1:P101** dan rumusnya `...2:...101`. |
| Banyak peringatan padahal data bersih | Angka berformat tidak lazim (mis. `Rp` dan spasi) atau pakai koma | Lihat isi sel yang disebut di peringatan. |
| Karakter `≥` tampil aneh | File tidak disimpan UTF-8 | Pastikan ada `<meta charset="utf-8">` di `<head>` dan simpan sebagai UTF-8. |

**Kebiasaan yang membantu:** sebelum bertanya, siapkan (1) apa yang kamu harapkan terjadi, (2) apa yang sebenarnya terjadi, (3) tulisan merah di Console (tangkapan layar).

---

## 8. Mengumpulkan hasil

1. Pastikan di folder `sesi-2/hasil/` ada:
   - `rekap.html` (pekerjaanmu), dan
   - `catatan-sesi-2.md` (isi semua tabel di dalamnya).
2. Kirim ke GitHub:
   ```bash
   git add sesi-2/hasil
   git commit -m "Sesi 2: hasil latihan <namamu>"
   git push -u origin sesi-2-<namamu>
   ```
3. Kirim tautan cabang (branch) ke mentor.

> ⚠️ Folder `hasil/` hanya boleh berisi **data dummy**. Jangan menaruh file Excel atau data asli perusahaan di dalamnya.

---

## 9. Daftar selesai

- [ ] `hasil/rekap.html` terbuka tanpa tulisan merah di Console
- [ ] Enam perintah uji di Tugas 3 sudah dicatat, beserta tebakan dan hasilnya
- [ ] **Isi data contoh → Proses** sesuai dengan materi
- [ ] 20 baris: hasil alat **COCOK** dengan Excel
- [ ] 100 baris: hasil alat **COCOK** dengan Excel
- [ ] Kasus aneh di data 100 baris sudah dicatat
- [ ] Setelah kolom digeser, hasilnya tetap sama
- [ ] Jumlah peringatan = jumlah sel bukan angka
- [ ] `catatan-sesi-2.md` terisi lengkap
- [ ] Sudah di-*push* ke GitHub (hanya data dummy)

## 10. Refleksi (jawab singkat di catatan)

1. Mengapa alat ini memakai **Copy-Paste**, bukan membuka file `.xlsx`?
2. Mengapa hasil harus **dicocokkan dengan Excel**? Apa risikonya bila langsung dipercaya?
3. Bagian mana yang paling sulit hari ini, dan apa yang ingin kamu tanyakan ke mentor?
