# Multi-AI Development Workflow

## Operating model

The human maintainer/orchestrator controls architecture, issue scope, prioritization, merge decisions, and security acceptance.

AI workers implement narrowly scoped issues and submit PRs. No AI worker is trusted to merge its own work.

## Work allocation

A task should be decomposed so that workers can operate independently. Good examples:

- `ci`: add Python lint/test workflow
- `ci`: add TypeScript lint/typecheck workflow
- `security`: add authorization test fixtures
- `security`: add RLS policy tests
- `docs`: write database ownership ADR
- `infra`: add local PostgreSQL compose service

Bad example:

- `build the entire backend`

## Branch isolation

Every worker gets a dedicated branch/worktree:

```text
git worktree add ../worker-security sec/security-rls-tests
```

Workers should never share a working directory.

## PR contract

Each worker PR must include:

- issue reference
- implementation summary
- files changed
- tests executed
- security/privacy impact
- known limitations

## Parallelism rules

Parallel workers may work on separate files or independently designed modules. Do not run concurrent workers against the same migration, lockfile, generated artifact, or central configuration file without deliberate coordination.

When two PRs modify the same critical security boundary, merge the first reviewed PR before asking the second worker to rebase.

## Review order

1. CI result
2. authorization/data ownership correctness
3. security-sensitive diff
4. tests
5. maintainability
6. feature behavior

A green build is necessary, not sufficient.

## AI handoff format

The orchestrator should give every worker:

```text
ROLE:
TASK:
FILES/BOUNDARY:
MUST NOT CHANGE:
SECURITY REQUIREMENTS:
TEST REQUIREMENTS:
DEFINITION OF DONE:
PR TITLE:
BRANCH NAME:
```

## Orchestrator rule

The orchestrator should not duplicate implementation work. Its job is to decompose requirements, assign scoped work, resolve conflicts, review security assumptions, and decide what gets merged.
