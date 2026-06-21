# Algoritma_P12_23343081_Qadrian Zakhri

## Analisis Pemilihan Penyimpanan Data, Audit Keamanan, dan Offline Support pada Flutter

---

# 1. Decision Tree Pemilihan Mekanisme Penyimpanan Data

Tujuan dari decision tree ini adalah membantu developer menentukan mekanisme penyimpanan yang paling sesuai berdasarkan karakteristik data dan kebutuhan aplikasi.

## Diagram Alur
<img width="1024" height="504" alt="image" src="https://github.com/user-attachments/assets/717513e2-1596-49a8-bb33-5ce87a43b10d" />

```

## Ringkasan Hasil

| Kebutuhan                    | Rekomendasi         |
| ---------------------------- | ------------------- |
| Token Login                  | Secure Storage      |
| Data Sensitif Relasional     | SQLite + Encryption |
| Data Sensitif Non Relasional | Encrypted Hive      |
| Query Kompleks               | SQLite              |
| Cache Cepat                  | Hive                |
| Pengaturan Aplikasi          | SharedPreferences   |
| Sinkronisasi Real-Time       | Firebase Firestore  |
| Data Besar Offline           | SQLite              |

---

# 2. Algoritma Audit Keamanan Penyimpanan Data Flutter

## Tujuan

Melakukan evaluasi terhadap seluruh mekanisme penyimpanan data yang digunakan aplikasi Flutter untuk memastikan keamanan dan kepatuhan terhadap standar pengembangan.

---

## Tahap 1: Identifikasi Semua Titik Penyimpanan Data

### Aktivitas

Inventarisasi seluruh lokasi penyimpanan data:

* SharedPreferences
* Secure Storage
* SQLite
* Hive
* File Storage
* Cache Image
* Firebase Local Cache

### Output

Tabel inventaris data.

Contoh:

| Lokasi            | Data         |
| ----------------- | ------------ |
| SharedPreferences | Tema         |
| Secure Storage    | Token        |
| SQLite            | Keranjang    |
| Hive              | Cache Produk |

### Risiko

* Ada storage yang tidak teridentifikasi
* Penyimpanan tersembunyi pada package pihak ketiga

---

## Tahap 2: Klasifikasi Sensitivitas Data

### Kategori

#### Rendah

* Tema
* Bahasa
* Preferensi aplikasi

#### Sedang

* Wishlist
* Riwayat pencarian

#### Tinggi

* Email
* Nomor telepon
* Alamat

#### Sangat Tinggi

* Password
* Access Token
* Refresh Token

### Output

Matriks sensitivitas data.

---

## Tahap 3: Pemeriksaan Mekanisme Penyimpanan

### Pemeriksaan

#### SharedPreferences

Periksa apakah digunakan untuk:

* Password
* Token
* Data pribadi

Jika Ya → Temuan kritis.

---

#### SQLite

Periksa:

* Apakah database terenkripsi?
* Apakah ada proteksi backup?

---

#### Hive

Periksa:

* Apakah box terenkripsi?
* Apakah data sensitif disimpan?

---

#### Secure Storage

Periksa:

* Token tersimpan dengan benar
* Tidak ada duplikasi di lokasi lain

### Output

Daftar pelanggaran keamanan.

---

## Tahap 4: Pemeriksaan Alur Data

Analisis:

```text
API
 ↓
Repository
 ↓
Storage
 ↓
UI
```

Periksa:

* Apakah token pernah tampil di log?
* Apakah data sensitif muncul pada debug console?
* Apakah data sensitif tersimpan di cache?

### Risiko

* Kebocoran data
* Reverse engineering

---

## Tahap 5: Penyusunan Laporan

### Format Laporan

#### Temuan

Contoh:

| Temuan                              | Risiko |
| ----------------------------------- | ------ |
| Token disimpan di SharedPreferences | Tinggi |
| Database tidak terenkripsi          | Tinggi |
| Cache tidak dibersihkan             | Sedang |

#### Rekomendasi

| Temuan            | Solusi          |
| ----------------- | --------------- |
| SharedPreferences | Secure Storage  |
| SQLite biasa      | SQLCipher       |
| Hive biasa        | Hive Encryption |

---

# 3. Langkah Menambahkan Offline Support pada Aplikasi Flutter

## Kondisi Awal

Aplikasi hanya mengambil data dari API dan tidak dapat digunakan tanpa internet.

---

## Tahap 1: Identifikasi Data yang Perlu Di-cache

### Data Prioritas Tinggi

* Produk
* Kategori
* Keranjang
* Profil pengguna
* Riwayat pesanan

### Data yang Tidak Perlu

* OTP
* Notifikasi sementara
* Session sementara

### Output

Daftar data yang akan tersedia offline.

---

## Tahap 2: Memilih Mekanisme Caching

### SharedPreferences

Untuk:

* Pengaturan aplikasi

### Secure Storage

Untuk:

* Token
* Kredensial

### Hive

Untuk:

* Cache cepat
* Data sederhana

### SQLite

Untuk:

* Data relasional
* Riwayat transaksi
* Keranjang

---

## Tahap 3: Menambahkan Local Data Source

Contoh:

```dart
class ProductLocalDataSource {

  Future<List<ProductModel>>
  getCachedProducts() async {}

  Future<void>
  saveProducts(
      List<ProductModel> products) async {}
}
```

---

## Tahap 4: Memodifikasi Repository Pattern

Sebelum:

```text
UI
 ↓
Repository
 ↓
API
```

Sesudah:

```text
UI
 ↓
Repository
 ↓
Remote Data Source
 ↓
API

Jika gagal:
 ↓
Local Data Source
 ↓
SQLite/Hive
```

---

## Contoh Implementasi

```dart
@override
Future<List<Product>>
getProducts() async {

  try {

    final remoteData =
        await remote.getProducts();

    await local.saveProducts(
      remoteData,
    );

    return remoteData;

  } catch (_) {

    return await local
        .getCachedProducts();
  }
}
```

---

## Tahap 5: Sinkronisasi Saat Online Kembali

Ketika koneksi tersedia:

1. Ambil data terbaru dari server.
2. Bandingkan dengan cache.
3. Perbarui database lokal.
4. Perbarui UI.

---

## Tahap 6: Menambahkan Deteksi Koneksi

Package:

```yaml
connectivity_plus
```

Contoh:

```dart
final result =
    await Connectivity()
        .checkConnectivity();
```

Status:

* Online
* Offline

---

## Tahap 7: Menampilkan Indikator Offline

Contoh UI:

```text
⚠ Mode Offline Aktif

Menampilkan data terakhir yang tersimpan.
```

---

## Tahap 8: Testing Skenario Offline

### Skenario 1

Internet tersedia.

Expected:

* Data dari API.

---

### Skenario 2

Internet terputus.

Expected:

* Data dari cache lokal.

---

### Skenario 3

Internet kembali.

Expected:

* Sinkronisasi otomatis.

---

### Skenario 4

Cache kosong dan offline.

Expected:

```text
Tidak ada data offline tersedia.
```

---

# Kesimpulan

Pemilihan mekanisme penyimpanan harus mempertimbangkan sensitivitas data, kebutuhan query, volume data, dan kebutuhan sinkronisasi. Audit keamanan membantu memastikan tidak ada data penting yang disimpan secara tidak aman. Dengan menambahkan Local Data Source, Repository Pattern, dan mekanisme caching yang tepat, aplikasi Flutter dapat mendukung mode offline secara efektif serta memberikan pengalaman pengguna yang lebih baik.
