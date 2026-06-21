# Dekomposisi_P13_23343081_Qadrian Zakhri

## Dekomposisi Fitur Login, Pembagian Tim MVVM, dan Implementasi Daftar Produk dengan Clean Architecture + BLoC

---

# 1. Dekomposisi Fitur "Login dan Autentikasi" dalam Clean Architecture

## Gambaran Umum

Fitur Login dan Autentikasi bertujuan untuk memverifikasi identitas pengguna dan memberikan akses ke aplikasi menggunakan token autentikasi.

---

## A. Entity yang Terlibat

Entity merupakan representasi objek bisnis utama yang tidak bergantung pada framework maupun database.

### User Entity

```dart
class User {
  final int id;
  final String name;
  final String email;

  User({
    required this.id,
    required this.name,
    required this.email,
  });
}
```

### AuthToken Entity

```dart
class AuthToken {
  final String accessToken;
  final String refreshToken;

  AuthToken({
    required this.accessToken,
    required this.refreshToken,
  });
}
```

### Daftar Entity

| Entity       | Fungsi                       |
| ------------ | ---------------------------- |
| User         | Menyimpan informasi pengguna |
| AuthToken    | Menyimpan token autentikasi  |
| LoginRequest | Menyimpan email dan password |

---

## B. Use Case yang Diperlukan

Use Case berisi aturan bisnis aplikasi.

### Login Use Case

Tugas:

* Mengirim email dan password
* Memvalidasi hasil login

### Logout Use Case

Tugas:

* Menghapus token
* Mengakhiri sesi pengguna

### CheckAuthStatus Use Case

Tugas:

* Memeriksa status login pengguna

### RefreshToken Use Case

Tugas:

* Meminta token baru saat token lama kadaluarsa

---

## Daftar Use Case

| Use Case        | Fungsi                |
| --------------- | --------------------- |
| Login           | Login pengguna        |
| Logout          | Keluar dari aplikasi  |
| CheckAuthStatus | Mengecek sesi login   |
| RefreshToken    | Memperbarui token     |
| SaveToken       | Menyimpan token lokal |

---

## C. Data Source yang Digunakan

### Remote Data Source

Sumber data dari server/API.

Contoh endpoint:

```http
POST /api/login
POST /api/logout
POST /api/refresh-token
```

Contoh file:

```text
auth_remote_data_source.dart
```

---

### Local Data Source

Digunakan untuk menyimpan token.

Contoh:

```text
flutter_secure_storage
```

File:

```text
auth_local_data_source.dart
```

---

## D. Alur Presentation Layer

### Flow Login

```text
User Input
     │
     ▼
Login Page
     │
     ▼
Login Button
     │
     ▼
LoginBloc
     │
     ▼
Login Use Case
     │
     ▼
Repository
     │
     ▼
Remote Data Source
     │
     ▼
API Server
     │
     ▼
Response
     │
     ▼
Repository
     │
     ▼
Local Storage
     │
     ▼
Bloc State Success
     │
     ▼
Dashboard Page
```

### State pada Presentation Layer

```text
Initial
Loading
Success
Failure
```

---

# 2. Dekomposisi Pekerjaan Tim MVVM (4 Developer)

## Tujuan

Membagi pekerjaan agar setiap developer memiliki tanggung jawab yang jelas dan tidak terjadi konflik saat pengembangan.

---

## Developer 1 — Model Layer

### Tanggung Jawab

Membuat:

* Entity
* Model
* Repository Interface
* Data Mapping

### File yang Dikerjakan

```text
models/
entities/
repositories/
```

### Tidak Bertanggung Jawab

* UI
* State Management

---

## Developer 2 — ViewModel Layer

### Tanggung Jawab

Membuat:

* Business Logic
* State Management
* API Call melalui Repository

### File yang Dikerjakan

```text
viewmodels/
providers/
bloc/
```

### Tidak Bertanggung Jawab

* Desain UI
* Database

---

## Developer 3 — View Layer

### Tanggung Jawab

Membuat:

* Halaman
* Widget
* Navigasi
* Responsive Layout

### File yang Dikerjakan

```text
pages/
screens/
widgets/
```

### Tidak Bertanggung Jawab

* Query API
* Logic bisnis

---

## Developer 4 — Testing dan QA

### Tanggung Jawab

Membuat:

* Unit Test
* Widget Test
* Integration Test

### File yang Dikerjakan

```text
test/
integration_test/
```

### Tidak Bertanggung Jawab

* Menambah fitur baru

---

## Diagram Pembagian Tim

```text
Developer 1
   │
   ├─ Model
   ├─ Entity
   └─ Repository

Developer 2
   │
   ├─ ViewModel
   ├─ State Management
   └─ Business Logic

Developer 3
   │
   ├─ UI
   ├─ Widget
   └─ Navigation

Developer 4
   │
   ├─ Unit Test
   ├─ Widget Test
   └─ Integration Test
```

---

# 3. Daftar File untuk Implementasi "Daftar Produk" dengan Clean Architecture + BLoC

## Struktur Folder

```text
lib/
│
├── core/
│
├── features/
│   └── products/
│
│       ├── data/
│       ├── domain/
│       └── presentation/
│
└── injection_container.dart
```

---

## A. Domain Layer

### product.dart

Lokasi:

```text
features/products/domain/entities/product.dart
```

Tanggung Jawab:

* Entity produk

---

### product_repository.dart

Lokasi:

```text
features/products/domain/repositories/product_repository.dart
```

Tanggung Jawab:

* Kontrak repository

---

### get_products.dart

Lokasi:

```text
features/products/domain/usecases/get_products.dart
```

Tanggung Jawab:

* Mengambil daftar produk

---

## B. Data Layer

### product_model.dart

Lokasi:

```text
features/products/data/models/product_model.dart
```

Tanggung Jawab:

* Parsing JSON

---

### product_remote_data_source.dart

Lokasi:

```text
features/products/data/datasources/product_remote_data_source.dart
```

Tanggung Jawab:

* Request API produk

---

### product_local_data_source.dart

Lokasi:

```text
features/products/data/datasources/product_local_data_source.dart
```

Tanggung Jawab:

* Cache produk

---

### product_repository_impl.dart

Lokasi:

```text
features/products/data/repositories/product_repository_impl.dart
```

Tanggung Jawab:

* Implementasi repository

---

## C. Presentation Layer (BLoC)

### product_event.dart

Lokasi:

```text
features/products/presentation/bloc/product_event.dart
```

Tanggung Jawab:

* Event BLoC

Contoh:

```text
LoadProducts
RefreshProducts
```

---

### product_state.dart

Lokasi:

```text
features/products/presentation/bloc/product_state.dart
```

Tanggung Jawab:

* State BLoC

Contoh:

```text
Initial
Loading
Loaded
Error
```

---

### product_bloc.dart

Lokasi:

```text
features/products/presentation/bloc/product_bloc.dart
```

Tanggung Jawab:

* Menghubungkan UI dengan Use Case

---

### product_page.dart

Lokasi:

```text
features/products/presentation/pages/product_page.dart
```

Tanggung Jawab:

* Menampilkan daftar produk

---

### product_card.dart

Lokasi:

```text
features/products/presentation/widgets/product_card.dart
```

Tanggung Jawab:

* Widget item produk

---

## D. Dependency Injection

### injection_container.dart

Lokasi:

```text
lib/injection_container.dart
```

Tanggung Jawab:

* Registrasi dependency

Contoh:

```text
Dio
Repository
UseCase
Bloc
```

---

## Ringkasan File

| File                            | Layer        | Tanggung Jawab          |
| ------------------------------- | ------------ | ----------------------- |
| product.dart                    | Domain       | Entity                  |
| product_repository.dart         | Domain       | Interface Repository    |
| get_products.dart               | Domain       | Use Case                |
| product_model.dart              | Data         | JSON Mapping            |
| product_remote_data_source.dart | Data         | API                     |
| product_local_data_source.dart  | Data         | Cache                   |
| product_repository_impl.dart    | Data         | Implementasi Repository |
| product_event.dart              | Presentation | Event                   |
| product_state.dart              | Presentation | State                   |
| product_bloc.dart               | Presentation | Logic BLoC              |
| product_page.dart               | Presentation | Halaman Produk          |
| product_card.dart               | Presentation | Widget Produk           |
| injection_container.dart        | Core         | Dependency Injection    |

---

# Kesimpulan

Dengan Clean Architecture, fitur Login dan Daftar Produk dipisahkan menjadi Domain, Data, dan Presentation Layer sehingga kode lebih mudah dipelihara, diuji, dan dikembangkan. Pembagian tugas pada proyek MVVM juga menjadi lebih jelas karena setiap developer memiliki batas tanggung jawab yang spesifik sehingga konflik pekerjaan dapat diminimalkan.
