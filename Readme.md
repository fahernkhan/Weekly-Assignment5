# Weekly Assignment 5

**by Fathurrahman Hernanda Khasan DF12**

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled.png)

Pada Projek kali ini saya akan membuat pipline ETL sederhana dengan kriteria: 

1. Data dari csv pada folder weekly assignment di load pada postgresQL local

machine.

2. Kemudian melakukan transformasi, filtering, enriching data.

3. Setelah datanya bersih, atau sesuai yang dikehendaki, load ke Cloud.

SQL di google cloud platform dengan nama table yang sama.

4. Setelah datanyanya landing pada Cloud SQL, offload ke BigQuery secara as is

atau apa adannya.

### Tambahan dokumentasi Untuk proyek ini diberi dokumentasi di github:

### Keterangan transformasi sampai settingan databasenya, data catalognya, data modelnya.

Oke langsung saja berikut Penjelasannya dalam penjelasan ini aku bagi penjelasannya menjadi 3 section Yaitu:

1. **Section 1 “Extract”**
2. **Section 2 “Transform”**
3. **Section 3 “Load”**

# Extract

Pada proses Extract disini saya melakukannya dengan mengambil data banksim_artificial.csv yang disediakan oleh mentor. pertama tama pada proses ekstract saya mengconnectkan data banksim_artificial.csv tadi ke jupyter notebook saya menggunakan psycopg2 dan membaca dataframenya menggunakan pandas.

Mengconnectkan database menggunakan psycopg2:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%201.png)

Melhat Karakteristik:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%202.png)

Setelah dilihat bahwa terlihat jelas ya bahwa dataset ini masih belum begitu clean ya bisa dilihat penggunaan tipe data dan bentuk dari dataset masih mengandung karakter yang tidak diperlukan oleh karena itu nanti akan kita bersihkan di saat proses transformasi namun sebelum itu kita perlu membuat table untuk menampung dile dataset ini:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%203.png)

Setelah tabel selesai dibuat baru masukkan data banksim_artificial.csv ke dalam database tersebut:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%204.png)

# Transform

Setelah dataset di load ke database saatnya kita melakukan transform data, masih ingatkan sebelumnya bahwa data yang kita pakai disini masih kotor oleh karena itu kita perlu membersihkannya agar enak digunakan untuk analytics.

pertama-tama untuk membersihkan data ini saya menghilangkan dan mengubah karakter yang tidak penting di database:

Menghilangkan tanda (“”) yang tidak perlu:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%205.png)

Setelah itu Saya melihat ada anomaly di gender yaitu terdapat nilai U di gender oleh karena itu saya melakukan karena terdapat nilai ‘U’ disana maka selanjutnya saya mengubah kareakter ini ke angka ‘2’ atau modus dan rata rata dari age.

Alasan mengapa menggunakan modus atau rata-rata adalah pilihan yang masuk akal untuk mengganti nilai 'u' dalam data usia yang salah. Tujuannya adalah untuk memberikan representasi yang masuk akal dari data usia yang valid tanpa memperkenalkan bias yang signifikan.  

Anomaly data:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%206.png)

Transformasi nilai yang anomaly:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%207.png)

setelah melakukan hal ini sebenarnya masih ada yang anomaly lagi yaitu bagian kedua zipcode disana keduanya memiliki nilai yang sama semuanya namun karena saya disini bertugas sebagai data engineer saya mentransformasikan data tersebut lebih ke mengubah tipe datanya saja dan untuk menghandle hal ini saya lebih percaya ke data analyst atau data scientist yang transformasi karena mungkin saja ini adalah data yang penting jadi saya tidak drop kolomnya dan hanya mengubah tipe datanya saja.

Update datatype zipcode:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%208.png)

Update data type age yang sebelumnya yang masih varchar atau string di transformasikan ke small integer: 

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%209.png)

Nah untuk validasi dan pengecekan apakah datanya sudah bersih dan siap dimasukkan ke staging atau cloud sql atau ke data warehouse di bigquey disini saya kembali menggunakan pandas.

Cek tabel:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2010.png)

Cek statistik dan data type:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2011.png)

Cek Null value:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2012.png)

Nice mantap kita telah menyelesaikan transformasi datanya dengan maksimal semoga dengan ini data analyst dan data scientist nyman menggunakan data ini 😁. Oke saatnya masuk ke proses terakhir yaitu Load.

# **Load**

Proses Load disini ada 2 objektif yaitu data di load ke staging area yaitu google cloud sql kemudian data di load ke data warehouse, pada kasus ini data warehousenya adalah google bigquery.

Untuk load data ke google cloud sql disini saya menggunakan treatment yaitu dengan file sql yang dibuat dari database on premise saya yang dibantu dengan comment pg_dump yang di konfig sesuai gcp. berikut untuk dokumentasi lebih jelasnya confignya seperti apa:

[Export and import using pg_dump, pg_dumpall, and pg_restore  |  Cloud SQL for PostgreSQL  |  Google Cloud](https://cloud.google.com/sql/docs/postgres/import-export/import-export-dmp)

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2013.png)

Sebelum itu kita juga perlu membuat GCS bucket untuk menampung dan upload file .sql:

Membuat GCS bucket:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2014.png)

Upload file ke GCS:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2015.png)

Dan pada tahap ini kita perlu setup google cloud sqlnya terlebih dahulu dengan create instance.

Create instance dan pilih postgreSQL:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2016.png)

Pilih Memory sesuai data yang digunakan disini saya memilih yang paling kecil karena sudah cukup dan untuk meminimalkan cost.

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2017.png)

Tunggu hingga Instance berhasil dibuat.

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2018.png)

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2019.png)

Kemudian masuk kedalam instance cloud sql tersebut:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2020.png)

Dan pilih file pg_dump .sql yang telah di upload ke GCS sebelumnya.

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2021.png)

Pilih file format SQL

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2022.png)

Tunggu hingga selesai lalu pergi ke cloud system studio untuk cek data masuk atau belum:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2023.png)

jika sudah query saja sesukanya untuk cek datanya dan pastikan datanya sesuai.

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2024.png)

Setelah selesai load data ke cloud sql selanjutnya kita akan melaksanakan objective kedua yaitu connecting data yang ada di cloud sql dan dipindahkan ke google bigquery maka dari itu langkah pertama yang harus dilakukan adalah Create dataset di google bigquey terlebih dahulu:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2025.png)

Kemudian create table dengan kolom dan tipe data yang sesuai:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2026.png)

jika sudah connectkan dengan tools dataflow dan create job:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2027.png)

atur target dan flow yang diinginkan disini saya menggunakan template dari GCP postgre to Bigquery lalu isi sesuai spesifikasimu lalu run job:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2028.png)

## Alternatif ke dua menggunakan Google DataStream

Jika menggunakan dataflow dianggap lebih susah dan ribet confignya ada alternatif lain dan kebetulan disini saya lebih fokus menggunakan ini dalam pipeline saya yaitu menggunakan datastream.

Pertama-tama cari datastream di search bar kemudian hidupkan APInya agar bisa digunakan:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2029.png)

Kemudian create stream:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2030.png)

Kemudian isi configurasi sesuai yang diperlukan dan pilih source postgresql dan destinationnya adalah bigquery sesuai target dari pipeline ini: 

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2031.png)

Kemudian isi kredensial yang sesuai target, dalam hal ini postgresql di cloud sql:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2032.png)

Kemudian define connectivity methodnya disini saya memilih ip allowlisting dan pilih lokasi servernya ketika sudah akan muncul 5 server setelah itu jangan lupa untuk memasukkan kelima koneksi tadi ke connections cloud sql supaya servicenya bisa saling berkomunkasi, lalu save.

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2033.png)

Kemudian test konesinya pastikan ‘Test passed’: 

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2034.png)

Pada bagian configure source masukan nama dari koneksi yang ingin dibuat dan pilih shema database dan tabel mana yang ingin di replikasi:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2035.png)

Disini juga bisa mengecek column selection dan pastikan pilih yang ingin direplikasi kalau saya disini ingin mereplikasi semua kolomnya:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2036.png)

Setelah dari bagian configure source anda bisa ke bagian define destination dan isi communication profile name dan idnya:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2037.png)

Lanjut ke configure destination dan isi sesuai keperluan:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2038.png)

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2039.png)

lalu pilih waktu untuk trigger bahkan bisa realtime streaming namanya saja data stream🙂:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2040.png)

kemudian review dan run validation: 

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2041.png)

Ini dia pentingnya validasi bisa dilihat dibawah ini ada beberapa fungsi yang tidak tervalidasi, dan jika ada yang memiliki permasalahan seperti dibawah bisa ikuti cara setelah ini, so jan lupa untuk ck dan validasi sebelum deploy.

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2042.png)

Permasalahan sebelumnya terjadi karena ada beberapa fungsi yang belum saya config di bagian get started oleh karena itu perlu beberapa penyesuaian configurasi terutama di bagian permissions dan ikuti dengan baik prerequisitenya dari awal hingga akhir yang dikonfigurasi di postgresql cloud sql. 

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2043.png)

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2044.png)

Jika sudah lakukan validasi lagi maka niscaya validation run complate semua 😀. 

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2045.png)

Kemudian lakukan create dan start:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2046.png)

dan pastikan statusnya berubah yang sebelumnya startting menjadi running:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2047.png)

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2048.png)

Last kamu bisa cek di google bigquery dengan melakukan select all, dan pastikan data sudah masuk semuanya.

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2049.png)

Jadi itu adalah gambaran singkat dokumentasi pembuatan simple pipeline ETL dari postgresql onpremise hingga ke data warehouse, berikut adalah gambar pipeline yang telah dibuat sebelumnya:

![Untitled](Weekly%20Assignment%205%20bbcd21d9fec04b50881fcd3b3c43607f/Untitled%2050.png)

keterangan: 

1. Mengambil dataset berbentuk csv.
2. Memasukkan dataset ke database (postgresql).
3. Transformasi data dari tipe data dan proporsi agar menjadi lebih bersih.
4. Memasukkan data berbentuk sql dari database on premise ke Google Cloud Storage.
5. Memasukkan atau migrasi data dari databse on premise yang sudah di transformasi yang disimpan ke gcs ke postgre sql cloud sql sebagai data staging.
6. Mengatur aliran data dalam sistem menggunakan dataflow, atau menggunakan datastream untuk mengalirkan  data secara terus-menerus yang memungkinkan analisis real-time.
7. Menyimpan data akhir ke data warehouse yaitu google bigquery. 

Bisa disimpulkan pipeline ini memulai dengan pengambilan dataset dalam format CSV, kemudian memasukkan dataset ke database PostgreSQL. Selanjutnya, dilakukan transformasi data untuk membersihkan tipe data dan proporsi. Data yang telah diubah kemudian dimasukkan ke Google Cloud Storage dalam format SQL dari database on premise. Proses migrasi data dari database on premise yang telah diubah ke PostgreSQL Cloud SQL dilakukan sebagai data staging. Aliran data dalam sistem dapat diatur menggunakan Dataflow atau menggunakan Datastream untuk analisis real-time. Akhirnya, data disimpan ke data warehouse Google BigQuery.

link code: [https://github.com/fahernkhan/Weekly-Assignment-IYKRA](https://github.com/fahernkhan/Weekly-Assignment-IYKRA)

thanks🦄