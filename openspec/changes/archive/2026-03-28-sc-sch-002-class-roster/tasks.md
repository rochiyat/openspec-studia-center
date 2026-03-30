## 1. Skema database & migrasi

- [x] 1.1 Tambahkan enum status kelas, model `Kelas` (kode unik, nama, tahun ajaran, FK `levelId`, opsional `tentorId`/`ruangId`) dan `AnggotaKelas` (`kelasId`, `siswaId`, `@@unique([kelasId, siswaId])`)
- [x] 1.2 Generate migrasi dan jalankan `prisma migrate` / `deploy` di lingkungan dev

## 2. API backend

- [x] 2.1 Helper/generator kode kelas (prefix **CLS**) jika kode opsional di POST
- [x] 2.2 Route CRUD `/api/kelas` dengan Zod + `authenticate` + `requireRoles` untuk mutasi (`admin-sistem`, `staf-akademik`, `staf-administrasi`)
- [x] 2.3 Route anggota `/api/kelas/:kelasId/anggota` (GET/POST; DELETE anggota per siswa)
- [x] 2.4 `recordAudit` untuk kelas dan mutasi anggota; tangani 400/409

## 3. Dokumentasi API

- [x] 3.1 Tag OpenAPI `kelas` (dan konsisten untuk sub-route anggota)
- [x] 3.2 Tes integrasi ringan 401/403

## 4. Frontend

- [x] 4.1 Halaman daftar kelas + form buat/edit (minimal)
- [x] 4.2 Halaman atau panel roster: tambah/hapus siswa ke kelas terpilih dengan dropdown `/api/siswa`
