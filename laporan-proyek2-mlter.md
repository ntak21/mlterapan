# Laporan Proyek Machine Learning - Ariftra Rahmawati

## Project Overview

Saat ini industri perfilman semakin maju seiring pertumbuhan platform streaming yang memberikan kemudahan akses bagi pengguna ke berbagai jenis tontonan. Dengan adanya platform streaming, pengguna memiliki pilihan film yang hampir tak terbatas. Akan tetapi, dengan begitu banyaknya pilihan, pengguna sering kali merasa kewalahan dalam memilih film yang sesuai dengan preferensi mereka. Oleh karena itu, muncul kebutuhan akan sistem rekomendasi film yang dapat membantu pengguna menemukan konten yang relevan dan menarik.

Salah satu pendekatan model yang terbukti efektif dalam meningkatkan engagement pengguna dan mengurangi churn rate adalah model berbasis kolaboratif dan konten.Bentuk model tersebut digunakan oleh salah satu perusahaan layanan streaming terbesar, yaitu Netflix. Pembuatan sistem rekomendasi diharapkan tidak hanya meningkatkan kepuasan pengguna tetapi juga dapat meningkatkan loyalitas pelanggan dan retensi pengguna, yang pada gilirannya memberikan keuntungan bisnis yang signifikan bagi penyedia layanan streaming.


Referensi: [How Netflix’s Recommendations System Works](https://help.netflix.com/en/node/100639)

## Business Understanding

- **Problem statement**
  
  Berdasarkan kondisi yang telah diuraikan sebelumnya, akan dikembangkan sebuah sistem rekomendasi film untuk menjawab permasalahan berikut.

 - Bagaimana cara membuat sistem rekomendasi film berdasarkan data riwayat preferensi dan rating film yang diberikan oleh pengguna itu sendiri?
 - Bagaimana membuat sistem rekomendasi film berdasarkan data pengguna lain yang memiliki minat serupa?

- **Goals**
  - Membuat sistem rekomendasi film berdasarkan data riwayat preferensi dan rating film yang diberikan oleh pengguna itu sendiri.
  - Mmembuat sistem rekomendasi film berdasarkan data pengguna lain yang memiliki minat serupa.

- **Solution Statement**

  Untuk membuat sistem rekomendasi film berdasarkan data riwayat preferensi dan rating film yang diberikan oleh pengguna itu sendiri, akan digunakan metode Content Based Filtering. Algoritma Content Based Filtering akan mempelajari minat pengguna dan menyarankan item serupa yang pernah disukai di masa lalu atau sedang dilihat di masa kini.
  
  Untuk membuat sistem rekomendasi film berdasarkan berdasarkan data pengguna lain yang memiliki minat serupa, akan digunakan metode Collaborative Filtering. Collaborative Filtering merupakan metode yang mengandalkan data pengguna lain yang memiliki minat/preferensi serupa dengan melakukan pencarian pola kesamaan dan perbedaan dalam pilihan film untuk memberikan rekomendasi yang relevan.

  Metrik evaluasi yang akan digunakan untuk menganalisis performa model adalah RMSE dan MAE.

## Data Understanding

Dataset yang digunakan mendeskripsikan aktivitas pemberian rating bintang 5 dan penandaan teks bebas dari MovieLens, sebuah layanan rekomendasi film.

- Dataset ini berisi 100836 rating dan 3683 tag untuk 9742 film.
- Dataset terbagi ke dalam file movies.csv, links.csv, ratings.csv, dan tags.csv.

dataset: https://grouplens.org/datasets/movielens/latest/

### Deskripsi Variabel

- "movies.csv" memilki attribut:
  - movieId' : Nomor unik yang merupakan identifikasi untuk masing-masing film dalam dataset.
  - 'title' : Judul dari film yang bersangkutan.
  - 'genres' : Genre-genre yang diwakili oleh film tersebut. Satu film bisa memiliki beberapa genre yang dipisahkan oleh karakter '|'.
- "links.csv" memiliki attribut:
  - 'movieId' : Nomor unik yang merupakan identifikasi untuk masing-masing film dalam dataset.
  - 'imdbId' : Nomor unik yang merupakan identifikasi untuk masing-masing film pada IMDb (Internet Movie Database).
  - 'tmdbId' : Nomor unik yang merupakan identifikasi untuk masing-masing film pada TMDb (The Movie Database).
- "ratings.csv" memiliki attribut:
  - 'userId' : Nomor unik yang merupakan identifikasi untuk masing-masing pengguna dalam dataset.
  - 'movieId' : Nomor unik yang merupakan identifikasi untuk masing-masing film dalam dataset.
  - 'rating' : Penilaian yang diberikan oleh pengguna untuk suatu film. Nilai rating berkisar antara 0.5 hingga 5.0 dalam interval 0.5.

- "tags.csv" memiliki attribut:
  - 'userId' : Nomor unik yang merupakan identifikasi untuk masing-masing pengguna dalam dataset.
  - 'movieId' : Nomor unik yang merupakan identifikasi untuk masing-masing film dalam dataset.
  - 'tag' : Kata kunci atau tag yang diberikan oleh pengguna untuk suatu film.

 ### Menggabungkan tabel
Pada tahap ini, tabel-tabel dari dataset digabungkan untuk menghasilkan sebuah dataframe terintegrasi. Tabel movies dan links digabungkan untuk menyertakan ID IMDb dan TMDb bersama dengan detail film lainnya. Selanjutnya, tabel ratings dan tags digabungkan untuk mengaitkan penilaian pengguna dengan tag yang mereka berikan. Akhirnya, semua data ini digabungkan menjadi satu dataframe komprehensif yang mencakup semua informasi relevan untuk analisis lebih lanjut.

### Menangani Missing Value
Untuk menangani nilai yang hilang dalam dataset:
- Kolom genres: Nilai null di kolom ini diisi dengan string 'Unknown' untuk memastikan setiap film memiliki genre yang terdefinisi.
- Kolom imdbId dan tmdbId: Baris dengan nilai null pada kolom-kolom ini dihapus karena informasi ID ini penting untuk identifikasi film di platform eksternal.
- Kolom tag: Nilai null di kolom tag diisi dengan string kosong untuk menghindari kehilangan informasi tentang tag yang mungkin tidak ada.

 ### Exploratory Data Analysis
- Univariate Analysis
  Analisis univariat dilakukan untuk mengeksplorasi distribusi rating film:
  -   Distribusi Rating: Histogram rating menunjukkan sebaran penilaian film yang   diberikan oleh pengguna. Plot ini menggambarkan jumlah film pada setiap tingkat rating dan memberikan wawasan tentang preferensi rating umum di dataset.
- Multivariate Analysis
  Analisis multivariat dilakukan untuk memahami hubungan antara variabel:
  -   Jumlah Film Berdasarkan Genre: Barplot menunjukkan jumlah film dalam setiap genre yang terdaftar di dataset. Ini memberikan gambaran tentang genre mana yang paling umum dan membantu dalam memahami preferensi genre secara keseluruhan.

## Data Preparation
###  Train Test Split
Train-test split adalah langkah penting dalam proses pengembangan model yang membagi dataset menjadi dua subset: satu untuk melatih model (train set) dan satu untuk menguji model (test set). Pada kasus ini, kita akan menggunakan proporsi pembagian data latih dan data uji sebesar 80:20 dengan fungsi train_test_split dari sklearn.

## Modeling
Pada tahap ini, akan dikembangkan model machine learning dengan dua algoritma. Kemudian, akan dievaluasi performa masing-masing algoritma berdasarkan akurasinya untuk menjawab problem statement dari tahap business understanding.

Dua algoritma yang akan digunakan adalah:
- Content Based Filtering
- Collaborative Filtering

### 

## Evaluation
