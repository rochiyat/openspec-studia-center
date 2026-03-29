## Context

Studia Center sudah memiliki katalog **Program** / **Bidang studi**, **Tentor**, **Ruang**, dan platform **User** + RBAC. Belum ada entitas untuk peserta didik sebagai master data yang dapat direferensikan modul akademik dan administrasi.

## Goals / Non-Goals

**Goals:**

- Menyimpan **Siswa** dengan nomor induk unik, identitas inti, relasi ke **Program** dan **Bidang studi** yang konsisten (bidang wajib berada di bawah program yang dipilih).
- Menyediakan API CRUD dengan pola yang sama seperti modul master lain (list/detail, create, patch, delete, audit).
- Membatasi mutasi kepada peran operasional yang relevan (admin sistem, staf akademik, staf administrasi).
- UI admin minimal untuk pendaftaran dan daftar siswa.

**Non-Goals:**

- Portal siswa, login siswa, atau tautan ke `User` (bisa ditambah iterasi berikutnya).
- Pembayaran, jadwal, presensi, atau dokumen upload.

## Decisions

1. **Nama model & tabel** — Prisma: `Siswa`, `@@map("siswas")` (konsisten dengan bahasa domain Indonesia di model lain).

2. **Nomor induk** — Field `nomorInduk` (String, unique); format & sequence mengikuti pola `XXX-NNN` dengan prefix **`STD`** (file helper terpisah atau perluasan konvensi kode, tidak memakai prefix katalog LVL/PRG/BST).

3. **Relasi katalog** — `programId` dan `bidangStudiId` wajib; validasi server: `BidangStudi.programId === programId` pada create/update.

4. **Status & identitas** — Enum `SiswaStatus` (minimal: aktif, nonaktif, lulus, keluar); field opsional: `tanggalLahir`, `jenisKelamin`, kontak; detail alamat dapat ditambah iterasi berikutnya.

5. **RBAC mutasi** — `requireRoles(ADMIN_SISTEM, STAF_AKADEMIK, STAF_ADMINISTRASI)`; pembacaan memerlukan otentikasi (selaras modul master lain).

6. **Route API** — Prefix `/api/siswa` (koleksi) dan `/:id` untuk detail.

7. **OpenAPI** — Tag `student-registration` atau `siswa`; dipilih **`siswa`** agar singkat di Swagger.

## Risks / Trade-offs

- **Duplikasi email** antar siswa vs user — email pada siswa opsional; tidak disamakan dengan `User` di MVP.  
- **Ubah program/bidang** — bisa mempengaruhi laporan historis; diterima untuk MVP (tanpa riwayat perpindahan).

## Migration Plan

1. Tambah enum + model + migrasi Prisma; `migrate deploy` di dev/staging.  
2. Deploy backend + UI; tidak ada migrasi data dari sistem lama.  
3. Rollback: revert deploy + restore snapshot DB bila gagal.

## Open Questions

- Apakah **NIS** harus immutable setelah dibuat? (Default: boleh diubah oleh peran yang berwenang dengan audit.)  
- Apakah staf administrasi saja yang boleh create — atau semua tiga peran? (Default: ketiga peran untuk mutasi.)
