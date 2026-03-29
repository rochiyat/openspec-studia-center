## 1. Skema database & migrasi

- [x] 1.1 Tambahkan enum status penempatan, model `PenempatanLevel` (`siswaId`, `levelId`, `tahunAjaran`, status) dengan `@@unique([siswaId, tahunAjaran])` dan FK ke `Siswa` / `Level`
- [x] 1.2 Generate migrasi dan jalankan `prisma migrate` / `deploy` di lingkungan dev

## 2. API backend

- [x] 2.1 Route CRUD `/api/penempatan-level` dengan Zod (validasi format tahun ajaran, FK) + `authenticate` + `requireRoles` untuk mutasi (`admin-sistem`, `staf-akademik`, `staf-administrasi`)
- [x] 2.2 Tangani konflik unik (409) dan referensi tidak ada (400)
- [x] 2.3 Panggil `recordAudit` pada create/update/delete

## 3. Dokumentasi API

- [x] 3.1 Tag OpenAPI `penempatan-level`
- [x] 3.2 Tes integrasi ringan 401/403

## 4. Frontend

- [x] 4.1 Halaman daftar + form penempatan (siswa, level, tahun ajaran, status)
- [x] 4.2 Dropdown data dari `/api/siswa` dan `/api/levels` (minimal)
