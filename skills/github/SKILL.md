---
name: lyra-github
description: "Integrasi GitHub via GitHub CLI (gh). LYRA bisa cari repo, cek notifikasi, baca issues, monitor repo favorit, dan bantu manage proyek GitHub."
homepage: https://cli.github.com
metadata: {"clawdbot":{"emoji":"🐙","requires":{"bins":["gh"]},"install":[{"id":"gh-apt","kind":"run","label":"Install GitHub CLI (apt)","run":"curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg && echo \"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main\" | sudo tee /etc/apt/sources.list.d/github-cli.list && sudo apt update && sudo apt install gh -y"}]}}
---

# LYRA GitHub Integration

GitHub CLI (gh) sudah terinstall. LYRA pakai ini untuk semua operasi GitHub.

## References

- `references/setup.md` — cara login dan konfigurasi gh CLI
- `references/commands.md` — command lengkap yang tersedia

## Setup Pertama Kali

Login ke GitHub:
```bash
gh auth login
```

Ikuti wizard:
1. Pilih GitHub.com
2. Pilih HTTPS
3. Pilih Login with a web browser ATAU paste token
4. Kalau token: buka https://github.com/settings/tokens → buat token → paste

Verifikasi:
```bash
gh auth status
```

## Operasi GitHub

### Cari Repository
```bash
gh search repos [query] --sort stars
gh search repos [query] --language python --sort stars
```

### Info Repository
```bash
gh repo view [user/repo]
gh repo view [user/repo] --web
```

### Lihat Issues
```bash
gh issue list --repo [user/repo]
gh issue view [number] --repo [user/repo]
```

### Lihat Pull Requests
```bash
gh pr list --repo [user/repo]
gh pr view [number] --repo [user/repo]
```

### Notifikasi
```bash
gh api notifications
```

### Clone Repository
```bash
gh repo clone [user/repo]
```

### Buat Repository Baru
```bash
gh repo create [nama] --public
gh repo create [nama] --private
```

### Trending (via web scraping)
LYRA fetch https://github.com/trending untuk repo trending.

## Command Discord

```
!gh cari [query]      — cari repository
!gh trending          — repo trending hari ini
!gh trending [lang]   — trending by bahasa (python, javascript, dll)
!gh repo [user/x]     — info detail repo
!gh issues [user/x]   — lihat issues terbaru
!gh pr [user/x]       — lihat pull requests
!gh notif             — notifikasi GitHub kamu
!gh clone [user/x]    — clone repo ke lokal (konfirmasi dulu)
!gh buat [nama]       — buat repo baru
```

## Monitor Repo Favorit

Pemilik bisa set repo yang mau dipantau. Simpan list di `~/.lyra/github-watch.json`:
```json
{
  "watch": ["user/repo1", "user/repo2"],
  "notify_on": ["release", "issue"]
}
```

LYRA cek setiap 6 jam dan kirim notifikasi kalau ada yang baru.
