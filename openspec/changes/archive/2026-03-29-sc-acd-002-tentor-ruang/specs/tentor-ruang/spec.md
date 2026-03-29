## ADDED Requirements

### Requirement: Tentor dapat dikelola sebagai master data (CRUD)

Sistem SHALL menyediakan pengelolaan data tentor dengan kode unik dan nama; setiap tentor SHALL dapat ditautkan ke paling banyak satu akun `User` untuk menyelaraskan identitas login dengan profil pengajar.

#### Scenario: Membuat tentor baru

- **WHEN** pengguna berwenang mengirim data tentor dengan nama dan kode yang valid (kode dapat dihasilkan sistem jika tidak diisi)
- **THEN** tentor tersimpan dan kode unik terjaga dalam lingkup entitas tentor

#### Scenario: Menautkan tentor ke pengguna

- **WHEN** pengguna berwenang mengisi referensi ke `User` yang ada pada tentor
- **THEN** asosiasi tersimpan dan tidak lebih dari satu tentor memakai `User` yang sama

#### Scenario: Daftar tentor

- **WHEN** pengguna berwenang meminta daftar tentor
- **THEN** sistem mengembalikan kumpulan tentor yang dapat dipakai untuk administrasi dan dropdown

### Requirement: Ruang dapat dikelola sebagai master data (CRUD)

Sistem SHALL menyediakan pengelolaan ruang belajar dengan kode unik, nama, dan kapasitas opsional.

#### Scenario: Membuat ruang

- **WHEN** pengguna berwenang mengirim data ruang dengan kode dan nama yang valid
- **THEN** ruang tersimpan dan kode unik terjaga

#### Scenario: Daftar ruang

- **WHEN** pengguna berwenang meminta daftar ruang
- **THEN** sistem mengembalikan kumpulan ruang yang dapat dipakai untuk penjadwalan atau referensi berikutnya

### Requirement: Kode tentor dan ruang konsisten dan tidak bentrok

Sistem SHALL memastikan kode untuk tentor unik dalam koleksi tentor dan kode untuk ruang unik dalam koleksi ruang; aturan pembentukan kode SHALL didokumentasikan dan ditegakkan oleh validasi server.

#### Scenario: Konflik kode tentor

- **WHEN** pengguna mengirim kode tentor yang sudah dipakai tentor lain
- **THEN** sistem menolak dengan pesan yang jelas

#### Scenario: Konflik kode ruang

- **WHEN** pengguna mengirim kode ruang yang sudah dipakai ruang lain
- **THEN** sistem menolak dengan pesan yang jelas

### Requirement: Akses API tentor dan ruang memerlukan otentikasi; mutasi memerlukan peran yang sesuai

Sistem SHALL membatasi pembuatan, pembaruan, dan penghapusan data tentor dan ruang kepada pengguna dengan peran yang diizinkan sesuai kebijakan implementasi (selaras pola katalog: minimal peran administrasi akademik); pembacaan SHALL memerlukan otentikasi.

#### Scenario: Tanpa token

- **WHEN** klien memanggil API tentor atau ruang tanpa otentikasi yang valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang melakukan mutasi

- **WHEN** pengguna terotentikasi tanpa peran yang diizinkan mencoba membuat atau mengubah data tentor atau ruang
- **THEN** sistem menolak dengan kode HTTP 403
