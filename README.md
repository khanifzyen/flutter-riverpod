---
description: Riverpod merupakan salah satu solusi state management dalam Flutter.
---

# Introduction

**Riverpod** hadir untuk melengkapi kekurangan dari **Provider** (_state management_ yang lain). Tetapi Riverpod tidak hanya solusi _state management_, tetapi juga sebagai pengganti _design pattern_ seperti **Singleton**, **Dependency Injection**, dan **Service Locator**. Ia juga hadir untuk membantu melakukan _fetching, caching,_ dan _cancelling network request_ dimana ia juga bisa melakukan _handling error._

Riverpod sendiri terbagi menjadi 3 _packages_ yaitu:

* `riverpod`: digunakan dengan Dart SDK (biasanya untuk server side atau aplikasi CLI)&#x20;
* `flutter_riverpod`: digunakan dengan Flutter SDK
* `hooks_riverpod`: digunakan dengan _package_ `flutter_hooks`

Untuk memulai buatlah project baru dengan nama `riverpod_demo`

```bash
flutter create riverpod_demo
```

Kemudian anda bisa menambahkan _dependency_ **flutter\_riverpod** pada file `pubspec.yaml`

```yaml
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.2
  flutter_riverpod: ^2.1.3
```

