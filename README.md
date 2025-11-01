

````markdown
# 🪙 DonationBox Smart Contract (Algorand + Python)

**DonationBox** adalah smart contract sederhana berbasis **Algorand** yang dibuat menggunakan **Algopy** dan standar **ARC4**.  
Tujuannya adalah mensimulasikan *kotak donasi digital* di blockchain — tempat pengguna bisa “menyumbang”, melihat total donasi, dan mereset totalnya.

---

## 🧠 Fitur

- 💸 **donate(amount)** — Menambahkan jumlah donasi ke total.  
- 📊 **get_total()** — Menampilkan total donasi yang sudah terkumpul.  
- 🔁 **reset()** — Mengatur ulang total donasi menjadi nol.  

---

## 🧩 Kode Utama

```python
from algopy import ARC4Contract, UInt64, String
from algopy.arc4 import abimethod

class DonationBox(ARC4Contract):
    def __init__(self) -> None:
        self.total_donations = UInt64(0)

    @abimethod()
    def donate(self, amount: UInt64) -> String:
        self.total_donations += amount
        return String(f"Thank you for donating {amount} microAlgos!")

    @abimethod()
    def get_total(self) -> UInt64:
        return self.total_donations

    @abimethod()
    def reset(self) -> String:
        self.total_donations = UInt64(0)
        return String("Donation box has been reset!")
````

---

## ⚙️ Cara Menjalankan

### 1️⃣ Persiapan Lingkungan

Pastikan kamu sudah menginstal:

* **Python 3.10+**
* **pip**
* **Algopy SDK**
  Jalankan di terminal:

  ```bash
  pip install algopy beaker
  ```

### 2️⃣ Simpan Smart Contract

Buat file bernama:

```
donation_box.py
```

Lalu salin kode di atas ke dalam file tersebut.

### 3️⃣ Jalankan di Beaker Sandbox (simulasi Algorand)

Tambahkan file baru bernama:

```
run_donation.py
```

isi dengan kode ini:

```python
from beaker import sandbox
from donation_box import DonationBox

# Buat akun dan deploy kontrak di sandbox Algorand
app_client = sandbox.ApplicationClient(
    sandbox.get_accounts().pop(),
    DonationBox()
)

# Deploy aplikasi
app_client.create()

# Kirim donasi 100000 microAlgos
app_client.call(DonationBox.donate, amount=100000)

# Cek total donasi
result = app_client.call(DonationBox.get_total)
print("Total donations:", result.return_value)

# Reset donasi
app_client.call(DonationBox.reset)
print("Donation box has been reset!")
```

Lalu jalankan:

```bash
python run_donation.py
```

Jika berhasil, kamu akan melihat output seperti:

```
Total donations: 100000
Donation box has been reset!
```

---

## 📦 Struktur Folder

```
donation_box/
│
├── donation_box.py      # Kode smart contract utama
├── run_donation.py      # Script untuk menjalankan kontrak
└── README.md            # Dokumentasi proyek
```

---

## 🧾 Penjelasan Singkat

| Fungsi           | Keterangan                          |
| ---------------- | ----------------------------------- |
| `__init__()`     | Mengatur total donasi awal ke 0     |
| `donate(amount)` | Menambahkan jumlah donasi ke total  |
| `get_total()`    | Mengembalikan total donasi saat ini |
| `reset()`        | Menghapus total donasi (menjadi 0)  |

---

## 🧑‍💻 Dibuat Dengan

* **Python** 🐍
* **Algopy / ARC4** 💎
* **Beaker (Algorand Sandbox)** ⚙️

---

## 📜 Lisensi

Proyek ini bersifat open-source.
Silakan gunakan dan modifikasi untuk pembelajaran atau pengembangan lebih lanjut.

---

````

---

### 🚀 **Cara cepat ringkas**
1. Buat folder → `donation_box`  
2. Isi tiga file:
   - `donation_box.py` (isi kode kontrak)
   - `run_donation.py` (isi script run)
   - `README.md` (isi teks di atas)
3. Jalankan:
   ```bash
   pip install algopy beaker
   python run_donation.py
````

---

