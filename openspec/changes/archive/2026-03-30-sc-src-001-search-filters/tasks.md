## 1. Backend — Siswa

- [x] 1.1 Parse query `q`, `programId`, `bidangStudiId`, `status` pada `GET /api/siswa`; bangun `where` Prisma (AND; `q` → OR pada `namaLengkap` / `nomorInduk` dengan mode insensitive); validasi ID → 400 jika tidak ada
- [x] 1.2 Perbarui schema OpenAPI (Fastify) untuk query string `GET /api/siswa`

## 2. Backend — Kelas

- [x] 2.1 Parse query `q`, `tahunAjaran`, `levelId`, `status` pada `GET /api/kelas`; bangun `where` Prisma; validasi ID → 400 jika tidak ada
- [x] 2.2 Perbarui schema OpenAPI untuk query string `GET /api/kelas`

## 3. Frontend

- [x] 3.1 `MasterSiswaPage`: input `q` + filter program, bidang studi, status; sertakan di `queryKey` dan URL fetch ke `/api/siswa?...`
- [x] 3.2 `MasterKelasPage`: input `q` + filter tahun ajaran, level, status; sertakan di `queryKey` dan fetch ke `/api/kelas?...`

## 4. Verifikasi

- [x] 4.1 Tes ringkas: `GET /api/siswa` dan `GET /api/kelas` tanpa query tetap 200; dengan `q` kosong setara; atau smoke tambahan sesuai pola `app.test.ts`
