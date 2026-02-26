# SKILL: Summarizer

## Deskripsi
LYRA bisa ringkas artikel, video YouTube, PDF, thread, atau teks apapun — hasilnya selalu bahasa Indonesia yang mudah dipahami.

---

## Command Discord

```
!ringkas [URL]          — ringkas dari URL
!ringkas [teks]         — ringkas teks yang di-paste
!tldr [URL]             — versi super singkat (3 poin)
!jelasin [URL/teks]     — ringkas + jelaskan dengan bahasa pemula
```

---

## Format Output

```
📄 Ringkasan
🔗 Sumber: [URL]

Intinya: [1 kalimat]

Poin Penting:
• [poin 1]
• [poin 2]
• [poin 3]

Catatan LYRA: [relevan buat apa?]
```

---

## Dukungan Konten

- 🌐 Artikel web → scraping + ringkasan
- 🎥 YouTube → ambil transcript → ringkas
- 📄 PDF → ekstrak teks → ringkas
- 💬 Teks panjang → paste langsung

---

## Script

```python
#!/usr/bin/env python3
from youtube_transcript_api import YouTubeTranscriptApi
import PyPDF2, requests, re
from bs4 import BeautifulSoup

def detect_type(url):
    if 'youtube.com' in url or 'youtu.be' in url: return 'youtube'
    if url.endswith('.pdf'): return 'pdf'
    return 'web'

def get_youtube_transcript(url):
    vid_id = re.search(r'(?:v=|youtu\.be/)([a-zA-Z0-9_-]{11})', url)
    if not vid_id: return None
    transcript = YouTubeTranscriptApi.get_transcript(vid_id.group(1), languages=['id','en'])
    return ' '.join([t['text'] for t in transcript])[:4000]

def scrape_web(url):
    soup = BeautifulSoup(requests.get(url, timeout=10,
        headers={'User-Agent': 'LYRA/1.0'}).content, 'html.parser')
    for t in soup(['script','style','nav','footer']): t.decompose()
    return soup.get_text(separator=' ', strip=True)[:4000]

def extract_pdf(path):
    reader = PyPDF2.PdfReader(path)
    return ''.join([p.extract_text() for p in reader.pages[:10]])[:5000]

# pip install youtube-transcript-api PyPDF2 beautifulsoup4
```
