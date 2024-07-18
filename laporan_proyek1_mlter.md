# Laporan Proyek Machine Learning - Ariftra Rahmawati

## Domain Proyek

Maternal Health merupakan aspek penting dari kesehatan masyarakat, dengan angka kematian dan morbiditas ibu menjadi indikator utama kualitas sistem kesehatan global. Pentingnya pemantauan kesehatan maternal telah banyak dibahas dalam literatur medis, salah satunya oleh WHO, yang menekankan perlunya pemantauan kesehatan maternal yang komprehensif untuk mengurangi angka kematian ibu (WHO, 2019).

Komplikasi terkait kehamilan dapat menimbulkan risiko signifikan bagi ibu dan janin, yang berpotensi mengakibatkan masalah kesehatan yang serius atau bahkan kematian. Untuk mengurangi risiko tersebut, diperlukan upaya pencegahan yang efektif. Salah satu pendekatan penting adalah memprediksi tingkat risiko komplikasi kehamilan. Dengan memprediksi risiko ini, penyedia layanan kesehatan dapat memprioritaskan dan mengelola kehamilan berisiko tinggi dengan lebih efektif, serta memastikan layanan kesehatan yang lebih baik.

Referensi: World Health Organization: WHO. (2024, April 26). Maternal mortality. https://www.who.int/news-room/fact-sheets/detail/maternal-mortality

## Business Understanding
### Problem statement
Berdasarkan kondisi yang telah diuraikan sebelumnya, akan dikembangkan sebuah sistem prediksi risiko kesehatan maternal untuk menjawab permasalahan berikut.

- Dari serangkaian fitur (faktor kesehatan) yang ada, apakah yang paling berpengaruh terhadap risiko kesehatan maternal?
- Bagaimana risiko kehamilan bagi ibu dengan kondisi kesehatan tertentu?

### Goals
- Mengetahui fitur yang paling berkorelasi dengan risiko kesehatan maternal
- Membuat model machine learning yang dapat memprediksi risiko kesehatan maternal seakurat mungkin berdasarkan fitur-fitur yang ada.

### Solution Statement
Untuk mengetahui fitur yang paling berpengaruh terhadap risiko kesehatan maternal akan dilakukan analisis korelasi antar fitur numerik dengan fitur target (risk level) menggunakan correlation matrix.  
Risiko kesehatan dengan kategori tingkatan tertentu termasuk ke dalam permasalahan klasifikasi. Oleh karena itu, metodologi pada proyek ini adalah membangun model klasifikasi dengan Risk Level sebagai target. Pengembangan model akan menggunakan dua algoritma machine learning yaitu Random Forest dan Logistic Regression. Dari kedua model akan dipilih satu model dengan akurasi terbaik. Metrik evaluasi yang akan digunakan pada kasus ini adalah accuracy score menggunakan sklearn.metrics.
## Data Understanding
Dataset yang digunakan adalah Maternal Health Risk Data yang diambil dari situs Kaggle. Data pada dataset ini didapatkan dari berbagai rumah sakit, klinik komunitas, pusat kesehatan maternal melalui sistem pemantauan risiko berbasis IoT.

sumber: https://www.kaggle.com/datasets/csafrit2/maternal-health-risk-data/data

- Dataset terdiri dari 1014 baris data risiko kehamilan dengan berbagai kondisi kesehatan yang menyertai.
- Terdapat 7 kolom, yaitu: Age, SystolicBP, DiastolicBP, BS, BodyTemp, HeartRate, dan RiskLevel.
- Fitur numerik: Age, SystolicBP, DiastolicBP, BS, BodyTemp, dan HeartRate
- Fitur non numerik: RiskLevel
- RiskLevel merupakan fitur target

![image](https://github.com/user-attachments/assets/b290b0a1-eb50-4b47-b21e-deded2487fc6)


### Deskripsi Variabel 
##Deskripsi Variabel

- Age: usia dalam tahun pada saat ibu hamil
- SystolicBP: tekanan darah saat jantung berkontraksi (sistole) (mmHg)
- DiastolicBP:  tekanan darah saat jantung berelaksasi (diastole) (mmHg)
- BS: Kadar glukosa darah dalam konsentrasi molar, mmol/L.
- BodyTemp: suhu badan dalam fahrenheit
- HeartRate: detak jantung saat istirahat normal dalam bit per menit
- Risk Level: tingkat intensitas risiko kehamilan yang diprediksi (low risk, mid risk, high risk)

### Exploratory Data Analysis
Beberapa tahapan yang dilakukan dalam Exploratory Data Analysis yaitu:
1) Menangani Missing Value
2) Menangani outliers dengan metode IQR
Outliers adalah nilai-nilai ekstrem yang dapat mempengaruhi hasil analisis dan pemodelan. Metode Interquartile Range (IQR) digunakan untuk mendeteksi dan menangani outliers. IQR adalah rentang antara kuartil pertama (Q1) dan kuartil ketiga (Q3) dari distribusi data. Nilai yang berada di bawah (Q1 - 1.5 * IQR) atau di atas (Q3 + 1.5 * IQR) dianggap sebagai outliers. Dengan menghapus outliers, kita dapat mengurangi pengaruh negatif mereka pada model.

![image](https://github.com/user-attachments/assets/28070f66-bed7-42f4-9e26-38d690977767)

Dari visualisasi boxplot terhadap fitur-fitur di atas, terlihat bahwa data 'Age', 'SystolicBP', 'BS', 'BodyTemp', dan 'HeartRate' memiliki outliers. Namun apabila dilihat pada deskripsi data, fitur BodyTemp memiliki rentang yang normal, yaitu 98 hingga 103 Fahrenheit sehingga suhu badan pada sekitar nilai maksimum tidak dapat dianggap sebagai outliers. Begitu pula dengan Blood Sugar dengan nilai hingga 19 mmol/L yang masih mungkin, meskipun mengindikasikan seseorang memiliki gula darah sangat tinggi (hiperglikemia).

Sehingga outliers yang perlu dihilangkan pada data adalah HeartRate.

4) Univariate analysis terhadap fitur kategori dan numerik

![image](https://github.com/user-attachments/assets/13b9e4cc-7b20-432c-913c-6b41aac77f28)

Dari analisis terhadap risk level, didapatkan bahwa terdapat 3 kategori pada RiskLevel, yaitu: Low Risk, Mid Risk, dan High Risk. Dari data persentase dapat disimpulkan bahwa lebih dari 50% sampel memiliki risiko kehamilan rendah, dan sekitar 34% memilki risiko kehamilan sedang.
![image](https://github.com/user-attachments/assets/723d13ae-fa9f-4f81-8c36-a879919e5bbd)

Dari analisis terhadap fitur numerik didapatkan informasi bahwa:
- Usia ibu hamil paling banyak berusia sekitar 15 hingga 20 tahun.
- Mayoritas suhu badan berada pada nilai normal yaitu sekitar 98 F atau 36 derajat Celcius.
- Menurut informasi yang dilansir dari Halodoc, tekanan darah normal berkisar pada 90–120 mmHg (sistolik) dan 60–80 mmHg (diastolik). Berdasarkan plot, sebagian besar sampel memiliki tekanan darah pada rentang normal.
- Ambang batas atas gula darah normal adalah 11,1 mmol/L. Pada histogram terlihat bahwa sebagian besar sampel memiliki gula darah normal.
- Mayoritas sampel memilki suhu badan dan heart rate normal.

4) Multivariate analysis menggunakan corrplot
   ![image](https://github.com/user-attachments/assets/780d6d14-492f-472e-bc2f-de4660c7b880)

Correlation matrix menunjukkan:
- korelasi yang kuat antara fitur SystolicBP dan DiastolicBP.
- korelasi sedang antara usia dengan tekanan darah, usia dengan gula darah, dan tekanan darah dengan gula darah.
- variabel yang memiliki korelasi paling tinggi terhadap risiko kehamilan adalah gula darah, yaitu dengan nilai korelasi 0.57 (korelasi sedang).
- Selain itu, tekanan darah (sistolik dan diastolik) juga berkorelasi sedang dengan risiko kehamilan.

## Data Preparation

Pada tahap ini dilakukan proses transformasi pada data sehingga menjadi bentuk yang cocok untuk proses pemodelan. Ada beberapa tahapan yang dilakukan yaitu:
- encoding fitur kategori
  
Sebelum membangun model machine learning, kita perlu mengubah variabel categorical menjadi format numerik. Selain itu, encoding fitur kategori RiskLevel juga diperlukan untuk menganalisis korelasi antara Risk Level dengan variabel numerik lainnya.
Untuk itu dilakukan encoding untuk RiskLevel menggunakan 'OrdinalEncoder' dari skicit-learn.

- pembagian dataset dengan fungsi train_test_split dari library sklearn
Pada kasus ini, kita akan menggunakan proporsi pembagian data latih dan data uji sebesar 80:20 dengan fungsi train_test_split dari skicit-learn.

## Modelling
Pada tahap ini, akan dikembangkan model machine learning dengan dua algoritma. Kemudian, akan dievaluasi performa masing-masing algoritma berdasarkan akurasinya untuk menjawab problem statement dari tahap business understanding.

Dua algoritma yang akan digunakan adalah:
- Random Forest
- Logistic Regression

### Random Forest
Algoritma random forest adalah salah satu algoritma supervised learning. Ia dapat digunakan untuk menyelesaikan masalah klasifikasi dan regresi. Random forest juga merupakan algoritma yang sering digunakan karena cukup sederhana tetapi memiliki stabilitas yang baik.

Pada model ini akan digunakan fungsi RandomForestClassifier dari sklearn.ensemble.

Parameter yang digunakan:
- n_estimators: Jumlah pohon dalam forest. Pada contoh ini digunakan 100 pohon.
- max_depth: Kedalaman maksimum setiap pohon. Disetel ke 10 untuk mencegah overfitting.
- random_state: Untuk memastikan hasil dapat direproduksi, pada model ini digunakan random state 42.


```sh
# Train and evaluate Random Forest
rf_model = RandomForestClassifier(n_estimators=100, max_depth=10, random_state=42)
rf_model.fit(X_train, y_train)
```
```sh
#Prediction and accuracy Random Forest
y_train_pred_rf = rf_model.predict(X_train)
y_test_pred_rf = rf_model.predict(X_test)
train_accuracy_rf = accuracy_score(y_train, y_train_pred_rf)
test_accuracy_rf = accuracy_score(y_test, y_test_pred_rf)
models.loc['train_accuracy', 'RandomForest'] = train_accuracy_rf
models.loc['test_accuracy', 'RandomForest'] = test_accuracy_rf
```

### Logistic Regression
Logistic Regression adalah metode statistik dan machine learning yang digunakan untuk klasifikasi biner, yaitu memprediksi probabilitas suatu contoh termasuk dalam salah satu dari dua kelas. Pada model ini akan digunakan fungsi LogisticRegression dari sklearn.linear_model.

Parameter yang digunakan:
- max_iter: Jumlah maksimum iterasi untuk algoritma pengoptimalan konvergen. Pada contoh ini, ditetapkan maksimun iterasi 1000 untuk memastikan konvergensi.
- random_state: Untuk memastikan hasil yang dapat direproduksi, pada model ini digunakan random state 42.
- solver: Algoritma yang digunakan untuk optimisasi. Disetel ke 'liblinear', yang sering kali efektif untuk dataset kecil.

```sh
# Train and evaluate Logistic Regression
lr_model = LogisticRegression(max_iter=1000, solver='liblinear', random_state=42)
lr_model.fit(X_train, y_train)
```

```sh
# Predictions and accuracy for Logistic Regression
y_train_pred_lr = lr_model.predict(X_train)
y_test_pred_lr = lr_model.predict(X_test)
train_accuracy_lr = accuracy_score(y_train, y_train_pred_lr)
test_accuracy_lr = accuracy_score(y_test, y_test_pred_lr)
models.loc['train_accuracy', 'Logistic Regression'] = train_accuracy_lr
models.loc['test_accuracy', 'Logistic Regression'] = test_accuracy_lr
```

## Evaluation
Pada tahap evaluasi, kita akan menilai performa dari kedua model yang telah dibuat berdasarkan metrik akurasi pada data latih (train accuracy) dan data uji (test accuracy). Akurasi adalah metrik yang mengukur proporsi prediksi yang benar dibandingkan dengan keseluruhan prediksi. Ini memberikan gambaran umum tentang seberapa baik model memprediksi kelas target.

Pada evaluasi model, kita menggunakan metrik akurasi sebagai berikut:

- Train Accuracy: Akurasi model pada data latih, yang menunjukkan seberapa baik model memprediksi kelas target pada data yang digunakan untuk melatih model.

- Test Accuracy: Akurasi model pada data uji, yang menunjukkan seberapa baik model memprediksi kelas target pada data yang tidak digunakan untuk melatih model. Ini memberikan indikasi kemampuan generalisasi model terhadap data baru.

![Accuracy](https://github.com/ntak21/mlterapan/blob/main/accuracy.png)


Dari hasil accuracy test, didapatkan bahwa Random Forest memiliki nilai akurasi yang lebih baik dibandingkan Logistic Regression, dimana nilai akurasi terhadap data latihnya sebesar 93% dan nilai akurasi terhadap data uji sebesar 82%.

Dengan demikian, model menggunakan metode Random Forest memiliki akurasi yang baik untuk memprediksi risiko kehamilan. Dari hasil analisis terhadap serangkaian fitur (faktor kesehatan) yang ada, dapat diketahui bahwa risiko kehamilan paling dipengaruhi oleh angka gula darah dan tekanan darah. Ibu dengan gula darah dan tekanan darah tinggi memiliki risiko kehamilan yang lebih tinggi.

Dengan mengetahui faktor risiko kehamilan dan menggunakan model prediksi di atas, diharapkan penyedia layanan kesehatan dapat mengidentifikasi kehamilan berisiko dengan lebih mudah dan memberikan penanganan terhadap kehamilan berisiko dengan lebih efektif.




