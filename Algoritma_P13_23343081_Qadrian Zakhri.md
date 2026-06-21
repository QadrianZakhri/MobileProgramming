# Algoritma_P13_23343081_Qadrian Zakhri

# Evaluasi Arsitektur, Pair Programming Virtual, dan Refactoring ke MVVM

---

# 1. Algoritma Evaluasi Arsitektur untuk Proyek Baru

## Tujuan

Menentukan arsitektur aplikasi yang paling sesuai berdasarkan kondisi proyek, ukuran tim, kompleksitas bisnis, kebutuhan testing, dan batas waktu pengembangan.

---

## Flowchart Evaluasi Arsitektur

<img width="641" height="357" alt="image" src="https://github.com/user-attachments/assets/be395c7e-127c-41d8-a905-33181616ad2b" />


---

## Langkah Evaluasi

### Langkah 1 – Identifikasi Skala Tim

Pertanyaan:

* Berapa jumlah developer?
* Apakah ada pembagian tugas khusus?

### Keputusan

| Ukuran Tim | Rekomendasi Awal          |
| ---------- | ------------------------- |
| 1–2 orang  | MVVM                      |
| 3–5 orang  | MVVM / Clean Architecture |
| >5 orang   | Clean Architecture        |

---

### Langkah 2 – Estimasi Kompleksitas Domain Bisnis

Contoh:

#### Rendah

* To-do List
* Catatan pribadi

#### Sedang

* Sistem reservasi
* E-commerce sederhana

#### Tinggi

* Mobile Banking
* Sistem kesehatan
* ERP

---

### Langkah 3 – Pertimbangan Testability

Pertanyaan:

* Apakah unit test wajib?
* Apakah bisnis logic kompleks?

Jika ya:

→ Gunakan Clean Architecture.

---

### Langkah 4 – Evaluasi Timeline

| Timeline       | Pilihan            |
| -------------- | ------------------ |
| Sangat singkat | MVVM               |
| Menengah       | MVVM               |
| Panjang        | Clean Architecture |

---

### Langkah 5 – Pilih Arsitektur

| Kondisi                  | Arsitektur         |
| ------------------------ | ------------------ |
| Tim kecil, cepat selesai | MVVM               |
| Bisnis kompleks          | Clean Architecture |
| Testability tinggi       | Clean Architecture |
| Prototipe cepat          | MVC/MVVM           |

---

### Langkah 6 – Dokumentasikan Alasan

Contoh:

```text
Dipilih Clean Architecture karena:

- Tim terdiri dari 6 developer
- Domain bisnis kompleks
- Unit testing wajib
- Proyek jangka panjang
```

---

# 2. Pair Programming Virtual Menggunakan VS Code Live Share

## Tujuan

Memungkinkan dua developer bekerja bersama secara real-time meskipun berada di lokasi berbeda.

---

## Langkah 1 – Persiapan

### Kedua Developer Menginstal

* Visual Studio Code
* Extension Live Share

---

## Langkah 2 – Membuka Sesi Live Share

Host:

```text
Live Share
↓
Start Collaboration Session
↓
Copy Link
↓
Kirim ke Partner
```

Guest:

```text
Klik Link
↓
Join Session
```

---

## Langkah 3 – Menentukan Driver dan Navigator

### Driver

Bertugas:

* Menulis kode
* Mengoperasikan editor

### Navigator

Bertugas:

* Memberikan arahan
* Mengidentifikasi bug
* Meninjau desain

---

## Contoh Pembagian

| Role      | Tugas                   |
| --------- | ----------------------- |
| Driver    | Menulis ViewModel       |
| Navigator | Memastikan logika benar |

---

## Langkah 4 – Mengimplementasikan ViewModel Bersama

Contoh:

```dart
class ProductViewModel {

  final ProductRepository repository;

  ProductViewModel(this.repository);

  Future<List<Product>> getProducts() async {
    return await repository.getProducts();
  }
}
```

---

## Langkah 5 – Diskusi dan Validasi

Navigator memeriksa:

* Naming convention
* Clean code
* Potensi bug
* Dependency yang digunakan

---

## Langkah 6 – Code Review Singkat

Checklist:

* Kode berhasil build
* Tidak ada warning
* Logic sesuai kebutuhan
* Struktur folder benar

---

## Langkah 7 – Commit dan Push

Contoh:

```bash
git add .
git commit -m "Implement Product ViewModel"
git push
```

---

## Langkah 8 – Dokumentasi di GitHub

Tambahkan:

```md
## Pair Programming Session

Tanggal: 20 Juni 2026

Driver:
- Developer A

Navigator:
- Developer B

Hasil:
- ProductViewModel selesai dibuat
- Repository berhasil diintegrasikan
```

---

# 3. Prosedur Refactoring God Widget Menjadi MVVM

## Kondisi Awal

Semua kode berada dalam satu widget besar.

Contoh:

```text
ProductPage
├─ UI
├─ API Call
├─ Validation
├─ Business Logic
├─ Database
└─ State Management
```

Masalah:

* Sulit diuji
* Sulit dipelihara
* Sulit dikembangkan

---

## Tujuan Refactoring

Mengubah struktur menjadi:

```text
View
 ↓
ViewModel
 ↓
Repository
 ↓
Data Source
```

Tanpa mengubah perilaku aplikasi.

---

## Langkah 1 – Membuat Backup

### Aktivitas

* Commit ke Git
* Buat branch baru

```bash
git checkout -b refactor-mvvm
```

### Risiko

* Kehilangan kode lama

---

## Langkah 2 – Menambahkan Testing Dasar

Sebelum refactor:

* Catat fitur yang berjalan
* Buat checklist

Contoh:

```text
✓ Login berhasil
✓ Produk tampil
✓ Logout berhasil
```

### Risiko

* Tidak mengetahui fitur yang rusak

---

## Langkah 3 – Identifikasi Logic dan UI

Pisahkan:

### UI

* Scaffold
* Button
* TextField

### Logic

* API Call
* Validasi
* Perhitungan

### Risiko

* Logic terlewat

---

## Langkah 4 – Membuat Model

Pindahkan data ke model.

Contoh:

```dart
class Product {
  final int id;
  final String name;
}
```

### Risiko

* Mapping data salah

---

## Langkah 5 – Membuat Repository

Pindahkan akses data.

Contoh:

```dart
abstract class ProductRepository {
  Future<List<Product>> getProducts();
}
```

### Risiko

* API tidak lagi terhubung

---

## Langkah 6 – Membuat ViewModel

Pindahkan business logic.

Contoh:

```dart
class ProductViewModel {

  final ProductRepository repository;

  ProductViewModel(this.repository);

  Future<List<Product>>
  getProducts() async {
    return repository.getProducts();
  }
}
```

### Risiko

* State tidak tersinkronisasi

---

## Langkah 7 – Hubungkan View ke ViewModel

Sebelumnya:

```text
UI
 ↓
API
```

Sesudah:

```text
UI
 ↓
ViewModel
 ↓
Repository
```

### Risiko

* Error dependency

---

## Langkah 8 – Refactor Bertahap

Jangan memindahkan semua sekaligus.

Urutan:

1. Model
2. Repository
3. ViewModel
4. UI

### Risiko

* Sulit menemukan sumber bug

---

## Langkah 9 – Regression Testing

Uji kembali seluruh fitur.

Checklist:

```text
✓ Login
✓ Logout
✓ Load Produk
✓ Refresh Data
✓ Error Handling
```

### Risiko

* Ada fitur yang berubah tanpa disadari

---

## Langkah 10 – Merge ke Branch Utama

Jika seluruh test berhasil:

```bash
git merge refactor-mvvm
```

---

# Kesimpulan

Pemilihan arsitektur harus mempertimbangkan ukuran tim, kompleksitas bisnis, kebutuhan testing, dan timeline proyek. Pair programming dengan VS Code Live Share membantu meningkatkan kualitas kode dan kolaborasi tim. Refactoring dari God Widget ke MVVM harus dilakukan secara bertahap dengan dukungan testing dan version control agar fungsionalitas aplikasi tetap terjaga selama proses perubahan.
