# PRD — Sistem Informasi Akademik Studia Center

## Ringkasan
Sistem Informasi Akademik Studia Center adalah platform untuk mengelola operasional akademik lembaga bimbingan belajar, mulai dari pendaftaran siswa, pengelompokan level, pengaturan jadwal, pencatatan pertemuan dan absensi, input nilai, hingga pembuatan laporan progress, perkembangan, dan jadwal.

Dokumen ini merupakan adaptasi modern dari rancangan dalam skripsi *Rancangan Sistem Informasi Akademik pada Studia Center dengan Metodologi Berorientasi Obyek* (2012), dengan penambahan kebutuhan produk modern agar siap dibangun menggunakan agent AI.

## Latar Belakang
Proses akademik pada Studia Center pada dokumen sumber masih berjalan secara manual. Dampaknya adalah penumpukan dokumen, kesulitan pencarian data, tingginya risiko kesalahan saat pengolahan laporan, dan keterlambatan pembuatan laporan. Sistem baru dibutuhkan agar proses akademik menjadi lebih cepat, akurat, dan mudah diawasi.

## Tujuan Produk
- Meningkatkan kualitas pelayanan akademik di Studia Center.
- Mengurangi kesalahan pencatatan, pencarian data, dan pembuatan laporan.
- Menyediakan informasi akademik siswa secara cepat dan terstruktur.
- Menjadi dasar operasional yang siap diotomasi dengan agent AI.

## Scope MVP
### Master Data
- Data siswa
- Data level
- Data program kursus
- Data bidang studi / pelajaran
- Data tentor
- Data ruang

### Transaksi Akademik
- Pendaftaran siswa
- Penempatan siswa ke level / tahun ajaran
- Pengaturan jadwal kursus
- Pencatatan pertemuan
- Pencatatan absensi
- Input nilai

### Laporan
- Progress report siswa
- Laporan perkembangan siswa
- Laporan jadwal siswa / tentor

## Di Luar Scope MVP
- Modul keuangan penuh
- Payroll tentor
- Inventaris lengkap
- CRM marketing
- Portal cabang multi-tenant

## Pengguna dan Role
### Admin Sistem
Mengelola pengguna, role, hak akses, dan konfigurasi sistem.

### Staf Administrasi
Mengelola pendaftaran, master data siswa, dokumen operasional, dan laporan administratif.

### Staf Akademik
Mengelola level, jadwal, pertemuan, monitoring performa siswa, dan laporan akademik.

### Tentor
Mengisi kehadiran, catatan pertemuan, evaluasi, dan nilai siswa.

### Pimpinan
Memantau dashboard, laporan perkembangan, dan data operasional utama.

### Orang Tua / Siswa
Melihat progress report, jadwal, dan perkembangan belajar.

## Problem Statement
1. Dokumen masih berbasis kertas sehingga pencarian data lambat.
2. Histori siswa saat naik level rawan hilang.
3. Jadwal siswa dan tentor sulit disusun dan diubah.
4. Data absensi dan nilai sering tersebar dan tidak konsisten.
5. Laporan progress dan perkembangan membutuhkan waktu lama untuk dibuat.

## Visi Produk
Menjadi sistem akademik terpusat untuk bimbel yang rapi, cepat, akurat, dan siap dioperasikan bersama workflow agent AI.

## Outcome yang Diharapkan
- Proses administrasi lebih cepat.
- Kesalahan input menurun.
- Laporan dapat dibuat tepat waktu.
- Pimpinan memiliki visibilitas terhadap performa siswa dan operasional.
- Orang tua memperoleh informasi perkembangan belajar yang lebih jelas.

## Use Case Utama
### 1. Pendaftaran Siswa
Staf menerima formulir pendaftaran, memasukkan data siswa, memilih program, menentukan tipe kelas, dan membuat data siswa aktif.

### 2. Penempatan Siswa ke Level
Staf menempatkan siswa ke level dan tahun ajaran tertentu agar histori akademik tetap tersimpan.

### 3. Penyusunan Jadwal
Staf akademik membuat jadwal berdasarkan program, tentor, ruang, hari, jam, kapasitas, dan periode.

### 4. Pencatatan Pertemuan
Tentor atau staf mencatat materi, kehadiran, kemauan belajar, pemahaman, dan konsentrasi siswa pada setiap pertemuan.

### 5. Input Nilai
Tentor atau staf memasukkan hasil try out / evaluasi untuk tiap siswa dan bidang studi.

### 6. Cetak / Generate Laporan
Sistem menghasilkan progress report, laporan perkembangan, dan laporan jadwal.

## Functional Requirements
### Modul Master Data
1. Sistem harus mendukung CRUD data siswa.
2. Sistem harus mendukung CRUD data level.
3. Sistem harus mendukung CRUD data program kursus.
4. Sistem harus mendukung CRUD data bidang studi.
5. Sistem harus mendukung CRUD data tentor.
6. Sistem harus mendukung CRUD data ruang.
7. Sistem harus menghasilkan kode data tertentu secara otomatis, misalnya nomor induk, kode program, kode tentor, kode ruang, dan kode jadwal.

### Modul Siswa-Level
8. Sistem harus dapat menyimpan relasi siswa dengan level dan tahun ajaran.
9. Sistem harus menjaga histori siswa saat naik level.
10. Sistem harus memungkinkan pencarian siswa berdasarkan nomor induk dan nama.

### Modul Jadwal
11. Sistem harus memungkinkan pembuatan jadwal berdasarkan siswa-level, tentor, program, dan ruang.
12. Sistem harus menyimpan hari, jam, kapasitas, awal periode, akhir periode, dan semester / periode.
13. Sistem harus mencegah bentrok tentor pada slot waktu yang sama.
14. Sistem harus mencegah bentrok ruang pada slot waktu yang sama.
15. Sistem harus menampilkan daftar siswa yang masuk ke jadwal tertentu.

### Modul Pertemuan dan Absensi
16. Sistem harus mencatat pertemuan per jadwal.
17. Sistem harus menyimpan tanggal pertemuan dan materi.
18. Sistem harus menyimpan status kehadiran siswa.
19. Sistem harus menyimpan indikator pembelajaran seperti kemauan, pemahaman, dan konsentrasi.
20. Sistem harus menampilkan histori pertemuan per siswa.

### Modul Nilai
21. Sistem harus memungkinkan input nilai per siswa, per pertemuan, dan per bidang studi.
22. Sistem harus menyimpan waktu ujian / input nilai.
23. Sistem harus menghubungkan nilai dengan siswa, level, jadwal, pertemuan, dan bidang studi.
24. Sistem harus menampilkan riwayat nilai per siswa.

### Modul Laporan
25. Sistem harus menghasilkan progress report berdasarkan bulan, siswa, dan level.
26. Sistem harus menghasilkan laporan perkembangan siswa berdasarkan periode.
27. Sistem harus menghasilkan laporan jadwal berdasarkan periode.
28. Sistem harus mendukung ekspor ke PDF.
29. Sistem harus menyimpan histori laporan yang pernah digenerate.

### Search dan Monitoring
30. Sistem harus menyediakan pencarian cepat untuk siswa, tentor, program, jadwal, pertemuan, dan laporan.
31. Sistem harus menyediakan filter berdasarkan periode, level, program, dan tentor.
32. Sistem harus menyediakan dashboard ringkas untuk pimpinan.

## Non-Functional Requirements
### Security
- Setiap pengguna harus login.
- Sistem harus menerapkan role-based access control.
- Setiap perubahan penting harus tercatat di audit log.

### Performance
- Pencarian data utama maksimal 2 detik pada beban operasional normal.
- Generate laporan standar maksimal 10 detik.

### Reliability
- Sistem harus memiliki backup harian.
- Sistem harus mendukung restore data.

### Availability
- Sistem target berjalan berbasis web dan dapat diakses oleh staf dan tentor.

### Maintainability
- Arsitektur modul harus terpisah dengan API yang terdokumentasi.
- Semua business rules utama harus dapat diubah tanpa mengubah keseluruhan sistem.


## Rekomendasi Stack Teknologi

### Prinsip Pemilihan
Stack teknologi untuk produk ini disarankan berorientasi pada:
- type safety end-to-end
- setup yang cepat untuk iterasi produk
- kemudahan maintenance
- cocok untuk workflow OpenSpec dan AI-assisted development
- mudah diskalakan dari MVP ke fase automation dan agent AI

### Frontend
#### Pilihan Utama
- React
- Vite
- TypeScript

#### Rekomendasi Tambahan
- React Router untuk routing aplikasi admin dan portal
- TanStack Query untuk data fetching, caching, dan sinkronisasi state server
- Zustand untuk state client yang ringan
- Tailwind CSS untuk styling
- shadcn/ui untuk komponen UI admin yang cepat dan konsisten
- React Hook Form + Zod untuk form handling dan validasi
- Vitest + Testing Library untuk unit test frontend
- Playwright untuk end-to-end test

#### Rekomendasi Implementasi Frontend
Rekomendasi utama untuk frontend adalah:
- React + Vite + TypeScript
- React Router
- TanStack Query
- Zustand
- Tailwind CSS
- shadcn/ui
- React Hook Form + Zod

Kombinasi ini cocok untuk dashboard operasional seperti master data, jadwal, absensi, nilai, dan laporan karena ringan, cepat dikembangkan, dan mudah dipahami agent AI saat generate kode.

### Backend
#### Pilihan Utama
- Node.js
- TypeScript
- PostgreSQL
- Neon sebagai managed Postgres
- Prisma ORM

#### Rekomendasi Framework Backend
Rekomendasi utama backend adalah:
- Node.js + TypeScript + Fastify

Alasan:
- lebih ringan dan cepat untuk API operasional
- sederhana untuk vertical slice development
- cocok untuk service modular
- enak dipadukan dengan Prisma dan Zod
- overhead lebih rendah untuk MVP dibanding framework yang terlalu besar

#### Alternatif
- NestJS dapat dipilih jika tim nanti membesar dan membutuhkan pola arsitektur yang lebih opinionated sejak awal

#### Rekomendasi Tambahan Backend
- Zod untuk validasi schema request dan response
- JWT + refresh token atau session-based auth sesuai strategi auth
- Pino untuk logging
- OpenAPI / Swagger untuk dokumentasi API
- Vitest untuk unit test service / business logic
- Supertest atau integration test berbasis HTTP untuk pengujian API

### Database
#### Pilihan Utama
- PostgreSQL
- Neon

#### Rekomendasi Database
PostgreSQL di Neon sangat cocok untuk sistem ini karena:
- relasi data akademik cukup kuat dan terstruktur
- mendukung query laporan dengan baik
- cocok untuk transaksi seperti siswa-level, jadwal, pertemuan, dan nilai
- Prisma memiliki integrasi yang baik dengan Postgres

### ORM
#### Pilihan Utama
- Prisma ORM

#### Rekomendasi ORM
Prisma direkomendasikan karena:
- schema database mudah dibaca dan dipelihara
- migrasi lebih rapi untuk workflow produk
- type-safe query di TypeScript
- cocok untuk agent AI karena model data eksplisit

### Redis
#### Status
Opsional, tidak wajib untuk MVP awal.

#### Redis Direkomendasikan Jika
- butuh cache untuk dashboard atau laporan yang berat
- butuh rate limiting
- butuh session store terpusat
- butuh job queue / background jobs
- butuh orkestrasi task agent AI, notifikasi, atau generate report async

#### Rekomendasi Penggunaan Redis
Untuk MVP awal, sistem dapat dimulai tanpa Redis.
Tambahkan Redis ketika mulai masuk ke:
- report generation async
- parent notification queue
- AI agent orchestration
- caching query yang mahal

Jika Redis dipakai, rekomendasi tambahannya:
- BullMQ untuk queue dan worker
- ioredis sebagai client

### Rekomendasi Arsitektur Implementasi
#### Frontend App
- aplikasi web admin untuk staf, akademik, pimpinan
- halaman mobile-friendly minimum untuk tentor
- kemungkinan portal orang tua dipisahkan sebagai app / route terpisah pada fase berikutnya

#### Backend App
Disarankan dibagi menjadi:
- REST API utama untuk operasional sistem
- worker/background process untuk laporan async, notifikasi, dan agent AI
- database Postgres sebagai source of truth utama

#### Pola Pengembangan
Disarankan menggunakan:
- monorepo dengan pnpm workspace
- pemisahan package frontend, backend, shared types, dan worker
- shared schema / validation bila memungkinkan

Contoh struktur:
- apps/web
- apps/api
- apps/worker
- packages/shared
- packages/config

### Rekomendasi Deployment
#### Frontend
- Vercel atau Cloudflare Pages

#### Backend
- Railway, Fly.io, atau Render untuk MVP
- jika membutuhkan kontrol lebih besar, backend dapat dipindahkan ke container-based deployment

#### Database
- Neon PostgreSQL

### Rekomendasi Final Stack
#### Frontend Final Recommendation
- React
- Vite
- TypeScript
- React Router
- TanStack Query
- Zustand
- Tailwind CSS
- shadcn/ui
- React Hook Form
- Zod

#### Backend Final Recommendation
- Node.js
- TypeScript
- Fastify
- PostgreSQL
- Neon
- Prisma ORM
- Zod
- Pino

#### Optional When Needed
- Redis
- BullMQ
- ioredis

### Catatan Keputusan Teknologi
Untuk fase MVP, prioritas utama adalah menjaga sistem tetap sederhana, typed, dan cepat dikembangkan.
Karena itu, kombinasi yang paling direkomendasikan adalah:
- frontend: React + Vite + TypeScript + TanStack Query + Tailwind + shadcn/ui
- backend: Node.js + TypeScript + Fastify + Prisma + PostgreSQL di Neon
- Redis ditambahkan hanya saat kebutuhan queue, cache, atau orchestration benar-benar muncul


## Data Model Utama
### Entitas Inti
- Siswa
- Level
- Program
- BidangStudi
- Tentor
- Ruang
- SiswaLevel
- Jadwal
- Pertemuan
- DetailPertemuan
- Nilai

### Atribut Inti per Entitas
#### Siswa
- no_induk
- nama
- jenis_kelamin
- tempat_lahir
- tanggal_lahir
- asal_sekolah
- nama_orang_tua
- pekerjaan_orang_tua
- alamat
- telepon

#### Level
- kd_lvl
- nama_level

#### Program
- kd_program
- nama_program

#### BidangStudi
- kd_bidstud
- nama_bidang_studi
- kd_program

#### Tentor
- kd_tentor
- nama_tentor
- pendidikan
- jenis_kelamin
- tempat_lahir
- tanggal_lahir
- alamat
- telepon

#### Ruang
- kd_ruang
- nama_ruang

#### SiswaLevel
- no_induk
- kd_lvl
- tahun_ajaran

#### Jadwal
- kd_jadwal
- hari
- jam
- kapasitas
- awal
- akhir
- semester
- kd_program
- kd_tentor
- kd_ruang

#### Pertemuan
- no_pertemuan
- tanggal_pertemuan
- materi
- kd_bidstud

#### DetailPertemuan
- no_induk
- kd_lvl
- kd_jadwal
- no_pertemuan
- kemauan
- pemahaman
- konsentrasi
- status_kehadiran

#### Nilai
- no_induk
- kd_lvl
- kd_jadwal
- no_pertemuan
- kd_bidstud
- nilai
- waktu

## Business Rules
1. Siswa dapat mengikuti lebih dari satu program bila diizinkan kebijakan lembaga.
2. Setiap siswa aktif harus memiliki level aktif.
3. Setiap jadwal harus memiliki tepat satu tentor utama.
4. Pertemuan hanya dapat dicatat untuk jadwal yang aktif.
5. Nilai hanya dapat diinput untuk siswa yang terdaftar pada jadwal terkait.
6. Progress report ditarik dari kombinasi data nilai dan detail pertemuan.
7. Laporan perkembangan dihitung dari perubahan nilai antar periode.
8. Kelas private dan reguler harus dibedakan pada data pendaftaran / jadwal.

## UX Requirements
- Navigasi utama terdiri dari Master, Transaksi, Laporan, dan Pengaturan.
- Form entry harus sederhana dan cepat dipahami staf.
- Input relasional harus menggunakan pencarian / autocomplete.
- Halaman tentor harus mobile-friendly.
- Laporan harus bisa dipreview sebelum dicetak.

## Dashboard yang Disarankan
### Dashboard Pimpinan
- jumlah siswa aktif
- jumlah kelas aktif
- jumlah tentor aktif
- siswa dengan performa naik
- siswa dengan performa turun
- tingkat kehadiran siswa

### Dashboard Akademik
- jadwal hari ini
- tentor yang belum mengisi pertemuan
- siswa yang sering absen
- kelas yang penuh / hampir penuh

### Dashboard Tentor
- jadwal mengajar hari ini
- daftar siswa per kelas
- pertemuan yang belum diisi
- ringkasan performa siswa

## Integrasi Agent AI
### 1. Registration Agent
Fungsi:
- memvalidasi kelengkapan data pendaftaran
- mendeteksi potensi duplikasi siswa
- menyarankan penempatan awal level / program

### 2. Scheduling Agent
Fungsi:
- membantu menyusun jadwal
- mendeteksi bentrok ruang dan tentor
- menyarankan penggabungan atau pembukaan kelas baru

### 3. Session Notes Agent
Fungsi:
- membantu tentor merapikan catatan pertemuan
- mengubah catatan bebas menjadi format terstruktur
- menandai siswa yang butuh perhatian khusus

### 4. Assessment Agent
Fungsi:
- membantu validasi input nilai
- menghitung tren perkembangan
- memberi alert saat performa menurun

### 5. Reporting Agent
Fungsi:
- menyusun progress report otomatis
- membuat ringkasan perkembangan per siswa
- menyiapkan insight untuk pimpinan

### 6. Parent Communication Agent
Fungsi:
- menyusun pesan perkembangan belajar untuk orang tua
- mengirim pengingat jadwal atau absensi
- memberi notifikasi jika nilai menurun atau kehadiran bermasalah

## Guardrails untuk Agent AI
- Agent tidak boleh mengubah data final tanpa persetujuan user berwenang.
- Semua rekomendasi agent harus bisa direview sebelum dipublish.
- Setiap aksi agent harus tercatat di audit log.
- Agent hanya boleh mengakses data sesuai role pengguna.
- Nilai akhir tidak boleh diubah otomatis tanpa approval.

## Acceptance Criteria MVP
### Pendaftaran
- Data siswa baru dapat dibuat.
- Nomor induk otomatis terbentuk.
- Tipe kelas dapat dipilih.
- Data siswa dapat dicari kembali.

### Leveling
- Siswa dapat dihubungkan ke level dan tahun ajaran.
- Histori level lama tetap tersimpan.

### Jadwal
- Jadwal dapat dibuat dengan tentor, ruang, program, dan periode.
- Bentrok ruang dan tentor terdeteksi.
- Daftar siswa pada suatu jadwal terlihat jelas.

### Pertemuan
- Pertemuan dapat dicatat per jadwal.
- Kehadiran siswa dapat direkam.
- Materi dan evaluasi belajar dapat disimpan.

### Nilai
- Nilai dapat diinput dan dilacak per siswa.
- Riwayat nilai dapat ditampilkan.

### Laporan
- Progress report dapat digenerate berdasarkan bulan, siswa, dan level.
- Laporan perkembangan dapat digenerate berdasarkan periode.
- Laporan jadwal dapat digenerate berdasarkan periode.

## KPI Awal
- Waktu pencarian data turun minimal 80%.
- Waktu pembuatan laporan turun minimal 70%.
- Error input manual turun minimal 60%.
- 100% pertemuan memiliki catatan digital.
- 90% progress report bulanan terbit tepat waktu.

## Risiko dan Open Questions
1. Apakah portal orang tua akan masuk MVP atau fase berikutnya?
2. Apakah keuangan hanya reminder SPP atau modul penuh?
3. Apakah satu siswa bisa aktif pada beberapa program sekaligus?
4. Bagaimana aturan promosi level?
5. Siapa yang berwenang final approval nilai?
6. Bagaimana kebijakan penjadwalan kelas private?

## Roadmap
### Phase 1
- Auth dan role
- Master data
- Siswa-level
- Jadwal
- Pertemuan dan absensi
- Nilai
- Tiga laporan utama

### Phase 2
- Dashboard pimpinan
- Reminder absensi
- Alert performa siswa
- Export PDF dan notifikasi

### Phase 3
- Agent AI untuk penjadwalan
- Agent AI untuk catatan pertemuan
- Agent AI untuk laporan dan komunikasi orang tua

## Catatan Sumber
PRD ini disusun dari analisis skripsi *Rancangan Sistem Informasi Akademik pada Studia Center dengan Metodologi Berorientasi Obyek* dan dimodernisasi agar siap digunakan sebagai dokumen implementasi produk serta workflow agent AI.
