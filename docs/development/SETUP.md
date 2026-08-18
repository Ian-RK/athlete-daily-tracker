# Local Development Setup

This document establishes the developer workstation before feature development begins.

## 1. Required tools

Install and verify:

- Git
- GitHub CLI (`gh`)
- Docker Desktop using WSL 2 on Windows
- VS Code or another editor
- Node.js LTS for the future PWA
- Python through `uv` for the future FastAPI service

Do not install project dependencies until the repository scaffold is committed.

## 2. Windows checks

Open PowerShell:

```powershell
git --version
gh --version
docker --version
docker compose version
wsl --version
node --version
npm --version
uv --version
```

If WSL is not installed:

```powershell
wsl --install
wsl --update
```

Restart when Windows requests it, then verify:

```powershell
wsl -l -v
```

Docker Desktop should use the WSL 2 engine. Keep the repository inside the WSL Linux filesystem for the Linux-native development workflow used by Docker.

## 3. Git identity

Set the identity used for commits:

```powershell
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR VERIFIED GITHUB EMAIL"
git config --global init.defaultBranch main
git config --global pull.rebase true
git config --global fetch.prune true
```

Do not put tokens or passwords in Git configuration.

## 4. GitHub CLI authentication

```powershell
gh auth login
```

Choose GitHub.com, HTTPS, and the browser authentication flow unless your account/workflow requires SSH.

Verify:

```powershell
gh auth status
```

## 5. Repository creation

Create the GitHub repository as **private** during the foundation phase because the software's purpose is handling sensitive data. The source may become public later only after an explicit privacy/security review.

Suggested repository name:

```text
athlete-daily-tracker
```

Create it through GitHub, then clone it:

```powershell
gh repo clone YOUR_GITHUB_USERNAME/athlete-daily-tracker
cd athlete-daily-tracker
```

## 6. Initial repository commit

Copy the foundation scaffold into the repository. At minimum it should contain:

```text
README.md
SECURITY.md
CONTRIBUTING.md
AGENTS.md
.github/
docs/
apps/
packages/
db/
```

Then:

```powershell
git status
git add .
git commit -m "chore: establish secure repository foundation"
git push -u origin main
```

Do not begin application feature work yet.

## 7. Local data rule

No real athlete data should exist in the repository or test fixtures. The first database fixtures must use synthetic users such as Alice, Bob, and Admin.

## 8. Expected foundation completion state

The repository is ready for feature development only when:

- `main` is protected
- pull requests are required
- required CI checks exist
- secret scanning/push protection is enabled where the chosen GitHub plan supports it
- dependency/security scanning is enabled
- AI worker rules are documented
- the security test strategy exists
- no secrets or real personal data are present
