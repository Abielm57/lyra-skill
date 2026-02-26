---
name: lyra-ramadan
description: "Jadwal sholat 5 waktu, imsak, buka puasa, dan konten islami untuk Pasuruan Jawa Timur. Gratis tanpa API key menggunakan AlAdhan API."
homepage: https://aladhan.com/prayer-times-api
metadata: {"clawdbot":{"emoji":"🌙","requires":{},"install":[]}}
---

# LYRA Ramadan & Islamic Life

Skill ini membuat LYRA aware terhadap waktu sholat, Ramadan, dan kehidupan islami sehari-hari.
Menggunakan AlAdhan API — gratis, tanpa API key, akurat untuk Indonesia.

## References

- `references/aladhan-api.md` — dokumentasi API jadwal sholat
- `references/doa-harian.md` — kumpulan doa harian

## API Jadwal Sholat

```
GET https://api.aladhan.com/v1/timingsByCity/{DD-MM-YYYY}
  ?city=Pasuruan
  &country=Indonesia
  &method=11
```

Method 11 = Kementerian Agama Indonesia (paling akurat untuk Indonesia).

Contoh fetch hari ini:
```bash
curl "https://api.aladhan.com/v1/timingsByCity/$(date +%d-%m-%Y)?city=Pasuruan&country=Indonesia&method=11"
```

Response berisi waktu: Imsak, Fajr (Subuh), Dhuhr (Dzuhur), Asr (Ashar), Maghrib, Isha (Isya).

## Jadwal Reminder Harian

| Waktu | Aksi LYRA |
|-------|-----------|
| Imsak - 20 menit | Kirim reminder imsak ke Discord |
| Subuh | Kirim notif sholat Subuh |
| Dzuhur | Kirim notif sholat Dzuhur |
| Ashar | Kirim notif sholat Ashar |
| Maghrib | Kirim notif buka puasa (Ramadan) + sholat Maghrib |
| Isya | Kirim notif sholat Isya + tarawih (Ramadan) |
| 04:30 WIB | Briefing Ramadan pagi (hanya saat Ramadan) |

## Format Notifikasi

### Reminder Imsak (20 menit sebelum)
```
⏰ Imsak 20 menit lagi!
Imsak: [waktu] | Subuh: [waktu]
Sahurnya udah selesai belum? 🍽️
```

### Waktu Sholat
```
🕌 Waktunya sholat [Subuh/Dzuhur/Ashar/Maghrib/Isya] — [waktu] WIB
```

### Buka Puasa (Maghrib, selama Ramadan)
```
🌙 Alhamdulillah, waktunya berbuka!
Maghrib: [waktu]

"Dzahabaz zhoma-u wabtallatil uruuqu wa tsabatal ajru insyaa Allah"

Selamat berbuka puasa 🤲
```

### Briefing Ramadan (04:30 WIB)
```
🌅 Selamat pagi & selamat berpuasa!
Hari ke-[X] Ramadan [Tahun H]

📖 "[Ayat hari ini]"
— QS. [Surah]:[ayat]

🕌 Jadwal hari ini:
Imsak [w] | Subuh [w] | Dzuhur [w]
Ashar [w] | Maghrib/Buka [w] | Isya [w]
Tarawih: setelah Isya

Semangat puasanya! 💪
```

## Command Discord

```
!sholat      — jadwal sholat lengkap hari ini
!imsak       — berapa menit lagi imsak
!buka        — berapa menit lagi buka puasa
!puasa       — hari ke berapa Ramadan sekarang
!hijriah     — tanggal Hijriah hari ini
!doa makan   — doa sebelum/sesudah makan
!doa tidur   — doa sebelum tidur
!doa safar   — doa bepergian
!quran [x]   — tampilkan ayat (contoh: !quran Al-Baqarah 1-5)
```

## Cek Ramadan

Untuk cek apakah sekarang sedang bulan Ramadan, gunakan endpoint hijriah:
```bash
curl "https://api.aladhan.com/v1/gToH/$(date +%d-%m-%Y)"
```

Cek field `data.hijri.month.number` — kalau 9, berarti Ramadan.
