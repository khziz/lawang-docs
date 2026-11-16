# Constructor & object global bawaan

Melengkapi daftar callable global engine. Constructor umumnya dipanggil dengan `new`. `String`, `Number`, `Boolean`, `Object`, `Array`, `Function`, `Date`, `RegExp`, dan `Error` juga bisa dipanggil sebagai fungsi biasa. `BigInt` dan `Symbol` **tidak boleh** memakai `new`.

Metode statis dan prototipe tidak dirinci sebagai API Lawang.

## Tipe dasar

| Nama | Contoh dan hasil |
|---|---|
| `Object(value?)` | `Object()`, `Object({id: 7})` menghasilkan object |
| `Function(...parameters, body)` | `Function("return 7")()`, `Function("a", "return a+1")(2)` membangun fungsi dari string |
| `Array(...items)` / `Array(length)` | `Array(3)` array berlubang panjang 3. `Array(1,2)` dua elemen |
| `String(value?)` | `String(7)` → `"7"`. `String()` → `""`. `new String("x")` adalah wrapper object |
| `Number(value?)` | `Number("42")` → `42`. `Number()` → `0`. `Number("x")` → `NaN` |
| `Boolean(value?)` | `Boolean(0)` → `false`. `Boolean("false")` → `true`. `new Boolean(false)` adalah object truthy |
| `BigInt(value)` | `BigInt("9007199254740993")`, `BigInt(7)`. Hasil bigint, bukan number |
| `Symbol(description?)` | `Symbol()`, `Symbol("id")`. Masing-masing unik |
| `RegExp(pattern, flags?)` | `RegExp("ab+")`, `new RegExp("ab+", "i")`. Pattern/flag invalid melempar error |
| `Date(value?)` / `Date(y,m,...)` | `new Date()`, `new Date("2026-10-23T00:00:00Z")`, `new Date(2026,9,23)`. `Date()` tanpa `new` menghasilkan string waktu kini |

## Koleksi dan lanjutan

| Nama | Contoh dan hasil |
|---|---|
| `Map(iterable?)` / `Set(iterable?)` | `new Map()`, `new Map([["a",1]])`, `new Set()`, `new Set([1,1,2])` |
| `WeakMap(iterable?)` / `WeakSet(iterable?)` | `const k={}; new WeakMap([[k,1]]); new WeakSet([k])`. Koleksi lemah |
| `Promise(executor)` | `new Promise(resolve => resolve(7))`. **Bukan** penyedia event loop |
| `Proxy(target, handler)` | `new Proxy({id:7}, {})`. Target dan handler harus object |
| `Iterator` | Constructor abstrak. Gunakan subclass yang menyediakan `next()`, bukan `new Iterator()` langsung |

## Error

| Nama | Contoh |
|---|---|
| `Error(message?, options?)` | `new Error("gagal")`, `new Error("gagal", {cause: "asal"})` |
| `TypeError`, `ReferenceError`, `SyntaxError` | `new TypeError("tipe salah")`, bentuk argumen mengikuti `Error` |
| `RangeError`, `EvalError`, `URIError`, `GoError` | `new RangeError("batas")`, `new GoError("gagal")`. `GoError` adalah tambahan engine |
| `AggregateError(errors, message?, options?)` | `new AggregateError([new Error("A")], "gagal")` |

## Buffer dan typed array

| Nama | Contoh |
|---|---|
| `ArrayBuffer(byteLength)` | `new ArrayBuffer(0)`, `new ArrayBuffer(16)` |
| `DataView(buffer, offset?, length?)` | `new DataView(new ArrayBuffer(8), 2, 4)` |
| `TypedArray(length / array / buffer, offset?, length?)` | `new Uint8Array(4)`, `new Uint8Array([1,2])`, `new Uint8Array(new ArrayBuffer(8), 2, 4)` |

**Keluarga typed array:** `Int8Array`, `Uint8Array`, `Uint8ClampedArray`, `Int16Array`, `Uint16Array`, `Int32Array`, `Uint32Array`, `Float32Array`, `Float64Array`, `BigInt64Array`, `BigUint64Array`.

`BigInt64Array` dan `BigUint64Array` membutuhkan nilai bigint: `new BigInt64Array([1n, 2n])`.

## Object namespace (tidak bisa dipanggil)

* `JSON`: `JSON.parse("{\"id\":7}")`, `JSON.stringify({id:7})`
* `Math`: `Math.floor(3.9)`
* `Reflect`: `Reflect.get({id:7}, "id")`
* `globalThis`: menunjuk global object

`NaN`, `Infinity`, dan `undefined` adalah nilai global, bukan fungsi.
