## Why

Stakeholder bimbel membutuhkan **laporan ringkas dalam PDF** (nilai, presensi, atau ringkasan per kelas) untuk arsip dan komunikasi, serta **jejak siapa membuat laporan kapan** agar akuntabilitas dan audit operasional tidak bergantung pada file manual. Data akademik sudah ada di sistem; penyediaan ekspor PDF dan histori adalah langkah alami.

## What Changes

- Definisi **jenis laporan** (minimal: ringkasan nilai per kelas, ringkasan presensi per kelas — dapat diperluas) dan **generasi file PDF** di sisi server.
- Penyimpanan file dengan referensi aman (path/identifier di DB, bukan menyimpan blob besar di tabel utama jika tidak perlu).
- **Riwayat laporan**: metadata pembuat, waktu, jenis, parameter ringkas (mis. `kelasId`), status; daftar untuk staf yang berwenang.
- API terotentikasi untuk meminta generasi, mengunduh, dan melihat daftar riwayat; **audit** untuk aksi sensitif sesuai pola platform.
- UI minimal: pemilihan jenis & parameter, tombol unduh, daftar riwayat terbaru.

## Capabilities

### New Capabilities

- `reports-pdf-history`: Generasi laporan PDF, penyimpanan referensi file, riwayat laporan, akses terkontrol unduh/list, dan audit.

### Modified Capabilities

- _(Tidak ada)_ — perilaku domain nilai/presensi/kelas tidak diubah di level requirement; laporan membaca data existing.

## Impact

- **Backend:** dependensi PDF (mis. `pdfkit` atau setara), direktori/penyimpanan file (env), model DB baru, route API, migrasi Prisma.
- **Frontend:** halaman atau panel Laporan.
- **Infrastruktur:** ruang disk atau object storage untuk file; tanpa **BREAKING** pada API existing jika hanya menambah endpoint.
