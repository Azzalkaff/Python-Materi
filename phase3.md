# 🏗️ FASE 3 — Berpikir ala Python (Menengah Atas)

> **Misi Fase ini:** Menulis kode yang tidak hanya *benar*, tapi juga **elegan, efisien, dan Pythonic**. Ini adalah fase di mana kode kamu mulai terlihat seperti ditulis oleh programmer berpengalaman.

---

# Topik 8: Comprehensions (List, Dict, Set)

## 1. The Core Truth (First Principles)

Pola ini sangat umum dalam programming:
1. Ambil sebuah koleksi data
2. Proses atau filter setiap elemennya
3. Simpan hasilnya ke koleksi baru

Tanpa comprehension, kamu butuh 4-5 baris untuk ini. Python menyediakan cara untuk melakukannya dalam **satu baris yang ekspresif** — bukan sekadar mempersingkat, tapi membuat *niat kode lebih mudah dibaca*.

---

## 2. The Basic Anatomy

```python
angka = [1, 2, 3, 4, 5]

# CARA LAMA (imperatif) — "bagaimana" caranya
kuadrat = []
for n in angka:
    kuadrat.append(n ** 2)

# CARA PYTHONIC (deklaratif) — "apa" yang diinginkan
kuadrat = [n ** 2 for n in angka]

print(kuadrat)  # [1, 4, 9, 16, 25]
```

> 💡 **Pola membacanya:** *"Buat list berisi `n ** 2` untuk setiap `n` di dalam `angka`"* — persis seperti kalimat matematika.

---

## 3. Deep Dive & Mechanisms

### List Comprehension + Filter (If)

```python
angka = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Ambil hanya angka genap
genap = [n for n in angka if n % 2 == 0]
print(genap)  # [2, 4, 6, 8, 10]

# Kuadratkan, tapi hanya yang genap
kuadrat_genap = [n ** 2 for n in angka if n % 2 == 0]
print(kuadrat_genap)  # [4, 16, 36, 64, 100]

# Transformasi teks
nama = ["  budi  ", "  SITI  ", "andi"]
nama_bersih = [n.strip().title() for n in nama]
print(nama_bersih)  # ['Budi', 'Siti', 'Andi']
```

### If-Else di dalam Comprehension

```python
angka = [1, 2, 3, 4, 5, 6]

# If-else untuk transformasi (bukan filter)
# Pola: [nilai_jika_true if kondisi else nilai_jika_false for item in koleksi]
label = ["genap" if n % 2 == 0 else "ganjil" for n in angka]
print(label)  # ['ganjil', 'genap', 'ganjil', 'genap', 'ganjil', 'genap']

# Contoh nyata: normalisasi nilai ujian
nilai = [45, 82, 55, 91, 30, 70]
status = ["lulus" if n >= 60 else "remedial" for n in nilai]
print(status)  # ['remedial', 'lulus', 'remedial', 'lulus', 'remedial', 'lulus']
```

### Dictionary Comprehension

```python
# Buat dictionary dari dua list
nama_list = ["Budi", "Siti", "Andi"]
nilai_list = [85, 92, 78]

# zip() menggabungkan dua list menjadi pasangan
rapor = {nama: nilai for nama, nilai in zip(nama_list, nilai_list)}
print(rapor)  # {'Budi': 85, 'Siti': 92, 'Andi': 78}

# Transformasi dictionary yang sudah ada
harga_asli = {"apel": 5000, "mangga": 8000, "jeruk": 6000}

# Buat dictionary baru dengan diskon 10%
harga_diskon = {item: harga * 0.9 for item, harga in harga_asli.items()}
print(harga_diskon)  # {'apel': 4500.0, 'mangga': 7200.0, 'jeruk': 5400.0}

# Filter: hanya item yang harga aslinya di atas 5000
harga_mahal = {item: harga for item, harga in harga_asli.items() if harga > 5000}
print(harga_mahal)  # {'mangga': 8000, 'jeruk': 6000}
```

### Set Comprehension

```python
kalimat = "the quick brown fox jumps over the lazy dog"

# Ambil semua huruf unik yang digunakan (set otomatis hapus duplikat)
huruf_unik = {huruf for huruf in kalimat if huruf != " "}
print(len(huruf_unik))  # 26 — semua huruf alfabet ada!

# Angka kuadrat yang unik
angka = [1, -1, 2, -2, 3, -3]
kuadrat_unik = {n ** 2 for n in angka}
print(kuadrat_unik)  # {1, 4, 9} — duplikat dihapus otomatis
```

### ⚠️ Nested Comprehension — Gunakan dengan Bijak

```python
# Matrix 3x3
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# Ratakan matrix menjadi list 1D
# Cara baca: "n untuk setiap baris di matrix, untuk setiap n di baris"
rata = [n for baris in matrix for n in baris]
print(rata)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Jika terlalu kompleks, jangan dipaksakan jadi comprehension!
# Keterbacaan > keringkasan
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Baca comprehension seperti kalimat matematika himpunan: `{x² | x ∈ bilangan, x genap}` → `[x**2 for x in bilangan if x % 2 == 0]`.

```python
# ❌ BAD: Comprehension yang terlalu kompleks — susah dibaca
hasil = [f(x) for x in data if g(x) and h(x) for y in x if y > 0]

# ✅ GOOD: Jika logikanya rumit, gunakan for loop biasa yang jelas
hasil = []
for x in data:
    if g(x) and h(x):
        for y in x:
            if y > 0:
                hasil.append(f(x))

# ❌ BAD: Comprehension dengan side effects (efek samping)
# Comprehension bukan tempat untuk print atau mengubah state luar
[print(x) for x in data]  # Anti-pattern!

# ✅ GOOD: Gunakan for loop jika tujuannya adalah efek samping
for x in data:
    print(x)
```

**Panduan simpel:**
- Satu kondisi, satu transformasi → **pakai comprehension**
- Logika kompleks atau side effects → **pakai for loop biasa**

---
---

# Topik 9: Variable Scope (LEGB Rule)

## 1. The Core Truth (First Principles)

Bayangkan kamu bekerja di kantor berlantai banyak. Ada **dokumen pribadi di meja kamu** (hanya kamu yang bisa akses), **dokumen tim di lantai kamu** (satu tim bisa akses), dan **peraturan perusahaan di lobby** (semua orang bisa akses).

**Scope** adalah aturan yang menentukan: *"Di mana sebuah variable bisa dilihat dan digunakan?"* Tanpa aturan ini, semua variable akan saling tumpang tindih dan program menjadi chaos.

Python menggunakan aturan **LEGB** untuk menentukan variable mana yang dimaksud saat ada nama yang sama.

---

## 2. The Basic Anatomy

```python
x = "global"       # Scope: Global (bisa diakses di mana saja)

def fungsi_luar():
    x = "enclosing"  # Scope: Enclosing (untuk fungsi di dalamnya)

    def fungsi_dalam():
        x = "local"    # Scope: Local (hanya di sini)
        print(x)       # "local" ← Python cari dari dalam ke luar

    fungsi_dalam()
    print(x)           # "enclosing"

fungsi_luar()
print(x)               # "global"
```

---

## 3. Deep Dive & Mechanisms

### Urutan Pencarian LEGB

Python mencari nama variable dalam urutan ini:

```
L → Local      : Di dalam fungsi saat ini
E → Enclosing  : Di fungsi pembungkus (jika ada)
G → Global     : Di level modul/file
B → Built-in   : Fungsi bawaan Python (print, len, range, dll)
```

```python
# Ilustrasi lengkap LEGB
nama = "Global"  # (G) Global


def luar():
    nama = "Enclosing"  # (E) Enclosing

    def dalam():
        nama = "Local"  # (L) Local
        print(nama)     # "Local" ← L ditemukan, berhenti cari

    def dalam_tanpa_local():
        # Tidak ada L → naik ke E
        print(nama)     # "Enclosing"

    dalam()
    dalam_tanpa_local()


def hanya_global():
    # Tidak ada L, tidak ada E → naik ke G
    print(nama)  # "Global"


luar()
hanya_global()
```

### Keyword `global` — Memodifikasi Variable Global

```python
# Tanpa global: assignment di dalam fungsi membuat variable LOCAL baru
counter = 0

def tambah_salah():
    counter = counter + 1  # ❌ UnboundLocalError!
    # Python bingung: counter di kiri adalah local (baru),
    # tapi counter di kanan belum punya nilai local

def tambah_benar():
    global counter         # Deklarasikan: "counter yang saya maksud adalah yang global"
    counter = counter + 1  # ✅ Sekarang memodifikasi yang global

tambah_benar()
tambah_benar()
print(counter)  # 2
```

> ⚠️ **Catatan:** Penggunaan `global` sebaiknya dihindari — ini membuat kode sulit di-debug. Solusi yang lebih baik adalah menggunakan return value atau class.

### Keyword `nonlocal` — Memodifikasi Variable Enclosing

```python
def buat_counter():
    hitung = 0  # Variable di scope Enclosing

    def tambah():
        nonlocal hitung   # Modifikasi variable di scope Enclosing
        hitung += 1
        return hitung

    return tambah

counter = buat_counter()
print(counter())  # 1
print(counter())  # 2
print(counter())  # 3
# Setiap panggilan "mengingat" nilai hitung sebelumnya — ini adalah Closure!
# (Akan dibahas lebih dalam di Fase 5)
```

### Built-in Scope — Jangan Ditimpa!

```python
# Built-in adalah nama bawaan Python: print, len, list, range, type, dll

# ❌ BERBAHAYA: Menimpa nama built-in
list = [1, 2, 3]   # Sekarang "list" bukan fungsi lagi, tapi sebuah list!
# list("hello")    # ❌ TypeError — fungsi list() sudah tertimpa!

# Jika sudah terlanjur, hapus variable lokal untuk "restore" built-in
del list
print(list("hello"))  # ✅ ['h', 'e', 'l', 'l', 'o'] — kembali normal

# ✅ GOOD: Jangan gunakan nama built-in sebagai nama variable
data_list = [1, 2, 3]     # Bukan "list"
panjang_str = len("halo") # Bukan "len"
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Bayangkan **boneka Rusia (Matryoshka)** — boneka terkecil (Local) ada di dalam boneka menengah (Enclosing), yang ada di dalam boneka besar (Global), yang ada di dalam boneka terbesar (Built-in). Python selalu mencari dari boneka **terkecil ke terbesar**.

```python
# ❌ BAD: Mengandalkan global variable untuk berbagi state
total = 0

def tambah_item(harga):
    global total
    total += harga  # Side effect tersembunyi — susah di-test!

# ✅ GOOD: Gunakan parameter dan return value
def tambah_item(total_saat_ini, harga):
    return total_saat_ini + harga  # Murni, bisa diprediksi

total = 0
total = tambah_item(total, 50000)
total = tambah_item(total, 30000)
print(total)  # 80000
```

---
---

# Topik 10: `*args` dan `**kwargs`

## 1. The Core Truth (First Principles)

Kadang kamu tidak tahu **berapa banyak argumen** yang akan diberikan ke sebuah fungsi. Bayangkan membuat fungsi `penjumlahan()` — apakah untuk 2 angka? 5? 100?

Memaksa pengguna selalu memberikan jumlah argumen yang tepat adalah kaku dan tidak fleksibel. Python menyediakan `*args` dan `**kwargs` sebagai cara untuk membuat fungsi yang **menerima argumen dalam jumlah berapa pun**.

---

## 2. The Basic Anatomy

```python
# *args: kumpulkan semua positional argument menjadi TUPLE
def jumlahkan(*args):
    print(args)         # (1, 2, 3, 4, 5) ← tuple
    return sum(args)

print(jumlahkan(1, 2))           # 3
print(jumlahkan(1, 2, 3, 4, 5))  # 15


# **kwargs: kumpulkan semua keyword argument menjadi DICTIONARY
def perkenalan(**kwargs):
    print(kwargs)  # {'nama': 'Budi', 'kota': 'Jakarta'} ← dict
    for key, value in kwargs.items():
        print(f"{key}: {value}")

perkenalan(nama="Budi", kota="Jakarta", umur=25)
```

---

## 3. Deep Dive & Mechanisms

### Kombinasi Parameter Biasa + `*args` + `**kwargs`

```python
# Urutan parameter wajib: biasa → *args → **kwargs
def buat_laporan(judul, *poin, **metadata):
    print(f"=== {judul} ===")

    for i, poin_item in enumerate(poin, 1):
        print(f"  {i}. {poin_item}")

    if metadata:
        print("--- Info ---")
        for key, value in metadata.items():
            print(f"  {key}: {value}")

buat_laporan(
    "Laporan Penjualan",          # → judul (positional biasa)
    "Pendapatan naik 20%",        # → poin (masuk *args)
    "Target Q4 tercapai",         # → poin (masuk *args)
    "Ada peluang ekspansi",       # → poin (masuk *args)
    penulis="Budi",               # → metadata (masuk **kwargs)
    tanggal="2024-01-15"          # → metadata (masuk **kwargs)
)

# Output:
# === Laporan Penjualan ===
#   1. Pendapatan naik 20%
#   2. Target Q4 tercapai
#   3. Ada peluang ekspansi
# --- Info ---
#   penulis: Budi
#   tanggal: 2024-01-15
```

### Unpacking `*` dan `**` saat Memanggil Fungsi

```python
# * untuk unpack list/tuple menjadi positional arguments
def tambah(a, b, c):
    return a + b + c

angka = [10, 20, 30]
print(tambah(*angka))   # Sama dengan tambah(10, 20, 30) → 60


# ** untuk unpack dictionary menjadi keyword arguments
def buat_user(nama, umur, kota):
    return f"{nama}, {umur} tahun, dari {kota}"

data = {"nama": "Siti", "umur": 28, "kota": "Bandung"}
print(buat_user(**data))  # Siti, 28 tahun, dari Bandung


# Sangat berguna untuk menggabungkan dua dictionary (Python 3.9+)
default_config = {"debug": False, "timeout": 30, "retries": 3}
custom_config = {"timeout": 60, "verbose": True}

config = {**default_config, **custom_config}
# Key yang sama: custom_config menimpa default_config
print(config)
# {'debug': False, 'timeout': 60, 'retries': 3, 'verbose': True}
```

### Contoh Nyata: Decorator dan Wrapper

```python
# *args dan **kwargs paling sering dipakai di wrapper function
# agar wrapper bisa meneruskan SEMUA argumen ke fungsi asli

def logger(fungsi):
    """Wrapper yang mencatat setiap pemanggilan fungsi."""
    def wrapper(*args, **kwargs):
        print(f"Memanggil: {fungsi.__name__}")
        hasil = fungsi(*args, **kwargs)  # Teruskan semua argumen
        print(f"Selesai: {fungsi.__name__}")
        return hasil
    return wrapper

@logger
def kirim_email(tujuan, subjek, isi=""):
    print(f"Email ke {tujuan}: {subjek}")

kirim_email("budi@mail.com", "Halo!", isi="Apa kabar?")
# Memanggil: kirim_email
# Email ke budi@mail.com: Halo!
# Selesai: kirim_email
```

---

## 4. Mental Model & Best Practices

**Mental Model:**
- `*args` = **ember** yang menampung semua bola (positional args) yang dilempar
- `**kwargs` = **koper** berlabel yang menampung semua barang bernama (keyword args)

```python
# ❌ BAD: Terlalu bergantung pada *args sehingga interface tidak jelas
def proses(*args):
    # Pengguna tidak tahu harus kirim apa!
    nama = args[0]
    umur = args[1]

# ✅ GOOD: Parameter eksplisit jika jumlahnya sudah diketahui
def proses(nama, umur):
    pass  # Jelas dan self-documenting

# ✅ GOOD: Gunakan *args hanya jika memang variadic (jumlah tidak pasti)
def log(*pesan):
    for p in pesan:
        print(f"[LOG] {p}")

log("Server started")
log("Connected to DB", "Cache warmed up", "Ready to serve")
```

---
---

# Topik 11: Lambda & Higher-Order Functions

## 1. The Core Truth (First Principles)

Di Python, **fungsi adalah objek** — sama seperti integer atau string. Ini berarti fungsi bisa:
- Disimpan dalam variable
- Dimasukkan sebagai argumen ke fungsi lain
- Dikembalikan dari fungsi lain

Fungsi yang menerima atau mengembalikan fungsi lain disebut **Higher-Order Function**. Konsep ini memungkinkan pola yang sangat powerful: kamu bisa mengubah *perilaku* suatu operasi hanya dengan mengganti fungsi yang diberikan.

**Lambda** adalah cara membuat fungsi kecil tanpa nama secara cepat — seperti catatan tempel vs. dokumen resmi.

---

## 2. The Basic Anatomy

```python
# Fungsi biasa
def kuadrat(x):
    return x ** 2

# Lambda — fungsi tanpa nama, satu ekspresi
kuadrat_lambda = lambda x: x ** 2

print(kuadrat(5))        # 25
print(kuadrat_lambda(5)) # 25 — hasil sama
```

> 💡 **Lambda hanya untuk ekspresi satu baris.** Jika butuh lebih dari satu baris atau ada logika kompleks, gunakan `def` biasa.

---

## 3. Deep Dive & Mechanisms

### `sorted()` dengan Key Function

```python
# sorted() menerima parameter "key" — fungsi yang menentukan cara urut
produk = [
    {"nama": "Laptop", "harga": 15_000_000},
    {"nama": "Mouse", "harga": 250_000},
    {"nama": "Monitor", "harga": 3_500_000},
]

# Urutkan berdasarkan harga (dari murah ke mahal)
terurut = sorted(produk, key=lambda p: p["harga"])
for p in terurut:
    print(f"{p['nama']}: {p['harga']:,}")

# Mouse: 250,000
# Monitor: 3,500,000
# Laptop: 15,000,000

# Urutkan berdasarkan panjang nama
kata = ["banana", "apel", "kiwi", "semangka", "mangga"]
print(sorted(kata, key=lambda s: len(s)))
# ['apel', 'kiwi', 'banana', 'mangga', 'semangka']

# Urutkan dari mahal ke murah (reverse=True)
termahal = sorted(produk, key=lambda p: p["harga"], reverse=True)
```

### `map()` — Terapkan Fungsi ke Setiap Elemen

```python
angka = [1, 2, 3, 4, 5]

# map(fungsi, iterable) — terapkan fungsi ke setiap elemen
hasil = list(map(lambda x: x ** 2, angka))
print(hasil)  # [1, 4, 9, 16, 25]

# Contoh nyata: konversi tipe data
data_string = ["1", "2", "3", "4", "5"]
data_int = list(map(int, data_string))  # int adalah fungsi juga!
print(data_int)   # [1, 2, 3, 4, 5]
print(sum(data_int))  # 15

# Terapkan ke beberapa list sekaligus
a = [1, 2, 3]
b = [10, 20, 30]
hasil = list(map(lambda x, y: x + y, a, b))
print(hasil)  # [11, 22, 33]
```

### `filter()` — Saring Elemen Berdasarkan Kondisi

```python
angka = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# filter(fungsi, iterable) — simpan hanya yang fungsi-nya return True
genap = list(filter(lambda x: x % 2 == 0, angka))
print(genap)  # [2, 4, 6, 8, 10]

# Contoh nyata: filter produk yang stoknya ada
produk = [
    {"nama": "A", "stok": 5},
    {"nama": "B", "stok": 0},
    {"nama": "C", "stok": 3},
    {"nama": "D", "stok": 0},
]

tersedia = list(filter(lambda p: p["stok"] > 0, produk))
print([p["nama"] for p in tersedia])  # ['A', 'C']
```

### `reduce()` — Akumulasi Menjadi Satu Nilai

```python
from functools import reduce

angka = [1, 2, 3, 4, 5]

# reduce(fungsi, iterable) — akumulasi dari kiri ke kanan
# Langkah: ((((1+2)+3)+4)+5) = 15
total = reduce(lambda acc, x: acc + x, angka)
print(total)  # 15

# Tapi untuk kasus ini, sum() lebih baik!
# reduce() berguna untuk operasi yang tidak punya built-in

# Contoh: cari nilai maksimum secara manual
maks = reduce(lambda a, b: a if a > b else b, angka)
print(maks)  # 5

# Contoh: gabungkan list of lists menjadi satu list
nested = [[1, 2], [3, 4], [5, 6]]
flat = reduce(lambda acc, lst: acc + lst, nested)
print(flat)  # [1, 2, 3, 4, 5, 6]
```

### Fungsi sebagai Argumen — Higher-Order Function Buatan Sendiri

```python
# Kamu bisa membuat higher-order function sendiri
def terapkan_operasi(angka_list, operasi):
    """Terapkan fungsi 'operasi' ke setiap elemen."""
    return [operasi(x) for x in angka_list]

angka = [1, 2, 3, 4, 5]

# Kirim fungsi berbeda untuk perilaku berbeda
print(terapkan_operasi(angka, lambda x: x * 2))    # [2, 4, 6, 8, 10]
print(terapkan_operasi(angka, lambda x: x ** 3))   # [1, 8, 27, 64, 125]
print(terapkan_operasi(angka, str))                 # ['1', '2', '3', '4', '5']
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Lambda seperti **post-it note** — cepat ditulis untuk tugas kecil sekali pakai. Fungsi `def` seperti **dokumen resmi** — untuk hal yang penting, butuh nama, dan akan dipakai ulang.

| | `lambda` | `def` |
|---|---|---|
| Nama | Anonim | Punya nama |
| Panjang | Satu ekspresi | Berapa baris pun |
| Dokumentasi | ❌ Tidak bisa | ✅ Bisa (docstring) |
| Cocok untuk | Argumen fungsi sekali pakai | Semua kasus lain |

```python
# ❌ BAD: Lambda tersimpan di variable — tidak ada gunanya vs def
proses = lambda x, y: x * 2 + y  # Susah dibaca, tidak bisa didokumentasi

# ✅ GOOD: Gunakan def jika lambda disimpan sebagai variable
def proses(x, y):
    """Gandakan x lalu tambah y."""
    return x * 2 + y

# ✅ GOOD: Lambda tepat dipakai sebagai argumen fungsi lain
data = [3, 1, 4, 1, 5, 9]
data.sort(key=lambda x: -x)  # Urutkan descending
print(data)  # [9, 5, 4, 3, 1, 1]

# ❌ BAD: Nested lambda — sangat susah dibaca
f = lambda x: lambda y: x + y

# ✅ GOOD: Gunakan def untuk closure seperti ini
def tambahkan(x):
    def dengan(y):
        return x + y
    return dengan

tambah5 = tambahkan(5)
print(tambah5(3))  # 8
```

---

# ✅ Summary & Checklist Fase 3

**Comprehensions:**
- Gunakan untuk transformasi dan filter koleksi data secara ekspresif
- Pola: `[ekspresi for item in koleksi if kondisi]`
- Jika logika terlalu rumit → kembalikan ke `for` loop biasa
- Tersedia untuk: List `[]`, Dictionary `{}`, Set `{}`

**Variable Scope (LEGB):**
- Python mencari variable dari **L**ocal → **E**nclosing → **G**lobal → **B**uilt-in
- Hindari `global` — lebih baik pakai `return` dan parameter
- Jangan timpa nama built-in Python (`list`, `len`, `print`, dll)

**`*args` dan `**kwargs`:**
- `*args` = tangkap semua positional argument sebagai **tuple**
- `**kwargs` = tangkap semua keyword argument sebagai **dictionary**
- Urutan wajib: `def f(biasa, *args, **kwargs)`
- Gunakan `*list` dan `**dict` untuk unpack saat memanggil fungsi

**Lambda & Higher-Order Functions:**
- Fungsi di Python adalah **objek** — bisa dikirim sebagai argumen
- Lambda untuk fungsi kecil sekali pakai (terutama di `sorted`, `map`, `filter`)
- `map()` → transformasi, `filter()` → seleksi, `reduce()` → akumulasi
- Jika lambda disimpan ke variable → gunakan `def` biasa

---

> 🎯 **Fase 3 selesai!** Di Fase 4 kita masuk ke **Object-Oriented Programming** — cara berpikir yang benar-benar mengubah cara kamu membangun program skala besar. Ketik **"lanjut Fase 4"** jika sudah siap!
