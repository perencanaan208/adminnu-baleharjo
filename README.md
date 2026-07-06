[README (2).md](https://github.com/user-attachments/files/29693505/README.2.md)
# Sistem Administrasi NU Baleharjo — Redirect Pendek (GitHub + Vercel)

Repo ini **bukan** aplikasi baru. Isinya cuma konfigurasi *redirect* supaya
URL Vercel yang pendek (misal `adminnu-baleharjo.vercel.app`) langsung
mengalihkan ke Sistem Administrasi NU Baleharjo yang aslinya jalan di
Google Apps Script.

## Kenapa redirect, bukan iframe?

Awalnya dicoba pakai `<iframe>` supaya URL pendek tetap tampil di address
bar. Ternyata **Google Apps Script Web App tidak bisa di-iframe dengan
baik dari domain luar** — ada proses redirect/autentikasi internal Google
yang gagal kalau dibungkus iframe dari domain lain, hasilnya malah muncul
halaman error "Google Drive: file tidak ditemukan" yang membingungkan.
Ini keterbatasan dari sisi Google, bukan sesuatu yang bisa diperbaiki dari
kode kita.

Solusinya: pakai **redirect otomatis**. Begitu orang buka URL Vercel yang
pendek, browser langsung dialihkan ke URL GAS yang asli (URL di address
bar akan berubah jadi yang panjang setelah dialihkan — itu normal, karena
browser memang harus benar-benar pindah ke halaman GAS-nya).

**Semua fitur (login admin, login bendahara, tampilan publik, simpan data,
dll) tetap 100% berjalan di Google Apps Script**, sama seperti kalau
diakses langsung dari URL `/exec`. Repo ini tidak menyimpan logika atau
data apa pun.

---

## 1. Push ke GitHub

```bash
cd nu-baleharjo-admin-viewer
git init
git add .
git commit -m "Redirect ke Sistem Administrasi NU Baleharjo"
git remote add origin https://github.com/USERNAME_ANDA/nu-baleharjo-admin.git
git branch -M main
git push -u origin main
```

Ganti `USERNAME_ANDA` dengan username GitHub Anda. Kalau repo belum ada,
buat dulu lewat https://github.com/new (jangan centang "Add a README").

## 2. Hubungkan ke Vercel

1. Buka https://vercel.com, login pakai akun GitHub
2. **Add New...** → **Project** → pilih repo ini
3. Framework Preset: **Other**
4. **Deploy**

Selesai — URL Vercel Anda (misal `adminnu-baleharjo.vercel.app`) sekarang
langsung redirect ke Sistem Administrasi NU Baleharjo.

## 3. Kalau URL GAS berubah

URL `/exec` GAS **tidak akan berubah** selama Anda selalu update
deployment yang sama lewat:

> Deploy → Manage deployments → (ikon pensil) Edit → Version: **New version** → Deploy

Kalau suatu saat memang berubah (misalnya karena membuat deployment baru
dari nol), update di **2 tempat** pada repo ini:

1. File `vercel.json` — ganti URL di bagian `"destination"`
2. File `index.html` — ganti URL di 3 tempat (meta refresh, script, dan link fallback)

Lalu commit & push:
```bash
git add vercel.json index.html
git commit -m "Update URL GAS"
git push
```
Vercel otomatis redeploy dalam beberapa detik.

## Catatan penting: file harus bernama PERSIS

Kalau upload file lewat GitHub web (drag & drop), pastikan nama filenya
**persis**: `index.html`, `vercel.json`, `.gitignore`, `README.md` — tanpa
tambahan seperti `(1)`, `(2)`, dsb. Nama yang tidak persis (misal
`index (2).html`) akan membuat Vercel tidak menemukan file yang
dibutuhkan dan muncul error 404.
