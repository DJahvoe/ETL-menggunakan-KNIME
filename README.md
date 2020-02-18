# **Japanese Hostel Dataset**

## _Business Understanding_

### Kemungkinan proses yang dapat dilakukan

- Mencari jarak hostel terdekat dari lokasi tertentu
- Mencari rating hostel tertinggi
- Mencari harga hostel yang murah

## _Data Understanding_

- jumlah data sebanyak **342 baris**

- hostel.name: **nama** hostel
- City: **nama kota** tempat lokasi hostel
- price.from: **harga minimal** untuk menginap 1 malam
- Distance: **jarak dari pusat kota** (km)
- summary.score: kesimpulan **skor rating secara keseluruhan**
- rating.band: **label** rating
- atmosphere: skor rating untuk **atmosphere**
- cleanliness: skor rating untuk **kebersihan**
- facilities: skor rating untuk **fasilitas**
- location.y: skor rating **lokasi**
- security: skor rating untuk **keamanan**
- staff: skor rating untuk **staff**
- valueformoney: skor rating **nilai kepuasan** atas uang yang telah dibayar
- lon: derajat **garis bujur**
- lat: derajat **garis lintang**

## _Data Preparation_
![Split Data](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/Splitting%20Data.jpg)
### Proses splitting data:
1. Load file Hostel.csv
2. Split column dengan memisah antara,

   - kolom tabel Description
     - hostel.name
     - City
     - price.from
     - Distance
     - lon
     - lat
   - dan kolom tabel Rating
     - summary.score
     - rating.band
     - atmosphere
     - cleanliness
     - facilities
     - location.y
     - security
     - staff
     - valueformoney
![Splitter setting](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/Column%20Splitter.jpg)
3. Simpan dalam file sesuai nama masing - masing
   - tabel Description disimpan sebagai Hostel_Desc.csv
   - tabel Rating disimpan sebagai Hostel_Rating.csv

### Store Hostel_Rating.csv dalam Database
![Store Localhost](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/Store%20Hostel_Rating%20to%20localhost_2.jpg)
1. Connect dengan _MySQL Connector_
2. Load Hostel_Rating.csv dengan _CSV Reader_
3. Store data hostel_rating dengan _DB Writer_
## _Modelling_
![Baca Data Dua Sumber](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/Membaca%20data%20dari%20dua%20sumber.jpg)
### Membaca data dari dua sumber

- Membaca data hostel_rating dari Database
  1. Membuat koneksi menggunakan _MySQL Connector_ (menginput Hostname, Database name, Username & password)
  2. Dengan menggunakan _DB Table Selector_, Menginput Schema _hostel_ dan Tabel _hostel_rating_
  3. Memasukan hasil ke dalam _DB Reader_
  4. Execute
  5. Data dapat terbaca dengan membuka data table
  ![Data Hostel Rating](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/hostel_rating%20table.jpg)
- Membaca data Hostel_Desc
  1. Menggunakan _CSV Reader_ memasukkan path di mana file Hostel_Desc.csv disimpan
  2. Execute
  3. Data dapat terbaca dengan membuka data table
  ![Data Hostel_Desc](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/hostel_desc%20table.jpg)

### Proses Append
![Proses Append](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/Append%20Dataset_2.jpg)
1. Memasukkan hasil _Read hostel_rating table_ dan _Load Hostel_Desc.csv_ ke input _Column Appender_
2. Melakukan setting _Column Appender_ (Tidak diperlukan apabila kedua tabel memiliki ID yang sama)
3. Execute

## _Evaluation_
![Append Table](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/appended%20table.jpg)
Proses Append berhasil dilakukan

## _Deployment_
![Hasil Append](https://github.com/DJahvoe/ETL-menggunakan-KNIME/blob/master/screenshot/Save%20data%20to%20localfile%20and%20database_2.jpg)
### Penyimpanan hasil append ke dalam file dan database

- Penyimpanan tabel dalam file CSV
  1. Menerima data dari _Column Appender_
  2. Memasukan path yang diinginkan ke dalam _CSV Writer_
  3. Execute
  4. Tabel hasil append berhasil disimpan pada file CSV
- Penyimpanan tabel dalam Database
  1. Menerima data dari _Column Appender_
  2. Membuat koneksi menggunakan _MySQL Connector_
  3. Memasukan data Schema dan Table yang ingin di-insert pada _DB Writer_
  4. Execute
  5. Tabel hasil append berhasil disimpan pada Database
