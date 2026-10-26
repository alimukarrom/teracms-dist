# TeraCMS — Repo Distribusi Rilis

Repo ini adalah rumah rilis TeraCMS: isinya hanya **manifes versi** dan,
sebagai aset GitHub Releases, **paket zip jadi**. Bukan kode sumber (kode
sumber Core tinggal di repo privat `teracms-core`).

Instalasi TeraCMS membaca manifes dari repo ini tanpa token apa pun, lalu
membandingkan versi Core, plugin, dan temanya (halaman admin "Pembaruan").
Kontrak lengkapnya ada di `docs/UPDATE-CHANNEL.md` repo sumber.

## Struktur

```
core/stable.json            manifes Core kanal stable
core/beta.json              manifes Core kanal beta
plugins/<slug>/stable.json  manifes plugin
themes/<slug>/stable.json   manifes tema
packages/                   hasil build lokal — TIDAK ikut commit
```

Paket zip diterbitkan sebagai aset Releases dengan pola tag
`core-v<versi>`, `plugin-<slug>-v<versi>`, `theme-<slug>-v<versi>`;
`package_url` di tiap manifes menunjuk ke aset itu.

## Menerbitkan ke GitHub (langkah manual sekali saja, oleh pemilik)

Repo GitHub-nya belum dibuat — server build tidak memegang kredensial
akun GitHub (deploy key bersifat per-repo), jadi langkah ini manual:

1. Buat repo **publik kosong** bernama `teracms-dist` di GitHub.
2. Tambahkan deploy key repo itu di server build bila memakai SSH
   per-repo, lalu di folder ini:

   ```bash
   git remote add origin git@github.com:alimukarrom/teracms-dist.git
   git push -u origin main
   ```

3. Buat Release pertama: tag `core-v1.0.0`, lampirkan berkas
   `packages/teracms-core-1.0.0.zip` dari folder lokal ini.

Rilis berikutnya: jalankan `php artisan teracms:build-release` di repo
sumber, commit + push manifes yang berubah di sini, unggah zip baru ke
Releases dengan tag polanya, pasang di staging lewat kanal beta, amati
24 jam, baru sentuh manifes `stable` (Blueprint Bab 7.5–7.6).
