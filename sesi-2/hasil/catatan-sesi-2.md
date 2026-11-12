# Catatan Sesi 2 — Nama: ______________________  Tanggal: ______________

## Tugas 3 — Uji Console

| Perintah | Tebakanku | Hasil sebenarnya |
|---|---|---|
| `pecahTSV('a\tb\n1\t2\n')` | | |
| `keAngka('1.250.000')` | | |
| `keAngka('1,5')` | | |
| `keAngka('(500)')` | | |
| `keAngka('abc', [])` | | |
| `keTanggal('46304')` | | |
| `keAngka('Rp 2.500.000')` | | |
| `keAngka('-1.000')` | | |

Mengapa `keAngka('abc', [])` menghasilkan `0` dan bukan error?

> _jawabanku:_

## Tugas 5 — Data contoh 8 baris

| Pertanyaan | Jawabanku |
|---|---|
| Jumlah baris di alat | |
| Total di alat | |
| Pesan peringatan | |
| Nomor baris yang kosong | |
| Nomor baris bernilai negatif | |
| Nomor baris bernilai bukan angka | |
| Nomor baris tanpa nama gudang | |

Mengapa baris Shampoo ikut terhitung di versi sederhana?

> _jawabanku:_

## Tugas 6 dan 7 — Bukti Excel

| Data | Jumlah baris (alat) | Jumlah baris (Excel) | Total (alat) | Total (Excel) | COCOK / TIDAK COCOK |
|---|---|---|---|---|---|
| 20 baris | | | | | |
| 100 baris | | | | | |
| 100 baris, kolom digeser (Tugas 8) | | | | | |

### Kasus aneh di data 100 baris

| Jenis kasus | Jumlah | Nomor baris |
|---|---|---|
| Baris kosong | | |
| Nilai negatif | | |
| Nilai bukan angka | | |
| Baris tanpa nama gudang | | |

Apakah alat menganggap baris kosong sebagai data? Mengapa tidak?

> _jawabanku:_

## Tugas 8 — Mengapa urutan kolom tidak berpengaruh?

> _jawabanku:_

## Tugas 9 — Peringatan

| | Jumlah |
|---|---|
| Peringatan di alat | |
| `=COUNTA(H2:H101)-COUNT(H2:H101)` di Excel | |

Mengapa alat memberi peringatan, bukan menghentikan program?

> _jawabanku:_

## Tugas 10 — Tantangan (opsional)

- (a) Judul bergeser terbaca? Baris judul ke-___ . Alasannya: 
- (b) Tabel tebakan vs hasil:

| Judul baru | Tebakanku | Hasil sebenarnya |
|---|---|---|
| `Nilai Stok` | | |
| `NILAI SISA STOK (RP)` | | |
| `Harga Sisa` | | |

- (c) Baris kode yang kuubah agar `Harga Sisa` terbaca:

```js
// tulis di sini
```

## Refleksi

1. Mengapa Copy-Paste, bukan membuka `.xlsx`?
2. Mengapa hasil harus dicocokkan dengan Excel?
3. Bagian tersulit hari ini / pertanyaan untuk mentor:

## Kendala yang kutemui dan cara mengatasinya

> _isi di sini_
