Nama : Yayan Kurniawan
# Data Science in Marketing : Customer Segmentation with Python
<img width="2560" height="1579" alt="image" src="https://github.com/user-attachments/assets/27878194-b916-44f7-9adf-0a0b698a8d2f" />

## Business Understanding
### Latar Belakang
Perusahaan e-commerce tempat kamu bekerja sedang berupaya memahami karakteristik pelanggannya secara lebih mendalam. Dengan pemahaman tersebut, perusahaan dapat menyusun strategi pemasaran yang lebih efektif dan efisien. Dalam era digital, strategi pemasaran yang terpersonalisasi terbukti lebih menarik minat pelanggan dan meningkatkan konversi penjualan. Salah satu pendekatan yang dapat digunakan untuk mengenali pelanggan lebih baik adalah melalui segmentasi pelanggan berdasarkan kesamaan karakteristik mereka.

### Permasalahan
Saat ini perusahaan belum memiliki pemahaman yang cukup mendalam tentang karakteristik masing-masing pelanggan. Pendekatan pemasaran yang digunakan masih bersifat umum dan belum disesuaikan dengan profil pelanggan tertentu. Hal ini menyebabkan kurang optimalnya strategi pemasaran, serta potensi pemborosan biaya karena pendekatan yang tidak tepat sasaran.

### Tujuan Bisnis
Meningkatkan efektivitas dan efisiensi strategi pemasaran dengan cara memahami karakteristik pelanggan secara lebih akurat. Dengan demikian, perusahaan dapat menawarkan produk dan promosi yang lebih relevan sesuai dengan kebutuhan dan preferensi tiap segmen pelanggan.

### Tujuan Analisis
Melakukan segmentasi pelanggan menggunakan algoritma K-Prototypes, yang memungkinkan pengelompokan data campuran (numerik dan kategorikal), untuk:

- Mengidentifikasi kelompok-kelompok pelanggan yang memiliki kesamaan karakteristik,

- Memberikan insight terhadap tiap segmen pelanggan,

- Menjadi dasar rekomendasi dalam penyusunan strategi pemasaran yang lebih tepat sasaran.

## Data Understanding
<img width="589" height="276" alt="image" src="https://github.com/user-attachments/assets/24571b08-e471-42d8-aca5-928a5dff0401" />

Data tersebut memiliki tujuh kolom dengan penjelasan sebagai berikut:

Customer ID: Kode pelanggan dengan format campuran teks CUST- diikuti angka
- Nama Pelanggan: Nama dari pelanggan dengan format teks tentunya
- Jenis Kelamin: Jenis kelamin dari pelanggan, hanya terdapat dua isi data kategori yaitu Pria dan Wanita
- Umur: Umur dari pelanggan dalam format angka
- Profesi: Profesi dari pelanggan, juga bertipe teks kategori yang terdiri dari Wiraswasta, Pelajar, Professional, Ibu Rumah Tangga, dan Mahasiswa.
- Tipe Residen: Tipe tempat tinggal dari pelanggan kita, untuk dataset ini hanya ada dua kategori: Cluster dan Sector.
- Nilai Belanja Setahun: Merupakan total belanja yang sudah dikeluarkan oleh pelanggan tersebut.


## Hands-On Coding
### Mempersiapkan Library
<pre>
# Import library untuk manipulasi dan analisis data
import pandas as pd  
# Import library untuk visualisasi dasar
import matplotlib.pyplot as plt  
# Import library visualisasi yang lebih menarik berbasis matplotlib
import seaborn as sns  
# Import LabelEncoder dari scikit-learn untuk mengubah data kategorikal menjadi numerik
from sklearn.preprocessing import LabelEncoder  
# Import algoritma clustering untuk data kategorikal
from kmodes.kmodes import KModes  
# Import algoritma clustering untuk data campuran (numerik dan kategorikal)
from kmodes.kprototypes import KPrototypes  
# Import library untuk menyimpan model yang telah dibuat
import pickle  
# Import Path dari pathlib untuk mengelola path file secara fleksibel
from pathlib import Path
</pre> 

### Exploratory Data Analysis (EDA)
Membaca Data Pelanggan

<pre> 
# import dataset  
df = pd.read_csv("https://storage.googleapis.com/dqlab-dataset/customer_segments.txt", sep="\t")  
# menampilkan data  
print(df.head())
</pre> 

Output:

<img width="487" height="140" alt="image" src="https://github.com/user-attachments/assets/49786ea3-37b5-4f34-a007-3da36633595f" />

