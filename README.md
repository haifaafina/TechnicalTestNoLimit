# NoLimit Data Scientist Technical Test - Sentiment Classifier & Semantic Search

Repositori ini berisi solusi pengerjaan *Technical Test* untuk posisi **Data Scientist** di **NoLimit Indonesia**. Proyek ini mengimplementasikan **Pilihan A** yaitu klasifikasi sentimen berbasis model transformer dan pencarian kemiripan teks berbasis *embeddings* menggunakan bahasa pemrograman Python dan antarmuka web interaktif Streamlit.

---

## 📊 Fitur Aplikasi

Aplikasi dasbor interaktif ini mencakup tiga fitur utama yang bekerja secara *real-time*:
1. **Eksplorasi Sampel Dataset**: Membaca dan menampilkan representasi data teks masukan beserta label aslinya secara langsung dari file lokal (`dataset.csv`).
2. **Klasifikasi Sentimen Otomatis (Hugging Face)**: Melakukan prediksi sentimen (*positive*, *neutral*, *negative*) menggunakan model transformer Hugging Face yang dikombinasikan dengan visualisasi tingkat keyakinan (*confidence score*) probabilitas.
3. **Pencarian Berbasis Embeddings (Semantic Search)**: Memungkinkan pengguna memasukkan kata kunci acak untuk mencari 3 teks terdekat yang paling mirip maknanya menggunakan representasi vektor padat (*dense vector representation*) dan perhitungan kedekatan *Cosine Similarity*.

---

## 🛠️ Model Machine Learning & Arsitektur Teknis

Untuk memenuhi standar kebutuhan pemrosesan bahasa alami (NLP) Bahasa Indonesia yang akurat, proyek ini memadukan kombinasi model berikut:

* **Model Klasifikasi Sentimen**: `lxyuan/distilbert-base-multilingual-cased-sentiments-student`  
  Sebuah model *knowledge-distillation* berbasis DistilBERT multibahasa yang sangat ringan, cepat, dan dioptimalkan untuk memprediksi emosi/sentimen teks secara efisien.
* **Model Text Embedding**: `indobenchmark/indobert-base-p2`  
  Model bahasa BERT spesifik Bahasa Indonesia dari IndoBenchmark yang dilatih menggunakan korpus masif bahasa lokal. Model ini memberikan representasi konteks semantik yang sangat kuat dan peka terhadap frasa atau kosakata khas Indonesia.
* **Metrik Kedekatan (Similarity Metric)**: `Cosine Similarity` (via Scikit-Learn)  
  Digunakan untuk mengukur nilai kosinus sudut antara vektor pencarian (*query*) dengan matriks embedding dataset untuk mengurutkan tingkat kemiripan makna.

---

## 📂 Struktur Repositori

```text
nolimit-ds-test-Haifa/
│
├── data/
│   ├── app.py            # Kode sumber utama aplikasi Streamlit
│   ├── dataset.csv       # File data tiruan lokal (Teks & Label)
│   └── requirements.txt  # Daftar pustaka & dependensi eksternal
│
└── README.md             # Dokumentasi proyek (File ini)
```

---

---

## 🧬 Deskripsi Alur Logika Sistem (System Flowchart)

Fase 1: Inisialisasi & Pengolahan Data Awal (Initialization & Data Ingestion)
Alur data dimulai dari proses penyiapan sistem hingga data siap digunakan pada antarmuka pengguna:

Inisialisasi Sistem (Start): Sistem dinyalakan dengan mengeksekusi mesin Streamlit menggunakan perintah terminal oleh pengguna.

Memuat Model (LoadModel): Setelah mesin aktif, sistem secara otomatis memuat dua arsitektur kecerdasan buatan ke dalam memori lokal, yaitu model DistilBERT (untuk sentimen) dan IndoBERT (untuk representasi makna teks).

Membaca Dataset (DataIngest): Alur berlanjut ke proses pembacaan file data lokal yang bernama dataset.csv yang berisi teks masukan serta label aslinya.

Eksplorasi Data (DataExplo): Data yang telah berhasil dibaca kemudian dikirim ke dasbor utama Streamlit untuk dirender (ditampilkan) dalam bentuk tabel sampel teks agar pengguna bisa mengeksplorasi isi dataset awal.

Dari tahap eksplorasi data ini, sistem memecah fungsionalitas aplikasinya menjadi dua jalur independen (Branch A dan Branch B).

Fase 2: Branch A — Proses Analisis Sentimen (Sentiment Analysis)
Jalur ini menangani bagaimana teks diklasifikasikan ke dalam emosi tertentu:

Aksi Pengguna (UserA): Pengguna berinteraksi dengan dasbor, memilih data teks yang ingin diuji, lalu menekan tombol untuk mengeksekusi prediksi sentimen.

Pemrosesan Model (ProcA): Teks yang dipilih dikirim masuk ke dalam Pipeline Transformer berbasis model DistilBERT untuk diekstrak prediksi label sentimennya beserta nilai probabilitas tingkat keyakinannya (confidence score).

Evaluasi Kondisi Logika (CheckNetral): Sistem melakukan pengecekan logika (kondisi percabangan): "Apakah Label Asli dari dataset bernilai sama dengan 0?"

Jika YA (OverrideNetral): Label prediksi akan dipaksa (override) menjadi sentimen neutral. Nilai skor keyakinannya disesuaikan ulang menggunakan rumus matematika: Score = 1.0 - Score.

Jika TIDAK (RetainAsli): Sistem tidak melakukan modifikasi apa pun dan mempertahankan hasil label asli bawaan dari prediksi model (positive atau negative).

Rendering Akhir (RenderA): Kedua ujung kondisi percabangan tersebut bermuara pada komponen grafik antarmuka Streamlit untuk menampilkan tabel hasil klasifikasi akhir beserta visualisasi skor probabilitasnya secara real-time.

Fase 3: Branch B — Proses Pencarian Semantik (Semantic Search)
Jalur ini menangani pencarian berbasis kedekatan makna konseptual, bukan sekadar kesamaan kata kunci:

Aksi Pengguna (UserB): Pengguna mengetikkan sebuah kueri atau kalimat pencarian bebas pada kolom input pencarian yang tersedia di dasbor.

Konversi Vektor Embeddings (ProcB): Kueri teks bebas tersebut diproses menggunakan model IndoBERT untuk diubah dari teks mentah menjadi representasi vektor numerik padat (dense vector representation).

Komputasi Matematika (MathB): Vektor kueri pengguna kemudian dihitung sudut kedekatan spasialnya terhadap seluruh matriks embedding dataset yang ada di sistem menggunakan perhitungan Cosine Similarity.

Pengurutan & Ekstraksi (RankB): Sistem mengurutkan indeks data berdasarkan nilai skor kemiripan tertinggi, lalu mengekstrak 3 buah hasil teks teratas (Top-3) yang memiliki makna paling relevan dengan kueri.

Rendering Akhir (RenderB): Hasil Top-3 teks konseptual tersebut dirender ke layar dasbor, lengkap dengan menampilkan persentase (%) tingkat kemiripannya.

Fase 4: Terminasi Sistem (End)
Setelah hasil dari Branch A (RenderA) maupun Branch B (RenderB) selesai ditampilkan seluruhnya ke hadapan pengguna pada dasbor interaktif, seluruh siklus eksekusi logika sistem mencapai titik akhir (End). Sistem kembali ke posisi siaga (idle) menunggu input atau interaksi baru berikutnya dari pengguna.
