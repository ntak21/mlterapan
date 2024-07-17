# Laporan Proyek Machine Learning - Ariftra Rahmawati

## Domain Proyek

Maternal Health merupakan aspek penting dari kesehatan masyarakat, dengan angka kematian dan morbiditas ibu menjadi indikator utama kualitas sistem kesehatan global. Komplikasi terkait kehamilan dapat menimbulkan risiko signifikan bagi ibu dan janin, yang berpotensi mengakibatkan masalah kesehatan yang serius atau bahkan kematian. Untuk mengurangi risiko tersebut, diperlukan upaya pencegahan salah satunya dengan memprediksi tingkat risiko komplikasi. Hal ini memungkinkan penyedia layanan kesehatan untuk memprioritaskan dan mengelola kehamilan berisiko tinggi dengan lebih efektif serta memastikan layanan kesehatan yang lebih baik.

Pentingnya pemantauan kesehatan maternal telah banyak dibahas dalam literatur medis, salah satunya oleh WHO yang menekankan perlunya pemantauan kesehatan maternal yang komprehensif untuk mengurangi angka kematian ibu (WHO, 2019).

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
Risiko kesehatan dengan kategori tingkatan tertentu termasuk ke dalam permasalahan klasifikasi. Oleh karena itu, metodologi pada proyek ini adalah membangun model klasifikasi dengan Risk Level sebagai target. Pengembangan model akan menggunakan dua algoritma machine learning yaitu Random Forest dan Logistic Regression. Dari kedua model akan dipilih satu model dengan akurasi terbaik.
Metrik evaluasi yang akan digunakan pada kasus ini adalah accuracy score menggunakan sklearn.metrics.

## Data Understanding
Dataset yang digunakan adalah Maternal Health Risk Data yang diambil dari situs Kaggle. Data pada dataset ini didapatkan dari berbagai rumah sakit, klinik komunitas, pusat kesehatan maternal melalui sistem pemantauan risiko berbasis IoT.

sumber: https://www.kaggle.com/datasets/csafrit2/maternal-health-risk-data/data

- Dataset terdiri dari 1014 baris data risiko kehamilan dengan berbagai kondisi kesehatan yang menyertai.
- Terdapat 7 kolom, yaitu: Age, SystolicBP, DiastolicBP, BS, BodyTemp, HeartRate, dan RiskLevel.
- Fitur numerik: Age, SystolicBP, DiastolicBP, BS, BodyTemp, dan HeartRate
- Fitur non numerik: RiskLevel
- RiskLevel merupakan fitur target

### Deskripsi Variabel 
Age: Age in years when a woman is pregnant.
SystolicBP: Upper value of Blood Pressure in mmHg, another significant attribute during pregnancy.
DiastolicBP: Lower value of Blood Pressure in mmHg, another significant attribute during pregnancy.
BS: Blood glucose levels is in terms of a molar concentration, mmol/L.
BodyTemp: body temperature in Fahrenheit
HeartRate: A normal resting heart rate in beats per minute.
Risk Level: Predicted Risk Intensity Level during pregnancy considering the previous attribute.

### Exploratory Data Analysis
Beberapa tahapan yang dilakukan dalam Exploratory Data Analysis yaitu:
1) Menangani outliers dengan metode IQR
2) Univariate analysis terhadap fitur kategori dan numerik
Dari analisis terhadap risk level, didapatkan bahwa terdapat 3 kategori pada RiskLevel, yaitu: Low Risk, Mid Risk, dan High Risk. Dari data persentase dapat disimpulkan bahwa lebih dari 50% sampel memiliki risiko kehamilan rendah, dan sekitar 34% memilki risiko kehamilan sedang.

Dari analisis terhadap fitur numerik didapatkan informasi bahwa:
- Usia ibu hamil paling banyak berusia sekitar 15 hingga 20 tahun.
- Mayoritas suhu badan berada pada nilai normal yaitu sekitar 98 F atau 36 derajat Celcius.
- Menurut informasi yang dilansir dari Halodoc, tekanan darah normal berkisar pada 90–120 mmHg (sistolik) dan 60–80 mmHg (diastolik). Berdasarkan plot, sebagian besar sampel memiliki tekanan darah pada rentang normal.
- Ambang batas atas gula darah normal adalah 11,1 mmol/L. Pada histogram terlihat bahwa sebagian besar sampel memiliki gula darah normal.
- Mayoritas sampel memilki suhu badan dan heart rate normal.

3) Multivariate analysis menggunakan corrplot
Correlation matrix menunjukkan:
- korelasi yang kuat antara fitur SystolicBP dan DiastolicBP.
- korelasi sedang antara usia dengan tekanan darah, usia dengan gula darah, dan tekanan darah dengan gula darah.
- variabel yang memiliki korelasi paling tinggi terhadap risiko kehamilan adalah gula darah, yaitu dengan nilai korelasi 0.57 (korelasi sedang).
- Selain itu, tekanan darah (sistolik dan diastolik) juga berkorelasi sedang dengan risiko kehamilan.

## Data Preparation

Pada tahap ini dilakukan proses transformasi pada data sehingga menjadi bentuk yang cocok untuk proses pemodelan. Ada beberapa tahapan yang dilakukan yaitu encoding fitur kategori dan pembagian dataset dengan fungsi train_test_split dari library sklearn.

## Modelling
Pada tahap ini, akan dikembangkan model machine learning dengan dua algoritma. Kemudian, akan dievaluasi performa masing-masing algoritma berdasarkan akurasinya untuk menjawab problem statement dari tahap business understanding.

Dua algoritma yang akan digunakan adalah:
- Random Forest
- Logistic Regression

### Random Forest
Algoritma random forest adalah salah satu algoritma supervised learning. Ia dapat digunakan untuk menyelesaikan masalah klasifikasi dan regresi. Random forest juga merupakan algoritma yang sering digunakan karena cukup sederhana tetapi memiliki stabilitas yang baik. 

Pada model ini akan digunakan fungsi RandomForestClassifier dari sklearn.ensemble.

```sh
# Train and evaluate Random Forest
rf_model = RandomForestClassifier(n_estimators=100, random_state=42)
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
```sh
# Train and evaluate Logistic Regression
lr_model = LogisticRegression(max_iter=1000, random_state=42)
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
Pada tahap evaluasi, kita akan menilai performa dari kedua model yang terlah dibuat berdasarkan train accuracy dan test_accuracy. Dari hasil accuracy test, didapatkan bahwa Random Forest memiliki nilai akurasi yang lebih baik dibandingkan Logistic Regression, dimana nilai akurasi terhadap data latihnya sebesar 93% dan nilai akurasi terhadap data uji sebesar 82%.

Dengan demikian, model menggunakan metode Random Forest memiliki akurasi yang baik untuk memprediksi risiko kehamilan. Dari hasil analisis terhadap serangkaian fitur (faktor kesehatan) yang ada, dapat diketahui bahwa risiko kehamilan paling dipengaruhi oleh angka gula darah dan tekanan darah. Ibu dengan gula darah dan tekanan darah tinggi memiliki risiko kehamilan yang lebih tinggi.

Dengan mengetahui faktor risiko kehamilan dan menggunakan model prediksi di atas, diharapkan penyedia layanan kesehatan dapat mengidentifikasi kehamilan berisiko dengan lebih mudah dan memberikan penanganan terhadap kehamilan berisiko dengan lebih efektif.




