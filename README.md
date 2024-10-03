# Infarstruktur-dan-platfrom-untuk-Datascience
EDA pada Statistik Spotify, yang mencakup

1. Menampilkan Data
2. Informasi Dasar
3. Duplikat atau Nilai Unik
4. Visualisasi Nilai Unik
5. Mengetahui jenis data dari dataset untuk mempermudah proses
6. Memfilter Data
7. Membuat Box Plot
8. Korelasi

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

## 3. Duplikat atau nilai unik
Mencari nilai unik atau duplikat
```py


