## 1. Skema database & migrasi

- [x] 1.1 Tambahkan enum status/jenis kelamin jika dipakai, model `Siswa` (nomor induk unik, nama, relasi ke `Program` & `BidangStudi`, field identitas/status) beserta indeks yang diperlukan
- [x] 1.2 Generate migrasi dan jalankan `prisma migrate` / `deploy` di lingkungan dev

## 2. API backend

- [x] 2.1 Helper pembentukan/validasi nomor induk (prefix **STD**, hindari bentrok dengan katalog lain)
- [x] 2.2 Route CRUD `/api/siswa` dengan Zod + `authenticate` + `requireRoles` untuk mutasi (`admin-sistem`, `staf-akademik`, `staf-administrasi`)
- [x] 2.3 Validasi konsistensi `programId` ↔ `bidangStudiId` pada create/update
- [x] 2.4 Panggil `recordAudit` pada create/update/delete siswa

## 3. Dokumentasi API

- [x] 3.1 Tag OpenAPI/Swagger `siswa` untuk endpoint baru
- [x] 3.2 Uji integrasi ringan untuk 401/403 dan alur validasi bidang studi

## 4. Frontend

- [x] 4.1 Halaman daftar + form pendaftaran siswa (minimal operasional)
- [x] 4.2 Dropdown berantai: pilih program lalu bidang studi yang valid
