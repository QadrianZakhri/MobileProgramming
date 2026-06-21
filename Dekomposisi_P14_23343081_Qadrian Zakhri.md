# Dekomposisi_P14_23343081_Qadrian Zakhri

# Dekomposisi Topik Keamanan Aplikasi Mobile

---

# 1. Domain Keamanan Aplikasi Mobile

Keamanan aplikasi mobile mencakup berbagai aspek yang saling berkaitan. Untuk memudahkan analisis dan implementasi, topik keamanan dapat dipecah menjadi beberapa domain utama.

---

## Domain 1 – Autentikasi dan Otorisasi

### Ancaman Utama

* Password lemah
* Credential stuffing
* Brute force attack
* Session hijacking
* Unauthorized access

### Mekanisme Mitigasi

* Multi-Factor Authentication (MFA)
* Biometric Authentication
* PIN Lock
* Session Timeout
* JWT Authentication

### Package Flutter yang Relevan

```yaml
local_auth
flutter_secure_storage
jwt_decoder
```

---

## Domain 2 – Keamanan Penyimpanan Data

### Ancaman Utama

* Kebocoran data lokal
* Reverse engineering
* Pencurian token
* Data exposure

### Mekanisme Mitigasi

* Enkripsi database
* Secure Storage
* Data minimization
* Encrypted cache

### Package Flutter yang Relevan

```yaml
flutter_secure_storage
hive
sqflite
```

---

## Domain 3 – Keamanan Komunikasi Jaringan

### Ancaman Utama

* Man-in-the-Middle (MITM)
* Packet sniffing
* SSL stripping
* Data interception

### Mekanisme Mitigasi

* HTTPS/TLS
* Certificate Pinning
* Secure API Gateway
* Token-based authentication

### Package Flutter yang Relevan

```yaml
dio
http
pretty_dio_logger
```

---

## Domain 4 – Keamanan Aplikasi dan Kode

### Ancaman Utama

* Reverse engineering
* Code tampering
* APK modification
* Runtime manipulation

### Mekanisme Mitigasi

* Code obfuscation
* Integrity checking
* App signing
* Runtime protection

### Tools yang Relevan

```text
ProGuard
R8
Flutter Obfuscation
```

---

## Domain 5 – Keamanan Perangkat dan Lingkungan

### Ancaman Utama

* Rooted device
* Jailbroken device
* Emulator attack
* Screen recording
* Malware

### Mekanisme Mitigasi

* Root detection
* Jailbreak detection
* Emulator detection
* Screen security flag

### Package Flutter yang Relevan

```yaml
flutter_jailbreak_detection
safe_device
```

---

## Domain 6 – Monitoring dan Audit Keamanan

### Ancaman Utama

* Aktivitas mencurigakan
* Penyalahgunaan akun
* Fraud
* Login tidak sah

### Mekanisme Mitigasi

* Audit log
* Activity monitoring
* Security analytics
* Alert system

### Tools yang Relevan

```text
Firebase Crashlytics
Firebase Analytics
Sentry
```

---

# Ringkasan Domain

| Domain              | Fokus Utama                     |
| ------------------- | ------------------------------- |
| Autentikasi         | Verifikasi identitas            |
| Penyimpanan Data    | Perlindungan data lokal         |
| Komunikasi Jaringan | Keamanan API dan jaringan       |
| Keamanan Kode       | Perlindungan aplikasi           |
| Keamanan Perangkat  | Perlindungan lingkungan runtime |
| Monitoring          | Deteksi dan audit ancaman       |

---

# 2. Skenario Login ke Aplikasi Perbankan Mobile

## Studi Kasus

Pengguna melakukan login ke aplikasi mobile banking menggunakan PIN dan biometrik.

---

## Langkah 1

Pengguna membuka aplikasi mobile banking.

### Keamanan

* Memeriksa integritas aplikasi
* Memastikan aplikasi belum dimodifikasi

---

## Langkah 2

Aplikasi melakukan pengecekan perangkat.

### Pemeriksaan

* Root detection
* Jailbreak detection
* Emulator detection

Jika perangkat tidak aman:

```text
Akses ditolak
```

---

## Langkah 3

Pengguna memasukkan PIN.

### Keamanan

* PIN tidak disimpan dalam plaintext
* Input dimasking

---

## Langkah 4

PIN divalidasi secara lokal.

### Pemeriksaan

* Panjang PIN
* Format PIN

Jika tidak valid:

```text
PIN tidak sesuai
```

---

## Langkah 5

Data login dienkripsi sebelum dikirim.

### Keamanan

* TLS aktif
* Request menggunakan HTTPS

---

## Langkah 6

Aplikasi mengirim request autentikasi.

Contoh:

```http
POST /api/auth/login
```

Data:

```json
{
  "pin": "encrypted",
  "device_id": "xxxx"
}
```

---

## Langkah 7

Server melakukan verifikasi.

Pemeriksaan:

* PIN
* Device ID
* Status akun
* Percobaan login

---

## Langkah 8

Server mengirim token autentikasi.

Contoh:

```json
{
  "access_token": "xxxxx",
  "refresh_token": "yyyyy"
}
```

---

## Langkah 9

Token disimpan secara aman.

### Penyimpanan

```text
flutter_secure_storage
```

### Keamanan

* Tidak disimpan di SharedPreferences
* Tidak ditampilkan di log

---

## Langkah 10

Aplikasi memuat profil pengguna.

Request:

```http
GET /api/profile
Authorization: Bearer TOKEN
```

---

## Langkah 11

Server memverifikasi token.

Jika valid:

```text
HTTP 200 OK
```

Jika tidak valid:

```text
HTTP 401 Unauthorized
```

---

## Langkah 12

Dashboard ditampilkan.

### Keamanan

* Session aktif
* Token tetap tersimpan aman
* Timeout otomatis disiapkan

---

## Diagram Alur Keamanan Login

```text
User
 ↓
Input PIN
 ↓
Validasi Lokal
 ↓
HTTPS Request
 ↓
Server Authentication
 ↓
Access Token
 ↓
Secure Storage
 ↓
Authorized API Call
 ↓
Dashboard
```

---

# 3. Roadmap Pembelajaran Keamanan Mobile

---

# A. Wajib Dikuasai Segera

Materi dasar yang harus dipahami seluruh Mobile Developer.

### Autentikasi

* PIN Authentication
* Biometric Authentication
* JWT Authentication
* Session Management

### Penyimpanan Data

* Secure Storage
* SharedPreferences
* Hive
* SQLite

### Komunikasi Jaringan

* HTTPS
* TLS
* REST API Security

### Dasar Keamanan

* OWASP Mobile Top 10
* Secure Coding Principles

---

## Checklist

```text
✓ JWT
✓ HTTPS
✓ Secure Storage
✓ Authentication
✓ OWASP Mobile Top 10
```

---

# B. Penting untuk Produksi

Materi yang sering digunakan pada aplikasi nyata.

### API Security

* Refresh Token
* Token Rotation
* OAuth 2.0

### Application Security

* Obfuscation
* App Signing
* Integrity Check

### Monitoring

* Crashlytics
* Security Logging
* Fraud Detection

### Device Security

* Root Detection
* Jailbreak Detection
* Emulator Detection

---

## Checklist

```text
✓ OAuth 2.0
✓ Root Detection
✓ SSL Pinning
✓ Crash Monitoring
✓ Obfuscation
```

---

# C. Lanjutan untuk Spesialis Keamanan

Materi tingkat lanjut untuk Security Engineer.

### Advanced Mobile Security

* SSL Pinning
* Certificate Pinning
* Runtime Protection
* Anti-Tampering

### Reverse Engineering

* APK Analysis
* Binary Analysis
* Static Analysis
* Dynamic Analysis

### Cryptography

* AES
* RSA
* ECC
* Key Management

### Security Testing

* Penetration Testing
* Mobile App Pentest
* Threat Modeling
* Vulnerability Assessment

### Compliance

* ISO 27001
* GDPR
* PCI DSS
* UU PDP Indonesia

---

## Checklist

```text
✓ Penetration Testing
✓ Threat Modeling
✓ Cryptography
✓ Reverse Engineering
✓ Compliance
```

---

# Kesimpulan

Keamanan aplikasi mobile tidak hanya berkaitan dengan login dan password, tetapi juga mencakup keamanan penyimpanan data, komunikasi jaringan, kode aplikasi, perangkat pengguna, dan proses monitoring. Untuk menjadi Mobile Developer profesional, penguasaan Secure Storage, HTTPS, JWT Authentication, dan OWASP Mobile Top 10 merupakan prioritas utama sebelum mempelajari topik lanjutan seperti SSL Pinning, Cryptography, dan Penetration Testing.
