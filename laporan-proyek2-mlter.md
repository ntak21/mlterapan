# Laporan Proyek Machine Learning - Ariftra Rahmawati

## Project Overview

Saat ini industri perfilman semakin maju seiring pertumbuhan platform streaming yang memberikan kemudahan akses bagi pengguna ke berbagai jenis tontonan. Dengan adanya platform streaming, pengguna memiliki pilihan film yang hampir tak terbatas. Akan tetapi, dengan begitu banyaknya pilihan, pengguna sering kali merasa kewalahan dalam memilih film yang sesuai dengan preferensi mereka. Oleh karena itu, muncul kebutuhan akan sistem rekomendasi film yang dapat membantu pengguna menemukan konten yang relevan dan menarik.

Salah satu pendekatan model yang terbukti efektif dalam meningkatkan engagement pengguna dan mengurangi churn rate adalah model berbasis kolaboratif dan konten.Bentuk model tersebut digunakan oleh salah satu perusahaan layanan streaming terbesar, yaitu Netflix. Pembuatan sistem rekomendasi diharapkan tidak hanya meningkatkan kepuasan pengguna tetapi juga dapat meningkatkan loyalitas pelanggan dan retensi pengguna, yang pada gilirannya memberikan keuntungan bisnis yang signifikan bagi penyedia layanan streaming.


Referensi: [How Netflix’s Recommendations System Works](https://help.netflix.com/en/node/100639)

## Business Understanding

- **Problem statement**
  
  Berdasarkan kondisi yang telah diuraikan sebelumnya, akan dikembangkan sebuah sistem rekomendasi film untuk menjawab permasalahan berikut.

  - Bagaimana cara membuat sistem rekomendasi film berdasarkan data riwayat preferensi pengguna itu sendiri?
  - Bagaimana membuat sistem rekomendasi film berdasarkan rating dan data pengguna lain yang memiliki minat serupa?

- **Goals**
  - Membuat sistem rekomendasi film berdasarkan data riwayat preferensi pengguna itu sendiri.
  - Membuat sistem rekomendasi film berdasarkan rating dan data pengguna lain yang memiliki minat serupa.

- **Solution Statement**

  Untuk membuat sistem rekomendasi film berdasarkan data riwayat preferensi diberikan oleh pengguna itu sendiri, akan digunakan metode Content Based Filtering. Algoritma Content Based Filtering akan mempelajari minat pengguna dan menyarankan item serupa yang pernah disukai di masa lalu atau sedang dilihat di masa kini.

  Untuk membuat sistem rekomendasi film berdasarkan berdasarkan rating dan data pengguna lain yang memiliki minat serupa, akan digunakan metode Collaborative Filtering. Collaborative Filtering merupakan metode yang mengandalkan data pengguna lain yang memiliki minat/preferensi serupa dengan melakukan pencarian pola kesamaan dan perbedaan dalam pilihan film untuk memberikan rekomendasi yang relevan.

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

 ### Exploratory Data Analysis
- Univariate Analysis
  Analisis univariat dilakukan untuk mengeksplorasi distribusi rating film:
  ![image](https://github.com/user-attachments/assets/727b7a62-dc15-4d8c-83d9-942ad788b017)
  - Distribusi Rating: Histogram ini memvisualisasikan jumlah rating yang diterima pada setiap tingkat rating. Rating berkisar dari 0.5 hingga 5.0, biasanya dalam interval 0.5.
  - Distribusi Rating Terbanyak: Rating yang paling sering diberikan oleh pengguna adalah 4.0. Hal ini menunjukkan kecenderungan pengguna untuk memberikan rating yang positif, menandakan kepuasan umum terhadap film yang mereka tonton.
 
    
- Multivariate Analysis
  Analisis multivariat dilakukan untuk memahami hubungan antara variabel:
  ![image](https://github.com/user-attachments/assets/bc4b6281-f5ab-4082-8ed6-70689bb703eb)
  - Jumlah Film Berdasarkan Genre: Barplot ini menunjukkan berapa banyak film yang terdapat dalam setiap genre. Dari plot ini, kita dapat melihat bahwa genre Drama dan Komedi adalah yang paling umum dalam dataset.

## Data Preparation

### Menggabungkan tabel
Pada tahap ini, tabel-tabel dari dataset digabungkan untuk menghasilkan sebuah dataframe terintegrasi. Tabel movies dan links digabungkan untuk menyertakan ID IMDb dan TMDb bersama dengan detail film lainnya. Selanjutnya, tabel ratings dan tags digabungkan untuk mengaitkan penilaian pengguna dengan tag yang mereka berikan. Akhirnya, semua data ini digabungkan menjadi satu dataframe komprehensif yang mencakup semua informasi relevan untuk analisis lebih lanjut.

### Menangani Missing Value
Untuk menangani nilai yang hilang dalam dataset:
- Kolom genres: Nilai null di kolom ini diisi dengan string 'Unknown' untuk memastikan setiap film memiliki genre yang terdefinisi.
- Kolom imdbId dan tmdbId: Baris dengan nilai null pada kolom-kolom ini dihapus karena informasi ID ini penting untuk identifikasi film di platform eksternal.
- Kolom tag: Nilai null di kolom tag diisi dengan string kosong untuk menghindari kehilangan informasi tentang tag yang mungkin tidak ada.

### Ekstraksi Fitur menggunakan TF-IDF

TF-IDF (Term Frequency-Inverse Document Frequency) adalah teknik yang digunakan untuk mengubah teks menjadi representasi numerik yang dapat digunakan dalam model pembelajaran mesin. Tujuan utamanya adalah untuk menilai seberapa penting sebuah kata dalam dokumen relatif terhadap seluruh koleksi dokumen.

Untuk model content-based filtering yang akan digunakan berikutnya, TF-IDF berfungsi merepresentasikan konten item (film) berdasarkan fitur teks seperti genre dan tags.

Selanjutnya digunakan juga AnnoyIndex untuk melakukan pencarian terdekat berdasarkan representasi TF-IDF dari film untuk menemukan film yang mirip. Annoy dapat mencari nearest neighbors dengan cepat di ruang fitur berdimensi tinggi menggunakan jarak sudut (cosine similarity).

Parameter yang digunakan:
- stop_words='english':
  Parameter ini menginstruksikan TfidfVectorizer untuk menghapus stop words bahasa     Inggris dari teks. Stop words adalah kata-kata umum seperti "the", "and", "is", yang biasanya tidak membawa informasi penting untuk tujuan analisis teks. Menghilangkan stop words dapat meningkatkan kualitas fitur yang dihasilkan karena kata-kata umum ini tidak memberikan banyak informasi tentang konten teks.
- f: Menentukan jumlah dimensi atau fitur yang digunakan dalam indeks Annoy.
- 'angular': Metrik jarak yang digunakan untuk mengukur kesamaan antara item dalam indeks. 'angular' mengacu pada jarak sudut atau cosine similarity.

###  Train Test Split
Train-test split adalah langkah penting dalam proses pengembangan model yang membagi dataset menjadi dua subset: satu untuk melatih model (train set) dan satu untuk menguji model (test set). Pada kasus ini, kita akan menggunakan proporsi pembagian data latih dan data uji sebesar 80:20 dengan fungsi train_test_split dari sklearn.

## Modeling
Pada tahap ini, akan dikembangkan model machine learning dengan dua algoritma. Kemudian, akan dievaluasi performa masing-masing algoritma berdasarkan akurasinya untuk menjawab problem statement dari tahap business understanding.

Dua algoritma yang akan digunakan adalah:
- Content Based Filtering
- Collaborative Filtering

### Content Based Filtering
Content-Based Filtering adalah teknik dalam sistem rekomendasi yang memanfaatkan fitur atau atribut dari item untuk memberikan rekomendasi. Dalam konteks rekomendasi film, metode ini menggunakan deskripsi atau atribut film—seperti genre, aktor—untuk menemukan film yang mirip dengan film yang telah disukai pengguna sebelumnya.

Beberapa parameter yang digunakan:
- title: Judul film yang dijadikan acuan untuk mendapatkan rekomendasi.
- k: Jumlah rekomendasi teratas yang ingin diambil.
- idx: Indeks film yang relevan berdasarkan judul.
- neighbors = index.get_nns_by_item(idx, k): Menggunakan indeks Annoy untuk mendapatkan k item terdekat dari indeks film.
  
Hasil Top 10 Recommendation:
![image](https://github.com/user-attachments/assets/8211fd44-edcb-439f-a385-85a3f0d1b086)


  
### Collaborative Filtering
Collaborative Filtering adalah metode dalam sistem rekomendasi yang memanfaatkan preferensi dan perilaku pengguna lain untuk memberikan rekomendasi. Berbeda dengan content-based filtering, yang fokus pada atribut item, collaborative filtering berfokus pada interaksi antara pengguna dan item. 

Dalam konteks film, collaborative filtering dapat dilakukan dengan dua pendekatan utama:
- User-Based Collaborative Filtering: Mencari pengguna yang memiliki selera yang sama dengan pengguna target dan merekomendasikan film yang disukai oleh pengguna serupa.
- Item-Based Collaborative Filtering: Mencari film yang mirip dengan film yang disukai pengguna dan merekomendasikan film serupa berdasarkan kesamaan dengan film yang telah dinilai atau ditonton oleh pengguna.
  
Pada kasus ini jenis collaborative filtering yang digunakan adalah Item-Based Collaborative Filtering dengan memanfaatkan algoritma SVD. 

Singular Value Decomposition (SVD) adalah teknik dekomposisi matriks yang digunakan dalam collaborative filtering untuk menemukan pola laten antara pengguna dan item. SVD memecah matriks pengguna-item menjadi tiga matriks: matriks pengguna, matriks item, dan matriks diagonal dari nilai singular, sehingga membantu mengidentifikasi hubungan tersembunyi di antara mereka.. 

Beberapa parameter yang digunakan:
- rating_scale=(0.5, 5.0): Menentukan skala rating dalam dataset. Dalam hal ini, rating berkisar dari 0.5 hingga 5.0.
- load_from_df: Mengonversi DataFrame ke dalam format yang dapat digunakan oleh scikit-surprise. Data harus memuat ID pengguna, ID film, dan rating.
- SVD(): SVD atau Singular Value Decomposition merupakan algoritma rekomendasi yang populer dalam collaborative filtering. SVD membagi matriks rating pengguna-item menjadi beberapa matriks yang lebih kecil untuk memprediksi rating yang hilang.

Hasil Top 10 Recommendations:
![image](https://github.com/user-attachments/assets/a64f815d-50cd-4e9d-aa95-b1fa3bb0fbf0)


## Evaluation

Pada tahap evaluasi digunakan parameter precision and recall untuk mengevaluasi hasil model Content Based Filtering, dan juga RMSE dan MAE untuk menganalisis performa model.
- Precision : Mengukur proporsi item yang direkomendasikan yang benar-benar relevan dengan preferensi pengguna.
  
$$
\text{Precision} = \frac{\text{Jumlah item relevan yang direkomendasikan}}{\text{Jumlah total item yang direkomendasikan}}
$$


- Recall: Mengukur proporsi item yang relevan yang berhasil direkomendasikan dari total item yang relevan.

  
$$
\text{Precision} = \frac{\text{Jumlah item relevan yang direkomendasikan}}{\text{Jumlah total item yang relevan}}
$$

![image](https://github.com/user-attachments/assets/4fe156c7-cdaf-4914-b5ad-223d92bf366d)

Nilai precision yang dihasilkan pada model content based filtering pada kasus ini adalah 0.17, dimana nilai tersebut masih relatif kecil. Hal ini mungkin disebabkan oleh keterbatasan fitur yang digunakan yaitu genre dan tag (dalam dataset tidak tersedia attribut lain seperti actor, director, atau description). Sedangkan nilai recall yang didapatkan adalah 1.00, menunjukkan bahwa sistem berhasil menemukan semua film yang relevan dalam daftar rekomendasi teratas yang seharusnya dimiliki oleh pengguna.

- RMSE (Root Mean Squared Error): Mengukur kesalahan rata-rata kuadrat prediksi model. RMSE memberikan penekanan lebih besar pada kesalahan yang lebih besar dan lebih sensitif terhadap outlier.
- MAE (Mean Absolute Error): Mengukur kesalahan rata-rata absolut dari prediksi model. MAE memberikan gambaran umum tentang seberapa akurat model dalam hal kesalahan rata-rata.
- 
![image](https://github.com/user-attachments/assets/27497b32-ac36-44a3-8ccb-e85c3630b816)


Berdasarkan hasil evaluasi model, Collaborative Filtering menunjukkan performa yang cukup baik yaitu dengan RMSE 0.477569 dan MAE 0.371782.

