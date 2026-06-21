# Algoritma_P14_23343081_Qadrian Zakhri

# Checklist Keamanan Pra-Rilis, Alur Penanganan Token Aman, dan Prosedur Security Audit Flutter

---

# 1. Checklist Keamanan Pra-Rilis Aplikasi Flutter

## Tujuan

Memastikan aplikasi Flutter memenuhi standar keamanan minimum sebelum dirilis ke pengguna.

---

# A. Audit Dependencies

## Pemeriksaan

Periksa seluruh package yang digunakan pada file:

```text
pubspec.yaml
```

## Langkah Verifikasi

### 1. Cek package usang

```bash
flutter pub outdated
```

### 2. Periksa kerentanan package

* Tinjau halaman package di pub.dev
* Periksa issue keamanan pada GitHub package

### 3. Hapus package yang tidak digunakan

```bash
dart pub deps
```

## Checklist

| Item                                  | Status |
| ------------------------------------- | ------ |
| Tidak ada package deprecated          | □      |
| Semua package diperbarui              | □      |
| Tidak ada dependency mencurigakan     | □      |
| Package tidak digunakan telah dihapus | □      |

---

# B. Pemeriksaan AndroidManifest.xml

## Lokasi

```text
android/app/src/main/AndroidManifest.xml
```

## Langkah Verifikasi

### Periksa Internet Permission

```xml
<uses-permission
    android:name="android.permission.INTERNET"/>
```

Pastikan hanya permission yang diperlukan yang digunakan.

---

### Periksa Debuggable

Pastikan:

```xml
android:debuggable="false"
```

untuk versi release.

---

### Periksa Backup Data

Disarankan:

```xml
android:allowBackup="false"
```

untuk aplikasi sensitif.

---

## Checklist

| Item                            | Status |
| ------------------------------- | ------ |
| Tidak ada permission berlebihan | □      |
| allowBackup diperiksa           | □      |
| debuggable=false                | □      |

---

# C. Pemeriksaan Info.plist (iOS)

## Lokasi

```text
ios/Runner/Info.plist
```

## Langkah Verifikasi

### Periksa App Transport Security

Pastikan HTTPS digunakan.

Hindari:

```xml
NSAllowsArbitraryLoads = true
```

---

### Periksa Permission

* Kamera
* Lokasi
* Mikrofon

Pastikan semuanya memiliki alasan penggunaan yang jelas.

---

## Checklist

| Item                        | Status |
| --------------------------- | ------ |
| HTTPS diwajibkan            | □      |
| Permission sesuai kebutuhan | □      |
| Tidak ada ATS bypass        | □      |

---

# D. Verifikasi Secure Storage

## Pemeriksaan

Pastikan data sensitif tidak disimpan di:

```text
SharedPreferences
```

---

### Data yang Wajib di Secure Storage

* Access Token
* Refresh Token
* Session ID

---

### Verifikasi

Cari kode:

```dart
SharedPreferences.getInstance()
```

Pastikan tidak digunakan untuk token.

---

### Contoh Aman

```dart
final storage =
    FlutterSecureStorage();

await storage.write(
  key: 'token',
  value: token,
);
```

---

## Checklist

| Item                                 | Status |
| ------------------------------------ | ------ |
| Token tidak ada di SharedPreferences | □      |
| Secure Storage digunakan             | □      |
| Password tidak disimpan lokal        | □      |

---

# E. Pengujian Certificate Pinning

## Tujuan

Mencegah serangan MITM (Man-In-The-Middle).

---

## Langkah Verifikasi

### 1. Jalankan aplikasi

### 2. Gunakan proxy

Contoh:

```text
Burp Suite
Charles Proxy
```

### 3. Intersep trafik

Expected:

```text
Koneksi ditolak
```

jika sertifikat tidak cocok.

---

## Checklist

| Item                         | Status |
| ---------------------------- | ------ |
| SSL Pinning aktif            | □      |
| Trafik tidak bisa diintersep | □      |
| Sertifikat diverifikasi      | □      |

---

# F. Aktivasi Code Obfuscation

## Android

Build:

```bash
flutter build apk \
--release \
--obfuscate \
--split-debug-info=debug-info/
```

---

## iOS

Build:

```bash
flutter build ios \
--release \
--obfuscate \
--split-debug-info=debug-info/
```

---

## Verifikasi

Buka APK hasil build.

Pastikan:

```text
Nama class tidak terbaca jelas
```

---

## Checklist

| Item                            | Status |
| ------------------------------- | ------ |
| Obfuscation aktif               | □      |
| Debug info tersimpan            | □      |
| Reverse engineering lebih sulit | □      |

---

# Ringkasan Checklist Pra-Rilis

```text
□ Audit Dependencies
□ AndroidManifest Review
□ Info.plist Review
□ Secure Storage Review
□ Certificate Pinning Test
□ Code Obfuscation
□ Release Build Verification
```

---

# 2. Flowchart Penanganan Token yang Aman

## Diagram Alur

<img width="996" height="500" alt="image" src="https://github.com/user-attachments/assets/d6e4b998-face-4c95-be20-c3d738173627" />


---

## Ringkasan Alur

```text
Login
 ↓
Secure Storage
 ↓
API Request
 ↓
401?
 ↓
Refresh Token
 ↓
Success → Retry Request
 ↓
Fail → Login Screen
```

---

# 3. Prosedur Security Audit Kode Flutter

## Tujuan

Melakukan review keamanan dasar terhadap proyek Flutter milik rekan tim.

---

# Tahap 1 – Memahami Struktur Proyek

## Aktivitas

Periksa:

```text
lib/
android/
ios/
pubspec.yaml
```

### Tujuan

Mengetahui:

* Arsitektur proyek
* Dependency yang digunakan
* Mekanisme penyimpanan data

---

# Tahap 2 – Identifikasi Data Sensitif

Cari:

```text
token
password
email
phone
apiKey
secret
```

Gunakan pencarian global VS Code:

```text
Ctrl + Shift + F
```

---

## Temuan yang Dicari

### Tidak Aman

```dart
prefs.setString(
 "token",
 token
);
```

### Aman

```dart
secureStorage.write(
 key: "token",
 value: token,
);
```

---

# Tahap 3 – Cari Anti-Pattern Keamanan

## Anti-Pattern 1

Hardcoded API Key

```dart
const apiKey =
"123456789";
```

---

## Anti-Pattern 2

HTTP Tanpa TLS

```text
http://api.example.com
```

---

## Anti-Pattern 3

Token di SharedPreferences

```dart
prefs.setString(...)
```

---

## Anti-Pattern 4

Logging Data Sensitif

```dart
print(token);
print(password);
```

---

## Anti-Pattern 5

Certificate Validation Dimatikan

```dart
badCertificateCallback =
    (cert, host, port) => true;
```

---

# Tahap 4 – Menilai Tingkat Risiko

## Klasifikasi

### Tinggi

* Token bocor
* Password tersimpan plaintext
* Certificate validation dinonaktifkan

### Sedang

* Logging berlebihan
* Permission tidak perlu

### Rendah

* Naming kurang jelas
* Dokumentasi kurang

---

# Tahap 5 – Memberikan Komentar Konstruktif

## Contoh Komentar Baik

```text
Saat ini access token disimpan
menggunakan SharedPreferences.

Disarankan menggunakan
flutter_secure_storage agar token
terlindungi oleh Android Keystore
dan iOS Keychain.
```

---

## Hindari

```text
Kode ini salah.
Keamanannya buruk.
```

---

# Tahap 6 – Dokumentasi Temuan Audit

## Format Laporan

| No | Temuan                     | Risiko | Rekomendasi          |
| -- | -------------------------- | ------ | -------------------- |
| 1  | Token di SharedPreferences | Tinggi | Secure Storage       |
| 2  | HTTP tanpa TLS             | Tinggi | HTTPS                |
| 3  | API Key Hardcoded          | Sedang | Environment Variable |

---

## Ringkasan Audit

```text
Tanggal Audit : xx/xx/xxxx
Reviewer      : Nama Mahasiswa
Project       : Nama Aplikasi

Temuan Tinggi : 2
Temuan Sedang : 1
Temuan Rendah : 3

Rekomendasi:
- Gunakan Secure Storage
- Aktifkan SSL Pinning
- Hapus hardcoded secret
```

---

# Kesimpulan

Keamanan aplikasi Flutter harus diperiksa sebelum rilis melalui audit dependency, review konfigurasi Android dan iOS, verifikasi penyimpanan data sensitif, pengujian certificate pinning, dan aktivasi code obfuscation. Selain itu, pengelolaan token harus mengikuti alur yang aman dengan dukungan refresh token dan secure storage. Security audit sederhana dapat dilakukan dengan mengidentifikasi data sensitif, mencari anti-pattern keamanan, serta mendokumentasikan temuan secara profesional dan konstruktif.
