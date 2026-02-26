---
name: lyra-web-search
description: "Web search gratis tanpa API key menggunakan DuckDuckGo. LYRA bisa cari informasi, berita terbaru, artikel, Wikipedia, dan riset topik apapun langsung dari chat."
homepage: https://duckduckgo.com
metadata: {"clawdbot":{"emoji":"🔍","requires":{"bins":["curl"]},"install":[]}}
---

# LYRA Web Search — DuckDuckGo (Gratis, Tanpa API Key)

DuckDuckGo punya endpoint yang bisa dipakai langsung tanpa API key.
Tidak perlu setup, tidak perlu daftar, langsung jalan.

## References

- `references/search-tips.md` — tips cari yang efektif

## Cara Kerja

### Instant Answer API (Gratis, Tanpa Key)
```bash
curl -s "https://api.duckduckgo.com/?q=[QUERY]&format=json&no_html=1&skip_disambig=1"
```

Response berisi:
- `AbstractText` — ringkasan singkat topik
- `AbstractURL` — sumber utama
- `RelatedTopics` — topik terkait

### Cari Wikipedia Indonesia
```bash
curl -s "https://id.wikipedia.org/api/rest_v1/page/summary/[TOPIK]"
```

### Fetch Konten Artikel
```bash
curl -sL "[URL]" | sed 's/<[^>]*>//g' | grep -v '^[[:space:]]*$' | head -100
```

### Fallback: SearXNG Publik
Kalau DDG tidak cukup, pakai instance SearXNG gratis:
```bash
curl -s "https://searx.be/search?q=[QUERY]&format=json&language=id"
```
Instance lain: `https://searxng.world`, `https://search.bus-hit.me`

## Command Discord

```
!cari [query]      — cari informasi umum
!berita [topik]    — berita dan update terbaru
!wiki [topik]      — ringkasan Wikipedia Indonesia
!riset [topik]     — riset mendalam dari banyak sumber
```

Contoh natural language:
```
"Lyra, cariin cara setup Docker di Ubuntu"
"Apa itu machine learning?"
"Berita teknologi terbaru"
"Riset tentang framework Python terbaik 2025"
```

## Format Hasil

```
🔍 Hasil: "[query]"

📝 [Jawaban langsung / ringkasan]

🔗 Sumber: [URL]

Topik terkait:
• [topik 1]
• [topik 2]

Mau aku cari lebih dalam?
```

## Format Riset Mendalam

```
📋 Riset: [Topik]

Ringkasan:
[2-3 kalimat inti]

Temuan Utama:
• [poin 1 + sumber]
• [poin 2 + sumber]
• [poin 3 + sumber]

Sumber:
• [URL 1] • [URL 2] • [URL 3]
```
