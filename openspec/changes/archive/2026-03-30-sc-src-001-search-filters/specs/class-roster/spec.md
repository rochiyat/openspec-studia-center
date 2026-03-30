## MODIFIED Requirements

### Requirement: Kelas dapat dibuat dan dikelola (CRUD)

Sistem SHALL menyediakan pengelolaan **Kelas** dengan kode unik, nama, tahun ajaran, dan referensi **Level** wajib; referensi **Tentor** dan **Ruang** opsional sesuai implementasi. Pada **permintaan daftar kelas** (koleksi), sistem SHALL menerima parameter query opsional `q`, `tahunAjaran`, `levelId`, dan `status` untuk membatasi hasil sesuai kontrak `master-list-search-filters`; jika tidak ada parameter filter, hasil SHALL setara daftar penuh.

#### Scenario: Membuat kelas

- **WHEN** pengguna berwenang mengirim data kelas dengan level dan tahun ajaran yang valid
- **THEN** kelas tersimpan dan kode unik terjaga

#### Scenario: Daftar kelas tanpa filter

- **WHEN** pengguna terotentikasi meminta daftar kelas tanpa parameter query filter
- **THEN** sistem mengembalikan data kelas dengan informasi level (dan referensi lain jika ada) untuk administrasi

#### Scenario: Daftar kelas dengan pencarian atau filter

- **WHEN** pengguna terotentikasi meminta daftar kelas dengan satu atau lebih parameter `q`, `tahunAjaran`, `levelId`, atau `status` yang valid
- **THEN** sistem mengembalikan subset kelas yang memenuhi kondisi tersebut dengan informasi level (dan referensi lain jika ada) untuk administrasi

#### Scenario: Referensi tidak valid

- **WHEN** pengguna mengirim `levelId`, `tentorId`, atau `ruangId` yang tidak ada
- **THEN** sistem menolak dengan kesalahan validasi yang jelas
