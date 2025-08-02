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
## Mempersiapkan Library dan Data
### Mempersiapkan Library
```
# Menginstal library 'kmodes' untuk melakukan clustering pada data kategorikal dan campuran
!pip install kmodes
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
``` 

### Membaca Data Pelanggan

```
# import dataset  
df = pd.read_csv("https://storage.googleapis.com/dqlab-dataset/customer_segments.txt", sep="\t")  
# menampilkan data  
print(df.head())
``` 

Output:

<img width="487" height="140" alt="image" src="https://github.com/user-attachments/assets/49786ea3-37b5-4f34-a007-3da36633595f" />

### Melihat Informasi dari Data
```
# Menampilkan informasi data  
df.info() 
```
Output:

<img width="263" height="186" alt="image" src="https://github.com/user-attachments/assets/01b10d6c-0c0b-44bd-adfa-b93ccf757d3a" />

### Kesimpulan
Setelah melakukan pemanggilan data dan melihat informasi data yang kamu miliki, kamu akhirnya mengetahui bahwa:

- Data yang akan digunakan terdiri dari 50 baris dan 7 kolom
- Tidak ada nilai null pada data
- Dua kolom memiliki tipe data numeric dan lima data bertipe string

## Melakukan Eksplorasi Data
### Eksplorasi Data Numerik
```
# Mengatur gaya visualisasi seaborn menjadi 'white' untuk tampilan plot yang lebih bersih dan minimalis
sns.set(style='white')
# Fungsi untuk membuat plot  
def observasi_num(features):  
    fig, axs = plt.subplots(2, 2, figsize=(10, 9))
    for i, kol in enumerate(features):
     sns.boxplot(df[kol], ax = axs[i][0])
     sns.distplot(df[kol], ax = axs[i][1])   
     axs[i][0].set_title('mean = %.2f\n median = %.2f\n std = %.2f'%(df[kol].mean(), df[kol].median(), df[kol].std()))
    plt.setp(axs)
    plt.tight_layout()
    plt.show()  
  
# Memanggil fungsi untuk membuat Plot untuk data numerik   
kolom_numerik = ['Umur','NilaiBelanjaSetahun']
observasi_num(kolom_numerik) 
```
Output:

<img width="983" height="884" alt="image" src="https://github.com/user-attachments/assets/f68bcef0-0479-4b3b-b06c-fab2377c6387" />

### Eksplorasi Data Kategorikal
```
# Siapkan list nama kolom kategorikal yang ingin di-plot
kolom_kategorikal = ['Jenis Kelamin', 'Profesi', 'Tipe Residen']

# Buat canvas untuk menampung 3 plot, dengan ukuran 7x10 inci
# subplots() akan mengembalikan sebuah objek Figure (fig) dan array of Axes (axs)
fig, axs = plt.subplots(3, 1, figsize=(7, 10))

# Lakukan perulangan untuk setiap kolom di 'kolom_kategorikal'
for i, kol in enumerate(kolom_kategorikal):
    # Buat plot count menggunakan seaborn untuk kolom saat ini.
    # Parameter 'order' digunakan untuk mengurutkan bar dari yang paling tinggi ke rendah.
    sns.countplot(x=kol, data=df, order=df[kol].value_counts().index, ax=axs[i])
    
    # Atur judul untuk subplot saat ini
    axs[i].set_title(f'Count Plot: {kol}', fontsize=15)
    
    # Beri anotasi (label angka) pada setiap bar di plot
    for p in axs[i].patches:
        axs[i].annotate(
            format(p.get_height(), '.0f'),  # Teks anotasi (tinggi bar)
            (p.get_x() + p.get_width() / 2., p.get_height()),  # Posisi teks (di tengah bar)
            ha='center', va='center',  # Penataan horizontal dan vertikal
            xytext=(0, 10),  # Offset teks 10 poin ke atas
            textcoords='offset points'
        )
    
    # Sembunyikan sumbu Y dan hapus 'spine' (garis tepi) di sisi kanan, atas, dan kiri
    sns.despine(right=True, top=True, left=True)
    axs[i].axes.yaxis.set_visible(False)

# Atur layout agar semua subplot dan judul tidak tumpang tindih
fig.tight_layout()

# Tampilkan plot ke layar
plt.show()
```
Output:

<img width="684" height="983" alt="image" src="https://github.com/user-attachments/assets/86445d72-920e-4d94-bc6c-0ae52d7ae3c4" />

### Kesimpulan
Dari hasil eksplorasi data tersebut kamu dapat mendapatkan informasi:

- Rata-rata dari umur pelanggan adalah 37.5 tahun
- Rata-rata dari nilai belanja setahun pelanggan adalah 7,069,874.82
- Jenis kelamin pelanggan di dominasi oleh wanita sebanyak 41 orang (82%) dan laki-laki sebanyak 9 orang (18%)
- Profesi terbanyak adalah Wiraswasta (40%) diikuti dengan Professional (36%) dan lainnya sebanyak (24%)
- Dari seluruh pelanggan 64% dari mereka tinggal di Cluster dan 36% nya tinggal di Sektor

## Mempersiapkan Data Sebelum Permodelan
### Standarisasi Kolom Numerik
```
kolom_numerik  = ['Umur','NilaiBelanjaSetahun']  
  
# Statistik sebelum Standardisasi  
print('Statistik Sebelum Standardisasi\n')  
print(df[kolom_numerik ].describe().round(1))  
# Standardisasi  
df_std = StandardScaler().fit_transform(df[kolom_numerik])  
# Membuat DataFrame  
df_std = pd.DataFrame(data=df_std, index=df.index, columns=df[kolom_numerik].columns)  
# Menampilkan contoh isi data dan summary statistic  
print('Contoh hasil standardisasi\n')  
print(df_std.head())  
print('Statistik hasil standardisasi\n')  
print(df_std.describe().round(0))
```

Output:

<img width="247" height="475" alt="image" src="https://github.com/user-attachments/assets/8c8ce75e-d7a2-43ac-a20a-a9eab4d0be95" />

### Konversi Kategorikal Data dengan Label Encoder
```
# Inisiasi nama kolom kategorikal  
kolom_kategorikal = ['Jenis Kelamin','Profesi','Tipe Residen']  
  
# Membuat salinan data frame  
df_encode = df[kolom_kategorikal].copy()  
  
# Melakukan labelEncoder untuk semua kolom kategorikal  
for col in kolom_kategorikal:  
    df_encode[col] = LabelEncoder().fit_transform(df_encode[col])  
      
# Menampilkan data  
print(df_encode.head())
```
Output:

<img width="297" height="103" alt="image" src="https://github.com/user-attachments/assets/7d43b00f-ee3d-4b82-bd81-196c04c6127c" />

### Menggabungkan Data untuk Permodelan
```
kolom_numerik  = ['Umur','NilaiBelanjaSetahun']
df_std = StandardScaler().fit_transform(df[kolom_numerik])
df_std = pd.DataFrame(data=df_std, index=df.index, columns=df[kolom_numerik].columns)
 
kolom_kategorikal = ['Jenis Kelamin','Profesi','Tipe Residen']
df_encode = df[kolom_kategorikal].copy()
for col in kolom_kategorikal:
    df_encode[col] = LabelEncoder().fit_transform(df_encode[col])

# Menggabungkan data frame
df_model = df_encode.merge(df_std, left_index = True, right_index=True, how = 'left')  
print (df_model.head())
```
Output:

<img width="504" height="101" alt="image" src="https://github.com/user-attachments/assets/497c0365-3fc4-4026-b55c-317552c58d99" />

## Permodelan
### Pengertian Clustering dan Algoritma K-Prototypes

Clustering adalah proses pembagian objek-objek ke dalam beberapa kelompok (cluster) berdasarkan tingkat kemiripan antara satu objek dengan yang lain.

Terdapat beberapa algoritma untuk melakukan clustering ini. Salah satu yang populer adalah k-means.

K-means sendiri umumnya hanya digunakan untuk data-data yang bersifat numerik. Sedangkan untuk yang data yang hanya bersifat kategorikal saja, dapat menggunakan k-modes.

Dan jika data yang dimiliki terdapat gabungan kategorikal dan numerikal variabel, maka bisa menggunakan algoritma k-prototype yang merupakan gabungan dari k-means dan k-modes. Hal ini bisa dilakukan dengan menggunakan library k-modes yang didalamnya terdapat modul kprototype.

Hal yang diperlukan untuk menggunakan algoritma kprototype yakni memasukkan jumlah cluster yang dikehendaki dan memberikan index kolom untuk kolom-kolom yang bersifat kategorikal.

### Mencari Jumlah Cluster yang Optimal
```
# Melakukan iterasi jumlah cluster dari 2 hingga 9 untuk mencari nilai cost (distorsi) terkecil
# Cost digunakan untuk mengevaluasi seberapa baik hasil clustering pada data kategorikal dan numerik
cost = {}
for k in range(2, 10):
    kproto = KPrototypes(n_clusters=k, random_state=75)
    kproto.fit_predict(df_model, categorical=[0, 1, 2])  # Menentukan indeks kolom kategorikal
    cost[k] = kproto.cost_

# Memvisualisasikan hasil cost dengan Elbow Plot untuk menentukan jumlah cluster optimal
sns.pointplot(x=list(cost.keys()), y=list(cost.values()))
plt.show()
```

Output:

<img width="548" height="417" alt="image" src="https://github.com/user-attachments/assets/ff89de33-3b82-4b26-9536-6958420aac56" />

Dari hasil tersebut, dapat mengetahui titik siku dari plot tersebut adalah pada saat k = 5. Sehingga kamu memutuskan untuk menggunakan 5 sebagai jumlah cluster optimalnya.

### Membuat Model
Model Kprototypes dengan nilai k = 5 dan random state = 75

```
# Inisialisasi dan training model K-Prototypes dengan 5 cluster
# categorical=[1,2,3] menunjukkan indeks kolom kategorikal dalam dataset
kproto = KPrototypes(n_clusters=5, random_state=75)
kproto = kproto.fit(df_model, categorical=[0, 1, 2])

# Simpan model hasil clustering ke dalam file pickle untuk digunakan kembali
pickle.dump(kproto, open('cluster.pkl', 'wb'))
```
### Menggunakan Model

```
# Menentukan segmen tiap pelanggan
clusters = kproto.predict(df_model, categorical=[0,1,2])
print('Segmen Pelanggan: {}\n'.format(clusters))
# Menggabungkan data awal dan segmen pelanggan
df_final = df.copy()
df_final['cluster'] = clusters
print(df_final.head())
```
Output:

<img width="660" height="250" alt="image" src="https://github.com/user-attachments/assets/954e02cc-689c-4a81-acb4-4f8e37fa93da" />

### Menampilkan Cluster Tiap Pelanggan

```
# Menampilkan data pelanggan berdasarkan cluster nya  
for i in range(0,5):
  print('\nPelanggan Cluster : {}\n'.format(i))
  print(df_final[df_final['cluster']==i])
```
Output:

<img width="794" height="2242" alt="image" src="https://github.com/user-attachments/assets/5e4c3d7e-1f0f-4dc2-b2af-b2f332ce3647" />


### Visualisasi Hasil Clustering - Box Plot

```
# Data Numerical
kolom_numerik = ['Umur', 'NilaiBelanjaSetahun']
for i in kolom_numerik:
  plt.figure(figsize=(6,4))
  ax = sns.boxplot(x='cluster', y=i, data=df_final)
  plt.title('\nBox Plot {}\n'.format(i), fontsize=12)
  plt.show()
```
Output:

<img width="541" height="443" alt="image" src="https://github.com/user-attachments/assets/9fc5219a-f040-4389-8e2d-1a16823c6e00" />

<img width="546" height="443" alt="image" src="https://github.com/user-attachments/assets/d25349e0-041d-4d6f-8b3c-50b3b3d94616" />


### Visualisasi Hasil Clustering - Count Plot

```
# Data Kategorikal
kolom_categorical=['Jenis Kelamin', 'Profesi', 'Tipe Residen']
for i in kolom_categorical:
  plt.figure(figsize=(6,4))
  ax = sns.countplot(data=df_final, x='cluster', hue=i)
  plt.title('\nCount Plot {}\n'.format(i), fontsize=12)
  ax.legend(loc="upper center")
  for p in ax.patches:
    ax.annotate(format(p.get_height(), '.0f'),
                (p.get_x() + p.get_width() / 2., p.get_height()),
                ha = 'center',
                va = 'center',
                xytext = (0,10),
                textcoords = 'offset points')
sns.despine(right='True', top='True',left='True')
ax.axes.yaxis.set_visible(False)
plt.show()
```

Output:

<img width="541" height="443" alt="image" src="https://github.com/user-attachments/assets/1daff144-65d5-4426-b2b4-cd93bb52ada3" />

<img width="541" height="443" alt="image" src="https://github.com/user-attachments/assets/300d76c2-bc11-429f-a80d-05ff6ca2d6a6" />

<img width="484" height="443" alt="image" src="https://github.com/user-attachments/assets/51ca436b-f05f-42b0-88a2-f9a0de09e4af" />

### Menamakan Cluster

Dari hasil observasi yang dilakukan kamu dapat memberikan nama segmen dari tiap tiap nomor klusternya, yaitu:

- Cluster 0: Diamond Young Entrepreneur, isi cluster ini adalah para wiraswasta yang memiliki nilai transaksi rata-rata mendekati 10 juta. Selain itu isi dari cluster ini memiliki umur sekitar 18 - 41 tahun dengan rata-ratanya adalah 29 tahun.
- Cluster 1: Diamond Senior Entrepreneur, isi cluster ini adalah para wiraswasta yang memiliki nilai transaksi rata-rata mendekati 10 juta. Isi dari cluster ini memiliki umur sekitar 45 - 64 tahun dengan rata-ratanya adalah 55 tahun.
- Cluster 2: Silver Students, isi cluster ini adalah para pelajar dan mahasiswa dengan rata-rata umur mereka adalah 16 tahun dan nilai belanja setahun mendekati 3 juta.
- Cluster 3: Gold Young Member, isi cluster ini adalah para profesional dan ibu rumah tangga yang berusia muda dengan rentang umur sekitar 20 - 40 tahun dan dengan rata-rata 30 tahun dan nilai belanja setahunnya mendekati 6 juta.
- Cluster 4: Gold Senior Member, isi cluster ini adalah para profesional dan ibu rumah tangga yang berusia tua dengan rentang umur 46 - 63 tahun dan dengan rata-rata 53 tahun dan nilai belanja setahunnya mendekati 6 juta.

```
# Mapping nama kolom  
df_final['segmen'] = df_final['cluster'].map({  
    0: 'Diamond Young Member',  
    1: 'Diamond Senior Member',  
    2: 'Silver Member',  
    3: 'Gold Young Member',  
    4: 'Gold Senior Member'  
})
print(df_final.info())
print(df_final.head())  
```

Output:

<img width="460" height="428" alt="image" src="https://github.com/user-attachments/assets/3f237c00-bc05-4a94-a4b7-ce4c89786b7f" />


### Kesimpulan
Yeay! Akhirnya kamu sudah berhasil melakukan segmentasi pelanggan dan mendapatkan nama yang cocok untuk masing masing cluster, yaitu:

- Cluster 0: Diamond Young Entrepreneur, isi cluster ini adalah para wiraswasta yang memiliki nilai transaksi rata-rata mendekati 10 juta. Selain itu isi dari cluster ini memiliki umur sekitar 18 - 41 tahun dengan rata-ratanya adalah 29 tahun.
- Cluster 1: Diamond Senior Entrepreneur, isi cluster ini adalah para wiraswasta yang memiliki nilai transaksi rata-rata mendekati 10 juta. Isi dari cluster ini memiliki umur sekitar 45 - 64 tahun dengan rata-ratanya adalah 55 tahun.
- Cluster 2: Silver Students, isi cluster ini adalah para pelajar dan mahasiswa dengan rata-rata umur mereka adalah 16 tahun dan nilai belanja setahun mendekati 3 juta.
- Cluster 3: Gold Young Member, isi cluster ini adalah para profesional dan ibu rumah tangga yang berusia muda dengan rentang umur sekitar 20 - 40 tahun dan dengan rata-rata 30 tahun dan nilai belanja setahunnya mendekati 6 juta.
- Cluster 4: Gold Senior Member, isi cluster ini adalah para profesional dan ibu rumah tangga yang berusia tua dengan rentang umur 46 - 63 tahun dan dengan rata-rata 53 tahun dan nilai belanja setahunnya mendekati 6 juta.

## Mengoperasikan Model

```
# Data Baru  
data = [{  
    'Customer_ID': 'CUST-100' ,  
    'Nama Pelanggan': 'Joko' ,  
    'Jenis Kelamin': 'Pria',  
    'Umur': 45,  
    'Profesi': 'Wiraswasta',  
    'Tipe Residen': 'Cluster' ,  
    'NilaiBelanjaSetahun': 8230000  
      
}]  
  
# Membuat Data Frame  
new_df = pd.DataFrame(data)  
  
# Melihat Data  
print(new_df) 
```

Output:

<img width="479" height="78" alt="image" src="https://github.com/user-attachments/assets/99a99bc8-7863-4118-a3cd-6e971be47e3a" />


### Membuat Fungsi Data Pemrosesan

Jadi fungsi ini nantinya akan bisa digunakan untuk:

***Melakukan konversi data kategorikal menjadi numerik***

Dari proses sebelumnya kita tau representasi tiap kode dan maksudnya yaitu:

**Jenis Kelamin**

- 0 : Pria
- 1 : Wanita

**Profesi**

- 0 : Ibu Rumah Tangga
- 1 : Mahasiswa
- 2 : Pelajar
- 3 : Professional
- 4 : Wiraswasta

**Tipe Residen**

- 1 : Sector
- 0 : Cluster

Selanjutnya kita harus membuat fungsi untuk merubah data kategorikal menjadi numerik berdasarkan referensi tersebut.

***Melakukan standardisasi kolom numerikal***

Untuk melakukan standardisasi dengan variable yang sama pada saat permodelan kita perlu menggunakan nilai rata-rata dan standard deviasi dari tiap variabel pada saat kita melakukan permodelan, yaitu:

**Umur**

- Rata - rata: 37.5
- Standard Deviasi: 14.7

**NilaiBelanjaSetahun**

- Rata - rata: 7069874.8
- Standard Deviasi: 2590619.0
  
Dari nilai-nilai tersebut kita dapat menghitung nilai standardisasi (Z) dengan menggunakan rumus Z = (x - u)/s dengan x adalah tiap nilai, u adalah rata-rata dan s adalah standard deviasi.

***Menggabungkan hasil dua proses sebelumnya menjadi satu dataframe***

```
def data_preprocess(data):  
    # Konversi Kategorikal data  
    kolom_kategorikal = ['Jenis Kelamin','Profesi','Tipe Residen']  
      
    df_encode = data[kolom_kategorikal].copy()  
  
    ## Jenis Kelamin   
    df_encode['Jenis Kelamin'] = df_encode['Jenis Kelamin'].map({  
        'Pria': 0,  
        'Wanita' : 1  
    })  
      
    ## Profesi  
    df_encode['Profesi'] = df_encode['Profesi'].map({  
        'Ibu Rumah Tangga': 0,  
        'Mahasiswa' : 1,  
        'Pelajar': 2,  
        'Professional': 3,  
        'Wiraswasta': 4  
    })  
      
    ## Tipe Residen  
    df_encode['Tipe Residen'] = df_encode['Tipe Residen'].map({  
        'Cluster': 0,  
        'Sector' : 1  
    })  
      
    # Standardisasi Numerical Data  
    kolom_numerik  = ['Umur','NilaiBelanjaSetahun']  
    df_std = data[kolom_numerik].copy()  
      
    ## Standardisasi Kolom Umur  
    df_std['Umur'] = (df_std['Umur'] - 37.5)/14.7  
      
    ## Standardisasi Kolom Nilai Belanja Setahun  
    df_std['NilaiBelanjaSetahun'] = (df_std['NilaiBelanjaSetahun'] - 7069874.8)/2590619.0  
      
    # Menggabungkan Kategorikal dan numerikal data  
    df_model = df_encode.merge(df_std, left_index = True,  
                           right_index=True, how = 'left')  
      
    return df_model  
  
# Menjalankan fungsi  
new_df_model = data_preprocess(new_df)  
  
print(new_df_model) 

```

Output:

<img width="448" height="40" alt="image" src="https://github.com/user-attachments/assets/ba4372fa-8190-41c4-b525-705771dc4238" />

### Memanggil Model dan Melakukan Prediksi

```
def modelling (data):  
      
    # Memanggil Model  
    kpoto = pickle.load(open('cluster.pkl', 'rb'))  
      
    # Melakukan Prediksi  
    clusters  = kpoto.predict(data,categorical=[0,1,2])  
      
    return clusters  
  
# Menjalankan Fungsi  
clusters = modelling(new_df_model)  
  
print(clusters) 
```
Output:

<img width="199" height="22" alt="image" src="https://github.com/user-attachments/assets/9084cc67-fd32-46aa-ad81-387f996b01ba" />

Cluster yang ditampilkan adalah 1, yaitu Diamond Senior Entrepreneur.

### Menamakan Segmen

```
def menamakan_segmen (data_asli, clusters):  
      
    # Menggabungkan cluster dan data asli  
    final_df  = data_asli.copy()  
    final_df['cluster'] = clusters
      
    # Menamakan segmen  
    final_df['segmen'] = final_df['cluster'].map({  
        0: 'Diamond Young Member',  
        1: 'Diamond Senior Member',  
        2: 'Silver Students',  
        3: 'Gold Young Member',  
        4: 'Gold Senior Member'  
    })  
      
    return final_df
  
# Menjalankan Fungsi  
new_final_df = menamakan_segmen (new_df,clusters)  
  
print(new_final_df)  
```

Output:

<img width="488" height="79" alt="image" src="https://github.com/user-attachments/assets/41052e64-badd-4bbf-9a2e-643d69c591f5" />

