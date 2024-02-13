# Membuat Provider/Network Request Pertama

Mengambil data dari internet (network request) merupakan inti dari aplikasi apa pun. Namun ada banyak hal yang perlu dipertimbangkan saat membuat network request:

* UI harus merender status Loading saat request sedang dibuat&#x20;
* Error handling harus dilakukan dengan baik&#x20;
* Request tersebut harus di-cache jika memungkinkan&#x20;

Di bagian ini, kita akan melihat bagaimana Riverpod dapat membantu kita mengatasi semua ini secara alami.

## Menyiapkan `ProviderScope`

Sebelum anda mulai membuat network request, pastikan `ProviderScope` ditambahkan di root aplikasi.

```dart
void main() {
  runApp(
    ProviderScope(
      child: MyApp(),
    ),
  );
}
```

Dengan cara ini maka akan mengaktifkan Riverpod untuk seluruh aplikasi.

## Melakukan network request didalam "provider"

Melakukan network request biasanya disebut dengan "bisnis logic". Di Riverpod, logika bisnis ditempatkan di dalam "provider". Provider adalah fungsi yang memiliki banyak fitur. Mereka berperilaku seperti fungsi pada umumnya, dengan manfaat tambahan berupa:

* dapat di-cache&#x20;
* memiliki penanganan eror/loading/success bawaan&#x20;
* bisa di-listen
* otomatis melakukan re-eksekusi ketika ada data berubah

Hal ini membuat provider sangat cocok untuk network request dengan method GET (untuk method POST/dan lain-lain, akan dibahas kemudian).

Sebagai contoh, mari kita buat sebuah aplikasi sederhana yang menyarankan aktivitas acak untuk dilakukan saat bosan. Untuk melakukannya, anda dapat menggunakan [Bored API](https://www.boredapi.com/). Secara khusus, anda akan melakukan request GET pada _endpoint_ `/api/activity`. _Endpoint_ ini mengembalikan objek JSON, yang akan kita ubah menjadi instance dari class Dart. Langkah selanjutnya adalah menampilkan aktivitas ini di UI. Anda juga akan memastikan untuk merender status loading saat request dibuat, dan ada juga penanganan error dengan baik.

## Mendifinisikan model

Sebelum memulai, anda perlu menentukan model data yang akan kita terima dari API. Model ini juga memerlukan cara untuk mengubah objek JSON menjadi instance dari class Dart. Berikut adalah response JSON dari Bored API:

```json
{
  "activity": "Start a collection",
  "type": "recreational",
  "participants": 1,
  "price": 0,
  "link": "",
  "key": "1718657",
  "accessibility": 0.5
}
```

Umumnya, disarankan untuk menggunakan generator kode seperti [freezed](https://pub.dev/packages/freezed) atau [json\_serializable](https://pub.dev/packages/json\_serializable) untuk menangani decoding JSON. Namun tentu saja, anda juga bisa melakukannya dengan manual.

```bash
flutter pub add freezed_annotation
flutter pub add dev:freezed
flutter pub add json_annotation
flutter pub add dev:json_serializable
```

Tulis kode model seperti berikut:

{% code title="models/activity.dart" %}
```dart

import 'package:freezed_annotation/freezed_annotation.dart';

part 'activity.freezed.dart';
part 'activity.g.dart';

/// The response of the `GET /api/activity` endpoint.
/// It is defined using `freezed` and `json_serializable`.
@freezed
class Activity with _$Activity {
  factory Activity({
    required String key,
    required String activity,
    required String type,
    required int participants,
    required double price,
  }) = _Activity;

  /// Convert a JSON object into an [Activity] instance.
  /// This enables type-safe reading of the API response.
  factory Activity.fromJson(Map<String, dynamic> json) =>
      _$ActivityFromJson(json);
}
```
{% endcode %}

## Membuat provider

Sekarang setelah anda memiliki model, anda dapat mulai melakukan query API. Untuk melakukannya, anda perlu membuat provider pertama.

Sintaks untuk mendefinisikan provider adalah sebagai berikut:

```dart
@riverpod
Result myFunction(MyFunctionRef ref) {
  <your logic here>
}
```

<table><thead><tr><th width="178">Kode</th><th>Deskripsi</th></tr></thead><tbody><tr><td><code>@riverpod</code></td><td><p>Semua provider harus diberikan anotasi dengan <code>@riverpod</code> atau <code>@Riverpod()</code>. Anotasi ini dapat ditempatkan pada function atau class global. Melalui anotasi ini, dimungkinkan untuk mengkonfigurasi provider.</p><p>Misalnya, kita dapat menonaktifkan "auto-dispose" (yang akan kita lihat nanti) dengan menulis <code>@Riverpod(keepAlive: true)</code>.</p></td></tr><tr><td><code>myFunction</code></td><td><p>Nama function yang dianotasi menentukan bagaimana provider akan berinteraksi. Untuk function<code>myFunction</code>, variabel <code>myFunctionProvider</code> akan di-generate.</p><p>Function yang dianotasi harus menjadikan "<code>ref</code>" sebagai parameter pertama. Selain itu, function tersebut dapat memiliki sejumlah parameter, termasuk parameter umum. Function ini juga bisa mengembalikan <code>Future/Stream</code> jika diinginkan.</p><p>Function ini akan dipanggil ketika provider pertama kali dibaca. Pembacaan selanjutnya/berulang-ulang tidak akan memanggil function itu lagi, melainkan mengembalikan nilai yang di-cache.</p></td></tr><tr><td><code>MyFunctionRef ref</code></td><td>Objek yang digunakan untuk berinteraksi dengan provider lain. Semua provider memiliki satu; baik sebagai parameter function provider, atau sebagai property Notifier. Jenis objek ini ditentukan oleh nama function/classnya.</td></tr></tbody></table>

Dalam kasus ini, anda ingin `GET` activity dari API. Karena GET adalah operasi asinkron, itu berarti anda perlu membuatnya menjadi `Future<Activity>`.

Dengan menggunakan sintaks yang ditentukan sebelumnya, anda dapat mendefinisikan provider sebagai berikut:

{% code title="providers/provider.dart" %}
```dart

import 'dart:convert';
import 'package:http/http.dart' as http;
import 'package:riverpod_annotation/riverpod_annotation.dart';
import '../models/activity.dart';

// Necessary for code-generation to work
part 'provider.g.dart';

/// This will create a provider named `activityProvider`
/// which will cache the result of this function.
@riverpod
Future<Activity> activity(ActivityRef ref) async {
  // Using package:http, we fetch a random activity from the Bored API.
  final response = await http.get(Uri.https('boredapi.com', '/api/activity'));
  // Using dart:convert, we then decode the JSON payload into a Map data structure.
  final json = jsonDecode(response.body) as Map<String, dynamic>;
  // Finally, we convert the Map into an Activity instance.
  return Activity.fromJson(json);
}
```
{% endcode %}
