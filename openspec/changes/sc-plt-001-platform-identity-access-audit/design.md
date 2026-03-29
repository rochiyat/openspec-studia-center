## Context

Studia Center membutuhkan fondasi keamanan sebelum modul master data dan transaksi akademik. PRD mensyaratkan login wajib, RBAC, dan audit log. Stack yang direkomendasikan di PRD: backend Node.js + TypeScript + Fastify + Prisma + PostgreSQL; frontend React + Vite + TypeScript. Tidak ada implementasi legacy yang perlu dimigrasi pada change ini.

## Goals / Non-Goals

**Goals:**

- Otentikasi stateless (JWT access + refresh) atau session server-side — satu pilihan tegas dan dokumentasi alur login/logout/refresh.
- Model data User, Role (dan Permission jika dipakai), relasi user–role, serta tabel AuditLog dengan indeks waktu dan aktor.
- Middleware/guard di Fastify untuk auth + RBAC; pola yang sama dipakai semua route baru.
- API admin minimal: CRUD user (scoped admin), assign role.
- Hook audit yang dapat dipanggil dari service layer modul lain.

**Non-Goals:**

- SSO/SAML, OAuth provider eksternal (Google, dll.) pada MVP ini.
- Multi-tenant cabang.
- Portal orang tua / siswa (hanya menyiapkan peran jika sudah ada definisi; UI portal bisa fase berikut).

## Decisions


| Keputusan           | Pilihan                                                                                                             | Alternatif                       | Alasan singkat                                                                              |
| ------------------- | ------------------------------------------------------------------------------------------------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------- |
| Otentikasi          | JWT access + refresh token (HTTP-only cookie atau header)                                                           | Session DB + cookie              | Cocok API + SPA; refresh memudahkan revoke; skalasi horizontal tanpa sticky session         |
| Penyimpanan refresh | Tabel `RefreshToken` (hash), TTL/revoke                                                                             | Redis                            | PRD: Redis opsional MVP — Postgres cukup untuk awal                                         |
| Role                | Enum/string `Role` di DB + join `UserRole`                                                                          | Permission granular per resource | PRD menyebut role per pekerjaan; permission halus bisa ditambah tanpa mengubah kontrak awal |
| Password            | Argon2 atau bcrypt (via library standar)                                                                            | Plain                            | —                                                                                           |
| Audit               | Tabel append-only `AuditLog` dengan `actorUserId`, `action`, `entityType`, `entityId`, `metadata` JSON, `createdAt` | Event streaming                  | Cukup untuk MVP; migrasi ke queue nanti jika perlu                                          |


## Risks / Trade-offs

- **JWT dicuri di klien** → Mitigasi: HTTPS wajib, refresh rotation pendek, CSRF jika cookie.
- **Audit log membesar** → Mitigasi: retention policy & arsip (fase berikut); indeks tanggal.
- **Definisi “peristiwa penting” belum lengkap dari domain** → Mitigasi: daftar minimal di MVP + extensible `action`/`metadata`; modul berikutnya menambah pemanggilan audit tanpa mengubah skema dasar jika memungkinkan.

## Migration Plan

- Deploy skema Prisma (user, role, token, audit) ke database kosong atau migrasi awal.
- Seed role default dari PRD + satu user admin (via env/secret sekali pakai) — dokumentasikan di README operasional.
- Rollback: migrasi down atau restore snapshot DB pra-deploy.

## Open Questions

- Apakah Orang Tua / Siswa membutuhkan akun terpisah di MVP (PRD open question) — jika ya, peran dan scope data perlu ditambahkan di iterasi berikutnya.
- Daftar final endpoint yang wajib audit sebelum rilis modul akademik (disepakati bersama product).

