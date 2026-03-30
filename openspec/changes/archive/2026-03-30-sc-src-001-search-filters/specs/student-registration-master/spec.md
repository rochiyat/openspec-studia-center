## MODIFIED Requirements

### Requirement: Siswa dapat didaftarkan dan dikelola sebagai master data (CRUD)

Sistem SHALL menyediakan pengelolaan data siswa dengan nomor induk unik, nama, relasi ke **Program** dan **Bidang studi** yang valid, serta status kepesertaan. Pada **permintaan daftar siswa** (koleksi), sistem SHALL menerima parameter query opsional `q`, `programId`, `bidangStudiId`, dan `status` untuk membatasi hasil sesuai kontrak `master-list-search-filters`; jika tidak ada parameter filter, hasil SHALL setara daftar penuh.

#### Scenario: Mendaftarkan siswa baru

- **WHEN** pengguna berwenang mengirim data siswa lengkap dengan program dan bidang studi yang konsisten
- **THEN** siswa tersimpan dan nomor induk unik terjaga

#### Scenario: Daftar siswa tanpa filter

- **WHEN** pengguna terotentikasi meminta daftar siswa tanpa parameter query filter
- **THEN** sistem mengembalikan kumpulan siswa dengan informasi program dan bidang studi yang dipakai untuk administrasi

#### Scenario: Daftar siswa dengan pencarian atau filter

- **WHEN** pengguna terotentikasi meminta daftar siswa dengan satu atau lebih parameter `q`, `programId`, `bidangStudiId`, atau `status` yang valid
- **THEN** sistem mengembalikan subset siswa yang memenuhi kondisi tersebut dengan informasi program dan bidang studi yang dipakai untuk administrasi

#### Scenario: Program dan bidang studi tidak konsisten

- **WHEN** pengguna mengirim bidang studi yang tidak termasuk dalam program yang dipilih
- **THEN** sistem menolak dengan kesalahan validasi yang jelas
