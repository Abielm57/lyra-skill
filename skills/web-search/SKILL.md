# SKILL: Web Search & Research

## Deskripsi
LYRA bisa cari informasi di Google, baca artikel, ringkas konten web, dan riset topik apapun. Hasil selalu dalam bahasa Indonesia.

---

## Setup (Pilih salah satu)

### Opsi A: Google Custom Search (100 query/hari gratis)
1. Buka https://programmablesearchengine.google.com → buat search engine → centang "Search the entire web"
2. Salin Search Engine ID
3. Di https://console.cloud.google.com → Enable "Custom Search API" → buat API Key
4. Simpan ke `~/.lyra/.env`:
```bash
GOOGLE_SEARCH_API_KEY=your_key_here
GOOGLE_SEARCH_ENGINE_ID=your_engine_id_here
```

### Opsi B: SerpAPI (100 query/bulan gratis)
```bash
SERPAPI_KEY=your_key_here
```

---

## Command Discord

```
!cari [query]         — cari di Google
!ringkas [URL]        — ringkas artikel dari URL
!riset [topik]        — riset mendalam dari banyak sumber
!youtube [query]      — cari video YouTube
!berita [topik]       — berita terbaru
!wiki [topik]         — ringkasan Wikipedia
```

---

## Format Hasil

```
🔍 Hasil: "[query]"

1. [Judul] — [URL]
   [Ringkasan 2 kalimat]

2. [Judul] — [URL]
   [Ringkasan 2 kalimat]

💡 Kesimpulan LYRA: [jawaban langsung ke pertanyaan]
```

---

## Script: web-search.py

```python
#!/usr/bin/env python3
import os, requests
from bs4 import BeautifulSoup

def google_search(query, num=5):
    url = "https://www.googleapis.com/customsearch/v1"
    params = {
        'key': os.getenv('GOOGLE_SEARCH_API_KEY'),
        'cx':  os.getenv('GOOGLE_SEARCH_ENGINE_ID'),
        'q':   query, 'num': num,
        'lr':  'lang_id', 'gl': 'id'
    }
    data = requests.get(url, params=params).json()
    return [{'title': i['title'], 'url': i['link'],
             'snippet': i['snippet']} for i in data.get('items', [])]

def scrape_url(url):
    try:
        soup = BeautifulSoup(
            requests.get(url, timeout=10,
                headers={'User-Agent': 'LYRA-Agent/1.0'}).content, 'html.parser')
        for tag in soup(['script','style','nav','footer']):
            tag.decompose()
        return soup.get_text(separator=' ', strip=True)[:3000]
    except Exception as e:
        return f"Tidak bisa akses: {e}"
```
