# Sales Forecasting FMCG menggunakan ARIMA

## 📌 Deskripsi Proyek
Proyek ini menyajikan analisis deret waktu (time-series) yang komprehensif dan model Forecasting untuk perusahaan Fast-Moving Consumer Goods (FMCG), yang mengkhususkan diri pada produk makanan dan makanan beku (frozen food). Menggunakan data transaksi historis penjualan harian di tahun 2024, tujuannya adalah untuk mengungkap pola penjualan, menganalisis performa produk di berbagai pasar regional, dan membangun model prediksi yang kuat (ARIMA) untuk melakukan Forecasting kuantitas penjualan di masa depan.

> **Catatan Privasi Data:** Dataset (`anonymized_data.csv`) dan notebook (`Forecast_Arima_Anonymized.ipynb`) dalam repositori ini telah dianonimkan. Informasi sensitif seperti Nama Pelanggan, Nama Salesman, PIC, dan Merek Perusahaan tertentu telah diganti dengan placeholder atau istilah umum untuk melindungi privasi bisnis, sambil tetap mempertahankan distribusi statistik dan kegunaan data untuk pemodelan.

---

## 💼 Masalah Bisnis (Perspektif Business Analyst)

Dalam industri FMCG dan Frozen Food yang sangat kompetitif, menyeimbangkan operasional rantai pasok (supply chain) dengan fluktuasi permintaan pasar adalah tantangan utama. Masalah bisnis inti yang dibahas dalam proyek ini adalah:

1. **Ketidakpastian Permintaan & Hambatan Rantai Pasok:**
   Fluktuasi dalam penjualan harian menyebabkan terjadinya **stockout** (kehilangan pendapatan dan ketidakpuasan pelanggan) atau **overstocking** (peningkatan biaya penyimpanan dan potensi pemborosan makanan).
2. **Alokasi Sumber Daya:**
   Tanpa data, sulit untuk menentukan provinsi, kota, dan lini produk mana yang memberikan pendapatan paling berdampak, sehingga pemasaran yang ditargetkan dan penyebaran tim penjualan menjadi tidak efisien.
3. **Manajemen Inventaris Reaktif vs. Proaktif:**
   Hanya mengandalkan rata-rata bergerak historis atau perasaan (gut feeling) membatasi kemampuan perusahaan untuk bersiap menghadapi lonjakan permintaan B2B/B2C yang tiba-tiba.

**Tujuan Bisnis:**
Mengubah data penjualan menjadi kecerdasan bisnis yang dapat ditindaklanjuti dengan mengidentifikasi pasar utama dan menciptakan mesin prediksi (ARIMA) yang memungkinkan bisnis untuk mengantisipasi volume permintaan beberapa hari sebelumnya.

---

## 🔬 Exploratory Data Analysis & Insights (Perspektif Data Analyst)

Sebelum melakukan Forecasting, analisis mendalam terhadap data transaksi menghasilkan beberapa wawasan strategis:

### 1. Penetrasi Pasar (Analisis Geospasial)
- **Provinsi Dominan:** `JAWA TIMUR`, `JAWA BARAT`, dan `DKI JAKARTA` menghasilkan mayoritas pendapatan mutlak. Ini menunjukkan bahwa jaringan distribusi perusahaan sangat tersentralisasi di Pulau Jawa.
- **Peluang yang Belum Terjamah:** Wilayah di luar Jawa (seperti Sulawesi dan Bali) menunjukkan daya tarik yang meningkat tetapi membutuhkan strategi rantai pasok yang berbeda (misalnya, penyimpanan dingin lokal) untuk mengimbangi potensi hambatan logistik.
- **Rekomendasi:** Anggaran pemasaran dan promosi harus disalurkan secara agresif untuk mempertahankan dominasi di Jawa, sementara kampanye sekunder dapat diuji di Bali dan Sulawesi.

### 2. Performa Portofolio Produk (Prinsip Pareto)
- Berdasarkan kuantitas penjualan dan total pendapatan (Omzet), sebagian kecil dari "Produk Utama" (misalnya, kategori *Tortilla Spesifik* atau *Daging*) berfungsi sebagai "cash cow" bisnis.
- **Rekomendasi:** Pastikan SKU berkinerja terbaik ini *tidak pernah* mengalami stockout. Sementara itu, SKU yang kurang berkinerja dapat dibundel dengan produk terlaris untuk meningkatkan perputaran inventaris.

### 3. Perilaku Deret Waktu (Time Series)
- **Musiman & Lonjakan:** Penjualan agregat harian menunjukkan variasi yang volatil, sangat dipengaruhi oleh "pesanan massal" (bulk orders) mendadak dari mitra B2B (misalnya grosir, hotel).
- **Deteksi Outlier:** Penerapan Statistical Z-Score membantu mengidentifikasi anomali basis data yang ekstrem ini. Penghalusan outlier ini merupakan langkah kritis agar model ARIMA tidak "mempelajari" anomali langka sebagai pola standar yang berulang.

---

## ⚙️ Arsitektur Solusi & Pemodelan (Perspektif Data Scientist)

Untuk memprediksi permintaan di masa depan, pipeline data end-to-end diimplementasikan dengan fokus pada Forecasting deret waktu statistik:

### 1. Pra-pemrosesan Data
- Mengagregasi ribuan pesanan transaksional harian ke dalam format deret waktu harian yang berkelanjutan (`Date` vs `Total Quantity`).
- Menangani celah lini masa yang hilang dengan interpolasi linier.

### 2. Stasioneritas & Pengujian Statistik
- Melakukan uji **Augmented Dickey-Fuller (ADF)** dan **KPSS** untuk mengevaluasi stasioneritas deret waktu.
- Differencing ($d$) diterapkan untuk menstabilkan rata-rata secara matematis sebelum memasukkan data ke algoritma ARIMA.

### 3. ARIMA (Auto-Regressive Integrated Moving Average)
- Mengeksplorasi berbagai kombinasi $p$ (Lag order), $d$ (Degree of differencing), dan $q$ (Order of moving average).
- **Evaluasi Model:** Model yang dipilih dioptimalkan dengan meminimalkan **Root Mean Squared Error (RMSE)**.
- **Forecasting:** Model berhasil menangkap tren dasar penjualan harian, memungkinkan perusahaan untuk memprediksi kebutuhan stok ulang dasar untuk minggu-minggu mendatang.

---

## 📊 Dampak Bisnis & Hasil
Dengan prediksi ARIMA yang disetel dengan cermat:
1. **Efisiensi Biaya:** Tim pengadaan sekarang dapat melakukan pesanan bahan baku berdasarkan Forecasting yang didukung secara statistik daripada intuisi, sehingga mengurangi kelebihan pembelian.
2. **Ketahanan Operasional:** Bagian produksi dapat merencanakan staf dan penggunaan mesin sebelumnya berdasarkan kurva permintaan yang diharapkan.
3. **Budaya Berbasis Data:** Notebook ini mengubah kebisingan transaksional mentah menjadi aset strategis yang bersih dan dapat diprediksi bagi perusahaan.

---

## 🛠️ Tech Stack
- **Bahasa:** Python 3
- **Manipulasi Data:** `pandas`, `numpy`
- **Visualisasi Data:** `matplotlib`, `seaborn`
- **Analisis Statistik & Forecasting:** `scipy`, `statsmodels.tsa.arima` (ARIMA), `scikit-learn`

---

## 🚀 Cara Menjalankan Proyek
1. Clone repositori ini ke mesin lokal Anda:
   ```bash
   git clone https://github.com/yourusername/forecast-sales-fmcg.git
   ```
2. Arahkan ke direktori proyek:
   ```bash
   cd forecast-sales-fmcg
   ```
3. Instal dependensi yang diperlukan:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn
   ```
4. Buka dan jalankan Jupyter Notebook:
   - Jalankan Jupyter Notebook atau buka melalui VS Code.
   - Jalankan **`Forecast_Arima_Anonymized.ipynb`**.
   - *Catatan: Jangan lupa bahwa notebook ini bergantung pada `anonymized_data.csv` yang disertakan bersamanya dalam repositori ini.*