# Introduction

**Riverpod** hadir untuk melengkapi kekurangan dari **Provider** (_state management_ yang lain). Tetapi Riverpod tidak hanya solusi _state management_, tetapi juga sebagai pengganti _design pattern_ seperti **Singleton**, **Dependency Injection**, dan **Service Locator**. Ia juga hadir untuk membantu melakukan _fetching, caching,_ dan _cancelling network request_ dimana ia juga bisa melakukan _handling error._

Untuk memulai buatlah project baru dengan nama `project_riverpod`

```bash
flutter create project_riverpod
```

Selanjutnya anda perlu menginstal package yang diperlukan, dengan anda melakukan instalasi package seperti berikut anda akan mendapatkan versi terbaru dari package tersebut:

```bash
flutter pub add flutter_riverpod
flutter pub add riverpod_annotation
flutter pub add dev:riverpod_generator
flutter pub add dev:build_runner
flutter pub add dev:custom_lint
flutter pub add dev:riverpod_lint
```

Keterangan:

* `flutter_riverpod` digunakan untuk mendapatkan semua fitur dari riverpod di dalam flutter
* `riverpod_annotation` digunakan untuk menandai class yang nanti akan menjadi provider, sehingga nanti ia akan mengenal annotasi `@riverpod` dan `@Riverpod(keepAlive:true)`
* `riverpod_generator` bertugas untuk mengenerate provider yang  sudah ditandai dengan annotation
* `build_runner` bertugas sebagai untuk menjalankan riverpod\_generator
* `custom_lint`  bertugas sebagai linter custom, dimana nanti riverpod lint didaftarkan
* `riverpod_lint` bertugas sebagai linter untuk riverpod

Untuk menjalankan riverpod generator dengan perintah `dart run build_runner watch`

Dengan `watch` nanti akan memantau ketika ada annotation baru, ia akan menggenerate otomatis jadi tidak masalah walaupun anda sama sekali belum mengetik kode, Ketika anda sudah selesai dan ingin publish app anda bisa mengganti `watch` dengan `build`

## Mengaktifkan riverpod\_lint/custom\_lint

Riverpod hadir dengan package `riverpod_lint` yang menyediakan aturan lint untuk membantu Anda menulis kode yang lebih baik, dan menyediakan opsi refactoring custom.

Package tersebut seharusnya sudah diinstal jika Anda mengikuti langkah sebelumnya, namun diperlukan langkah terpisah untuk mengaktifkannya.

Untuk mengaktifkan `riverpod_lint`, buat file `analysis_options.yaml` yang diletakkan setara dengan`pubspec.yaml` dan tulis kode berikut:

```yaml
analyzer:
  plugins:
    - custom_lint
```

Anda sekarang akan melihat peringatan di IDE jika Anda melakukan kesalahan saat menggunakan Riverpod di dalam kode Flutter anda.

## Contoh penggunaan: Hello World

{% code title="lib/main.dart" %}
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:riverpod_annotation/riverpod_annotation.dart';

part 'main.g.dart';

// Membuat "provider", dimana akan menyimpan nilai ("Hello world").
// Dengan menggunakan provider, ini dapat mengekspos nilai ini dari widget tree manapun.
@riverpod
String helloWorld(HelloWorldRef ref) {
  return 'Hello world';
}

void main() {
  runApp(
    // Agar widget dapat membaca provider, anda perlu membungkus induk widget/app
    // dengan "ProviderScope" widget.
    // Disinilah state dari provider akan disimpan.
    ProviderScope(
      child: MyApp(),
    ),
  );
}

// Extend ConsumerWidget dari sebelumnya StatelessWidget, agar bisa dibaca oleh Riverpod
class MyApp extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final String value = ref.watch(helloWorldProvider);

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text('Example')),
        body: Center(
          child: Text(value),
        ),
      ),
    );
  }
}
```
{% endcode %}

Simpan dan jalankan. Anda bisa lihat di debug console, bahwa sukses build, dan muncul file baru hasil  generator yaitu main.g.dart

```bash
[INFO] Starting Build
[INFO] Updating asset graph completed, took 0ms
[INFO] Running build completed, took 101ms
[INFO] Caching finalized dependency graph completed, took 21ms
[INFO] Succeeded after 124ms with 3 outputs (5 actions)
```

{% hint style="info" %}
Untuk meningkatkan produktifitas di VS Code, anda bisa memasang extensions `Flutter Riverpod Snippets`
{% endhint %}
