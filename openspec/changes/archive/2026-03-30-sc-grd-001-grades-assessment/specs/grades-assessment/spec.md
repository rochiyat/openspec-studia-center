## ADDED Requirements

### Requirement: Nilai dapat dicatat per siswa, kelas, bidang studi, tahun ajaran, dan komponen

Sistem SHALL menyediakan pencatatan **nilai** (skor numerik dalam rentang yang ditetapkan) untuk kombinasi **Siswa**, **Kelas**, **BidangStudi**, **tahun ajaran**, dan **komponen penilaian** terenumerasi. Untuk kombinasi yang sama, sistem SHALL mempertahankan satu rekaman (upsert, bukan duplikat baris).

#### Scenario: Menyimpan atau memperbarui nilai

- **WHEN** pengguna berwenang mengirim data nilai dengan referensi yang valid dan siswa merupakan anggota kelas yang dimaksud
- **THEN** nilai tersimpan atau diperbarui sesuai kombinasi siswa, kelas, bidang studi, tahun ajaran, dan komponen

#### Scenario: Daftar nilai untuk suatu kelas

- **WHEN** pengguna terotentikasi meminta daftar nilai untuk suatu kelas (dengan filter yang didukung)
- **THEN** sistem mengembalikan data nilai beserta referensi siswa (dan bidang studi) yang cukup untuk administrasi

#### Scenario: Siswa bukan anggota kelas

- **WHEN** pengguna mencoba mencatat nilai untuk siswa yang tidak terdaftar sebagai anggota kelas tersebut
- **THEN** sistem menolak dengan kesalahan validasi yang jelas

#### Scenario: Bidang studi tidak selaras dengan siswa

- **WHEN** pengguna mengirim `bidangStudiId` yang tidak sesuai dengan bidang studi siswa (sesuai aturan MVP)
- **THEN** sistem menolak dengan kesalahan validasi yang jelas

### Requirement: Akses API nilai memerlukan otentikasi; mutasi memerlukan peran yang sesuai

Sistem SHALL membatasi pembuatan, pembaruan, dan penghapusan nilai kepada pengguna dengan peran yang diizinkan (selaras modul akademik/kelas); pembacaan SHALL memerlukan otentikasi.

#### Scenario: Tanpa token

- **WHEN** klien memanggil API nilai tanpa otentikasi yang valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang melakukan mutasi

- **WHEN** pengguna terotentikasi tanpa peran yang diizinkan mencoba mengubah nilai
- **THEN** sistem menolak dengan kode HTTP 403

### Requirement: Perubahan penting pada nilai dapat diaudit

Sistem SHALL mencatat jejak audit untuk operasi pembuatan, pembaruan, dan penghapusan nilai, sesuai pola audit platform.

#### Scenario: Pencatatan audit

- **WHEN** operasi tersebut berhasil oleh pengguna berwenang
- **THEN** sistem menyimpan rekaman audit yang memungkinkan identifikasi aktor, entitas, dan waktu
