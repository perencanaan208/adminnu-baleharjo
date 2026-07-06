# Sistem Administrasi NU Baleharjo — Viewer (GitHub + Vercel)

Repo ini **bukan** aplikasi baru — isinya cuma 1 halaman (`index.html`) yang
menampilkan Sistem Administrasi NU Baleharjo (yang aslinya jalan di Google
Apps Script) lewat `<iframe>`, supaya bisa diakses dengan URL yang lebih
pendek dan rapi lewat Vercel.

**Semua fitur (login admin, login bendahara, tampilan publik, simpan data,
dll) tetap 100% jalan di Google Apps Script.** Repo ini tidak menyimpan
logika atau data apa pun — cuma "jendela" ke aplikasi aslinya. Karena itu:

- Setiap kali ada perubahan/update di GAS (kode atau data), begitu di-deploy
  di GAS, **otomatis langsung kelihatan** di sini juga — tidak perlu push
  ulang ke GitHub, tidak perlu redeploy Vercel. Karena iframe selalu memuat
  versi *live* dari GAS setiap kali halaman dibuka.
- Yang perlu di-push ulang ke GitHub **hanya kalau URL `/exec` dari GAS
  berubah** (jarang terjadi — lihat bagian "Kalau URL GAS berubah" di bawah).

---

## 1. Push ke GitHub

Kalau belum pernah pakai Git di komputer ini, install Git dulu, lalu:

```bash
cd nu-baleharjo-admin-viewer
git init
git add .
git commit -m "Initial commit: viewer Sistem Administrasi NU Baleharjo"
```

Buat repo baru di GitHub (lewat browser):
1. Buka https://github.com/new
2. Isi nama repo, misal `nu-baleharjo-admin`
3. **Jangan** centang "Add a README" (karena sudah ada dari sini)
4. Klik **Create repository**

Setelah repo dibuat, GitHub akan kasih perintah seperti ini — jalankan di
terminal:

```bash
git remote add origin https://github.com/USERNAME_ANDA/nu-baleharjo-admin.git
git branch -M main
git push -u origin main
```

Ganti `USERNAME_ANDA` dengan username GitHub Anda.

---

## 2. Hubungkan ke Vercel

1. Buka https://vercel.com dan login (bisa pakai akun GitHub langsung)
2. Klik **Add New...** → **Project**
3. Pilih repo `nu-baleharjo-admin` yang barusan di-push
4. Di bagian **Framework Preset**, pilih **Other** (karena ini cuma static HTML)
5. Biarkan pengaturan lain default (tidak perlu Build Command atau Output
   Directory khusus)
6. Klik **Deploy**

Setelah selesai (biasanya < 1 menit), Vercel akan kasih URL seperti:

```
https://nu-baleharjo-admin.vercel.app
```

URL ini yang bisa dipakai sehari-hari — jauh lebih pendek dibanding URL
`script.google.com/macros/s/.../exec` yang panjang.

### Pakai domain sendiri (opsional)

Kalau punya domain sendiri (misal `admin.nubaleharjo.org`), bisa dihubungkan
lewat **Project Settings → Domains** di Vercel, lalu ikuti instruksi untuk
setting DNS di penyedia domain Anda.

---

## 3. Kalau URL GAS berubah

URL `/exec` GAS **biasanya tidak berubah** selama Anda selalu update
deployment yang sama lewat:

> Deploy → Manage deployments → (ikon pensil) Edit → Version: **New version** → Deploy

URL hanya akan berubah kalau Anda membuat **deployment baru dari nol**
(Deploy → New deployment) alih-alih mengedit yang sudah ada. **Sebisa
mungkin hindari itu** — selalu edit deployment yang sama supaya URL tetap
sama dan viewer ini tidak perlu diutak-atik lagi.

Kalau suatu saat URL memang berubah:

1. Buka file `index.html` di repo ini
2. Cari baris:
   ```js
   var GAS_WEB_APP_URL = "https://script.google.com/macros/s/.../exec";
   ```
3. Ganti dengan URL `/exec` yang baru
4. Commit & push:
   ```bash
   git add index.html
   git commit -m "Update URL GAS"
   git push
   ```
5. Vercel otomatis redeploy dalam beberapa detik (karena sudah terhubung ke
   GitHub) — tidak perlu langkah manual lagi di Vercel.

---

## Catatan & keterbatasan

- Ini murni **jendela/viewer**, bukan salinan aplikasi. Semua data,
  keamanan, dan logika tetap sepenuhnya berada dan dikendalikan oleh Google
  Apps Script — sama seperti kalau diakses langsung dari URL `/exec`.
- Kalau suatu saat Google mengubah kebijakan sehingga halaman GAS tidak
  bisa lagi dimuat dalam `<iframe>`, viewer ini akan menampilkan tombol
  "Buka Sistem Administrasi" yang mengarah langsung ke URL asli GAS sebagai
  cadangan (fallback otomatis setelah 8 detik jika iframe gagal dimuat).
- Login admin/bendahara, sesi, dan seluruh keamanan tetap mengikuti aturan
  yang sudah dibuat di dalam Apps Script (`Kode.gs` / `index.html` GAS),
  tidak ada perubahan apa pun dari sisi ini.
