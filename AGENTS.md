# AI Agent Contract

This repository is intentionally designed for parallel AI workers.

## Before editing

Read:

1. `README.md`
2. `SECURITY.md`
3. `CONTRIBUTING.md`
4. `docs/architecture/SECURITY_ARCHITECTURE.md`
5. the issue/spec assigned to the worker

Do not infer requirements that are not in the issue or architecture documents.

## Scope discipline

- One worker = one issue = one focused PR.
- Do not modify unrelated files.
- Do not silently weaken tests, security gates, types, lint rules, or authorization.
- Do not add dependencies without documenting why they are necessary.
- Do not introduce secrets, real user data, or production configuration.
- Do not merge or push directly to `main`.

## Security contract

Never accept frontend-provided ownership, role, tenant, entitlement, storage path, source, or protected timestamp fields as authoritative.

For private records, derive identity server-side and preserve database ownership invariants.

When changing an endpoint, add or update a cross-user authorization test.

When changing file handling, add malicious-upload tests.

When changing authentication, sessions, OAuth, webhooks, exports, or audit logging, update the security documentation and tests.

## Completion contract

Before opening a PR:

- run the repository validation command(s)
- inspect the final diff
- explain tests run and their results in the PR
- report any known limitation instead of hiding it
