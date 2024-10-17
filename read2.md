## Eksplorasi Mandiri

1. Summary Statistic dari Data
2. Eksplorasi Data, sajikan data yang mungkin dalam beragam diagram chart
3. Lakukan Uji Korelasi

## 1. Summary Statistic Data
```py
print(f"Data tersebut memeiliki frekuensi sebanyak {len(data)}\n")
kolom=data.columns.to_list()
for y in kolom:
    if data[y].dtype == 'O':
        continue
    print(f'Dari kolom {y} nilai mean (rata-rata)',(data[y].mean()))
    print(f'Dari kolom {y} nilai median nya',(data[y].median()))
    print(f'Dari kolom {y} nilai modus nya',(data[y].mode()[0]))
    print(f'Dari kolom {y} nilai varians nya',(data[y].std(ddof=1)))
    print(f'Dari kolom {y} nilai standar deviasi nya',(data[y].var(ddof=1)),'\n')
```

![image](https://github.com/user-attachments/assets/ee81e7e5-41b2-4980-8e72-e60c8334220c)

Sebelum melakukan looping kita pisahkan terlebih dahulu kolom yang tipenya 'Object' karena kolom tersebut tidak memiliki mean, standar deviasi, dan lain-lain. Dengan menggunakan looping kita dapat mengeluarkan nilai yang dicari dari semua kolom yang ada di data secara berurutan

## 2. Eksplorasi Data
Setelah kita mengetahui informasi statistika deskriptifnya kita sekarang dapat mencari nilai outlier, cara mencari outlier terdapat dua cara yaitu:

### A.IQR
IQR sedikit lebih tahan dengan outlier daripada Z Score dikarenakan tidak menggunakan outlier untuk mencari nilai IQR

```py
for i in kolom:
    if data[i].dtype == 'O':
        continue
    Q1 = data[i].quantile(0.25)
    Q3 = data[i].quantile(0.75)
    IQR = Q3 - Q1
    
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    outliers_iqr = data[(data[i] < lower_bound) | (data[i] > upper_bound)]
    print(f'Lower bound {lower_bound} dan upper bound {upper_bound} di kolom {i}')
    display(f'Outlier dengan iqr di kolom {i}',outliers_iqr)
```

Sama seperti sebelum-sebelumnya lagi kita melakukan looping untuk memisahkan terlebih dahulu kolom yang tipenya `'Object'`. Kode tersebut menggunakan beberapa fungsi yang sudah ada didalam library pandas seperti `.quantile()` yang dimana digunakan untuk mencari quartil dari data tersebut, kemudian dari hasil quartil 3 dan quartil 1 kita kurangkan dan akan mendapatkan nilai IQR. Setelah kita mendapatkan nilai IQR kita dapat menetapkan `lower_bound`, dan `upper_bound`

![image](https://github.com/user-attachments/assets/c907b8da-05ad-4592-be41-81dd971d24bc)

![image](https://github.com/user-attachments/assets/74898491-059c-41fe-8db9-3de842d2b429)

IQR (Interquartile Range) pada dua kolom: "Streams" dan "Daily". Batas bawah dan atas IQR dihitung untuk mendeteksi outlier. Pada kolom "Streams", batas bawah adalah -120548177.75 dan batas atas adalah 1827334148.25, sedangkan pada kolom "Daily", batas bawah adalah -386425.25 dan batas atas 1242744.75. Baris yang berada di luar batas-batas tersebut dianggap sebagai outlier, yang kemudian ditandai dalam tabel berdasarkan nilai z-score untuk masing-masing kolom. 

## Visualisasi
```py
kolom_numerik=[]
for i in kolom:
    if data[i].dtype == 'O':
        continue
    kolom_numerik.append(i)

fig,ax=plt.subplots(nrows=1,ncols=5,figsize=(15,8))

for i,var in enumerate(kolom_numerik):
    sns.boxplot(y=data[var], ax=ax[i])
    ax[i].set_title(f'Boxplot dari data {var}')

plt.tight_layout()
plt.show()
```

![image](https://github.com/user-attachments/assets/d61f1059-376f-4183-aa48-3d6d57dadebf)


- `for i, var in enumerate(kolom_numerik):` kode ini digunakan untuk memanggil setiap nilai dan setiap index dari nilai yang dipanggil contoh: `0 MPG` 0 sebagai i dan MPG sebagai var
- `sns.boxplot(y=data[var], ax=ax[i])` kode yang di loop ini berguna untuk membuat masing-masing box plot. `y=data[var]` digunakan untuk memasukkan data yang ingin dibuat box plot di sumbu y kita bisa menggunakan `x=data[var` tetapi nantinya hasil dari box plot akan menjadi horizontal, lalu fungsi dari `ax=ax[i]` digunakan untuk menempatkan boxplot yang telah dibuat di axis sublot yang kita buat diatas
- `ax[i].set_title(f'Boxplot dari data {var}')` kode ini digunakan untuk memberikan judul dari setiap boxplot yang kita buat dengan nama kolom yang sesuai



## Z score
Jika kita menggunakan Z Score, Z Score sedikit kurang kebal dengan outlier, dengan arti karena Z Score didapatkan dengan menggunakan standar deviasi dan rata-rata yang dimana outlier tersebut juga ikut serta untuk menghasilkan Z Score.

```py
for i in kolom:
    if data[i].dtype == 'O':
        continue
    data[f'z_score {i}'] = stats.zscore(data[i])
    outliers_zscore = data[data[f'z_score {i}'].abs() > 3]
    display(f'Outlier z-score dari {i}',outliers_zscore)
```

![image](https://github.com/user-attachments/assets/cad7ba8b-7bf0-467f-881b-0c8e6a33e1a5)

# 3. Uji korelasi
Uji korelasi dengan pearson

```pyimport numpy as np
import pandas as pd
from scipy.stats import pearsonr

def korelasi_pearson_aman(x, y):
    """Melakukan korelasi Pearson dengan aman, menangani NaN dan nilai tak hingga."""
    x = np.asarray(x)
    y = np.asarray(y)

    if np.isnan(x).any() or np.isnan(y).any():
        return 'Error: Data mengandung nilai NaN. Harap bersihkan data.'
    if not np.isfinite(x).all() or not np.isfinite(y).all():
        return 'Error: Data mengandung nilai tak hingga. Harap bersihkan data.'
    
    return pearsonr(x, y)

def tingkat_korelasi(hasil_pearson):
    """Mengkategorikan kekuatan korelasi."""
    korelasi = hasil_pearson[0]
    if korelasi > 0.8:
        return "Sangat kuat"
    elif 0.6 < korelasi <= 0.8:
        return "Kuat"
    elif 0.4 < korelasi <= 0.6:
        return "Sedang"
    elif 0.2 < korelasi <= 0.4:
        return "Lemah"
    else:
        return "Sangat lemah"

# --- Pembersihan Data ---
# Mengganti nilai tak hingga dengan NaN
data.replace([np.inf, -np.inf], np.nan, inplace=True)

# Menghapus baris dengan nilai NaN di kolom 'Streams' dan 'Daily'
data.dropna(subset=['Streams', 'Daily'], inplace=True)
# --- Akhir Pembersihan Data ---

# Menghitung korelasi Pearson menggunakan fungsi yang aman
hasil1 = korelasi_pearson_aman(data['Streams'], data['Daily'])

# Menampilkan hasil dengan penanganan kesalahan dan kesimpulan
if isinstance(hasil1, str):
    print(hasil1)
else:
    tingkat_korelasi1 = tingkat_korelasi(hasil1)
    print(f"Korelasi antara kolom 'Streams' dan 'Daily' adalah {tingkat_korelasi1}")

    # --- Kesimpulan ---
    print("\nKesimpulan:")
    if tingkat_korelasi1 == "Sangat kuat" or tingkat_korelasi1 == "Kuat":
        print("Terdapat korelasi yang kuat antara jumlah streaming dan jumlah pendengar harian.")
        print("Ini menunjukkan bahwa lagu-lagu dengan jumlah streaming yang tinggi cenderung memiliki jumlah pendengar harian yang tinggi pula.")
    elif tingkat_korelasi1 == "Sedang":
        print("Terdapat korelasi moderat antara jumlah streaming dan jumlah pendengar harian.")
        print("Hubungan antara kedua variabel ini cukup terlihat, tetapi tidak sekuat korelasi yang kuat.")
    else:
        print("Korelasi antara jumlah streaming dan jumlah pendengar harian lemah atau sangat lemah.")
        print("Ini menunjukkan bahwa jumlah streaming tidak memiliki pengaruh yang signifikan terhadap jumlah pendengar harian.")
    # --- Akhir Kesimpulan ---
```
![image](https://github.com/user-attachments/assets/fa1f9fe6-46c4-4f62-b1a2-eda3a4e5810d)

Kesimpulan:
Terdapat korelasi moderat antara jumlah streaming dan jumlah pendengar harian.
Hubungan antara kedua variabel ini cukup terlihat, tetapi tidak sekuat korelasi yang kuat.

