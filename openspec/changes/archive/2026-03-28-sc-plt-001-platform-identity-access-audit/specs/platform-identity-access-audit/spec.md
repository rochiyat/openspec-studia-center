## ADDED Requirements

### Requirement: Pengguna terotentikasi dapat memulai sesi aplikasi

Sistem SHALL menyediakan mekanisme otentikasi sehingga hanya pengguna dengan kredensial valid yang dapat mengakses API dan UI yang dilindungi.

#### Scenario: Login berhasil

- **WHEN** pengguna mengirim kredensial yang valid ke endpoint login
- **THEN** sistem mengembalikan bukti sesi atau token yang memungkinkan akses berikutnya sesuai keputusan desain

#### Scenario: Login gagal

- **WHEN** pengguna mengirim kredensial yang tidak valid
- **THEN** sistem menolak dengan respons kesalahan generik (tanpa membocorkan apakah username atau password salah secara spesifik) dan tidak menerbitkan sesi

### Requirement: Pengguna dapat mengakhiri sesi

Sistem SHALL memungkinkan pengguna logout sehingga bukti sesi/token tidak lagi diterima untuk akses dilindungi.

#### Scenario: Logout

- **WHEN** pengguna yang terotentikasi meminta logout
- **THEN** sesi/token tersebut di-invalidasi atau dihapus sesuai model keamanan yang dipilih

### Requirement: Akses ke sumber daya dilindungi memeriksa otentikasi

Sistem SHALL menolak permintaan ke sumber daya yang dilindungi jika tidak ada sesi/token valid.

#### Scenario: Akses tanpa kredensial

- **WHEN** klien memanggil endpoint atau membuka rute yang membutuhkan otentikasi tanpa bukti sesi/token
- **THEN** sistem menolak dengan kode HTTP yang sesuai (mis. 401)

### Requirement: Kontrol akses berbasis peran (RBAC)

Sistem SHALL menerapkan role-based access control: setiap aksi atau endpoint sensitif SHALL memeriksa bahwa pengguna memiliki peran atau izin yang memadai.

#### Scenario: Akses ditolak karena peran tidak cukup

- **WHEN** pengguna terotentikasi dengan peran A mencoba melakukan operasi yang hanya diizinkan untuk peran B
- **THEN** sistem menolak dengan kode HTTP yang sesuai (mis. 403)

#### Scenario: Peran selaras kebutuhan PRD

- **WHEN** administrator mengonfigurasi pengguna dengan salah satu peran yang didefinisikan untuk Studia Center (minimal: Admin Sistem, Staf Administrasi, Staf Akademik, Tentor, Pimpinan)
- **THEN** sistem dapat membatasi fitur sesuai kebijakan peran yang didokumentasikan di desain

### Requirement: Administrasi pengguna dan peran (minimal MVP)

Sistem SHALL menyediakan kemampuan bagi Admin Sistem untuk mengelola akun pengguna dan penempatan peran agar operasional dapat berjalan.

#### Scenario: Membuat atau mengaktifkan pengguna

- **WHEN** Admin Sistem membuat pengguna baru atau mengaktifkan akun
- **THEN** pengguna tersebut dapat login sesuai aturan kredensial yang berlaku

### Requirement: Audit log untuk peristiwa penting

Sistem SHALL mencatat jejak audit untuk peristiwa yang diklasifikasikan sebagai penting (minimal: login sukses/gagal dapat dipertimbangkan; perubahan konfigurasi user/role; definisi perluasan di modul berikutnya).

#### Scenario: Peristiwa audit tercatat

- **WHEN** terjadi aksi yang masuk daftar peristiwa audit yang disepakati
- **THEN** sistem menyimpan rekaman minimal: siapa, apa, kapan, dan konteks yang cukup untuk review (tanpa menyimpan rahasia dalam bentuk plain text)

### Requirement: Perubahan data sensitif tidak mengabaikan audit

Sistem SHALL memastikan pola pemanggilan audit log dapat digunakan oleh modul domain lain tanpa duplikasi logika yang bertentangan.

#### Scenario: Ekstensi dari modul lain

- **WHEN** modul akademik lain memanggil API internal atau helper audit yang disediakan
- **THEN** rekaman audit konsisten dengan format dan penyimpanan yang sama
