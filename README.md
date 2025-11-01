
```markdown
# Smart Contract Kotak Donasi (Donation Box) - AlgoPy

Ini adalah contoh *smart contract* sederhana untuk blockchain **Algorand** yang dibangun menggunakan *framework* **AlgoPy**.

## 🎯 Tujuan

Kontrak ini berfungsi sebagai "kotak donasi" digital (Digital Donation Box) yang transparan. Tujuannya adalah untuk menerima, mencatat, dan melacak jumlah total donasi (dalam microAlgos) yang telah dikirimkan ke alamat kontrak ini.

## ⚙️ Fitur / Metode ABI

Kontrak ini mengekspos tiga metode utama yang dapat dipanggil (sesuai standar ARC4):

* **`donate(amount: UInt64) -> String`**
    * Memperbolehkan siapa saja untuk mengirimkan donasi.
    * Parameter `amount` akan ditambahkan ke total donasi yang tersimpan di *global state* kontrak.
    * Mengembalikan sebuah *string* sebagai pesan konfirmasi.

* **`get_total() -> UInt64`**
    * Metode "read-only" (hanya membaca) untuk mengecek total donasi.
    * Mengembalikan jumlah total donasi (dalam `UInt64`) yang telah terkumpul.

* **`reset() -> String`**
    * Mengatur ulang (me-reset) total donasi kembali ke `0`.
    * *Catatan: Dalam implementasi di dunia nyata, metode seperti ini harus dibatasi agar hanya bisa dipanggil oleh kreator kontrak.*

## 🚀 Cara Menggunakan

Proyek ini dapat digunakan sebagai contoh dasar untuk mempelajari pengembangan *smart contract* Algorand dengan `algopy`, khususnya untuk memahami cara kerja:
* Inisialisasi dan pengelolaan *global state* (`total_donations`).
* Pembuatan metode ABI (`@abimethod`) untuk memodifikasi dan membaca *state*.
```

-----

## 🔍 Penjelasan Kode

Kode Anda mendefinisikan sebuah *smart contract* Algorand bernama `DonationBox` menggunakan *framework* `algopy`.

Berikut adalah rincian dari setiap bagiannya:

### 1\. Impor dan Kelas Utama

```python
from algopy import ARC4Contract, UInt64, String
from algopy.arc4 import abimethod

class DonationBox(ARC4Contract):
```

  * Anda mengimpor komponen-komponen penting dari `algopy`.
  * `ARC4Contract`: Ini adalah kelas dasar untuk semua *smart contract* yang ingin Anda buat agar sesuai dengan standar **ARC4 (Algorand ABI)**. Ini memudahkan *tools* dan *front-end* untuk berinteraksi dengan kontrak Anda.
  * `UInt64`: Tipe data standar untuk angka bulat 64-bit tanpa tanda (positif). Ini adalah tipe data yang umum digunakan untuk nilai mata uang (seperti microAlgos) di Algorand.
  * `String`: Tipe data untuk teks/string.
  * `@abimethod`: Ini adalah *decorator* yang Anda tempatkan di atas sebuah fungsi untuk memberitahu `algopy` bahwa fungsi ini harus terekspos ke publik dan dapat dipanggil dari luar *blockchain*.

### 2\. Inisialisasi (`__init__`)

```python
    def __init__(self) -> None:
        self.total_donations= UInt64(0)
```

  * Metode `__init__` ini **hanya berjalan satu kali**, yaitu saat kontrak pertama kali di-*deploy* (dibuat) di *blockchain*.
  * Fungsinya adalah untuk mengatur *state* (penyimpanan) awal kontrak.
  * `self.total_donations = UInt64(0)`: Perintah ini membuat satu variabel di **Global State** (penyimpanan permanen kontrak) yang bernama `total_donations` dan memberinya nilai awal `0`.

### 3\. Metode `donate`

```python
    @abimethod()
    def donate(self, amount: UInt64) -> String:
        self.total_donations += amount
        return String("Thank you for donating {amount} microAlgos!")
```

  * `@abimethod()`: Menandakan ini adalah fungsi yang bisa dipanggil oleh siapa saja melalui transaksi.
  * `amount: UInt64`: Fungsi ini menerima satu argumen bernama `amount` dengan tipe `UInt64`.
  * `self.total_donations += amount`: Ini adalah inti dari fungsi. Ia mengambil nilai `amount` yang diberikan dalam transaksi dan **menambahkannya** ke nilai `total_donations` yang sudah tersimpan di *global state*.
  * `return String(...)`: Fungsi ini mengembalikan sebuah *string* konfirmasi.
      * **Penting:** Dalam kode Anda, `"{amount}"` akan dikembalikan sebagai teks harfiah (seperti yang tertulis), bukan nilai dari variabel `amount`. Jika Anda ingin menampilkan jumlahnya, Anda perlu memformat *string* tersebut secara berbeda (meskipun seringkali *string* statis sudah cukup untuk konfirmasi).

### 4\. Metode `get_total`

```python
    @abimethod()
    def get_total(self) -> UInt64:
        return self.total_donations
```

  * `@abimethod()`: Ini juga fungsi publik.
  * Fungsi ini tidak menerima argumen apa pun.
  * `return self.total_donations`: Perintah ini **membaca** nilai saat ini dari `total_donations` di *global state* dan mengembalikannya kepada pemanggil. Ini adalah operasi "read-only" (hanya baca).

### 5\. Metode `reset`

```python
    @abimethod()
    def reset(self) -> String:
        self.total_donations = UInt64(0)
        return String("Donation box has been reset!")
```

  * `@abimethod()`: Fungsi publik.
  * `self.total_donations = UInt64(0)`: Perintah ini **menimpa** nilai `total_donations` di *global state* dan mengaturnya kembali ke `0`.
  * `return String(...)`: Mengembalikan *string* konfirmasi bahwa reset telah berhasil.

Secara singkat, ini adalah kontrak yang sangat baik untuk contoh dasar: ia menunjukkan cara **membuat state** (`__init__`), **memperbarui state** (`donate`, `reset`), dan **membaca state** (`get_total`).

-----

