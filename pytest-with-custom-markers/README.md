# pytest-with-custom-markers

integration test 수행을 위한 `pytest`의 custom option 으로 등록해서 테스트 수행을 편리하게 할 수 있도록 합니다.

- `pytest` 실행시, `@pytest.mark.integration` decorator 가 설정된 test 는 실행되지 않습니다.
- `pytest --integration` 을 실행하는 경우, `@pytest.mark.integration` decorator 가 설정된 테스트를 포함하여 모든 테스트가 실행됩니다.

## Setup 

`pytest.ini` 에 custom marker 를 등록.

```ini
[pytest]
markers =
    integration: mark a test as a integration test.
```

`conftest.py` 에 `pytest_addoption`와 `pytest_runtest_setup`를 설정

```python
import pytest


def pytest_addoption(parser):
    parser.addoption(
        "--integration", action="store_true", help="run integration tests"
    )


def pytest_runtest_setup(item):
    if "integration" in item.keywords and not item.config.getvalue(
            "integration"
    ):
        pytest.skip("need --integration option to run")
```

테스트를 skip 할 곳에 `pytest.mark.integration` decorator 추가.

```python
import pytest

@pytest.mark.integration
def test_integration():
    # some integration tests
```

## Links

<https://docs.pytest.org/en/6.2.x/example/markers.html>
