# Pendaftaran & master data siswa

## Purpose

Master **Siswa** dengan nomor induk unik, identitas inti, relasi ke **Program** dan **Bidang studi** dari katalog, serta status kepesertaan, sebagai fondasi administrasi dan modul akademik berikutnya.

## Requirements

### Requirement: Siswa dapat didaftarkan dan dikelola sebagai master data (CRUD)

Sistem SHALL menyediakan pengelolaan data siswa dengan nomor induk unik, nama, relasi ke **Program** dan **Bidang studi** yang valid, serta status kepesertaan.

#### Scenario: Mendaftarkan siswa baru

- **WHEN** pengguna berwenang mengirim data siswa lengkap dengan program dan bidang studi yang konsisten
- **THEN** siswa tersimpan dan nomor induk unik terjaga

#### Scenario: Daftar siswa

- **WHEN** pengguna terotentikasi meminta daftar siswa
- **THEN** sistem mengembalikan kumpulan siswa dengan informasi program dan bidang studi yang dipakai untuk administrasi

#### Scenario: Program dan bidang studi tidak konsisten

- **WHEN** pengguna mengirim bidang studi yang tidak termasuk dalam program yang dipilih
- **THEN** sistem menolak dengan kesalahan validasi yang jelas

### Requirement: Nomor induk siswa unik dan terbentuk konsisten

Sistem SHALL memastikan nomor induk unik per siswa dan SHALL membentuk atau memvalidasi nomor induk sesuai aturan yang didokumentasikan di implementasi.

#### Scenario: Konflik nomor induk

- **WHEN** pengguna mengirim nomor induk yang sudah dipakai siswa lain
- **THEN** sistem menolak dengan pesan yang jelas

### Requirement: Akses API siswa memerlukan otentikasi; mutasi memerlukan peran yang sesuai

Sistem SHALL membatasi pembuatan, pembaruan, dan penghapusan data siswa kepada pengguna dengan peran yang diizinkan; pembacaan SHALL memerlukan otentikasi.

#### Scenario: Tanpa token

- **WHEN** klien memanggil API siswa tanpa otentikasi yang valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang melakukan mutasi

- **WHEN** pengguna terotentikasi tanpa peran yang diizinkan mencoba membuat atau mengubah data siswa
- **THEN** sistem menolak dengan kode HTTP 403

### Requirement: Perubahan data siswa yang penting dapat diaudit

Sistem SHALL mencatat jejak audit untuk operasi pembuatan, pembaruan, dan penghapusan data siswa sesuai pola audit platform.

#### Scenario: Pencatatan audit

- **WHEN** operasi CRUD siswa berhasil dilakukan oleh pengguna berwenang
- **THEN** sistem menyimpan rekaman audit yang memungkinkan identifikasi aktor, entitas, dan waktu
