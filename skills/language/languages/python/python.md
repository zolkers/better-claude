# Python

## Conventions

Follow PEP 8 strictly. Apply Pythonic idioms -- explicit is better than implicit, simple is better than complex.

### Structure & Complexity

- Max 3 nested statements of any type.
- Max 2 nested `if` statements.
- Max 20 lines per function.
- Max 30 methods per class.
- Max 4 parameters per function -- use a dataclass or dict beyond that.
- Avoid ternary nesting (`a if b else (c if d else e)`) -- one level max.
- Avoid boolean parameters that switch behavior -- use two functions or an enum instead.

### Naming Conventions

- **Variables & functions**: `snake_case`.
- **Classes**: `PascalCase`.
- **Constants**: `UPPER_SNAKE_CASE`.
- **Private**: prefix with `_` (convention), `__` for name mangling (rare).
- **Booleans**: prefix with `is_`, `has_`, `can_`, `should_`.
- **Modules & packages**: short `snake_case`, no hyphens.
- **Naming**: be descriptive, avoid abbreviations, use full words.

### Type Hints

- Use type hints on all function signatures (parameters and return types).
- Use `Optional[T]` or `T | None` (3.10+) for nullable values.
- Use `list[str]`, `dict[str, int]` (3.9+) over `List[str]`, `Dict[str, int]`.
- Use `typing.Protocol` for structural subtyping instead of ABCs when possible.
- Complex types: define `TypeAlias` or `TypeVar` for readability.

```python
def get_user(user_id: int) -> User | None:
    ...

def process_items(items: list[str], *, reverse: bool = False) -> list[str]:
    ...
```

### Imports

- Group imports in order: stdlib, third-party, local -- separated by blank lines.
- Use absolute imports over relative imports.
- Never use `from module import *`.
- Import modules, not individual objects, when the module name adds clarity (`os.path.join` not `from os.path import join`).

### Pythonic Idioms

- Use list/dict/set comprehensions over `map()`/`filter()` when readable.
- Use `enumerate()` instead of manual counter variables.
- Use `zip()` for parallel iteration.
- Use `with` statements for all context managers (files, locks, connections).
- Use `if not items:` instead of `if len(items) == 0:`.
- Use `f-strings` for string formatting -- never `%` or `.format()` for new code.
- Use unpacking: `a, b = func()` instead of `result = func(); a = result[0]`.

### Error Handling

- Catch specific exceptions -- never bare `except:` or `except Exception:`.
- Never leave except blocks empty -- at minimum `logging.exception()`.
- Use custom exceptions inheriting from a project base exception.
- Use `raise ... from e` to preserve exception chains.
- Don't use exceptions for control flow -- use LBYL (look before you leap) or `try/except` only for truly exceptional cases.

### Null Safety

- Never return `None` from a function that returns a collection -- return `[]`, `{}`, etc.
- Use `Optional[T]` in type hints when `None` is a valid return.
- Prefer `value or default` for simple defaults, but be careful with falsy values (0, "", False).

### Strings

- Use f-strings: `f"Hello {name}"`.
- Use `str.join()` for concatenation in loops.
- Use raw strings `r"..."` for regex patterns.
- Use triple quotes `"""..."""` for multiline strings.

### Data Structures

- Use `dataclasses` for data containers instead of plain dicts or tuples.
- Use `NamedTuple` for immutable records.
- Use `enum.Enum` for fixed sets of values -- never magic strings.
- Use `defaultdict`, `Counter`, `deque` from `collections` when they fit.
- Prefer `dict.get(key, default)` over `key in dict` checks.

```python
from dataclasses import dataclass

@dataclass
class User:
    name: str
    email: str
    age: int = 0
```

### Classes & OOP

- Prefer composition over inheritance.
- Use `@property` for computed attributes -- never expose internal state with getters/setters.
- Use `@staticmethod` for methods that don't need `self` or `cls`.
- Use `@classmethod` for alternative constructors.
- Use `__slots__` for classes with many instances to save memory.
- Use `ABC` and `@abstractmethod` for interfaces that must be implemented.

### Immutability

- Use `tuple` over `list` when the collection shouldn't change.
- Use `frozenset` over `set` for immutable sets.
- Use `@dataclass(frozen=True)` for immutable data classes.
- Constants at module level in `UPPER_SNAKE_CASE`.

### Concurrency

- Use `asyncio` for I/O-bound concurrency.
- Use `concurrent.futures.ThreadPoolExecutor` for threading.
- Use `multiprocessing` for CPU-bound parallelism.
- Never share mutable state between threads without locks.
- Prefer `asyncio.gather()` for concurrent async tasks.

### Logging

- Use the `logging` module -- never `print()` in production.
- Configure logging at the application entry point, not in libraries.
- Use `logging.getLogger(__name__)` in each module.
- Use lazy formatting: `logger.info("User %s logged in", user_id)` not f-strings in log calls.

### Testing

- Every public function should be testable in isolation.
- Use `pytest` over `unittest` -- less boilerplate, better output.
- Use fixtures for setup/teardown.
- Use `pytest.raises` for exception testing.
- Use `unittest.mock.patch` or `pytest-mock` for mocking.
- Name test files `test_<module>.py`, test functions `test_<behavior>`.

### Documentation (Docstrings)

- Use **Google Style** or **NumPy Style** docstrings for all public modules, classes, and functions.
- Every function docstring should include `Args:`, `Returns:`, and `Raises:` sections.
- Use triple double quotes `"""..."""`.

```python
def calculate_velocity(distance: float, time: float) -> float:
    """Calculates the average velocity.

    Args:
        distance: The total distance traveled in meters.
        time: The total time taken in seconds.

    Returns:
        The velocity in meters per second.

    Raises:
        ValueError: If time is zero or negative.
    """
    if time <= 0:
        raise ValueError("Time must be positive")
    return distance / time
```

## Questions to Ask

- Do you want code comments ?
- Do you want documentation ?
- Python version target (3.9+, 3.10+, 3.12+)?
- Type hints: strict or optional?
- Linter/formatter: ruff, black, flake8, mypy?
- Package manager: pip, poetry, uv, pipenv?
- Testing framework: pytest, unittest?
- Async required (asyncio, aiohttp)?
- Project type: CLI, web API, library, data pipeline, script?

## Project Structure

Default structure is **src layout** (recommended by PyPA):

```
project/
├── src/
│   └── package_name/
│       ├── __init__.py
│       ├── main.py               # Entry point
│       ├── models/
│       │   └── user.py
│       ├── services/
│       │   └── user_service.py
│       ├── utils/
│       │   └── helpers.py
│       └── config.py
├── tests/
│   ├── conftest.py               # Shared fixtures
│   ├── test_user.py
│   └── test_user_service.py
├── pyproject.toml                # Project metadata & dependencies
├── README.md
└── .gitignore
```

If the project is a **standalone script** (single file, automation, quick tasks):

```
script.py                         # Self-contained script
```

Script conventions:
- Use `if __name__ == "__main__":` as entry point -- never run code at module level.
- Use `argparse` for CLI arguments -- never hardcode inputs or use `sys.argv` directly.
- Use `pathlib.Path` over `os.path` for file operations.
- Use `sys.exit(0)` for success, `sys.exit(1)` for failure -- never bare `exit()`.
- Use shebang `#!/usr/bin/env python3` on the first line for Unix compatibility.
- Group the script into functions even if it's small -- no logic at top level except the `main()` call.
- Use `logging` even in scripts -- configure with `logging.basicConfig()` at the top of `main()`.
- Handle `KeyboardInterrupt` gracefully for long-running scripts.

```python
#!/usr/bin/env python3
"""Brief description of what this script does."""

import argparse
import logging
import sys
from pathlib import Path

logger = logging.getLogger(__name__)


def parse_args() -> argparse.Namespace:
    parser = argparse.ArgumentParser(description=__doc__)
    parser.add_argument("input", type=Path, help="Input file path")
    parser.add_argument("-o", "--output", type=Path, default=None)
    parser.add_argument("-v", "--verbose", action="store_true")
    return parser.parse_args()


def process(input_path: Path, output_path: Path | None) -> None:
    # core logic here
    ...


def main() -> int:
    args = parse_args()
    logging.basicConfig(level=logging.DEBUG if args.verbose else logging.INFO)
    try:
        process(args.input, args.output)
    except KeyboardInterrupt:
        logger.info("Interrupted by user")
        return 1
    except Exception:
        logger.exception("Unexpected error")
        return 1
    return 0


if __name__ == "__main__":
    sys.exit(main())
```

If the user prefers **flat layout** (simple scripts, small projects):

```
project/
├── package_name/
│   ├── __init__.py
│   ├── main.py
│   └── utils.py
├── tests/
├── pyproject.toml
└── README.md
```

If the user prefers **feature-based** (larger applications):

```
project/
├── src/
│   └── package_name/
│       ├── auth/
│       │   ├── __init__.py
│       │   ├── models.py
│       │   ├── service.py
│       │   └── routes.py
│       ├── user/
│       │   ├── __init__.py
│       │   ├── models.py
│       │   ├── service.py
│       │   └── routes.py
│       └── common/
│           ├── config.py
│           ├── exceptions.py
│           └── utils.py
├── tests/
├── pyproject.toml
└── README.md
```
