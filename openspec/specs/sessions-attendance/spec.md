# Sesi pertemuan dan presensi

## Purpose

Mencatat **sesi pertemuan** per **Kelas** (waktu, status sesi) dan **presensi** siswa per sesi, dengan akses terkontrol dan audit, memanfaatkan roster anggota kelas yang sudah ada.

## Requirements

### Requirement: Sesi pertemuan dapat dibuat dan dikelola per kelas

Sistem SHALL menyediakan pengelolaan **sesi pertemuan** yang selalu terikat pada satu **Kelas**, dengan waktu mulai wajib, waktu selesai opsional, dan status siklus hidup sesi. Sesi SHALL hanya relevan dalam konteks `kelasId` yang valid.

#### Scenario: Membuat sesi

- **WHEN** pengguna berwenang mengirim data sesi dengan `kelasId` dan waktu mulai yang valid
- **THEN** rekaman sesi tersimpan dan terhubung ke kelas yang dimaksud

#### Scenario: Daftar sesi per kelas

- **WHEN** pengguna terotentikasi meminta daftar sesi untuk suatu kelas
- **THEN** sistem mengembalikan sesi-sesi milik kelas tersebut dengan urutan yang dapat dipakai untuk operasional (mis. terbaru atau menurut waktu)

#### Scenario: Kelas tidak ada

- **WHEN** klien meminta sesi untuk `kelasId` yang tidak ada
- **THEN** sistem menolak dengan kesalahan yang jelas (mis. 404 atau 400 sesuai konvensi API)

### Requirement: Presensi siswa dapat dicatat per sesi

Sistem SHALL menyediakan pencatatan **presensi** untuk pasangan **sesi** dan **siswa** dengan status kehadiran terenumerasi. Untuk satu sesi, seorang siswa SHALL hanya memiliki satu rekaman presensi. Sistem SHALL menolak presensi untuk siswa yang bukan anggota kelas dari sesi tersebut.

#### Scenario: Mencatat atau memperbarui presensi

- **WHEN** pengguna berwenang mengirim presensi untuk siswa yang merupakan anggota kelas sesuai sesi
- **THEN** status kehadiran tersimpan atau diperbarui untuk pasangan sesi–siswa tersebut

#### Scenario: Siswa bukan anggota kelas

- **WHEN** pengguna mencoba mencatat presensi untuk siswa yang tidak terdaftar di kelas sesi tersebut
- **THEN** sistem menolak dengan kesalahan validasi yang jelas

#### Scenario: Duplikasi presensi

- **WHEN** sistem telah memiliki presensi untuk pasangan sesi dan siswa yang sama
- **THEN** pembaruan SHALL menggantikan status tanpa membuat baris duplikat

#### Scenario: Daftar presensi untuk satu sesi

- **WHEN** pengguna terotentikasi meminta daftar presensi untuk suatu sesi
- **THEN** sistem mengembalikan kumpulan presensi dengan referensi siswa yang cukup untuk roster kehadiran

### Requirement: Akses API sesi dan presensi memerlukan otentikasi; mutasi memerlukan peran yang sesuai

Sistem SHALL membatasi pembuatan, pembaruan, dan penghapusan sesi serta mutasi presensi kepada pengguna dengan peran yang diizinkan (selaras modul kelas/siswa); pembacaan SHALL memerlukan otentikasi.

#### Scenario: Tanpa token

- **WHEN** klien memanggil API sesi atau presensi tanpa otentikasi yang valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang melakukan mutasi

- **WHEN** pengguna terotentikasi tanpa peran yang diizinkan mencoba mengubah sesi atau presensi
- **THEN** sistem menolak dengan kode HTTP 403

### Requirement: Perubahan penting pada sesi dan presensi dapat diaudit

Sistem SHALL mencatat jejak audit untuk operasi pembuatan, pembaruan, dan penghapusan sesi serta mutasi presensi yang relevan, sesuai pola audit platform.

#### Scenario: Pencatatan audit

- **WHEN** operasi tersebut berhasil oleh pengguna berwenang
- **THEN** sistem menyimpan rekaman audit yang memungkinkan identifikasi aktor, entitas, dan waktu
