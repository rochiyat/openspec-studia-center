## Context

Aplikasi sudah memiliki data **Kelas**, **Nilai**, **Presensi**, **Siswa**, JWT, peran staf, dan **AuditLog**. Belum ada modul ekspor PDF maupun tabel riwayat laporan.

## Goals / Non-Goals

**Goals:**

- Enum **jenis laporan** MVP: `NILAI_KELAS`, `PRESENSI_KELAS` (nama pasti disesuaikan di skema).
- Rekaman **Laporan** (metadata): siapa yang meminta, kapan, jenis, `kelasId` wajib untuk MVP ini, nama file / path relatif, status `SIAP` / `GAGAL`, pesan error opsional.
- Endpoint **generate** (sinkron MVP): buat PDF → simpan ke `REPORTS_DIR` (env) → update rekaman.
- Endpoint **GET daftar** riwayat (paginasi sederhana opsional) dan **GET download** by id dengan cek kepemilikan/role.
- Audit: `laporan.generate`, `laporan.download` (atau gabung sesuai kebijakan tim).

**Non-Goals:**

- Antrian pekerjaan berat (Bull/Redis), email otomatis, template desain katalog penuh, multi-tenant storage terpisah.

## Decisions

1. **Library PDF**  
   - **Keputusan:** `pdfkit` (atau `pdf-lib`) di Node — cukup untuk tabel teks.  
   - **Alternatif:** Puppeteer + HTML — ditolak untuk MVP karena berat.

2. **Penyimpanan file**  
   - **Keputusan:** filesystem lokal di bawah `REPORTS_DIR` + nama file unik (`cuid` + `.pdf`).  
   - **Alternatif:** S3-compatible — fase berikutnya.

3. **Autorisasi**  
   - **Keputusan:** Sama seperti mutasi akademik: `admin-sistem`, `staf-akademik`, `staf-administrasi` untuk generate & lihat riwayat & unduh.  
   - **Catatan:** `tentor` tidak diberi akses di MVP (sesuai pola nilai).

4. **Isi PDF**  
   - **Keputusan:** Tabel ringkas: untuk nilai — siswa anggota × komponen; untuk presensi — agregat per sesi atau per siswa (detail di implementasi, minimal tidak kosong jika ada data).

5. **Retensi**  
   - **Keputusan:** Tidak menghapus otomatis di MVP; bisa ditambah cron nanti.

## Risks / Trade-offs

- **[Risks]** Disk penuh → **Mitigasi:** monitoring; batas ukuran file di generator.  
- **[Risks]** Path traversal pada download → **Mitigasi:** resolve path hanya dalam `REPORTS_DIR`, validasi id rekaman.

## Migration Plan

Migrasi Prisma → set `REPORTS_DIR` di `.env` → deploy backend → deploy frontend.

## Open Questions

- Batas maksimal ukuran PDF per request?  
- Apakah satu kelas boleh dibuatkan banyak laporan identik (diizinkan; riwayat menumpuk)?
