# Security Policy

## Security objective

The application must prevent one authenticated user from reading, modifying, deleting, exporting, or inferring another user's data through any client-controlled identifier, URL, request body, query parameter, file path, pagination/filter mechanism, or API call.

## Baseline

- OWASP ASVS Level 2.
- OWASP API Security guidance.
- Defense in depth: application authorization + PostgreSQL Row-Level Security (RLS).
- Deny by default.

## Data classification

Treat the following as sensitive: profile data, sleep, mood, stress, soreness, fatigue, training, HR/HRV, wearable records, notes, photos, exports, and integration metadata.

Secrets such as sessions, OAuth refresh tokens, API keys, database credentials, and signing material require stronger handling and never belong in logs or source control.

## Non-negotiable rules

1. Authentication and authorization are separate.
2. The authenticated user identity is derived server-side.
3. `user_id`, `owner_user_id`, `tenant_id`, roles, permissions, feature entitlements, storage paths, timestamps, and source metadata supplied by clients are not trusted.
4. Every private table has an ownership boundary.
5. Every private object query is scoped by authenticated identity.
6. PostgreSQL RLS must fail closed if application identity is absent.
7. The application runtime database role must not bypass RLS.
8. Object storage is private.
9. Signed URLs are short-lived and issued only after authorization.
10. The Raspberry Pi never receives database credentials, unrestricted API tokens, OAuth refresh tokens, or object-storage credentials.
11. All state-changing browser requests use CSRF protection.
12. Security regression tests are required before merging security-sensitive changes.

## Reporting vulnerabilities

Do not open a public issue containing an exploitable vulnerability or secret. Use a private security-reporting channel when the project is published.

## Release gate

A feature is not security-complete until authorization, validation, logging, rate-limiting, secrets, storage, and regression tests have been reviewed according to the feature's threat model.
