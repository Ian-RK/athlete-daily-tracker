# CI Roadmap

The CI foundation is intentionally staged so each gate can be introduced and tested through its own PR.

## Required eventually

### Repository / supply chain

- Secret scanning and push protection through GitHub Security settings where available.
- Dependabot alerts and security updates.
- Dependency lockfiles.
- GitHub Actions pinned to full commit SHAs.
- CodeQL for Python and TypeScript/JavaScript when application code exists.
- Container image scanning when images exist.
- SBOM generation for release artifacts.
- Artifact attestations for release builds where applicable.

### Application

- Python formatting/linting/type checking.
- Python unit and API tests.
- PostgreSQL migration tests.
- RLS cross-user tests.
- TypeScript lint/typecheck/build.
- Frontend security tests.
- Upload security tests.
- OAuth/webhook replay tests.

## CI design rule

Every required check must have a unique job name so GitHub branch protection cannot resolve ambiguous status checks.

## Merge gate

A PR may merge only when all required checks pass and the required review is complete.
