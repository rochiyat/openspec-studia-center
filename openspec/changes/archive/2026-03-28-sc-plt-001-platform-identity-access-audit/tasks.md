## 1. Skema data & migrasi

- [x] 1.1 Definisikan model Prisma: User (password hash, status), Role, relasi User–Role, RefreshToken (hash, expiresAt, revoked), AuditLog
- [x] 1.2 Jalankan migrasi awal ke PostgreSQL (Neon/local) dan pastikan `prisma generate` sukses

## 2. Otentikasi (API)

- [x] 2.1 Implementasi hash password (Argon2/bcrypt) saat create/update user
- [x] 2.2 Endpoint POST login: validasi kredensial, terbitkan access JWT + refresh token (simpan hash refresh di DB)
- [x] 2.3 Endpoint POST refresh: rotasi refresh token, tolak jika revoke/expired
- [x] 2.4 Endpoint POST logout: revoke refresh token yang dipakai
- [x] 2.5 Middleware Fastify: verifikasi JWT pada route dilindungi; inject `userId` / roles ke request

## 3. RBAC & admin pengguna

- [x] 3.1 Seed role sesuai PRD (Admin, Staf Administrasi, Staf Akademik, Tentor, Pimpinan) dan dokumentasi mapping izin per route (minimal matriks sederhana)
- [x] 3.2 Helper `requireRole(...roles)` atau setara untuk route admin vs akademik
- [x] 3.3 Endpoint admin (hanya Admin Sistem): CRUD user minimal — create, list, update status, assign role

## 4. Audit log

- [x] 4.1 Service `recordAudit({ actorUserId, action, entityType, entityId?, metadata? })` ke tabel AuditLog
- [x] 4.2 Panggil audit pada login sukses/gagal (opsional gagal dengan rate limit) dan pada perubahan user/role
- [x] 4.3 Dokumentasikan kontrak untuk modul domain lain (impor helper dari package shared jika monorepo)

## 5. API & dokumentasi

- [x] 5.1 Skema request/response Zod + OpenAPI untuk auth dan admin user
- [x] 5.2 Uji integrasi HTTP (login, refresh, logout, 401 tanpa token, 403 salah peran)

## 6. Frontend (jika scope change mencakup UI)

- [x] 6.1 Halaman login + penyimpanan token sesuai desain (header atau cookie HTTP-only)
- [x] 6.2 Guard route React Router + redirect jika tidak terotentikasi
- [x] 6.3 Layar admin minimal: daftar user & assign role (hanya untuk role admin)
