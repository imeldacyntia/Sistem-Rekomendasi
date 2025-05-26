# Laporan Proyek Machine Learning - Imelda Cyntia

## Project Overview

### Latar Belakang

Indonesia memiliki potensi besar dalam pengembangan ekowisata berkelanjutan berkat kekayaan alam dan budaya yang melimpah. Namun, implementasi kebijakan ekowisata masih menghadapi tantangan seperti infrastruktur yang kurang memadai, partisipasi masyarakat lokal yang rendah, dan dampak lingkungan negatif. Studi oleh Idrus et al. (2024) menunjukkan bahwa meskipun ada komitmen pemerintah untuk mempromosikan ekowisata, berbagai hambatan tersebut masih menjadi tantangan signifikan dalam implementasinya.

Di sisi lain, pemanfaatan teknologi digital seperti sistem rekomendasi terbukti mampu meningkatkan efisiensi pencarian informasi wisata serta mendorong keterlibatan pengguna secara personal. Adopsi teknologi digital dalam promosi pariwisata dapat meningkatkan visibilitas destinasi dan pengalaman wisatawan, khususnya dalam konteks lokal. Hal ini menunjukkan bahwa integrasi sistem rekomendasi dalam promosi ekowisata Indonesia menjadi solusi potensial untuk meningkatkan distribusi kunjungan wisata dan mendorong pariwisata berkelanjutan.

### Daftar Referensi

Idrus, S. H., Jaya, L. M. G., Yusuf, M., & Rijal, M. (2024). Evaluation of the Implementation of Ecotourism-Based Tourism Policies in Indonesia: Challenges and Opportunities. Riwayat: Educational Journal of History and Humanities, 7(4), 2589–2597. https://doi.org/10.24815/jr.v7i4.41367

## Business Understanding

### Problem Statements

1. Banyak wisatawan tidak memiliki akses yang cukup terhadap informasi destinasi ekowisata yang sesuai dengan minat atau preferensinya.

2. Destinasi ekowisata lokal di Indonesia belum terekspos secara luas di platform digital, sehingga kurang dikenal oleh wisatawan.

3. Belum tersedia sistem rekomendasi yang memanfaatkan preferensi pengguna secara personal dan terintegrasi dengan data lokal Indonesia, khususnya dalam domain ekowisata.

### Goals

1. Mengembangkan sistem rekomendasi destinasi ekowisata berbasis data yang sesuai dengan preferensi pengguna.

2. Meningkatkan visibilitas destinasi-destinasi ekowisata lokal di Indonesia melalui teknologi digital.

3. Mendukung pengembangan pariwisata yang berkelanjutan dengan memberikan saran destinasi yang lebih personal dan relevan.

### Solution Approach

Untuk mencapai tujuan di atas, sistem akan dikembangkan dengan mengimplementasikan lebih dari satu pendekatan rekomendasi yang saling melengkapi:

1. Content-Based Filtering
   Menganalisis karakteristik destinasi yang pernah disukai pengguna, seperti kategori dan kota. Sistem kemudian merekomendasikan destinasi lain dengan fitur serupa.

2. Collaborative Filtering
   Membangun sistem berdasarkan penilaian atau rating dari pengguna lain. Sistem ini akan menyarankan destinasi yang disukai oleh pengguna dengan preferensi yang mirip, meskipun belum pernah dikunjungi oleh pengguna tersebut.

## Data Understanding

Dataset yang digunakan dalam proyek ini berjudul **[Indonesia’s Ecotourism Dataset](https://www.kaggle.com/datasets/farazbeniqnomf/indonesiaecotourism))** dan tersedia di platform Kaggle. Dataset ini dikembangkan untuk mendukung pengembangan sistem rekomendasi berbasis ekowisata di Indonesia. Terdapat tiga file utama yang digunakan, yaitu: `eco_place.csv`, `eco_event.csv`, dan `eco_rating.csv`.

### 1. eco\_place.csv (Data Destinasi Wisata)

File ini berisi informasi tentang tempat-tempat wisata alam dan ekowisata di Indonesia, termasuk deskripsi, kategori, harga tiket, rating, lokasi, dan dokumentasi visual.

* **Jumlah data**: 182 baris
* **Jumlah kolom**: 13
* **Missing value**: Terdapat missing value pada `gallery_photo_img2` (2 baris) dan `gallery_photo_img3` (77 baris)

| No | Nama Kolom             | Tipe Data | Keterangan                                 |
| -- | ---------------------- | --------- | ------------------------------------------ |
| 0  | `place_id`             | int64     | ID unik untuk setiap tempat wisata         |
| 1  | `place_name`           | object    | Nama tempat wisata                         |
| 2  | `place_description`    | object    | Deskripsi tempat                           |
| 3  | `category`             | object    | Kategori tempat wisata (Alam, Budaya, dsb) |
| 4  | `city`                 | object    | Kota atau kabupaten tempat wisata          |
| 5  | `price`                | object    | Harga tiket masuk (dalam teks)             |
| 6  | `rating`               | float64   | Rata-rata rating dari pengunjung           |
| 7  | `description_location` | object    | Deskripsi lokasi                           |
| 8  | `place_img`            | object    | Link utama gambar tempat                   |
| 9  | `gallery_photo_img1`   | object    | Link gambar galeri 1                       |
| 10 | `gallery_photo_img2`   | object    | Link gambar galeri 2                       |
| 11 | `gallery_photo_img3`   | object    | Link gambar galeri 3                       |
| 12 | `place_map`            | object    | Link lokasi peta (embed maps)              |

### 2. eco\_event.csv (Data Event Wisata)

File ini berisi informasi tentang event atau kegiatan wisata yang diadakan di destinasi ekowisata tertentu. Data ini bermanfaat untuk menambah konteks dan nilai tambah pada sistem rekomendasi, terutama dalam hal promosi event spesial.

* **Jumlah data**: 6 baris
* **Jumlah kolom**: 6
* **Missing value**: Tidak ada

| No | Nama Kolom    | Tipe Data | Keterangan                                     |
| -- | ------------- | --------- | ---------------------------------------------- |
| 0  | `event_id`    | int64     | ID unik dari event wisata                      |
| 1  | `event_img`   | object    | Link gambar utama dari event                   |
| 2  | `event_name`  | object    | Nama dari event wisata                         |
| 3  | `event_place` | object    | Nama tempat atau destinasi penyelenggara event |
| 4  | `event_date`  | object    | Tanggal pelaksanaan event (format teks)        |
| 5  | `event_about` | object    | Deskripsi ringkas mengenai event               |


### 3. eco\_rating.csv (Data Rating oleh Pengguna)

File ini berisi interaksi pengguna berupa rating terhadap destinasi wisata. Data ini menjadi komponen utama dalam membangun model rekomendasi berbasis *collaborative filtering*.

* **Jumlah data**: 849 baris
* **Jumlah kolom**: 3
* **Missing value**: Tidak ada

| No | Nama Kolom    | Tipe Data | Keterangan                                      |
| -- | ------------- | --------- | ----------------------------------------------- |
| 0  | `user_id`     | int64     | ID unik pengguna yang memberikan rating         |
| 1  | `place_id`    | int64     | ID destinasi wisata yang dinilai                |
| 2  | `user_rating` | int64     | Nilai rating yang diberikan oleh pengguna (1–5) |


## Exploratory Data Analysis (EDA)

Tahap eksplorasi data ini dilakukan untuk memahami karakteristik umum dari dataset destinasi ekowisata Indonesia, termasuk distribusi kategori wisata, sebaran geografis, serta perilaku pengguna berdasarkan data rating. Visualisasi dilakukan untuk mendukung proses pemodelan sistem rekomendasi secara lebih informatif.

### 1. Eksplorasi Awal Tempat Wisata

![Distribusi Kategori Wisata](img/eksplorasi_nama_tempat.png)

Grafik menampilkan 10 tempat wisata pertama berdasarkan place_id. Urutannya konsisten dengan ID terkecil hingga terbesar, yang mencerminkan urutan input data bukan popularitas atau frekuensi. Ini berguna untuk melihat daftar awal tempat wisata unik dalam dataset, namun belum mencerminkan tempat yang paling banyak dikunjungi atau paling sering muncul.

### 2. Distribusi Kategori Destinasi Wisata

![Distribusi Kategori Wisata](img/kategori_wisata.png)

Grafik ini menunjukkan distribusi jumlah destinasi berdasarkan kategori individualnya. Terlihat bahwa kategori Cagar Alam mendominasi jumlah destinasi wisata dalam dataset ini, disusul oleh kategori Budaya, Bahari, dan Taman Nasional. Beberapa kategori lain seperti Taman Hiburan dan Desa Wisata tercatat dalam jumlah yang lebih kecil. Temuan ini memperkuat bahwa fokus utama pengembangan ekowisata di Indonesia masih sangat terpusat pada aspek keindahan alam, pelestarian lingkungan, serta nilai-nilai budaya lokal.

### 3. Sebaran Destinasi Berdasarkan Kota

![Distribusi Kota](img/sebaran_kota.png)

Visualisasi ini memperlihatkan 10 kota atau kabupaten dengan jumlah destinasi wisata terbanyak dalam dataset. Yogyakarta secara signifikan mendominasi dengan 53 destinasi, disusul oleh Bandung sebanyak 36 destinasi. Hal ini menunjukkan tingginya daya tarik wisata dan dokumentasi yang kuat di kedua kota tersebut, terutama dalam sektor budaya, alam, dan ekowisata.

Kota-kota lain seperti Semarang, Jakarta, Bogor, dan Malang juga masuk dalam daftar teratas, memperkuat posisi Pulau Jawa sebagai pusat konsentrasi destinasi wisata nasional. Visualisasi ini juga mengindikasikan bahwa dokumentasi dan promosi wisata digital cenderung lebih berkembang di wilayah barat Indonesia dibanding wilayah timur.

### 4. Interaksi Pengguna per Destinasi

![Interaksi Place ID](img/interaksi_placeid.png)

Visualisasi interaksi pengguna terhadap destinasi wisata menunjukkan bahwa terdapat beberapa tempat yang jauh lebih populer dibandingkan yang lain, dilihat dari jumlah rating yang diterima. Sepuluh destinasi teratas mencatat jumlah interaksi pengguna yang signifikan, dengan satu atau dua destinasi menonjol secara ekstrem dibandingkan yang lain. Hal ini mengindikasikan bahwa popularitas tidak terdistribusi secara merata, melainkan terkonsentrasi pada beberapa tempat saja

### 5. Rata-rata Rating pada Destinasi Terpopuler

![Distribusi Rating](img/rerata_rating_top10.png)

Visualisasi ini menunjukkan rata-rata nilai rating dari pengguna pada 10 destinasi wisata yang paling banyak mendapatkan rating. Meskipun sebuah destinasi populer (memiliki banyak interaksi), tidak selalu berarti destinasi tersebut mendapatkan rating tertinggi.

### 6. Distribusi Nilai Rating dari Pengguna

![Distribusi Rating](img/distribusi_rating_pengguna.png)

Distribusi nilai rating yang diberikan oleh pengguna menunjukkan pola yang cukup menarik. Dari visualisasi, tampak bahwa rating 4 merupakan yang paling dominan, diikuti oleh rating 5 dan 3. Hal ini mencerminkan bahwa sebagian besar pengguna merasa puas hingga sangat puas terhadap destinasi wisata yang mereka kunjungi. Sementara itu, rating 2 hanya muncul dalam jumlah kecil, dan rating 1 tidak ditemukan sama sekali dalam dataset. Pola ini dapat diinterpretasikan bahwa kualitas destinasi ekowisata dalam dataset cenderung positif, atau bisa juga mengindikasikan adanya bias pengguna yang lebih memilih memberikan penilaian sedang hingga tinggi. 

Berikut adalah versi **penyesuaian Data Preparation** berdasarkan tahapan yang **kamu** lakukan dalam proyek sistem rekomendasi ekowisata, disusun agar sejalan dengan struktur milik temanmu:


## Data Preparation

### Teknik Data Preparation

* **Menggabungkan dataset** rating dan data tempat wisata menggunakan `place_id`.
* **Removing Duplicates**: Menghapus data duplikat berdasarkan `place_id`.
* **Handling Missing Values**:

  * Menghapus nilai kosong (NaN) pada kolom penting seperti `user_id`, `place_id`, dan `user_rating`.
  * Menghapus baris yang memiliki nilai kosong pada kolom gambar `gallery_photo_img2` dan `gallery_photo_img3`, karena gambar digunakan dalam fitur deskriptif tempat wisata.
  * Mengganti nilai `'-'` dan nilai kosong di kolom `price` dengan `0`.
* **Membersihkan format penulisan**: Menghapus simbol non-numerik pada kolom `price` agar bisa dikonversi ke tipe numerik.
* **Menghapus kolom yang tidak relevan** untuk sistem rekomendasi, seperti gambar dan peta.
* **Mengonversi beberapa kolom menjadi list** (opsional, digunakan untuk keperluan transformasi lanjutan).

### Proses Data Preparation

1. Menggabungkan `df_rating` dan `df_place` menggunakan `pd.merge()` berdasarkan `place_id`.
2. Menghapus baris duplikat berdasarkan `place_id` menggunakan `drop_duplicates()`.
3. Menghapus baris dengan nilai kosong pada kolom penting (`user_id`, `place_id`, `user_rating`) dan kolom gambar (`gallery_photo_img2`, `gallery_photo_img3`).
4. Mengganti tanda `'-'` dan nilai kosong di kolom `price` dengan `0`, lalu membersihkan karakter non-numerik dan mengonversinya ke tipe `float`.
5. Menghapus kolom yang tidak digunakan dalam model rekomendasi seperti:

   * `place_img`, `gallery_photo_img1`, `gallery_photo_img2`, `gallery_photo_img3`
   * `place_map`, dan `description_location`.
6. Memastikan semua kolom bebas dari nilai kosong dan siap digunakan dalam tahap selanjutnya.
7. Mengonversi kolom `place_id`, `place_name`, `user_id`, dan `user_rating` menjadi list.

### Alasan Tahapan Data Preparation

* **Removing Duplicates**: Untuk menghindari bias akibat data tempat wisata yang muncul lebih dari satu kali.
* **Handling Missing Values**:

  * Menghindari kesalahan proses pemodelan akibat nilai kosong.
  * Gambar galeri dianggap penting sebagai bagian dari fitur deskriptif konten wisata.
  * Kolom `price` perlu bernilai numerik agar dapat dianalisis dan digunakan dalam sistem rekomendasi.
* **Membersihkan format penulisan**: Supaya nilai dalam `price` dikenali sebagai angka, bukan string.
* **Mereplace value dan drop kolom tidak relevan**: Untuk menyederhanakan data hanya pada fitur yang relevan dan signifikan bagi sistem rekomendasi berbasis konten maupun kolaboratif.

========================================================================
## Modeling

Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**:

- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation

Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**:

- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_

- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
