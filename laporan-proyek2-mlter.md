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

 

 ### Exploratory Data Analysis


## Data Preparation


## Modeling


## Evaluation
