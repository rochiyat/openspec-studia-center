## Why

Modul akademik Studia Center membutuhkan akses terkontrol dan jejak audit agar data siswa, jadwal, dan nilai tidak dapat diubah tanpa akun dan peran yang sesuai. Tanpa fondasi identitas, otorisasi, dan audit, requirement keamanan PRD (login wajib, RBAC, audit log) tidak dapat dipenuhi oleh modul lain.

## What Changes

- Otentikasi pengguna (login, sesi atau token refresh sesuai keputusan desain) dan logout.
- Manajemen pengguna serta peran (role) dan hak akses (permission atau mapping setara) untuk Admin, Staf, Akademik, Tentor, Pimpinan — selaras PRD.
- Pencatatan audit untuk peristiwa/perubahan penting yang akan didefinisikan bersama domain (minimal pola extensible).
- **BREAKING**: tidak ada — ini fondasi greenfield; modul lain belum ada.

## Capabilities

### New Capabilities

- `platform-identity-access-audit`: Otentikasi, manajemen user & RBAC, dan audit trail sebagai fondasi untuk seluruh API dan UI Studia Center.

### Modified Capabilities

- (tidak ada — belum ada spec utama di `openspec/specs/`)

## Impact

- Semua endpoint dan layar berikutnya bergantung pada middleware auth dan pemeriksaan peran.
- Kontrak event audit mempengaruhi modul transaksi (master data, jadwal, nilai, dll.) saat mereka menulis ke audit.
- Keputusan teknis (JWT vs session, struktur role) mengikat `design.md` dan implementasi `apps/api` / `apps/web` jika monorepo dipakai.

