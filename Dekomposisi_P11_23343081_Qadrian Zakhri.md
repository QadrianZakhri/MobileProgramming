# MobileProgramming

# Dokumentasi Integrasi API pada Aplikasi Flutter

## 1. Arsitektur Integrasi API (Minimal 5 Lapisan)

Agar aplikasi Flutter mudah dikembangkan dan dipelihara, sistem integrasi API sebaiknya dipisahkan menjadi beberapa lapisan (layer).

| Lapisan                        | Tanggung Jawab                                                          | Package Flutter                                        |
| ------------------------------ | ----------------------------------------------------------------------- | ------------------------------------------------------ |
| **1. Presentation Layer (UI)** | Menampilkan halaman, menerima input pengguna, menampilkan data dari API | `flutter`, `material.dart`                             |
| **2. State Management Layer**  | Mengelola state aplikasi dan menghubungkan UI dengan data               | `provider`, `flutter_bloc`, `riverpod`                 |
| **3. Repository Layer**        | Menjadi penghubung antara state management dan sumber data              | Tidak wajib package khusus                             |
| **4. API Service Layer**       | Mengirim request HTTP dan menerima response API                         | `http`, `dio`, `retrofit`                              |
| **5. Model/Data Layer**        | Menyimpan struktur data hasil parsing JSON                              | `json_annotation`, `json_serializable`                 |
| **6. Local Storage Layer**     | Menyimpan token, cache, dan data pengguna secara lokal                  | `shared_preferences`, `flutter_secure_storage`, `hive` |

### Diagram Arsitektur

```text
User
 ↓
Presentation Layer (UI)
 ↓
State Management Layer
 ↓
Repository Layer
 ↓
API Service Layer
 ↓
Backend API Server

             ↑
      Model/Data Layer

             ↑
      Local Storage Layer
```

---

## 2. Skenario Login Aplikasi Banking (Minimal 10 Langkah)

### Studi Kasus

Pengguna login ke aplikasi mobile banking menggunakan email dan password.

### Langkah-Langkah Teknis

### 1. Pengguna Membuka Halaman Login

Komponen Flutter:

```dart
TextField
ElevatedButton
Scaffold
```

UI menampilkan form email dan password.

---

### 2. Pengguna Mengisi Data Login

Komponen:

```dart
TextEditingController
Form
TextFormField
```

Data email dan password disimpan sementara di controller.

---

### 3. Pengguna Menekan Tombol Login

Komponen:

```dart
ElevatedButton
```

Event:

```dart
onPressed()
```

dipanggil.

---

### 4. Validasi Input

Flutter memeriksa:

* Email tidak kosong
* Password tidak kosong
* Format email valid

Contoh:

```dart
formKey.currentState!.validate();
```

---

### 5. State Management Mengubah Status Menjadi Loading

Contoh:

```dart
AuthProvider.login();
```

atau

```dart
AuthBloc.add(LoginEvent());
```

UI menampilkan:

```dart
CircularProgressIndicator()
```

---

### 6. Repository Menerima Request Login

Repository memanggil service:

```dart
authRepository.login(
 email,
 password
);
```

Tugas repository:

* Memisahkan UI dari API
* Mengelola sumber data

---

### 7. API Service Mengirim Request ke Backend

Package:

```yaml
dio
```

atau

```yaml
http
```

Request:

```http
POST /api/login
```

Body:

```json
{
  "email": "user@mail.com",
  "password": "123456"
}
```

---

### 8. Backend Memproses Login

Server melakukan:

* Verifikasi email
* Verifikasi password
* Pembuatan JWT Token

Response:

```json
{
  "success": true,
  "token": "eyJhbGci...",
  "user": {
    "id": 1,
    "name": "Andi"
  }
}
```

---

### 9. Model Melakukan Parsing JSON

Contoh model:

```dart
UserModel.fromJson(json)
```

Hasil JSON diubah menjadi objek Dart.

---

### 10. Token Disimpan ke Local Storage

Package:

```yaml
flutter_secure_storage
```

Contoh:

```dart
await storage.write(
 key: "token",
 value: token
);
```

Tujuan:

* Menjaga sesi login
* Menghindari login ulang

---

### 11. Data User Disimpan ke State Management

Contoh:

```dart
authProvider.user = user;
```

atau

```dart
emit(LoginSuccess(user));
```

---

### 12. Navigasi ke Dashboard

Komponen Flutter:

```dart
Navigator.pushReplacement()
```

atau

```dart
GoRouter
```

Contoh:

```dart
Navigator.pushReplacement(
 context,
 MaterialPageRoute(
  builder: (_) => DashboardPage(),
 ),
);
```

---

### 13. Dashboard Meminta Data Awal

Dashboard memanggil API:

```http
GET /api/account
```

Header:

```http
Authorization: Bearer TOKEN
```

---

### 14. Dashboard Berhasil Ditampilkan

Komponen:

```dart
FutureBuilder
ListView
Card
```

Data rekening, saldo, dan transaksi ditampilkan kepada pengguna.

---

## Ringkasan Alur Login

```text
User
 ↓
Login Screen
 ↓
Validation
 ↓
State Management
 ↓
Repository
 ↓
API Service
 ↓
Backend API
 ↓
JSON Response
 ↓
Model Parsing
 ↓
Secure Storage
 ↓
State Update
 ↓
Dashboard
```

---

# 3. Roadmap Pembelajaran Integrasi API Flutter

## Tingkat Dasar (Basic)

### Konsep Dasar

* Apa itu API
* REST API
* HTTP Request dan Response
* Status Code HTTP
* JSON dan Parsing JSON
* GET Request
* POST Request
* Menggunakan package `http`
* Future dan Async Await
* FutureBuilder

### Praktik Dasar

* Mengambil daftar data dari API
* Menampilkan data ke ListView
* Mengirim data form ke API

---

## Tingkat Menengah (Intermediate)

### Arsitektur

* Repository Pattern
* Service Layer
* MVVM
* Clean Architecture

### State Management

* Provider
* Riverpod
* Bloc/Cubit

### Integrasi API

* CRUD API
* PUT Request
* PATCH Request
* DELETE Request
* Upload File
* Multipart Request
* Pagination
* Search API
* Debounce Search

### Local Storage

* Shared Preferences
* Hive
* Secure Storage

### Error Handling

* Timeout
* Retry Mechanism
* Exception Handling
* Offline Handling

---

## Tingkat Lanjutan (Advanced)

### Authentication & Security

* JWT Authentication
* Refresh Token
* OAuth 2.0
* Biometric Login
* Secure Storage Encryption

### Advanced Networking

* Dio Interceptor
* Request Queue
* Caching
* API Rate Limiting
* SSL Pinning

### Real-Time Communication

* WebSocket
* Socket.IO
* Firebase Realtime Database
* Server Sent Events (SSE)

### Enterprise Architecture

* Clean Architecture
* Dependency Injection
* GetIt
* Injectable
* Modular Architecture

### Performance Optimization

* API Caching Strategy
* Lazy Loading
* Infinite Scrolling
* Background Sync
* Offline First Architecture

### Testing

* Unit Test API
* Repository Test
* Mock API
* Integration Test
* Widget Test

---

## Kesimpulan

Integrasi API pada Flutter idealnya menggunakan arsitektur berlapis yang terdiri dari **Presentation, State Management, Repository, API Service, Model, dan Local Storage**. Pada skenario login aplikasi banking, proses berlangsung melalui validasi input, komunikasi API, penyimpanan token secara aman, hingga navigasi ke dashboard. Untuk menguasai integrasi API secara profesional, pembelajaran dapat dilakukan bertahap mulai dari **Dasar → Menengah → Lanjutan** dengan fokus pada keamanan, performa, dan arsitektur aplikasi.
