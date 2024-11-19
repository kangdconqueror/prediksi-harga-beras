
# Laporan Proyek Machine Learning - Ma'shum Abdul Jabbar

## Domain Proyek

Proyek ini berfokus pada peramalan harga beras premium dan medium di Indonesia menggunakan machine learning. Masalah fluktuasi harga beras menjadi isu strategis karena mempengaruhi stabilitas ekonomi dan kesejahteraan masyarakat. Proyek ini bertujuan untuk melakukan analisis prediktif terhadap harga beras premium dan medium di Kabupaten Bogor menggunakan data historis harga harian.

**Rubrik/Kriteria Tambahan**:
- Masalah ini perlu diselesaikan untuk membantu pemerintah dan pelaku bisnis dalam pengambilan keputusan terkait tren kenaikan harga beras.
- Referensi:
  - FATASYA PUTRI NAYA, SALSABILA SARAH BERLIANTI, NAWAL PARCHA, & AISYAH KAYLA. (2024). PERAMALAN HARGA BERAS INDONESIA MENGGUNAKAN METODE ARIMA. _JURNAL EKONOMI, SOSIAL & HUMANIORA_, _6_(02), 184-193. Retrieved from https://www.jurnalintelektiva.com/index.php/jurnal/article/view/1063
  - Fitra, J., & Prasada, I. Y. (2024). Peramalan harga beras premium di Kabupaten Kebumen. _Agritechpedia: Journal of Agriculture and Technology_, _2_(01), 28–36. Retrieved from https://journal.eduartpia.id/index.php/agritechpedia/article/view/85

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

Data yang digunakan dalam proyek ini berasal dari Badan Pangan Nasional : https://badanpangan.go.id/. Data mencakup informasi harga harian beras premium di Kabupaten Bogor.


### Variabel-variabel pada dataset:
1. **komoditas**
   - **Tipe**: `string` (atau `object` dalam Pandas)
   - **Deskripsi**: Menyimpan nama komoditas yang harga per kilogramnya tercatat dalam dataset. Misalnya, nama-nama seperti "Beras Premium", "Beras Medium", atau komoditas pangan lainnya.
   
2. **tanggal**
   - **Tipe**: `datetime`
   - **Deskripsi**: Menyimpan tanggal spesifik dari data harga yang tercatat. Formatnya adalah `YYYY-MM-DD` (misalnya, `2022-01-01`). Kolom ini digunakan untuk menunjukkan kapan harga tercatat dan digunakan untuk analisis berdasarkan waktu (bulan, tahun).

3. **harga**
   - **Tipe**: `numeric` (integer atau float)
   - **Deskripsi**: Menyimpan harga per unit (biasanya per kilogram) untuk masing-masing komoditas pada tanggal tertentu. Nilai-nilai dalam kolom ini adalah angka yang menunjukkan harga yang dicatat. Data ini telah dibersihkan untuk memastikan tidak ada nilai yang tidak valid atau non-numerik.

4. **tahun**
   - **Tipe**: `integer`
   - **Deskripsi**: Menyimpan informasi tahun dari masing-masing tanggal harga. Variabel ini dihasilkan dari ekstraksi tahun dari kolom `tanggal`, yang digunakan untuk analisis berdasarkan tahun.

5. **bulan**
   - **Tipe**: `integer`
   - **Deskripsi**: Menyimpan informasi bulan (dalam angka 1-12, dengan 1 = Januari, 2 = Februari, dst.) dari masing-masing tanggal harga. Variabel ini dihasilkan dari ekstraksi bulan dari kolom `tanggal`, yang memudahkan analisis berdasarkan bulan dalam setahun.

**Rubrik/Kriteria Tambahan**:
- Dilakukan **exploratory data analysis** untuk melihat tren dan pola harga.
- Visualisasi berupa grafik tren harga digunakan untuk memahami fluktuasi harga.

1. **Analisis Pergerakan Harga Beras Premium dan Medium Per Tahun**
![enter image description here](https://github.com/kangdconqueror/prediksi-harga-beras/blob/main/1.png?raw=true)
2. **Perbandingan harga beras premium dan medium per bulan**
![enter image description here](https://github.com/kangdconqueror/prediksi-harga-beras/blob/main/2.png?raw=true)

3. **Menentukan pada bulan berapa harga beras tertinggi**
Harga beras premium tertinggi terjadi pada bulan Maret 2024 
Harga beras medium tertinggi terjadi pada bulan Maret 2024

4. **Menentukan pada bulan berapa harga beras terendah**
Harga beras premium terendah terjadi pada bulan Januari 2022 
Harga beras medium terendah terjadi pada bulan Januari 2022

5. **Melihat Pola Perubahan Harga beras (2022-2024)**
![enter image description here](https://github.com/kangdconqueror/prediksi-harga-beras/blob/main/3.png?raw=true)

---

## Data Preparation

Tahapan data preparation yang dilakukan meliputi:
1. **Handling Missing Values**: Mengisi nilai yang hilang dengan metode interpolasi.
2. **Scaling**: Normalisasi data harga untuk mempercepat konvergensi model.
3. **Feature Engineering**: Menambahkan fitur seperti rata-rata harga mingguan.

**Rubrik/Kriteria Tambahan**:
- Tahap Data Preparation adalah langkah penting dalam proses machine learning karena kualitas data sangat memengaruhi performa model.
- Visualisasi data setelah proses cleaning.

---

## Modeling

Model yang digunakan: **Random Forest Regressor**

### Parameter Utama:
- **RandomForestRegressor**: `max_depth=10`, `max_features='sqrt'`,  `min_samples_leaf=4`, `min_samples_split=10`, `random_state=42`

**Rubrik/Kriteria Tambahan**:
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
- **Root Mean Square Error (RMSE)**: Mengukur penyimpangan antara nilai aktual dan prediksi.

### Hasil Evaluasi:
Nilai Root Mean Squared Error (RMSE) menunjukkan tingkat kesalahan prediksi model. Dalam kasus ini, RMSE Beras Premium = 1602.84 dan RMSE Beras Medium = 1646.84 memiliki arti sebagai berikut:

1.  RMSE adalah rata-rata akar kuadrat dari kesalahan prediksi, yang diukur dalam satuan yang sama dengan target (dalam hal ini Rupiah).
2.  RMSE Premium (1602.84): Prediksi harga beras premium rata-rata meleset sebesar ±Rp 1,602.84 dari harga sebenarnya.
3.  RMSE Medium (1646.84): Prediksi harga beras medium rata-rata meleset sebesar ±Rp 1,646.84 dari harga sebenarnya.


---
## Testing
- Trend Prediksi Harga Beras Premium dan Medium
![enter image description here](https://github.com/kangdconqueror/prediksi-harga-beras/blob/main/4.png?raw=true)

Jenis Beras : Premium 
Tanggal : 2025-03-04 
Prediksi Harga : Rp 13,148.0
- Model ini hanya memberikan gambaran prediksi harga berdasarkan tren data historis, dan perlu ada kombinasi dengan faktor eksternal lain seperti kebijakan pemerintah, cuaca, atau perubahan permintaan dan pasokan pasar untuk menghasilkan prediksi yang lebih akurat. Oleh karena itu, hasil prediksi ini harus digunakan dengan hati-hati dan dipertimbangkan bersama faktor-faktor eksternal yang relevan untuk keputusan yang lebih tepat.
