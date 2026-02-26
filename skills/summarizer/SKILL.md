---
name: lyra-summarizer
description: "Ringkas artikel web, video YouTube, PDF, dan teks panjang. Hasil selalu dalam bahasa Indonesia. Gunakan yt-dlp untuk transcript YouTube dan curl/wget untuk artikel."
homepage: https://github.com/Abielm57/lyra-skill
metadata: {"clawdbot":{"emoji":"📄","requires":{"bins":["curl"]},"suggests":["yt-dlp"],"install":[{"id":"yt-dlp","kind":"run","label":"Install yt-dlp","run":"pip3 install yt-dlp --break-system-packages"}]}}
---

# LYRA Summarizer

Ringkas konten apapun — artikel, video, PDF, atau teks panjang.
Hasil selalu dalam bahasa Indonesia yang mudah dipahami.

## Yang Bisa Diringkas

| Tipe | Cara LYRA |
|------|-----------|
| Artikel web | Fetch URL → ekstrak teks → ringkas |
| Video YouTube | yt-dlp ambil subtitle → ringkas |
| PDF | Baca file → ekstrak teks → ringkas |
| Teks panjang | Paste langsung → ringkas |

## Fetch Artikel Web

```bash
# Ambil konten artikel
curl -sL "[URL]" | \
  sed 's/<[^>]*>//g' | \
  grep -v '^[[:space:]]*$' | \
  head -200
```

## Transcript YouTube

```bash
# Ambil subtitle/transcript video
yt-dlp --write-auto-sub --sub-lang id,en \
  --skip-download --sub-format vtt \
  -o "/tmp/transcript" "[URL_YOUTUBE]"
```

## Command Discord

```
!ringkas [URL]        — ringkas artikel atau video YouTube
!ringkas [teks]       — ringkas teks yang di-paste
!tldr [URL]           — 3 poin utama saja
!jelasin [URL/teks]   — ringkas + jelaskan untuk pemula
```

Contoh natural language:
```
"Lyra, ringkasin artikel ini dong: [URL]"
"TLDR dari video ini apa? [URL YouTube]"
"Ini teks panjang banget, tolong ringkasin: [paste teks]"
"Jelasin dokumentasi ini pake bahasa sederhana: [URL]"
```

## Format Output Standar

```
📄 Ringkasan
🔗 Sumber: [URL/Judul]
📅 [Tanggal jika ada]

Intinya (1 kalimat):
[satu kalimat yang menangkap inti konten]

Poin Penting:
• [poin 1]
• [poin 2]
• [poin 3]
• [poin 4 jika ada]

Catatan LYRA:
[Relevan buat apa? Worth reading/watching atau tidak?]

⏱️ Estimasi baca asli: [X menit]
```

## Format TLDR

```
⚡ TLDR: [judul]

1. [poin 1]
2. [poin 2]
3. [poin 3]
```

## Catatan

- Bahasa output selalu Indonesia, meski sumber bahasa Inggris
- Kalau URL tidak bisa diakses (paywall, login), LYRA kasih tahu
- Untuk video tanpa subtitle, LYRA coba auto-subtitle dulu
