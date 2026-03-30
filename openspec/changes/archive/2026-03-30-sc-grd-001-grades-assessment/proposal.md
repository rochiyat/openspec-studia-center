## Why

Operasional bimbel membutuhkan pencatatan **nilai** dan **komponen penilaian** (tugas, kuis, UTS, UAS, dll.) per siswa dalam konteks **kelas** dan **mata pelajaran (bidang studi)** agar rapor ringkas, tindak lanjut pengajar, dan komunikasi dengan orang tua tidak mengandalkan dokumen terpisah. Master siswa, kelas, dan roster sudah ada; penilaian adalah lapisan akademik berikutnya.

## What Changes

- Model data **nilai** (skor numerik) terikat ke **Siswa**, **Kelas**, **BidangStudi**, **tahun ajaran**, dan **komponen penilaian** terenumerasi, dengan unicitas logis per kombinasi (satu nilai per komponen per siswa per kelas per mapel per tahun ajaran).
- Validasi bahwa siswa merupakan **anggota kelas** yang bersangkutan dan **bidang studi** selaras dengan data siswa (atau aturan yang dijelaskan di desain).
- API terotentikasi untuk mengelola dan membaca nilai; mutasi oleh peran staf/admin akademik selaras modul lain; **audit** untuk perubahan penting.
- UI minimal: pilih kelas → daftar siswa anggota → input/ubah nilai per komponen per mapel (bidang studi).

## Capabilities

### New Capabilities

- `grades-assessment`: Pencatatan dan pengelolaan nilai per siswa per kelas per bidang studi dengan komponen penilaian, aturan akses API, dan audit.

### Modified Capabilities

- _(Tidak ada)_ — tidak mengubah requirement spec yang sudah ada; fitur baru memakai entitas **Kelas**, **Siswa**, **AnggotaKelas**, dan **BidangStudi** yang sudah ada.

## Impact

- **Backend:** Prisma (enum komponen, model nilai), migrasi, route REST, Zod, `recordAudit`.
- **Frontend:** halaman alur penilaian (master atau sub-menu), React Query.
- **Database:** tabel baru; tanpa **BREAKING** pada kontrak API lama jika endpoint existing tidak diubah.
