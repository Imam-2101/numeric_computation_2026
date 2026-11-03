# Algoritma Penyelesaian SPL 3 Variabel
## Metode Eliminasi Gauss dengan Partial Pivoting

### 1. Deskripsi Singkat
Sistem persamaan linear (SPL) tiga variabel berbentuk **A·x = b** diselesaikan dengan dua tahap:
1. **Eliminasi maju**: mengubah matriks augmented menjadi bentuk segitiga atas.
2. **Substitusi mundur**: mencari nilai variabel dari persamaan paling bawah ke atas.

Partial pivoting dipakai agar perhitungan lebih stabil, yaitu dengan memilih elemen
berharga mutlak terbesar pada kolom yang sedang diproses sebagai pivot.

### 2. Masukan dan Keluaran
- **Masukan**: matriks koefisien A berukuran 3×3 dan vektor konstanta b dengan 3 elemen.
- **Keluaran**: nilai x, y, z; atau pesan bahwa SPL tidak memiliki solusi tunggal.

### 3. Langkah-Langkah
1. Tetapkan toleransi ε = 10⁻¹² sebagai batas angka yang dianggap nol.
2. Terima matriks A dan vektor b, lalu susun matriks augmented M = [A | b].
3. Ulangi untuk kolom k = 1 sampai 2:
   - a. Telusuri elemen kolom k dari baris k sampai baris terakhir, lalu pilih
        baris p yang nilai mutlaknya paling besar.
   - b. Jika |M[p][k]| < ε, hentikan proses dan tampilkan
        "SPL tidak memiliki solusi tunggal".
   - c. Jika p ≠ k, tukar posisi baris k dan baris p.
   - d. Untuk setiap baris i di bawah baris k:
        - hitung pengali m = M[i][k] / M[k][k]
        - lakukan operasi baris Rᵢ ← Rᵢ − m·Rₖ
4. Setelah perulangan, periksa pivot terakhir M[3][3]. Jika |M[3][3]| < ε,
   hentikan proses dengan pesan yang sama seperti langkah 3b.
5. Lakukan substitusi mundur:
   - z = M[3][4] / M[3][3]
   - y = (M[2][4] − M[2][3]·z) / M[2][2]
   - x = (M[1][4] − M[1][2]·y − M[1][3]·z) / M[1][1]
6. Tampilkan nilai x, y, dan z.
7. Verifikasi hasil dengan menghitung residual ‖A·x − b‖. Jika mendekati 0,
   solusi dinyatakan benar.
8. Selesai.

### 4. Contoh Perhitungan Manual
Soal:
```
 2x +  y −  z =   8
−3x −  y + 2z = −11
−2x +  y + 2z =  −3
```

Matriks augmented awal:
```
[  2   1  -1 |   8 ]
[ -3  -1   2 | -11 ]
[ -2   1   2 |  -3 ]
```

**Kolom 1**: nilai mutlak terbesar adalah 3 (baris 2), maka R1 ↔ R2.
```
[ -3  -1   2 | -11 ]
[  2   1  -1 |   8 ]
[ -2   1   2 |  -3 ]
```
- R2 ← R2 − (−0,6667)·R1
- R3 ← R3 − (0,6667)·R1
```
[ -3   -1       2      | -11     ]
[  0    0,3333  0,3333 |  0,6667 ]
[  0    1,6667  0,6667 |  4,3333 ]
```

**Kolom 2**: nilai terbesar ada di baris 3 (1,6667), maka R2 ↔ R3.
```
[ -3   -1       2      | -11     ]
[  0    1,6667  0,6667 |  4,3333 ]
[  0    0,3333  0,3333 |  0,6667 ]
```
- R3 ← R3 − (0,2)·R2
```
[ -3   -1       2      | -11     ]
[  0    1,6667  0,6667 |  4,3333 ]
[  0    0       0,2    | -0,2    ]
```

**Substitusi mundur**:
- z = −0,2 / 0,2 = **−1**
- y = (4,3333 − 0,6667·(−1)) / 1,6667 = **3**
- x = (−11 − (−1)(3) − 2(−1)) / (−3) = **2**

Hasil: **x = 2, y = 3, z = −1**.

### 5. Catatan
- Jika pivot bernilai (hampir) nol, SPL tidak memiliki solusi tunggal
  (tidak ada solusi atau solusi tak hingga).
- Kompleksitas waktu metode ini sekitar O(n³).
