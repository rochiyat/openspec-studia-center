## Why

Pendaftaran siswa, penjadwalan, dan pelaporan akademik membutuhkan referensi **level**, **program kursus**, dan **bidang studi** yang konsisten. Tanpa master data ini di sistem, data transaksi tidak bisa dirujuk dengan stabil. Change ini menyediakan fondasi katalog setelah fondasi identitas & akses (SC-PLT-001) sudah ada.

## What Changes

- Model dan API **CRUD** untuk **Level** (kode + nama).
- Model dan API **CRUD** untuk **Program** (kode + nama).
- Model dan API **CRUD** untuk **Bidang studi** (kode + nama) dengan **wajib** terhubung ke satu **Program**.
- Kebijakan **pembentukan kode** otomatis atau teratur untuk `kd_lvl`, `kd_program`, `kd_bidstud` (detail di desain).
- **BREAKING**: tidak ada pada kontrak publik yang sudah ada; ini penambahan skema DB dan endpoint baru.

## Capabilities

### New Capabilities

- `catalog-program-kurikulum`: Master **Level**, **Program**, dan **Bidang studi** dengan relasi program–bidang studi, kode unik, dan API terlindungi otentikasi/RBAC.

### Modified Capabilities

- *(Tidak ada perubahan requirement pada spec `platform-identity-access-audit`; modul ini hanya mengonsumsi auth yang sudah ada.)*

## Impact

- **Backend:** migrasi Prisma baru, modul route `/api/...`, validasi Zod, pemanggilan `recordAudit` untuk mutasi penting (opsional/minimal di MVP katalog).
- **Frontend:** halaman atau layar master (daftar + form) untuk ketiga entitas; bergantung prioritas UI pada sprint yang sama atau berikutnya.
- **Dependensi:** membutuhkan **JWT + RBAC** dari implementasi SC-PLT-001.
