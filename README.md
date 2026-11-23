# ReiikuID — Next.js + Supabase

Starter website katalog Android dengan Next.js App Router, TypeScript, Tailwind CSS v4, Supabase Auth, Postgres, dan Storage.

## Persyaratan
- Node.js 20.9 atau lebih baru
- Akun GitHub dan Vercel
- Proyek Supabase

## 1. Jalankan lokal
```bash
npm install
cp .env.example .env.local
npm run dev
```
Buka http://localhost:3000. Isi `.env.local` dengan URL dan anon/publishable key dari Supabase.

## 2. Siapkan Supabase
1. Buat project di https://supabase.com.
2. Buka Project Settings → API, salin Project URL dan anon/publishable key.
3. Masukkan ke `.env.local`:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
4. Buka SQL Editor, jalankan isi `supabase/schema.sql`.
5. Buka Authentication → Users, buat akun admin.
6. Setelah akun dibuat, buka Authentication → Users → pilih user → Edit app metadata, tambahkan:
```json
{"role":"admin"}
```
   Pastikan menambahkan ke **App Metadata**, bukan User Metadata. Keluar dan masuk kembali di dashboard setelah mengubah role.
7. Jalankan `npm run dev` lagi bila sebelumnya server belum membaca env.

## 3. Deploy ke Vercel
1. Upload seluruh isi folder ini ke repository GitHub.
2. Di https://vercel.com pilih Add New → Project → import repository.
3. Framework akan terdeteksi sebagai Next.js.
4. Tambahkan Environment Variables yang sama dengan `.env.local` (jangan upload `.env.local` ke GitHub).
5. Deploy.
6. Buka Project → Settings → Domains, tambahkan `reiikuid.vercel.app` jika subdomain tersebut tersedia dan ikuti petunjuk Vercel.

## 4. Dashboard
- Buka `/admin`, contoh: `https://reiikuid.vercel.app/admin`.
- Login memakai akun Supabase yang app_metadata-nya memiliki `role: admin`.
- Isi detail aplikasi, ikon, dan file APK yang berhak kamu distribusikan.
- File dapat diunggah maksimal 100 MB sesuai batas bucket contoh di SQL.

## Struktur penting
- `app/page.tsx` — beranda dan katalog
- `app/app/[slug]/page.tsx` — detail aplikasi
- `app/admin/page.tsx` — dashboard admin
- `components/` — komponen katalog dan admin
- `lib/supabase/` — client/server Supabase
- `supabase/schema.sql` — tabel, RLS, dan bucket storage

## Catatan keamanan & batasan
- Ini starter yang perlu dikonfigurasi dengan proyek Supabase milikmu sebelum fitur database/login/upload bekerja.
- Jangan pernah menaruh Supabase service-role key di browser atau variabel `NEXT_PUBLIC_*`.
- File di bucket publik dapat diakses siapa pun yang memiliki URL. Jangan unggah file privat.
- Batasi distribusi pada APK buatan sendiri atau yang memiliki izin distribusi. Jangan mendistribusikan aplikasi bajakan atau modifikasi tanpa izin.
- Contoh ini belum memindai malware, memverifikasi tanda tangan APK, atau menerapkan rate limit unduhan. Sebelum menerima upload publik, tambahkan proses pemeriksaan keamanan dan validasi file di sisi server.
- Untuk produksi, pertimbangkan upload melalui server/action dengan pemeriksaan MIME dan ukuran yang ketat, log audit, backup database, dan kebijakan moderasi.
