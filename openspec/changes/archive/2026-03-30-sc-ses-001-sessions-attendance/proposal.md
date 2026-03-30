## Why

Bimbel perlu mencatat **pertemuan (sesi)** per kelas dan **kehadiran siswa** agar operasional, laporan, dan tindak lanjut akademik tidak mengandalkan spreadsheet terpisah. Master **Kelas** dan roster sudah ada; langkah natural berikutnya adalah jejak sesi dan presensi per siswa per sesi.

## What Changes

- Model data **sesi pertemuan** terikat ke **Kelas** (jadwal/waktu, status sesi, metadata ringkas seperti topik).
- Model **presensi** per pasangan **sesi × siswa** dengan status kehadiran yang terkontrol (tanpa duplikasi untuk pasangan yang sama).
- API terotentikasi untuk CRUD sesi (sesuai peran yang selaras modul akademik) dan untuk mencatat/membaca presensi; audit untuk mutasi penting.
- UI minimal: daftar sesi per kelas, form sesi, dan panel presensi (daftar anggota kelas dengan status kehadiran).

## Capabilities

### New Capabilities

- `sessions-attendance`: Sesi pertemuan per kelas, status sesi, dan presensi siswa per sesi (termasuk aturan akses API dan audit).

### Modified Capabilities

- _(Tidak ada)_ — perilaku spesifikasi **class-roster** tidak berubah; fitur baru mengandalkan kelas & anggota yang sudah ada.

## Impact

- **Backend:** Prisma schema + migrasi, route Fastify baru, Zod, `recordAudit`, pola role seperti `kelas`/`siswa`.
- **Frontend:** halaman atau alur baru di bawah master (mis. dari konteks kelas), `apiFetch`, React Query.
- **Database:** tabel baru; tidak ada **BREAKING** pada API existing jika kontrak lama tidak diubah.
