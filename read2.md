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


