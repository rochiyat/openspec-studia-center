## Context

Katalog **Level** dan master **Siswa** sudah tersedia. Diperlukan entitas transaksi/penempatan yang menyatakan siswa berada di level tertentu untuk **tahun ajaran** tertentu.

## Goals / Non-Goals

**Goals:**

- Menyimpan **penempatan level** dengan `siswaId`, `levelId`, `tahunAjaran` (string normalisasi manual di API, contoh `2025/2026`), dan status penempatan.
- Memastikan **unik** per pasangan `(siswaId, tahunAjaran)` pada MVP.
- Validasi: `Siswa` dan `Level` harus ada; tidak menghapus Level/Siswa jika masih direferensi (Restrict / cek aplikasi).
- Mutasi oleh `admin-sistem`, `staf-akademik`, `staf-administrasi` (selaras modul siswa).

**Non-Goals:**

- Jadwal kelas, biaya, atau alur naik kelas otomatis.
- Multi-penempatan aktif tanpa tahun ajaran (semua penempatan wajib punya `tahunAjaran`).

## Decisions

1. **Nama model** — `PenempatanLevel` dengan `@@map("penempatan_levels")`.

2. **Status** — Enum `PenempatanLevelStatus`: `AKTIF`, `DIBATALKAN`, `SELESAI`.

3. **Tahun ajaran** — `tahunAjaran` `String` panjang wajar (mis. 20 char); validasi format ringan opsional (regex `^\d{4}/\d{4}$`) di Zod.

4. **Route API** — `/api/penempatan-level` (koleksi REST konsisten dengan bahasa path yang ada).

5. **OpenAPI** — Tag `penempatan-level`.

6. **Audit** — `recordAudit` pada create/update/delete.

## Risks / Trade-offs

- **String tahun ajaran** bisa inkonsisten pengetikan → mitigasi: validasi format + dokumentasi UI placeholder.  
- **Satu baris per siswa+tahun** — jika butuh riwayat ubah level dalam tahun yang sama, iterasi berikutnya (versi/soft-delete).

## Migration Plan

1. Tambah enum + model + migrasi; `migrate deploy` di dev.  
2. Deploy backend + UI.  
3. Rollback: revert deploy + restore DB jika gagal.

## Open Questions

- Apakah penempatan **wajib** untuk setiap siswa aktif? (Default MVP: tidak dipaksakan oleh sistem.)
