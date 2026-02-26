---
name: lyra-task-manager
description: "Kelola to-do list, track task, dan reminder deadline. Data disimpan lokal di ~/.lyra/tasks.json. Terintegrasi dengan daily briefing."
homepage: https://github.com/Abielm57/lyra-skill
metadata: {"clawdbot":{"emoji":"✅","requires":{},"install":[]}}
---

# LYRA Task Manager

Simpel, lokal, efektif. Task disimpan di `~/.lyra/data/tasks.json`.

## Format Data Task

```json
{
  "tasks": [
    {
      "id": 1,
      "title": "Belajar Docker",
      "status": "todo",
      "priority": "high",
      "due_date": "2026-03-20",
      "created": "2026-03-01",
      "tags": ["belajar", "coding"]
    }
  ]
}
```

## Command Discord

```
!task                  — semua task aktif
!task tambah [desc]    — tambah task baru
!task tambah [desc] due [tanggal] — tambah dengan deadline
!task selesai [id]     — tandai selesai
!task hapus [id]       — hapus task (konfirmasi dulu)
!task urgent           — prioritas tinggi / jatuh tempo hari ini
!task [id]             — detail task tertentu
```

Contoh natural language:
```
"Lyra, ingatkan aku bayar hosting tanggal 25"
"Tambah task: pelajari Docker, deadline minggu depan, prioritas tinggi"
"Task apa yang harus dikerjain hari ini?"
"Tandai task 3 selesai"
"Hapus semua task yang sudah selesai"
```

## Format Tampilan Task

```
📋 Task Kamu — [tanggal]

🔴 URGENT / Due Hari Ini:
• [1] Bayar hosting ⚠️

🟡 In Progress:
• [2] Belajar React — due: 20 Mar

🟢 Belum Mulai:
• [3] Riset Docker

✅ Selesai Hari Ini: [N] task
Total aktif: [N] task
```

## Reminder Otomatis

LYRA ingatkan task mendekati deadline:
- Pagi (07.00): task yang due hari ini
- Siang (13.00): cek ulang task urgent
- Masuk ke Daily Briefing pagi

Notifikasi:
```
⏰ Reminder Task

🔴 Due hari ini:
• "Bayar hosting" — belum selesai!

🟡 Due besok:
• "Review kode project X"
```
