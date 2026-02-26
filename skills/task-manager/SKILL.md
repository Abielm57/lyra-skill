# SKILL: Task Manager

## Deskripsi
LYRA kelola to-do list, lacak task, dan ingatkan deadline. Data disimpan lokal di Ubuntu kamu.

---

## Command Discord

```
!task               — semua task aktif
!task tambah [desc] — tambah task baru
!task selesai [id]  — tandai selesai
!task urgent        — prioritas tinggi
!task [id] detail   — detail task tertentu
```

Contoh natural language:
```
"Lyra, ingatkan aku bayar hosting tanggal 25"
"Tambah task: pelajari Docker, deadline minggu depan"
"Task apa yang harus dikerjain hari ini?"
```

---

## Format Tampilan

```
📋 Task Kamu

🔴 URGENT:
• [1] Bayar hosting — due: besok ⚠️

🟡 In Progress:
• [2] Belajar React — due: 20 Mar

🟢 Belum Mulai:
• [3] Riset tentang Docker

✅ Selesai Hari Ini: 2 task
```

---

## Script: task-manager.py

```python
#!/usr/bin/env python3
import json
from datetime import date
from pathlib import Path

TASKS_FILE = Path.home() / '.lyra' / 'data' / 'tasks.json'

def load():
    if not TASKS_FILE.exists():
        TASKS_FILE.parent.mkdir(parents=True, exist_ok=True)
        return {'tasks': [], 'last_id': 0}
    return json.loads(TASKS_FILE.read_text())

def save(data):
    TASKS_FILE.write_text(json.dumps(data, ensure_ascii=False, indent=2))

def add(title, due=None, priority='medium'):
    data = load()
    data['last_id'] += 1
    data['tasks'].append({
        'id': data['last_id'], 'title': title,
        'status': 'todo', 'priority': priority,
        'due_date': due, 'created': date.today().isoformat()
    })
    save(data)
    return data['last_id']

def done(task_id):
    data = load()
    for t in data['tasks']:
        if t['id'] == task_id:
            t['status'] = 'done'
            save(data)
            return True
    return False

def get_active():
    return [t for t in load()['tasks'] if t['status'] != 'done']

def get_due_today():
    today = date.today().isoformat()
    return [t for t in get_active() if t.get('due_date') == today]
```

---

## Cron

```bash
# Cek deadline tiap pagi 07.00 dan siang 13.00
0 7,13 * * * python3 ~/.lyra/skills/task-manager/task-manager.py check
```
