# HanRouter — frontend

Frontend statis buat HanRouter (router API key model AI: DeepSeek / GLM / MiniMax).
Backend (`server.py`) jalan di VM dan diakses lewat ngrok — frontend ini cuma
 manggil API-nya.

## Deploy ke Vercel

1. Import repo ini di Vercel (Framework Preset: **Other** / tanpa build).
2. Deploy. Tidak ada build step — file HTML langsung diserve.
3. Custom domain: tambah `apitokenhan.my.id` di Vercel → Domains,
   lalu arahkan DNS (CNAME) ke alamat yang dikasih Vercel
   (biasanya `cname.vercel-dns.com`).

## Cara kerja

- `index.html` / `chat.html` — halaman chat (wajib API key `dsk-...`).
- `models.html` — daftar model. `balance.html` — cek saldo key.
  `admin.html` — panel admin (login password, kelola key/model/harga).
- Semua `fetch()` dialihkan ke `API_BASE = window.location.origin + '/'`
  (base URL = domain situs sendiri, mis. `apitokenhan.my.id`) +
  header `ngrok-skip-browser-warning`. `vercel.json` mem-proxy
  `/api/*`, `/v1/*`, dan endpoint `/admin/*` ke backend ngrok, jadi
  dari browser kelihatannya semua lewat domain sendiri.
  Lihat snippet di tiap file (`[netlify-static]`).
- API key dibuat di admin, lalu dimasukkan di kolom API KEY halaman chat.
  Pemakaian motong kuota key masing-masing.

## Backend (tidak di repo ini)

`server.py` + `models.json` + `keys.db` (SQLite) jalan di VM.
Ganti `API_BASE` di tiap file HTML kalau URL backend pindah.
