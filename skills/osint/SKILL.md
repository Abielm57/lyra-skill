# SKILL: OSINT & Perlindungan Data Pribadi

## Deskripsi
LYRA bisa membantu cek kebocoran data pribadi kamu — email, nomor HP, nama, dan informasi lain yang mungkin sudah tersebar di internet tanpa kamu tahu.

**PENTING:** Skill ini HANYA untuk melindungi data PEMILIK sendiri. Tidak untuk investigasi orang lain tanpa izin.

---

## Kemampuan

- ✅ Cek apakah email kamu bocor (HaveIBeenPwned)
- ✅ Cek nomor HP apakah ada di database spam/leak
- ✅ Cari jejak digital kamu di internet
- ✅ Monitor dark web mention (basic)
- ✅ Cek domain/website kamu ada vulnerability
- ✅ Audit akun media sosial yang terhubung
- ✅ Generate laporan privacy risk

---

## Command Discord

```
!osint email [emailkamu@gmail.com]    — cek email bocor
!osint hp [08xxxxxxxxxx]              — cek nomor HP
!osint nama [nama kamu]               — cari jejak digital
!osint audit                          — audit privacy menyeluruh
!osint laporan                        — laporan lengkap semua risiko
!osint monitor                        — aktifkan monitoring otomatis
```

---

## Cek Email Bocor (HaveIBeenPwned)

```python
#!/usr/bin/env python3
"""
Cek apakah email ada di database kebocoran data
Menggunakan HaveIBeenPwned API (gratis)
"""

import requests
import hashlib

HIBP_API = "https://haveibeenpwned.com/api/v3"
HIBP_KEY = "your_hibp_api_key"  # Daftar di haveibeenpwned.com/API

def check_email_breach(email):
    """Cek kebocoran email"""
    headers = {
        'hibp-api-key': HIBP_KEY,
        'user-agent': 'LYRA-PrivacyCheck/1.0'
    }
    
    url = f"{HIBP_API}/breachedaccount/{email}"
    params = {'truncateResponse': 'false'}
    
    response = requests.get(url, headers=headers, params=params)
    
    if response.status_code == 404:
        return {
            'status': 'AMAN',
            'breaches': [],
            'message': 'Email kamu tidak ditemukan di database kebocoran data yang diketahui ✅'
        }
    elif response.status_code == 200:
        breaches = response.json()
        return {
            'status': 'BOCOR',
            'count': len(breaches),
            'breaches': [{
                'nama': b['Name'],
                'tanggal': b['BreachDate'],
                'data_bocor': b['DataClasses'],
                'keterangan': b['Description'][:200]
            } for b in breaches]
        }

def check_password_pwned(password):
    """
    Cek apakah password pernah ada di database leak
    Menggunakan k-Anonymity — password TIDAK dikirim ke server
    """
    # Hash password dengan SHA1
    sha1 = hashlib.sha1(password.encode()).hexdigest().upper()
    prefix = sha1[:5]
    suffix = sha1[5:]
    
    # Kirim hanya 5 karakter pertama (aman, tidak bocorkan password)
    response = requests.get(f"https://api.pwnedpasswords.com/range/{prefix}")
    
    # Cek apakah suffix ada di respons
    hashes = response.text.splitlines()
    for h in hashes:
        hash_suffix, count = h.split(':')
        if hash_suffix == suffix:
            return {
                'status': 'BERBAHAYA',
                'count': int(count),
                'message': f'Password ini pernah bocor {count} kali! Ganti sekarang!'
            }
    
    return {
        'status': 'AMAN',
        'message': 'Password tidak ditemukan di database kebocoran yang diketahui ✅'
    }
```

---

## Cek Nomor HP

```python
def check_phone_number(phone_number):
    """
    Cek nomor HP di beberapa sumber:
    1. Truecaller (via scraping/API)
    2. GetContact
    3. Database spam yang publik
    """
    results = {
        'number': phone_number,
        'risks': []
    }
    
    # Cek format nomor
    # Normalkan ke format internasional
    if phone_number.startswith('08'):
        intl_number = '+62' + phone_number[1:]
    elif phone_number.startswith('628'):
        intl_number = '+' + phone_number
    else:
        intl_number = phone_number
    
    # Cek via NumVerify API (gratis 250/bulan)
    numverify_key = "your_numverify_key"
    url = f"http://apilayer.net/api/validate?access_key={numverify_key}&number={intl_number}"
    response = requests.get(url)
    data = response.json()
    
    results['valid'] = data.get('valid', False)
    results['carrier'] = data.get('carrier', 'Unknown')
    results['location'] = data.get('location', 'Unknown')
    
    return results
```

---

## Format Laporan Privacy

```
🔍 **LYRA Privacy Audit Report**
📅 Tanggal: [tanggal]

━━━━━━━━━━━━━━━━━━━━━━

📧 **EMAIL: [email kamu]**
Status: 🔴 BOCOR / 🟢 AMAN

🔴 Ditemukan di [N] database kebocoran:
• [Nama breach] — [tanggal] — Data: [tipe data yang bocor]
• [Nama breach] — [tanggal] — Data: [tipe data yang bocor]

⚠️ Risiko: Password kamu mungkin sudah diketahui orang lain

✅ Rekomendasi:
1. Ganti password segera
2. Aktifkan 2FA di semua akun
3. Cek akun-akun yang pakai email ini

━━━━━━━━━━━━━━━━━━━━━━

📱 **NOMOR HP: [nomor]**
Status: 🟡 PERLU PERHATIAN
• Terdaftar sebagai: [nama di Truecaller jika ada]
• Operator: [operator]
• Laporan spam: [ada/tidak]

━━━━━━━━━━━━━━━━━━━━━━

🌐 **JEJAK DIGITAL**
• Google results: [N] hasil ditemukan
• Media sosial: [platform yang terdeteksi]
• Data publik: [apa yang bisa ditemukan orang]

━━━━━━━━━━━━━━━━━━━━━━

🛡️ **SKOR PRIVACY: [X]/100**
[LOW/MEDIUM/HIGH RISK]

📋 **Action Items:**
1. [aksi prioritas 1]
2. [aksi prioritas 2]
3. [aksi prioritas 3]
```

---

## Monitor Otomatis (Bulanan)

```bash
# Cek kebocoran data baru setiap bulan
0 9 1 * * python3 /skills/osint/scripts/monthly-check.py
```

Kalau ada kebocoran baru ditemukan:
```
🚨 **ALERT KEAMANAN — LYRA**

Email kamu baru saja ditemukan di database kebocoran baru!

Breach: [nama]
Tanggal: [tanggal breach]
Data yang bocor: [tipe data]

⚡ Segera ganti password untuk: [daftar akun berisiko]

Mau aku bantu bikin daftar akun yang perlu diganti passwordnya?
```

---

## Tools OSINT yang Digunakan

| Tool | Fungsi | Gratis? |
|------|---------|---------|
| HaveIBeenPwned | Cek email breach | ✅ (butuh API key gratis) |
| PwnedPasswords | Cek password leak | ✅ Gratis penuh |
| NumVerify | Validasi nomor HP | ✅ 250/bulan gratis |
| Google Dorking | Jejak digital | ✅ Gratis |
| Shodan | Cek device exposed | ✅ Basic gratis |

---

## Etika & Batasan

LYRA hanya melakukan OSINT untuk:
- Data pribadi PEMILIK sendiri
- Verifikasi orang/bisnis yang akan diajak kerjasama (dengan izin eksplisit)

LYRA TIDAK akan:
- Investigasi orang lain tanpa izin
- Akses data yang memerlukan cara ilegal
- Share hasil OSINT ke pihak lain
