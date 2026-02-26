# 📖 TUTORIAL: Cara Install LYRA ke OpenClaw
### Panduan untuk Pemula — Ikuti Urutan Ini!

---

## TAHAP 0: Persiapan File di Ubuntu

Buka terminal Ubuntu kamu, lalu jalankan:

```bash
# Buat folder LYRA
mkdir -p ~/.lyra/skills
mkdir -p ~/.lyra/credentials
mkdir -p ~/.lyra/data
mkdir -p ~/.lyra/backups
mkdir -p ~/.lyra/config

# Pindahkan folder skill yang udah kita buat ke sini
# (Setelah kamu extract zip-nya)
cp -r ~/Downloads/lyra-skills/skills/* ~/.lyra/skills/
cp ~/Downloads/lyra-skills/SOUL.md ~/.lyra/SOUL.md
cp ~/Downloads/lyra-skills/MEMORY.md ~/.lyra/MEMORY.md

echo "✅ File LYRA siap!"
```

---

## TAHAP 1: First Contact di Discord

Buka Discord → server coding kamu → channel mana aja yang terhubung OpenClaw.

**Ketik pesan ini:**
```
Halo!
```

Tunggu OpenClaw balas. Biasanya dia bakal menyapa dan nanya nama kamu.

---

## TAHAP 2: Kasih Kepribadian LYRA

Setelah OpenClaw balas, kirim pesan ini (copy-paste langsung):

```
Aku mau kamu jadi LYRA — asisten AI pribadi perempuan yang cerdas dan proaktif.

Tolong baca dan terapkan identitas dari file ini:
```

Lalu copy-paste **seluruh isi** file `SOUL.md` langsung ke Discord.

Tunggu OpenClaw konfirmasi dia udah paham identitasnya.

---

## TAHAP 3: Install Skill RAMADAN (Prioritas!)

Karena lagi bulan Ramadan, install ini duluan.

Kirim ke Discord:
```
Lyra, install skill ini dan aktifkan sekarang:
```

Lalu copy-paste **seluruh isi** file `skills/ramadan/SKILL.md` ke Discord.

LYRA akan:
1. Baca skill tersebut
2. Setup cron job otomatis
3. Konfirmasi skill aktif
4. Test dengan kirim jadwal sholat hari ini

**Test:** Ketik `!sholat` → harusnya muncul jadwal sholat Pasuruan hari ini.

---

## TAHAP 4: Install Daily Briefing

```
Lyra, install skill ini:
```

Copy-paste isi `skills/daily-briefing/SKILL.md`

**Test:** Minta LYRA kirimin briefing sekarang:
```
Lyra, kirimin briefing hari ini dong
```

---

## TAHAP 5: Install Task Manager

```
Lyra, install skill task manager ini:
```

Copy-paste isi `skills/task-manager/SKILL.md`

**Test:**
```
!task tambah Belajar cara install skill LYRA — deadline besok
```

---

## TAHAP 6: Install Coding Agent

```
Lyra, install skill coding agent ini:
```

Copy-paste isi `skills/code-agent/SKILL.md`

**Test:**
```
Lyra, buatin aku script Python sederhana yang print "Halo, aku LYRA!" 5 kali
```

---

## TAHAP 7: Install Self-Upgrade

```
Lyra, install skill self-upgrade ini supaya kamu bisa berkembang sendiri:
```

Copy-paste isi `skills/self-upgrade/SKILL.md`

---

## TAHAP 8: Install OSINT (Keamanan Data)

```
Lyra, install skill OSINT ini untuk jaga keamanan dataku:
```

Copy-paste isi `skills/osint/SKILL.md`

Setelah install, langsung test:
```
!osint email emailkamu@gmail.com
```

---

## TAHAP 9: Install Skill Lainnya

Install satu per satu dengan cara yang sama:

```
Lyra, install skill [nama] ini:
[paste isi SKILL.md]
```

Urutan:
- `skills/email/SKILL.md` → butuh setup Google API dulu (ada di file-nya)
- `skills/calendar/SKILL.md` → butuh Google Calendar API
- `skills/web-search/SKILL.md` → butuh Google Search API key
- `skills/github/SKILL.md` → butuh GitHub token
- `skills/summarizer/SKILL.md` → langsung bisa pakai
- `skills/clawsec/SKILL.md` → install TERAKHIR

---

## TAHAP 10: Setup Memory

Kirim ke Discord:
```
Lyra, ini template memory kamu. Tolong simpan dan update terus dari percakapan kita:
```

Copy-paste isi `MEMORY.md`

---

## CARA ALTERNATIF: Via GitHub (Opsional)

Kalau mau LYRA bisa akses skill langsung dari internet:

1. Buat akun GitHub kalau belum punya: https://github.com
2. Buat repository baru → nama: `lyra-skills` → Private
3. Upload folder `lyra-skills` ke repo tersebut
4. Kirim ke OpenClaw di Discord:

```
Lyra, kamu bisa install skill dari repo GitHub ini:
https://github.com/USERNAME_KAMU/lyra-skills

Mulai dengan baca README.md dulu, terus install semua skill secara berurutan.
```

---

## Troubleshooting Umum

### LYRA tidak merespons skill baru
→ Coba kirim ulang isi SKILL.md dengan prefix yang jelas:
```
Lyra, ini adalah skill baru yang harus kamu install dan aktifkan:
[paste SKILL.md]
```

### Jadwal sholat tidak muncul
→ Cek koneksi internet Ubuntu:
```bash
curl -s "https://api.aladhan.com/v1/timingsByCity?city=Pasuruan&country=Indonesia&method=11" | python3 -m json.tool
```
Kalau muncul data → API OK. Beritahu LYRA untuk re-setup skill ramadan.

### LYRA lupa kepribadiannya
→ Kirim ulang SOUL.md:
```
Lyra, ini identitasmu. Tolong ingat dan terapkan:
[paste SOUL.md]
```

### Error saat install Python dependencies
```bash
pip3 install requests python-dateutil pytz beautifulsoup4 \
     google-api-python-client google-auth-httplib2 \
     google-auth-oauthlib youtube-transcript-api PyPDF2
```

---

## Cheat Sheet Command LYRA

Setelah semua skill terinstall:

| Command | Fungsi |
|---------|--------|
| `!sholat` | Jadwal sholat hari ini |
| `!buka` | Berapa menit lagi buka puasa |
| `!imsak` | Berapa menit lagi imsak |
| `!task` | Lihat semua task |
| `!task tambah [desc]` | Tambah task baru |
| `!cari [query]` | Cari di Google |
| `!ringkas [URL]` | Ringkas artikel |
| `!gh cari [query]` | Cari repo GitHub |
| `!email` | Cek email terbaru |
| `!osint email [email]` | Cek email bocor |
| `!code buat [desc]` | Generate kode |
| `!code fix [kode/error]` | Debug kode |

---

## Setelah Semua Terinstall — Test Final

Kirim ke Discord:
```
Lyra, perkenalkan dirimu dan kasih tau skill apa aja yang udah kamu punya sekarang.
```

LYRA harusnya memperkenalkan diri sebagai LYRA, list semua skill yang aktif, dan siap menerima perintah!

---

*Selamat! LYRA kamu udah hidup dan siap kerja 🎉*
