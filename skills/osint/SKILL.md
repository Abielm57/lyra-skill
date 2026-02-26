---
name: lyra-osint
description: "Cek kebocoran data pribadi pemilik — email bocor via HaveIBeenPwned, nomor HP, jejak digital, dan audit privasi. HANYA untuk data pemilik sendiri."
homepage: https://haveibeenpwned.com/API/v3
metadata: {"clawdbot":{"emoji":"🕵️","requires":{"bins":["curl"]},"install":[]}}
---

# LYRA OSINT & Perlindungan Data Pribadi

Cek apakah data pribadi pemilik sudah bocor atau tersebar di internet.
Skill ini HANYA untuk melindungi data pemilik sendiri.

## References

- `references/hibp-api.md` — dokumentasi HaveIBeenPwned API
- `references/privacy-checklist.md` — checklist keamanan digital

## Cek Email Bocor (HaveIBeenPwned)

### Tanpa API Key (Basic)
```bash
curl -s "https://haveibeenpwned.com/api/v3/breachedaccount/[EMAIL]" \
  -H "hibp-api-key: [API_KEY]" \
  -H "user-agent: LYRA-PrivacyCheck"
```

### Setup API Key (Gratis terbatas)
1. Buka https://haveibeenpwned.com/API/Key
2. Daftar dengan email
3. Kirim API key ke LYRA

### Tanpa API Key (Alternatif)
LYRA bisa cek via:
```bash
# Cek via dehashed.com atau leakcheck.io (beberapa gratis)
curl -s "https://leakcheck.io/api/public?check=[EMAIL]"
```

## Cek Password Bocor (Tanpa kirim password ke server)

Menggunakan k-Anonymity — password tidak pernah dikirim utuh:
```bash
# Hash password dengan SHA1, ambil 5 karakter pertama
HASH=$(echo -n "[PASSWORD]" | sha1sum | cut -c1-5 | tr '[:lower:]' '[:upper:]')

# Kirim hanya 5 karakter pertama
curl -s "https://api.pwnedpasswords.com/range/$HASH"

# Cek apakah sisa hash ada di response
```

## Cek Jejak Digital

```bash
# Cek informasi dari username/nama via sherlock (kalau terinstall)
sherlock [username]

# Atau manual search via SearXNG
# Cari: "[nama]" site:linkedin.com OR site:facebook.com OR site:twitter.com
```

## Command Discord

```
!osint email [x]    — cek email bocor
!osint audit        — audit privasi lengkap
!osint laporan      — laporan lengkap semua risiko
!osint monitor      — aktifkan monitoring bulanan
```

## Format Laporan Privacy

```
🔍 Privacy Report — [tanggal]

📧 EMAIL: [email]
Status: 🔴 BOCOR / 🟢 AMAN

🔴 Ditemukan di [N] database kebocoran:
• [Nama breach] — [tanggal] — Data: [tipe data]
• [Nama breach] — [tanggal] — Data: [tipe data]

✅ Rekomendasi:
1. Ganti password segera di semua akun yang pakai email ini
2. Aktifkan 2FA di semua akun penting
3. Cek apakah ada akun yang sudah dikompromis

🛡️ Skor Privacy: [X]/100
Risk Level: [LOW/MEDIUM/HIGH]
```

## Monitor Bulanan

Setiap bulan LYRA cek ulang dan kirim notif kalau ada kebocoran baru:
```
🚨 Security Alert!

Email kamu baru ditemukan di breach baru:
Breach: [nama] | Tanggal: [tanggal]
Data bocor: [tipe]

Segera ganti password untuk layanan ini!
```

## Etika

LYRA HANYA lakukan OSINT untuk:
- Data pribadi pemilik sendiri
- Verifikasi bisnis/orang yang akan diajak kerjasama (dengan izin eksplisit pemilik)

LYRA TIDAK akan investigasi orang lain tanpa izin yang jelas.
