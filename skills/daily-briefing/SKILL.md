# SKILL: Daily Briefing

## Deskripsi
LYRA kirim briefing harian otomatis tiap pagi — semua yang perlu kamu tahu dalam satu pesan.

---

## Jadwal

```bash
# Briefing pagi normal — 07.00 WIB
0 7 * * * python3 ~/.lyra/skills/daily-briefing/morning-brief.py

# Briefing Ramadan — 04.30 WIB (aktif selama Ramadan)
30 4 * * * python3 ~/.lyra/skills/daily-briefing/ramadan-brief.py

# Briefing malam (opsional) — 21.00 WIB
0 21 * * * python3 ~/.lyra/skills/daily-briefing/evening-brief.py
```

---

## Format Briefing Pagi Normal

```
🌅 Selamat pagi! Briefing kamu — [Hari, Tanggal]

🕌 Waktu Sholat:
Subuh [waktu] | Dzuhur [waktu] | Ashar [waktu] | Maghrib [waktu] | Isya [waktu]

📅 Agenda Hari Ini:
• [jam] — [event]
• Tidak ada agenda — hari santai!

📧 Email:
• [N] email urgent | [N] email penting

🌤️ Cuaca Pasuruan: [suhu]°C, [kondisi]

🔔 Reminder: [task/reminder yang jatuh tempo]

💡 [Tips atau motivasi harian]
```

---

## Format Briefing Ramadan (04.30 WIB)

```
🌙 Selamat pagi & selamat berpuasa!
Hari ke-[X] Ramadan [tahun H]

📖 "[Ayat hari ini]" — QS. [Surah]:[ayat]

🕌 Jadwal:
Subuh [waktu] | Dzuhur [waktu] | Ashar [waktu]
Maghrib/Buka: [waktu] | Isya: [waktu] | Tarawih: setelah Isya

📅 Agenda: [agenda hari ini]

Semangat puasanya! 💪
```

---

## Script: morning-brief.py

```python
#!/usr/bin/env python3
import json, requests
from datetime import datetime
import pytz

WIB = pytz.timezone('Asia/Jakarta')
NOW = datetime.now(WIB)

# Load jadwal sholat
with open('/tmp/jadwal-sholat.json') as f:
    sholat = json.load(f)

# Cek cuaca via wttr.in (gratis, tanpa API key)
weather = requests.get('https://wttr.in/Pasuruan?format=3').text

# Kirim ke Discord webhook
WEBHOOK = "https://discord.com/api/webhooks/YOUR_WEBHOOK_URL"

message = f"""🌅 **Selamat pagi! Briefing {NOW.strftime('%A, %d %B %Y')}**

🕌 **Waktu Sholat:**
Subuh {sholat['subuh']} | Dzuhur {sholat['dzuhur']} | Ashar {sholat['ashar']}
Maghrib {sholat['maghrib']} | Isya {sholat['isya']}

🌤️ **Cuaca:** {weather}
"""

requests.post(WEBHOOK, json={'content': message})
print("Briefing terkirim!")
```
