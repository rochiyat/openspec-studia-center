## ADDED Requirements

### Requirement: Laporan PDF dapat dibuat untuk konteks kelas yang didukung

Sistem SHALL menyediakan pembuatan **berkas PDF** untuk jenis laporan yang didukung (minimal terkait **kelas**), dengan parameter yang cukup untuk mengambil data dari modul akademik yang ada.

#### Scenario: Permintaan laporan dengan parameter valid

- **WHEN** pengguna berwenang meminta generasi laporan dengan jenis dan `kelasId` yang valid
- **THEN** sistem menghasilkan berkas PDF dan merekam metadata laporan dengan status sukses

#### Scenario: Kelas tidak ditemukan

- **WHEN** pengguna meminta laporan untuk `kelasId` yang tidak ada
- **THEN** sistem menolak dengan kesalahan yang jelas dan tidak menyimpan file tidak konsisten

### Requirement: Riwayat laporan dapat dilihat dan file dapat diunduh secara terkontrol

Sistem SHALL menyimpan **riwayat** generasi laporan (metadata: aktor, waktu, jenis, referensi kelas, lokasi/identifier file, status). Pengguna berwenang SHALL dapat **melihat daftar** riwayat dan **mengunduh** berkas sesuai rekaman yang ada.

#### Scenario: Daftar riwayat

- **WHEN** pengguna berwenang meminta daftar riwayat laporan
- **THEN** sistem mengembalikan entri yang cukup untuk identifikasi dan audit operasional

#### Scenario: Unduh berkas

- **WHEN** pengguna berwenang meminta unduhan untuk id laporan yang valid dan berkas masih tersedia
- **THEN** sistem mengembalikan berkas PDF dengan header yang sesuai

#### Scenario: Rekaman tidak ada atau file hilang

- **WHEN** id tidak ditemukan atau berkas tidak lagi ada di penyimpanan
- **THEN** sistem menolak dengan kode kesalahan yang jelas (mis. 404)

### Requirement: Akses API laporan memerlukan otentikasi dan peran yang sesuai

Sistem SHALL membatasi generasi, daftar, dan unduh laporan kepada pengguna dengan peran yang diizinkan (selaras modul akademik); semua endpoint terkait SHALL memerlukan otentikasi.

#### Scenario: Tanpa token

- **WHEN** klien memanggil API laporan tanpa otentikasi yang valid
- **THEN** sistem menolak dengan kode HTTP 401

#### Scenario: Peran tidak berwenang

- **WHEN** pengguna terotentikasi tanpa peran yang diizinkan mencoba membuat atau mengunduh laporan
- **THEN** sistem menolak dengan kode HTTP 403

### Requirement: Aksi penting terkait laporan dapat diaudit

Sistem SHALL mencatat jejak audit untuk generasi dan unduh laporan (atau gabungan yang setara), selaras pola audit platform.

#### Scenario: Pencatatan audit

- **WHEN** operasi tersebut berhasil oleh pengguna berwenang
- **THEN** sistem menyimpan rekaman audit yang memungkinkan identifikasi aktor, entitas, dan waktu
