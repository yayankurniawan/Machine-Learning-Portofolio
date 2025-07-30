# 📊 Credit Scoring dengan Random Forest
## Business Understanding
### Latar Belakang
Kantorku, PT. DQ perusahaan pinjam meminjam (peer to peer lending) baru-baru ini meluncurkan sebuah program baru yang memungkinkan  orang-orang menggunakan jasa kami dapat melakukan pengajuan peminjaman (kredit) melalui aplikasi. Keren banget! Terlebih lagi selama pandemi seperti ini yang mengharuskan kita membatasi kegiatan termasuk interaksi dengan banyak orang. 

Namun, setelah aku cek kembali proses verifikasinya, masih saja  dilakukan secara manual. Tentunya ini memakan waktu dan memperbesar peluang human error. Kondisi ini juga disadari oleh petinggi perusahaan. Aku sebagai data analyst pun diminta untuk memikirkan solusinya.

Aku sedang mempertimbangkan untuk menggunakan machine learning Random Forest. Sepengetahuanku, cara ini dapat  memudahkan untuk mengambil keputusan dalam penilaian calon peminjam yang akan mengajukan pinjaman. Sebelum menerapkannya secara makro, aku perlu mencoba salah satu sampel calon peminjam untuk diuji menggunakan Random Forest.  Aku pun mulai memproses datanya.

---

## 📊 Data Understanding

Dataset yang digunakan adalah `credit_scoring_dqlab.xlsx` yang berisi informasi calon peminjam dari perusahaan peer-to-peer lending. Dataset ini terdiri dari **900 baris** dan **7 kolom**.

### 🔎 Struktur Dataset

| Nama Kolom                | Tipe Data | Deskripsi                                                                 |
|---------------------------|-----------|---------------------------------------------------------------------------|
| `kode_kontrak`            | object    | ID unik untuk setiap kontrak kredit                                      |
| `pendapatan_setahun_juta`| int64     | Pendapatan tahunan peminjam (dalam juta Rupiah)                          |
| `kpr_aktif`               | object    | Status kepemilikan KPR aktif (`YA` atau `TIDAK`)                          |
| `durasi_pinjaman_bulan`  | int64     | Lama pinjaman dalam bulan                                                |
| `jumlah_tanggungan`      | int64     | Jumlah tanggungan finansial peminjam                                     |
| `rata_rata_overdue`      | object    | Rata-rata keterlambatan pembayaran (kategori: `0-30 days`, `>90 days`, dll) |
| `risk_rating`            | int64     | Skor risiko kredit (1 = rendah, 5 = tinggi)                              |

---

## ⚙️ Data Preparation

Tahap ini mencakup:

- Membaca dan memeriksa struktur dataset
- Mengubah kolom kategorikal menjadi numerik
- Menentukan fitur (`X`) dan target (`y`)
- Membagi data menjadi training dan testing

---

## 🤖 Modeling

Model yang digunakan adalah **Random Forest Classifier**, yang dikenal mampu menangani data dengan fitur numerik dan kategorikal, serta memberikan interpretasi melalui feature importance. Proses modeling juga mencakup:

- Pelatihan model dasar
- Analisis kontribusi fitur
- Tuning hyperparameter untuk meningkatkan performa

---

## 📈 Evaluation

Evaluasi dilakukan menggunakan metrik seperti:

- Accuracy
- Precision
- Recall
- F1-score

Langkah optimalisasi dilakukan dengan menghapus fitur yang kurang relevan dan membandingkan performa model sebelum dan sesudah tuning.

---

## Hasil Analisis
<img width="376" height="339" alt="image" src="https://github.com/user-attachments/assets/c9a6e489-3b2a-4f53-9b15-f3c87f424825" />

Dapat dilihat bahwa setelah hyperparameter tuning, terdapat peningkatan akurasi sebesar 1.58%.

Setelah melihat hasilnya, aku tersenyum puas dan menyandarkan punggungku dengan lega. Akhirnya selesai juga! Dengan tingkat akurasi yang baik, aku cukup percaya diri untuk mempresentasikan output ini kepada direktur dan para petinggi. Senang sekali keahlianku bisa berkontribusi menyelesaikan program pinjam-meminjam ini!



