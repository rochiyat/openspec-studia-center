## Why

Setelah penempatan siswa pada **level** per tahun ajaran, operasional bimbel perlu **kelas** (rombongan belajar) dan **daftar peserta (roster)** agar pengajar, ruang, dan administrasi dapat merujuk peserta secara konsisten—bukan hanya master siswa terpisah.

## What Changes

- Entitas **Kelas** terikat **Level** dan **tahun ajaran**, dengan atribut inti (kode/nama) serta opsional **tentor** dan **ruang**.
- Entitas **anggota kelas** (many-to-many `Siswa` ↔ `Kelas`) dengan unik per pasangan kelas–siswa.
- API untuk mengelola kelas dan menambah/menghapus anggota roster, dengan otentikasi + RBAC (selaras modul akademik) dan audit.
- UI admin minimal: daftar kelas, form kelas, dan pengelolaan anggota per kelas.

## Capabilities

### New Capabilities

- `class-roster`: Kelas akademik per level & tahun ajaran, roster siswa (anggota), CRUD + RBAC + audit.

### Modified Capabilities

- _(Tidak ada)_ — tidak mengubah requirement spesifikasi master yang sudah ada; kelas merujuk entitas yang sudah ada lewat FK.

## Impact

- **Backend:** Prisma, migrasi, route `/api/kelas` dan sub-resource anggota, OpenAPI.
- **Frontend:** halaman baru mengikuti pola master/siswa.
- **DB:** tabel `kelas` dan `anggota_kelas` (nama disepakati di desain).
