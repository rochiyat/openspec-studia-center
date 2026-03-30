## Context

Studia Center memiliki **Siswa** (satu `bidangStudiId`), **Kelas** (`tahunAjaran`, `levelId`), **AnggotaKelas**, otentikasi JWT, peran staf, dan **audit**. Belum ada entitas untuk menyimpan skor penilaian.

## Goals / Non-Goals

**Goals:**

- Satu rekaman **nilai** per kombinasi: `siswaId` + `kelasId` + `bidangStudiId` + `tahunAjaran` + `komponen` (enum), dengan skor numerik (0–100, presisi cukup untuk rapor sederhana).
- Validasi **siswa anggota kelas** untuk `kelasId` yang dipakai.
- Validasi **`bidangStudiId` sama dengan** `siswa.bidangStudiId` pada MVP (satu mapel utama per siswa sesuai skema saat ini).
- API REST + Swagger tag; audit pada create/update/delete nilai.

**Non-Goals:**

- Rumus bobot otomatis ke nilai akhir, rapor PDF, kurikulum nasional penuh, atau bank soal.
- Penilaian per **sesi** (bisa ditautkan nanti via metadata opsional — di luar scope MVP).

## Decisions

1. **Nama model & tabel**  
   - **Keputusan:** Model Prisma `Nilai` (map ke tabel `nilai` jika perlu escape; di PostgreSQL `nilai` bisa reserved — gunakan **`nilai_siswa`** atau **`penilaian_nilai`**).  
   - **Disarankan:** `@@map("nilai_siswa")` untuk hindari kata reserved.

2. **Unicitas**  
   - **Keputusan:** `@@unique([siswaId, kelasId, bidangStudiId, tahunAjaran, komponen])` — satu baris per komponen per konteks.

3. **Komponen**  
   - **Keputusan:** Enum `KomponenNilai`: `TUGAS`, `KUIS`, `UTS`, `UAS`, `LAINNYA` (dapat disesuaikan saat migrasi).

4. **Tipe skor**  
   - **Keputusan:** `Decimal(5,2)` atau `Int` 0–100 — **Int** untuk kesederhanaan validasi di Zod `0..100`.

5. **Peran**  
   - **Keputusan:** Mutasi mengikuti **kelas/siswa**: `admin-sistem`, `staf-akademik`, `staf-administrasi`; baca butuh login.

6. **API shape**  
   - **Keputusan:** Nested `/api/kelas/:kelasId/nilai` (GET daftar untuk kelas + filter opsional siswa), POST upsert satu atau batch kecil; alternatif `GET/PATCH /api/kelas/:kelasId/siswa/:siswaId/nilai` — pilih satu pola konsisten di implementasi (dokumentasikan di tugas).

## Risks / Trade-offs

- **[Risks]** Siswa pindah kelas di tengah tahun → nilai terikat `kelasId` historis — **Mitigasi:** nilai tetap pada kelas tempat dicatat; laporan gabung per periode jika perlu iterasi berikut.
- **[Risks]** Satu mapel per siswa di skema sekarang — **Mitigasi:** jika nanti multi-mapel, pecah requirement baru.

## Migration Plan

Migrasi Prisma → deploy backend → deploy frontend; rollback dengan revert migrasi.

## Open Questions

- Apakah **tentor** boleh mengisi nilai di MVP atau hanya staf kantor?
