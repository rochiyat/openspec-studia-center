# Kelas dan roster siswa

## Purpose

Mendefinisikan **Kelas** (rombongan belajar) per level dan tahun ajaran, dengan **roster** siswa (anggota kelas), akses API terkontrol, dan jejak audit selaras modul akademik.

## Requirements

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

### Requirement: Roster siswa per kelas dapat dikelola

Sistem SHALL menyediakan penambahan dan penghapusan **anggota kelas** (siswa dalam kelas) tanpa duplikasi pasangan kelas–siswa.

#### Scenario: Menambah peserta ke kelas

- **WHEN** pengguna berwenang menambahkan siswa yang valid ke kelas
- **THEN** siswa tercatat sebagai anggota kelas tersebut

#### Scenario: Duplikasi anggota

- **WHEN** pengguna mencoba menambahkan siswa yang sudah menjadi anggota kelas yang sama
- **THEN** sistem menolak dengan pesan yang jelas

#### Scenario: Daftar anggota

- **WHEN** pengguna terotentikasi meminta daftar anggota suatu kelas
- **THEN** sistem mengembalikan kumpulan siswa (atau referensi yang cukup) untuk roster

### Requirement: Akses API kelas dan roster memerlukan otentikasi; mutasi memerlukan peran yang sesuai

Sistem SHALL membatasi pembuatan, pembaruan, dan penghapusan kelas serta mutasi anggota kepada pengguna dengan peran yang diizinkan (selaras modul siswa/akademik); pembacaan SHALL memerlukan otentikasi.

#### Scenario: Tanpa token

- **WHEN** klien memanggil API kelas atau anggota tanpa otentikasi yang valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang melakukan mutasi

- **WHEN** pengguna terotentikasi tanpa peran yang diizinkan mencoba mengubah kelas atau anggota
- **THEN** sistem menolak dengan kode HTTP 403

### Requirement: Perubahan penting pada kelas dan anggota dapat diaudit

Sistem SHALL mencatat jejak audit untuk operasi pembuatan, pembaruan, dan penghapusan kelas serta penambahan/penghapusan anggota, sesuai pola audit platform.

#### Scenario: Pencatatan audit

- **WHEN** operasi tersebut berhasil oleh pengguna berwenang
- **THEN** sistem menyimpan rekaman audit yang memungkinkan identifikasi aktor, entitas, dan waktu
