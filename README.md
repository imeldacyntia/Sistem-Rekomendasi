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
   Menganalisis karakteristik destinasi yang pernah disukai pengguna, seperti kategori, deskripsi, dan lokasi geografis. Sistem kemudian merekomendasikan destinasi lain dengan fitur serupa.

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

### 1. Distribusi Kategori Destinasi Wisata

![Distribusi Kategori Wisata](img/kategori_wisata.png)

Grafik ini menunjukkan distribusi jumlah destinasi berdasarkan kategorinya. Terlihat bahwa kategori Alam mendominasi jumlah destinasi wisata dalam dataset ini, diikuti oleh kategori Budaya dan Edukasi. Beberapa kategori lain seperti Bahari, Religi, dan Urban tercatat dalam jumlah lebih kecil. Hal ini menegaskan bahwa ekowisata di Indonesia masih sangat terpusat pada aspek keindahan alam dan konservasi lingkungan.


### 2. Sebaran Destinasi Berdasarkan Kota

![Distribusi Kota](img/sebaran_kota.png)

Visualisasi ini memperlihatkan jumlah destinasi wisata berdasarkan kota atau kabupaten. **Yogyakarta** dan **Bandung** menjadi dua kota dengan jumlah destinasi terbanyak, mengindikasikan tingginya potensi wisata di daerah tersebut. Di sisi lain, beberapa kota seperti **Ambon** dan **Kupang** hanya memiliki sedikit destinasi dalam dataset, yang mungkin disebabkan oleh keterbatasan data atau kurangnya dokumentasi wisata digital di wilayah timur Indonesia.


### 3. Distribusi Durasi Kunjungan Wisata

![Durasi Kunjungan](img/durasi_kunjungan.png)

Durasi kunjungan wisata (dalam satuan menit) umumnya berkisar antara 60 hingga 120 menit, dengan puncaknya di durasi 90 menit. Hal ini menggambarkan bahwa sebagian besar wisatawan menghabiskan waktu yang relatif sedang untuk menjelajahi lokasi wisata alam. Durasi yang sangat singkat (<30 menit) tercatat sangat jarang, yang masuk akal mengingat sebagian besar lokasi wisata membutuhkan waktu tempuh dan eksplorasi lebih lama.

### 4. Distribusi Destinasi Berdasarkan Latitude

![Sebaran Latitude](img/sebaran_latitude.png)

Sebaran geografis destinasi wisata dalam dataset ini dikelompokkan berdasarkan rentang latitude. Rentang (-8.5, -7.5) mencatat jumlah destinasi terbanyak, mengindikasikan konsentrasi wisata alam di wilayah Jawa bagian tengah dan selatan. Sementara itu, wilayah dengan latitude lebih utara atau selatan dari rentang tersebut menunjukkan jumlah destinasi yang lebih sedikit. Distribusi ini mencerminkan penyebaran ekowisata yang masih terkonsentrasi di wilayah tertentu.


### 5. Interaksi Pengguna per Destinasi (place\_id)

![Interaksi Place ID](img/interaksi_placeid.png)

Grafik ini menunjukkan frekuensi interaksi pengguna (rating) terhadap masing-masing destinasi. Beberapa `place_id`, seperti ID 15 dan ID 22, menerima jumlah rating jauh lebih tinggi dibandingkan lainnya, yang menandakan popularitas tempat tersebut di kalangan pengguna. Mayoritas destinasi lainnya memiliki interaksi yang cukup merata di kisaran 10–20 ulasan. Pola ini dapat dimanfaatkan dalam pendekatan collaborative filtering untuk merekomendasikan destinasi berdasarkan destinasi populer di antara pengguna lain.


### 6. Distribusi Nilai Rating dari Pengguna

![Distribusi Rating](img/distribusi_rating.png)

Rating yang diberikan pengguna berkisar dari 1 hingga 5. Menariknya, rating 4 menjadi yang paling dominan, diikuti oleh rating 5 dan 3. Hal ini mengindikasikan bahwa mayoritas pengguna merasa cukup puas terhadap pengalaman wisata mereka. Sementara itu, rating 1 dan 2 hanya muncul dalam jumlah kecil, menunjukkan bahwa destinasi yang tersedia dalam dataset cenderung memiliki kualitas pelayanan atau daya tarik yang cukup baik.

## Data Preparation

Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**:

- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

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
