# Penempatan siswa ke level (per tahun ajaran)

## Purpose

Menghubungkan **Siswa** ke **Level** akademik untuk suatu **tahun ajaran**, dengan status penempatan dan jejak audit, sebagai dasar laporan kelas dan kebijakan akademik.

## Requirements

### Requirement: Penempatan siswa ke level dapat dikelola (CRUD)

Sistem SHALL menyediakan pengelolaan rekaman yang menghubungkan **Siswa** ke **Level** untuk suatu **tahun ajaran**, dengan status penempatan yang dapat dipantau.

#### Scenario: Membuat penempatan

- **WHEN** pengguna berwenang mengirim data penempatan dengan siswa, level, dan tahun ajaran yang valid
- **THEN** rekaman tersimpan dan tidak ada duplikasi untuk pasangan siswa dan tahun ajaran yang sama

#### Scenario: Daftar penempatan

- **WHEN** pengguna terotentikasi meminta daftar penempatan
- **THEN** sistem mengembalikan data dengan informasi siswa dan level yang cukup untuk administrasi

#### Scenario: Referensi tidak valid

- **WHEN** pengguna mengirim `siswaId` atau `levelId` yang tidak ada
- **THEN** sistem menolak dengan kesalahan validasi yang jelas

### Requirement: Unicitas penempatan per siswa dan tahun ajaran

Sistem SHALL memastikan untuk tahun ajaran yang sama seorang siswa tidak memiliki lebih dari satu rekaman penempatan level (pada cakupan MVP).

#### Scenario: Duplikasi tahun ajaran untuk siswa yang sama

- **WHEN** pengguna mencoba membuat penempatan kedua untuk siswa dan tahun ajaran yang sudah ada
- **THEN** sistem menolak dengan pesan yang jelas

### Requirement: Akses API penempatan memerlukan otentikasi; mutasi memerlukan peran yang sesuai

Sistem SHALL membatasi pembuatan, pembaruan, dan penghapusan data penempatan kepada pengguna dengan peran yang diizinkan (selaras modul siswa); pembacaan SHALL memerlukan otentikasi.

#### Scenario: Tanpa token

- **WHEN** klien memanggil API penempatan tanpa otentikasi yang valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang melakukan mutasi

- **WHEN** pengguna terotentikasi tanpa peran yang diizinkan mencoba membuat atau mengubah penempatan
- **THEN** sistem menolak dengan kode HTTP 403

### Requirement: Perubahan data penempatan penting dapat diaudit

Sistem SHALL mencatat jejak audit untuk operasi pembuatan, pembaruan, dan penghapusan penempatan level sesuai pola audit platform.

#### Scenario: Pencatatan audit

- **WHEN** operasi CRUD penempatan berhasil oleh pengguna berwenang
- **THEN** sistem menyimpan rekaman audit yang memungkinkan identifikasi aktor, entitas, dan waktu
