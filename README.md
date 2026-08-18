# Athlete Wellness Platform

Privacy-sensitive athlete wellness, training-feedback, wearable-integration, and portrait/kiosk platform.

## Security baseline

This repository treats sleep, mood, stress, soreness, training, HR/HRV, wearable data, notes, profile data, and photos as sensitive health-adjacent personal data.

The minimum application security target is OWASP ASVS Level 2. Authorization is enforced in FastAPI and at the PostgreSQL ownership boundary. Client-provided ownership, role, tenant, entitlement, storage-path, timestamp, and source metadata are never authoritative.

## Repository policy

- `main` is protected and merge-only.
- All application changes arrive through pull requests.
- Every AI worker operates on its own branch/worktree and owns a single scoped change.
- CI is the merge gate.
- No secrets or real user data belong in this repository.
- Security regressions become permanent automated tests.

## Planned architecture

React + TypeScript PWA → FastAPI → PostgreSQL

Private object storage is accessed only through authorized API requests and short-lived signed URLs. Raspberry Pi clients are treated as untrusted, capability-limited display devices.

## Status

Foundation stage. No production feature modules should be implemented until the security foundation and CI gates are passing.

See [`docs/development/SETUP.md`](docs/development/SETUP.md) and [`docs/development/AI_WORKFLOW.md`](docs/development/AI_WORKFLOW.md).
