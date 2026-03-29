## Why

Master **Siswa** dan **Level** sudah ada, tetapi belum ada rekaman formal **penempatan siswa pada level akademik** dalam suatu periode (tahun ajaran). Tanpa itu, laporan kelas, naik kelas, dan kebijakan akademik sulit dipetakan secara konsisten.

## What Changes

- Entitas **penempatan level** yang menghubungkan `Siswa` ↔ `Level` dengan **tahun ajaran** dan status penempatan.
- Aturan unik per kombinasi siswa + tahun ajaran (satu baris penempatan per siswa per tahun ajaran pada MVP).
- API CRUD terproteksi (otentikasi + RBAC untuk mutasi) dan audit pada operasi penting.
- UI admin minimal: daftar penempatan + form (pilih siswa, level, tahun ajaran).

## Capabilities

### New Capabilities

- `student-level-enrollment`: Penempatan siswa ke level per tahun ajaran (CRUD, validasi referensi, unik per siswa+tahun, RBAC, audit).

### Modified Capabilities

- _(Tidak ada)_ — spesifikasi `student-registration-master` tidak diubah; penempatan level adalah data tambahan, bukan pengganti pendaftaran awal.

## Impact

- **Backend:** Prisma + migrasi, route baru, validasi FK, OpenAPI.
- **Frontend:** halaman/route baru mengikuti pola master siswa.
- **DB:** tabel baru dengan FK ke `siswas` dan `Level`.
