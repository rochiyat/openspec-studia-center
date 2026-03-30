## ADDED Requirements

### Requirement: Daftar master dapat difilter dengan parameter query opsional

Untuk endpoint `GET` daftar resource master yang ditetapkan per rilis, sistem SHALL menerima **parameter query opsional** untuk **pencarian teks** (`q`) dan **filter facet** (ID atau enum sesuai domain). Filter SHALL dikombinasikan dengan logika **AND**. Jika tidak ada parameter filter, respons SHALL setara dengan daftar lengkap sesuai urutan default implementasi.

#### Scenario: Daftar tanpa filter

- **WHEN** klien terotentikasi meminta daftar tanpa menyertakan parameter pencarian atau filter
- **THEN** sistem mengembalikan kumpulan entri sesuai perilaku daftar penuh yang sudah ada

#### Scenario: Daftar dengan pencarian teks

- **WHEN** klien menyertakan parameter `q` yang tidak kosong setelah trim
- **THEN** sistem membatasi hasil ke entri yang cocok dengan aturan pencarian teks untuk resource tersebut (field yang diperiksa didokumentasikan per endpoint)

#### Scenario: Parameter ID filter tidak valid

- **WHEN** klien menyertakan parameter filter berupa ID yang tidak valid atau merujuk entitas yang tidak ada (sesuai keputusan implementasi)
- **THEN** sistem menolak permintaan dengan kesalahan klien yang jelas (mis. HTTP 400) atau perilaku lain yang didokumentasikan secara konsisten per endpoint

### Requirement: Kontrak parameter query untuk Siswa dan Kelas (MVP)

Untuk **daftar siswa** dan **daftar kelas**, sistem SHALL mendukung parameter berikut pada `GET` daftar:

- **`q`**: opsional; filter substring case-insensitive pada field teks yang ditetapkan per resource.
- **Siswa — filter facet opsional:** `programId`, `bidangStudiId`, `status` (nilai enum status siswa).
- **Kelas — filter facet opsional:** `tahunAjaran` (string tahun ajaran persis), `levelId`, `status` (nilai enum status kelas).

#### Scenario: Kombinasi filter siswa

- **WHEN** klien menyertakan `q` dan satu atau lebih filter facet siswa yang valid
- **THEN** hasil memenuhi semua kondisi yang diset secara bersamaan

#### Scenario: Kombinasi filter kelas

- **WHEN** klien menyertakan `q` dan satu atau lebih filter facet kelas yang valid
- **THEN** hasil memenuhi semua kondisi yang diset secara bersamaan
