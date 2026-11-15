# Algoritma Metode Bagidua (Bisection)
## Mencari Akar Persamaan Non-Linear f(x) = 0

**Nama:** Imam Bagus Saputra | **NIM:** 202511420024 | **Prodi:** Teknik Informatika

## 1. Dasar Metode

- **Masalah:** mencari nilai x = s sehingga f(s) = 0. Nilai s disebut akar.
- **Jenis:** metode tertutup, yaitu akar dicari di dalam selang [a, b] yang sudah dipastikan memuat akar, sehingga iterasinya selalu konvergen.
- **Syarat akar ada:** f(x) menerus di [a, b] dan f(a) · f(b) < 0 (nilai f di kedua ujung berbeda tanda).
- **Ide:** selang dibagi dua di titik tengah c. Dipilih separuh selang yang nilai f di ujungnya masih berbeda tanda. Proses diulang sampai selang sangat sempit.

## 2. Algoritma Utama

Urutan pengerjaan program (sesuai urutan cell di notebook):

1. Mulai.
2. Impor library `plotly`, lalu tetapkan fungsi f(x) dan pengaturan (bagian 3).
3. Cari selang yang mengandung akar dengan **Algoritma Cari Selang** (bagian 4).
4. Gambar **Diagram 1**: grafik f(x) dan selang berakar (bagian 6).
5. Jalankan **Algoritma Bagidua** pada selang tersebut (bagian 5).
6. Gambar **Diagram 2**: proses pembagian selang (bagian 7).
7. Tampilkan hasil akhir (bagian 8).
8. Selesai.

## 3. Pengaturan

| Nama | Arti | Nilai |
|---|---|---|
| f(x) | Fungsi yang dicari akarnya | x³ − 2x − 5 |
| A_AWAL, B_AKHIR | Rentang x yang diperiksa | 1 sampai 3 |
| H | Kenaikan absis (jarak antar titik) | 0,1 |
| EPSILON1 | Batas lebar selang akhir | 10⁻⁵ |
| EPSILON2 | Batas \|f(c)\| yang dianggap nol | 10⁻⁸ |
| DELTA | Batas galat relatif | 10⁻⁶ |
| MAKS_ITER | Batas jumlah iterasi (pengaman) | 100 |
| LEBAR, TINGGI | Ukuran diagram (piksel) | 650 dan 340 |

## 4. Algoritma Cari Selang (`cari_selang`)

**Tujuan:** menemukan selang kecil yang memuat akar dengan tabel nilai f(x).
**Input:** a, b, h  **Output:** daftar titik x, nilai f(x), dan daftar selang berakar

1. Hitung banyak langkah n = (b − a) / h.
2. Buat titik x_i = a + i · h untuk i = 0 sampai n.
3. Hitung f(x_i) untuk setiap titik.
4. Untuk i = 0 sampai n − 1, jika f(x_i) · f(x_{i+1}) < 0, simpan pasangan [x_i, x_{i+1}] sebagai selang berakar.
5. Kembalikan titik x, nilai f(x), dan daftar selang.

Setelah itu, ambil selang pertama sebagai selang kerja [a₀, b₀], lalu cetak f(a₀) dan f(b₀) sebagai bukti tandanya berbeda.

## 5. Algoritma Bagidua (`bagidua`)

**Input:** a, b (selang kerja)
**Output:** data setiap iterasi (r, a, c, b, f(a), f(c), f(b), selang baru, lebar)

1. **Cek syarat:** jika f(a) · f(b) ≥ 0, tampilkan "Selang tidak valid" dan berhenti.
2. Siapkan daftar `langkah` untuk menyimpan data iterasi, dan tetapkan c_lama = kosong.
3. Ulangi untuk r = 0, 1, 2, ... sampai MAKS_ITER:
   1. Hitung titik tengah **c = (a + b) / 2**.
   2. Hitung f(a), f(c), dan f(b), lalu simpan a, c, b beserta nilai f-nya ke `langkah`.
   3. **Pilih selang baru:**
      - Jika f(a) · f(c) < 0 (akar di kiri), maka **b ← c**, selang baru [a, c].
      - Jika tidak (akar di kanan), maka **a ← c**, selang baru [c, b].
   4. Hitung lebar selang baru = |a − b|, lalu catat selang baru dan lebarnya.
   5. Jika bukan iterasi pertama, hitung galat relatif = |c − c_lama| / |c|.
   6. Simpan c_lama ← c.
   7. **Cek kriteria berhenti.** Berhenti jika salah satu terpenuhi:
      - lebar < EPSILON1
      - |f(c)| < EPSILON2
      - galat relatif < DELTA
4. Kembalikan `langkah`.

## 6. Algoritma Diagram 1 (Grafik f(x) dan Selang Berakar)

1. Panggil `kurva` untuk membuat 301 titik halus dari A_AWAL sampai B_AKHIR (`kurva` membagi rentang menjadi 300 bagian sama lebar lalu menghitung f di tiap titik).
2. Gambar kurva f(x) (garis biru).
3. Gambar titik-titik tabel dari langkah 4 (titik oranye).
4. Gambar sumbu-X (garis hitam di y = 0).
5. Beri blok warna hijau pada selang [a₀, b₀] dengan keterangan "akar di [a₀, b₀]".
6. Atur ukuran diagram (LEBAR × TINGGI) lalu tampilkan.

## 7. Algoritma Diagram 2 (Proses Pembagian Selang)

1. Tentukan rentang tampilan tetap: dari a₀ − 0,1·(b₀ − a₀) sampai b₀ + 0,1·(b₀ − a₀), supaya grafik tidak melompat saat slider digeser.
2. Panggil `kurva` pada rentang tersebut, lalu gambar kurva f(x) dan sumbu-X.
3. Untuk setiap iterasi i, buat 4 objek (hanya iterasi 0 yang terlihat di awal):
   1. Garis putus-putus dari titik a, b, dan c₀ sampai c_i ke sumbu-X.
   2. Titik **a** (merah) dan **b** (oranye).
   3. Titik **c₀ sampai c_i** (hijau). Label hanya untuk c₀ sampai c₃ dan c_i, sedangkan nilai lengkapnya muncul saat kursor diarahkan.
   4. Garis merah tebal di sumbu-X sepanjang selang [a, b] iterasi tersebut.
4. Buat slider dengan satu posisi per iterasi. Posisi i menampilkan kurva dan 4 objek milik iterasi i saja, dan judul diagram diganti dengan a, b, c, selang baru, dan lebarnya.
5. Atur ukuran diagram (LEBAR × (TINGGI + 60) agar slider muat), lalu tampilkan.

## 8. Algoritma Hasil Akhir

1. Ambil data iterasi terakhir dari `langkah`.
2. Tampilkan hampiran akar x = c iterasi terakhir.
3. Tampilkan f(x), jumlah iterasi, dan lebar selang terakhir.
4. **Pemeriksaan:** hitung f(akar) dan pastikan nilainya mendekati 0.

## 9. Contoh Perhitungan

Soal: **f(x) = x³ − 2x − 5**, rentang [1 ; 3], h = 0,1, EPSILON1 = 0,00001.

### 9.1 Mencari selang

Ada 21 titik (x = 1,0 ; 1,1 ; ... ; 3,0). Nilai di sekitar akar:

| x | 1,9 | 2,0 | 2,1 | 2,2 |
|---|---|---|---|---|
| f(x) | −1,941 | −1,000 | **+0,061** | 1,248 |

Tanda berubah dari negatif ke positif antara x = 2,0 dan x = 2,1, jadi selang kerja adalah **[2,0 ; 2,1]**. Cek syarat: f(2,0) · f(2,1) = (−1)(0,061) < 0 ✓.

### 9.2 Iterasi bagidua

| r | a | c | b | f(c) | Selang baru | Lebar |
|---|---|---|---|---|---|---|
| 0 | 2,000000 | 2,050000 | 2,100000 | −0,484875 | [c, b] | 0,050000 |
| 1 | 2,050000 | 2,075000 | 2,100000 | −0,215828 | [c, b] | 0,025000 |
| 2 | 2,075000 | 2,087500 | 2,100000 | −0,078393 | [c, b] | 0,012500 |
| 3 | 2,087500 | 2,093750 | 2,100000 | −0,008942 | [c, b] | 0,006250 |
| 4 | 2,093750 | 2,096875 | 2,100000 | **+0,025968** | [a, c] | 0,003125 |
| ... | ... | ... | ... | ... | ... | ... |
| 13 | 2,094543 | 2,094550 | 2,094556 | −0,000021 | [c, b] | 0,000006 |

**Cara membaca:**
- r = 0: f(a) = −1 dan f(c) = −0,485 sama tanda, jadi f(a)·f(c) > 0. Akar di kanan, maka a ← c.
- r = 4: f(c) = +0,026 berbeda tanda dengan f(a) = −0,009, jadi f(a)·f(c) < 0. Akar di kiri, maka b ← c.
- Lebar selang selalu menjadi **setengahnya** tiap iterasi.

**Alasan berhenti pada r = 13 (iterasi ke-14):** lebar 0,000006 < 0,00001. Perhitungannya: 0,1 / 2¹³ ≈ 0,0000122 (belum cukup) dan 0,1 / 2¹⁴ ≈ 0,0000061 (sudah cukup).

### 9.3 Hasil

- **Hampiran akar: x ≈ 2,094550**
- f(x) ≈ −2,14 × 10⁻⁵ (hampir nol)
- Jumlah iterasi: 14
- Pemeriksaan: f(2,094550) ≈ −0,000021, mendekati 0 ✓

## 10. Kesesuaian dengan Kode

| Bagian algoritma | Bagian kode / cell |
|---|---|
| 3. Pengaturan | Cell 3 (`f`, `A_AWAL`, ..., `LEBAR`, `TINGGI`) |
| 4. Cari selang | Cell 5 (`cari_selang`) dan Cell 11 (menjalankannya) |
| 5. Bagidua | Cell 7 (`bagidua`) dan Cell 17 (menjalankannya) |
| 6. Diagram 1 | Cell 9 (`kurva`) dan Cell 14 |
| 7. Diagram 2 | Cell 20 |
| 8. Hasil akhir | Cell 23 |
