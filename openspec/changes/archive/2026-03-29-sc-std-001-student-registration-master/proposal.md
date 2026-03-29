## Why

Pendaftaran dan operasional bimbel membutuhkan **rekaman siswa** yang konsisten (identitas, program/bidang, status) sebelum modul pembayaran, jadwal, atau presensi. Tanpa master siswa yang terpusat, data tersebar dan sulit dirujuk oleh modul lain.

## What Changes

- Entitas master **Siswa** dengan nomor induk unik, data identitas inti, relasi ke **Program** dan **Bidang studi** (katalog SC-ACD-001), serta status kepesertaan.
- API REST CRUD terproteksi (otentikasi + RBAC untuk mutasi) mengikuti pola backend yang ada (Zod, Fastify, audit).
- UI admin minimal: daftar + form pendaftaran/penyuntingan siswa (termasuk dropdown berantai program → bidang studi).
- Opsional: asosiasi ke `User` untuk akses portal di iterasi berikutnya — scope MVP fokus ke master data administratif.

## Capabilities

### New Capabilities

- `student-registration-master`: Pendaftaran dan pengelolaan master data siswa (CRUD, nomor induk unik, relasi katalog, status, RBAC/401/403, audit).

### Modified Capabilities

- _(Tidak ada)_ — spesifikasi identity/RBAC di `platform-identity-access-audit` tidak diubah pada level requirement; penempatan peran untuk staf yang mengelola siswa mengikuti kebijakan peran yang sudah ada.

## Impact

- **Backend:** `prisma/schema.prisma`, migrasi, route `/api/...`, helper kode/validasi, tag OpenAPI.
- **Frontend:** halaman/route admin baru mengikuti pola master katalog / tentor-ruang.
- **DB:** tabel baru dengan FK ke `Program` dan `BidangStudi`.
