## Context

Backend Studia Center memakai Prisma + Fastify, pola katalog SC-ACD-001 (`/levels`, `/programs`, `/bidang-studi`) dengan `authenticate`, `requireRoles` untuk mutasi, dan `recordAudit` pada operasi penting. Peran RBAC sudah diseed termasuk `tentor`. Belum ada entitas domain untuk profil pengajar atau ruang fisik.

## Goals / Non-Goals

**Goals:**

- Menyimpan master **Tentor** dan **Ruang** dengan kode unik dan field inti yang cukup untuk administrasi dan referensi modul berikutnya (jadwal, kelas).
- Menyediakan API CRUD konsisten dengan katalog (list, detail, create, patch, delete) dan proteksi mutasi oleh `admin-sistem` dan `staf-akademik` (sama seperti penulis katalog).
- Menautkan **Tentor** ke paling banyak satu `User` (FK opsional) agar akun dengan peran Tentor dapat dipetakan ke rekaman master.
- UI admin minimal mengikuti pola halaman katalog.

**Non-Goals:**

- Penjadwalan, availability tentor, atau booking ruang.
- Multi-cabang / zona waktu / alamat panjang untuk ruang (hanya field inti; bisa ditambah iterasi berikutnya).

## Decisions

1. **Nama entitas & tabel**  
   - Prisma: `Tentor`, `Ruang` (singular model, plural tabel `Tentor` → `tentors`, `Ruang` → `ruangs` atau `ruangan` — pilih **`Tentor` / `Ruang`** dengan `@@map` eksplisit: `tentors`, `ruangs` untuk konsistensi bahasa campuran yang sudah dipakai proyek).

2. **Field Tentor**  
   - `code` (String, unique), `name` (String), `userId` (String?, unique) → FK `User` `onDelete: SetNull`.  
   - Tidak menduplikasi email/password di Tentor; identitas login tetap di `User`.  
   - Validasi: jika `userId` diisi, user harus ada; tidak wajib punya peran Tentor di seed (bisa ditegakkan di iterasi berikutnya).

3. **Field Ruang**  
   - `code` (unique), `name`, `capacity` (Int?, nullable = tidak ditetapkan), cukup untuk MVP.

4. **Pembentukan kode**  
   - Reuse pola `catalog-codes.ts` bila cocok (prefix + sequence), atau helper baru `tentor-ruang-codes.ts` dengan aturan serupa (panjang, charset) agar tidak bentrok dengan Level/Program.  
   - **Keputusan:** helper terpisah ringan atau perpanjangan konstanta prefix di file yang sama — hindari collision dengan prefix katalog yang sudah ada (`LVL`, `PRG`, dll.).

5. **Route API**  
   - `/api/tentors` dan `/api/ruang` (bahasa Inggris plural untuk tentor, Indonesian singular resource name untuk ruang agar selaras keinginan domain "ruang"; alternatif `/api/ruangs` — **keputusan:** gunakan **`/api/ruang`** untuk koleksi dan `/:id` agar natural dibaca).

6. **Audit**  
   - Panggil `recordAudit` pada create/update/delete Tentor dan Ruang (selaras tugas opsional katalog).

7. **OpenAPI**  
   - Tag baru `tentor-ruang` atau gabung `catalog` — **keputusan:** tag **`tentor-ruang`** untuk pemisahan dokumentasi.

## Risks / Trade-offs

- **FK Tentor → User** bisa orphan jika user dihapus (Cascade tidak dipilih untuk menjaga history); mitigasi: `onDelete: SetNull` pada tentor.userId.  
- **Kode otomatis vs manual:** seperti katalog, izinkan kode opsional di POST dengan fallback generate — konsistensi UX.  
- **Ruang** tanpa geo/lantai mungkin kurang untuk gedung besar — diterima sebagai trade-off MVP.

## Migration Plan

1. Tambah model + migrasi Prisma di branch terpisah; jalankan `migrate deploy` di dev/staging.  
2. Deploy backend baru; tidak ada migrasi data dari entitas lama (greenfield).  
3. Rollback: revert deploy + `migrate resolve` / restore snapshot DB jika gagal — tidak ada transform data kompleks.

## Open Questions

- Apakah satu `User` dengan peran Tentor **wajib** punya baris `Tentor`? (Default desain: tidak wajib — master data terpisah dari penempatan peran.)  
- Apakah nama tampilan Tentor harus sinkron dengan `User.displayName`? (Default: tidak otomatis; edit manual di master.)
