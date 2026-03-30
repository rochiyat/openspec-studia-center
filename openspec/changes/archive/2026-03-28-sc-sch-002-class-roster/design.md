## Context

Tersedia **Level**, **Siswa**, **Tentor**, **Ruang**, dan **PenempatanLevel**. Fitur ini menambahkan **Kelas** sebagai grup pembelajaran dan **AnggotaKelas** sebagai roster peserta.

## Goals / Non-Goals

**Goals:**

- **Kelas**: `kode` unik, `nama`, `tahunAjaran` (format `YYYY/YYYY` konsisten dengan penempatan level), FK `levelId` wajib; `tentorId` & `ruangId` opsional; status operasional.
- **AnggotaKelas**: `kelasId` + `siswaId`, `@@unique([kelasId, siswaId])`.
- API REST + RBAC mutasi (`admin-sistem`, `staf-akademik`, `staf-administrasi`).
- Audit pada create/update/delete kelas dan mutasi anggota.

**Non-Goals:**

- Jadwal jam/hari per pertemuan (akan modul jadwal terpisah).
- Validasi otomatis bahwa siswa sudah `PenempatanLevel` yang cocok — opsional iterasi berikutnya; MVP boleh hanya cek FK ada.

## Decisions

1. **Nama model** — `Kelas` (`@@map("kelas")`), `AnggotaKelas` (`@@map("anggota_kelas")`).

2. **Kode kelas** — Unik global; format `CLS-NNN` diimplementasikan dengan helper mirip master lain (prefix **CLS**).

3. **Enum status kelas** — `KelasStatus`: `AKTIF`, `SELESAI`, `DIBATALKAN`.

4. **Route** — `/api/kelas` (CRUD kelas); `/api/kelas/:kelasId/anggota` (GET koleksi, POST tambah siswa, DELETE oleh query atau `/:siswaId`).

5. **OpenAPI** — Tag `class-roster` atau `kelas`; dipilih **`kelas`** agar singkat di Swagger.

6. **Menambah anggota** — Validasi `siswa` ada; opsional cek duplikasi.

## Risks / Trade-offs

- Siswa dari program berbeda bisa masuk kelas yang sama jika tidak divalidasi — diterima di MVP; bisa diperketat dengan aturan bisnis berikutnya.

## Migration Plan

1. Tambah model + migrasi; `migrate deploy` di dev.  
2. Deploy backend + UI.  
3. Rollback: revert + restore DB bila perlu.

## Open Questions

- Apakah satu siswa boleh di **beberapa** kelas aktif dalam tahun ajaran yang sama? (Default MVP: **boleh** — hanya dicegah duplikasi di kelas yang sama.)
