# Deploy ke Vercel — Step by Step

## Pre-requisites
- Node.js 20+
- Akun Vercel (vercel.com)
- Supabase project sudah dibuat

---

## 1. Install dependencies
```bash
npm install
```

## 2. Push ke GitHub
```bash
git init
git add .
git commit -m "feat: initial commit"
git remote add origin https://github.com/YOURUSER/YOURREPO.git
git push -u origin main
```

## 3. Import ke Vercel
1. Buka https://vercel.com/new
2. Pilih repo GitHub Anda
3. Framework Preset: **Other**
4. Build Command: `npm run build`
5. Output Directory: `.output/public`
6. Install Command: `npm install`

## 4. Set Environment Variables di Vercel
Pergi ke **Project Settings → Environment Variables**, tambahkan:

| Key | Value |
|-----|-------|
| `SUPABASE_URL` | `https://xxx.supabase.co` |
| `SUPABASE_PUBLISHABLE_KEY` | `eyJ...` (anon key) |
| `SUPABASE_SERVICE_ROLE_KEY` | `eyJ...` (service role key) |
| `VITE_SUPABASE_URL` | sama dengan `SUPABASE_URL` |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | sama dengan `SUPABASE_PUBLISHABLE_KEY` |
| `LOVABLE_API_KEY` | API key dari Lovable project Anda |

> **Penting**: `VITE_*` env vars di-inject ke client bundle saat build. Non-`VITE_*` vars hanya tersedia di server.

## 5. Jalankan Supabase Migration
Di Supabase Dashboard → SQL Editor, jalankan:
```
supabase/migrations/20260515000001_visitor_events.sql
```

## 6. Deploy
Klik **Deploy** — Vercel akan build dan deploy otomatis. Setiap push ke `main` akan trigger redeploy.

---

## Troubleshooting

**Build error: `@lovable.dev/vite-tanstack-config` not found**  
→ Sudah diremove. Pastikan pakai `vite.config.ts` dari versi ini.

**`SUPABASE_URL` undefined at runtime**  
→ Pastikan set **semua** environment variables (termasuk `VITE_` prefix untuk client-side).

**API routes 404**  
→ Periksa `vercel.json` rewrites sudah benar dan file ada di `src/routes/api/`.

**SSR error 500**  
→ Cek Vercel Function Logs di dashboard. Biasanya missing env var.
