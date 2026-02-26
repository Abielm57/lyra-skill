---
name: lyra-daily-briefing
description: "Briefing harian otomatis setiap pagi — jadwal sholat, agenda, email penting, cuaca, dan reminder. Versi Ramadan kirim jam 04.30 WIB dengan ayat harian."
homepage: https://github.com/Abielm57/lyra-skill
metadata: {"clawdbot":{"emoji":"🌅","requires":{},"suggests":["lyra-ramadan","lyra-email","lyra-task-manager"],"install":[]}}
---

# LYRA Daily Briefing

Briefing otomatis setiap pagi — semua yang perlu diketahui dalam satu pesan.

## Jadwal Pengiriman

| Kondisi | Waktu | Channel |
|---------|-------|---------|
| Normal | 07.00 WIB | #lyra-briefing |
| Ramadan | 04.30 WIB | #lyra-briefing |
| Malam (opsional) | 21.00 WIB | #lyra-briefing |

## Sumber Data

- **Jadwal sholat**: AlAdhan API (lihat skill lyra-ramadan)
- **Cuaca**: `https://wttr.in/Pasuruan?format=3` (gratis, tanpa API key)
- **Email**: Himalaya CLI (lihat skill lyra-email)
- **Task**: ~/.lyra/data/tasks.json (lihat skill lyra-task-manager)
- **Kalender**: Google Calendar API (opsional)

## Format Briefing Pagi Normal

```
🌅 Selamat pagi! Briefing [Hari, DD MMM YYYY]

🕌 Waktu Sholat Pasuruan:
Subuh [w] | Dzuhur [w] | Ashar [w] | Maghrib [w] | Isya [w]

📅 Agenda Hari Ini:
• [jam] — [event dari kalender]
• Tidak ada agenda — hari santai!

📧 Email:
• [N] urgent perlu respons | [N] penting

🌤️ Cuaca: [output dari wttr.in]

🔔 Reminder:
• [task yang due hari ini]

💡 Tips: [1 tips produktivitas atau motivasi]
```

## Format Briefing Ramadan (04.30 WIB)

```
🌙 Selamat pagi & selamat berpuasa!
Hari ke-[X] Ramadan [Tahun H] | [DD MMM YYYY]

📖 "[Ayat hari ini dalam Arab]"
"[Terjemahan Indonesia]"
— QS. [Surah]:[ayat]

🕌 Jadwal Hari Ini:
Imsak [w] | Subuh [w] | Dzuhur [w]
Ashar [w] | Maghrib/Buka [w] | Isya [w]
Tarawih: setelah Isya

📅 Agenda: [agenda hari ini jika ada]
🔔 Reminder: [task penting hari ini]

Semangat puasanya! 💪
```

## Format Briefing Malam (Opsional, 21.00)

```
🌙 Recap Hari Ini — [tanggal]

✅ Selesai hari ini: [N task]
📋 Masih pending: [N task]

📅 Besok: [agenda besok]
📧 Email belum dibalas: [N email]

[Kalau Ramadan:]
⏰ Imsak besok: [waktu] (ingat set alarm!)
```

## Cara Cek Cuaca

```bash
curl "https://wttr.in/Pasuruan?format=3"
# Output: Pasuruan: ⛅️  +28°C
```

Format lebih detail:
```bash
curl "https://wttr.in/Pasuruan?format=%l:+%c+%t+%h+humidity"
```
