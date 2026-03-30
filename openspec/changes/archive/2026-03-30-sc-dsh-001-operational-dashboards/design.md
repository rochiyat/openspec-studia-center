## Context

Aplikasi sudah memiliki master **Siswa**, **Kelas**, **PenempatanLevel**, **Sesi**, **AuditLog**, dan peran JWT. Belum ada endpoint khusus agregat untuk “dashboard”.

## Goals / Non-Goals

**Goals:**

- Satu endpoint **GET** yang mengembalikan JSON ringkas dengan angka-angka operasional (MVP).
- **Peran:** `admin-sistem`, `staf-akademik`, `staf-administrasi`, `pimpinan` (baca dashboard). **Tentor** tidak mendapat akses di MVP (selaras data lintas institusi).
- **UI:** satu halaman dengan beberapa **card**/blok angka; styling konsisten dengan halaman master (sederhana).

**Non-Goals:**

- Grafik time-series, export PDF, caching Redis, atau materialized view.
- Filter multi-dimensi lanjutan atau per-tenant; personalisasi per pengguna.

## Decisions

1. **Path API** — `GET /api/dashboard/operational-summary` (prefiks `/api` tetap).

2. **Isi MVP (agregat)**  
   - Jumlah **siswa** per `SiswaStatus` (minimal: AKTIF, total).  
   - Jumlah **kelas** dengan `KelasStatus.AKTIF`.  
   - Jumlah **penempatan level** dengan status `AKTIF` (opsional jika query murah).  
   - Jumlah **sesi** dengan status `DIJADWALKAN` (atau total sesi — pilih satu, dokumentasikan).  
   - Tanpa parameter query di MVP (snapshot global); parameter `tahunAjaran` bisa fase berikutnya.

3. **Implementasi** — `Promise.all` + `prisma.*.count({ where: ... })`; hindari N+1.

4. **OpenAPI** — tag `operational-dashboard` atau `dashboard`.

5. **Audit** — tidak wajib `recordAudit` per GET di MVP; bisa ditambah jika kebijakan mensyaratkan.

## Risks / Trade-offs

- **Beban DB** pada setiap load halaman → **Mitigasi:** query `count` saja; tidak memuat entri penuh.
- **Angka tidak real-time detik** → cukup untuk operasional harian.

## Migration Plan

Deploy backend lalu frontend; tidak ada migrasi skema.

## Open Questions

- Apakah **tentor** perlu melihat subset metrik (mis. hanya kelasnya) di fase berikutnya?
