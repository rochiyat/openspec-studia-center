## ADDED Requirements

### Requirement: Ringkasan operasional tersedia melalui API teragregasi

Sistem SHALL menyediakan endpoint **baca saja** yang mengembalikan **agregat numerik** operasional (hitungan entitas relevan dari data yang sudah ada) untuk mendukung dashboard, tanpa mengembalikan daftar penuh entitas individu sebagai muatan utama respons.

#### Scenario: Pengguna berwenang meminta ringkasan

- **WHEN** pengguna dengan peran yang diizinkan memanggil endpoint ringkasan operasional
- **THEN** sistem mengembalikan struktur JSON dengan field angka yang didokumentasikan (minimal total siswa aktif, jumlah kelas aktif, dan setidaknya satu metrik tambahan terkait aktivitas akademik)

#### Scenario: Tanpa otentikasi

- **WHEN** klien memanggil endpoint ringkasan tanpa token valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang

- **WHEN** pengguna terotentikasi dengan peran yang tidak diizinkan (mis. peran yang dibatasi hanya ke konteks mengajar sendiri)
- **THEN** sistem menolak dengan kode HTTP 403

### Requirement: Dashboard operasional dapat ditampilkan di antarmuka web

Sistem SHALL menyediakan **halaman** (atau area setara) yang menampilkan ringkasan angka dari API operasional kepada pengguna yang telah masuk dan berwenang, dengan tampilan yang dapat dipahami tanpa pelatihan khusus (mis. label teks untuk setiap angka).

#### Scenario: Menampilkan ringkasan setelah masuk

- **WHEN** pengguna berwenang membuka halaman dashboard operasional
- **THEN** antarmuka memuat data dari API ringkasan dan menampilkan angka-angka utama kepada pengguna

#### Scenario: Gagal memuat data

- **WHEN** API mengembalikan kesalahan atau tidak tersedia
- **THEN** antarmuka menampilkan pesan kesalahan singkat yang dapat dipahami pengguna
