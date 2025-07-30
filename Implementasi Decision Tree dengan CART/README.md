# 📁 Implementasi Decision Tree dengan CART

## 📊 Business Understanding

### 🧩 Latar Belakang
Bank ABC sedang menghadapi tantangan dalam pengelolaan permohonan kredit nasabah. Andra, selaku Manajer Pinjaman Kredit, kesulitan menganalisis data nasabah karena jumlahnya sangat besar dan keterbatasan SDM di divisinya. Akibatnya, banyak penyaluran kredit yang tidak tepat sasaran dan berujung pada kredit macet, yang berdampak negatif terhadap kondisi keuangan bank.

Dalam rapat perusahaan, Direktur Bank ABC, Kroma, memutuskan untuk melibatkan divisi Data Science guna membantu menyelesaikan masalah ini. Senja, seorang Ilmuwan Data dari Bank BCA, ditugaskan untuk menganalisis data historis permohonan kredit yang dimiliki Bank ABC.

### 🎯 Tujuan
Tujuan dari proyek ini adalah:
- Mengembangkan model klasifikasi otomatis untuk menilai tingkat risiko nasabah berdasarkan data historis.
- Mengelompokkan nasabah ke dalam kategori risiko (rendah, sedang, tinggi) guna membantu pengambilan keputusan dalam penyaluran kredit.
- Mengurangi potensi kredit macet dengan memberikan rekomendasi berbasis data kepada manajemen kredit.
- Memaksimalkan pemanfaatan data historis yang sebelumnya belum digunakan secara optimal dalam proses evaluasi kredit.

---

## 🧠 Data Understanding

| Kolom                    | Tipe Data       | Deskripsi                                                                 |
|--------------------------|-----------------|---------------------------------------------------------------------------|
| `kode_kontrak`           | Kategorikal     | ID unik untuk setiap kontrak pinjaman                                    |
| `pendapatan_setahun_juta`| Numerik         | Pendapatan tahunan nasabah dalam satuan juta rupiah                      |
| `kpr_aktif`              | Kategorikal     | Status kepemilikan KPR (YA/TIDAK)                                         |
| `durasi_pinjaman_bulan` | Numerik         | Lama pinjaman dalam bulan                                                 |
| `jumlah_tanggungan`     | Numerik         | Jumlah tanggungan keluarga                                                |
| `rata_rata_overdue`     | Kategorikal     | Rata-rata keterlambatan pembayaran (rentang hari)                        |
| `risk_rating`           | Numerik Ordinal | Target klasifikasi: tingkat risiko kredit (skala 1–4)                    |

---

## 1️⃣ Data Preparation
- **Eksplorasi Dataset**: Memeriksa tipe data tiap kolom dan melakukan analisis statistik sederhana (min, max, mean).
- **Seleksi Fitur**: Memilih atribut yang relevan untuk klasifikasi dan menghapus kolom yang tidak diperlukan seperti `rata_rata_overdue`.
- **Transformasi Data**: Mengubah tipe data string ke boolean atau numerik agar kompatibel dengan algoritma Decision Tree.
- **Pembagian Dataset**: Memisahkan data menjadi `training set` dan `testing set` untuk memastikan evaluasi dilakukan pada data yang belum pernah dilatih.

---

## 2️⃣ Pembuatan Model
- **Inisialisasi Model**: Menggunakan algoritma Decision Tree dengan pendekatan CART (Classification and Regression Tree).
- **Pelatihan Model**: Melatih model menggunakan data training untuk membentuk struktur pohon keputusan.
- **Visualisasi Model**: Menampilkan struktur pohon untuk memahami logika klasifikasi dan interpretasi keputusan.

---

## 3️⃣ Evaluasi Model
- **Pengukuran Kinerja**: Menggunakan data testing untuk mengevaluasi akurasi dan performa model.
- **Hyperparameter Tuning**: Menyesuaikan parameter seperti `max_depth`, `min_samples_split`, dan lainnya untuk meningkatkan akurasi dan menghindari overfitting.

---




