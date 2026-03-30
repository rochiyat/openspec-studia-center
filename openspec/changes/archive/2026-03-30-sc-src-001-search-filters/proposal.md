## Why

Daftar master (siswa, kelas, penempatan level, tentor, ruang, dll.) saat ini memuat **seluruh baris** tanpa pencarian teks atau filter di API; UI hanya menampilkan tabel penuh sehingga sulit dipakai saat data bertambah. Perlu **pencarian ringkas** dan **filter** opsional pada endpoint daftar serta kontrol di halaman master agar pengguna staf dapat menemukan entri dengan cepat.

## What Changes

- Menambahkan **parameter query opsional** pada endpoint `GET` daftar resource master yang disepakati (MVP: minimal **Siswa** dan **Kelas**; perluasan ke resource lain mengikuti pola yang sama).
- **Pencarian teks** (`q` atau nama serupa) yang memfilter berdasarkan field yang relevan (mis. nama, kode/nomor induk untuk siswa; kode/nama kelas untuk kelas).
- **Filter facet** opsional (mis. `programId`, `bidangStudiId`, `status`, `tahunAjaran`, `levelId`) sesuai kolom yang sudah ada di model; kombinasi filter bersifat **AND**.
- **Perilaku backward compatible**: tanpa query parameter, respons tetap setara daftar penuh seperti sekarang (urutan default boleh dipertahankan).
- **Frontend**: field pencarian + dropdown filter pada halaman master terkait; `queryKey` React Query memasukkan parameter agar cache konsisten.
- **Dokumentasi OpenAPI**: query parameters tercatat di schema route yang disentuh.

## Capabilities

### New Capabilities

- `master-list-search-filters`: Kontrak perilaku daftar master dengan **query string opsional** untuk pencarian teks dan filter facet, konsisten antar resource, tanpa mengubah bentuk respons `{ items: [...] }` kecuali ditambahkan metadata opsional di fase berikutnya (MVP tidak wajib).

### Modified Capabilities

- `student-registration-master`: Persyaratan daftar siswa diperluas dengan perilaku filter/pencarian opsional pada `GET` daftar (delta spec).
- `class-roster`: Persyaratan daftar kelas diperluas dengan perilaku filter/pencarian opsional pada `GET` daftar (delta spec).

## Impact

- **Backend**: `backend/src/routes/siswa/index.ts`, `backend/src/routes/kelas/index.ts` (dan helper Prisma `where`/`contains` case-insensitive jika dipilih).
- **Frontend**: `MasterSiswaPage.tsx`, `MasterKelasPage.tsx` (state filter + URL query atau state lokal + refetch).
- **Tes**: tambah kasus ringkas di `app.test.ts` atau uji route terpisah untuk memastikan parameter tidak merusak respons default.
- **Tanpa** dependensi paket baru jika memungkinkan (Prisma `contains` + `mode: 'insensitive'` untuk Postgres).
