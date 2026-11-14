# Algoritma Penyelesaian SPL 3 Variabel
## Metode Eliminasi Gauss dengan Partial Pivoting

**Nama:** Imam Bagus Saputra | **NIM:** 202511420024 | **Prodi:** Teknik Informatika

## 1. Bentuk Umum

```
a11·x + a12·y + a13·z = b1
a21·x + a22·y + a23·z = b2
a31·x + a32·y + a33·z = b3
```

- **A**: matriks koefisien (3×3)
- **b**: vektor konstanta (3×1)
- **Ide metode:** ubah sistem menjadi bentuk segitiga atas dengan operasi baris, lalu hitung z, y, x dari baris paling bawah ke atas.

## 2. Algoritma Utama (`main`)

1. Mulai.
2. Impor NumPy, tetapkan EPSILON = 10⁻¹² (batas nilai dianggap nol), atur format cetak matriks.
3. Tampilkan menu: 1) contoh soal, 2) input sendiri.
4. Baca pilihan pengguna.
5. Jika pilihan = 2, ambil A dan b dengan **Algoritma Input** (bagian 3).
6. Jika bukan, pakai contoh soal:
   - A = [[2, 1, −1], [−3, −1, 2], [−2, 1, 2]]
   - b = [8, −11, −3]
7. Selesaikan dengan **Algoritma Eliminasi Gauss** (bagian 4), simpan hasilnya di x.
8. Jika solusi tunggal ditemukan:
   1. Tampilkan x, y, z.
   2. Bandingkan dengan `numpy.linalg.solve(A, b)`.
   3. Hitung residual ‖A·x − b‖ (harus mendekati 0).
9. Selesai.

## 3. Algoritma Input (`baca_input`)

1. Tetapkan n = 3.
2. Untuk baris i = 1 sampai 3: baca 3 angka dipisah spasi, simpan ke A. Jika jumlahnya bukan 3, tampilkan kesalahan.
3. Baca 3 angka untuk b. Jika jumlahnya bukan 3, tampilkan kesalahan.
4. Kembalikan A dan b.

## 4. Algoritma Eliminasi Gauss (`gauss_eliminasi`)

**Input:** A (3×3), b (3×1)  **Output:** x = [x, y, z], atau None

### A. Persiapan

1. Ubah A dan b menjadi array NumPy bertipe desimal, tetapkan n = jumlah elemen b.
2. Jika A bukan matriks n × n, tampilkan kesalahan.
3. Bentuk matriks augmented M = [A | b] (ukuran 3×4).

### B. Eliminasi Maju

4. Untuk kolom k = 1 sampai 2:
   1. **Cari pivot:** pilih baris p (dari baris k ke bawah) dengan |M[p][k]| terbesar.
   2. **Cek pivot:** jika |M[p][k]| < EPSILON, tampilkan "tidak memiliki solusi tunggal" dan kembalikan None.
   3. **Tukar baris:** jika p ≠ k, tukar baris k dan p.
   4. **Nolkan kolom k di bawah pivot:** untuk setiap baris i di bawah k,
      - hitung m = M[i][k] / M[k][k]
      - R_i ← R_i − m · R_k
5. Jika |M[3][3]| < EPSILON, tampilkan "tidak memiliki solusi tunggal" dan kembalikan None.

### C. Substitusi Mundur

6. Siapkan array x berisi nol.
7. z = M[3][4] / M[3][3]
8. Untuk baris 2, lalu baris 1:
   - jumlah = Σ M[i][j] · x[j] (j = i+1 sampai 3)
   - x[i] = (M[i][4] − jumlah) / M[i][i]

   Artinya:
   - y = (M[2][4] − M[2][3]·z) / M[2][2]
   - x = (M[1][4] − M[1][2]·y − M[1][3]·z) / M[1][1]
9. Kembalikan x.

## 5. Contoh Perhitungan

Sistem:

```
 2x +  y −  z =   8
−3x −  y + 2z = −11
−2x +  y + 2z =  −3
```

**Matriks augmented awal**

```
[  2   1  -1 |   8 ]
[ -3  -1   2 | -11 ]
[ -2   1   2 |  -3 ]
```

**Kolom 1:** |−3| terbesar (baris 2), maka R1 ↔ R2.

| Operasi | Pengali m |
|---|---|
| R2 ← R2 − (−0,6667)·R1 | 2 / (−3) |
| R3 ← R3 − (0,6667)·R1 | (−2) / (−3) |

```
[ -3  -1       2      | -11     ]
[  0   0,3333  0,3333 |  0,6667 ]
[  0   1,6667  0,6667 |  4,3333 ]
```

**Kolom 2:** 1,6667 terbesar (baris 3), maka R2 ↔ R3.

| Operasi | Pengali m |
|---|---|
| R3 ← R3 − (0,2)·R2 | 0,3333 / 1,6667 |

```
[ -3  -1       2      | -11     ]
[  0   1,6667  0,6667 |  4,3333 ]
[  0   0       0,2    | -0,2    ]
```

**Substitusi mundur**

| Variabel | Perhitungan | Hasil |
|---|---|---|
| z | −0,2 / 0,2 | **−1** |
| y | (4,3333 − 0,6667·(−1)) / 1,6667 | **3** |
| x | (−11 − (−1)·3 − 2·(−1)) / (−3) | **2** |

**Verifikasi**

| Persamaan | Perhitungan | Hasil |
|---|---|---|
| 2x + y − z | 4 + 3 + 1 | 8 ✓ |
| −3x − y + 2z | −6 − 3 − 2 | −11 ✓ |
| −2x + y + 2z | −4 + 3 − 2 | −3 ✓ |

**Solusi: x = 2, y = 3, z = −1**

## 6. Kapan Metode Gagal?

Program berhenti dengan pesan "tidak memiliki solusi tunggal" jika pivot mendekati nol, yaitu bila ada persamaan yang kelipatan persamaan lain (solusi tak hingga) atau saling bertentangan (tanpa solusi).
