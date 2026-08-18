# Security Architecture

## System boundaries

```text
Browser / PWA
    -> HTTPS -> reverse proxy
    -> FastAPI
    -> PostgreSQL
    -> private object storage

Wearable provider
    -> OAuth callback / signed webhook -> FastAPI

Raspberry Pi kiosk
    -> device-authenticated portrait API -> private object storage
```

The browser and Raspberry Pi are untrusted clients. PostgreSQL is never internet-accessible.

## Identity

Authentication is delegated to a mature OIDC-capable identity provider rather than implementing password hashing, reset, MFA, or token rotation from scratch.

Browser sessions use secure server-controlled cookies. State-changing requests require CSRF protection.

## Authorization

Authentication identifies the actor. Authorization determines allowed operations. Ownership determines which objects are in scope.

The authenticated user identity is derived by the backend and propagated to the database transaction. Client-provided ownership fields are ignored/rejected.

## Database boundary

Every private table contains `owner_user_id` referencing `users.id`.

PostgreSQL Row-Level Security is enabled and forced for private tables. The application runtime database role must not own those tables and must not have `BYPASSRLS`.

## Object storage

Buckets are private. Storage keys are generated server-side. The browser and Pi never browse a bucket directly.

The API authorizes the object, then issues a short-lived signed URL.

## Raspberry Pi

The kiosk is treated as physically compromisable. It receives a device identity with only the permissions required to fetch its assigned portrait manifest/photos. It never stores database credentials, OAuth refresh tokens, or unrestricted service credentials.

## Wearables

OAuth uses authorization code + PKCE, transaction-bound state, strict redirect URI allowlists, and encrypted token storage. Webhooks require source signature verification, timestamp/replay protection, and idempotency.

## Security invariant

For every private object:

```text
ALLOW only when:
    authenticated
    AND authorized for the operation
    AND object is within authenticated owner's scope
```
