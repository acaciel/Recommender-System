# Laporan Proyek Machine Learning - Anissa Shanniyah Aprilia

## Project Overview

Musik adalah bagian penting dalam kehidupan sehari-hari, dan dengan banyaknya lagu yang tersedia secara digital, pengguna kerap kesulitan menemukan lagu yang sesuai dengan selera mereka. Untuk mengatasi hal tersebut, sistem rekomendasi lagu menjadi solusi efektif dalam memberikan pengalaman mendengarkan yang lebih personal.

Proyek ini bertujuan membangun sistem rekomendasi lagu berdasarkan kemiripan fitur audio seperti energi, tempo, dan danceability dari lagu-lagu populer. Pendekatan yang digunakan adalah content-based filtering menggunakan cosine similarity antar fitur numerikal.

## Business Understanding

### Problem Statements

- Bagaimana cara merekomendasikan lagu yang mirip dengan lagu favorit pengguna berdasarkan fitur audio?
- Bagaimana membuat sistem yang mampu menyarankan lagu-lagu populer lainnya dengan karakteristik yang serupa?

### Goals

- Mengembangkan sistem rekomendasi lagu berdasarkan kemiripan fitur numerik yang tersedia dalam dataset.
- Menyediakan antarmuka sederhana bagi pengguna untuk memilih lagu dan melihat daftar lagu yang direkomendasikan.

### Solution Statements

- Menggunakan pendekatan **content-based filtering** dengan menghitung kemiripan cosine antar fitur numerikal dari lagu.
- Menyediakan fungsi rekomendasi yang menerima input judul lagu dan mengeluarkan daftar lagu yang mirip.
- Membangun antarmuka pengguna menggunakan `ipywidgets` agar rekomendasi bisa dicoba langsung di notebook.

## Data Understanding

Dataset yang digunakan adalah `top10s.csv`, yang berisi daftar lagu-lagu populer dari tahun 2010 hingga 2019. Dataset ini mencakup berbagai fitur audio yang diperoleh dari platform streaming seperti Spotify.

Beberapa variabel dalam dataset:

- `title`: judul lagu
- `artist`: nama artis
- `top genre`: genre utama
- `year`: tahun rilis
- `bpm`: beats per minute (tempo lagu)
- `nrgy`: energi lagu
- `dnce`: danceability
- `dB`: loudness (desibel)
- `live`: live performance score
- `val`: valence (positivitas lagu)
- `dur`: durasi lagu dalam detik
- `acous`, `spch`, `pop`: fitur-fitur audio tambahan

## Data Preparation

Langkah-langkah yang dilakukan:

- Melakukan normalisasi pada fitur numerik menggunakan `MinMaxScaler` agar berada dalam skala yang seragam (0–1).
- Menyiapkan subset fitur numerik (`bpm`, `nrgy`, `dnce`, `val`, `dur`, `acous`, `spch`, `pop`) sebagai dasar penghitungan kemiripan antar lagu.

Normalisasi penting agar tidak ada fitur yang mendominasi dalam penghitungan similarity.

## Modeling

Pendekatan yang digunakan adalah:

### Content-Based Filtering

- Menggunakan **cosine similarity** untuk mengukur kemiripan antar lagu berdasarkan vektor fitur numerik yang telah dinormalisasi.

Fungsi Rekomendasi berdasarkan lagu:
```python
def recommend_by_title(song_title, df, similarity_matrix, top_n=10):
```
Cara Kerja:
- Sistem mengambil fitur numerik lagu yang dipilih user (seperti bpm, nrgy, dnce, dll).
- Lalu menghitung cosine similarity terhadap semua lagu lain di dataset.
- Output: 10 lagu yang paling mirip dari segi karakteristik konten (energi, tempo, suasana, dll) — meskipun artis atau judulnya berbeda.

Fungsi Rekomendasi berdasarkan genre:
```python
def recommend_by_genre(genre, df, similarity_matrix, top_n=10):
```
Cara Kerja:
- Sistem memilih satu lagu dari genre tersebut.
- Kemudian mencari lagu-lagu lain yang mirip secara konten dengan lagu itu.
- Cocok untuk menemukan lagu mirip dari genre favorit.

Fungsi Rekomendasi berdasarkan artis:
```python
def recommend_by_artist(artist, df, similarity_matrix, top_n=10):
```
Cara Kerja:
- Sistem mengambil semua lagu dari artis yang dipilih, lalu menghitung rata-rata representasi kontennya.
- Lalu sistem mencari lagu lain yang mirip dengan gaya keseluruhan artis tersebut.
- Cocok untuk menemukan lagu mirip dengan “cita rasa” lagu-lagu seorang artis.
