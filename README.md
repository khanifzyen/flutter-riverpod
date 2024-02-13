---
description: Riverpod merupakan salah satu solusi state management dalam Flutter.
---

# Introduction

**Riverpod** hadir untuk melengkapi kekurangan dari **Provider** (_state management_ yang lain). Tetapi Riverpod tidak hanya solusi _state management_, tetapi juga sebagai pengganti _design pattern_ seperti **Singleton**, **Dependency Injection**, dan **Service Locator**. Ia juga hadir untuk membantu melakukan _fetching, caching,_ dan _cancelling network request_ dimana ia juga bisa melakukan _handling error._

Untuk memulai buatlah project baru dengan nama `riverpod_demo`

```bash
flutter create riverpod_demo
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

Saat ini kita tidak perlu menjalankan generator karena anda belum membuat kode satupun. Untuk menjalankan riverpod generator dengan perintah `dart run build_runner watch`

Dengan `watch` nanti akan memantau ketika ada annotation baru, ia akan menggenerate otomatis, ketika anda sudah selesai dan ingin publish app anda bisa mengganti `watch` dengan `build`
