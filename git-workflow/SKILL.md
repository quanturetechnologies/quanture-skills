---
name: git-workflow
description: Manages Git workflows, branches, commits, pull requests, and GitHub Actions following Quanture standards. Use when creating branches, writing commit messages, reviewing PRs, resolving conflicts, or setting up CI/CD pipelines.
---

# Git Workflow — Quanture Standards

## Branch naming

```
main          — production, always deployable
develop       — integration branch
feature/      — new features: feature/user-auth
fix/          — bug fixes: fix/login-redirect
hotfix/       — urgent prod fixes: hotfix/payment-crash
release/      — release prep: release/v1.2.0
```

## Commit messages (Conventional Commits)

```
feat(auth): implement JWT login endpoint
fix(dashboard): correct date formatting in reports
chore: update dependencies
docs: add API documentation for /items endpoint
refactor(service): extract validation logic
test(auth): add unit tests for token expiry
```

Format: `type(scope): description`
- Body: explain WHY, not WHAT
- Max 72 chars on first line

## Daily workflow

```bash
# Start a feature
git checkout develop
git pull origin develop
git checkout -b feature/my-feature

# Work, commit often
git add -p          # stage hunks selectively
git commit -m "feat(module): add feature"

# Keep branch updated
git fetch origin
git rebase origin/develop

# Push
git push origin feature/my-feature
```

## Pull request checklist

Before opening a PR:
- [ ] Branch is up to date with `develop`
- [ ] All tests pass locally
- [ ] No debug code, no console.log/print left behind
- [ ] No hardcoded credentials or tokens
- [ ] Self-reviewed the diff

PR description must include:
- What was changed and why
- How to test it
- Screenshots (if UI change)

## Security rules

- **Never** commit `.env` files — use `.env.example`
- **Never** force-push to `main` or `develop`
- Use `git secret` or GitHub Secrets for sensitive values
- Enable branch protection on `main`: require PR + passing CI

## Conflict resolution

```bash
git fetch origin
git rebase origin/develop
# resolve conflicts in editor
git add resolved-file.py
git rebase --continue
```

Never use `git merge` during active development — use rebase to keep history clean.

## Tagging releases

```bash
git tag -a v1.2.0 -m "Release v1.2.0 — user auth + dashboard fixes"
git push origin v1.2.0
```

## Reference

- **GitHub Actions CI**: See [reference/github-actions.md](reference/github-actions.md)
- **Git hooks**: See [reference/hooks.md](reference/hooks.md)
