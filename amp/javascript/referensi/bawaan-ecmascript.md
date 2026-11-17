# Fungsi global bawaan ECMAScript

Fungsi berikut tersedia dari global object Sobek (sesuai versi dependency repository). Semantiknya berlaku untuk **fungsi global**, bukan metode bernama mirip seperti `Number.isNaN`.

## Parsing angka

### `parseInt(text, radix?)`

Membaca awalan integer dari string. `radix` 2 sampai 36. Hasil number atau `NaN`. Gunakan radix eksplisit.

```js
parseInt('42', 10);   // 42
parseInt('ff', 16);   // 255
parseInt('10px', 10); // 10
parseInt('x', 10);    // NaN
```

### `parseFloat(text)`

Membaca awalan bilangan desimal. Hasil number atau `NaN`.

```js
parseFloat('3.14');  // 3.14
parseFloat('2.5kg'); // 2.5
parseFloat('x');     // NaN
```

## Pemeriksaan angka

### `isNaN(value)`

Mengonversi `value` ke number, lalu memeriksa `NaN`. Konversi tipe tertentu bisa melempar error.

```js
isNaN('abc');      // true
isNaN('12');       // false
isNaN(undefined);  // true
isNaN(null);       // false
```

### `isFinite(value)`

Mengonversi `value` ke number, lalu memeriksa apakah berhingga.

```js
isFinite('12');     // true
isFinite(Infinity); // false
isFinite(NaN);      // false
isFinite(null);     // true
```

## Encoding URL

| Fungsi | Kegunaan |
|---|---|
| `encodeURI(uri)` | Mengodekan URI **lengkap**. Delimiter seperti `:`, `/`, `?`, `&` tetap |
| `decodeURI(uri)` | Mendekode URI lengkap. Escape invalid melempar `URIError` |
| `encodeURIComponent(value)` | Mengodekan **satu komponen**, termasuk delimiter |
| `decodeURIComponent(value)` | Mendekode komponen. Tidak mengubah `+` menjadi spasi |

```js
encodeURI('https://example.com/a b?q=x'); // https://example.com/a%20b?q=x
encodeURI('/a b');                        // /a%20b
decodeURI('/a%20b');                      // /a b
decodeURI('%2F');                         // %2F (delimiter tetap ter-encode)
try { decodeURI('%'); } catch (err) {}

encodeURIComponent('a&b c');              // a%26b%20c
const url = 'https://example.com/?q=' + encodeURIComponent('kopi susu');
decodeURIComponent('a%26b%20c');          // a&b c
decodeURIComponent('a+b');                // a+b
try { decodeURIComponent('%'); } catch (err) {}
```

{% hint style="info" %}
Untuk nilai query string, gunakan `encodeURIComponent`.
{% endhint %}

## Lain-lain

### `eval(source)`

Mengevaluasi string JavaScript dan mengembalikan completion value. Non-string dikembalikan apa adanya. Error sintaks atau runtime melempar exception.

```js
eval('1 + 2');       // 3
eval(7);             // 7
eval('({id: 7})');   // {id:7}
```

### `escape(value)` dan `unescape(value)`

Fungsi **legacy** untuk encoding string (`%XX` / `%uXXXX`). Bukan encoder URL modern, hindari untuk kode baru.

```js
escape('a b');       // a%20b
escape('@/+');       // @/+
unescape('a%20b');   // a b
unescape('%u0041');  // A
```
