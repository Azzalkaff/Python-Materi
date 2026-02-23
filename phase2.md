# 🏗️ FASE 2 — Mengelola Data & Logika (Menengah)

> **Misi Fase ini:** Belajar menulis kode yang bisa **dipakai ulang** dan mengelola **kumpulan data** dengan efisien. Ini adalah fondasi dari hampir semua program nyata.

---

# Topik 4: Functions, Parameters, and Return Values

## 1. The Core Truth (First Principles)

Bayangkan kamu seorang koki. Setiap kali ada pesanan nasi goreng, apakah kamu membaca resep dari awal lagi? Tidak — kamu sudah **hafal prosedurnya** dan tinggal menjalankannya.

**Function** adalah cara mengemas sekumpulan instruksi dengan nama, agar bisa dipanggil berkali-kali tanpa menulis ulang. Tanpa function, kode akan penuh dengan **duplikasi** — dan duplikasi adalah musuh utama programmer.

Prinsipnya: **DRY — Don't Repeat Yourself.**

---

## 2. The Basic Anatomy

```python
# Struktur dasar function
def sapa(nama):           # def = define, "sapa" = nama function
    """Menyapa pengguna."""  # Docstring: dokumentasi function
    pesan = f"Halo, {nama}!"
    return pesan          # Mengembalikan nilai ke pemanggil

# Memanggil function
hasil = sapa("Budi")
print(hasil)  # Halo, Budi!
```

---

## 3. Deep Dive & Mechanisms

### Tiga Jenis Parameter

```python
# 1. POSITIONAL ARGUMENT — urutan penting
def kurangi(a, b):
    return a - b

print(kurangi(10, 3))   # 7  → a=10, b=3
print(kurangi(3, 10))   # -7 → a=3, b=10 (urutan berbeda, hasil berbeda!)


# 2. DEFAULT PARAMETER — nilai cadangan jika tidak diisi
def buat_profil(nama, kota="Jakarta"):  # kota punya nilai default
    return f"{nama} dari {kota}"

print(buat_profil("Budi"))           # Budi dari Jakarta
print(buat_profil("Siti", "Bandung")) # Siti dari Bandung


# 3. KEYWORD ARGUMENT — sebut nama parameternya, urutan bebas
def pesan_tiket(tujuan, kelas, jumlah):
    return f"{jumlah} tiket ke {tujuan}, kelas {kelas}"

# Dengan keyword, urutan tidak masalah
print(pesan_tiket(kelas="Bisnis", jumlah=2, tujuan="Bali"))
# 2 tiket ke Bali, kelas Bisnis
```

### Return Values

```python
# Function tanpa return → mengembalikan None secara implisit
def cetak_saja(teks):
    print(teks)

hasil = cetak_saja("halo")
print(hasil)   # None ← bukan error, tapi tidak ada nilai kembali


# Mengembalikan BANYAK nilai sekaligus
def hitung_statistik(angka_list):
    total = sum(angka_list)
    rata = total / len(angka_list)
    return total, rata  # Python membungkusnya sebagai tuple

total, rata_rata = hitung_statistik([10, 20, 30])
print(total)      # 60
print(rata_rata)  # 20.0


# Early return — keluar dari function lebih awal
def bagi(a, b):
    if b == 0:
        return "Error: tidak bisa bagi dengan nol"  # Keluar di sini
    return a / b  # Hanya dieksekusi jika b != 0

print(bagi(10, 2))  # 5.0
print(bagi(10, 0))  # Error: tidak bisa bagi dengan nol
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Function adalah **mesin kapsul** — kamu masukkan bahan (parameter), mesin bekerja di dalam, lalu mengeluarkan produk (return value). Kamu tidak perlu tahu cara kerjanya di dalam setiap saat.

```python
# ❌ BAD: Satu function melakukan terlalu banyak hal
def proses_pengguna(nama, umur, email):
    print(f"Nama: {nama}")
    print(f"Umur: {umur}")
    if "@" not in email:
        print("Email tidak valid")
    # ... 50 baris lagi

# ✅ GOOD: Satu function, satu tanggung jawab
def validasi_email(email):
    return "@" in email

def tampilkan_profil(nama, umur):
    print(f"Nama: {nama}, Umur: {umur}")

# ❌ BAD: Nama function tidak menjelaskan aksinya
def data(x):
    return x * 2

# ✅ GOOD: Nama function adalah kata kerja yang jelas
def gandakan_nilai(nilai):
    return nilai * 2
```

---
---

# Topik 5: Lists & Tuples (Mutable vs Immutable)

## 1. The Core Truth (First Principles)

Sebelumnya, kamu menyimpan **satu nilai** per variable. Tapi bagaimana jika kamu punya 100 nama siswa? Membuat 100 variable adalah mimpi buruk.

**List** adalah wadah yang bisa menyimpan **banyak nilai sekaligus** dalam satu variable, berurutan, dan bisa diubah-ubah. **Tuple** adalah versi List yang **dibekukan** — isinya tidak bisa diubah setelah dibuat.

---

## 2. The Basic Anatomy

```python
# LIST: gunakan [] — bisa diubah (mutable)
belanjaan = ["apel", "susu", "roti"]

# TUPLE: gunakan () — tidak bisa diubah (immutable)
koordinat = (106.8, -6.2)   # longitude, latitude Jakarta

print(belanjaan[0])   # "apel" ← index mulai dari 0!
print(koordinat[1])   # -6.2
```

---

## 3. Deep Dive & Mechanisms

### Operasi Penting pada List

```python
nilai_siswa = [85, 92, 78, 95, 60]

# AKSES elemen
print(nilai_siswa[0])    # 85  ← elemen pertama
print(nilai_siswa[-1])   # 60  ← elemen terakhir (index negatif!)
print(nilai_siswa[1:3])  # [92, 78] ← slicing: index 1 sampai 2

# MODIFIKASI
nilai_siswa.append(88)       # Tambah di akhir
nilai_siswa.insert(0, 100)   # Sisipkan di index 0
nilai_siswa.remove(60)       # Hapus nilai 60 (yang pertama ditemukan)
terhapus = nilai_siswa.pop() # Ambil dan hapus elemen terakhir

# INFORMASI
print(len(nilai_siswa))      # Jumlah elemen
print(max(nilai_siswa))      # Nilai terbesar
print(min(nilai_siswa))      # Nilai terkecil
print(sum(nilai_siswa))      # Total semua nilai

# SORTING
nilai_siswa.sort()             # Urutkan (ubah list asli)
terurut = sorted(nilai_siswa)  # Urutkan (buat list baru, asli aman)
```

### Slicing — Mengambil Sebagian List

```python
huruf = ["a", "b", "c", "d", "e"]
#  index:  0    1    2    3    4
# neg idx:-5   -4   -3   -2   -1

print(huruf[1:4])    # ['b', 'c', 'd'] → index 1 sampai 3
print(huruf[:3])     # ['a', 'b', 'c'] → dari awal sampai index 2
print(huruf[2:])     # ['c', 'd', 'e'] → dari index 2 sampai akhir
print(huruf[::-1])   # ['e', 'd', 'c', 'b', 'a'] → dibalik!
```

### Mutable vs Immutable — Perbedaan Krusial

```python
# LIST bisa diubah
nama_list = ["Budi", "Siti"]
nama_list[0] = "Andi"   # ✅ Berhasil
print(nama_list)        # ['Andi', 'Siti']

# TUPLE tidak bisa diubah
nama_tuple = ("Budi", "Siti")
# nama_tuple[0] = "Andi"  # ❌ TypeError!

# Kapan pakai Tuple?
# → Data yang tidak boleh berubah: koordinat GPS, RGB warna, pasangan kunci-nilai
MERAH = (255, 0, 0)       # Warna merah dalam RGB — tidak masuk akal jika berubah
JAKARTA = (106.8, -6.2)   # Koordinat kota — bukan untuk diubah
```

### List Bersarang (Matrix/2D)

```python
# List di dalam list — berguna untuk tabel atau grid
papan_catur = [
    ["R", "N", "B", "Q"],  # baris 0
    ["P", "P", "P", "P"],  # baris 1
    [" ", " ", " ", " "],  # baris 2
]

print(papan_catur[0][3])   # "Q" → baris 0, kolom 3
print(papan_catur[1][0])   # "P" → baris 1, kolom 0
```

---

## 4. Mental Model & Best Practices

**Mental Model:**
- **List** = daftar belanjaan di kertas — bisa ditambah, dihapus, dicoret, diganti.
- **Tuple** = data di kartu identitas — sekali dicetak, tidak berubah.

| Fitur | List `[]` | Tuple `()` |
|---|---|---|
| Bisa diubah | ✅ Ya | ❌ Tidak |
| Kecepatan | Lebih lambat | Lebih cepat |
| Kegunaan | Data dinamis | Data tetap/konstan |
| Bisa jadi dict key | ❌ Tidak | ✅ Ya |

```python
# ❌ BAD: Gunakan list untuk data yang tidak seharusnya berubah
posisi_kantor = [-6.2, 106.8]  # Bisa tidak sengaja diubah!

# ✅ GOOD: Gunakan tuple untuk data konstanta
POSISI_KANTOR = (-6.2, 106.8)  # Aman, tidak bisa diubah

# ❌ BAD: Mengecek keberadaan elemen di list yang besar (lambat)
data_besar = list(range(1_000_000))
print(999999 in data_besar)  # Lambat — Python cek satu per satu

# ✅ GOOD: Gunakan Set untuk pengecekan keberadaan (akan dibahas Topik 6)
```

---
---

# Topik 6: Dictionaries & Sets

## 1. The Core Truth (First Principles)

List bagus untuk menyimpan data berurutan, tapi punya masalah: untuk menemukan data, Python harus **memeriksa satu per satu dari awal**. Bayangkan mencari nomor telepon seseorang di buku yang tidak berurutan — kamu harus baca semua halaman.

**Dictionary** seperti buku telepon yang benar: kamu langsung cari berdasarkan **nama (key)**, dan langsung dapat **nomor (value)**. Ini disebut **Hash Table** — pencarian hampir seketika, tidak peduli ukuran datanya.

**Set** adalah Dictionary tanpa value — hanya kumpulan key yang unik, tanpa duplikat.

---

## 2. The Basic Anatomy

```python
# DICTIONARY: pasangan key → value, gunakan {}
profil = {
    "nama": "Budi",
    "umur": 25,
    "kota": "Jakarta"
}

# Akses nilai berdasarkan key
print(profil["nama"])   # Budi

# SET: kumpulan unik, tidak berurutan, gunakan {}
hobi = {"membaca", "coding", "membaca"}  # Duplikat otomatis dihapus
print(hobi)  # {'membaca', 'coding'} — hanya 2 elemen
```

---

## 3. Deep Dive & Mechanisms

### Operasi Dictionary

```python
mahasiswa = {
    "nama": "Siti",
    "nim": "12345",
    "ipk": 3.75
}

# AKSES — dua cara
print(mahasiswa["nama"])              # "Siti" — error jika key tidak ada
print(mahasiswa.get("jurusan"))       # None — aman, tidak error
print(mahasiswa.get("jurusan", "TI")) # "TI" — nilai default jika tidak ada

# TAMBAH & UBAH
mahasiswa["jurusan"] = "Informatika"  # Tambah key baru
mahasiswa["ipk"] = 3.80               # Ubah nilai yang ada

# HAPUS
del mahasiswa["nim"]                  # Hapus key + value
nilai = mahasiswa.pop("ipk")          # Hapus + ambil nilainya

# ITERASI
for key in mahasiswa:
    print(key)                        # Iterasi key saja

for key, value in mahasiswa.items():  # Iterasi key dan value
    print(f"{key}: {value}")

print(list(mahasiswa.keys()))         # Semua key
print(list(mahasiswa.values()))       # Semua value
```

### Dictionary Bersarang

```python
# Sangat umum di data nyata (JSON, API response, dll)
karyawan = {
    "E001": {
        "nama": "Budi",
        "divisi": "Engineering",
        "gaji": 15_000_000
    },
    "E002": {
        "nama": "Siti",
        "divisi": "Marketing",
        "gaji": 12_000_000
    }
}

# Akses data bersarang
print(karyawan["E001"]["nama"])    # Budi
print(karyawan["E002"]["gaji"])    # 12000000

# Iterasi dictionary bersarang
for id_karyawan, data in karyawan.items():
    print(f"{id_karyawan}: {data['nama']} - {data['divisi']}")
```

### Set — Kumpulan Unik & Operasi Himpunan

```python
tim_a = {"Budi", "Siti", "Andi"}
tim_b = {"Siti", "Andi", "Rudi"}

# Operasi himpunan klasik
print(tim_a | tim_b)   # Union: semua anggota          {'Budi','Siti','Andi','Rudi'}
print(tim_a & tim_b)   # Intersection: anggota bersama {'Siti', 'Andi'}
print(tim_a - tim_b)   # Difference: hanya di tim_a    {'Budi'}

# Kegunaan praktis set: hapus duplikat dari list
angka_duplikat = [1, 2, 2, 3, 3, 3, 4]
unik = list(set(angka_duplikat))
print(unik)  # [1, 2, 3, 4]

# Pengecekan keanggotaan — jauh lebih cepat dari list
email_terdaftar = {"budi@mail.com", "siti@mail.com", "andi@mail.com"}
print("budi@mail.com" in email_terdaftar)   # True  — hampir instan
print("rudi@mail.com" in email_terdaftar)   # False — hampir instan
```

---

## 4. Mental Model & Best Practices

**Mental Model:**
- **Dictionary** = kamus — cari kata (key), dapat definisi (value)
- **Set** = daftar hadir — hanya nama unik, tidak ada duplikat, tidak ada nomor urut

| Fitur | List `[]` | Dictionary `{}` | Set `{}` |
|---|---|---|---|
| Akses dengan | Index angka | Key nama | Tidak bisa akses individual |
| Urutan | ✅ Berurutan | ✅ (Python 3.7+) | ❌ Tidak berurutan |
| Duplikat | ✅ Boleh | Key unik, value boleh sama | ❌ Tidak boleh |
| Kecepatan cari | Lambat O(n) | Cepat O(1) | Cepat O(1) |

```python
# ❌ BAD: Pakai list untuk data yang punya "nama" (key)
pengguna = ["Budi", 25, "Jakarta"]
print(pengguna[1])   # 25 — apa ini? Tidak jelas!

# ✅ GOOD: Pakai dictionary agar data bermakna
pengguna = {"nama": "Budi", "umur": 25, "kota": "Jakarta"}
print(pengguna["umur"])   # 25 — jelas!

# ❌ BAD: Akses dictionary tanpa pengaman
data = {"nama": "Budi"}
print(data["email"])   # ❌ KeyError! key tidak ada

# ✅ GOOD: Gunakan .get() dengan nilai default
print(data.get("email", "tidak tersedia"))   # "tidak tersedia"
```

---
---

# Topik 7: Error Handling (Try, Except, Finally)

## 1. The Core Truth (First Principles)

Program di dunia nyata berhadapan dengan **hal yang tidak terduga**: file tidak ada, koneksi internet putus, pengguna mengetik huruf padahal diminta angka. Tanpa penanganan error, satu kejadian tak terduga akan **menghancurkan seluruh program**.

**Error Handling** adalah cara program untuk mengatakan: *"Saya tahu ada yang bisa salah di sini, dan ini rencana saya jika itu terjadi."* Sama seperti pilot punya prosedur darurat — bukan karena pesawat pasti jatuh, tapi karena persiapan adalah profesionalisme.

---

## 2. The Basic Anatomy

```python
try:
    # Kode yang mungkin menghasilkan error
    hasil = 10 / 0
except ZeroDivisionError:
    # Kode yang dijalankan JIKA error terjadi
    print("Tidak bisa bagi dengan nol!")

# Program terus berjalan, tidak crash
print("Program masih hidup ✅")
```

---

## 3. Deep Dive & Mechanisms

### Menangkap Berbagai Jenis Error

```python
def proses_input(teks):
    try:
        angka = int(teks)       # Bisa gagal jika teks bukan angka
        hasil = 100 / angka     # Bisa gagal jika angka = 0
        return hasil

    except ValueError:
        # int("halo") → ValueError
        return "Error: input harus berupa angka"

    except ZeroDivisionError:
        # 100 / 0 → ZeroDivisionError
        return "Error: angka tidak boleh nol"

    except Exception as e:
        # Tangkap SEMUA error lain sebagai jaring pengaman
        return f"Error tidak terduga: {e}"

print(proses_input("halo"))  # Error: input harus berupa angka
print(proses_input("0"))     # Error: angka tidak boleh nol
print(proses_input("5"))     # 20.0
```

### `finally` — Selalu Dijalankan

```python
def baca_file(nama_file):
    file = None
    try:
        file = open(nama_file, "r")
        isi = file.read()
        return isi

    except FileNotFoundError:
        return f"File '{nama_file}' tidak ditemukan"

    finally:
        # Blok ini SELALU jalan, error atau tidak
        # Gunakan untuk: tutup file, tutup koneksi database, bersihkan resource
        if file:
            file.close()
            print("File berhasil ditutup")

hasil = baca_file("tidak_ada.txt")
# File berhasil ditutup  ← finally tetap jalan meski error
print(hasil)
# File 'tidak_ada.txt' tidak ditemukan
```

### `else` — Jalan Hanya Jika Tidak Ada Error

```python
try:
    angka = int("42")
except ValueError:
    print("Bukan angka!")
else:
    # Hanya jalan jika try SUKSES tanpa error
    print(f"Berhasil konversi: {angka}")
finally:
    # Selalu jalan
    print("Selesai.")

# Output:
# Berhasil konversi: 42
# Selesai.
```

### Urutan Eksekusi Lengkap

```python
# Rangkuman alur try-except-else-finally
try:
    # 1. Coba jalankan ini
    pass
except TipeError:
    # 2. Jalan HANYA jika ada error yang cocok
    pass
else:
    # 3. Jalan HANYA jika try sukses (tidak ada error)
    pass
finally:
    # 4. SELALU jalan, apapun yang terjadi
    pass
```

---

## 4. Mental Model & Best Practices

**Mental Model:** Try-except seperti **jaring pengaman di sirkus**. Akrobat tetap berusaha melakukan aksinya (try). Jika jatuh, jaring menangkapnya (except) — bukan untuk mencegah dia mencoba, tapi untuk memastikan jatuh tidak berakibat fatal.

```python
# ❌ BAD: Tangkap semua error secara buta
try:
    proses_data()
except Exception:
    pass  # Semua error ditelan diam-diam — BERBAHAYA, bug jadi tersembunyi!

# ✅ GOOD: Tangkap error yang spesifik dan lakukan sesuatu yang bermakna
try:
    proses_data()
except ValueError as e:
    print(f"Data tidak valid: {e}")
    # Lalu ambil tindakan nyata: log error, beritahu user, dst.

# ❌ BAD: Letakkan terlalu banyak kode di dalam try
try:
    langkah_1()
    langkah_2()
    langkah_3()
    langkah_4()
    langkah_5()
except Exception:
    print("Ada yang salah")  # Tapi yang mana?!

# ✅ GOOD: Try sekecil mungkin, hanya di bagian yang memang berisiko
hasil = langkah_1()
try:
    koneksi = langkah_2_yang_bisa_gagal(hasil)
except ConnectionError as e:
    print(f"Gagal konek: {e}")
```

---

# ✅ Summary & Checklist Fase 2

**Functions:**
- Satu function = satu tanggung jawab (prinsip DRY)
- Gunakan **default parameter** untuk nilai opsional
- Gunakan **keyword argument** agar pemanggilan lebih jelas
- Function tanpa `return` mengembalikan `None`

**Lists & Tuples:**
- List `[]` = mutable (bisa diubah), Tuple `()` = immutable (tidak bisa diubah)
- Akses elemen dengan index (mulai 0), gunakan index negatif untuk dari belakang
- Gunakan Tuple untuk data yang **tidak boleh** berubah (koordinat, konstanta)

**Dictionaries & Sets:**
- Dictionary = pasangan key-value, akses dengan key (bukan index angka)
- Selalu gunakan `.get()` bukan `[]` jika key mungkin tidak ada
- Set untuk kumpulan unik dan pengecekan keanggotaan yang cepat

**Error Handling:**
- `try` → `except` spesifik → `else` (sukses) → `finally` (selalu)
- **Jangan** tangkap `Exception` secara buta dan `pass` diam-diam
- `finally` untuk membersihkan resource (tutup file, koneksi, dll)

---

> 🎯 **Fase 2 selesai!** Ketik **"lanjut Fase 3"** saat siap — di Fase 3 kita masuk ke cara berpikir *Pythonic* yang akan membuat kode kamu terlihat jauh lebih elegan dan profesional.
