# scoop-quarkdown (fork)

Scoop bucket for [Quarkdown](https://github.com/iamgio/quarkdown) — a modern Markdown-based typesetting system.

Fork of [quarkdown-labs/scoop-quarkdown](https://github.com/quarkdown-labs/scoop-quarkdown) with customizations.

## Customizations vs Upstream

| Perubahan | Fork (ini) | Upstream |
|-----------|-----------|----------|
| `depends` | `[]` (kosong) | `["nodejs-lts"]` |
| `post_install` | Tanpa `set PATH` untuk nodejs | Menambahkan nodejs ke `PATH` |

Alasan: fork ini menggunakan bundled browser dari Puppeteer (lihat [PR #157](https://github.com/iamgio/quarkdown/pull/157)), sehingga tidak perlu dependency `nodejs-lts` dan tidak perlu menambahkan nodejs ke `PATH`.

## Cara Update ke Versi Baru

### 1. Update file `bucket/quarkdown.json`

Ubah tiga field berikut:

- `"version"` → versi baru
- `"url"` → sesuaikan URL download dengan versi baru
- `"hash"` → hash SHA-256 dari zip versi baru

**Jangan** ubah `"depends"` — tetap `[]`.
**Jangan** tambahkan kembali baris `set PATH` untuk nodejs di `post_install`.

### 2. Dapatkan hash

```powershell
# Download dulu, lalu:
Get-FileHash quarkdown-windows-x64.zip -Algorithm SHA256
```

### 3. Commit dan push

```powershell
git add bucket/quarkdown.json
git commit -m "Update quarkdown to vX.Y.Z"
git push origin main
```

## Cara Sinkron dengan Upstream (Rebase)

Jika upstream punya commit baru (misal perubahan infrastruktur bucket):

```powershell
# 1. Fetch upstream
git fetch upstream

# 2. Rebase di atas upstream/main
git rebase upstream/main

# 3. Pastikan perubahan fork masih terjaga
#    - depends: [] (bukan ["nodejs-lts"])
#    - Tidak ada baris 'set PATH=...nodejs-lts...' di post_install
#    - Jika ada conflict, resolve dengan mempertahankan versi fork

# 4. Force push
git push --force-with-lease origin main
```

> **Penting:** Setelah rebase, selalu verifikasi bahwa 2 perubahan fork di atas masih utuh.
