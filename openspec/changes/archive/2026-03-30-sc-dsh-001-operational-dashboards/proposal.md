## Why

Pengguna staf dan pimpinan membutuhkan **gambaran cepat** kondisi operasional (jumlah siswa, kelas, penempatan, aktivitas terkini) tanpa membuka setiap menu master. Saat ini tidak ada ringkasan terpusat. Dashboard operasional mengurangi waktu untuk memahami “keadaan hari ini” dan mendukung keputusan administratif ringan.

## What Changes

- Endpoint API **read-only** yang mengembalikan **agregat ringkas** (hitungan / total) dari data yang sudah ada (siswa, kelas, penempatan level, dll.) untuk periode atau snapshot yang ditetapkan di implementasi MVP.
- **Otentikasi wajib** dan **pembatasan peran** selaras pengguna yang boleh melihat data agregat institusi (bukan tentor-only).
- **Halaman frontend** yang menampilkan kartu/ringkasan angka dari API tersebut (tanpa grafik kompleks di MVP).
- **Tanpa** mengubah model data inti atau alur CRUD existing (**BREAKING**: tidak ada).

## Capabilities

### New Capabilities

- `operational-dashboards`: Ringkasan metrik operasional via API teragregasi dan tampilan UI dashboard untuk pengguna berwenang.

### Modified Capabilities

- *(Tidak ada — perilaku domain existing tidak berubah; hanya tambah baca agregat.)*

## Impact

- **Backend:** route baru (mis. di `src/routes/`), `buildApp` + tag Swagger, `requireRoles`, tanpa dependensi baru jika agregasi cukup dengan Prisma.
- **Frontend:** halaman baru + routing + link dari beranda (atau navigasi setara).
- **Audit:** opsional log akses ringkas; MVP boleh tanpa audit per GET jika selaras kebijakan tim (dicatat di design).
