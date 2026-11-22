# Analisis Pola dan Prediksi Churn Pelanggan Netflix

Proyek Akhir **Praktikum Data Science** (Semester Ganjil 2026/2027), Program Studi Informatika, UPN "Veteran" Yogyakarta.

**Judul:** Analisis Pola Churn Pelanggan Netflix Berdasarkan Perilaku Menonton dan Karakteristik Langganan Menggunakan Data Mining: Studi pada Dataset 5.000 Pelanggan

## Ringkasan proyek

Layanan streaming kehilangan pendapatan ketika pelanggan berhenti berlangganan (*churn*). Proyek ini menjawab empat pertanyaan pada dataset 5.000 pelanggan: (1) pola apa yang berasosiasi dengan churn, (2) variabel mana yang terkuat dan mana yang tidak berasosiasi, (3) seberapa baik churn dapat diprediksi dan model mana yang layak dipakai, dan (4) pelanggan aktif seperti apa yang berisiko beserta *revenue-at-risk*-nya.

Seluruh alur CRISP-DM berada dalam **satu notebook**, [`Netflix_Churn_Final_Project.ipynb`](Netflix_Churn_Final_Project.ipynb), dengan penjelasan tertulis di setiap langkah:

1. Business Understanding (masalah, pertanyaan, metode, metrik, kriteria berhasil)
2. Data Gathering (sumber, tautan, lisensi, arti kolom, alasan memilih dataset)
3. EDA (statistik deskriptif, visualisasi, temuan di bawah tiap visual)
4. Cleaning dan Preprocessing (audit, penanganan data, sebelum vs sesudah, split, encoding, scaling, feature selection beserta alasan membuang, membuat, dan memakai fitur)
5. Modelling (pilihan model dan alasan, test design, tuning dengan validasi silang, log iterasi; Logistic Regression, Random Forest, Gradient Boosting, dan baseline)
6. Evaluasi (metrik, matriks konfusi, kurva ROC/PR, analisis ambang, interval kepercayaan, pemilihan model terbaik, ringkasan terhadap kriteria berhasil)
7. Konklusi (jawaban pertanyaan, arti metrik, keputusan, keterbatasan, saran, rencana deployment di atas kertas, review proses dan proyek)

### Hasil utama

- Churn berasosiasi dengan **perilaku**, bukan identitas: jam tonton rendah (churn 85,7 % pada kuintil terbawah vs 8,1 % pada teratas), jeda login > 30 hari (churn melonjak ke sekitar 75 %), paket Basic (61,8 %), pembayaran Crypto/Gift Card (58–60 %), dan 1–3 profil (57–59 %). `age`, `gender`, `region`, `device`, dan `favorite_genre` tidak berasosiasi (p > 0,05).
- Dua skenario fitur dibandingkan karena `last_login_days` berisiko *temporal leakage*: skenario **A** (dengan fitur itu) mencapai ROC-AUC 0,9999 pada data uji, skor yang **tidak dipercaya**; skenario **B** (tanpa fitur itu) adalah **model utama**.
- Model utama (skenario B, Gradient Boosting, ambang 0,425 yang dipilih dari data latih): **ROC-AUC 0,875, recall churn 0,845, precision 0,738, akurasi 0,771** pada 1.000 pelanggan uji. Pada ambang baku 0,5, recall 0,742 (sedikit di bawah target 0,75). Baseline: akurasi 0,503.
- Model menandai 747 dari 2.485 pelanggan aktif sebagai berisiko (USD 9.880,53 pendapatan bulanan). Aturan sederhana "tidak login > 30 hari" menandai 615 pelanggan (USD 9.088,85). Kedua daftar nyaris tidak beririsan (4 pelanggan), sehingga layak digabung.
- Peringatan: dataset **diindikasikan sintetis** (distribusi hampir seragam, churn 50,3 %). Temuan adalah asosiasi pada dataset ini, bukan fakta tentang pelanggan Netflix sebenarnya.

## Anggota kelompok dan kontribusi

| Nama | NIM | Kontribusi |
| --- | --- | --- |
| Radhitya Rasta Pramoja | 123240142 | Cleaning, Preprocessing, dan Feature Selection; penyusunan README |
| Constantine Rylianno Sumakul | 123240139 | Exploratory Data Analysis (statistik deskriptif, visualisasi, uji statistik) |
| Syahnita | 123240006 | Business Understanding dan Data Gathering |
| Liu Luqyana | 123240223 | Modelling, Evaluasi, dan Konklusi |

**Dosen pengampu:** Dhimas Arief Dharmawan

## Sumber data

| Item | Keterangan |
| --- | --- |
| Judul | Netflix Customer Churn & Engagement Analytics |
| Penyedia | Pengguna Kaggle `zeyadmohamed26` |
| Tautan | https://www.kaggle.com/datasets/zeyadmohamed26/netflix-customer-churn-and-engagement-analytics |
| Berkas | `data/netflix_customer_churn.csv` (545.934 byte, 5.000 baris × 14 kolom), salinan tanpa perubahan |
| Tanggal akses | 10 Oktober 2026 |
| Dokumentasi sumber | Halaman Kaggle menyebutnya benchmark yang dikurasi tanpa asal data primer; tidak menyatakan data sintetis (dicek 22 November 2026) |
| Lisensi | Apache 2.0 (metadata resmi Kaggle; lihat [`data/LICENSE_DATASET.md`](data/LICENSE_DATASET.md)) |

Atribusi: zeyadmohamed26. (n.d.). *Netflix customer churn & engagement analytics* [Data set]. Kaggle. Diakses 10 Oktober 2026.

## Struktur repositori

```
.
├── README.md
├── Netflix_Churn_Final_Project.ipynb   # notebook utama, sudah dieksekusi (Business Understanding sampai Konklusi)
├── requirements.txt
├── data/
│   ├── netflix_customer_churn.csv      # dataset (tidak dimodifikasi)
│   └── LICENSE_DATASET.md              # atribusi dan lisensi dataset
├── figures/                            # 17 gambar yang dihasilkan notebook
└── outputs/                            # tabel hasil (CSV), matriks train/test, hasil_ringkas.json
```

## Cara menjalankan

Diuji dengan Python 3.12 (pandas 3.0, numpy 2.5, scipy 1.18, scikit-learn 1.9, matplotlib 3.11, seaborn 0.13).

```bash
# 1. (opsional) buat virtual environment
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate

# 2. pasang dependensi
pip install -r requirements.txt

# 3. jalankan notebook dari folder ini
jupyter notebook Netflix_Churn_Final_Project.ipynb
# lalu: Kernel -> Restart & Run All   (sekitar 5 menit)
```

Tanpa membuka antarmuka:

```bash
jupyter nbconvert --to notebook --execute --inplace Netflix_Churn_Final_Project.ipynb
```

Syarat: notebook harus dijalankan dari folder yang berisi `data/` (path dibaca relatif). Gambar dan tabel ditulis ulang ke `figures/` dan `outputs/`.

## Reprodusibilitas

- Seed acak tetap (`SEED = 42`) untuk split, validasi silang, model, dan permutation importance. Dua eksekusi penuh dari awal menghasilkan angka yang sama.
- Pembagian data stratified 80:20 (4.000 latih, 1.000 uji). Parameter encoder dan scaler serta seluruh uji seleksi fitur dihitung **hanya dari data latih**. Data uji dipakai sekali, pada tahap Evaluasi.
- Pemilihan model memakai aturan yang ditetapkan sebelum melihat data uji (satu standard error pada validasi silang, lalu recall, lalu kesederhanaan). Ambang keputusan dipilih dari prediksi out-of-fold data latih.
- Angka model berbasis pohon dapat bergeser pada desimal ke-4 antar versi scikit-learn atau sistem operasi.

## Keterbatasan (ringkas)

Data diindikasikan sintetis; hanya asosiasi, bukan sebab-akibat; tidak ada dimensi waktu sehingga `last_login_days` tidak dapat dipastikan bebas kebocoran; fitur terbatas (tanpa kepuasan atau keluhan); estimasi dari satu split uji 1.000 baris (recall ±3–4 poin); revenue-at-risk berupa estimasi. Rincian lengkap ada pada bagian 7 notebook.
