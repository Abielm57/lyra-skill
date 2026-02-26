---
name: lyra-self-upgrade
description: "LYRA bisa berkembang sendiri — deteksi gap kemampuan, buat skill baru, dan belajar dari setiap percakapan. Weekly self-review otomatis setiap Minggu malam."
homepage: https://github.com/Abielm57/lyra-skill
metadata: {"clawdbot":{"emoji":"🔧","requires":{},"install":[]}}
---

# LYRA Self-Upgrade

LYRA bisa berkembang sendiri tanpa diperintah.

## Cara Kerja

### 1. Deteksi Gap
LYRA mengenali situasi yang belum bisa dihandle ketika:
- Ada permintaan yang tidak ada skill-nya
- Pola permintaan sama muncul lebih dari 3 kali
- Pemilik bilang "harusnya bisa..." atau "kenapa ga bisa..."

Kalau terdeteksi gap, LYRA tanya dulu:
```
Hmm, aku belum punya skill untuk [X].
Mau aku buatin skill baru buat ini?
Kira-kira butuh waktu sebentar untuk aku pelajari dan setup.
```

### 2. Buat Skill Baru
Setelah dapat izin, LYRA:
1. Research cara terbaik handle situasi tersebut
2. Tulis SKILL.md baru mengikuti format OpenClaw
3. Test apakah bisa dijalankan
4. Simpan ke `~/.lyra/skills/[nama-skill]/SKILL.md`
5. Notifikasi pemilik

Format skill baru yang LYRA buat mengikuti template:
```
---
name: lyra-[nama]
description: "[deskripsi singkat]"
homepage: [referensi]
metadata: {"clawdbot":{"emoji":"[emoji]","requires":{},"install":[]}}
---

# [Nama Skill]

## Deskripsi
[apa yang skill ini lakukan]

## Cara Pakai
[instruksi jelas]

## Command
[command yang bisa dipakai]
```

### 3. Notifikasi ke Pemilik
```
🔧 LYRA upgrade diri sendiri!

Skill baru: [nama]
Kenapa dibuat: [alasan]
Sekarang bisa: [kemampuan baru]

Coba dengan: [contoh command]
```

### 4. Belajar dari Percakapan
LYRA aktif update MEMORY.md dengan:
- Preferensi pemilik yang terdeteksi dari feedback
- Topik yang sering ditanyakan
- Pola jam aktif dan kebiasaan
- Proyek dan ide yang pernah disebut

## Weekly Self-Review

Setiap Minggu malam, LYRA kirim ringkasan:

```
📊 Weekly Review LYRA
Minggu ke-[X], [tanggal]

✅ Yang berjalan baik:
• [skill X dipakai N kali]
• [workflow Y berhasil]

⚠️ Yang perlu diperbaiki:
• [error atau gap yang ditemukan]

🆕 Skill baru yang dibuat minggu ini:
• [nama] — [alasan]

💡 Saran untuk minggu depan:
• [rekomendasi LYRA]

Mau aku fokus ke skill mana minggu depan?
```

## Batasan Self-Upgrade

LYRA TIDAK akan membuat skill yang:
- Melanggar aturan keamanan utama (lihat lyra-clawsec)
- Akses data pribadi orang lain
- Memodifikasi SOUL.md tanpa izin eksplisit pemilik
