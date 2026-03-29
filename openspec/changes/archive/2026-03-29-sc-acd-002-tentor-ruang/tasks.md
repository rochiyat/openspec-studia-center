## 1. Skema database & migrasi

- [x] 1.1 Tambahkan model Prisma `Tentor` (kode unik, nama, `userId` opsional unik → `User`) dan `Ruang` (kode unik, nama, kapasitas opsional) beserta `@@map` tabel yang konsisten
- [x] 1.2 Generate migrasi dan jalankan `prisma migrate` / `deploy` di lingkungan dev

## 2. API backend

- [x] 2.1 Helper pembentukan/validasi kode tentor & ruang (hindari bentrok dengan prefix katalog; dokumentasikan di kode)
- [x] 2.2 Route CRUD `/api/tentors` dan `/api/ruang` dengan Zod + `authenticate` + `requireRoles` untuk mutasi (selaras penulis katalog: `admin-sistem`, `staf-akademik`)
- [x] 2.3 Validasi FK `userId` ke `User` yang ada; tangani konflik unik
- [x] 2.4 Panggil `recordAudit` pada create/update/delete tentor dan ruang

## 3. Dokumentasi API

- [x] 3.1 Tag OpenAPI/Swagger `tentor-ruang` untuk endpoint baru
- [x] 3.2 Uji manual atau tes integrasi ringan untuk CRUD dan 401/403

## 4. Frontend

- [x] 4.1 Halaman daftar + form untuk Tentor dan Ruang (minimal operasional)
- [x] 4.2 Untuk tentor: pemilih pengguna (opsional) jika UI admin sudah punya pola pemetaan user
