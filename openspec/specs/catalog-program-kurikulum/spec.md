# Katalog program & kurikulum

## Purpose

Master **Level**, **Program**, dan **Bidang studi** dengan kode unik dan relasi bidang studi → program, sebagai referensi untuk pendaftaran, jadwal, dan modul akademik lainnya.

## Requirements

### Requirement: Level dapat dikelola secara penuh (CRUD)

Sistem SHALL menyediakan pengelolaan data level dengan kode unik dan nama level. Pengguna yang berwenang SHALL dapat membuat, membaca, memperbarui, dan menghapus level sesuai kebijakan RBAC.

#### Scenario: Membuat level baru

- **WHEN** pengguna berwenang mengirim data level dengan kode dan nama yang valid
- **THEN** level tersimpan dan kode tidak bentrok dengan level lain

#### Scenario: Daftar level

- **WHEN** pengguna berwenang meminta daftar level
- **THEN** sistem mengembalikan kumpulan level yang dapat dipakai untuk dropdown dan pencarian

### Requirement: Program kursus dapat dikelola secara penuh (CRUD)

Sistem SHALL menyediakan pengelolaan program kursus dengan kode unik dan nama program.

#### Scenario: Membuat program

- **WHEN** pengguna berwenang mengirim data program dengan kode dan nama yang valid
- **THEN** program tersimpan dan kode unik terjaga

### Requirement: Bidang studi terikat pada program dan dapat dikelola (CRUD)

Sistem SHALL menyediakan pengelolaan bidang studi dengan kode unik dan nama; setiap bidang studi SHALL terhubung ke tepat satu program.

#### Scenario: Membuat bidang studi untuk sebuah program

- **WHEN** pengguna berwenang mengirim data bidang studi termasuk referensi program yang ada
- **THEN** bidang studi tersimpan dan terasosiasi ke program tersebut

#### Scenario: Mencegah bidang studi tanpa program

- **WHEN** pengguna mencoba membuat bidang studi tanpa program valid
- **THEN** sistem menolak permintaan dengan kesalahan validasi

### Requirement: Kode entitas katalog terbentuk secara konsisten

Sistem SHALL memastikan kode untuk level, program, dan bidang studi unik dalam jenisnya dan terbentuk menurut aturan yang didokumentasikan (otomatis atau tervalidasi manual).

#### Scenario: Konflik kode

- **WHEN** pengguna mengirim kode yang sudah dipakai entitas lain pada jenis yang sama
- **THEN** sistem menolak dengan pesan yang jelas

### Requirement: Akses API katalog memerlukan otentikasi dan peran yang sesuai

Sistem SHALL membatasi mutasi data katalog kepada pengguna dengan peran yang diizinkan; pembacaan dapat diizinkan lebih luas sesuai kebijakan yang ditetapkan di implementasi.

#### Scenario: Tanpa token

- **WHEN** klien memanggil API katalog tanpa otentikasi yang valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang melakukan mutasi

- **WHEN** pengguna terotentikasi tanpa peran yang diizinkan mencoba membuat atau mengubah data katalog
- **THEN** sistem menolak dengan kode HTTP 403
