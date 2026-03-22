# Python Testing Patterns

## Structure

```
tests/
├── conftest.py        # shared fixtures
├── test_unit/
│   └── test_module.py
└── test_integration/
    └── test_api.py
```

## Basic test

```python
import pytest

def test_process_data_returns_dict():
    result = process_data("input")
    assert isinstance(result, dict)
    assert "key" in result

def test_process_data_raises_on_invalid():
    with pytest.raises(ValueError):
        process_data(None)
```

## Fixtures

```python
@pytest.fixture
def sample_config():
    return {"host": "localhost", "port": 5432}

def test_with_config(sample_config):
    assert sample_config["port"] == 5432
```

## Mocking secrets

```python
from unittest.mock import patch

def test_with_env():
    with patch.dict("os.environ", {"SECRET_KEY": "test_key"}):
        result = function_using_env()
        assert result is not None
```

Never use real credentials in tests.

## Run

```bash
pytest tests/ -v --cov=src --cov-report=term-missing
```
