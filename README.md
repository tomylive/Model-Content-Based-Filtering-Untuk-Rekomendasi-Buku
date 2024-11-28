![image](https://github.com/user-attachments/assets/99e960ad-2d12-4a44-8269-e4e46d321b86)

# Laporan Proyek Machine Learning - Irfan Saputra Nasution
## Project Overview

Dalam era perkembangan teknologi yang pesat, berbagai bidang bisnis mulai memanfaatkan machine learning untuk meningkatkan produktivitas dan efisiensi. Salah satu contohnya adalah penerapan sistem rekomendasi, yang memungkinkan bisnis memperoleh keuntungan lebih dari peningkatan pembelian barang yang direkomendasikan serta peningkatan jumlah pengunjung. Penerapan ini juga dapat digunakan di toko buku atau perpustakaan, di mana sistem rekomendasi buku membantu pengunjung menemukan bacaan yang sesuai dengan minat mereka. Selain itu, sistem ini juga berperan penting dalam meningkatkan literasi masyarakat, yang hingga kini masih tergolong rendah. [1]

Dataset yang digunakan dalam proyek pengembangan model ini berasal dari **Book Recommendation Dataset**, yang berisi data pengguna anonim beserta informasi demografi mereka. Dataset ini mencakup peringkat buku, baik yang diberikan secara eksplisit maupun implisit, dan dikumpulkan selama periode empat minggu pada tahun 2004 dari komunitas **Book-Crossing**. Dengan membangun sistem rekomendasi berbasis dataset tersebut, diharapkan tercipta solusi inovatif yang dapat memberikan rekomendasi buku yang bermanfaat bagi pembaca, penulis, penerbit, maupun penjual. Selain itu, sistem ini berpotensi mendukung perkembangan ekosistem industri buku secara keseluruhan.

## Business Understanding
### Problem Statement
Berdasarkan latar belakang di atas, berikut ini merupakan rincian masalah yang dapat diselesaikan pada proyek ini:
* Bagaimana membuat sistem rekomendasi yang dipersonalisasi berdasarkan data pengguna dengan menggunakan teknik content-based filtering?

### Goals
Perusahaan akan mengembangkan sebuah sistem rekomendasi dengan tujuan utama untuk memberikan rekomendasi buku yang dipersonalisasi bagi setiap pengguna. Sistem ini akan dibangun menggunakan **teknik content-based filtering**, yang akan menganalisis preferensi pengguna berdasarkan buku-buku yang pernah mereka beli atau beri rating, serta mencocokkannya dengan karakteristik buku lainnya untuk memberikan rekomendasi yang relevan.

### Solution Statements
Perusahaan akan membangun sistem rekomendasi buku dengan alur sebagai berikut:

1. **Data Understanding**: Tahap awal untuk memahami data yang terdiri dari tiga file terpisah: pengguna (users), peringkat (ratings), dan buku (books). Tahapan ini mencakup:
   - **Data Loading**: Membaca dataset untuk mengetahui isi dan informasi di dalamnya.
   - **Univariate Exploratory Data Analysis**: Menganalisis dan mengeksplorasi setiap variabel dalam data.
   - **Data Preprocessing**: Menggabungkan file-file tersebut menjadi satu kesatuan data yang siap digunakan dalam pemodelan.

2. **Data Preparation**: Pada tahap ini, data dipersiapkan dengan mengatasi missing value dan menyamakan nomor ISBN buku. Karena setiap nomor ISBN mewakili satu kategori buku, pengecekan dilakukan untuk memastikan tiap buku hanya memiliki satu ISBN.

3. **Modeling**: Proses pengembangan model dilakukan dengan dua pendekatan:
   - **Content-Based Filtering**: Sistem merekomendasikan buku berdasarkan kesamaan dengan buku yang pernah disukai pengguna sebelumnya, dengan fitur utama seperti nama penulis. Teknik TF-IDF (Term Frequency-Inverse Document Frequency) digunakan untuk merepresentasikan fitur buku, dan cosine similarity digunakan untuk menghitung kesamaan antar buku.

4. **Evaluation**: Mengukur kinerja model dan menilai keberhasilannya menggunakan beberapa metrik. Untuk content-based filtering, metrik evaluasi yang digunakan adalah **Precision**, **Recall**, dan **F1 Score**

## Data Understanding
| Jenis | Keterangan |
| ------ | ------ |
| Title | _Banana Quality_ |
| Source | [Kaggle](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset/data) |
| Maintainer | [arashnic](https://www.kaggle.com/arashnic) |
| License | Other (specified in description) |
| Visibility | Publik |
| Tags | _Online Communicataties, Literature, Art, Recommender System, Culture and Humanities_ |
| Usability | 10.00 |

Data yang digunakan dalam proyek ini adalah **Book Recommendation Dataset** yang diunduh dari platform Kaggle. Dataset ini dikumpulkan oleh **Cai-Nicolas Ziegler** selama periode 4 minggu pada tahun 2004 dari komunitas **Book-Crossing**. Dataset ini mencakup 278.858 pengguna anonim, namun menyertakan informasi demografis, yang memberikan total 1.149.780 peringkat, baik secara eksplisit maupun implisit, terhadap 271.379 buku.

**Book Recommendation Dataset** terdiri dari tiga file terpisah dalam format CSV, yaitu:
1. **Books**: Berisi 271.360 baris dan 8 kolom, yang mencakup informasi tentang buku seperti nomor ISBN, judul buku, penulis, tahun terbit, penerbit, serta URL gambar buku dalam tiga ukuran (small, medium, large).
2. **Ratings**: Terdiri dari 1.149.780 baris dan 3 kolom, yang menyimpan data mengenai user ID, nomor ISBN buku, dan peringkat yang diberikan oleh pengguna.
3. **Users**: Terdiri dari 278.858 baris dan 3 kolom, berisi user ID, lokasi pengguna, serta usia pengguna.

Ini adalah struktur folder dataset yang telah diunduh dan siap untuk diolah dalam proyek sistem rekomendasi.

* book-dataset <- Nama folder utama.
   - books.csv <- Berisi informasi mengenai buku.
   - ratings.csv <- Berisi data rating atau peringkat buku yang diberikan oleh pengguna.
      - users.csv Berisi informasi pengguna atau pembaca.

### Berikut adalah variabel-variabel yang terdapat dalam **Book Recommendation Dataset**:

1. **Dataset Books**:
   - **ISBN**: Nomor ISBN buku.
   - **Book-Title**: Judul buku.
   - **Book-Author**: Nama penulis buku.
   - **Year-Of-Publication**: Tahun terbit buku.
   - **Publisher**: Nama penerbit.
   - **Image-URL-S**: URL gambar buku ukuran kecil.
   - **Image-URL-M**: URL gambar buku ukuran sedang.
   - **Image-URL-L**: URL gambar buku ukuran besar.

2. **Dataset Ratings**:
   - **User-ID**: Kode unik pengguna yang memberi penilaian.
   - **ISBN**: Nomor ISBN buku yang dinilai.
   - **Book-Rating**: Peringkat yang diberikan oleh pengguna.

3. **Dataset Users**:
   - **User-ID**: Kode unik pengguna.
   - **Location**: Lokasi pengguna.
   - **Age**: Usia pengguna.
  
### Data Loading
Pada bagian ini, dataset akan diambil langsung dari folder yang berisi **Book Recommendation Dataset** yang telah diunduh. Seperti yang dijelaskan sebelumnya, terdapat tiga file dataset di dalam folder tersebut, yaitu **Books**, **Ratings**, dan **Users**, yang akan digunakan dalam proses pengembangan model. Data dari **Books**, **Ratings**, dan **Users** dapat dilihat pada **Gambar 1 - 3**.

![image](https://github.com/user-attachments/assets/336e7ce5-181f-43f1-8117-65152e420ed0)
Gambar 1. Tampilan Isi dari Dataset Books


![image](https://github.com/user-attachments/assets/32501454-7697-4a51-bbef-cfab89a987bc)

Gambar 2. Tampilan Isi dari Dataset Ratings

![image](https://github.com/user-attachments/assets/939cf854-e902-4837-be17-c491cd1d2a35)

Gambar 3. Tampilan Isi dari Dataset Users

Berdasarkan tampilan dataset pada **Gambar 1 - 3**, diperoleh informasi sebagai berikut:

1. **Variabel pada dataset Books**:
   - Terdapat 271.360 jenis buku dengan 8 kolom:
     - **ISBN**: Nomor identitas unik buku.
     - **Book-Title**: Judul buku.
     - **Book-Author**: Nama penulis buku.
     - **Year-Of-Publication**: Tahun publikasi buku.
     - **Publisher**: Nama penerbit buku.
     - **Image-URL-S**: URL gambar ukuran kecil.
     - **Image-URL-M**: URL gambar ukuran sedang.
     - **Image-URL-L**: URL gambar ukuran besar.

2. **Variabel pada dataset Ratings**:
   - Terdapat 340.556 penilaian terhadap buku dengan 3 kolom:
     - **User-ID**: Kode unik untuk pengguna anonim yang memberikan penilaian.
     - **ISBN**: Nomor identitas buku.
     - **Book-Rating**: Penilaian yang diberikan untuk buku.

3. **Variabel pada dataset Users**:
   - Terdapat 278.858 pengguna anonim dengan 3 kolom:
     - **User-ID**: Kode unik pengguna anonim.
     - **Location**: Lokasi tempat tinggal pengguna.
     - **Age**: Usia pengguna.

Setelah memperoleh informasi dari dataset tersebut, langkah berikutnya adalah melakukan eksplorasi lebih lanjut pada dataset.

### Mengidentifikasi Missing Value atau Duplicate
**Menghilangkan Missing Value atau Duplicate**: 
Pertama, dilakukan pengecekan missing value menggunakan kode `books.isnull().sum()`. Ditemukan bahwa sebagian besar fitur memiliki missing value, kecuali **User-ID**, **ISBN**, dan **Book-Rating** yang tidak memiliki missing value. Fitur dengan missing value terbesar adalah **'Publisher'**, yaitu sebanyak 118.650 dari total dataset 1.149.780 baris. Dan untuk duplicatenya sendiri tidak ada

### Univariate Exploratory Data Analysis
Pada tahap ini, akan dilakukan analisis dan eksplorasi terhadap setiap variabel untuk memahami distribusi serta karakteristik masing-masing variabel. Pemahaman ini penting untuk menentukan pendekatan atau algoritma yang paling sesuai untuk diterapkan pada data. Variabel-variabel yang terdapat dalam **Book Recommendation Dataset** adalah sebagai berikut:

1. **Books**: Berisi informasi tentang buku.
2. **Ratings**: Berisi rating atau peringkat yang diberikan kepada buku oleh pengguna atau pembaca.
3. **Users**: Berisi informasi pengguna, termasuk informasi demografisnya.

#### Variabel Books
Dengan menggunakan fungsi **info()**, diketahui bahwa dataset **books** dari file **books.csv** memiliki 271.360 entri dan 8 kolom: ISBN, Book-Title, Book-Author, Year-Of-Publication, Publisher, Image-URL-S, Image-URL-M, dan Image-URL-L. Namun, kolom **Year-Of-Publication** bertipe data **object**, padahal umumnya tahun publikasi menggunakan tipe data **integer**. Ketika mencoba mengubah tipe data menggunakan kode `books['Year-Of-Publication'].astype('int')`, muncul error **ValueError: invalid literal for int() with base 10: 'DK Publishing Inc'**, yang berarti terdapat nilai salah input dalam kolom tersebut.

Setelah ditelusuri, ditemukan dua nilai teks yang tidak sesuai, yaitu **'DK Publishing Inc'** dan **'Gallimard'**. Kedua nilai teks ini akan dihapus dari kolom **Year-Of-Publication**, kemudian tipe data kolom tersebut akan diubah menjadi **integer**.

Langkah berikutnya adalah menghapus variabel yang tidak relevan untuk pengembangan model. Karena sistem rekomendasi berbasis konten (content-based filtering) akan memberikan rekomendasi berdasarkan kesamaan judul buku dan nama penulis yang pernah dibaca pengguna, maka kolom seperti **Image-URL-S**, **Image-URL-M**, dan **Image-URL-L** tidak diperlukan, sehingga kolom-kolom ini akan dihapus. Setelah proses ini selesai, tampilan dataset **books** akan terlihat seperti pada **Gambar 4**.

![image](https://github.com/user-attachments/assets/169b8986-5449-4861-b45c-4c228b483085)

Gambar 4. Tampilan Dataset setelah penghapusan beberapa fitur dan nilai

Setelah proses penghapusan beberapa nilai dan fitur, dataset kini hanya terdiri dari 5 kolom dengan jumlah entri sebagai berikut:

- Jumlah nomor ISBN buku: 271.357
- Jumlah judul buku: 242.132
- Jumlah penulis buku: 102.022
- Jumlah tahun publikasi: 116
- Jumlah nama penerbit: 16.805

Terlihat bahwa jumlah judul buku sebanyak 242.132 tidak sebanding dengan jumlah nomor ISBN yang berjumlah 271.357, yang menunjukkan adanya beberapa buku tanpa nomor ISBN. Mengingat satu ISBN seharusnya hanya dimiliki oleh satu buku, dataset ini akan difilter untuk memastikan setiap buku memiliki satu nomor ISBN.

Selanjutnya, distribusi data dilakukan untuk menampilkan 10 nama penulis teratas berdasarkan jumlah buku yang telah mereka tulis, sebagaimana terlihat pada **Gambar 5**.

![image](https://github.com/user-attachments/assets/4cd33bc7-e009-42e0-bff9-8f35e6160bff)

Gambar 5. Distribusi Data mengenai 10 nama penulis ternama berdasarkan jumlah buku

Berdasarkan informasi pada **Gambar 5**, diketahui bahwa penulis **Agatha Christie** menulis lebih dari 600 buku, menjadikannya penulis dengan jumlah buku terbanyak dalam dataset. Selain itu, ditemukan beberapa penulis lain yang menulis lebih dari satu judul buku.

#### Variabel Ratings

Selanjutnya, dilakukan eksplorasi pada variabel **ratings**, yaitu penilaian yang diberikan pembaca atau pengguna terhadap buku. Dengan menggunakan fungsi **info()**, diketahui bahwa terdapat 1.149.780 entri dan 3 kolom dalam dataset ini:
- **User-ID**: Kode unik pengguna anonim yang memberikan peringkat.
- **ISBN**: Identitas berupa nomor unik buku.
- **Book-Rating**: Peringkat yang diberikan kepada buku oleh pengguna, dengan nilai berkisar antara 0 hingga 10, di mana 0 merupakan rating terendah dan 10 tertinggi.

#### Variabel Users
Eksplorasi terakhir dilakukan pada variabel users, yang berisi informasi mengenai pengguna anonim beserta data demografi mereka. Dengan menggunakan fungsi info(), diketahui bahwa variabel ini memiliki 278.858 entri dan terdiri dari 3 kolom:

* User-ID: Kode unik dari pengguna anonim.
* Location: Lokasi pengguna.
* Age: Usia pengguna, dengan beberapa entri yang tidak mencantumkan usia.

Data pengguna ini berguna jika ingin membangun sistem rekomendasi yang mempertimbangkan faktor demografi atau kondisi sosial pengguna. Namun, untuk studi kasus kali ini, data users tidak akan digunakan dalam pengembangan model. Model yang akan dikembangkan hanya menggunakan data dari variabel books dan ratings.

### Data Prepocessing
Seperti yang telah dijelaskan dalam tahap **data understanding**, folder **Book Recommendation Dataset** terdiri dari tiga file terpisah: **books**, **ratings**, dan **users**. Pada tahap ini, dilakukan proses penggabungan file **books** dan **ratings** menjadi satu kesatuan dataset untuk keperluan pengembangan model.

Setelah penggabungan, dataset memiliki 7 variabel dan 1.149.780 baris data. Tampilan dari dataset yang sudah digabungkan dapat dilihat pada **Gambar 6**, dan inilah dataset yang akan digunakan untuk membangun sistem rekomendasi.

![image](https://github.com/user-attachments/assets/895069ad-c362-4b80-8dcc-a9ea0779ddce)

Gambar 6. Tampilan dataset yang sudah diproses penggabungan (merge) antara variabel ratings dan books

## Data Preparation
Teknik yang digunakan dalam pengembangan model, yaitu **content-based filtering** dan berikut adalah tahap **data preparation**: 

### Data Preparation untuk Model Pengembangan dengan Content-Based Filtering
Pada tahap ini, dilakukan beberapa langkah persiapan data, antara lain:

1. **Menyamakan Jenis Buku Berdasarkan ISBN**: 
Sebelum masuk ke tahap pemodelan, perlu dipastikan bahwa setiap nomor ISBN hanya mewakili satu judul buku, karena satu ISBN untuk lebih dari satu judul buku dapat menyebabkan bias dalam data. Dilakukan pengecekan ulang setelah proses pembersihan data, dan ditemukan bahwa jumlah ISBN tidak sama dengan jumlah judul buku. Oleh karena itu, dilakukan proses **deduplikasi** pada kolom **'ISBN'** untuk memastikan setiap ISBN unik. Setelah proses ini, dataset tersisa 270.145 baris data.

2. **Menghapus data missing value**
Jumlah ini dianggap tidak terlalu signifikan, sehingga missing value akan dihapus menggunakan proses **drop**, dan disimpan dalam variabel baru bernama **all_books_clean**. Setelah proses penghapusan missing value, dataset tersisa 1.031.129 baris.
   
Selanjutnya, dibuatkan **dictionary** untuk menghubungkan key-value dari data seperti **isbn_id**, **book_title**, **book_author**, **year_of_publication**, dan **publisher**. Hasilnya disimpan dalam variabel **books_new** yang terlihat seperti pada **Gambar 7**.

Ini memastikan bahwa data sudah siap untuk digunakan dalam pengembangan model rekomendasi berbasis konten (content-based filtering).

![image](https://github.com/user-attachments/assets/b9dbe95b-6200-4763-8f2d-c7f55472b9b9)

Gambar 7. Tampilan dataset books_new setelah menghilangkan missing value dan menyamakan jenis buku berdasarkan ISBN

Berdasarkan informasi sebelumnya, dataset memiliki sekitar 270.145 baris data. Karena ukuran dataset yang besar dapat memerlukan alokasi memori yang signifikan, pada proyek ini hanya akan digunakan sebagian data untuk pengembangan model. Oleh karena itu, hanya data dari baris pertama hingga baris ke-20.000 (tidak termasuk baris ke-20.000) yang akan diambil. **Dataset books_new** inilah yang akan digunakan dalam proses pengembangan model menggunakan teknik **Content-Based Filtering**.

Dalam pengembangan model ini, dilakukan pencarian representasi fitur penting dari setiap judul buku menggunakan TF-IDF (Term Frequency - Inverse Document Frequency) Vectorizer. TF-IDF adalah alat yang digunakan untuk mengubah dokumen teks menjadi representasi vektor berdasarkan frekuensi kemunculan kata (TF) dan seberapa unik atau jarangnya kemunculan kata tersebut di seluruh koleksi dokumen (IDF). Vektor ini digunakan untuk menemukan fitur penting dari judul buku berdasarkan nama penulis menggunakan teknik Content Based Filtering.

Pada proyek ini, digunakan fungsi `tfidfvectorizer()` dari library Scikit-Learn. Langkah pertama adalah mengimpor fungsi `tfidfvectorizer()` untuk menghitung IDF pada data `book_author`. Kemudian, dilakukan pemetaan array dari indeks fitur integer ke nama fitur menggunakan fungsi `get_feature_name_out()`. Setelah itu, dilakukan proses fitting dan transformasi data ke dalam bentuk matriks berukuran (20000, 8746), di mana 20000 adalah jumlah data dan 8746 adalah jumlah penulis buku yang berbeda. Untuk menghasilkan vektor TF-IDF dalam bentuk matriks, digunakan fungsi `todense()`.

Langkah terakhir adalah menampilkan matriks TF-IDF untuk beberapa judul buku dan nama penulis buku dalam bentuk dataframe. Pada dataframe ini, kolom-kolom diisi dengan nama penulis buku, sementara baris-barisnya diisi dengan judul buku, seperti yang ditunjukkan pada Gambar 8.

![image](https://github.com/user-attachments/assets/b7851502-977e-4ef9-bf3b-d92c9b720022)

Gambar 8. Dataframe dari matriks tf-idf

Berdasarkan Gambar 8, matriks TF-IDF berhasil mengidentifikasi fitur penting untuk setiap kategori judul buku menggunakan fungsi `tfidfvectorizer`. Dalam kasus ini, dataset yang digunakan hanya berupa sampel, sehingga matriks lengkapnya tidak ditampilkan. Dari total 20.000 data yang ada, dipilih sampel acak yang terdiri dari 10 judul buku yang ditampilkan pada baris vertikal dan 15 nama penulis buku pada baris horizontal.


## Modeling
### Model Development dengan Content Based Filtering

Pada tahap ini, dikembangkan model menggunakan teknik Content Based Filtering. Content Based Filtering adalah pendekatan dalam sistem rekomendasi yang memanfaatkan informasi atau "konten" dari item atau pengguna untuk memberikan rekomendasi. Inti dari teknik ini adalah mencocokkan preferensi pengguna dengan karakteristik atau konten dari item yang telah dilihat atau disukai sebelumnya. Sebagai contoh, jika seorang pengguna menyukai atau pernah membeli buku berjudul "Introduction to Machine Learning" yang ditulis oleh "Alex Smola", maka sistem akan mencari buku lain yang memiliki karakteristik serupa dan merekomendasikannya dalam bentuk top-N recommendation kepada pengguna tersebut.

**Kelebihan dari Content Based Filtering:**

1. Memberikan rekomendasi yang dipersonalisasi berdasarkan preferensi unik setiap pengguna.
2. Mampu mengatasi masalah cold-start (ketika data pengguna sangat sedikit) dan memberikan rekomendasi yang relevan sejak awal.
3. Tidak bergantung pada data pengguna lain untuk memberikan rekomendasi.
4. Cocok untuk item yang memiliki fitur atau atribut yang mudah diukur atau diidentifikasi, seperti genre, penulis, atau karakteristik lainnya.

**Kekurangan dari Content Based Filtering:**

1. Kurang efektif dalam memberikan rekomendasi yang mengejutkan karena hanya merekomendasikan item yang mirip dengan yang sudah diketahui pengguna.
2. Sulit untuk menangkap preferensi pengguna yang kompleks atau dinamis, terutama jika item yang mirip tidak mencerminkan preferensi yang mendalam.
3. Risiko terjebak dalam "filter bubble," di mana pengguna hanya menerima rekomendasi yang sesuai dengan preferensi yang sudah diketahui, tanpa adanya variasi.

Selanjutnya, untuk menghitung derajat kesamaan (similarity degree) antar judul buku, digunakan teknik cosine similarity. Metode ini berfungsi untuk mengukur kesamaan antara dua vektor dalam ruang berdimensi tinggi dengan menghitung sudut kosinus antara kedua vektor tersebut; semakin kecil sudutnya, semakin besar kesamaan di antara vektor-vektor tersebut. Pada proyek ini, fungsi `cosine_similarity` dari library Scikit-Learn digunakan untuk menghitung nilai kesamaan berdasarkan matriks TF-IDF yang telah dibuat.

Dengan menggunakan fungsi `cosine_similarity()`, diperoleh nilai kesamaan antar judul buku dalam bentuk matriks kesamaan yang berupa array. Selanjutnya, dibuat dataframe dari hasil perhitungan cosine similarity, dengan baris dan kolom yang mewakili nama judul buku. Tampilan dataframe hasil perhitungan cosine similarity ini ditunjukkan pada Gambar 9.

![image](https://github.com/user-attachments/assets/67ff992e-e3b2-4d79-a2d5-4feb686db10f)

Gambar 9. Dataframe hasil perhitungan cosine similarity

Berdasarkan Gambar 9, penggunaan cosine similarity berhasil mengidentifikasi kesamaan antara satu judul buku dengan judul buku lainnya. Matriks kesamaan yang dihasilkan memiliki ukuran 20.000 x 20.000, yang berarti tingkat kesamaan telah diidentifikasi untuk 20.000 judul buku. Namun, karena keterbatasan alokasi memori, tidak semua data dapat ditampilkan. Oleh karena itu, hanya ditampilkan sampel berupa 5 judul buku pada baris vertikal dan 5 judul buku pada baris horizontal.

Dengan data kesamaan (similarity) antar judul buku yang telah diperoleh, proses selanjutnya adalah memberikan rekomendasi buku yang mirip dengan judul buku yang sebelumnya pernah dibeli atau dibaca oleh pengguna.

**Implementasi Rekomendasi**

Pada tahap ini, dibuat fungsi bernama `book_recommendations` dengan beberapa parameter sebagai berikut:

- **book_title**: Nama judul buku yang menjadi acuan (indeks dalam dataframe kesamaan).
- **similarity_data**: Dataframe yang berisi informasi kesamaan antar judul buku yang telah dihitung sebelumnya.
- **items**: Nama dan fitur yang digunakan untuk menentukan kemiripan, dalam hal ini adalah 'book_title' dan 'book_author'.
- **k**: Jumlah rekomendasi top-N yang akan diberikan oleh sistem. Secara default, nilai k adalah 5.

Fungsi `book_recommendations` dibuat untuk menampilkan hasil rekomendasi berbasis konten berupa judul buku dengan nama penulis yang sama dengan judul buku yang pernah dibeli atau dibaca oleh pengguna. Sistem rekomendasi ini mengeluarkan top-N recommendation, sehingga fungsi ini akan memberikan sejumlah rekomendasi judul buku sesuai dengan parameter k yang telah ditentukan.

Dalam fungsi `book_recommendations`, digunakan fungsi `argpartition()` untuk mengambil k nilai tertinggi dari data kesamaan. Selanjutnya, data disusun berdasarkan bobot kesamaan dari yang tertinggi ke terendah, dan hasilnya dimasukkan ke dalam variabel bernama 'closest'. Kemudian, judul buku yang digunakan sebagai referensi (book_title) dihapus dari daftar rekomendasi agar tidak muncul dalam hasil yang diberikan. Dengan demikian, fungsi ini mencari judul buku yang mirip dengan judul buku yang dimasukkan sebagai input pada parameter `book_title`. Contoh penggunaan fungsi ini dan daftar judul buku yang digunakan dapat dilihat pada Gambar 10.

![image](https://github.com/user-attachments/assets/8e8025a0-d9d0-414a-b050-ce7ba50751e4)

Gambar 10. Contoh judul buku yang digunakan untuk menampilkan rekomendasi

Pada Gambar 10, terlihat bahwa judul buku 'Dialogues with Silence: Prayers and Drawings' yang ditulis oleh 'Thomas Merton'. Selanjutnya, digunakan fungsi book_recommendations untuk menampilkan rekomendasi 5 buku teratas yang direkomendasikan sistem berdasarkan judul buku di Gambar 10. Hasilnya terlihat pada Gambar 11.

![image](https://github.com/user-attachments/assets/7f46681d-5a7f-419b-930e-d8ef393706ec)

Gambar 11. Hasil Rekomendasi 5 buku teratas dengan kategori nama penulis

## Evaluation
###  Evaluasi Model Content Based Filtering

**Evaluasi Model Content-Based Filtering**

Dalam proyek ini, model Content-Based Filtering dievaluasi menggunakan tiga metrik utama: Precision, Recall, dan F1-Score, yang bertujuan untuk mengukur kinerja model secara menyeluruh.

- **Precision** mengukur proporsi item relevan yang direkomendasikan oleh model terhadap total item yang direkomendasikan.
- **Recall** mengukur proporsi item relevan yang direkomendasikan oleh model terhadap total item relevan yang seharusnya direkomendasikan.
- **F1-Score** merupakan gabungan dari Precision dan Recall, yang memberikan satu nilai untuk menilai keseimbangan antara keduanya.

Berikut adalah rumus perhitungan metrik evaluasi untuk sistem rekomendasi berbasis konten:

$$Precision = \frac{Jumlah\ item\ revelan\ yang\ dihasilkan}{Total\ item\ yang\ dihasilkan}$$

$$Recall = \frac{Jumlah\ item\ relevan\ yang\ dihasilkan}{Total\ item\ yang\ seharusnya\ direkomendasikan}$$

$$F1\ Score = 2 * \frac{Precision * Recall}{Precision + Recall}$$


**Proses Evaluasi Model**

Untuk menghitung metrik ini, diperlukan data berupa label sebenarnya (ground truth) untuk membandingkan hasil prediksi model. Dalam proyek ini, label ground truth dibuat menggunakan hasil derajat kesamaan yang dihitung dengan cosine similarity, di mana setiap baris dan kolom mewakili judul buku, dan setiap nilai sel mewakili label (1 untuk "mirip" dan 0 untuk "tidak mirip").

Keputusan untuk menentukan apakah nilai kesamaan (similarity) antara dua item dianggap 1 (mirip) atau 0 (tidak mirip) dilakukan dengan menetapkan ambang batas (threshold) sebesar 0.5. Threshold ini ditentukan berdasarkan analisis hasil rekomendasi sebelumnya dan disesuaikan dengan kebutuhan model. Matriks ground truth kemudian dibuat menggunakan fungsi `np.where()` dari NumPy, yang menetapkan nilai 1 jika cosine similarity antara dua item lebih besar atau sama dengan 0.5, dan 0 jika di bawah ambang tersebut. Matriks ini disajikan dalam bentuk dataframe dengan indeks berupa judul buku.

Setelah label ground truth dibuat, evaluasi model dilakukan dengan menghitung metrik precision, recall, dan F1 score melalui langkah-langkah berikut:

1. **Mengimpor Fungsi Evaluasi:** Fungsi `precision_recall_fscore_support` dari library Scikit-Learn digunakan untuk menghitung metrik evaluasi.
2. **Sampling Data:** Karena keterbatasan memori, hanya 10.000 sampel dari cosine similarity dan matriks ground truth yang diambil untuk mempercepat perhitungan.
3. **Konversi Data:** Matriks cosine similarity dan ground truth dikonversi menjadi array satu dimensi untuk mempermudah perbandingan dan perhitungan metrik.
4. **Perhitungan Metrik:** Hasil evaluasi disimpan dalam array `predictions`. Fungsi `precision_recall_fscore_support` digunakan untuk menghitung precision, recall, dan F1 score dengan parameter `average='binary'` untuk klasifikasi biner (1 atau 0) dan `zero_division=1` untuk menghindari kesalahan pembagian dengan nol jika ada kelas yang tidak muncul dalam prediksi.

**Hasil Evaluasi Metrik:**

- **Precision: 1.0**
- **Recall: 1.0**
- **F1-Score: 1.0**

**Interpretasi Hasil Evaluasi:**

- Nilai **Precision** sebesar 1.0 menunjukkan bahwa semua item yang direkomendasikan oleh model benar-benar relevan, tanpa ada kesalahan (false positive).
- Nilai **Recall** sebesar 1.0 menunjukkan bahwa model berhasil mengidentifikasi 100% dari semua item yang relevan, tanpa ada item yang terlewat (false negative).
- Nilai **F1-Score** sebesar 1.0 mengindikasikan keseimbangan sempurna antara precision dan recall, yang berarti model berkinerja sangat baik dalam menangani kedua kelas (positif dan negatif).

**Dampak Evaluasi Model Terhadap Business Understanding:**

1. **Menjawab Problem Statement:**
   - Evaluasi model menunjukkan bahwa sistem rekomendasi yang dikembangkan mampu memberikan rekomendasi yang dipersonalisasi dan relevan bagi pengguna berdasarkan preferensi mereka. Ini sesuai dengan problem statement yang diajukan, yakni bagaimana membuat sistem rekomendasi buku yang efektif dan personalisasi.

2. **Mencapai Goals yang Diharapkan:**
   - Model berhasil mencapai tujuan yang diharapkan, yaitu menghasilkan rekomendasi buku yang relevan untuk setiap pengguna. Nilai precision, recall, dan F1-Score yang sempurna menunjukkan bahwa model dapat mengidentifikasi dengan baik buku-buku yang sesuai dengan preferensi pengguna, sehingga meningkatkan pengalaman membaca mereka.

3. **Dampak Solusi Statement:**
   - Solusi yang direncanakan, yaitu penerapan teknik content-based filtering, memberikan dampak positif. Model mampu memberikan rekomendasi yang sesuai dengan minat dan preferensi pengguna tanpa perlu data dari pengguna lain (collaborative filtering). Hal ini menunjukkan bahwa teknik yang dipilih berhasil mendukung tujuan bisnis dalam meningkatkan kepuasan pengguna dan potensi penjualan buku.

Dengan demikian, evaluasi model ini menunjukkan bahwa sistem rekomendasi yang dikembangkan tidak hanya memenuhi kebutuhan teknis tetapi juga berdampak positif terhadap tujuan bisnis. Hal ini diharapkan dapat meningkatkan engagement pengguna dan, pada akhirnya, mendukung pertumbuhan industri buku secara keseluruhan.

## Referensi
[1] Puritat, K., & Intawong, K. (2020). Development of an Open Source Automated Library System with Book Recommendation System for Small Libraries
