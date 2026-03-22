---
name: python-dev
description: Develops, reviews, and debugs Python code following Quanture standards. Use when writing Python scripts, APIs, data processing pipelines, automation tasks, or any Python-related development work.
---

# Python Development — Quanture Standards

## Quick start

Always activate virtual environment before working:

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

## Code standards

- Python 3.10+ required
- Type hints on all functions
- Black formatter: `black .`
- Linting: `ruff check .`
- Tests: `pytest tests/`

```python
from typing import Optional

def process_data(input: str, limit: Optional[int] = None) -> dict:
    """Brief docstring explaining what this does."""
    # implementation
    ...
```

## Security rules

- **Never** hardcode credentials, tokens, or passwords
- Load secrets from environment variables: `os.environ.get("SECRET_KEY")`
- Validate and sanitize all external inputs
- Use `subprocess.run([...], shell=False)` — never `shell=True` with user input
- Log errors but never log sensitive data

## Project structure

```
project/
├── src/
│   └── module/
├── tests/
├── requirements.txt
├── .env.example       # template without real values
└── README.md
```

## Error handling

```python
try:
    result = risky_operation()
except SpecificError as e:
    logger.error("Operation failed: %s", e)
    raise
```

Never use bare `except:` — always catch specific exceptions.

## Database queries

Use parameterized queries always:

```python
# GOOD
cursor.execute("SELECT * FROM users WHERE id = %s", (user_id,))

# NEVER — SQL injection risk
cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")
```

## Advanced reference

- **APIs**: See [reference/apis.md](reference/apis.md)
- **Testing patterns**: See [reference/testing.md](reference/testing.md)
- **Common utilities**: See [reference/utils.md](reference/utils.md)
