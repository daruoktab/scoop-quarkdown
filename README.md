# scoop-quarkdown (fork)

Scoop bucket for [Quarkdown](https://github.com/iamgio/quarkdown) — a modern Markdown-based typesetting system.

Fork of [quarkdown-labs/scoop-quarkdown](https://github.com/quarkdown-labs/scoop-quarkdown).

## Status Fork

Sejak **v2.6.0**, upstream sudah mengadopsi pendekatan yang sebelumnya menjadi ciri khas fork ini:
tidak lagi bergantung pada `nodejs-lts` / npm puppeteer. Upstream kini meng-install
`chrome-headless-shell` via `scripts/install-chrome.ps1` yang dibundel di dalam zip, dan
men-set `QD_CHROME_PATH` di `bin/quarkdown.bat`.

**Konsekuensinya: manifest `bucket/quarkdown.json` di fork ini identik dengan upstream.**
Satu-satunya perbedaan fork adalah file `README.md` ini.

## Cara Update ke Versi Baru (Sederhana)

Karena manifest sudah identik dengan upstream, update cukup menyalin versi terbaru upstream:

```powershell
# 1. Fetch dan lihat versi terbaru upstream
git fetch upstream
git --no-pager log --oneline upstream/main ^main

# 2. Ambil penuh manifest upstream (tidak ada customisasi yang perlu dipertahankan)
git checkout upstream/main -- bucket/quarkdown.json

# 3. Commit dan push
git add bucket/quarkdown.json
git commit -m "Update quarkdown to vX.Y.Z"
git push origin main
```

> Jika isi file berubah struktural (misal `post_install`, `depends`), cukup terima perubahan
> upstream apa adanya — fork ini mengikuti upstream secara penuh.

## Cara Sinkron Penuh dengan Upstream

```powershell
git fetch upstream
git reset --hard upstream/main
git cherry-pick <commit-README-terakhir>
git push --force-with-lease origin main
```

> `git reset --hard` menghapus riwayat lokal yang berbeda dari upstream. Buat branch backup
> (`git branch backup/<nama>`) dulu jika ada commit lokal yang ingin disimpan.

## Cara Test Manifest Lokal Sebelum Push

```powershell
# Arahkan clone bucket scoop ke repo lokal dulu
git -C "$HOME\scoop\buckets\quarkdown" remote add local D:\Codings\scoop-quarkdown
git -C "$HOME\scoop\buckets\quarkdown" fetch local
git -C "$HOME\scoop\buckets\quarkdown" reset --hard local/main

# Update app dari manifest lokal
scoop update quarkdown

# Test versi dan konversi PDF
quarkdown --version
quarkdown compile <file.qd> --pdf --out <output-dir> --timeout 120
```

> Catatan: nama bucket & path mungkin berbeda (`scoop bucket list` untuk cek).
> Setelah test, kembalikan clone bucket ke remote dan hapus remote `local`-nya.
