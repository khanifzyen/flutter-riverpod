# ProviderScope

Sebelum membahas `ProviderScope`, anda bisa membuat sebuah variabel global yang diletakkan diluar _function_ `main()`.

{% code title="lib/main.dart" %}
```dart
import 'package:flutter/material.dart';

String name = '';

void main() {
  name = 'Akhmad';
  runApp(const MyApp());
}
...
```
{% endcode %}

{% code title="lib/homepage.dart" %}
```dart
import 'package:flutter/material.dart';
import 'main.dart';

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    name = 'Khanif';
...
```
{% endcode %}

Bayangkan jika ada 10 atau 100 file yang akan mengakses dan mengubah nilai variabel global tersebut maka akan terdapat masalah jika variabel global tidak diset menjadi `final` yaitu akan sangat sulit untuk mencari tahu siapa yang mengubah variabel global tersebut. Untuk itulah `ProviderScope` dan `Provider` digunakan.

selain&#x20;
