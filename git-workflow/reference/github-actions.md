# GitHub Actions CI Reference

## Basic CI pipeline

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"
          cache: pip

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Lint
        run: ruff check .

      - name: Test
        run: pytest tests/ --cov=src
        env:
          DB_HOST: localhost
          SECRET_KEY: ${{ secrets.SECRET_KEY }}
```

## Secrets — never hardcode

```yaml
env:
  DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # auto-provided
```

Add secrets in: GitHub repo → Settings → Secrets and variables → Actions

## Deploy on merge to main

```yaml
deploy:
  needs: test
  runs-on: ubuntu-latest
  if: github.ref == 'refs/heads/main'
  steps:
    - name: Deploy
      run: ./scripts/deploy.sh
      env:
        DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
```

## Cache dependencies (faster runs)

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: ${{ runner.os }}-pip-${{ hashFiles('requirements.txt') }}
```
