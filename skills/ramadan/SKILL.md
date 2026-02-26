# SKILL: Ramadan & Islamic Life Assistant

## Deskripsi
LYRA aware jadwal Islam harian — sholat 5 waktu, imsak, buka puasa, konten islami.
Otomatis menyesuaikan lokasi Pasuruan, Jawa Timur. API gratis, tidak butuh key!

---

## API Jadwal Sholat

```
https://api.aladhan.com/v1/timingsByCity?city=Pasuruan&country=Indonesia&method=11
```

Method 11 = Standar KEMENAG Indonesia ✅ (paling akurat untuk Indonesia)

---

## Cron Jobs

```bash
# Update jadwal sholat tiap hari jam 00.01 WIB
1 0 * * * python3 ~/.lyra/skills/ramadan/update-schedule.py

# Cek reminder imsak jam 03.00 WIB
0 3 * * * python3 ~/.lyra/skills/ramadan/imsak-check.py

# Briefing Ramadan pagi jam 04.30 WIB
30 4 * * * python3 ~/.lyra/skills/ramadan/morning-ramadan.py
```

---

## Notifikasi Discord

### 20 Menit Sebelum Imsak
```
⏰ **Imsak 20 menit lagi!**
Imsak: [waktu] | Subuh: [waktu]
Pastikan sahurnya udah selesai ya 🍽️
```

### Waktu Berbuka (Maghrib)
```
🌙 **Alhamdulillah, waktunya berbuka!**
Maghrib: [waktu]
*"Dzahabaz zhoma-u wabtallatil uruuqu wa tsabatal ajru insyaa Allah"*
Selamat berbuka 🤲
```

### Sholat 5 Waktu
```
🕌 Waktunya sholat **[Subuh/Dzuhur/Ashar/Maghrib/Isya]** — [waktu] WIB
```

### Briefing Pagi Ramadan
```
🌅 Selamat pagi & selamat berpuasa! Hari ke-[X] Ramadan [tahun H]

📖 "[Ayat hari ini]" — QS. [Surah]:[ayat]

🕌 Jadwal hari ini:
• Subuh: [waktu] | Dzuhur: [waktu] | Ashar: [waktu]
• Maghrib (buka): [waktu] | Isya: [waktu] | Tarawih: setelah Isya

Semangat puasanya! 💪
```

---

## Command Discord

```
!sholat     — jadwal sholat hari ini
!imsak      — berapa menit lagi imsak
!buka       — berapa menit lagi buka puasa
!puasa      — hari ke berapa Ramadan
!hijriah    — tanggal Hijriah hari ini
!doa [nama] — tampilkan doa (makan, tidur, bepergian, dll)
```

---

## Script: update-schedule.py

```python
#!/usr/bin/env python3
import requests, json
from datetime import datetime
import pytz

WIB = pytz.timezone('Asia/Jakarta')
DATE = datetime.now(WIB).strftime('%d-%m-%Y')

response = requests.get(
    f"https://api.aladhan.com/v1/timingsByCity/{DATE}",
    params={'city': 'Pasuruan', 'country': 'Indonesia', 'method': 11}
)
timings = response.json()['data']['timings']

schedule = {
    'date': DATE,
    'imsak':   timings['Imsak'],
    'subuh':   timings['Fajr'],
    'dzuhur':  timings['Dhuhr'],
    'ashar':   timings['Asr'],
    'maghrib': timings['Maghrib'],
    'isya':    timings['Isha']
}

with open('/tmp/jadwal-sholat.json', 'w') as f:
    json.dump(schedule, f, indent=2)

print(f"✅ Jadwal sholat diupdate: {schedule}")
```
