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
- Menyediakan interface sederhana bagi pengguna untuk memilih lagu dan melihat daftar lagu yang direkomendasikan.

### Solution Statements

- Menggunakan pendekatan **content-based filtering** dengan menghitung kemiripan cosine antar fitur numerikal dari lagu.
- Menyediakan fungsi rekomendasi yang menerima input judul lagu dan mengeluarkan daftar lagu yang mirip.
- Membangun user interface menggunakan `ipywidgets` agar rekomendasi bisa dicoba langsung di notebook.

## Data Understanding
- Dataset Top Spotify songs from 2010-2019-BY YEAR (https://www.kaggle.com/datasets/leonardopena/top-spotify-songs-from-20102019-by-year?select=top10s.csv)
- Dataset yang digunakan adalah `top10s.csv`, yang berisi daftar lagu-lagu populer dari tahun 2010 hingga 2019.
- Jumlah data: Dataset terdiri dari 603 baris dan 15 kolom
- tidak terdapat missing values ataupun data duplikat

Dataset ini mencakup berbagai fitur audio yang diperoleh dari platform streaming seperti Spotify. Beberapa variabel dalam dataset:

- `Unnamed: 0` : kolom nomor
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
- `acous`: acousticness
- `spch`: speechiness
- `pop`: popularity

### Exploratory Data Analysis
EDA dilakukan untuk memahami distribusi data dan hubungan antar fitur, serta menentukan langkah preprocessing yang sesuai.

- Distribusi Lagu per Tahun
Visualisasi ini menunjukkan jumlah lagu populer yang tercatat dalam dataset berdasarkan tahun rilisnya.
- 10 Genre Terpopuler
Visualisasi menggunakan barplot untuk menunjukkan 10 genre terpopuler berdasarkan frekuensi kemunculannya dalam dataset.
- Korelasi Antar Fitur Numerik Lagu
Korelasi antar fitur menggunakan heatmap membantu untuk memahami hubungan antar variabel dalam lagu. Contohnya, terdapat korelasi positif antara `bpm` dan `nrgy`, yang menunjukkan bahwa lagu dengan tempo cepat cenderung memiliki energi yang lebih tinggi. Selain itu, terdapat korelasi positif antara `val` dan `dnce`, yang berarti lagu yang enak untuk berdansa biasanya memiliki suasana yang lebih ceria. Sementara itu, korelasi negatif antara `nrgy` dan `acous` mengindikasikan bahwa lagu akustik cenderung memiliki energi yang lebih rendah. Secara keseluruhan, korelasi antar fitur tidak terlalu tinggi, sehingga semua fitur tetap bisa digunakan karena memberikan informasi yang berbeda satu sama lain.

## Data Preparation

Langkah-langkah yang dilakukan:
- Menghapus kolom `Unnamed: 0`
- Mengubah nama kolom ke huruf kecil
- Melakukan normalisasi pada fitur numerik menggunakan `MinMaxScaler` agar berada dalam skala yang seragam (0–1).
- Menyiapkan subset fitur numerik (`bpm`, `nrgy`, `dnce`, `val`, `dur`, `acous`, `spch`, `live`) sebagai dasar penghitungan kemiripan antar lagu.

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

Contoh Output:
![image](https://github.com/user-attachments/assets/d1dc7414-39cc-4749-8ed8-a7dd75091ed5)

Fungsi Rekomendasi berdasarkan genre:
```python
def recommend_by_genre(genre, df, similarity_matrix, top_n=10):
```
Cara Kerja:
- Sistem memilih satu lagu dari genre tersebut.
- Kemudian mencari lagu-lagu lain yang mirip secara konten dengan lagu itu.
- Cocok untuk menemukan lagu mirip dari genre favorit.

Contoh Output:
![image](https://github.com/user-attachments/assets/588a5dff-56db-497b-a5d9-c6dddbb9949f)

Fungsi Rekomendasi berdasarkan artis:
```python
def recommend_by_artist(artist, df, similarity_matrix, top_n=10):
```
Cara Kerja:
- Sistem mengambil semua lagu dari artis yang dipilih, lalu menghitung rata-rata representasi kontennya.
- Lalu sistem mencari lagu lain yang mirip dengan gaya keseluruhan artis tersebut.
- Cocok untuk menemukan lagu mirip dengan “cita rasa” lagu-lagu seorang artis.

Contoh Output:
![image](https://github.com/user-attachments/assets/36a564ca-7a51-469e-af4b-445bce191cdb)

## Evaluasi
Untuk mengevaluasi performa sistem rekomendasi berbasis konten, dilakukan simulasi **precision@10** terhadap 3 aspek utama:

1. **Genre (Top Genre)**
2. **Artis**
3. **BPM (Beat Per Minute)**

Evaluasi dilakukan dengan mengambil 20 lagu secara acak sebagai query, lalu dihitung proporsi dari 10 rekomendasi teratas yang memiliki karakteristik serupa.

### 1. Precision Berdasarkan Artis

Evaluasi ini mengukur seberapa sering rekomendasi berasal dari artis yang sama.

- **Rata-rata Precision@10**: **0.05**

📌 *Insight*: Rekomendasi dari artis yang sama sangat rendah, yaitu hanya sekitar 5%. Ini menunjukkan bahwa model lebih menekankan kemiripan dari segi fitur lagu (konten), bukan identitas artis. Hasil ini juga mencerminkan kemampuan sistem untuk memberikan rekomendasi lintas artis, yang bisa membantu dalam diversifikasi lagu bagi pengguna.

---

### 2. Precision Berdasarkan Genre

Rekomendasi dianggap relevan jika lagu yang direkomendasikan memiliki genre yang sama dengan lagu input.

- **Rata-rata Precision@10**: **0.57**  

📌 *Insight*: Sistem cukup baik dalam mengelompokkan lagu berdasarkan genre. Hal ini menunjukkan bahwa fitur numerik seperti `bpm`, `val`, dan `nrgy` dapat menangkap karakteristik umum dari genre yang sama.

---

### 3. Precision Berdasarkan BPM

Rekomendasi dianggap relevan jika BPM-nya dalam rentang ±5 dari lagu input.

- **Rata-rata Precision@10**: **0.33**

📌 *Insight*: Sekitar 33% rekomendasi memiliki tempo yang sangat mirip. Meskipun tidak terlalu tinggi, ini masih menunjukkan bahwa tempo merupakan salah satu fitur yang ikut mempengaruhi hasil rekomendasi. Namun, fitur lain seperti energi (`nrgy`) dan danceability (`dnce`) kemungkinan berkontribusi lebih besar pada kemiripan yang dihitung.

---

### 🧠 Kesimpulan Evaluasi

- Sistem content-based ini cukup efektif dalam mengenali genre dan sebagian besar karakteristik musikal dari lagu.
- Sistem cocok untuk digunakan dalam skenario *“temukan lagu dengan suasana/karakter mirip”*, bukan *“temukan lagu lain dari artis ini”*.
