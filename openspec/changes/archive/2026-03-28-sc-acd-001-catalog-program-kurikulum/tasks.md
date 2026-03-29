## 1. Skema database & migrasi

- [x] 1.1 Tambahkan model Prisma `Level`, `Program`, `BidangStudi` (kode unik, nama, relasi `BidangStudi` → `Program`) beserta indeks yang diperlukan
- [x] 1.2 Generate migrasi dan jalankan `prisma migrate` / `deploy` di lingkungan dev

## 2. API backend

- [x] 2.1 Helper pembentukan/validasi kode (`kd_lvl`, `kd_program`, `kd_bidstud`) sesuai keputusan desain
- [x] 2.2 Route CRUD `/api/levels`, `/api/programs`, `/api/bidang-studi` dengan Zod + preHandler `authenticate` + `requireRoles` untuk mutasi
- [x] 2.3 Cegah hapus program jika masih ada bidang studi (atau gunakan `onDelete: Restrict` di Prisma)
- [x] 2.4 (Opsional) Panggil `recordAudit` pada create/update/delete entitas katalog

## 3. Dokumentasi API

- [x] 3.1 Tag OpenAPI/Swagger untuk grup `catalog` atau `master`
- [x] 3.2 Uji manual atau tes integrasi ringan untuk CRUD dan 401/403

## 4. Frontend (jika scope sprint ini)

- [x] 4.1 Halaman daftar + form untuk Level, Program, Bidang studi (minimal operasional)
- [x] 4.2 Dropdown berantai: pilih program saat membuat/edit bidang studi
