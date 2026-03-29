## Context

Studia Center (PRD) mendefinisikan entitas **Level**, **Program**, **BidangStudi** dengan kode bisnis. Stack backend: Fastify, Prisma, PostgreSQL. Auth/RBAC sudah diimplementasikan (`ROLE_CODES`, middleware JWT).

## Goals / Non-Goals

**Goals:**

- Tabel `Level`, `Program`, `BidangStudi` di PostgreSQL dengan integritas referensial: setiap bidang studi punya `programId` wajib.
- Kode unik per entitas (`code` atau kolom `kd_*` setara) untuk dipakai transaksi downstream.
- REST API dengan validasi Zod; route dilindungi `authenticate` + peran yang diizinkan mengelola master (minimal **Admin Sistem** dan **Staf Akademik** — matriks disepakati di implementasi).
- Pembentukan kode: **urutan DB** atau **prefix + sequence** (mis. `LVL-001`, `PRG-001`, `BST-001`) dengan dokumentasi satu sumber di helper.

**Non-Goals:**

- **Tentor** dan **Ruang** (change terpisah SC-ACD-002).
- Soft delete / riwayat perubahan katalog penuh (bisa fase berikutnya).
- Impor massal CSV pada MVP.

## Decisions

| Keputusan | Pilihan | Alasan |
|-----------|---------|--------|
| Primary key | `cuid()` internal + `code` unique untuk kunci bisnis | Kompatibel dengan join di modul lain; kode tetap stabil untuk UI/laporan |
| Relasi bidang studi | `BidangStudi.programId` → `Program.id` | Sesuai PRD `kd_program` pada bidang studi |
| Siapa boleh CRUD | `admin-sistem`, `staf-akademik` (preHandler `requireRoles`) | Selaras PRD; dapat diperketat per endpoint |
| Audit | `recordAudit` pada create/update/delete entitas katalog | Konsisten dengan platform audit |

## Risks / Trade-offs

- **Duplikasi nama** dengan kode berbeda — diterima; uniqueness pada `code`, bukan nama.
- **Penghapusan program** dengan bidang studi aktif — gunakan `onDelete: Restrict` atau cek manual sebelum delete.

## Migration Plan

- Tambah migrasi Prisma baru; `migrate deploy` ke lingkungan; tidak ada data legacy untuk backfill.

## Open Questions

- Format final kode (panjang, prefix) jika lembaga ingin menyamakan dengan kode kertas lama.
- Apakah **Staf Administrasi** ikut mengubah katalog atau hanya akademik (keputusan produk).
