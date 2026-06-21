# Dekomposisi_P15_23343081_Qadrian Zakhri

# Analisis Tren AI On-Device dan Roadmap Menjadi Mobile AI Developer

---

# 1. Dekomposisi Tren AI On-Device

## A. Definisi dan Sejarah Singkat

### Definisi

AI On-Device adalah teknologi kecerdasan buatan yang berjalan langsung pada perangkat pengguna (smartphone, tablet, laptop, atau perangkat IoT) tanpa harus selalu mengirim data ke server cloud.

Pada pendekatan ini, proses inferensi model AI dilakukan secara lokal di perangkat.

---

### Sejarah Singkat

#### 2010–2015

AI masih bergantung pada cloud karena keterbatasan hardware perangkat mobile.

#### 2016–2020

Muncul TensorFlow Lite dan Core ML yang memungkinkan model AI berjalan di smartphone.

#### 2021–2024

Perangkat mulai memiliki AI Accelerator:

* Apple Neural Engine
* Google Tensor Processor
* Qualcomm AI Engine

#### 2025–Sekarang

Muncul tren AI generatif yang berjalan langsung di perangkat:

* Gemini Nano
* Apple Intelligence
* Microsoft Copilot On Device

---

## B. Teknologi Pemungkin

### Hardware

* NPU (Neural Processing Unit)
* GPU Mobile
* AI Accelerator Chip

Contoh:

* Snapdragon AI Engine
* Apple Neural Engine
* MediaTek APU

---

### Software

#### TensorFlow Lite

Model AI ringan untuk perangkat mobile.

#### ONNX Runtime

Framework AI lintas platform.

#### MediaPipe

Framework AI untuk vision dan gesture.

#### Firebase ML

Layanan machine learning mobile.

---

### Flutter Package

```yaml
tflite_flutter
google_mlkit_text_recognition
google_mlkit_face_detection
camera
image
```

---

## C. Contoh Implementasi Nyata

### Google Lens

Fitur:

* OCR
* Penerjemah kamera
* Pencarian objek

---

### Apple Intelligence

Fitur:

* Ringkasan teks
* Penulisan otomatis
* AI Assistant

---

### Samsung Galaxy AI

Fitur:

* Live Translation
* Circle to Search
* AI Editing

---

### Mobile Banking

Fitur:

* Face Recognition
* Fraud Detection
* Document Verification

---

## D. Peluang untuk Developer Flutter

### Bidang Pendidikan

Contoh aplikasi:

* Pengenal tulisan tangan
* Penerjemah otomatis
* Asisten belajar

---

### Bidang Kesehatan

Contoh aplikasi:

* Deteksi makanan
* Monitoring kesehatan
* Analisis citra sederhana

---

### Bidang Bisnis

Contoh aplikasi:

* Scanner invoice
* OCR dokumen
* AI Customer Service

---

### Bidang Smart City

Contoh aplikasi:

* Smart Parking
* Monitoring kendaraan
* Traffic Recognition

---

## E. Tantangan Adopsi di Indonesia

### Infrastruktur Perangkat

Tidak semua pengguna memiliki smartphone dengan NPU modern.

---

### Kualitas Dataset Lokal

Masih terbatas untuk:

* Bahasa daerah
* Dokumen lokal
* Tulisan tangan Indonesia

---

### SDM AI Mobile

Jumlah developer yang menguasai:

* Flutter
* Machine Learning
* Mobile Deployment

masih relatif sedikit.

---

### Regulasi Data

Harus memperhatikan:

* UU PDP
* Privasi pengguna
* Persetujuan penggunaan data

---

## F. Proyeksi 3–5 Tahun ke Depan

### Tahun 2026–2027

AI menjadi fitur standar pada aplikasi mobile.

---

### Tahun 2027–2028

Lebih banyak model berjalan langsung di perangkat.

---

### Tahun 2028–2030

Mayoritas aplikasi produktivitas memiliki AI Assistant lokal.

---

### Dampak untuk Flutter Developer

Skill berikut menjadi sangat bernilai:

* Flutter
* TensorFlow Lite
* Computer Vision
* AI Deployment

---

# 2. Roadmap Menjadi Mobile AI Developer

## Tahap 1 – Fondasi Flutter

### Keahlian Dasar yang Harus Dimiliki

* Widget Flutter
* State Management
* REST API
* Local Storage
* Firebase
* Clean Architecture

---

### Checklist

```text
✓ Widget
✓ Navigation
✓ API Integration
✓ Provider/BLoC
✓ SQLite/Hive
✓ Firebase
```

---

## Tahap 2 – Belajar Package AI

### Computer Vision

```yaml
google_mlkit_face_detection
google_mlkit_object_detection
google_mlkit_text_recognition
```

---

### TensorFlow Lite

```yaml
tflite_flutter
tflite_flutter_helper
```

---

### Kamera dan Gambar

```yaml
camera
image_picker
image
```

---

## Tahap 3 – Proyek Latihan

### Proyek 1

OCR Scanner

Fitur:

* Scan dokumen
* Ekstraksi teks

---

### Proyek 2

Face Detection

Fitur:

* Deteksi wajah
* Bounding box

---

### Proyek 3

Food Recognition

Fitur:

* Identifikasi makanan
* Prediksi kalori

---

### Proyek 4

AI Chat Mobile

Fitur:

* Chatbot AI
* Integrasi model lokal

---

### Proyek 5

Smart Attendance

Fitur:

* Face Recognition
* Rekap absensi

---

## Tahap 4 – Membangun Portofolio

Portofolio ideal berisi:

### Minimal 3–5 Proyek

Contoh:

1. OCR Scanner
2. Face Detection App
3. AI Translator
4. Smart Attendance
5. AI Chat Assistant

---

### GitHub

Berisi:

* Source Code
* README
* Dokumentasi

---

### Video Demo

Upload ke:

* YouTube
* LinkedIn
* Portfolio Website

---

## Tahap 5 – Target Karier di Indonesia

### Startup Teknologi

GoTo

Bidang:

* Transportasi
* Pembayaran
* AI Recommendation

---

### E-Commerce

Tokopedia

Bukalapak

---

### Perbankan Digital

Bank Jago

Bank Neo Commerce

---

### Teknologi Pendidikan

Ruangguru

---

### Konsultan Teknologi

Accenture

---

# 3. Sub-Topik untuk Menguasai AI On-Device

## A. Fondasi (Wajib Sekarang)

### Flutter

* Widget
* Navigation
* State Management
* API Integration

### Dasar AI

* Machine Learning Basics
* Dataset
* Model Training
* Inference

### Dasar Mobile AI

* TensorFlow Lite
* ML Kit
* Camera Integration

---

## Checklist Fondasi

```text
✓ Flutter Dasar
✓ REST API
✓ State Management
✓ TensorFlow Lite
✓ ML Kit
✓ Camera
```

---

## B. Menengah (3 Bulan ke Depan)

### Computer Vision

* OCR
* Face Detection
* Object Detection

### AI Deployment

* Model Conversion
* Quantization
* Model Optimization

### Mobile Performance

* Memory Optimization
* Battery Optimization
* Background Processing

---

## Checklist Menengah

```text
✓ OCR
✓ Face Detection
✓ Object Detection
✓ Model Optimization
✓ Performance Tuning
```

---

## C. Lanjutan (6–12 Bulan ke Depan)

### Advanced AI

* LLM Mobile
* Edge AI
* Generative AI

### Security

* AI Privacy
* Secure Inference
* Data Protection

### Research

* Federated Learning
* TinyML
* Multimodal AI

---

## Checklist Lanjutan

```text
✓ Edge AI
✓ Generative AI
✓ TinyML
✓ Federated Learning
✓ AI Security
```

---

# Kesimpulan

AI On-Device merupakan salah satu tren teknologi paling menjanjikan dalam pengembangan aplikasi mobile. Dengan menguasai Flutter, TensorFlow Lite, ML Kit, dan Computer Vision, seorang developer dapat membangun aplikasi cerdas yang berjalan langsung di perangkat pengguna tanpa bergantung penuh pada cloud. Dalam 3–5 tahun ke depan, kebutuhan Mobile AI Developer diperkirakan meningkat seiring berkembangnya perangkat yang memiliki kemampuan AI bawaan dan meningkatnya permintaan aplikasi berbasis kecerdasan buatan.
