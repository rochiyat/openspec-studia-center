## 1. Skema database & migrasi

- [x] 1.1 Tambahkan enum status sesi dan status presensi; model `Sesi` (FK `kelasId`, waktu mulai/selesai opsional, judul/topik opsional, status) dan `PresensiSesi` (composite id `sesiId` + `siswaId`, status presensi) dengan relasi ke `Kelas` dan `Siswa`
- [x] 1.2 Indeks yang relevan (`kelasId`, `siswaId`, waktu); validasi referensi anggota kelas di layer aplikasi
- [x] 1.3 Generate migrasi Prisma dan jalankan `migrate deploy` / `generate` di dev

## 2. API backend

- [x] 2.1 Route nested `/api/kelas/:kelasId/sesi` (GET, POST) dan `/api/kelas/:kelasId/sesi/:sesiId` (GET, PATCH, DELETE) dengan Zod + `authenticate` + `requireRoles` untuk mutasi
- [x] 2.2 Route presensi `/api/kelas/:kelasId/sesi/:sesiId/presensi` (GET; POST atau PATCH untuk upsert per siswa / batch sesuai desain implementasi)
- [x] 2.3 Validasi siswa adalah anggota kelas; tangani 400/404/409; `recordAudit` untuk mutasi sesi dan presensi
- [x] 2.4 Tag OpenAPI (mis. `sessions-attendance` atau `sesi`) + tes ringan 401/403 selaras modul lain

## 3. Frontend

- [x] 3.1 Halaman atau alur: pilih kelas → daftar sesi → buat/edit sesi
- [x] 3.2 Panel presensi untuk sesi terpilih: daftar anggota kelas dengan set status kehadiran dan simpan
