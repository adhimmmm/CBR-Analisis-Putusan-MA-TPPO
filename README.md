# Sistem Case-Based Reasoning (CBR) untuk Prediksi Amar Putusan Pengadilan

## Deskripsi Proyek

Proyek ini merupakan implementasi **Case-Based Reasoning (CBR)** untuk membantu pencarian kasus hukum yang serupa dan memprediksi amar putusan berdasarkan putusan-putusan terdahulu.

Sistem dibangun menggunakan pendekatan:

* **TF-IDF + Cosine Similarity** untuk retrieval kasus.
* **IndoBERT + Cosine Similarity** untuk retrieval berbasis embedding.
* **Support Vector Machine (SVM)** untuk prediksi amar putusan.
* **Case Reuse (Weighted Similarity)** untuk menghasilkan solusi berdasarkan kasus terdahulu.

Dataset yang digunakan berasal dari **Direktori Putusan Mahkamah Agung Republik Indonesia** dengan domain perkara:

> **Pidana Khusus (Narkotika)**

---

# Tujuan Proyek

1. Membangun basis kasus (case base) dari dokumen putusan pengadilan.
2. Merepresentasikan kasus ke dalam bentuk terstruktur.
3. Menemukan kasus lama yang paling mirip dengan kasus baru.
4. Menggunakan solusi dari kasus terdahulu untuk memprediksi amar putusan.
5. Mengevaluasi performa model retrieval dan prediction.

---

# Arsitektur Sistem

```text
Tahap 1 : Case Base Construction
        ↓
Tahap 2 : Case Representation
        ↓
Tahap 3 : Case Retrieval
        ↓
Tahap 4 : Case Solution Reuse
        ↓
Tahap 5 : Model Evaluation
```

---

# Struktur Folder

```text
project/

│
├── data/
│   │
│   ├── raw/
│   │   ├── case_001.txt
│   │   ├── case_002.txt
│   │   └── ...
│   │
│   ├── processed/
│   │   └── cases.csv
│   │
│   ├── eval/
│   │   ├── queries.json
│   │   ├── retrieval_metrics.csv
│   │   └── prediction_metrics.csv
│   │
│   └── results/
│       └── predictions.csv
│
├── models/
│   ├── tfidf_vectorizer.pkl
│   └── svm_model.pkl
│
├── notebooks/
│   ├── 01_case_base.ipynb
│   ├── 02_case_representation.ipynb
│   ├── 03_case_retrieval.ipynb
│   ├── 04_solution_reuse.ipynb
│   └── 05_evaluation.ipynb
│
├── requirements.txt
├── README.md
└── .gitignore
```

---

# Tahapan Pengerjaan

## Tahap 1 — Case Base Construction

### Tujuan

Mengumpulkan dan membersihkan dokumen putusan.

### Proses

* Mengunduh dokumen putusan dari Direktori MA.
* Mengubah PDF menjadi teks.
* Membersihkan teks:

  * menghapus header
  * menghapus footer
  * menghapus nomor halaman
  * menghapus watermark
* Menyimpan hasil ke folder:

```text
data/raw/
```

### Output

```text
data/raw/*.txt
```

---

## Tahap 2 — Case Representation

### Tujuan

Merepresentasikan dokumen putusan ke dalam format terstruktur.

### Metadata yang diekstraksi

* Nomor Perkara
* Tanggal Putusan
* Pasal
* Ringkasan Fakta
* Amar Putusan
* Teks Lengkap

### Feature Engineering

* Jumlah kata
* Jumlah karakter
* Retrieval text

### Output

```text
data/processed/cases.csv
```

---

## Tahap 3 — Case Retrieval

### Tujuan

Mencari kasus lama yang paling mirip dengan query.

### Metode yang digunakan

#### 1. TF-IDF + Cosine Similarity

Mengubah dokumen menjadi representasi vektor menggunakan:

```python
TfidfVectorizer()
```

Kemudian menghitung kemiripan menggunakan:

```python
cosine_similarity()
```

---

#### 2. IndoBERT + Cosine Similarity

Menggunakan model:

```text
indobenchmark/indobert-base-p1
```

untuk menghasilkan embedding dokumen.

---

#### 3. Support Vector Machine (SVM)

Digunakan untuk memprediksi kategori amar putusan.

---

### Output

```text
models/tfidf_vectorizer.pkl
models/svm_model.pkl
data/eval/queries.json
```

---

## Tahap 4 — Case Solution Reuse

### Tujuan

Menggunakan kasus terdahulu sebagai solusi untuk kasus baru.

### Metode

* Mengambil Top-K kasus hasil retrieval.
* Mengambil amar putusan dari kasus tersebut.
* Menggunakan:

```text
Majority Voting
```

atau

```text
Weighted Similarity Voting
```

untuk menentukan solusi akhir.

### Output

```text
data/results/predictions.csv
```

Contoh:

| query_id | predicted_solution       | top_5_case_ids |
| -------- | ------------------------ | -------------- |
| 14       | ditolak dengan perbaikan | 12,13,16,17,8  |

---

## Tahap 5 — Model Evaluation

### Evaluasi Retrieval

Menggunakan:

```text
Hit@5 Accuracy
```

untuk:

* TF-IDF + Cosine
* IndoBERT + Cosine

---

### Evaluasi Prediction

Menggunakan:

* Accuracy
* Precision
* Recall
* F1-Score

untuk:

* TF-IDF + SVM
* TF-IDF + Reuse

---

### Visualisasi

* Perbandingan model retrieval
* Perbandingan model prediction
* Distribusi amar putusan
* Confusion Matrix
* Error Analysis

---

# Instalasi

## Clone Repository

```bash
git clone https://github.com/username/nama-repository.git
```

Masuk ke folder project:

```bash
cd nama-repository
```

---

# Membuat Virtual Environment (Opsional)

Windows:

```bash
python -m venv venv
```

Aktivasi:

```bash
venv\Scripts\activate
```

Linux / MacOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

---

# Install Dependency

Install seluruh library:

```bash
pip install -r requirements.txt
```

---

# Contoh requirements.txt

```text
pandas
numpy
scikit-learn
matplotlib
seaborn
torch
transformers
joblib
pdfminer.six
beautifulsoup4
jupyter
```

---

# Cara Menjalankan Pipeline

## Tahap 1

Jalankan:

```bash
jupyter notebook notebooks/01_case_base.ipynb
```

Hasil:

```text
data/raw/
```

---

## Tahap 2

Jalankan:

```bash
jupyter notebook notebooks/02_case_representation.ipynb
```

Hasil:

```text
data/processed/cases.csv
```

---

## Tahap 3

Jalankan:

```bash
jupyter notebook notebooks/03_case_retrieval.ipynb
```

Hasil:

```text
models/tfidf_vectorizer.pkl
models/svm_model.pkl
data/eval/queries.json
```

---

## Tahap 4

Jalankan:

```bash
jupyter notebook notebooks/04_solution_reuse.ipynb
```

Hasil:

```text
data/results/predictions.csv
```

---

## Tahap 5

Jalankan:

```bash
jupyter notebook notebooks/05_evaluation.ipynb
```

Hasil:

```text
data/eval/retrieval_metrics.csv
data/eval/prediction_metrics.csv
```

---

# Cara Menambahkan Dataset Baru

Jika ingin menambahkan data baru:

1. Tambahkan dokumen putusan baru ke folder:

```text
data/raw/
```

2. Jalankan ulang:

```text
Tahap 2
Tahap 3
Tahap 4
Tahap 5
```

Model akan dilatih ulang menggunakan seluruh dataset terbaru.

---

# Hasil Evaluasi

Model yang digunakan:

| Model             | Tugas      |
| ----------------- | ---------- |
| TF-IDF + Cosine   | Retrieval  |
| IndoBERT + Cosine | Retrieval  |
| TF-IDF + SVM      | Prediction |
| TF-IDF + Reuse    | Prediction |

---

# Author

**Fauzil Adhim - 202310370311364**

**M.Deanova Wishal Andika -  202310370311384**

Program Studi Informatika 
Universitas Muhammadiyah Malang

---

Copyright © 2026
