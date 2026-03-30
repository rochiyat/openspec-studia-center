## 1. Backend

- [x] 1.1 Tambah `GET /api/dashboard/operational-summary` — agregat: siswa aktif (count), kelas aktif, penempatan level aktif, sesi dijadwalkan (atau total sesi sesuai design); `authenticate` + `requireRoles` untuk admin/staf/pimpinan (bukan tentor)
- [x] 1.2 Daftarkan route di `app.ts`, tag OpenAPI `operational-dashboard` (atau nama setara)
- [x] 1.3 Tes Vitest: GET tanpa token → 401; token tentor → 403

## 2. Frontend

- [x] 2.1 Halaman `OperationalDashboardPage` (atau nama setara) — fetch ringkasan, tampilkan kartu angka + pesan error ringkas
- [x] 2.2 Route `/dashboard` (atau `/operational`) dengan `RequireAuth`, link dari `HomePage` untuk peran yang sesuai

## 3. Verifikasi manual

- [x] 3.1 Smoke: pengguna staf dapat membuka halaman dan melihat angka (lingkungan dengan DB)
