## Context

Aplikasi Studia Center sudah memiliki **Kelas** (`Kelas`), **anggota kelas** (`AnggotaKelas`), otentikasi JWT, peran (`admin-sistem`, `staf-akademik`, `staf-administrasi`, `tentor`, dll.), dan **`recordAudit`**. Belum ada entitas untuk satu kali pertemuan mengajar atau rekam kehadiran per siswa.

## Goals / Non-Goals

**Goals:**

- Satu entitas **Sesi** (pertemuan) per `kelasId` dengan waktu mulai (dan opsional waktu selesai), status siklus hidup sesi, serta field ringkas (mis. judul/topik).
- Satu entitas **Presensi** per `sesiId` + `siswaId` dengan status kehadiran terenumerasi.
- API REST konsisten dengan modul lain + Swagger tag dedikasi.
- UI cukup untuk operasional harian (buat sesi, buka presensi, isi status).

**Non-Goals:**

- Integrasi kalender eksternal (Google Calendar), notifikasi push, atau QR check-in.
- Rekonsiliasi gaji tentor berbasis jam mengajar (bisa dilandasi data sesi di masa depan, di luar scope ini).
- Presensi tanpa sesi (harus ada rekaman sesi terlebih dahulu).

## Decisions

1. **Penempatan sesi di bawah Kelas**  
   - **Keputusan:** `Sesi` memiliki FK wajib `kelasId` → `Kelas`.  
   - **Alasan:** Selaras domain bimbel (pertemuan terjadi dalam konteks kelas).  
   - **Alternatif:** Sesi global dengan referensi banyak kelas — ditolak karena kompleksitas tanpa kebutuhan saat ini.

2. **Presensi hanya untuk siswa yang menjadi anggota kelas**  
   - **Keputusan:** Saat mencatat presensi, server memvalidasi `AnggotaKelas` untuk `(kelasId, siswaId)` yang sama dengan sesi tersebut.  
   - **Alasan:** Mencegah presensi untuk siswa yang tidak roster di kelas itu.  
   - **Alternatif:** Izinkan semua siswa — ditolak karena merusak integritas roster.

3. **Unicitas presensi**  
   - **Keputusan:** Composite primary key `@@id([sesiId, siswaId])` pada tabel presensi (satu baris per siswa per sesi).  
   - **Alasan:** Idempotent, query sederhana, cocok dengan pola `AnggotaKelas`.

4. **Enumerasi status**  
   - **Sesi:** mis. `DIJADWALKAN`, `SELESAI`, `DIBATALKAN` (nama pasti disesuaikan di migrasi).  
   - **Presensi:** mis. `HADIR`, `TIDAK_HADIR`, `IZIN`, `SAKIT`, `TERLAMBAT` — cukup untuk MVP; perluasan enum = migrasi terkontrol.

5. **Peran mutasi**  
   - **Keputusan:** Sama seperti modul kelas/siswa: baca butuh login; tulis untuk `admin-sistem`, `staf-akademik`, `staf-administrasi`. **Tentor** bisa dibatasi hanya baca di MVP atau diberi izin terbatas — **default desain:** mutasi sesi & presensi mengikuti pola kelas (staf); jika produk ingin tentor mengisi presensi, tambah kebijakan peran di iterasi berikut (dicatat di Open Questions).

6. **Urutan API**  
   - **Keputusan:** Prefix `/api/kelas/:kelasId/sesi` (nested) untuk list/create; detail/patch/delete `/api/kelas/:kelasId/sesi/:sesiId`; presensi `/api/kelas/:kelasId/sesi/:sesiId/presensi` (GET koleksi, POST upsert batch atau per siswa — implementasi memilih satu pola yang sederhana, dokumentasikan di implementasi).

## Risks / Trade-offs

- **[Risks] Duplikasi jadwal** (dua sesi overlap waktu untuk kelas yang sama) → **Mitigasi:** tidak memaksa unik di MVP; bisa ditambah constraint lunak atau peringatan UI nanti.
- **[Risks] Volume baris presensi** → **Mitigasi:** indeks pada `sesiId`, `siswaId`; batch upsert untuk UI.
- **[Risks] Peran tentor** → **Mitigasi:** keputusan produk eksplisit (lihat Open Questions).

## Migration Plan

1. Deploy migrasi Prisma ke lingkungan dev/staging.  
2. Deploy backend dengan route baru (backward-compatible).  
3. Deploy frontend.  
4. **Rollback:** revert deploy + migrasi down (skrip migrasi harus reversible) jika gagal.

## Open Questions

- Apakah **tentor** harus bisa mengisi presensi di lapangan pada rilis pertama, atau hanya staf kantor?
- Apakah satu sesi boleh **tanpa waktu selesai** (hanya `waktuMulai`) secara wajib — asumsi desain: `waktuMulai` wajib, `waktuSelesai` opsional.
