# Contributing

## Branch policy

Never develop directly on `main`.

Branch format:

- `feat/<scope>-<short-name>`
- `fix/<scope>-<short-name>`
- `sec/<scope>-<short-name>`
- `ci/<scope>-<short-name>`
- `docs/<scope>-<short-name>`
- `chore/<scope>-<short-name>`

One branch should represent one coherent change.

## Pull requests

Every PR must:

1. State the problem and intended behavior.
2. Identify security/privacy impact.
3. Identify changed trust boundaries or authorization behavior.
4. Include tests for new security-sensitive behavior.
5. Avoid unrelated refactors.
6. Pass all required CI checks.

## AI-generated work

AI workers are contributors, not maintainers. They must not merge their own changes to `main`.

Each AI worker receives one scoped issue/specification, creates one branch, runs the required checks, and opens a PR. The orchestrator/maintainer reviews the diff and CI results before merge.

## Secrets and data

Never commit:

- `.env` files containing real values
- API keys
- OAuth secrets/tokens
- database credentials
- private keys
- production dumps
- real user photos or wellness records

Use synthetic fixtures only.

## Database changes

Schema changes require migrations and corresponding authorization/RLS tests when private data is affected.
