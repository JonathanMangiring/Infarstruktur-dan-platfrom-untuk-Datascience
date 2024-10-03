# Infarstruktur-dan-platfrom-untuk-Datascience
EDA pada Statistik Spotify, yang mencakup

1. Memuat Data
2. Informasi Dasar
3. Duplikat atau Nilai Unik
4. Visualisasi Nilai Unik
5. Menemukan Nilai Null
6. Mengganti Nilai Null (Jika Ditemukan)
7. Mengganti Nilai Null (Jika Ditemukan)
8. Mengetahui jenis data dari dataset untuk mempermudah proses
9. Memfilter Data
10. Membuat Box Plot
11. Korelasi

## 1. Menampilkan Data
```py
import pandas as pd

df = pd.read_csv('spotify_data.csv')
df
```
![image](https://github.com/user-attachments/assets/ab73ade7-54cc-4acb-a37b-bc20a066624f)

Data yang ditampilkan memiliki total 2500 baris, yang berarti ada 2500 lagu yang tercantum. Data ini disusun dalam format CSV dan diakses menggunakan library pandas di Python.

## 2. Informasi Dasar
Gambar tersebut menampilkan data dalam bentuk tabel yang terdiri dari tiga kolom: Songs & Artist, Streams, dan Daily.

Berikut adalah penjelasan mengenai kolom-kolom tersebut:

- Songs & Artist: Berisi nama lagu dan artis yang membawakan lagu tersebut. Contoh: "The Weeknd Blinding Lights", "Ed Sheeran - Shape of You".
- Streams: Menunjukkan total jumlah streaming atau jumlah kali lagu tersebut telah diputar di platform Spotify, dinyatakan dalam angka besar.
- Daily: Menunjukkan rata-rata streaming harian atau jumlah putaran lagu tersebut per hari.

Melihat informasi dataset, Seperti nama column, Non-Null count, dan Tipedata
```py
df.info()
```
![image](https://github.com/user-attachments/assets/f71d2c05-561c-4477-90bb-1ebb29299b30)

Melihat Statistika deskriptif
```py
df.describe()
```
![image](https://github.com/user-attachments/assets/fc0d5fea-ecb6-423a-bdf4-db9a3321dbea)

Fungsi df.describe() pada Pandas digunakan untuk memberikan ringkasan statistik dari data numerik dalam DataFrame. Ketika dipanggil pada DataFrame seperti contoh data Spotify tersebut, hasilnya akan menampilkan statistik dasar untuk kolom numerik seperti Streams dan Daily. Statistik yang dihasilkan biasanya meliputi:

- count: Jumlah total data non-null dalam kolom.
- mean: Nilai rata-rata dari kolom.
- std: Standar deviasi, yang menunjukkan seberapa tersebar data dari rata-ratanya.
- min: Nilai minimum dalam kolom.
- 25%: Kuartil pertama (nilai di mana 25% dari data lebih rendah).
- 50% (median): Nilai tengah dari data (50% dari data berada di atas dan di bawah nilai ini).
- 75%: Kuartil ketiga (nilai di mana 75% dari data lebih rendah).
- max: Nilai maksimum dalam kolom.

Sebelum mencari nilai duplikat atau unik kita lakukan Datacleansing terlebih dahulu
```py
df.isnull().sum()
```
![image](https://github.com/user-attachments/assets/688caf7f-299d-474a-ac25-6571a65af974)

Terdapat 2 missing value pada column Daily, lalu kita isi 2 data kosong menggunakan rata-rata nilai daily
```py
mean = df['Daily'].mean()
df['Daily'] = df['Daily'].fillna(mean)
```
![image](https://github.com/user-attachments/assets/94162156-7965-43f9-9af8-d7156e93196f)

## 3. Duplikat atau nilai unik
Mencari nilai unik atau duplikat
```py
#Columns Streams
stream = df['Streams'].duplicated().sum()
display(stream)

#Columns daily
daily = df['Daily'].duplicated().sum()
display(daily)
```
![image](https://github.com/user-attachments/assets/068ccddc-5ec2-4540-a14a-5764b6103c12)

Pada Columns stream terdapat 2 data duplikat, sedangkan pada columns daily terdapat 5 data duplikat

```py
df_duplicate = df.duplicated().sum()
df_unique = df.nunique()
print(df_unique)
```

- `df_duplicate = df.duplicated().sum()` Kode ini digunakan untuk menghitung jumlah baris yang duplikat (sama) dalam dataframe df.

  `df.duplicated()`: Fungsi ini akan mengembalikan sebuah series boolean yang menunjukkan apakah setiap baris dalam dataframe df adalah duplikat atau tidak. Jika sebuah baris adalah duplikat, maka nilai dalam series ini akan menjadi True.

  `sum()`: Fungsi ini akan menghitung jumlah nilai True dalam series boolean yang dihasilkan oleh `df.duplicated()`. Jadi, jika ada 5 baris yang duplikat, maka hasilnya akan menjadi 5.
Jadi, df_duplicate akan menyimpan jumlah baris yang duplikat dalam dataframe df.

- `df_unique = df.nunique()` Kode ini digunakan untuk menghitung jumlah nilai unik (tidak duplikat) dalam setiap kolom dalam dataframe df.

  `df.nunique()`: Fungsi ini akan mengembalikan sebuah series yang menunjukkan jumlah nilai unik dalam setiap kolom dalam dataframe df. Jadi, `df_unique` akan menyimpan jumlah nilai unik dalam setiap kolom dalam dataframe df.

Kode tersebut menghasilkan output :

![image](https://github.com/user-attachments/assets/8cace4f1-1636-4d5c-9c79-a75fb4ff96b3)

Dari 2500 Jumlah data terdapat 
Songs & Artist    2474,
Streams           2498,
Daily             2495,
yang tidak duplikat.

## 4. Visualisasi nilai unik
Menggunakan matplotlib sebagai plt.
```py
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(10, 5))
df_unique.plot(kind='bar', color='skyblue')
plt.title("Jumlah Nilai Unik di Setiap Kolom")
plt.ylabel("Jumlah Nilai Unik")
plt.ylim(2400, 2500)
plt.xticks(rotation=45)
plt.show()
```
- `plt.figure(figsize=(10, 5))` Kode ini digunakan untuk membuat sebuah figure (gambar) dengan ukuran tertentu.
  `plt.figure()`: Fungsi ini akan membuat sebuah figure baru.
  `figsize=(10, 5):` Parameter ini akan menentukan ukuran figure tersebut, yaitu 10 inci lebar dan 5 inci tinggi.
- `df_unique.plot(kind='bar', color='skyblue')` Kode ini digunakan untuk membuat sebuah plot (grafik) dari data df_unique.
  `df_unique.plot()`: Fungsi ini akan membuat sebuah plot dari data df_unique.
  `kind='bar'`: Parameter ini akan menentukan jenis plot yang akan dibuat, yaitu plot bar (grafik batang).
  `color='skyblue'`: Parameter ini akan menentukan warna plot tersebut, yaitu biru langit (skyblue).
- `plt.title()`: Fungsi ini akan menambahkan judul pada plot.
- `plt.ylabel()`: Fungsi ini akan menambahkan label pada sumbu Y.
- `plt.ylim()`: Fungsi ini akan menentukan batas sumbu Y.
- `plt.xticks()`: Fungsi ini akan memutar label pada sumbu X.
- `plt.show()`: Fungsi ini akan menampilkan plot.

![image](https://github.com/user-attachments/assets/06501ba4-a577-41d5-b22c-7acb7e36fb53)

## 8. Mengetahui jenis data dari dataset untuk mempermudah proses
Sebelum kita membuat visualisasi, kita perlu mengetahui jenis data apa yang tersedia di dataset. Kita bisa mengetahuinya dengan menggunakan `df.dtypes`

![image](https://github.com/user-attachments/assets/a7a8ca49-4e3e-4abb-9bfe-726f39e49ea4)

## 10. Membuat box plot
```py
plt.figure(figsize=(8, 6))
sns.boxplot(data=df, y='Streams')
plt.title('Boxplot of Streams')
plt.show()
```

- `sns.boxplot()`: Fungsi ini akan membuat sebuah plot boxplot.
  `data=df`: Parameter ini akan menentukan data yang akan digunakan untuk membuat plot, yaitu dataframe df.
  `y='Streams'`: Parameter ini akan menentukan kolom dalam dataframe df yang akan digunakan untuk membuat plot, yaitu kolom 'Streams'.

![image](https://github.com/user-attachments/assets/21189a4b-1529-462d-93e6-37cd45a9f834)

Boxplot di atas menunjukkan distribusi data untuk kolom Streams, dan tampaknya ada beberapa outlier (ditandai dengan titik-titik di luar batas wajar).

Selanjutnya, kita akan menangani outlier ini menggunakan metode IQR (Interquartile Range), dengan menghapus nilai-nilai yang berada di luar batas wajar.
```py
# Menghitung IQR dan menentukan batas bawah dan atas untuk deteksi outlier
Q1 = df['Streams'].quantile(0.25)
Q3 = df['Streams'].quantile(0.75)
IQR = Q3 - Q1

# Menentukan batas bawah dan atas untuk deteksi outlie
batas_bawah = Q1 - 1.5 * IQR
batas_atas = Q3 + 1.5 * IQR

# Menghapus outliers
df_clean = df[(df['Streams'] >= batas_bawah) & (df['Streams'] <= batas_atas)]

# Membuat boxplot lagi setelah penghapusan outlier
plt.figure(figsize=(8, 6))
sns.boxplot(data=df_clean, y='Streams')
plt.title('Boxplot of Streams after Outlier Removal')
plt.show()
```
- Menghitung IQR dan menentukan batas bawah dan atas untuk deteksi outlier
  
  `Q1 = df['Streams'].quantile(0.25)`: Kode ini menghitung kuartil pertama (Q1) dari data dalam kolom 'Streams' dari dataframe df. Q1 adalah nilai yang membagi data menjadi dua bagian yang sama besar, dengan 25% data di bawah Q1 dan 75% data di atas Q1.
  
  `Q3 = df['Streams'].quantile(0.75)`: Kode ini menghitung kuartil ketiga (Q3) dari data dalam kolom 'Streams' dari dataframe df. Q3 adalah nilai yang membagi data menjadi dua bagian yang sama besar, dengan 75% data di bawah Q3 dan 25% data di atas Q3.

  `IQR = Q3 - Q1`: Kode ini menghitung rentang antar kuartil (IQR) dari data dalam kolom 'Streams' dari dataframe df. IQR adalah perbedaan antara Q3 dan Q1.

- Menentukan batas bawah dan atas untuk deteksi outlier

  `lower_bound = Q1 - 1.5 * IQR`: Kode ini menentukan batas bawah untuk deteksi outlier. Batas bawah ini adalah Q1 minus 1,5 kali IQR.

   `upper_bound = Q3 + 1.5 * IQR`: Kode ini menentukan batas atas untuk deteksi outlier. Batas atas ini adalah Q3 plus 1,5 kali IQR.

- Menghapus outlier
  
  `df_clean = df[(df['Streams'] >= lower_bound) & (df['Streams'] <= upper_bound)]`: Kode ini menghapus outlier dari data dalam kolom 'Streams' dari dataframe df. Data yang dihapus adalah data yang memiliki nilai di luar batas bawah dan atas yang telah ditentukan.

- Membuat boxplot lagi setelah penghapusan outlier

  `plt.figure(figsize=(8, 6))`: Kode ini membuat sebuah figure dengan ukuran 8x6 inci.

  `sns.boxplot(data=df_clean, y='Streams')`: Kode ini membuat sebuah boxplot dari data dalam kolom 'Streams' dari dataframe df_clean.

  `plt.title('Boxplot of Streams after Outlier Removal')`: Kode ini menambahkan judul pada boxplot.

  `plt.show()`: Kode ini menampilkan boxplot.


  




