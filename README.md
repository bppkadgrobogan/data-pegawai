# Aplikasi Pencarian Data Pegawai
**BPPKAD Kabupaten Grobogan**

Aplikasi web sederhana untuk mencari data pegawai berdasarkan **NIK**, **NIP**, atau **Nama**.

---

## Struktur Folder

```
sipdri-app/
├── public/
│   ├── index.html      ← Halaman utama aplikasi
│   └── pegawai.json    ← Database pegawai (diperbarui dari Excel)
├── vercel.json         ← Konfigurasi Vercel
└── README.md
```

---

## Cara Deploy ke Vercel + GitHub

### 1. Upload ke GitHub
1. Buka [github.com](https://github.com) → buat repository baru (mis. `data-pegawai`)
2. Upload seluruh isi folder ini ke repository tersebut

### 2. Deploy ke Vercel
1. Buka [vercel.com](https://vercel.com) → Login dengan akun GitHub
2. Klik **"Add New Project"** → pilih repository `data-pegawai`
3. Klik **Deploy** — selesai! Vercel otomatis mendeteksi konfigurasi

Aplikasi akan tersedia di URL seperti: `https://data-pegawai.vercel.app`

---

## Cara Update Database Pegawai

Setiap kali ada data baru dari SIPDRI:

### Langkah 1: Konversi Excel ke JSON
Jalankan script Python berikut:

```python
import openpyxl, json

wb = openpyxl.load_workbook('FILE_BARU.xlsx', read_only=True)
ws = wb.active
rows = list(ws.iter_rows(values_only=True))
header = rows[0]

data = []
for row in rows[1:]:
    d = {header[i]: (str(row[i]) if row[i] is not None else '') for i in range(len(header))}
    data.append({
        'nip': d.get('nip_pegawai',''),
        'nama': d.get('nama_pegawai',''),
        'nik': d.get('nik_pegawai',''),
        'npwp': d.get('npwp_pegawai',''),
        'tgl_lahir': d.get('tanggal_lahir_pegawai',''),
        'jabatan': d.get('nama_jabatan',''),
        'eselon': d.get('eselon',''),
        'status_asn': d.get('status_asn',''),
        'golongan': d.get('golongan',''),
        'masa_kerja': d.get('masa_kerja_golongan',''),
        'alamat': d.get('alamat',''),
        'status_nikah': d.get('status_pernikahan',''),
        'bank': d.get('nama_bank',''),
        'rekening': d.get('nomor_rekening_bank_pegawai',''),
        'gaji_pokok': d.get('gaji_pokok',''),
        'tunjangan_keluarga': d.get('tunjangan_keluarga',''),
        'tunjangan_jabatan': d.get('tunjangan_jabatan',''),
        'tunjangan_fungsional': d.get('tunjangan_fungsional',''),
        'tunjangan_fungsional_umum': d.get('tunjangan_fungsional_umum',''),
        'tunjangan_beras': d.get('tunjangan_beras',''),
        'jumlah_gaji': d.get('jumlah_gaji_dan_tunjangan',''),
        'jumlah_potongan': d.get('jumlah_potongan',''),
        'jumlah_transfer': d.get('jumlah_ditransfer',''),
        'skpd': d.get('skpd',''),
    })

with open('public/pegawai.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, ensure_ascii=False, separators=(',',':'))

print(f'Selesai: {len(data)} pegawai diekspor')
```

### Langkah 2: Upload ke GitHub
1. Ganti file `public/pegawai.json` di repository GitHub dengan file baru
2. **Vercel otomatis deploy ulang** dalam ±30 detik setelah file diupload

---

## Keamanan
- Aplikasi ini sebaiknya dilindungi dengan password jika berisi data sensitif gaji
- Pertimbangkan menggunakan Vercel Password Protection (fitur berbayar) atau Basic Auth
- Untuk data non-sensitif (hanya nama & jabatan), dapat dibiarkan publik

---

## Fitur Aplikasi
- ✅ Cari berdasarkan NIK, NIP, atau Nama (real-time)
- ✅ Filter berdasarkan SKPD (70 unit kerja)
- ✅ Filter berdasarkan Status ASN (PNS / PPPK)
- ✅ Detail lengkap: data pribadi, kepegawaian, gaji & tunjangan
- ✅ Pagination 15 data per halaman
- ✅ Highlight kata kunci hasil pencarian
- ✅ Responsive (bisa dibuka di HP)
