## Context

API master (`GET /api/siswa`, `GET /api/kelas`) mengembalikan seluruh baris; Postgres + Prisma dipakai di backend; frontend memakai React Query dengan `queryKey` per resource. Tidak ada pagination server di MVP ini.

## Goals / Non-Goals

**Goals:**

- Parameter query **opsional** yang stabil dan terdokumentasi: `q` untuk substring teks (case-insensitive), plus filter ID/status sesuai domain.
- Implementasi **Prisma** `where` dengan `AND` dari filter yang diset; `q` memakai `OR` internal pada field teks yang disepakati.
- UI **MasterSiswaPage** dan **MasterKelasPage** dengan input pencarian + select filter; refetch saat parameter berubah (debounce opsional untuk `q`, minimal 300 ms jika perlu).

**Non-Goals:**

- Pagination cursor, sort multi-kolom, full-text search GIN, atau indeks DB baru (bisa fase berikut).
- Filter pada daftar katalog (`/api/programs`, dll.) di luar scope MVP kecuali diperlukan sebagai sumber dropdown yang sudah ada.

## Decisions

1. **Nama parameter**  
   - **`q`** — string bebas untuk pencarian ringkas (trim; kosong = abaikan).  
   - **Siswa:** `programId`, `bidangStudiId`, `status` (enum `SiswaStatus`).  
   - **Kelas:** `tahunAjaran` (exact string `YYYY/YYYY`), `levelId`, `status` (enum `KelasStatus`).

2. **Semantik pencarian `q`**  
   - **Siswa:** `namaLengkap` atau `nomorInduk` mengandung `q` (mode `insensitive`).  
   - **Kelas:** `nama` atau `kode` mengandung `q` (mode `insensitive`).

3. **Validasi**  
   - `programId` / `bidangStudiId` / `levelId` — jika bukan cuid valid atau tidak ada di DB, kembalikan **400** dengan pesan jelas (hindari silent empty). Alternatif: abaikan unknown ID — dipilih **400** agar UI salah filter ketahuan.

4. **Backward compatibility**  
   - Tanpa query → `findMany` setara perilaku sekarang (include/orderBy sama).

5. **OpenAPI**  
   - Tambahkan `querystring` di schema route Fastify untuk parameter yang didukung.

## Risks / Trade-offs

- **[Risks]** Query tanpa limit pada tabel besar → lambat. **Mitigasi:** MVP asumsi volume bimbel moderat; catat di Open Questions batas `take` opsional.  
- **[Risks]** Filter kombinasi menghasilkan kosong — perilaku normal; dokumentasikan di UI.

## Migration Plan

Deploy backend dulu (parameter opsional), lalu frontend; tidak perlu migrasi DB.

## Open Questions

- Apakah perlu **`limit` maksimal** (mis. 500) pada `GET` daftar pada rilis yang sama?  
- Apakah **`status` default** di UI harus “semua” tanpa mengirim parameter?
