# 🏗️ FASE 4 — Paradigma & Struktur Berpikir (Lanjutan)

> **Misi Fase ini:** Belajar membangun program skala besar dengan cara **memodelkan dunia nyata ke dalam kode**. Fase ini adalah lompatan terbesar dalam perjalanan belajar Python kamu.

---

# Topik 12: OOP — Classes, Objects, dan `__init__`

## 1. The Core Truth (First Principles)

Bayangkan kamu membuat game dengan 100 karakter. Setiap karakter punya nama, HP, level, dan bisa menyerang. Dengan pendekatan lama, kamu butuh ratusan variable terpisah dan fungsi yang tidak terorganisir.

**Object-Oriented Programming (OOP)** adalah cara mengatasi ini dengan satu ide sederhana: **kelompokkan data (atribut) dan perilaku (method) yang berkaitan ke dalam satu unit bernama Object.**

**Class** adalah *cetak biru* (blueprint). **Object** adalah *hasil cetakannya*. Seperti blueprint rumah vs. rumah yang sudah dibangun — satu blueprint bisa menghasilkan banyak rumah.

---

## 2. The Basic Anatomy

```python
# CLASS = cetak biru / template
class Karakter:
    # __init__ = constructor: otomatis dipanggil saat object dibuat
    # self = referensi ke object itu sendiri
    def __init__(self, nama, hp):
        # Atribut instance: data milik SETIAP object
        self.nama = nama
        self.hp = hp

    # Method: perilaku yang bisa dilakukan object
    def sapa(self):
        print(f"Halo, aku {self.nama} dengan HP {self.hp}!")


# OBJECT = hasil instantiasi dari class
hero = Karakter("Andi", 100)   # __init__ dipanggil otomatis
musuh = Karakter("Goblin", 50)

hero.sapa()   # Halo, aku Andi dengan HP 100!
musuh.sapa()  # Halo, aku Goblin dengan HP 50!
```

> 💡 **`self` selalu jadi parameter pertama** di setiap method. Python secara otomatis mengisinya dengan object yang memanggil method tersebut. Kamu tidak perlu mengisi `self` saat memanggil.

---

## 3. Deep Dive & Mechanisms

### Atribut Instance vs. Atribut Class

```python
class Karakter:
    # ATRIBUT CLASS: milik semua object, nilainya sama
    # Diakses via nama class atau self
    max_level = 99
    jumlah_karakter = 0  # Berguna untuk melacak berapa object dibuat

    def __init__(self, nama, hp, level=1):
        # ATRIBUT INSTANCE: milik masing-masing object, nilainya bisa beda
        self.nama = nama
        self.hp = hp
        self.level = level

        # Modifikasi atribut class saat object baru dibuat
        Karakter.jumlah_karakter += 1

    def info(self):
        print(f"{self.nama} | HP: {self.hp} | Level: {self.level}/{self.max_level}")


hero = Karakter("Andi", 100)
mage = Karakter("Siti", 80, level=5)

hero.info()  # Andi | HP: 100 | Level: 1/99
mage.info()  # Siti | HP: 80  | Level: 5/99

print(Karakter.jumlah_karakter)  # 2 ← atribut class diakses via nama class
```

### Method yang Lengkap

```python
class BankAccount:
    suku_bunga = 0.05  # Atribut class: sama untuk semua akun

    def __init__(self, pemilik, saldo_awal=0):
        self.pemilik = pemilik
        self.saldo = saldo_awal
        self._riwayat = []  # _ di depan = konvensi "semi-private"

    # INSTANCE METHOD: beroperasi pada data instance (self)
    def setor(self, jumlah):
        if jumlah <= 0:
            print("Jumlah setor harus positif!")
            return
        self.saldo += jumlah
        self._riwayat.append(f"Setor: +{jumlah:,}")
        print(f"Setor {jumlah:,} berhasil. Saldo: {self.saldo:,}")

    def tarik(self, jumlah):
        if jumlah > self.saldo:
            print("Saldo tidak cukup!")
            return
        self.saldo -= jumlah
        self._riwayat.append(f"Tarik: -{jumlah:,}")
        print(f"Tarik {jumlah:,} berhasil. Saldo: {self.saldo:,}")

    def cetak_riwayat(self):
        print(f"\n=== Riwayat {self.pemilik} ===")
        for transaksi in self._riwayat:
            print(f"  {transaksi}")

    # CLASS METHOD: beroperasi pada atribut class, bukan instance
    # Gunakan @classmethod dan parameter "cls" (bukan "self")
    @classmethod
    def set_suku_bunga(cls, bunga_baru):
        cls.suku_bunga = bunga_baru
        print(f"Suku bunga diubah ke {bunga_baru * 100}%")

    # STATIC METHOD: tidak butuh self atau cls — fungsi biasa yang "numpang" di class
    # Gunakan jika fungsi masih logis ada di class tapi tidak butuh data instance/class
    @staticmethod
    def validasi_jumlah(jumlah):
        return isinstance(jumlah, (int, float)) and jumlah > 0


# Penggunaan
akun = BankAccount("Budi", saldo_awal=500_000)
akun.setor(200_000)    # Setor 200,000 berhasil. Saldo: 700,000
akun.tarik(100_000)    # Tarik 100,000 berhasil. Saldo: 600,000
akun.cetak_riwayat()

BankAccount.set_suku_bunga(0.06)              # Class method dipanggil via class
print(BankAccount.validasi_jumlah(50_000))    # True
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Class adalah **formulir kosong**. `__init__` adalah cara mengisi formulir itu. Setiap object adalah **formulir yang sudah terisi** dengan data berbeda.

```python
# ❌ BAD: Semua logika ditumpuk tanpa struktur
nama1, hp1, level1 = "Andi", 100, 5
nama2, hp2, level2 = "Siti", 80, 3

def serang(nama_penyerang, hp_target, damage):
    hp_target -= damage
    print(f"{nama_penyerang} menyerang! HP target: {hp_target}")

# ✅ GOOD: Data dan perilaku dikapsulkan dalam class
class Karakter:
    def __init__(self, nama, hp, level):
        self.nama = nama
        self.hp = hp
        self.level = level

    def serang(self, target, damage):
        target.hp -= damage
        print(f"{self.nama} menyerang {target.nama}! HP {target.nama}: {target.hp}")

andi = Karakter("Andi", 100, 5)
siti = Karakter("Siti", 80, 3)
andi.serang(siti, 20)  # Andi menyerang Siti! HP Siti: 60
```

---
---

# Topik 13: Inheritance, Polymorphism & Encapsulation

## 1. The Core Truth (First Principles)

Tiga pilar OOP ini muncul dari satu masalah nyata dalam program skala besar: **duplikasi dan ketidakfleksibelan**.

- **Inheritance (Pewarisan):** Kenapa menulis ulang kode yang sama? Biarkan class baru *mewarisi* semua yang dimiliki class induknya.
- **Polymorphism:** Bagaimana cara memanggil aksi yang sama (misal: `bersuara()`) tapi menghasilkan perilaku berbeda untuk setiap jenis object?
- **Encapsulation:** Bagaimana melindungi data internal agar tidak bisa dimodifikasi sembarangan dari luar?

---

## 2. The Basic Anatomy

```python
# CLASS INDUK (Parent / Base Class)
class Hewan:
    def __init__(self, nama, umur):
        self.nama = nama
        self.umur = umur

    def info(self):
        print(f"{self.nama}, {self.umur} tahun")

    def bersuara(self):
        print("...")  # Placeholder — akan di-override


# CLASS ANAK (Child / Derived Class) — mewarisi Hewan
class Anjing(Hewan):  # ← Anjing adalah Hewan
    def bersuara(self):
        print(f"{self.nama}: Guk guk!")  # Override method induk


class Kucing(Hewan):
    def bersuara(self):
        print(f"{self.nama}: Meong!")


# Polymorphism: panggil method yang SAMA, perilaku BERBEDA
hewan_list = [Anjing("Rex", 3), Kucing("Mimi", 2), Anjing("Buddy", 5)]

for h in hewan_list:
    h.bersuara()  # Python tahu method mana yang dipanggil secara otomatis!

# Rex: Guk guk!
# Mimi: Meong!
# Buddy: Guk guk!
```

---

## 3. Deep Dive & Mechanisms

### `super()` — Memanggil Constructor Induk

```python
class Kendaraan:
    def __init__(self, merk, tahun, kecepatan_max):
        self.merk = merk
        self.tahun = tahun
        self.kecepatan_max = kecepatan_max

    def info(self):
        print(f"{self.merk} ({self.tahun}) - Max: {self.kecepatan_max} km/h")


class Mobil(Kendaraan):
    def __init__(self, merk, tahun, kecepatan_max, jumlah_pintu):
        # super() memanggil __init__ dari class induk
        # Hindari duplikasi: tidak perlu tulis ulang merk, tahun, kecepatan_max
        super().__init__(merk, tahun, kecepatan_max)
        self.jumlah_pintu = jumlah_pintu  # Atribut tambahan khusus Mobil

    def info(self):
        super().info()  # Panggil info() dari induk dulu
        print(f"  Jumlah pintu: {self.jumlah_pintu}")


class Sepeda(Kendaraan):
    def __init__(self, merk, tahun, kecepatan_max, jenis):
        super().__init__(merk, tahun, kecepatan_max)
        self.jenis = jenis  # "gunung", "balap", "lipat"

    def info(self):
        super().info()
        print(f"  Jenis: {self.jenis}")


avanza = Mobil("Toyota", 2022, 160, 4)
mtb = Sepeda("Polygon", 2023, 40, "gunung")

avanza.info()
# Toyota (2022) - Max: 160 km/h
#   Jumlah pintu: 4

mtb.info()
# Polygon (2023) - Max: 40 km/h
#   Jenis: gunung
```

### Encapsulation — Melindungi Data Internal

```python
class RekeningBank:
    def __init__(self, pemilik, pin, saldo=0):
        self.pemilik = pemilik          # Public: bebas diakses
        self._saldo = saldo             # Protected (_): konvensi "jangan langsung diakses"
        self.__pin = pin                # Private (__): benar-benar disembunyikan Python

    # PROPERTY: akses atribut seperti biasa, tapi ada logika di baliknya
    @property
    def saldo(self):
        """Getter: cara aman untuk membaca saldo."""
        return self.__format_saldo(self._saldo)

    @property
    def info_akun(self):
        return f"Pemilik: {self.pemilik} | Saldo: {self.saldo}"

    # SETTER: validasi sebelum mengubah nilai
    @saldo.setter
    def saldo(self, nilai_baru):
        if nilai_baru < 0:
            raise ValueError("Saldo tidak boleh negatif!")
        self._saldo = nilai_baru

    def verifikasi_pin(self, pin_input):
        # Satu-satunya cara akses __pin dari luar
        return self.__pin == pin_input

    def __format_saldo(self, jumlah):
        # Private method: hanya bisa dipakai di dalam class
        return f"Rp {jumlah:,.0f}"


akun = RekeningBank("Budi", pin="1234", saldo=1_000_000)

# Akses via property (bersih, terkontrol)
print(akun.saldo)        # Rp 1,000,000
print(akun.info_akun)    # Pemilik: Budi | Saldo: Rp 1,000,000

# Setter dengan validasi
akun.saldo = 2_000_000   # ✅ OK
# akun.saldo = -500_000  # ❌ ValueError!

# Private attribute tidak bisa diakses langsung dari luar
# print(akun.__pin)      # ❌ AttributeError!
print(akun.verifikasi_pin("1234"))  # True ← cara yang benar
```

### Cek Hubungan Inheritance

```python
print(isinstance(avanza, Mobil))      # True  ← avanza adalah Mobil
print(isinstance(avanza, Kendaraan))  # True  ← avanza JUGA adalah Kendaraan
print(isinstance(avanza, Sepeda))     # False

print(issubclass(Mobil, Kendaraan))   # True  ← Mobil adalah turunan Kendaraan
print(issubclass(Mobil, Sepeda))      # False
```

---

## 4. Mental Model & Best Practices

**Mental Model untuk tiga pilar:**
- **Inheritance** = *"Anak mewarisi sifat orang tua"* — Anjing adalah Hewan, jadi punya semua sifat Hewan.
- **Polymorphism** = *"Satu remote, banyak TV"* — tombol yang sama menghasilkan aksi berbeda tergantung TV mana yang dipegang.
- **Encapsulation** = *"Mesin mobil tertutup kap"* — kamu pakai pedal gas, tidak perlu tahu cara kerjanya di dalam.

```python
# ❌ BAD: Inheritance hanya untuk berbagi kode (bukan hubungan IS-A)
class Utilitas:
    def kirim_email(self): ...
    def format_tanggal(self): ...

class PenggunaBaru(Utilitas):  # Pengguna BUKAN utilitas!
    pass

# ✅ GOOD: Inheritance hanya jika memang ada hubungan IS-A
class Hewan:
    def bergerak(self): ...

class Kucing(Hewan):  # Kucing ADALAH Hewan ✅
    pass

# ✅ GOOD: Untuk berbagi kode tanpa hubungan IS-A, gunakan Composition
class Pengguna:
    def __init__(self):
        self.notifikasi = LayananNotifikasi()  # HAS-A, bukan IS-A

    def daftar(self):
        self.notifikasi.kirim_email("Selamat datang!")
```

---
---

# Topik 14: Magic/Dunder Methods

## 1. The Core Truth (First Principles)

Pernahkah kamu bertanya-tanya kenapa `print(daftar)` menampilkan isi list dengan rapi? Atau kenapa kamu bisa menulis `list1 + list2`? Semua itu bukan keajaiban — Python memanggil **method khusus tersembunyi** yang ada di balik operasi tersebut.

**Dunder Methods** (double underscore = `__nama__`) adalah *kait (hooks)* yang Python sediakan agar class buatan kamu bisa berperilaku seperti tipe data bawaan Python. Ini adalah cara Python mengimplementasikan **operator overloading** dan **protokol bawaan**.

---

## 2. The Basic Anatomy

```python
class Titik:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    # Dipanggil oleh print() dan str()
    def __str__(self):
        return f"Titik({self.x}, {self.y})"

    # Dipanggil di REPL dan repr() — lebih teknis/detail
    def __repr__(self):
        return f"Titik(x={self.x}, y={self.y})"


p = Titik(3, 4)
print(p)    # Titik(3, 4)    ← memanggil __str__
print(repr(p))  # Titik(x=3, y=4) ← memanggil __repr__
```

---

## 3. Deep Dive & Mechanisms

### Operator Overloading

```python
class Vektor:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"({self.x}, {self.y})"

    # Dipanggil saat: vektor1 + vektor2
    def __add__(self, other):
        return Vektor(self.x + other.x, self.y + other.y)

    # Dipanggil saat: vektor1 - vektor2
    def __sub__(self, other):
        return Vektor(self.x - other.x, self.y - other.y)

    # Dipanggil saat: vektor * angka
    def __mul__(self, skalar):
        return Vektor(self.x * skalar, self.y * skalar)

    # Dipanggil saat: len(vektor)
    def __len__(self):
        import math
        return int(math.sqrt(self.x**2 + self.y**2))

    # Dipanggil saat: vektor1 == vektor2
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y


v1 = Vektor(1, 2)
v2 = Vektor(3, 4)

print(v1 + v2)   # (4, 6)  ← __add__
print(v2 - v1)   # (2, 2)  ← __sub__
print(v1 * 3)    # (3, 6)  ← __mul__
print(len(v2))   # 5       ← __len__
print(v1 == Vektor(1, 2))  # True ← __eq__
```

### Dunder untuk Koleksi — Buat Class seperti List/Dict

```python
class PlaylistMusik:
    def __init__(self, nama):
        self.nama = nama
        self._lagu = []

    # Dipanggil saat: len(playlist)
    def __len__(self):
        return len(self._lagu)

    # Dipanggil saat: playlist[0]
    def __getitem__(self, index):
        return self._lagu[index]

    # Dipanggil saat: for lagu in playlist
    def __iter__(self):
        return iter(self._lagu)

    # Dipanggil saat: "lagu" in playlist
    def __contains__(self, lagu):
        return lagu in self._lagu

    def tambah(self, lagu):
        self._lagu.append(lagu)

    def __str__(self):
        return f"Playlist '{self.nama}' ({len(self)} lagu)"


playlist = PlaylistMusik("Favorit")
playlist.tambah("Bohemian Rhapsody")
playlist.tambah("Stairway to Heaven")
playlist.tambah("Hotel California")

print(playlist)          # Playlist 'Favorit' (3 lagu)
print(len(playlist))     # 3
print(playlist[0])       # Bohemian Rhapsody

for lagu in playlist:    # Bisa di-iterasi
    print(f"  🎵 {lagu}")

print("Hotel California" in playlist)  # True
```

### Context Manager Dunder (`__enter__` & `__exit__`)

```python
class Timer:
    """Mengukur waktu eksekusi sebuah blok kode."""
    import time

    def __enter__(self):
        # Dipanggil saat masuk blok "with"
        import time
        self._mulai = time.time()
        return self  # Nilai yang diikat oleh "as"

    def __exit__(self, exc_type, exc_val, exc_tb):
        # Dipanggil saat keluar blok "with" (berhasil ATAU error)
        import time
        durasi = time.time() - self._mulai
        print(f"Selesai dalam {durasi:.4f} detik")
        return False  # False = jangan suppress exception jika ada


with Timer() as t:
    total = sum(range(1_000_000))

# Selesai dalam 0.0312 detik
```

### Tabel Dunder Methods Paling Penting

| Dunder | Dipanggil Saat | Contoh |
|---|---|---|
| `__init__` | Object dibuat | `Titik(3, 4)` |
| `__str__` | `print(obj)` | `print(p)` |
| `__repr__` | `repr(obj)` atau di REPL | `repr(p)` |
| `__len__` | `len(obj)` | `len(playlist)` |
| `__add__` | `obj + obj2` | `v1 + v2` |
| `__eq__` | `obj == obj2` | `v1 == v2` |
| `__lt__` | `obj < obj2` | `v1 < v2` |
| `__getitem__` | `obj[key]` | `playlist[0]` |
| `__contains__` | `x in obj` | `"lagu" in playlist` |
| `__iter__` | `for x in obj` | Looping object |
| `__enter__` | Masuk `with` | `with Timer():` |
| `__exit__` | Keluar `with` | Setelah blok with |

---

## 4. Mental Model & Best Practices

**Mental Model:** Dunder methods adalah **soket listrik** — Python menyediakan colokan standar (`+`, `len()`, `print()`, `in`, dll). Class kamu tinggal **menancapkan kabel** (`__add__`, `__len__`, `__str__`, `__contains__`) agar bisa terhubung ke sistem Python.

```python
# ❌ BAD: Selalu definisikan __repr__ dan __str__ berbeda
# Jika kamu HANYA bisa buat satu, buat __repr__
# Karena jika __str__ tidak ada, Python fallback ke __repr__

class Produk:
    def __init__(self, nama, harga):
        self.nama = nama
        self.harga = harga
    # ❌ Tanpa __str__ / __repr__:
    # print(Produk("Laptop", 15000000)) → <__main__.Produk object at 0x...>

# ✅ GOOD: Selalu definisikan minimal __repr__
class Produk:
    def __init__(self, nama, harga):
        self.nama = nama
        self.harga = harga

    def __repr__(self):
        # Format yang bisa di-copy-paste untuk membuat ulang object
        return f"Produk(nama='{self.nama}', harga={self.harga})"

    def __str__(self):
        # Format yang ramah untuk pengguna
        return f"{self.nama} - Rp {self.harga:,}"
```

---
---

# Topik 15: Context Managers & `with` Statement

## 1. The Core Truth (First Principles)

Bayangkan kamu meminjam buku dari perpustakaan. Ada dua hal yang **wajib** terjadi: meminjam saat mulai baca, dan **mengembalikan** setelah selesai — tidak peduli apakah kamu selesai baca atau bukunya terbakar di tengah jalan.

Di programming, banyak resource seperti **file, koneksi database, network socket** yang wajib "dikembalikan" (ditutup/dilepas) setelah digunakan. Jika tidak, terjadi **resource leak** — program makan memori terus sampai crash.

**Context Manager** adalah mekanisme yang menjamin: *"Buka resource → Kerjakan → Tutup resource — apapun yang terjadi."*

---

## 2. The Basic Anatomy

```python
# TANPA context manager — berbahaya
file = open("data.txt", "w")
file.write("Halo!")
# ❌ Jika ada error di sini, file.close() tidak pernah dipanggil!
file.close()

# DENGAN context manager — aman dan bersih
with open("data.txt", "w") as file:
    file.write("Halo!")
# ✅ file.close() dipanggil OTOMATIS saat keluar blok with
# Bahkan jika ada exception sekalipun!
```

---

## 3. Deep Dive & Mechanisms

### Operasi File yang Lengkap

```python
# MENULIS file
with open("catatan.txt", "w", encoding="utf-8") as f:
    f.write("Baris pertama\n")
    f.write("Baris kedua\n")
    f.writelines(["Baris ketiga\n", "Baris keempat\n"])

# MEMBACA seluruh isi
with open("catatan.txt", "r", encoding="utf-8") as f:
    isi = f.read()
    print(isi)

# MEMBACA baris per baris (hemat memori untuk file besar)
with open("catatan.txt", "r", encoding="utf-8") as f:
    for nomor, baris in enumerate(f, start=1):
        print(f"{nomor}: {baris.strip()}")

# MENAMBAH isi (append) tanpa menghapus yang sudah ada
with open("catatan.txt", "a", encoding="utf-8") as f:
    f.write("Baris tambahan\n")
```

### Mode File — Tabel Referensi

| Mode | Arti | File tidak ada |
|---|---|---|
| `"r"` | Baca saja | ❌ Error |
| `"w"` | Tulis (timpa isi lama) | ✅ Buat baru |
| `"a"` | Append (tambah di akhir) | ✅ Buat baru |
| `"r+"` | Baca + tulis | ❌ Error |
| `"rb"` | Baca mode binary | ❌ Error |
| `"wb"` | Tulis mode binary | ✅ Buat baru |

### Membuat Context Manager Sendiri dengan `contextlib`

```python
from contextlib import contextmanager

# Cara paling simpel membuat context manager: gunakan @contextmanager
@contextmanager
def koneksi_database(nama_db):
    """Simulasi koneksi database yang selalu ditutup dengan benar."""
    print(f"[DB] Membuka koneksi ke '{nama_db}'...")
    koneksi = {"db": nama_db, "aktif": True}  # Simulasi object koneksi

    try:
        yield koneksi  # ← Nilai yang diikat oleh "as"
        # Kode di dalam blok "with" berjalan di sini
    except Exception as e:
        print(f"[DB] Error: {e}. Melakukan rollback...")
        raise  # Re-raise exception agar tidak ditelan diam-diam
    finally:
        # SELALU berjalan — inilah jaminannya
        koneksi["aktif"] = False
        print(f"[DB] Koneksi ke '{nama_db}' ditutup.")


# Penggunaan
with koneksi_database("produksi") as db:
    print(f"[APP] Menggunakan database: {db['db']}")
    print(f"[APP] Koneksi aktif: {db['aktif']}")
    # Simulasi query...

# Output:
# [DB] Membuka koneksi ke 'produksi'...
# [APP] Menggunakan database: produksi
# [APP] Koneksi aktif: True
# [DB] Koneksi ke 'produksi' ditutup.
```

### Multiple Context Managers Sekaligus

```python
# Buka dua file sekaligus — keduanya otomatis ditutup
with open("input.txt", "r") as sumber, open("output.txt", "w") as tujuan:
    for baris in sumber:
        baris_besar = baris.upper()
        tujuan.write(baris_besar)

print("Konversi selesai, kedua file sudah ditutup.")
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Context Manager seperti **pegawai hotel yang profesional**: dia membukakan pintu kamar (setup), memastikan kamu nyaman selama menginap (blok with), dan memastikan kunci dikembalikan saat checkout (teardown) — tidak peduli apakah kamu checkout normal atau darurat.

```python
# ❌ BAD: Membuka resource tanpa jaminan penutupan
def proses_data(nama_file):
    f = open(nama_file)
    data = f.read()
    hasil = data.upper()   # Jika ini error, f tidak pernah ditutup!
    f.close()
    return hasil

# ✅ GOOD: Gunakan with untuk jaminan penutupan
def proses_data(nama_file):
    with open(nama_file, encoding="utf-8") as f:
        data = f.read()
    return data.upper()  # Aman: file sudah ditutup sebelum sampai sini

# ✅ GOOD: Tangani kemungkinan file tidak ada
def baca_konfigurasi(nama_file):
    try:
        with open(nama_file, encoding="utf-8") as f:
            return f.read()
    except FileNotFoundError:
        print(f"File '{nama_file}' tidak ditemukan, gunakan default.")
        return ""
```

---

# ✅ Summary & Checklist Fase 4

**Classes & Objects:**
- Class = blueprint, Object = hasil cetakannya
- `__init__` adalah constructor — otomatis dipanggil saat object dibuat
- `self` = referensi ke object itu sendiri, selalu parameter pertama
- Bedakan **atribut instance** (milik tiap object) vs **atribut class** (milik semua)

**Inheritance, Polymorphism, Encapsulation:**
- Inheritance hanya untuk hubungan **IS-A** yang nyata, bukan sekadar berbagi kode
- Selalu panggil `super().__init__()` di constructor class anak
- `_atribut` = protected (konvensi), `__atribut` = private (di-mangle Python)
- Gunakan `@property` untuk akses atribut yang terkontrol

**Dunder Methods:**
- Dunder adalah kait untuk mengintegrasikan class dengan sintaks Python
- Selalu definisikan minimal `__repr__` agar object mudah di-debug
- Gunakan `__str__` untuk tampilan ramah pengguna, `__repr__` untuk developer

**Context Managers:**
- Gunakan `with` untuk **semua** operasi yang butuh cleanup (file, DB, lock)
- `with open(...) as f:` adalah penggunaan paling umum
- Buat context manager sendiri dengan `@contextmanager` dari `contextlib`
- Jaminan: blok `finally` di balik layar **selalu** berjalan

---

> 🎯 **Fase 4 selesai!** Ini adalah fase terberat — kalau kamu sampai di sini, kamu sudah berpikir seperti seorang software engineer. Fase 5 adalah **sihir kelas berat** yang membedakan programmer biasa dari yang senior. Ketik **"lanjut Fase 5"** jika sudah siap!
