# KONTENADAPT v1.2 - Setup Guide
## Mesin Kurasi Konten Bea Cukai / Beacukaipapin

## File aplikasi

1. `KONTENADAPT_Frontend.html` - frontend web berbasis HTML + Tailwind.
2. `KONTENADAPT_Backend.gs` - backend Google Apps Script.
3. `KONTENADAPT_Sheets_Structure.txt` - struktur Google Sheets terbaru.
4. `Setup_Guide.md` - panduan setup ini.

## Fitur v1.2

- **Auto-Setup Backend**: Sheet dan header otomatis terbentuk saat request pertama masuk. Tidak perlu manual create sheet.
- **Backend-Managed Config**: API Key OpenAI dan Sheet ID dikelola di backend. Frontend hanya perlu GAS URL.
- **Talent Pool 9 Karakter**: Pre-populated dengan karakter tim Bea Cukai Pangkalpinang.
- Dashboard ringkasan konten aktif, review, produksi, views, ide terbaru, dan konten published.
- Input referensi viral dari TikTok/Instagram/YouTube/manual.
- Upload screenshot/video sebagai referensi lokal; nama file disimpan di caption referensi.
- AI Format Analyzer: format, hook, scene count, emosi, durasi, audio, visual style.
- Bea Cukai Transformer: caption, hook, scene, CTA, audio, hashtag, tone, target audiens.
- Scene re-mapper melalui output `scene_breakdown`.
- Risk filter: Aman / Perlu review / Tidak direkomendasikan.
- Approval workflow: Draft, Review, Revisi, Approved, Produksi, Published.
- Assign PIC/talent dan deadline produksi.
- Detail konten, update status produksi, dan input analytics.
- Database Google Sheets: KONTEN_MASTER, USERS, ANALYTICS, TALENT_PROFILE, KNOWLEDGE_BASE, RISK_LOG, SETTINGS.

## Setup Google Sheets & Backend (WAJIB)

1. Buat Google Spreadsheet baru, misalnya `KontenAdapt Database`.
2. Ambil **Spreadsheet ID** dari URL.
3. Buka Apps Script dari spreadsheet tersebut (Ekstensi > Apps Script).
4. Paste isi `KONTENADAPT_Backend.gs` ke editor.
5. **Ganti** `YOUR_GOOGLE_SHEET_ID_HERE` di bagian `CONFIG.SHEET_ID` dengan Spreadsheet ID Anda.
6. (Opsional) Isi `OPENAI_API_KEY` langsung di sheet SETTINGS setelah deploy, atau hardcode di `CONFIG.OPENAI_API_KEY`.
7. Deploy backend sebagai Web App (lihat bawah).

> **Catatan**: Fungsi `setup()` masih tersedia untuk dijalankan manual, tapi tidak wajib karena `ensureInitialized()` akan otomatis membuat sheet + header + data awal saat endpoint pertama kali dipanggil.

## Setup OpenAI API Key

Pilih salah satu cara:

**Cara A - Sheet SETTINGS (Recommended untuk tim)**
1. Deploy backend.
2. Buka frontend, masukkan GAS URL di Settings, klik Test.
3. Sheet SETTINGS akan auto-terbentuk.
4. Buka Google Sheet > sheet SETTINGS > isi nilai OPENAI_API_KEY.

**Cara B - Hardcode di Backend**
1. Di `KONTENADAPT_Backend.gs`, ubah baris:
   ```
   OPENAI_API_KEY: null,
   ```
   menjadi:
   ```
   OPENAI_API_KEY: 'sk-xxxxxxxxxxxx',
   ```
2. Redeploy backend.

## Deploy backend Google Apps Script

1. Klik **Deploy** -> **New deployment**.
2. Pilih **Web app**.
3. Execute as: **Me**.
4. Access: **Anyone** (untuk testing internal) atau **Anyone with Google account**.
5. Copy **Web App URL**.

## Setup Frontend

### Untuk testing lokal:
1. Buka `KONTENADAPT_Frontend.html` di browser.
2. Masuk ke tab **Settings**.
3. Paste **Web App URL** ke field GAS URL.
4. Klik **Test** untuk verifikasi koneksi.
5. Jika "Backend Connected" muncul, aplikasi siap digunakan.

### Untuk deploy ke Netlify / GitHub Pages / Vercel:
1. Rename `KONTENADAPT_Frontend.html` menjadi `index.html`.
2. (Opsional) Hardcode GAS_URL langsung di file agar tim tidak perlu input manual:
   ```javascript
   let CONFIG = {
       GAS_URL: 'https://script.google.com/macros/s/XXXX/exec' // ganti dengan URL Anda
   };
   ```
3. Upload ke platform hosting statis (Netlify, GitHub Pages, Vercel, Surge, dll).
4. Bagikan URL ke tim. Mereka tinggal buka dan langsung pakai, tidak perlu setup apa pun.

## Talent Pool (9 Karakter)

Sheet TALENT_PROFILE sudah terisi otomatis dengan:

1. **Agung Hermawan** - Koordinator / Bos (Leadership, Authoritative)
2. **Imadelia Tasya Eartam** - Petugas Frontdesk (Galau tapi Cekatan)
3. **Alsyanda Kaltsum Maghfira** - Petugas Frontdesk (Tegas, Professional)
4. **Imam Berlian** - Petugas / Pengguna Jasa (Natural, NPC-style)
5. **M. Arif Anugrah** - Petugas (Tegas Mengayomi, Edukatif)
6. **Ficri Ramadiansyah** - Petugas / Talent (Berwibawa, Kocak, Hobi Nyanyi)
7. **Harso Haryadi** - Pengguna Jasa (Bapak-Bapak Gagap Ekspresif)
8. **Okwan Wamancha** - Pengguna Jasa (Random, Spontan)
9. **Tri Purna Putra** - Multi Talent (Fleksibel, Bisa Apa Saja)

## Test cepat

1. Buka frontend.
2. Verifikasi koneksi backend (dot hijau di navbar).
3. Masuk Dashboard atau Input Ide.
4. Pilih platform, paste caption viral, pilih kategori.
5. Klik Generate Brief Konten.
6. Review hasil AI, pilih Approve & Assign atau Revisi.
7. Buka Pipeline, klik kartu konten untuk update status dan analytics.

## Troubleshooting

- `GAS URL belum diatur`: isi tab Settings dengan Web App URL.
- `Backend belum terkonfigurasi`: cek apakah `CONFIG.SHEET_ID` sudah diganti di backend.
- `OpenAI API Key not configured`: simpan API key di sheet SETTINGS atau hardcode di CONFIG backend.
- `Sheet not found`: biasanya terjadi jika SHEET_ID salah. Pastikan ID dari URL spreadsheet benar.
- CORS/fetch error: pastikan frontend memakai versi terbaru, backend sudah redeploy, dan Web App URL benar.
- AI tidak merespons: cek API key valid dan masih ada quota.
