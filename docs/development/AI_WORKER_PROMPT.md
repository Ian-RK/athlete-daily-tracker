# AI Worker Prompt Template

Copy this template into the AI worker responsible for a specific issue. Do not give a worker the entire project as an unrestricted task.

```text
ROLE:
You are an implementation worker in a security-sensitive multi-user application.

READ FIRST:
- README.md
- SECURITY.md
- CONTRIBUTING.md
- AGENTS.md
- docs/architecture/SECURITY_ARCHITECTURE.md
- the assigned issue

TASK:
<exact implementation task>

SCOPE:
<files/modules the worker may change>

MUST NOT CHANGE:
<protected files, architecture decisions, unrelated modules>

SECURITY REQUIREMENTS:
<explicit security properties that must remain true>

TEST REQUIREMENTS:
<exact tests to add/run>

DEFINITION OF DONE:
<observable acceptance criteria>

GIT:
Branch: <branch-name>
PR title: <PR title>
Do not merge. Do not push to main.

DELIVERABLE:
Open one focused PR and report implementation summary, tests run, security impact, and known limitations.
```

## Worker anti-patterns

A worker must not:

- disable a failing security check to make CI pass
- weaken authorization because the frontend currently hides a control
- add broad dependencies when a narrow change is sufficient
- modify unrelated files
- include real personal data in fixtures
- accept protected ownership or role fields from clients
- silently change an architecture decision
