## Why

Operasional bimbel membutuhkan master **tentor** (pengajar) dan **ruang** (lokasi belajar) yang konsisten agar jadwal, kelas, dan penugasan dapat direferensikan tanpa data tersebar di spreadsheet. Setelah katalog program (SC-ACD-001) tersedia, langkah natural berikutnya adalah mendefinisikan sumber daya manusia dan fisik sebagai fondasi modul akademik berikutnya.

## What Changes

- Model data dan migrasi Postgres untuk entitas **Tentor** dan **Ruang** (kode unik, atribut inti, relasi yang disepakati di desain).
- Asosiasi **Tentor** ke akun `User` (opsional atau unik per user, sesuai desain) agar selaras dengan RBAC peran **Tentor** yang sudah direncanakan di platform.
- API REST CRUD terproteksi (otentikasi + peran untuk mutasi) mengikuti pola backend yang ada (Zod, Fastify, audit opsional).
- UI admin minimal: daftar + form untuk mengelola tentor dan ruang.
- Tidak ada perubahan kontrak otentikasi global; hanya penambahan route dan skema data.

## Capabilities

### New Capabilities

- `tentor-ruang`: Master data tentor dan ruang (CRUD, kode unik, aturan RBAC/401/403, konsistensi dengan pola katalog & platform identity).

### Modified Capabilities

- _(Tidak ada)_ — perilaku identity/RBAC di `platform-identity-access-audit` tidak diubah pada level requirement; peran **Tentor** sudah tercakup sebagai peran yang dapat ditetapkan ke pengguna.

## Impact

- **Backend:** `prisma/schema.prisma`, migrasi baru, route `/api/...`, validasi kode, seed bila diperlukan, tag OpenAPI.
- **Frontend:** halaman/route admin baru mengikuti pola modul katalog.
- **DB:** tabel baru; tidak mengubah tabel katalog kecuali relasi eksplisit ditambahkan di desain (mis. FK opsional).
