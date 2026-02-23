# 🚀 Panduan Git Collaboration untuk Tim 4 Pemula

> **Versi:** 1.0 | **Durasi Belajar:** 4 Minggu | **Level:** Pemula

Selamat datang di panduan kolaborasi Git untuk tim kamu! Panduan ini dirancang khusus untuk tim 4 orang yang baru pertama kali bekerja bersama menggunakan Git dan GitHub. Ikuti panduan ini secara berurutan — dari yang paling mudah hingga paling kompleks.

---

## 👥 Peran Tim

| Peran | Jumlah | Tanggung Jawab |
|-------|--------|----------------|
| 🏠 **Repository Owner / Maintainer** | 1 orang | Membuat repo, mengatur akses, meng-approve & merge Pull Request |
| 👨‍💻 **Contributor** | 3 orang | Mengembangkan fitur di branch masing-masing, membuat Pull Request |

---

## 📅 MINGGU 1: Setup Awal Tim

### 1.1 Instalasi Git

**Windows:**
```bash
# Download installer dari https://git-scm.com/download/win
# Jalankan installer, pilih semua opsi default
# Verifikasi instalasi:
git --version
```

**macOS:**
```bash
# Opsi 1: Pakai Homebrew (direkomendasikan)
brew install git

# Opsi 2: Install Xcode Command Line Tools
xcode-select --install

# Verifikasi:
git --version
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install git -y
git --version
```

**Contoh output yang benar:**
```
git version 2.43.0
```

---

### 1.2 Konfigurasi Identitas Git

> 💡 **TIP:** Gunakan nama asli dan email yang sama dengan akun GitHub kamu. Ini penting agar commit tercatat atas namamu.

Semua anggota tim wajib menjalankan perintah ini di komputer masing-masing:

```bash
git config --global user.name "Nama Lengkap Kamu"
git config --global user.email "email@kamu.com"
git config --global core.editor "code --wait"   # Pakai VS Code sebagai editor default
git config --global init.defaultBranch main
```

**Cek konfigurasi sudah benar:**
```bash
git config --list
```

**Output yang diharapkan:**
```
user.name=Budi Santoso
user.email=budi@example.com
core.editor=code --wait
init.defaultBranch=main
```

---

### 1.3 Membuat Akun GitHub & Setup SSH Key

**Langkah 1 — Daftar GitHub:**
1. Buka [github.com](https://github.com) → klik **Sign up**
2. Gunakan email yang sama dengan konfigurasi Git di atas
3. Verifikasi email kamu

**Langkah 2 — Generate SSH Key (semua anggota tim):**

```bash
# Generate SSH key baru
ssh-keygen -t ed25519 -C "email@kamu.com"

# Tekan Enter untuk semua pertanyaan (gunakan lokasi default)
# Output:
# Generating public/private ed25519 key pair.
# Enter file in which to save the key (/Users/kamu/.ssh/id_ed25519): [Enter]
# Enter passphrase (empty for no passphrase): [Enter]
# Enter same passphrase again: [Enter]
```

**Langkah 3 — Tambahkan SSH Key ke GitHub:**

```bash
# Tampilkan isi public key
cat ~/.ssh/id_ed25519.pub

# Copy semua teksnya (mulai dari "ssh-ed25519 ...")
```

Kemudian:
1. Login GitHub → **Settings** → **SSH and GPG keys** → **New SSH key**
2. Paste public key yang sudah dicopy
3. Beri judul yang deskriptif (contoh: "Laptop Pribadi Budi")

**Langkah 4 — Test koneksi SSH:**
```bash
ssh -T git@github.com
# Output sukses: Hi username! You've successfully authenticated...
```

---

### 1.4 Setup Repository (Dilakukan Repository Owner)

> ⚠️ **WARNING:** Langkah ini hanya dilakukan oleh **1 orang** (Repository Owner). Anggota lain cukup menunggu undangan.

**Repository Owner:**
```bash
# 1. Buat folder proyek
mkdir nama-proyek-tim
cd nama-proyek-tim

# 2. Inisialisasi Git
git init

# 3. Buat file README awal
echo "# Nama Proyek Tim" > README.md

# 4. Commit pertama
git add README.md
git commit -m "feat: initial commit - setup project"

# 5. Buat repository di GitHub (lewat website github.com)
# Kemudian hubungkan:
git remote add origin git@github.com:username/nama-repo.git
git branch -M main
git push -u origin main
```

**Mengundang Anggota Tim (Repository Owner di GitHub):**
1. Buka repository → **Settings** → **Collaborators** → **Add people**
2. Masukkan username GitHub masing-masing anggota
3. Pilih role: **Write**

**Contributor menerima undangan:**
```bash
# Setelah menerima undangan via email, clone repository:
git clone git@github.com:username-owner/nama-repo.git
cd nama-repo
```

---

### 1.5 Naming Convention Tim

**Branch naming:**
```
# Format: tipe/deskripsi-singkat
feature/halaman-login
feature/navbar-responsive
bugfix/form-validasi-email
hotfix/crash-saat-login
```

**Commit message format (Conventional Commits):**
```
# Format: tipe(scope): deskripsi singkat
feat(auth): tambah fitur login dengan Google
fix(navbar): perbaiki tampilan di mobile
docs(readme): update instruksi instalasi
style(button): rapikan padding dan warna
refactor(api): sederhanakan fungsi fetch data
```

---

### ✅ Checklist Minggu 1

- [ ] Git terinstal di semua komputer anggota
- [ ] Konfigurasi nama dan email Git selesai
- [ ] Semua anggota punya akun GitHub
- [ ] SSH Key terkonfigurasi di semua komputer
- [ ] Repository Owner membuat repo dan mengundang semua anggota
- [ ] Semua Contributor berhasil clone repository
- [ ] Tim sepakat dengan naming convention

---

## 📅 MINGGU 2: Workflow Kolaborasi Harian

Bayangkan Git seperti **sistem pengeditan dokumen bersama di kantor**. `main` adalah dokumen resmi yang sudah final. Setiap kali kamu mau mengedit, kamu fotokopi dulu (buat branch), edit di salinan itu, lalu ajukan ke atasan (Pull Request) untuk diperiksa sebelum masuk ke dokumen resmi.

### 2.1 Daily Workflow — Panduan Lengkap

```
┌─────────────────────────────────────────────────────────┐
│                    WORKFLOW HARIAN                      │
│                                                         │
│  1. Sinkronisasi  →  2. Buat Branch  →  3. Coding      │
│                                              ↓          │
│  7. Merge & Clean  ←  6. Code Review  ←  4. Commit     │
│                                              ↓          │
│                                        5. Push & PR     │
└─────────────────────────────────────────────────────────┘
```

---

**LANGKAH 1 — Sinkronisasi Sebelum Mulai Kerja:**

```bash
# Selalu lakukan ini SETIAP PAGI sebelum mulai coding
git checkout main
git pull origin main

# Pastikan tidak ada perubahan yang belum di-commit
git status
# Output yang diharapkan: "nothing to commit, working tree clean"
```

---

**LANGKAH 2 — Membuat Feature Branch:**

```bash
# Buat branch baru dari main yang sudah update
git checkout -b feature/nama-fitur-kamu

# Contoh:
git checkout -b feature/halaman-registrasi

# Verifikasi kamu sudah di branch yang benar:
git branch
# Output: * feature/halaman-registrasi
#           main
```

---

**LANGKAH 3 — Coding & LANGKAH 4 — Commit yang Baik:**

```bash
# Cek file apa saja yang berubah
git status

# Lihat detail perubahannya
git diff

# Tambahkan file yang siap di-commit
git add nama-file.html          # Satu file spesifik
git add css/                    # Satu folder
git add .                       # Semua perubahan (hati-hati!)

# Buat commit dengan pesan yang jelas
git commit -m "feat(registrasi): tambah form pendaftaran user baru"

# Contoh output:
# [feature/halaman-registrasi a3f5d1c] feat(registrasi): tambah form pendaftaran
#  1 file changed, 45 insertions(+)
#  create mode 100644 registrasi.html
```

> 💡 **TIP:** Commit sesering mungkin untuk setiap perubahan logis yang selesai. Jangan menumpuk terlalu banyak perubahan dalam satu commit.

> ⚠️ **WARNING:** Jangan pernah `git add .` tanpa mengecek `git status` terlebih dahulu. Kamu bisa tidak sengaja commit file yang tidak perlu (seperti file konfigurasi lokal atau folder `node_modules`).

---

**LANGKAH 5 — Push ke Remote & Buat Pull Request:**

```bash
# Push branch kamu ke GitHub
git push origin feature/halaman-registrasi

# Jika pertama kali push branch ini, gunakan:
git push --set-upstream origin feature/halaman-registrasi
# atau shortcut:
git push -u origin feature/halaman-registrasi
```

Kemudian buat Pull Request di GitHub:
1. Buka repository di GitHub
2. Kamu akan melihat banner "Compare & pull request" → klik
3. Isi template Pull Request (lihat bagian Template di bawah)
4. Assign reviewer ke salah satu anggota tim
5. Klik **Create pull request**

---

**LANGKAH 6 — Code Review:**

Reviewer membuka Pull Request dan:
1. Baca deskripsi PR dengan teliti
2. Klik tab **Files changed** untuk melihat perubahan kode
3. Klik **+** di samping baris kode untuk memberi komentar
4. Setelah review, klik **Review changes** → pilih:
   - **Approve** jika sudah oke
   - **Request changes** jika ada yang perlu diperbaiki
   - **Comment** jika hanya mau bertanya

---

**LANGKAH 7 — Merge & Bersih-Bersih:**

Repository Owner merge PR yang sudah di-approve:
1. Klik **Squash and merge** (direkomendasikan untuk tim kecil)
2. Konfirmasi merge

Setelah merge, semua anggota tim:
```bash
# Kembali ke main dan ambil perubahan terbaru
git checkout main
git pull origin main

# Hapus branch yang sudah di-merge
git branch -d feature/halaman-registrasi           # Hapus lokal
git push origin --delete feature/halaman-registrasi # Hapus remote
```

---

### 2.2 Template Pull Request

```markdown
## 📋 Deskripsi
Jelaskan apa yang kamu kerjakan dan mengapa.

## 🔧 Jenis Perubahan
- [ ] Bug fix
- [ ] Fitur baru
- [ ] Perubahan kode yang tidak mempengaruhi fungsionalitas
- [ ] Update dokumentasi

## ✅ Checklist
- [ ] Kode sudah ditest secara manual
- [ ] Tidak ada console.log yang ketinggalan
- [ ] Sudah sync dengan main terbaru sebelum PR

## 📸 Screenshot (jika ada perubahan UI)
(Tambahkan screenshot di sini)

## 📝 Catatan untuk Reviewer
Hal khusus yang perlu diperhatikan saat review.
```

---

### ✅ Checklist Minggu 2

- [ ] Semua anggota memahami 7 langkah workflow harian
- [ ] Berhasil membuat branch pertama
- [ ] Berhasil membuat commit dengan pesan yang benar
- [ ] Berhasil push dan membuat Pull Request pertama
- [ ] Berhasil melakukan code review
- [ ] Repository Owner berhasil merge PR pertama

---

## 📅 MINGGU 3: Mengelola Merge Conflict

### 3.1 Apa Itu Merge Conflict?

Bayangkan dua orang mengedit **paragraf yang sama** di dokumen Word secara bersamaan. Saat mereka mau menggabungkan — sistem bingung: versi siapa yang dipakai? Itulah merge conflict.

Merge conflict terjadi ketika:
- Dua orang mengedit **baris yang sama** di file yang sama
- Satu orang mengubah file yang sudah dihapus orang lain

---

### 3.2 Membaca Conflict Marker

Saat conflict terjadi, Git akan menambahkan tanda khusus ke dalam file:

```html
<!DOCTYPE html>
<html>
<head>
<<<<<<< HEAD
    <title>Toko Online Kami</title>
=======
    <title>Toko Online Kita Bersama</title>
>>>>>>> feature/update-judul
</head>
</html>
```

**Cara membacanya:**
- `<<<<<<< HEAD` → awal dari versi kamu (branch yang kamu sedang pakai)
- `=======` → pemisah antara dua versi
- `>>>>>>> feature/update-judul` → awal dari versi yang mau di-merge

---

### 3.3 Cara Resolve Conflict

```bash
# Langkah 1: Cek file mana yang conflict
git status
# Output: both modified: index.html

# Langkah 2: Buka file yang conflict (di VS Code atau editor lain)
code index.html

# Langkah 3: Edit file — pilih salah satu versi atau gabungkan keduanya
# Hapus semua marker (<<<, ===, >>>)
# Hasil akhir yang kamu inginkan:
# <title>Toko Online Kita Bersama</title>

# Langkah 4: Tandai conflict sebagai selesai
git add index.html

# Langkah 5: Selesaikan merge
git commit -m "fix: resolve merge conflict pada judul halaman"
```

> 💡 **TIP di VS Code:** VS Code punya fitur built-in untuk resolve conflict. Kamu akan melihat tombol **Accept Current Change**, **Accept Incoming Change**, atau **Accept Both Changes** di atas setiap conflict marker. Ini jauh lebih mudah daripada edit manual!

> ⚠️ **WARNING:** Pastikan kamu **menghapus SEMUA marker conflict** (`<<<<<<<`, `=======`, `>>>>>>>`) sebelum commit. Jika marker masih ada, kodenya akan rusak!

---

### 3.4 Cara Mencegah Conflict

Komunikasi tim yang baik adalah kunci mencegah conflict:

1. **Bagi tugas dengan jelas** — Pastikan tidak ada dua orang yang mengerjakan file yang sama secara bersamaan
2. **Sync sesering mungkin** — Lakukan `git pull origin main` setiap pagi
3. **Buat branch kecil** — Selesaikan dan merge dalam 1-2 hari, jangan biarkan branch hidup terlalu lama
4. **Komunikasi di chat tim** — "Hei, aku lagi ngerjain `navbar.html` ya!"

---

### 3.5 Simulasi Latihan Conflict

**Skenario:** Dua contributor (A dan B) mengedit file yang sama.

```bash
# --- CONTRIBUTOR A ---
git checkout main && git pull
git checkout -b feature/update-footer
# Edit baris terakhir di index.html: "Copyright 2024"
git add . && git commit -m "style(footer): update tahun copyright"
git push -u origin feature/update-footer
# Buat PR → Owner merge

# --- CONTRIBUTOR B (belum tahu PR A sudah di-merge) ---
git checkout main  # main-nya masih lama!
git checkout -b feature/redesign-footer
# Edit baris terakhir yang sama di index.html: "© 2024 Tim Kami"
git add . && git commit -m "style(footer): redesign footer"
git push -u origin feature/redesign-footer
# Buat PR → Muncul notif conflict!

# Cara B menyelesaikan conflict:
git checkout feature/redesign-footer
git pull origin main        # Ambil perubahan terbaru dari main (termasuk PR si A)
# Conflict muncul! Selesaikan conflict di file
git add .
git commit -m "fix: resolve conflict pada footer"
git push
```

---

### ✅ Checklist Minggu 3

- [ ] Memahami apa itu merge conflict dan kenapa terjadi
- [ ] Bisa membaca dan menginterpretasi conflict marker
- [ ] Berhasil simulate dan resolve conflict pertama
- [ ] Tim sudah membuat aturan pembagian file/task yang jelas
- [ ] Memahami cara mencegah conflict

---

## 📅 MINGGU 4: Branching Strategy untuk Tim Kecil

### 4.1 GitHub Flow

GitHub Flow adalah strategi branching yang sederhana dan cocok untuk tim kecil. Prinsipnya: **`main` selalu dalam kondisi stabil dan siap deploy.**

```
main ──●──────────────────────────●──────────── (selalu stabil)
       │                          │
       └──● feature/login         │
          └──● tambah validasi    │
             └──● perbaiki UI ────┘ (merge via PR)

main ──●──────────────────────────●──────────── 
                                  │
                                  └──● feature/register
                                     └──● selesai ──────┘ (merge via PR)
```

**Aturan GitHub Flow:**
1. `main` = production-ready, tidak boleh rusak
2. Semua pekerjaan dilakukan di branch terpisah
3. Branch harus punya nama yang deskriptif
4. PR dibuat sesegera mungkin untuk diskusi awal
5. Merge ke `main` hanya setelah review dan approve

---

### 4.2 Kapan Buat Branch Baru vs Commit Langsung?

| Situasi | Yang Harus Dilakukan |
|---------|---------------------|
| Fitur baru | ✅ Buat branch baru: `feature/nama-fitur` |
| Perbaikan bug | ✅ Buat branch baru: `bugfix/deskripsi-bug` |
| Update kecil di README | ⚠️ Buat branch: `docs/update-readme` |
| Typo di komentar kode | ✅ Bisa commit langsung ke branch yang sedang dikerjakan |
| **Push langsung ke `main`** | ❌ **JANGAN PERNAH** |

---

### 4.3 Hotfix — Perbaikan Darurat

Hotfix adalah perbaikan mendesak untuk bug kritis yang sudah ada di `main`.

```bash
# Langkah 1: Buat branch hotfix dari main
git checkout main
git pull origin main
git checkout -b hotfix/perbaiki-crash-login

# Langkah 2: Perbaiki bug sesegera mungkin
# (edit file yang bermasalah)
git add .
git commit -m "fix(auth): perbaiki crash saat password kosong"

# Langkah 3: Push dan buat PR dengan label "URGENT"
git push -u origin hotfix/perbaiki-crash-login
# Buat PR → minta review segera → merge

# Langkah 4: Setelah merge, update semua branch yang sedang aktif
git checkout feature/branch-kamu-yang-lain
git merge main   # Atau: git rebase main
```

---

### 4.4 Membersihkan Branch Lama

```bash
# Lihat semua branch (lokal dan remote)
git branch -a

# Hapus branch lokal yang sudah di-merge
git branch -d nama-branch          # Aman — hanya hapus jika sudah di-merge
git branch -D nama-branch          # Paksa hapus (gunakan dengan hati-hati!)

# Hapus branch remote
git push origin --delete nama-branch

# Bersihkan referensi branch remote yang sudah tidak ada
git remote prune origin
git fetch --prune
```

---

### ✅ Checklist Minggu 4

- [ ] Tim memahami dan menerapkan GitHub Flow
- [ ] Tidak ada yang push langsung ke `main`
- [ ] Berhasil membuat dan merge hotfix
- [ ] Branch-branch lama sudah dibersihkan
- [ ] Tim sudah nyaman dengan seluruh workflow

---

## 🏆 Best Practices & Etika Tim

### Panduan Commit Message (Conventional Commits)

**Format:** `tipe(scope): deskripsi singkat`

| Tipe | Kapan Dipakai | Contoh |
|------|---------------|--------|
| `feat` | Fitur baru | `feat(cart): tambah tombol hapus item` |
| `fix` | Perbaikan bug | `fix(login): perbaiki validasi email` |
| `docs` | Update dokumentasi | `docs(readme): tambah cara instalasi` |
| `style` | Perubahan format/style (bukan logika) | `style(css): rapikan indentasi` |
| `refactor` | Refactor kode (bukan fitur/fix) | `refactor(api): sederhanakan fungsi fetch` |
| `test` | Tambah atau perbaiki test | `test(auth): tambah unit test login` |
| `chore` | Update konfigurasi, tools, dll | `chore: update dependency versi terbaru` |

---

### Tabel Commit BAIK vs BURUK

| ❌ BURUK | ✅ BAIK |
|---------|---------|
| `update file` | `feat(navbar): tambah dropdown menu kategori` |
| `fix bug` | `fix(checkout): perbaiki total harga tidak update` |
| `asdfsdf` | `style(button): seragamkan ukuran padding tombol` |
| `wip` | `feat(auth): [WIP] setup JWT authentication` |
| `final final` | `feat(landing): selesaikan halaman utama` |
| `kemarin lupa commit` | `fix(form): validasi field nama yang kosong` |

---

### Etika Pull Request

**Sebagai pembuat PR:**
- Pastikan PR kecil dan fokus (idealnya <300 baris perubahan)
- Isi deskripsi PR dengan jelas — jangan biarkan reviewer menebak-nebak
- Tag reviewer dengan sopan: "@Budi bisa review PR ini saat senggang?"
- Respond komentar review dalam 24 jam
- Jangan langsung tersinggung — code review itu tentang kode, bukan orangnya

**Sebagai reviewer:**
- Review dalam 24 jam jika memungkinkan
- Bedakan antara must-fix dan nice-to-have dengan label: `[BLOCKING]` vs `[SUGGESTION]`
- Contoh komentar yang baik: *"[SUGGESTION] Mungkin bisa pakai `const` daripada `let` di sini karena nilainya tidak berubah?"*
- Apresiasi kerja yang baik: *"Kode ini rapi banget! Suka struktur fungsinya."*
- Tanyakan jika tidak mengerti, jangan langsung judge

---

### Hal yang JANGAN Dilakukan

- ❌ Push langsung ke `main` tanpa PR
- ❌ Merge PR milik sendiri tanpa review orang lain
- ❌ Force push ke branch yang dibagikan (`git push --force`)
- ❌ Commit file credential/password/API key
- ❌ Commit folder `node_modules`, `.env`, atau file build
- ❌ Biarkan branch terbuka lebih dari seminggu tanpa aktivitas
- ❌ Commit dengan pesan yang tidak informatif

---

## 📖 Cheat Sheet — 30+ Perintah Git

### Setup & Konfigurasi

| Perintah | Fungsi |
|----------|--------|
| `git config --global user.name "Nama"` | Set nama pengguna |
| `git config --global user.email "email"` | Set email pengguna |
| `git config --list` | Lihat semua konfigurasi |
| `git init` | Inisialisasi repo baru |
| `git clone <url>` | Clone repository dari remote |

### Status & Informasi

| Perintah | Fungsi |
|----------|--------|
| `git status` | Lihat status file (modified, staged, dll) |
| `git log` | Lihat riwayat commit |
| `git log --oneline` | Riwayat commit ringkas |
| `git log --oneline --graph` | Riwayat dengan diagram branch |
| `git diff` | Lihat perubahan yang belum di-stage |
| `git diff --staged` | Lihat perubahan yang sudah di-stage |
| `git show <commit-hash>` | Detail satu commit tertentu |

### Branch

| Perintah | Fungsi |
|----------|--------|
| `git branch` | Lihat semua branch lokal |
| `git branch -a` | Lihat semua branch (lokal + remote) |
| `git checkout -b <nama>` | Buat branch baru dan pindah ke sana |
| `git checkout <nama>` | Pindah ke branch yang sudah ada |
| `git branch -d <nama>` | Hapus branch lokal (sudah di-merge) |
| `git branch -D <nama>` | Paksa hapus branch lokal |

### Staging & Commit

| Perintah | Fungsi |
|----------|--------|
| `git add <file>` | Stage satu file |
| `git add .` | Stage semua perubahan |
| `git add -p` | Stage perubahan secara interaktif |
| `git commit -m "pesan"` | Commit dengan pesan |
| `git commit --amend` | Edit commit terakhir (sebelum push!) |

### Remote & Sync

| Perintah | Fungsi |
|----------|--------|
| `git remote -v` | Lihat daftar remote |
| `git push origin <branch>` | Push branch ke remote |
| `git push -u origin <branch>` | Push dan set tracking |
| `git pull origin main` | Ambil & merge dari remote |
| `git fetch origin` | Ambil info dari remote tanpa merge |

### Undo & Perbaikan

| Perintah | Fungsi |
|----------|--------|
| `git restore <file>` | Batalkan perubahan yang belum di-stage |
| `git restore --staged <file>` | Un-stage file |
| `git revert <hash>` | Batalkan commit (aman, buat commit baru) |
| `git stash` | Simpan perubahan sementara |
| `git stash pop` | Kembalikan perubahan dari stash |

---

### 🆘 Diagram Situasi Darurat

```
❓ Salah commit, belum push
   └─ git commit --amend          ← Ubah pesan/isi commit terakhir

❓ Mau batalkan commit terakhir, tapi simpan perubahannya
   └─ git reset --soft HEAD~1     ← File tetap ada di staging

❓ Mau batalkan commit terakhir, dan buang perubahannya
   └─ git reset --hard HEAD~1     ← ⚠️ HATI-HATI! File hilang permanen

❓ File tidak sengaja masuk staging
   └─ git restore --staged <file> ← Un-stage file

❓ Perubahan lokal ingin dibatalkan
   └─ git restore <file>          ← Kembalikan ke kondisi commit terakhir

❓ Mau pindah branch tapi ada perubahan yang belum siap commit
   └─ git stash                   ← Simpan dulu
      (pindah branch, kerjakan)
      git stash pop                ← Kembalikan perubahan

❓ Sudah push, mau batalkan commit
   └─ git revert <hash>           ← Buat "undo commit" yang aman
      git push                     ← Push revert commit
```

---

## 🔧 Troubleshooting 10 Masalah Umum

**1. "Permission denied (publickey)" saat push/pull**
```bash
# Cek SSH key sudah terdaftar di GitHub
ssh -T git@github.com
# Jika gagal: ulangi langkah setup SSH key di bagian 1.3
```

**2. "Please commit your changes or stash them before you merge"**
```bash
# Ada perubahan yang belum di-commit
git stash      # simpan sementara
git pull       # sync
git stash pop  # kembalikan perubahan
```

**3. "Your branch is behind 'origin/main' by X commits"**
```bash
git pull origin main
# Jika conflict, selesaikan conflict terlebih dahulu
```

**4. Tidak sengaja commit ke branch `main` langsung**
```bash
# Pindahkan commit ke branch baru
git branch feature/nama-fitur  # buat branch dari kondisi sekarang
git reset --hard origin/main   # kembalikan main ke kondisi remote
git checkout feature/nama-fitur # pindah ke branch baru
```

**5. Lupa nama branch yang sedang dikerjakan**
```bash
git branch      # list semua branch, yang aktif ditandai *
git status      # juga menampilkan nama branch saat ini
```

**6. "Merge conflict in file" saat pull**
```bash
# Edit file yang conflict (hapus marker <<<<, ====, >>>>)
git add <file-yang-diselesaikan>
git commit -m "fix: resolve merge conflict"
```

**7. Sudah push tapi mau ganti commit message terakhir**
```bash
# Tidak bisa mengubah commit yang sudah di-push tanpa force push
# JANGAN force push ke branch yang dibagi bersama
# Sebaiknya: buat commit baru: git commit --allow-empty -m "typo fix pada commit sebelumnya"
```

**8. File yang tidak perlu sudah terlanjur di-commit**
```bash
# Tambahkan ke .gitignore, lalu:
git rm --cached nama-file-atau-folder
git commit -m "chore: hapus file yang tidak perlu dari tracking"
```

**9. "fatal: not a git repository"**
```bash
# Kamu berada di luar folder repo
ls -la   # cek apakah ada folder .git
# Kalau tidak ada, init atau clone ulang repositorynya
```

**10. Push ditolak: "Updates were rejected because the remote contains work that you do not have locally"**
```bash
# Ada commit di remote yang tidak ada di lokal
git pull origin <nama-branch>
# Selesaikan conflict jika ada, lalu push ulang
```

---

## 📅 MINGGU 4+: Rencana Latihan 4 Minggu

### Jadwal Belajar Harian (15-30 menit)

| Hari | Aktivitas |
|------|-----------|
| Senin | Baca materi minggu ini (10 mnt) + praktek perintah baru (20 mnt) |
| Selasa | Praktek workflow dengan mini project tim (30 mnt) |
| Rabu | Simulasi skenario spesifik (conflict, hotfix, dll) (25 mnt) |
| Kamis | Code review PR anggota lain (15 mnt) |
| Jumat | Review & diskusi apa yang dipelajari minggu ini (20 mnt) |
| Weekend | Eksplorasi mandiri / istirahat |

---

### Mini Project Simulasi

**Project: Website Tim Sederhana (4 halaman)**

Bagi tugas:
- Member A (Owner): Setup repo + halaman `index.html`
- Member B: Halaman `about.html`
- Member C: Halaman `portfolio.html`
- Member D: Halaman `contact.html`

Semua mengerjakan di branch terpisah, buat PR, lakukan code review, dan merge secara bergilir.

---

### Milestone Per Minggu

| Minggu | Milestone |
|--------|-----------|
| **Minggu 1** | ✅ Semua anggota bisa clone repo dan push commit pertama |
| **Minggu 2** | ✅ Satu siklus PR lengkap: branch → commit → push → PR → review → merge |
| **Minggu 3** | ✅ Berhasil mensimulasi dan menyelesaikan merge conflict |
| **Minggu 4** | ✅ Mini project selesai dengan semua fitur di-merge via proper PR workflow |

---

## 📎 Lampiran

### Template `.gitignore` untuk Proyek Web

```gitignore
# Dependencies
node_modules/
vendor/

# Environment & Config
.env
.env.local
.env.*.local

# Build output
dist/
build/
out/

# OS generated
.DS_Store
.DS_Store?
._*
Thumbs.db
Desktop.ini

# Editor
.vscode/settings.json
.idea/
*.suo
*.ntvs*
*.njsproj
*.sln

# Logs
*.log
npm-debug.log*
yarn-debug.log*

# Cache
.cache/
.parcel-cache/
```

### Rekomendasi Ekstensi VS Code untuk Git

| Ekstensi | Fungsi |
|----------|--------|
| **GitLens** | Tampilkan siapa yang mengubah setiap baris kode |
| **Git Graph** | Visualisasi diagram branch yang cantik |
| **Git History** | Jelajahi riwayat commit dengan mudah |
| **GitHub Pull Requests** | Buat dan review PR langsung dari VS Code |

---

> 🎉 **Selamat!** Kamu sudah memiliki panduan lengkap untuk berkolaborasi menggunakan Git. Ingat, kunci utama adalah **konsistensi** dan **komunikasi tim yang baik**. Selamat belajar dan berkolaborasi!

---

*Panduan ini dibuat untuk tim 4 orang pemula yang ingin belajar Git collaboration dari awal.*
