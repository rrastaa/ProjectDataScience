# Analisis Pola Churn Pelanggan Netflix — Business & Data Understanding

Proyek UTS **Data Science** (Semester Ganjil 2026/2027), Program Studi Informatika,
UPN "Veteran" Yogyakarta. Mengikuti kerangka CRISP-DM; repositori ini mencakup fase
**Business Understanding** dan **Data Understanding**.

**Judul:** Analisis Pola Churn Pelanggan Netflix Berdasarkan Perilaku Menonton dan Karakteristik Langganan Menggunakan Data Mining: Studi pada Dataset 5.000 Pelanggan

**Anggota kelompok**

| Nama | NIM |
| --- | --- |
| Radhitya Rasta Pramoja | 123240242 |
| Constantine Rylianno Sumakul | 123240139 |
| Syahnita | 123240006 |
| Liu Luqyana | 123240223 |

**Dosen pengampu:** Dhimas Arief Dharmawan

## Struktur berkas

```
.
├── Netflix_Churn_Data_Understanding.ipynb   # Notebook pemeriksaan data dan EDA (sudah dieksekusi)
├── README.md
├── requirements.txt
├── data/
│   └── netflix_customer_churn.csv           # Dataset mentah (tidak dimodifikasi)
├── figures/                                 # 7 grafik EDA (PNG, 300 dpi)
│   ├── fig1_target_dan_paket.png            # Gambar 1
│   ├── fig2_recency_dan_intensitas.png      # Gambar 2
│   ├── fig3_churn_per_kategori.png          # Gambar 3
│   ├── figB1_distribusi_numerik.png         # Gambar B1
│   ├── figB2_boxplot_menurut_churn.png      # Gambar B2
│   ├── figB3_paket_x_pembayaran.png         # Gambar B3
│   └── figB4_korelasi_spearman.png          # Gambar B4
└── outputs/                                 # Tabel hasil (CSV)
    ├── outlier_iqr.csv                      # Tabel A2
    ├── chi_square.csv                       # Tabel 9 (+ Cramér's V)
    └── mann_whitney.csv                     # Tabel A3
```

## Sumber data dan lisensi

| Item | Keterangan |
| --- | --- |
| Judul | Netflix Customer Churn & Engagement Analytics |
| Penyedia | Pengguna Kaggle `zeyadmohamed26` |
| URL | https://www.kaggle.com/datasets/zeyadmohamed26/netflix-customer-churn-and-engagement-analytics |
| Berkas | `netflix_customer_churn.csv` — 545.934 byte, 5.000 baris × 14 kolom |
| Tanggal akses | 10 Oktober 2026 |
| Lisensi | **Apache 2.0** (kolom *License* pada metadata halaman Kaggle, diperiksa 10 Oktober 2026) |

Dataset diunduh dari Kaggle untuk keperluan akademik. Badge pada deskripsi dataset menyebut CC0 1.0,
tetapi metadata resmi Kaggle menyatakan **Apache 2.0**; proyek ini mengikuti metadata resmi. Apache 2.0
mengizinkan penggunaan, modifikasi, dan redistribusi dengan syarat atribusi dicantumkan dan salinan
lisensi disertakan (https://www.apache.org/licenses/LICENSE-2.0). Data **diindikasikan sintetis** (lihat bagian *Representativitas* pada laporan),
sehingga temuan tidak boleh diklaim sebagai fakta tentang pelanggan Netflix yang sebenarnya.

**Atribusi:** zeyadmohamed26. (n.d.). *Netflix customer churn & engagement analytics* [Data set]. Kaggle.
Diakses 10 Oktober 2026, dari https://www.kaggle.com/datasets/zeyadmohamed26/netflix-customer-churn-and-engagement-analytics
Berkas `data/netflix_customer_churn.csv` pada repositori ini adalah salinan tanpa perubahan.

## Cara menjalankan ulang

Python yang diuji: 3.12 (pandas 3.0, numpy 2.4, scipy 1.17, matplotlib 3.10, seaborn 0.13).

```bash
# 1. (opsional) buat virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 2. pasang dependensi
pip install -r requirements.txt

# 3. jalankan notebook dari folder ini
jupyter notebook Netflix_Churn_Data_Understanding.ipynb
# lalu: Kernel -> Restart & Run All
```

Atau tanpa membuka antarmuka:

```bash
jupyter nbconvert --to notebook --execute --inplace Netflix_Churn_Data_Understanding.ipynb
```

Syarat penting: notebook harus dijalankan dari folder yang berisi `data/`, karena path dibaca secara
relatif (`data/netflix_customer_churn.csv`). Gambar dan tabel akan ditulis ulang ke `figures/` dan `outputs/`.

## Reprodusibilitas

- Seed acak tetap (`SEED = 42`). Analisis pada tahap ini deskriptif dan deterministik, jadi angka sama pada setiap eksekusi.
- Semua uji memakai taraf signifikansi 5 %. Chi-square dan Cramér's V dihitung tanpa koreksi kontinuitas; Mann-Whitney U dua sisi.
- Pada `outputs/mann_whitney.csv`, p-value `0.0` untuk `watch_hours` dan `avg_watch_time_per_day` adalah *underflow* floating point (p < 1e-300), dibaca sebagai "< 0,001" pada laporan.

## Temuan utama (ringkas)

- Churn 2.515 dari 5.000 pelanggan (50,3 %); kelas hampir seimbang.
- Pola terkuat ada pada perilaku: `watch_hours` (Spearman −0,54) dan `last_login_days` (+0,47).
- Basic churn 61,8 % vs Premium 43,7 %; Crypto dan Gift Card churn lebih tinggi; akun 4–5 profil churn lebih rendah.
- Gender, wilayah, perangkat, genre favorit, dan usia tidak berasosiasi dengan churn (p > 0,05).
- Peringatan: `last_login_days` berisiko *temporal leakage*; `avg_watch_time_per_day` adalah kolom turunan dari dua kolom lain.

## Tahap selanjutnya (UAS)

Data Preparation → Modeling → Evaluation → Deployment. Rencana awal: buang `customer_id`, pilih salah
satu antara `subscription_type` dan `monthly_fee`, tangani `avg_watch_time_per_day` dan `last_login_days`
terhadap kebocoran, *stratified split* 80:20, baseline majority-class (50,3 %), lalu bandingkan model.
