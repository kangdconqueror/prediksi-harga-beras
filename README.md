
# Laporan Proyek Machine Learning - Ma'shum Abdul Jabbar

## Domain Proyek

- Proyek ini berfokus pada peramalan harga beras premium dan medium di Indonesia menggunakan machine learning. Masalah fluktuasi harga beras menjadi isu strategis karena mempengaruhi stabilitas ekonomi dan kesejahteraan masyarakat. Proyek ini bertujuan untuk melakukan analisis prediktif terhadap harga beras premium dan medium di Kabupaten Bogor menggunakan data historis harga harian. Masalah ini perlu diselesaikan untuk membantu pemerintah dan pelaku bisnis dalam pengambilan keputusan terkait tren kenaikan harga beras.
- Masalah fluktuasi harga beras menjadi isu strategis karena memengaruhi stabilitas ekonomi dan kesejahteraan masyarakat. Penelitian sebelumnya menunjukkan bahwa harga beras di Indonesia cenderung meningkat setiap tahunnya, dengan prediksi bahwa harga beras nasional akan mencapai Rp14.924 pada Desember 2024 akibat kondisi alam seperti El Niño (Naya et al., 2024). Namun, penelitian lain menunjukkan bahwa di tingkat lokal, seperti di Kabupaten Kebumen, harga premium beras dapat menurun selama puncak panen raya pada akhir tahun 2024 (Fitra & Prasada, 2024).  Hal ini menunjukkan bahwa tren harga beras memiliki variasi regional yang perlu dianalisis lebih lanjut untuk mendukung kebijakan strategis yang relevan.

---

## Business Understanding

### Problem Statements
- Bagaimana memprediksi harga beras premium dan medium berdasarkan data historis?
- Faktor apa saja yang memengaruhi akurasi prediksi harga beras?

### Goals
- Menghasilkan model prediktif dengan akurasi tinggi untuk meramalkan harga beras.
- Memberikan wawasan terkait pola harga berdasarkan analisis data.

### Solution Statements
- Menggunakan algoritma **Random Forest Regressor** untuk membangun model prediksi.
- Melakukan hyperparameter tuning untuk meningkatkan performa model.
- Menganalisis fitur yang paling berpengaruh terhadap prediksi harga beras menggunakan feature importance dari model Random Forest.

---

## Data Understanding

### Deskripsi Data
Data yang digunakan dalam proyek ini berasal dari **Badan Pangan Nasional**: [https://badanpangan.go.id/](https://badanpangan.go.id/). Dataset mencakup informasi harga harian beras premium dan medium di Kabupaten Bogor untuk periode tahun 2022 hingga 2024.

### Jumlah Data
Dataset terdiri dari **2.106 baris** dan **5 kolom**, dengan rincian berikut:

| #   | Kolom      | Non-Null Count | Dtype           | Deskripsi                                                                                                                                                   |
|-----|------------|----------------|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1   | `komoditas`| 2,106          | `object`        | Menyimpan nama komoditas seperti "Beras Premium" atau "Beras Medium".                                                                                     |
| 2   | `tanggal`  | 2,106          | `datetime64[ns]`| Menyimpan tanggal spesifik dengan format `YYYY-MM-DD` (misalnya `2022-01-01`). Kolom ini digunakan untuk analisis berbasis waktu.                        |
| 3   | `harga`    | 2,106          | `int64`         | Harga per kilogram untuk setiap komoditas pada tanggal tertentu, dalam satuan Rupiah.                                                                    |
| 4   | `tahun`    | 2,106          | `int32`         | Informasi tahun yang dihasilkan dari kolom `tanggal`.                                                                                                    |
| 5   | `bulan`    | 2,106          | `int32`         | Informasi bulan (1-12) yang diekstrak dari kolom `tanggal`.                                                                                              |

### Kondisi Data
1. **Missing Values**:  
   Tidak ada nilai kosong (`NaN`) dalam dataset. Semua kolom memiliki jumlah nilai yang lengkap.

2. **Duplikat**:  
   Tidak ditemukan data duplikat setelah dilakukan pengecekan dengan metode `.duplicated()`.

3. **Outlier**:  
   - Harga beras diperiksa menggunakan metode statistik (IQR). Beberapa harga ekstrem ditemukan tetapi relevan dengan konteks pasar (bukan anomali data).  
   - Tidak dilakukan penghapusan nilai ekstrem, karena merupakan bagian dari fenomena yang diteliti.

### Uraian Fitur
1. **`komoditas`**  
   - **Tipe**: `object`  
   - **Deskripsi**: Menyimpan nama jenis komoditas, seperti "Beras Premium" atau "Beras Medium".

2. **`tanggal`**  
   - **Tipe**: `datetime64[ns]`  
   - **Deskripsi**: Tanggal spesifik harga dicatat, digunakan untuk analisis berbasis waktu.

3. **`harga`**  
   - **Tipe**: `int64`  
   - **Deskripsi**: Harga beras (dalam Rupiah) per kilogram untuk setiap jenis komoditas pada tanggal tertentu.

4. **`tahun`**  
   - **Tipe**: `int32`  
   - **Deskripsi**: Informasi tahun yang diekstrak dari kolom `tanggal`.

5. **`bulan`**  
   - **Tipe**: `int32`  
   - **Deskripsi**: Informasi bulan (1-12) yang diekstrak dari kolom `tanggal`, untuk analisis musiman.

### Exploratory Data Analysis
1. **Analisis Pergerakan Harga Beras Premium dan Medium Per Tahun**  
   Tren harga beras dari tahun 2022 hingga 2024 divisualisasikan.  
   ![Trend Harga Per Tahun](https://github.com/kangdconqueror/prediksi-harga-beras/blob/main/1.png?raw=true)

2. **Perbandingan Harga Beras Premium dan Medium Per Bulan**  
   Grafik memperlihatkan perbedaan harga antar jenis beras untuk setiap bulan.  
   ![Perbandingan Harga Per Bulan](https://github.com/kangdconqueror/prediksi-harga-beras/blob/main/2.png?raw=true)

3. **Menentukan pada bulan berapa harga beras tertinggi** 
     - **Premium**: Maret 2024  
     - **Medium**: Maret 2024  
	 
4. **Menentukan pada bulan berapa harga beras terendah** 
     - **Premium**: Januari 2022  
     - **Medium**: Januari 2022  

5. **Melihat Pola Perubahan Harga beras (2022-2024)**  
   Perubahan harga beras dari tahun ke tahun divisualisasikan untuk memahami pola musiman dan tren.  
   ![Pola Perubahan Harga](https://github.com/kangdconqueror/prediksi-harga-beras/blob/main/3.png?raw=true)

6. **Pada Tanggal Berapa Saja Harga Beras Tertinggi dan Terendah**
   Tabel Harga Tertinggi

| Komoditas       | Tanggal Mulai      | Tanggal Selesai    | Harga  | Total Hari |
|------------------|--------------------|--------------------|--------|------------|
| Beras Premium    | 2024-02-19        | 2024-03-04        | 17,000 | 15         |
| Beras Medium     | 2024-02-12        | 2024-04-01        | 15,000 | 49         |

   Tabel Harga Terendah

| Komoditas       | Tanggal Mulai      | Tanggal Selesai    | Harga  | Total Hari |
|------------------|--------------------|--------------------|--------|------------|
| Beras Premium    | 2022-01-01        | 2023-01-14        | 12,000 | 313        |
| Beras Medium     | 2022-01-01        | 2022-12-17        | 10,000 | 290        |
---

## Data Preparation

Tahapan data preparation yang dilakukan meliputi:
1. **Handling Missing Values**: Mengisi nilai yang hilang dengan metode interpolasi.
2. **Scaling**: Normalisasi data harga untuk mempercepat konvergensi model.
3. **Feature Engineering**: Menambahkan fitur seperti rata-rata harga mingguan.

- Tahap Data Preparation adalah langkah penting dalam proses machine learning karena kualitas data sangat memengaruhi performa model.
- Visualisasi data setelah proses cleaning.

---

## Model Development

### Algoritma yang Digunakan: Random Forest Regressor
Random Forest Regressor adalah algoritma ensemble learning berbasis pohon keputusan (_decision tree_). Model ini menggabungkan prediksi dari banyak pohon keputusan untuk meningkatkan akurasi dan mengurangi risiko overfitting.

#### Cara Kerja:
1. **Bagging**: Model membangun beberapa pohon keputusan menggunakan subset data yang diambil secara acak (_bootstrap sampling_). 
2. **Prediksi Akhir**: Hasil prediksi adalah rata-rata dari semua pohon keputusan (untuk masalah regresi).
3. **Feature Importance**: Model menghitung seberapa besar kontribusi tiap fitur dalam proses prediksi, membantu analisis lebih lanjut.

#### Keunggulan:
- Mampu menangani data dengan hubungan non-linear.
- Robust terhadap overfitting karena proses _bagging_.
- Tidak memerlukan asumsi distribusi data.
- Efektif untuk dataset dengan banyak fitur, bahkan jika beberapa fitur kurang relevan.

#### Parameter yang Digunakan:
- `max_depth=10`: Membatasi kedalaman pohon untuk menghindari overfitting.
- `max_features='sqrt'`: Menentukan jumlah maksimum fitur yang dipertimbangkan dalam setiap split.
- `min_samples_leaf=4`: Menetapkan jumlah minimum sampel di setiap daun pohon.
- `min_samples_split=10`: Menentukan jumlah minimum sampel yang diperlukan untuk melakukan split.
- `random_state=42`: Menjamin hasil yang konsisten dengan mengatur seed random.

---

## Modeling

Model yang digunakan: **Random Forest Regressor**

### Parameter Utama:
- **RandomForestRegressor**: `max_depth=10`, `max_features='sqrt'`,  `min_samples_leaf=4`, `min_samples_split=10`, `random_state=42`

- Pemilihan Random Forest Regressor dalam proyek ini didasarkan pada beberapa alasan yang relevan dengan kebutuhan analisis dan sifat data.
- Data sering kali mengandung hubungan non-linear atau interaksi yang sulit ditangkap oleh model linear biasa. Random Forest sangat baik dalam menangani kompleksitas ini karena menggunakan banyak _decision trees_.
- Overfitting adalah masalah umum dalam machine learning, terutama jika model terlalu rumit, model ini lebih tahan terhadap Overfitting,
- Dalam beberapa kasus, dataset memiliki banyak fitur, dan tidak semua fitur tersebut relevan. Secara otomatis model ini menghitung tingkat kepentingan fitur (_feature importance_), sehingga membantu mengidentifikasi fitur yang paling relevan.
- Robust terhadap Data yang Hilang atau Outlier.
- Fleksibilitas dalam Hyperparameter Tuning
- Hyperparameter tuning dilakukan dengan GridSearchCV.
- Pengaturan hyperparameter yang optimal diperlukan untuk meningkatkan akurasi prediksi dan mencegah overfitting atau underfitting.

---

## Evaluation

### Metrik Evaluasi:
Model dievaluasi menggunakan metrik **Root Mean Square Error (RMSE)**, yang mengukur penyimpangan antara nilai aktual dan prediksi. Hasil evaluasi adalah sebagai berikut:
- **RMSE Beras Premium**: 1602.84
- **RMSE Beras Medium**: 1646.84

### Analisis Hasil Evaluasi:
1. **Akurasi Model**  
   Nilai RMSE menunjukkan bahwa prediksi model untuk harga beras premium memiliki rata-rata kesalahan sebesar ±Rp 1,602.84, sedangkan untuk harga beras medium sebesar ±Rp 1,646.84. Nilai ini cukup rendah dibandingkan dengan fluktuasi harga rata-rata harian yang terdapat pada dataset, sehingga model dianggap mampu memberikan prediksi yang akurat.

2. **Pencapaian Problem Statements**  
   - **Bagaimana memprediksi harga beras premium dan medium berdasarkan data historis?**  
     Model berhasil memberikan prediksi harga berdasarkan data historis dengan tingkat kesalahan yang relatif kecil. Pola harga historis yang diolah oleh model memungkinkan proyeksi harga mendekati realitas pasar.
   - **Faktor apa saja yang memengaruhi akurasi prediksi harga beras?**  
     Analisis fitur menunjukkan bahwa bulan, tahun, dan tren musiman memiliki pengaruh signifikan terhadap harga. Faktor-faktor ini selaras dengan pola pasar yang dipengaruhi oleh musim panen, distribusi, dan permintaan.

3. **Pencapaian Goals**  
   - **Menghasilkan model prediktif dengan akurasi tinggi untuk meramalkan harga beras.**  
     RMSE yang relatif rendah mengindikasikan bahwa model cukup andal untuk memberikan prediksi harga. Ini dapat membantu pemerintah dan pelaku usaha dalam pengambilan keputusan terkait harga beras.
   - **Memberikan wawasan terkait pola harga berdasarkan analisis data.**  
     Pola harga yang ditemukan menunjukkan bahwa harga beras cenderung lebih tinggi pada bulan Februari-Maret dan lebih rendah pada Januari. Hal ini sejalan dengan pola musiman akibat pasokan yang meningkat setelah musim panen. Wawasan ini dapat digunakan untuk merencanakan intervensi kebijakan atau strategi pemasaran.

4. **Dampak Solusi Statements**  
   - **Menggunakan algoritma Random Forest Regressor untuk membangun model prediksi.**  
     Pemilihan algoritma ini terbukti tepat karena Random Forest dapat menangkap hubungan non-linear yang kompleks dalam data historis harga beras. 
   - **Melakukan hyperparameter tuning untuk meningkatkan performa model.**  
     Hyperparameter tuning meningkatkan kemampuan model untuk menangkap pola harga secara lebih akurat, sebagaimana ditunjukkan oleh nilai RMSE yang kompetitif.
   - **Menganalisis fitur yang paling berpengaruh terhadap prediksi harga beras menggunakan feature importance.**  
     Feature importance menunjukkan bahwa fitur `bulan` dan `tahun` paling berkontribusi dalam prediksi, mengonfirmasi bahwa analisis berbasis waktu adalah pendekatan yang efektif dalam memproyeksikan harga.

### Kesimpulan:
Evaluasi menunjukkan bahwa model telah berhasil menjawab **Problem Statements**, mencapai **Goals**, dan memberikan solusi yang relevan terhadap kebutuhan yang dirumuskan dalam **Business Understanding**. Selain itu, wawasan yang dihasilkan dapat digunakan untuk mendukung pengambilan keputusan strategis terkait harga beras, seperti:
- Menentukan waktu optimal untuk mengintervensi pasar.
- Merencanakan strategi distribusi berdasarkan prediksi harga di masa mendatang.
- Mengantisipasi periode harga tinggi yang dapat memengaruhi daya beli masyarakat.


---
## Testing
- Trend Prediksi Harga Beras Premium dan Medium
![enter image description here](https://github.com/kangdconqueror/prediksi-harga-beras/blob/main/4.png?raw=true)

Jenis Beras : Premium 
Tanggal : 2025-03-04 
Prediksi Harga : Rp 13,148.0
- Model ini hanya memberikan gambaran prediksi harga berdasarkan tren data historis, dan perlu ada kombinasi dengan faktor eksternal lain seperti kebijakan pemerintah, cuaca, atau perubahan permintaan dan pasokan pasar untuk menghasilkan prediksi yang lebih akurat. Oleh karena itu, hasil prediksi ini harus digunakan dengan hati-hati dan dipertimbangkan bersama faktor-faktor eksternal yang relevan untuk keputusan yang lebih tepat.

## Referensi

1. Naya, F. P., Berlianti, S. S., Parcha, N., & Kayla, A. (2024). Peramalan harga beras Indonesia menggunakan metode ARIMA. *Jurnal Ekonomi, Sosial & Humaniora*, *6*(02), 184-193. Retrieved from [https://www.jurnalintelektiva.com/index.php/jurnal/article/view/1063](https://www.jurnalintelektiva.com/index.php/jurnal/article/view/1063)

2. Fitra, J., & Prasada, I. Y. (2024). Peramalan harga beras premium di Kabupaten Kebumen. *Agritechpedia: Journal of Agriculture and Technology*, *2*(01), 28–36. Retrieved from [https://journal.eduartpia.id/index.php/agritechpedia/article/view/85](https://journal.eduartpia.id/index.php/agritechpedia/article/view/85)