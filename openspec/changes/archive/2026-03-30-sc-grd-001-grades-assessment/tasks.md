## 1. Skema database & migrasi

- [x] 1.1 Tambahkan enum `KomponenNilai` dan model `NilaiSiswa` (atau nama setara) dengan FK `siswaId`, `kelasId`, `bidangStudiId`, `tahunAjaran`, `komponen`, skor integer 0–100, `@@unique` pada kombinasi tersebut, relasi ke `Siswa`, `Kelas`, `BidangStudi`
- [x] 1.2 Indeks untuk query per `kelasId` dan `siswaId`
- [x] 1.3 Generate migrasi Prisma dan jalankan `migrate deploy` / `generate` di dev

## 2. API backend

- [x] 2.1 Route `GET /api/kelas/:kelasId/nilai` (daftar nilai untuk kelas) dan `POST` atau `PUT` untuk upsert satu/batch nilai dengan Zod + `authenticate` + `requireRoles` untuk mutasi
- [x] 2.2 Route `DELETE` atau semantik hapus nilai per id jika disediakan; validasi anggota kelas + `bidangStudiId` selaras `siswa`; `recordAudit` pada mutasi
- [x] 2.3 Tag OpenAPI `grades-assessment` + tes ringan 401/403

## 3. Frontend

- [x] 3.1 Halaman alur: pilih kelas → tabel siswa anggota → input/edit nilai per komponen (per baris atau form ringkas)
- [x] 3.2 Penanganan error validasi dari API (pesan ringkas ke pengguna)
