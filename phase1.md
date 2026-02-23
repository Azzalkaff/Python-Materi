# 🏗️ FASE 1 — Membangun Fondasi (Dasar Logika)

> **Misi Fase ini:** Pahami bagaimana Python *berpikir* dan *membuat keputusan*. Semua program di dunia, sekompleks apapun, dibangun dari tiga fondasi ini.

---

# Topik 1: Variables & Primitive Data Types

## 1. The Core Truth (First Principles)

Bayangkan otak kamu sedang mengerjakan soal matematika: *"Jika harga apel 5000, dan kamu beli 3, berapa total?"* Kamu **menyimpan angka itu di memori otak** sementara menghitung.

Komputer bekerja persis sama. **Variable** adalah cara program menyimpan nilai di memori RAM agar bisa digunakan lagi nanti.

Tanpa variable, kamu harus mengetik ulang nilai yang sama berkali-kali — dan jika nilainya berubah, kamu harus ganti di 100 tempat sekaligus. Ini jelas tidak masuk akal.

**Primitive Data Types** adalah "jenis wadah" yang berbeda untuk jenis data yang berbeda — seperti wadah air vs. wadah beras. Python perlu tahu jenisnya agar tahu *operasi apa yang boleh dilakukan*.

---

## 2. The Basic Anatomy

Python punya **4 tipe data primitif** utama:

| Tipe | Nama | Contoh Nilai | Kegunaan |
|---|---|---|---|
| `int` | Integer | `10`, `-3`, `0` | Angka bulat |
| `float` | Float | `3.14`, `-0.5` | Angka desimal |
| `str` | String | `"halo"`, `'Python'` | Teks |
| `bool` | Boolean | `True`, `False` | Nilai benar/salah |

```python
# Cara membuat variable: nama = nilai
# Python otomatis tahu tipenya (ini disebut "dynamic typing")

umur = 25          # int  → angka bulat
tinggi = 170.5     # float → angka desimal
nama = "Budi"      # str  → teks, gunakan "" atau ''
is_student = True  # bool → hanya True atau False (kapital!)

# Cek tipe data dengan fungsi type()
print(type(umur))       # <class 'int'>
print(type(tinggi))     # <class 'float'>
print(type(nama))       # <class 'str'>
print(type(is_student)) # <class 'bool'>
```

---

## 3. Deep Dive & Mechanisms

### 🔢 Integer & Float — Operasi Matematika

```python
a = 10
b = 3

print(a + b)   # 13  → penjumlahan
print(a - b)   # 7   → pengurangan
print(a * b)   # 30  → perkalian
print(a / b)   # 3.3333... → pembagian (SELALU menghasilkan float)
print(a // b)  # 3   → pembagian bulat (buang desimal)
print(a % b)   # 1   → modulus (sisa bagi) — sangat berguna!
print(a ** b)  # 1000 → pangkat (10³)
```

> 💡 **Kenapa `%` (modulus) penting?** Untuk cek apakah angka genap/ganjil: `angka % 2 == 0` berarti genap.

### 📝 String — Teks dan Manipulasinya

```python
nama_depan = "Budi"
nama_belakang = "Santoso"

# Menggabungkan string (concatenation)
nama_lengkap = nama_depan + " " + nama_belakang
print(nama_lengkap)  # Budi Santoso

# f-string: cara TERBAIK dan MODERN untuk menyisipkan variable ke teks
umur = 25
perkenalan = f"Nama saya {nama_lengkap}, umur {umur} tahun."
print(perkenalan)  # Nama saya Budi Santoso, umur 25 tahun.

# Beberapa method string yang sering dipakai
teks = "  halo dunia  "
print(teks.upper())    # "  HALO DUNIA  "
print(teks.strip())    # "halo dunia" (hapus spasi di pinggir)
print(teks.replace("dunia", "Python"))  # "  halo Python  "
print(len("Python"))   # 6 (panjang string)
```

### ✅ Boolean — Logika Benar/Salah

```python
# Boolean sering dihasilkan dari operasi perbandingan
print(10 > 5)   # True
print(10 == 5)  # False  (== untuk cek kesamaan, bukan = untuk assign!)
print(10 != 5)  # True   (tidak sama dengan)

# Boolean bisa dikombinasikan
print(True and False)  # False (keduanya harus True)
print(True or False)   # True  (salah satu True sudah cukup)
print(not True)        # False (kebalikan)
```

### ⚠️ Edge Cases yang Wajib Tahu

```python
# 1. Konversi antar tipe (Type Casting)
angka_str = "42"
angka_int = int(angka_str)   # str → int
print(angka_int + 8)         # 50 (bisa dihitung sekarang)

# 2. Apa yang terjadi jika konversi gagal?
# int("halo")  # ❌ ValueError! "halo" tidak bisa jadi angka

# 3. Nilai yang dianggap False oleh Python (Falsy values)
print(bool(0))    # False
print(bool(""))   # False (string kosong)
print(bool(None)) # False (None = tidak ada nilai)

# 4. Multiple assignment — cara cepat Python
x, y, z = 1, 2, 3
print(x, y, z)   # 1 2 3
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Bayangkan variable sebagai **label tempel** yang ditempelkan ke sebuah kotak berisi nilai. Kamu bisa menempelkan label yang sama ke kotak yang berbeda kapan saja (itulah mengapa bisa `x = 5` lalu `x = "halo"` di Python).

**✅ Do's & Don'ts:**

```python
# ❌ BAD: Nama variable tidak jelas
x = "Budi"
n = 25
f = True

# ✅ GOOD: Nama variable deskriptif (snake_case untuk Python)
nama_pengguna = "Budi"
umur_pengguna = 25
is_aktif = True

# ❌ BAD: Gunakan + untuk template string yang panjang
pesan = "Halo " + nama_pengguna + ", kamu berumur " + str(umur_pengguna) + " tahun"

# ✅ GOOD: Gunakan f-string, lebih bersih dan mudah dibaca
pesan = f"Halo {nama_pengguna}, kamu berumur {umur_pengguna} tahun"
```

---
---

# Topik 2: Conditional Statements (If, Elif, Else)

## 1. The Core Truth (First Principles)

Setiap hari kamu membuat keputusan bercabang: *"Jika hujan → bawa payung. Jika tidak → pakai baju tipis."*

Program yang tidak bisa membuat keputusan hanya bisa melakukan satu hal yang sama selamanya — tidak berguna di dunia nyata. **Conditional statement** memberi program kemampuan untuk *memilih jalur eksekusi yang berbeda* berdasarkan kondisi.

---

## 2. The Basic Anatomy

```python
nilai = 75

if nilai >= 80:
    print("Lulus dengan baik")   # Hanya jalan jika kondisi True
elif nilai >= 60:
    print("Lulus")               # Dicek jika kondisi pertama False
else:
    print("Tidak lulus")         # Jalan jika SEMUA kondisi di atas False

# Output: Lulus
```

> 💡 **Indentasi (spasi 4) adalah WAJIB di Python.** Ini bukan gaya penulisan — ini sintaks! Tanpa indentasi, Python akan error.

---

## 3. Deep Dive & Mechanisms

### Struktur Lengkap

```python
saldo = 150_000  # underscore boleh di angka besar, untuk keterbacaan
harga = 200_000

if saldo >= harga:
    # Blok ini jalan jika saldo cukup
    saldo = saldo - harga
    print(f"Pembelian berhasil! Sisa saldo: {saldo}")
elif saldo >= harga * 0.5:
    # Blok ini jalan jika saldo setengah dari harga
    print("Saldo kurang, tapi lebih dari 50%. Mau cicil?")
else:
    # Blok ini jalan jika kedua kondisi di atas False
    kekurangan = harga - saldo
    print(f"Saldo tidak cukup. Kekurangan: {kekurangan}")

# Output: Saldo kurang, tapi lebih dari 50%. Mau cicil?
```

### Kondisi Majemuk dengan `and` / `or`

```python
umur = 20
punya_ktp = True

# Kedua kondisi harus terpenuhi
if umur >= 17 and punya_ktp:
    print("Boleh masuk")

# Salah satu kondisi cukup
diskon_pelajar = False
diskon_member = True

if diskon_pelajar or diskon_member:
    print("Dapat diskon!")  # Output: Dapat diskon!
```

### Nested If (If di dalam If)

```python
# Gunakan ini dengan hemat — terlalu dalam membuat kode susah dibaca
cuaca = "hujan"
punya_payung = False

if cuaca == "hujan":
    if punya_payung:
        print("Aman, bawa payung")
    else:
        print("Tunggu di dalam dulu")
else:
    print("Cuaca cerah, berangkat!")
```

### Ternary Expression — If singkat dalam satu baris

```python
# Untuk kondisi sederhana, bisa ditulis dalam satu baris
nilai = 75
status = "Lulus" if nilai >= 60 else "Tidak Lulus"
print(status)  # Lulus
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Bayangkan **pohon keputusan** — di setiap cabang ada pertanyaan ya/tidak, dan program mengikuti satu jalur dari atas ke bawah sampai menemukan kondisi yang `True`.

```python
# ❌ BAD: Membandingkan boolean dengan == True (redundant)
is_aktif = True
if is_aktif == True:
    print("aktif")

# ✅ GOOD: Boolean sudah True/False, langsung pakai
if is_aktif:
    print("aktif")

# ❌ BAD: Nested if yang bisa disederhanakan
if umur >= 18:
    if punya_ktp:
        print("Valid")

# ✅ GOOD: Gabung dengan and
if umur >= 18 and punya_ktp:
    print("Valid")
```

---
---

# Topik 3: Looping Mechanisms

## 1. The Core Truth (First Principles)

Bayangkan kamu diminta menulis "saya tidak akan mencontek" sebanyak 100 kali. Tanpa loop, kamu harus menulis `print(...)` sebanyak 100 baris. Itu tidak masuk akal.

**Loop** memberi program kemampuan untuk **mengulang blok kode** tanpa menulis ulang — prinsip dasarnya adalah *otomasi pengulangan*. Ini mungkin konsep paling sering digunakan dalam programming.

---

## 2. The Basic Anatomy

Python punya **2 jenis loop utama:**

| Loop | Dipakai Ketika... |
|---|---|
| `for` | Kamu tahu *berapa kali* atau *atas apa* harus diulang |
| `while` | Kamu ulang selama *kondisi tertentu* masih True |

```python
# FOR LOOP — ulang untuk setiap item di urutan
buah = ["apel", "mangga", "jeruk"]
for item in buah:
    print(item)
# apel
# mangga
# jeruk

# WHILE LOOP — ulang selama kondisi True
hitung = 1
while hitung <= 3:
    print(hitung)
    hitung += 1  # WAJIB: update kondisi agar tidak infinite loop!
# 1
# 2
# 3
```

---

## 3. Deep Dive & Mechanisms

### For Loop dengan `range()`

```python
# range(stop)          → 0 sampai stop-1
# range(start, stop)   → start sampai stop-1
# range(start, stop, step) → dengan lompatan

for i in range(5):          # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 6):       # 1, 2, 3, 4, 5
    print(i)

for i in range(0, 11, 2):   # 0, 2, 4, 6, 8, 10
    print(i)

# Countdown
for i in range(5, 0, -1):   # 5, 4, 3, 2, 1
    print(i)
```

### `enumerate()` — Loop dengan Index

```python
menu = ["Nasi Goreng", "Mie Ayam", "Soto"]

# Tanpa enumerate — cara lama dan kurang Pythonic
for i in range(len(menu)):
    print(f"{i+1}. {menu[i]}")

# Dengan enumerate — cara Python yang benar
for nomor, makanan in enumerate(menu, start=1):
    print(f"{nomor}. {makanan}")

# Output keduanya sama:
# 1. Nasi Goreng
# 2. Mie Ayam
# 3. Soto
```

### `break` dan `continue` — Kontrol Alur Loop

```python
# BREAK: keluar dari loop sepenuhnya
print("=== BREAK ===")
for i in range(10):
    if i == 5:
        break       # Stop! Tidak lanjut ke 6, 7, 8, 9
    print(i)
# 0, 1, 2, 3, 4

# CONTINUE: skip iterasi ini, lanjut ke berikutnya
print("=== CONTINUE ===")
for i in range(6):
    if i == 3:
        continue    # Lewati angka 3, lanjut ke 4
    print(i)
# 0, 1, 2, 4, 5
```

> 💡 **Analogi:** `break` = keluar dari antrian. `continue` = skip giliran tapi tetap di antrian.

### While Loop & Bahaya Infinite Loop

```python
# Contoh while loop yang aman: simulasi login
max_percobaan = 3
percobaan = 0
password_benar = "python123"

while percobaan < max_percobaan:
    tebakan = input("Masukkan password: ")
    percobaan += 1

    if tebakan == password_benar:
        print("Login berhasil!")
        break
    else:
        sisa = max_percobaan - percobaan
        print(f"Salah! Sisa percobaan: {sisa}")
else:
    # Blok ini jalan jika while selesai TANPA break
    print("Akun terkunci!")
```

### Loop Bersarang (Nested Loop)

```python
# Gunakan untuk masalah yang memang 2 dimensi
# Contoh: membuat tabel perkalian
for baris in range(1, 4):      # 1, 2, 3
    for kolom in range(1, 4):  # 1, 2, 3
        hasil = baris * kolom
        print(f"{baris}x{kolom}={hasil}", end="  ")
    print()  # pindah baris

# Output:
# 1x1=1  1x2=2  1x3=3
# 2x1=2  2x2=4  2x3=6
# 3x1=3  3x2=6  3x3=9
```

---

## 4. Mental Model & Best Practices

**Mental Model untuk `for`:** Bayangkan **ban berjalan di pabrik** — setiap item lewat satu per satu, dan kamu melakukan aksi yang sama ke setiap item.

**Mental Model untuk `while`:** Bayangkan **lampu lalu lintas** — selama lampu merah, kamu terus menunggu. Begitu hijau (kondisi False), kamu jalan.

```python
# ❌ BAD: Menggunakan while ketika jumlah iterasi sudah diketahui
i = 0
while i < len(buah):
    print(buah[i])
    i += 1

# ✅ GOOD: Gunakan for yang lebih bersih
for item in buah:
    print(item)

# ❌ BAD: Modifikasi list saat sedang diiterasi (berbahaya!)
angka = [1, 2, 3, 4, 5]
for n in angka:
    if n % 2 == 0:
        angka.remove(n)  # Bisa skip elemen!

# ✅ GOOD: Iterasi dari copy, modifikasi yang asli
for n in angka[:]:  # angka[:] membuat copy
    if n % 2 == 0:
        angka.remove(n)
```

---

# ✅ 5. Summary & Checklist Fase 1

**Variables & Data Types:**
- Variable adalah label untuk menyimpan nilai di memori
- 4 tipe primitif: `int`, `float`, `str`, `bool`
- Gunakan **f-string** untuk menyisipkan variable ke teks
- Gunakan **nama deskriptif** dengan format `snake_case`

**Conditional Statements:**
- `if` → `elif` → `else` dibaca dari atas ke bawah, yang pertama `True` yang dieksekusi
- Gunakan `and` / `or` untuk menggabungkan kondisi
- Hindari nested if yang terlalu dalam

**Loops:**
- `for` untuk urutan yang diketahui, `while` untuk kondisi yang tidak pasti
- `break` = keluar loop, `continue` = skip iterasi ini
- Selalu pastikan `while` loop punya jalan keluar (update kondisi)
- Gunakan `enumerate()` saat butuh index di `for` loop

---

> 🎯 **Fase 1 selesai!** Ketik **"lanjut Fase 2"** jika sudah siap, atau tanyakan bagian mana yang ingin diperdalam dulu. Tidak ada terburu-buru.
