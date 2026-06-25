# JCDSJKTPM-036_Alpha

# FINTel — Sistem Prediksi Churn Pelanggan

Proyek data science untuk memprediksi churn pelanggan telekomunikasi menggunakan machine learning, dilengkapi dengan segmentasi risiko dan rekomendasi retensi yang dipersonalisasi.

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://fintelchurnpredictionapp-01.streamlit.app/)
[![Tableau Dashboard](https://img.shields.io/badge/Tableau-Dashboard-blue?logo=tableau&logoColor=white)](https://public.tableau.com/app/profile/akbar.kanugraha/viz/Fintel_Dashboard/Dashboard1?publish=yes)

**Kelompok A**

**Anggota:** Akbar Kanugraha · Khaerun Nisa'Tri Safaati · Indira Faisa Afgani

---

## Latar Belakang

Churn pelanggan merupakan salah satu tantangan terbesar bagi perusahaan telekomunikasi. Kehilangan pelanggan berdampak langsung pada pendapatan, dan biaya untuk mendapatkan pelanggan baru umumnya jauh lebih tinggi dibanding mempertahankan yang sudah ada.

Proyek ini berangkat dari kebutuhan bisnis untuk mengidentifikasi pelanggan yang berpotensi berhenti berlangganan sebelum hal itu terjadi, sehingga tim retensi bisa mengambil tindakan lebih awal dengan strategi yang tepat sasaran.

---

## Tujuan Project

- Membangun model machine learning untuk memprediksi kemungkinan churn pelanggan
- Melakukan segmentasi risiko pelanggan ke dalam tiga kategori: **Rendah**, **Sedang**, dan **Tinggi**
- Memberikan rekomendasi retensi yang dipersonalisasi berdasarkan faktor-faktor yang paling memengaruhi prediksi tiap individu (via SHAP)
- Menghitung estimasi dampak bisnis dari intervensi retensi

---

## Dataset

Dataset yang digunakan adalah **Telco Customer Churn – IBM Dataset**, dengan rincian:

| Atribut | Detail |
|---|---|
| Jumlah Data | 7.032 pelanggan |
| Jumlah Fitur (awal) | 33 fitur |
| Fitur yang Digunakan | 20 fitur (setelah seleksi SelectKBest, ANOVA F, k=20) |
| Target Variable | `Churn Label` (Yes / No) |
| Tingkat Churn | ±26,58% |

Fitur mencakup informasi kontrak, layanan yang digunakan (internet, telepon, streaming), metode pembayaran, durasi berlangganan, dan biaya bulanan. Data berasal dari pelanggan di wilayah California, Amerika Serikat.

---

## Alur Pengerjaan Project

**Data Understanding**
Eksplorasi awal dataset, memahami tipe data, distribusi variabel, dan identifikasi missing values atau data yang tidak konsisten.

**Data Cleaning**
Menangani nilai kosong (terutama pada kolom `Total Charges`), mengonversi tipe data, dan menghapus kolom yang tidak relevan untuk pemodelan.

**Exploratory Data Analysis (EDA)**
Analisis distribusi churn berdasarkan berbagai segmen seperti jenis kontrak, layanan internet, masa berlangganan, dan add-on. Visualisasi pola churn per kota juga dilakukan.

**Feature Engineering**
Encoding variabel kategorikal menggunakan One-Hot Encoding (drop='first') untuk 16 fitur kategorikal, dan Target Encoding untuk fitur City.

**Preprocessing**
Seleksi fitur menggunakan SelectKBest (ANOVA F, k=20), normalisasi fitur numerik, dan penanganan imbalance data menggunakan  class_weight='balanced' pada Logistic Regression.

**Model Training**
Benchmarking beberapa algoritma: Logistic Regression, Random Forest, XGBoost, dan variasi lainnya. Evaluasi dilakukan dengan mempertimbangkan konteks bisnis, sehingga F2-Score (yang lebih menekankan recall) dipilih sebagai metrik utama.

**Model Evaluation**
Model dievaluasi menggunakan F2-Score, ROC-AUC, dan threshold optimal. Logistic Regression terpilih sebagai model final karena performa terbaik pada metrik bisnis yang relevan.

**SHAP Analysis**
Interpretasi model menggunakan SHAP (SHapley Additive exPlanations) untuk memahami kontribusi tiap fitur terhadap prediksi, baik secara global maupun per individu pelanggan.

**Business Impact Analysis**
Estimasi potensi pendapatan yang dapat diselamatkan dari intervensi retensi berdasarkan median monthly charges dan jumlah pelanggan berisiko tinggi.

---

## Hasil Model

Model final yang digunakan adalah **Logistic Regression** dengan optimasi threshold.

Logistic Regression dipilih karena:
- Performanya unggul pada F2-Score dibandingkan model lain dalam benchmarking
- Lebih interpretatif dan cocok untuk kebutuhan bisnis yang memerlukan transparansi keputusan
- Konsisten dan stabil di berbagai kondisi data

| Metrik | Nilai |
|---|---|
| F2-Score (β=2) | **0.7444** |
| ROC-AUC | **0.8349** |
| Threshold Optimal | **0.2643** |

Threshold dioptimasi menggunakan out-of-fold prediction di TRAIN set (cross_val_predict), bukan dari test set, agar test set tetap murni untuk evaluasi final (hindari data leakage).

---

## Analisis SHAP

SHAP digunakan untuk dua tujuan:

**Global Explanation** — memahami fitur apa yang paling berpengaruh secara keseluruhan terhadap keputusan model. Fitur seperti `Tenure Months`, `Contract`, `Monthly Charge`, dan jumlah add-on menjadi kontributor utama.

**Individual Explanation** — untuk setiap pelanggan, SHAP menghitung kontribusi masing-masing fitur terhadap skor prediksi individu tersebut. Top-3 faktor pendorong churn per pelanggan inilah yang kemudian dijadikan dasar rekomendasi retensi yang dipersonalisasi.

Misalnya, jika masa berlangganan pendek menjadi faktor utama churn untuk pelanggan tertentu, sistem akan merekomendasikan program loyalty atau diskon perpanjangan kontrak.

---

## Business Impact

Berdasarkan analisis bisnis:

- **Median biaya bulanan** pelanggan: $70
- **Potensi pendapatan berisiko** (pelanggan dengan risiko tinggi): ±$139K per bulan
- Dengan intervensi retensi yang tepat, sebagian besar potensi kehilangan pendapatan ini dapat diminimalisir

Dashboard juga menampilkan breakdown pendapatan tertinggi per kota, distribusi churn berdasarkan jumlah add-on, dan pola churn berdasarkan masa berlangganan.

---

## Dashboard

## Dashboard

Dashboard interaktif dibangun menggunakan Tableau Public untuk memberikan gambaran menyeluruh tentang kondisi churn pelanggan di California. Tersedia filter berdasarkan metode pembayaran, jenis kontrak, layanan internet, dan layanan telepon.

KPI utama yang ditampilkan:
- **Total Pelanggan**: 7,032
- **Tingkat Churn**: 26.58%
- **Median Biaya Bulanan**: $70
- **Pendapatan Risiko Hilang**: $139K

Insight yang dapat diperoleh:
- Peta sebaran tingkat churn per kota di California
- Kota dengan pendapatan tertinggi (Los Angeles, San Diego, San Jose, Sacramento, San Francisco)
- Pola churn berdasarkan masa berlangganan — churn rate tertinggi di awal tenure
- Distribusi churn berdasarkan jumlah add-on
- Alasan utama pelanggan churn (attitude of support person, competitor offered higher download speed, dll.)
- Komposisi kontrak, layanan internet, layanan telepon, dan metode pembayaran

**Akses Dashboard Tableau:**  
[🔗 FINTel Dashboard — Tableau Public](https://public.tableau.com/app/profile/akbar.kanugraha/viz/Fintel_Dashboard/Dashboard1?publish=yes)

**Screenshot Dashboard:**

![FINTel Dashboard](assets/screenshot_dashboard.png)

---

## Aplikasi Streamlit

Aplikasi Streamlit menyediakan antarmuka untuk melakukan prediksi churn secara langsung. Terdapat tiga tab utama:

**Pelanggan Lama** — Masukkan Customer ID untuk melihat laporan churn analysis pelanggan yang sudah ada di database, termasuk profil pelanggan, skor churn, segmentasi risiko, penjelasan SHAP, dan rekomendasi retensi.

**Pelanggan Baru** — Input manual data pelanggan baru (kontrak, layanan, biaya, dll.) untuk mendapatkan prediksi churn secara real-time.

**Prediksi Secara Keseluruhan (CSV)** — Upload file CSV berisi data banyak pelanggan sekaligus untuk mendapatkan prediksi bulk, disertai visualisasi distribusi probabilitas churn, distribusi segmen risiko, dan opsi download hasil prediksi.

Untuk setiap prediksi, aplikasi menampilkan:
- Probabilitas churn dalam bentuk gauge chart
- Segmentasi risiko (Rendah / Sedang / Tinggi)
- Top-3 fitur pendorong churn berdasarkan SHAP
- Rekomendasi retensi yang dipersonalisasi

**Screenshot Aplikasi:**

![FINTel Streamlit App](assets/screenshot_app.png)

---

## Struktur Repository

```
fintel-churn-prediction/
│
├── data/
│   └── df_clean.csv                  # Dataset hasil preprocessing
│
├── models/
│   └── artifacts.pkl                 # Model, scaler, SHAP values, dan metadata
│
├── utils/
│   ├── prediction.py                 # Fungsi prediksi dan segmentasi
│   └── visualization.py              # Fungsi visualisasi (gauge, SHAP bar, dll.)
│
├── app.py                            # Aplikasi utama Streamlit
├── fintel_churn_analysis.ipynb       # Notebook EDA, preprocessing, dan modeling
├── requirements.txt                  # Daftar dependensi
└── README.md
```

---

## Cara Menjalankan Project

**Clone Repository**
```bash
git clone https://github.com/[username]/fintel-churn-prediction.git
cd fintel-churn-prediction
```

**Install Dependencies**
```bash
pip install -r requirements.txt
```

**Menjalankan Streamlit**
```bash
streamlit run app.py
```

---

## Demo

Aplikasi sudah di-deploy dan dapat diakses di:

**[https://fintelchurnpredictionapp-01.streamlit.app/](https://fintelchurnpredictionapp-01.streamlit.app/)**

---

## Teknologi yang Digunakan

| Kategori | Library / Tools |
|---|---|
| Bahasa | Python 3.x |
| Data Manipulation | pandas, numpy |
| Machine Learning | scikit-learn, imbalanced-learn |
| Interpretasi Model | shap |
| Visualisasi | plotly |
| Aplikasi Web | streamlit |
| Model Persistence | pickle |

---

## Pengembangan Selanjutnya

Beberapa hal yang bisa dikembangkan lebih lanjut:

- **Model yang lebih kompleks** — Eksperimen dengan ensemble method atau neural network untuk kemungkinan performa yang lebih baik
- **Retraining otomatis** — Membangun pipeline untuk retraining model secara berkala ketika ada data baru
- **Integrasi CRM** — Menghubungkan output prediksi langsung ke sistem CRM untuk otomatisasi follow-up tim retensi
- **Monitoring model** — Menambahkan sistem monitoring untuk mendeteksi data drift dan model degradation
- **A/B Testing** — Mengukur efektivitas rekomendasi retensi yang dihasilkan model terhadap pelanggan yang benar-benar diintervensi

---
