# GitHub Commit Analysis with Linear Regression

## Overview
Project ini bertujuan untuk menganalisis pola commit pada dataset **GitHub Sample Commits** dari BigQuery Public Dataset, lalu membangun model **Linear Regression** untuk memprediksi jumlah file yang berubah (`files_changed`) berdasarkan metadata commit dan karakteristik perubahan kode.

Target prediksi:
- **`files_changed`** → jumlah file yang berubah pada setiap commit

Metode yang digunakan:
- Exploratory Data Analysis (EDA)
- Preprocessing
- Linear Regression (Scikit-Learn)

---

# 1. Exploratory Data Analysis (EDA)

## Tujuan
Tahap EDA dilakukan untuk memahami distribusi data, pola commit, serta hubungan antar variabel sebelum proses modeling.

## Aktivitas EDA
Beberapa analisis eksploratif yang dilakukan:

### 1.1 Distribusi Target (`files_changed`)
Visualisasi distribusi jumlah file yang berubah pada setiap commit untuk melihat pola sebaran target.

### 1.2 Hubungan Fitur dengan Target
Menganalisis hubungan antara fitur numerik seperti:
- `subject_length`
- `message_length`
- `commit_hour`
- `commit_year`
- `insertions`
- `deletions`
- `net_change`

terhadap target `files_changed`.

### 1.3 Korelasi Antar Variabel
Heatmap korelasi digunakan untuk melihat kekuatan hubungan antar fitur numerik.

## Insight EDA
Hasil EDA menunjukkan bahwa:
- Distribusi `files_changed` cenderung right-skewed.
- Fitur `insertions`, `deletions`, dan `net_change` memiliki hubungan paling kuat terhadap `files_changed`.
- Fitur waktu seperti `commit_hour` dan `commit_year` memiliki pengaruh relatif kecil.

---

# 2. Preprocessing

## Tujuan
Tahap preprocessing dilakukan untuk membersihkan dan menyiapkan data agar siap digunakan dalam proses machine learning.

## Langkah Preprocessing

### 2.1 Feature Selection
Memilih fitur numerik yang relevan untuk modeling:

- `subject_length`
- `message_length`
- `commit_hour`
- `commit_year`
- `insertions`
- `deletions`
- `net_change`

Target:
- `files_changed`

### 2.2 Handling Missing Values
Memeriksa missing values pada seluruh fitur numerik.  
Nilai kosong ditangani menggunakan **median imputation** agar lebih robust terhadap outlier.

### 2.3 Data Type Validation
Memastikan seluruh fitur bertipe numerik (`int64` / `float64`) agar kompatibel dengan model regresi.

### 2.4 Feature-Target Split
Memisahkan data menjadi:
- `X` → fitur prediktor
- `y` → target (`files_changed`)

### 2.5 Train-Test Split
Membagi data menjadi:
- 80% training set
- 20% testing set

---

# 3. Modeling

## Model yang Digunakan
Model yang digunakan adalah **Linear Regression** dari Scikit-Learn.

### Alasan Pemilihan Model
Linear Regression dipilih karena:
- sederhana dan mudah diinterpretasikan,
- cocok sebagai baseline regression model,
- mampu menjelaskan hubungan linear antara fitur dan target.

## Proses Modeling
Tahapan modeling meliputi:
1. Melatih model menggunakan data training
2. Memprediksi data testing
3. Mengevaluasi performa model

---

# 4. Model Evaluation

Evaluasi model dilakukan menggunakan metrik regresi standar:

- **MAE (Mean Absolute Error)**
- **MSE (Mean Squared Error)**
- **RMSE (Root Mean Squared Error)**
- **R² Score**

## Hasil Evaluasi Linear Regression

```text
Linear Regression Evaluation
========================================
MAE  : 0.9814
MSE  : 92.3478
RMSE : 9.6098
R²   : 0.9912

```
Berdasarkan hasil evaluasi, model Linear Regression menunjukkan performa yang sangat baik dalam memprediksi jumlah file yang berubah (files_changed) pada setiap commit. Nilai MAE sebesar 0.9814 menunjukkan bahwa rata-rata kesalahan prediksi model hanya sekitar satu file per commit, sehingga model memiliki tingkat presisi yang tinggi dalam memperkirakan jumlah file yang berubah.

Nilai MSE sebesar 92.3478 dan RMSE sebesar 9.6098 menunjukkan bahwa meskipun masih terdapat beberapa prediksi dengan error yang lebih besar, secara umum tingkat kesalahan model tetap berada dalam batas yang wajar. Perbedaan antara MAE dan RMSE mengindikasikan bahwa terdapat beberapa commit dengan perubahan file yang cukup besar, namun pengaruhnya tidak secara signifikan menurunkan performa model secara keseluruhan.

Sementara itu, nilai R² sebesar 0.9912 menunjukkan bahwa model mampu menjelaskan sekitar 99.12% variasi pada target files_changed. Hal ini menunjukkan bahwa hubungan antara fitur-fitur prediktor—terutama insertions, deletions, dan net_change—dengan jumlah file yang berubah sangat kuat dan mampu ditangkap dengan baik oleh model linear.

Secara keseluruhan, hasil ini menunjukkan bahwa model Linear Regression sangat efektif digunakan sebagai baseline model untuk memprediksi jumlah file yang berubah pada commit GitHub. Model mampu menangkap hubungan linear antar fitur dengan sangat baik, menghasilkan performa prediksi yang stabil, akurat, dan dapat diandalkan untuk analisis commit berbasis regresi.

