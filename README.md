
# GetX Flutter - 3 Pilar Utama

GetX adalah framework populer di Flutter yang menyediakan solusi sederhana, cepat, ringan, dan powerful untuk membangun aplikasi modern dengan kode yang lebih bersih serta minim boilerplate. GetX banyak digunakan karena menggabungkan tiga kebutuhan utama aplikasi ke dalam satu package.

## Tiga Pilar Utama GetX

1. State Management  
2. Route Management  
3. Dependency Management  

Dengan tiga fitur ini, developer dapat membuat aplikasi yang scalable, rapi, mudah di-maintain, dan cepat dikembangkan.

---

# Daftar Isi

- [GetX Flutter - 3 Pilar Utama](#getx-flutter---3-pilar-utama)
- [1. State Management](#1-state-management)
- [2. Route Management](#2-route-management)
- [3. Dependency Management](#3-dependency-management)
- [Perbandingan 3 Pilar](#perbandingan-3-pilar)
- [Struktur Project GetX](#struktur-project-getx)
- [Setup Awal](#setup-awal)
- [Best Practice](#best-practice)
- [Kesimpulan](#kesimpulan)

---

# 1. State Management

## Apa Itu State Management?

State Management adalah cara mengatur perubahan data pada aplikasi agar tampilan UI otomatis ikut berubah ketika data berubah.

Contoh kasus:

- Counter bertambah
- Loading indicator
- Data API tampil
- Cart update
- Form validation
- Search realtime
- Toggle dark mode

GetX menyediakan state management yang ringan, cepat, dan mudah digunakan.

---

## Jenis State Management di GetX

### 1. Reactive State Management

Menggunakan:

- `.obs`
- `Obx()`

Jika nilai berubah, UI akan otomatis rebuild.

### Contoh Controller

```dart
import 'package:get/get.dart';

class HomeController extends GetxController {
  var count = 0.obs;

  void increment() {
    count++;
  }
}


### Contoh UI

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

class HomePage extends StatelessWidget {
  final controller = Get.put(HomeController());

  HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Reactive State"),
      ),
      body: Center(
        child: Obx(() => Text(
              '${controller.count}',
              style: const TextStyle(fontSize: 30),
            )),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: controller.increment,
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

---

### 2. Simple State Management

Menggunakan:

* `GetBuilder()`
* `update()`

### Controller

```dart
class HomeController extends GetxController {
  int count = 0;

  void increment() {
    count++;
    update();
  }
}
```

### UI

```dart
GetBuilder<HomeController>(
  init: HomeController(),
  builder: (controller) {
    return Text(
      '${controller.count}',
      style: TextStyle(fontSize: 30),
    );
  },
)
```

---

## Worker pada GetX

Worker digunakan untuk memantau perubahan state dan menjalankan aksi tertentu.

### ever()

Dipanggil setiap data berubah.

```dart
ever(count, (_) {
  print("Count berubah");
});
```

### once()

Dipanggil sekali saat pertama berubah.

```dart
once(count, (_) {
  print("Pertama kali berubah");
});
```

### debounce()

Menunggu user berhenti mengetik.

```dart
debounce(search, (_) {
  searchApi();
}, time: Duration(milliseconds: 500));
```

### interval()

Membatasi trigger per interval waktu.

```dart
interval(buttonTap, (_) {
  sendRequest();
}, time: Duration(seconds: 1));
```

---

## Kelebihan State Management GetX

* Ringan
* Cepat
* Tidak perlu setState berlebihan
* Reaktif
* Kode lebih bersih
* Cocok untuk project kecil maupun besar

---

# 2. Route Management

## Apa Itu Route Management?

Route Management adalah sistem navigasi antar halaman tanpa perlu `BuildContext`.

---

## Tanpa GetX

```dart
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (_) => HomePage(),
  ),
);
```

---

## Dengan GetX

```dart
Get.to(HomePage());
```

Lebih singkat, bersih, dan praktis.

---

## Navigasi Dasar

### Pindah Halaman

```dart
Get.to(ProfilePage());
```

### Ganti Halaman

Halaman lama dihapus dari stack.

```dart
Get.off(LoginPage());
```

### Hapus Semua Halaman

```dart
Get.offAll(HomePage());
```

### Kembali ke Halaman Sebelumnya

```dart
Get.back();
```

---

## Named Route

### Setup Route

```dart
GetMaterialApp(
  initialRoute: '/',
  getPages: [
    GetPage(
      name: '/',
      page: () => HomePage(),
    ),
    GetPage(
      name: '/login',
      page: () => LoginPage(),
    ),
    GetPage(
      name: '/profile',
      page: () => ProfilePage(),
    ),
  ],
);
```

### Navigasi dengan Nama Route

```dart
Get.toNamed('/profile');
```

---

## Kirim Data Antar Halaman

### Mengirim Data

```dart
Get.to(ProfilePage(), arguments: "Aqshal");
```

### Mengambil Data

```dart
String nama = Get.arguments;
```

---

## Dynamic Route Parameter

### Setup

```dart
GetPage(
  name: '/user/:id',
  page: () => UserPage(),
);
```

### Navigasi

```dart
Get.toNamed('/user/10');
```

### Ambil Parameter

```dart
String id = Get.parameters['id']!;
```

---

## Middleware Route

Contoh proteksi login:

```dart
class AuthMiddleware extends GetMiddleware {
  @override
  RouteSettings? redirect(String? route) {
    bool isLogin = false;

    if (!isLogin) {
      return const RouteSettings(name: '/login');
    }
    return null;
  }
}
```

---

## Kelebihan Route Management

* Tidak butuh context
* Navigasi cepat
* Named route
* Mudah kirim data
* Support middleware
* Struktur lebih clean

---

# 3. Dependency Management

## Apa Itu Dependency Management?

Dependency Management adalah sistem untuk membuat, menyimpan, menyediakan, dan menghapus object seperti Controller, Service, Repository, dan lainnya secara otomatis.

GetX menggunakan konsep:

> Dependency Injection (DI)

---

## Get.put()

Membuat object langsung.

```dart
Get.put(HomeController());
```

Ambil object:

```dart
Get.find<HomeController>();
```

---

## Get.lazyPut()

Object dibuat saat pertama kali dipanggil.

```dart
Get.lazyPut(() => AuthController());
```

Lebih hemat memory.

---

## Get.putAsync()

Untuk object asynchronous.

```dart
await Get.putAsync(() async {
  return await DatabaseService().init();
});
```

Cocok untuk:

* SharedPreferences
* Hive
* SQLite
* Firebase Init

---

## Get.create()

Selalu membuat instance baru.

```dart
Get.create(() => CartController());
```

---

## Permanent Dependency

Jika object harus tetap hidup selama aplikasi berjalan.

```dart
Get.put(AuthService(), permanent: true);
```

Contoh:

* Session login
* Theme service
* Language service

---

## Auto Dispose

GetX dapat menghapus dependency otomatis saat tidak digunakan lagi sehingga aplikasi lebih ringan.

---

## Binding (Best Practice)

Dependency sebaiknya dipisahkan menggunakan Binding.

### HomeBinding

```dart
class HomeBinding extends Bindings {
  @override
  void dependencies() {
    Get.put(HomeController());
  }
}
```

### Route dengan Binding

```dart
GetPage(
  name: '/home',
  page: () => HomePage(),
  binding: HomeBinding(),
);
```

Saat halaman dibuka, controller otomatis tersedia.

---

## Kelebihan Dependency Management

* Tidak perlu instance manual
* Reusable object
* Auto dispose
* Lebih scalable
* Clean architecture friendly
* Kode lebih rapi

---

# Perbandingan 3 Pilar

| Pilar                 | Fungsi                          |
| --------------------- | ------------------------------- |
| State Management      | Mengatur data dan UI            |
| Route Management      | Navigasi antar halaman          |
| Dependency Management | Mengelola object dan controller |

---

# Struktur Project GetX

```text
lib/
│── app/
│   ├── modules/
│   │   ├── home/
│   │   │   ├── home_page.dart
│   │   │   ├── home_controller.dart
│   │   │   └── home_binding.dart
│   │   ├── auth/
│   │   │   ├── login_page.dart
│   │   │   ├── auth_controller.dart
│   │   │   └── auth_binding.dart
│   ├── routes/
│   │   ├── app_pages.dart
│   │   └── app_routes.dart
│── main.dart
```

---

# Setup Awal

## Tambahkan Package

```yaml
dependencies:
  flutter:
    sdk: flutter
  get: ^4.6.6
```

---

## main.dart

```dart
import 'package:flutter/material.dart';
import 'package:get/get.dart';

void main() {
  runApp(
    GetMaterialApp(
      debugShowCheckedModeBanner: false,
      home: HomePage(),
    ),
  );
}
```

---

# Best Practice

## Gunakan Folder Modular

Pisahkan berdasarkan fitur:

* home
* auth
* profile
* settings

---

## Pisahkan Controller dan UI

Jangan campur logic dengan tampilan.

---

## Gunakan Binding

Agar dependency rapi dan otomatis.

---

## Gunakan Named Route

Untuk project besar lebih mudah dikelola.

---

## Gunakan Obx Hanya Jika Perlu

Agar performa tetap optimal.

---

# Kesimpulan

GetX memiliki 3 pilar utama yang membuat pengembangan Flutter jauh lebih cepat dan efisien:

1. **State Management** untuk mengatur perubahan data dan UI
2. **Route Management** untuk navigasi antar halaman
3. **Dependency Management** untuk mengelola object dan controller

Dengan GetX, aplikasi menjadi:

* Lebih clean
* Minim boilerplate
* Mudah maintenance
* Cepat dikembangkan
* Cocok untuk project besar
* Struktur profesional

---
