# Threat Model

## Assets

Sensitive athlete wellness records, training records, wearable measurements, profile data, notes, photos, exports, OAuth connections, sessions, device credentials, database backups, and application secrets.

## Attackers

Unauthenticated internet attacker; authenticated malicious user; compromised account; credential-stuffing attacker; malicious browser content; malicious wearable callback; attacker with temporary physical access to a Raspberry Pi; developer/CI supply-chain compromise; accidental insider; leaked backup recipient.

## Critical abuse cases

### Broken object-level authorization

Attack: change an object ID in URL/body/query.

Mitigation: server-derived owner scope + FastAPI authorization dependency + PostgreSQL RLS.

Test: Alice requests, updates, deletes, searches, or exports Bob's resources.

### Mass assignment

Attack: submit protected fields such as `owner_user_id`, `role`, `is_admin`, timestamps, source, or feature entitlements.

Mitigation: explicit Pydantic input schemas with forbidden extra fields and server-controlled fields.

Test: malicious JSON containing every protected field.

### Account takeover

Attack: credential stuffing, reset abuse, stolen session.

Mitigation: mature authentication provider, throttling, breach-password screening, short-lived/revocable sessions, MFA/passkeys for administrators, step-up authentication for sensitive actions.

Test: repeated login/reset attempts, session revocation, password change, lost-device recovery.

### XSS/CSRF

Attack: malicious notes or cross-origin state-changing requests.

Mitigation: contextual escaping, CSP, secure cookies, SameSite, CSRF token and Origin checks.

Test: script payloads and cross-origin POST/PATCH/DELETE.

### SQL injection

Attack: crafted query/filter/input values.

Mitigation: parameterized database access and server-side field allowlists.

Test: SQL payloads in IDs, filters, sort fields, and notes.

### File upload abuse

Attack: spoofed MIME, SVG/HTML, executable, polyglot, oversized/decompression-bomb-style image, path traversal.

Mitigation: private quarantine, server-side inspection, allowlisted image formats, size/dimension limits, image decode/re-encode, generated storage keys, optional malware scan.

Test: malicious and malformed uploads.

### Object-storage exposure

Attack: public bucket, predictable storage path, long-lived URL.

Mitigation: private bucket, authorization before signing, short-lived signed URLs.

Test: anonymous access, cross-user signed URL, expired/revoked object.

### OAuth compromise

Attack: callback substitution, state replay, token exposure, malicious redirect.

Mitigation: PKCE, state, nonce when applicable, exact redirect URI allowlist, server-side token exchange, encrypted token storage.

Test: invalid/replayed state, mismatched provider, reused code.

### Webhook replay/spoofing

Attack: forged or replayed provider event.

Mitigation: signature verification, timestamp freshness, event idempotency, provider-specific schema validation.

Test: altered signature, stale timestamp, duplicate event.

### Raspberry Pi compromise

Attack: stolen or modified device extracts credentials or uses API beyond its intended scope.

Mitigation: no master secrets on device, narrow device identity, revocation, minimal local cache, kiosk hardening.

Test: revoked device, stale credential, unauthorized API endpoint.

### Rate-limit bypass

Attack: rotate IPs or identifiers and scrape/export resources.

Mitigation: combined IP/account/device/global buckets, pagination and export limits.

Test: repeated requests across changing IP/user-agent/identifiers.

### Backup compromise

Attack: stolen database/object backup.

Mitigation: encrypted backups, isolated credentials, restricted backup storage, retention policy, restore testing.

Test: restore into isolated environment and verify data/security policy integrity.
