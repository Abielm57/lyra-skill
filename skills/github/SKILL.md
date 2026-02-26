# SKILL: GitHub Integration

## Deskripsi
LYRA bisa berinteraksi dengan GitHub — cari repo, baca kode, monitor notifikasi, dan bantu manage proyek kamu.

---

## Setup

1. Buka https://github.com/settings/tokens → Generate new token (classic)
2. Centang scope: `repo`, `read:user`
3. Simpan ke `~/.lyra/.env`:
```bash
GITHUB_TOKEN=ghp_your_token_here
GITHUB_USERNAME=username_kamu
```

---

## Command Discord

```
!gh cari [query]         — cari repository
!gh trending             — repo trending hari ini
!gh trending python      — trending by bahasa
!gh repo [user/repo]     — info detail repo
!gh issues [user/repo]   — lihat issues terbaru
!gh notif               — cek notifikasi GitHub kamu
!gh readme [user/repo]  — baca README repo
```

---

## Monitor Repo Favorit

Edit `~/.lyra/config/github-watch.json`:
```json
{
  "watch": ["username/repo-kamu"],
  "notify_on": ["new_release", "new_issue"]
}
```

Notifikasi otomatis:
```
🔔 GitHub Alert — [user/repo]
🆕 Release baru: v2.0.0
[ringkasan changelog]
```

---

## Script: github-search.py

```python
#!/usr/bin/env python3
import os, requests

HEADERS = {
    'Authorization': f"token {os.getenv('GITHUB_TOKEN')}",
    'Accept': 'application/vnd.github.v3+json'
}

def search_repos(query, limit=5):
    data = requests.get(
        "https://api.github.com/search/repositories",
        headers=HEADERS,
        params={'q': query, 'sort': 'stars', 'order': 'desc', 'per_page': limit}
    ).json()
    
    return [{
        'name':        r['full_name'],
        'description': r.get('description', '-'),
        'stars':       r['stargazers_count'],
        'language':    r.get('language', 'Unknown'),
        'url':         r['html_url']
    } for r in data.get('items', [])]

def get_notifications():
    return requests.get(
        "https://api.github.com/notifications", headers=HEADERS).json()
```
