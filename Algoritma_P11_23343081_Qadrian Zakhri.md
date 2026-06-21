# Algoritma_P11_23343081_Qadrian Zakhri

## Praktikum Integrasi API dan Repository Pattern Flutter

---

# 1. Flowchart Alur Checkout Aplikasi E-Commerce

## Deskripsi Alur

Proses checkout pada aplikasi e-commerce melibatkan beberapa titik keputusan penting seperti autentikasi pengguna, koneksi jaringan, validasi stok, proses pembayaran, dan refresh token apabila token akses telah kadaluarsa.

### Flowchart
<img width="1021" height="501" alt="image" src="https://github.com/user-attachments/assets/b748fa04-0b50-4e28-b055-ca41eae26899" />

```

---

# 2. Algoritma Penanganan Error

## A. Tidak Ada Koneksi Internet

### Kondisi

Aplikasi tidak dapat terhubung ke jaringan internet.

### Algoritma

1. Periksa status koneksi.
2. Jika koneksi tidak tersedia:

   * Batalkan request API.
   * Simpan request bila diperlukan.
   * Tampilkan pesan error.
3. Tunggu pengguna mencoba kembali.

### Tindakan Aplikasi

* Tidak mengirim request ke server.
* Menampilkan tombol "Coba Lagi".

### Tampilan UI

```text
⚠ Tidak ada koneksi internet

Periksa jaringan Anda lalu coba kembali.

[ Coba Lagi ]
```

---

## B. Server Error (HTTP 500)

### Kondisi

Server mengalami gangguan internal.

### Algoritma

1. Request dikirim.
2. Server mengembalikan status 500.
3. Catat log error.
4. Tampilkan pesan umum kepada pengguna.
5. Berikan opsi retry.

### Tindakan Aplikasi

* Menghentikan proses.
* Menampilkan dialog error.

### Tampilan UI

```text
⚠ Server sedang mengalami gangguan

Silakan coba beberapa saat lagi.

[ Coba Lagi ]
```

---

## C. Request Timeout

### Kondisi

Server tidak memberikan respons dalam batas waktu tertentu.

### Algoritma

1. Kirim request.
2. Mulai timer timeout.
3. Jika melebihi batas:

   * Batalkan request.
   * Tampilkan pesan timeout.
4. Berikan opsi retry.

### Tindakan Aplikasi

* Retry manual atau otomatis.
* Menampilkan loading yang dihentikan.

### Tampilan UI

```text
⌛ Permintaan melebihi batas waktu

Periksa koneksi Anda.

[ Coba Lagi ]
```

---

## D. Token Kadaluarsa (HTTP 401)

### Kondisi

Access token tidak valid atau telah habis masa berlaku.

### Algoritma

1. Request dikirim.
2. Server mengembalikan HTTP 401.
3. Ambil refresh token.
4. Kirim request refresh token.
5. Jika berhasil:

   * Simpan token baru.
   * Ulangi request sebelumnya.
6. Jika gagal:

   * Hapus data login.
   * Arahkan ke halaman login.

### Tindakan Aplikasi

* Refresh token otomatis.
* Login ulang jika refresh gagal.

### Tampilan UI

```text
Sesi Anda telah berakhir.

Silakan login kembali.

[ Login ]
```

---

## E. Format Data Tidak Sesuai

### Kondisi

JSON dari server tidak sesuai dengan model aplikasi.

### Algoritma

1. Terima response API.
2. Parsing JSON.
3. Jika parsing gagal:

   * Catat error.
   * Gunakan data default bila memungkinkan.
   * Tampilkan pesan kesalahan.

### Tindakan Aplikasi

* Menghindari crash aplikasi.
* Menampilkan fallback UI.

### Tampilan UI

```text
⚠ Data tidak dapat diproses

Silakan coba beberapa saat lagi.
```

---

# 3. Langkah Implementasi Repository Pattern

Repository Pattern digunakan untuk memisahkan logika bisnis dari sumber data.

---

## Langkah 1: Membuat Abstract Repository

Tujuan:
Mendefinisikan kontrak fungsi yang harus diimplementasikan.

```dart
abstract class ProductRepository {
  Future<List<Product>> getProducts();
}
```

---

## Langkah 2: Membuat Remote Data Source

Tujuan:
Mengambil data dari API.

```dart
class ProductRemoteDataSource {

  final Dio dio;

  ProductRemoteDataSource(this.dio);

  Future<List<ProductModel>> getProducts() async {
    final response = await dio.get('/products');
    return ProductModel.fromList(response.data);
  }
}
```

---

## Langkah 3: Membuat Local Data Source

Tujuan:
Mengambil atau menyimpan data lokal.

```dart
class ProductLocalDataSource {

  Future<void> saveProducts(
      List<ProductModel> products) async {
  }

  Future<List<ProductModel>> getProducts() async {
    return [];
  }
}
```

---

## Langkah 4: Membuat Repository Implementation

Tujuan:
Menggabungkan Remote dan Local Data Source.

```dart
class ProductRepositoryImpl
    implements ProductRepository {

  final ProductRemoteDataSource remote;
  final ProductLocalDataSource local;

  ProductRepositoryImpl(
    this.remote,
    this.local,
  );

  @override
  Future<List<Product>> getProducts() async {

    try {

      final products =
          await remote.getProducts();

      await local.saveProducts(products);

      return products;

    } catch (_) {

      return await local.getProducts();
    }
  }
}
```

---

## Langkah 5: Menambahkan Dependency Injection

Tujuan:
Mengelola objek secara terpusat.

Contoh menggunakan GetIt:

```dart
final sl = GetIt.instance;

void setup() {

  sl.registerLazySingleton(
    () => Dio(),
  );

  sl.registerLazySingleton(
    () => ProductRemoteDataSource(
      sl(),
    ),
  );

  sl.registerLazySingleton(
    () => ProductLocalDataSource(),
  );

  sl.registerLazySingleton<ProductRepository>(
    () => ProductRepositoryImpl(
      sl(),
      sl(),
    ),
  );
}
```

---

## Langkah 6: Menghubungkan ke UI Layer

Tujuan:
Menampilkan data ke pengguna.

```dart
class ProductPage extends StatelessWidget {

  final repository =
      sl<ProductRepository>();

  @override
  Widget build(BuildContext context) {

    return FutureBuilder(

      future: repository.getProducts(),

      builder: (context, snapshot) {

        if (!snapshot.hasData) {
          return CircularProgressIndicator();
        }

        return ListView.builder(
          itemCount: snapshot.data!.length,
          itemBuilder: (_, index) {
            return Text(
              snapshot.data![index].name,
            );
          },
        );
      },
    );
  }
}
```

---

# Kesimpulan

Repository Pattern memisahkan akses data dari tampilan aplikasi sehingga kode menjadi lebih terstruktur, mudah diuji, dan mudah dikembangkan. Penanganan error yang baik serta alur checkout yang jelas sangat penting untuk menjaga pengalaman pengguna pada aplikasi e-commerce modern.
