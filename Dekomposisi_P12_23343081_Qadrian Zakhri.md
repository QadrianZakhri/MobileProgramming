# Dekomposisi_P12_23343081_Qadrian Zakhri

## Analisis Arsitektur Penyimpanan Data Lokal pada Aplikasi E-Commerce

---

# 1. Kategori Data Lokal pada Aplikasi E-Commerce

Dalam aplikasi e-commerce, tidak semua data memiliki karakteristik yang sama. Oleh karena itu, strategi penyimpanan harus disesuaikan berdasarkan volume data, frekuensi akses, tingkat sensitivitas, dan kebutuhan performa.

| No | Kategori Data       | Jenis Data                           | Volume       | Frekuensi Akses | Sensitivitas  | Mekanisme Penyimpanan |
| -- | ------------------- | ------------------------------------ | ------------ | --------------- | ------------- | --------------------- |
| 1  | Data Pengguna       | Nama, email, alamat, nomor telepon   | Kecil        | Tinggi          | Tinggi        | SQLite + Enkripsi     |
| 2  | Token Autentikasi   | Access Token, Refresh Token          | Sangat Kecil | Sangat Tinggi   | Sangat Tinggi | Secure Storage        |
| 3  | Keranjang Belanja   | Produk yang dipilih pengguna         | Sedang       | Sangat Tinggi   | Sedang        | SQLite / Hive         |
| 4  | Riwayat Pesanan     | Data transaksi dan status pesanan    | Besar        | Sedang          | Tinggi        | SQLite                |
| 5  | Cache Produk        | Daftar produk, kategori, gambar      | Besar        | Tinggi          | Rendah        | Hive / SQLite         |
| 6  | Pengaturan Aplikasi | Tema, bahasa, notifikasi             | Sangat Kecil | Sedang          | Rendah        | SharedPreferences     |
| 7  | Wishlist            | Produk favorit pengguna              | Sedang       | Tinggi          | Rendah        | SQLite                |
| 8  | Log Aktivitas       | Catatan error dan aktivitas aplikasi | Besar        | Rendah          | Sedang        | SQLite / File Storage |

---

## Penjelasan Kategori

### Data Pengguna

Berisi identitas pengguna yang digunakan selama aplikasi berjalan.

Contoh:

* Nama
* Email
* Alamat pengiriman
* Nomor telepon

Karena bersifat pribadi, data ini sebaiknya disimpan dalam database lokal yang mendukung enkripsi.

---

### Token Autentikasi

Berisi:

* Access Token
* Refresh Token

Data ini memiliki tingkat sensitivitas tertinggi sehingga direkomendasikan menggunakan:

* flutter_secure_storage
* Android Keystore
* iOS Keychain

---

### Keranjang Belanja

Berisi:

* Produk yang dipilih
* Jumlah item
* Variasi produk

Data sering berubah sehingga membutuhkan akses baca-tulis cepat.

---

### Cache Produk

Berfungsi untuk:

* Mempercepat loading
* Mendukung mode offline
* Mengurangi request API

Biasanya menjadi kategori data dengan volume terbesar.

---

# 2. Proses Migrasi Database SQLite dari Versi 1 ke Versi 2

## Studi Kasus

Versi 1:

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT
);
```

Versi 2:

Menambahkan kolom:

```sql
email TEXT
```

---

## Langkah 1: Analisis Kebutuhan Perubahan

### Aktivitas

Menentukan perubahan schema yang dibutuhkan.

Contoh:

* Menambah kolom
* Menghapus kolom
* Menambah tabel
* Menambah index

### Risiko

* Perubahan tidak kompatibel
* Kehilangan data lama
* Query lama menjadi error

---

## Langkah 2: Mendefinisikan Schema Baru

Schema versi 2:

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    name TEXT,
    email TEXT
);
```

### Risiko

* Salah tipe data
* Konflik constraint
* Kesalahan relasi tabel

---

## Langkah 3: Menulis Kode Migrasi

Contoh Flutter SQLite (sqflite):

```dart
onUpgrade: (db, oldVersion, newVersion) async {

  if (oldVersion < 2) {

    await db.execute(
      'ALTER TABLE users ADD COLUMN email TEXT'
    );

  }

}
```

### Risiko

* SQL syntax error
* Migrasi gagal di tengah proses
* Database menjadi inkonsisten

---

## Langkah 4: Membuat Backup Data

Sebelum migrasi:

* Salin database lama
* Simpan snapshot data

### Risiko

* Backup tidak valid
* Backup tidak dapat dipulihkan

---

## Langkah 5: Unit Testing Migrasi

Pengujian:

1. Install aplikasi versi 1
2. Isi data pengguna
3. Upgrade ke versi 2
4. Verifikasi data lama tetap ada

### Risiko

* Data lama hilang
* Field baru tidak terbentuk

---

## Langkah 6: Pengujian Integrasi

Menguji:

* Login
* Checkout
* Wishlist
* Riwayat pesanan

### Risiko

* Query lama gagal
* Crash pada halaman tertentu

---

## Langkah 7: Uji Performa

Memastikan migrasi tetap cepat walaupun:

* 100 data
* 1.000 data
* 10.000 data

### Risiko

* Startup aplikasi menjadi lambat
* ANR (Application Not Responding)

---

## Langkah 8: Deployment Bertahap

Tahapan:

1. Internal testing
2. Beta testing
3. Release production

### Risiko

* Bug tidak terdeteksi
* Migrasi gagal pada perangkat tertentu

---

## Langkah 9: Monitoring Pasca Deployment

Pantau:

* Crash report
* Error database
* Feedback pengguna

### Risiko

* Data korup
* Kehilangan data pengguna

---

## Ringkasan Alur Migrasi

```text
Perubahan Kebutuhan
        ↓
Desain Schema Baru
        ↓
Menulis Script Migrasi
        ↓
Backup Database
        ↓
Unit Testing
        ↓
Integration Testing
        ↓
Performance Testing
        ↓
Deployment
        ↓
Monitoring
```

---

# 3. Pertanyaan yang Harus Ditanyakan kepada Klien

Sebelum menentukan strategi penyimpanan data, tim pengembang harus memahami kebutuhan bisnis dan teknis aplikasi.

---

## A. Kebutuhan Teknis

### Struktur Data

1. Data apa saja yang akan disimpan?
2. Apakah data bersifat relasional?
3. Berapa estimasi jumlah data per pengguna?
4. Apakah aplikasi harus bekerja secara offline?
5. Seberapa sering data berubah?

### Performa

6. Berapa target waktu respon aplikasi?
7. Berapa jumlah pengguna aktif harian?
8. Apakah dibutuhkan cache lokal?
9. Apakah aplikasi harus mendukung sinkronisasi otomatis?
10. Apakah data perlu disimpan permanen atau sementara?

### Skalabilitas

11. Apakah volume data diperkirakan meningkat pesat?
12. Apakah aplikasi akan digunakan pada banyak perangkat?

---

## B. Kebutuhan Bisnis

### Operasional

1. Data apa yang paling penting bagi bisnis?
2. Berapa lama data harus disimpan?
3. Apakah diperlukan histori perubahan data?
4. Apakah data harus dapat dipulihkan jika terjadi kegagalan?

### Pengalaman Pengguna

5. Apakah pengguna harus bisa mengakses data saat offline?
6. Seberapa cepat halaman harus dimuat?
7. Apakah sinkronisasi real-time diperlukan?

### Anggaran

8. Berapa anggaran untuk penyimpanan data?
9. Apakah ada batasan biaya infrastruktur?
10. Apakah solusi open-source lebih diutamakan?

---

## C. Persyaratan Keamanan dan Regulasi

### Keamanan

1. Apakah data pengguna bersifat rahasia?
2. Apakah token autentikasi harus dienkripsi?
3. Apakah diperlukan proteksi terhadap pencurian perangkat?

### Regulasi

4. Apakah harus mematuhi GDPR?
5. Apakah harus mematuhi UU Perlindungan Data Pribadi (PDP)?
6. Apakah data harus disimpan dalam wilayah negara tertentu?

### Audit dan Kepatuhan

7. Apakah diperlukan audit log?
8. Siapa yang boleh mengakses data?
9. Apakah diperlukan pencatatan aktivitas pengguna?
10. Berapa lama data harus dipertahankan untuk kebutuhan audit?

---

# Kesimpulan

Pemilihan strategi penyimpanan data tidak hanya mempertimbangkan aspek teknis, tetapi juga kebutuhan bisnis dan regulasi. Pada aplikasi e-commerce, kombinasi SQLite, Secure Storage, Hive, dan SharedPreferences dapat digunakan sesuai karakteristik data yang disimpan. Sebelum implementasi, proses analisis kebutuhan dan perencanaan migrasi database harus dilakukan secara matang untuk menghindari kehilangan data dan gangguan layanan.
