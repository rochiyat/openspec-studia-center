## 1. Skema & penyimpanan

- [x] 1.1 Tambahkan enum jenis laporan dan model `LaporanPdf` (metadata: pembuat, kelas, jenis, path/nama file, status, waktu); migrasi Prisma
- [x] 1.2 Konfigurasi `REPORTS_DIR` (env) dan pastikan direktori dapat dibuat saat startup atau on-demand

## 2. Backend

- [x] 2.1 Dependensi PDF + utilitas generate (minimal tabel untuk NILAI_KELAS dan PRESENSI_KELAS)
- [x] 2.2 `POST /api/laporan/generate` (body: jenis, kelasId) → buat PDF, simpan file, simpan rekaman
- [x] 2.3 `GET /api/laporan` (daftar riwayat) dan `GET /api/laporan/:id/download` (stream PDF) dengan validasi path aman
- [x] 2.4 `authenticate` + `requireRoles` selaras staf akademik; `recordAudit`; tag OpenAPI `reports-pdf-history`; tes 401/403

## 3. Frontend

- [x] 3.1 Halaman: pilih jenis laporan + kelas → generate → tautan unduh / refresh daftar riwayat
- [x] 3.2 Tampilkan error API ringkas ke pengguna
