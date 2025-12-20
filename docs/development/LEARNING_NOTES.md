# Python Game Development & CI/CD Learning Notes

**Project:** Alien Invasion Game  
**Date:** December 20, 2025  
**Topics Covered:** Python project setup, testing, linting, CI/CD pipelines

---

## Table of Contents

1. [Common Python Errors & Fixes](#common-python-errors--fixes)
2. [Git Best Practices](#git-best-practices)
3. [Python Module System](#python-module-system)
4. [Virtual Environments](#virtual-environments)
5. [Dependency Management](#dependency-management)
6. [Testing with Pytest](#testing-with-pytest)
7. [Code Quality Tools](#code-quality-tools)
8. [Configuration Files](#configuration-files)
9. [CI/CD Pipeline](#cicd-pipeline)
10. [Project Automation](#project-automation)

---

## Common Python Errors & Fixes

### 1. Pygame Display Dimension Swap

**Error:** Screen created as 800x1200 instead of 1200x800

**Problem:**

```python
# WRONG - dimensions swapped
self.screen = pygame.display.set_mode(
    (self.settings.screen_height, self.settings.screen_width)
)
```

**Fix:**

```python
# CORRECT - (width, height)
self.screen = pygame.display.set_mode(
    (self.settings.screen_width, self.settings.screen_height)
)
```

**Remember:** `pygame.display.set_mode()` expects `(width, height)`

---

### 2. Initialization Order Issues

**Error:** AttributeError when Ship tries to access screen before it's created

**Problem:**

```python
def __init__(self):
    pygame.init()
    self.settings = Settings()
    self.ship = Ship(self)  # ❌ self.screen doesn't exist yet
    self.screen = pygame.display.set_mode(...)
```

**Fix:**

```python
def __init__(self):
    pygame.init()
    self.settings = Settings()
    self.screen = pygame.display.set_mode(...)  # Create screen first
    self.ship = Ship(self)  # ✅ Now ship can access self.screen
```

**Remember:** Initialize dependencies before objects that need them

---

## Git Best Practices

### What NOT to Commit

**Never commit these files:**

- `*.pyc` - Compiled Python bytecode
- `__pycache__/` - Cache directories
- `venv/`, `.venv/` - Virtual environments
- `.DS_Store` - macOS system files
- `.idea/`, `.vscode/` - IDE settings (usually)

**Why?**

- Generated automatically
- Platform-specific
- Bloat repository
- Can be recreated from source

### Recommended .gitignore

```gitignore
# Byte-compiled / optimized files
__pycache__/
*.py[cod]
*$py.class

# Virtual environments
venv/
ENV/
env/
.venv/

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db

# Distribution
build/
dist/
*.egg-info/
```

### Remove Already Committed Files

```bash
# Remove from git but keep locally
git rm -r --cached __pycache__/
git commit -m "Remove cached .pyc files"
```

---

## Python Module System

### Standard vs Non-Standard Locations

**Standard Locations** (automatic import):

1. Current directory
2. Python standard library (e.g., `sys`, `os`, `json`)
3. `site-packages` (pip installed packages)
4. `PYTHONPATH` environment variable

**Non-Standard Locations** (require manual setup):

```python
from sys import path
path.append('../packages')  # Add custom location
import extra.iota  # Now Python can find it
```

### Package Structure

**With `__init__.py`:**

```
myproject/
├── main.py
└── utils/
    ├── __init__.py     # Makes it a package
    ├── helpers.py
    └── config.py
```

**Python 3.3+:** `__init__.py` is optional but recommended

---

### The `__init__.py` File

**Purpose:**

1. Marks directory as a Python package
2. Runs initialization code
3. Controls exports with `__all__`
4. Better IDE support

**Example with `__all__`:**

```python
# utils/__init__.py
from .helpers import public_func
from .config import settings

__all__ = ['public_func', 'settings']  # Only these exported with *
```

**Usage:**

```python
from utils import *  # Only imports public_func and settings
from utils import anything  # Explicit imports always work
```

**Best Practice:** Always create `__init__.py` (even if empty) for clarity

---

## Virtual Environments

### What is a Virtual Environment?

An isolated Python workspace that keeps project dependencies separate.

**Without virtual environment (PROBLEM):**

```
System Python
├── pygame 2.5.0 (alien_invasion)
├── pygame 1.9.0 (old_game) ❌ CONFLICT!
└── 100+ packages from all projects
```

**With virtual environments (SOLUTION):**

```
System Python
├── venv_alien_invasion/
│   └── pygame 2.5.0 ✅
└── venv_old_game/
    └── pygame 1.9.0 ✅
```

### Benefits

✅ **Isolation** - Each project has own packages  
✅ **Version control** - Different versions per project  
✅ **Clean** - Don't pollute system Python  
✅ **Reproducible** - Easy to recreate environment  
✅ **No permissions** - No sudo required

### Complete Workflow

```bash
# 1. Create virtual environment (once per project)
python3 -m venv venv

# 2. Activate (every time you work on project)
source venv/bin/activate  # Mac/Linux
# venv\Scripts\activate   # Windows

# 3. Verify activation
which python  # Should point to venv/bin/python
# Prompt shows: (venv) $

# 4. Install dependencies
pip install -r requirements.txt

# 5. Work on project
python alien_invasion.py

# 6. Deactivate when done
deactivate
```

### Important Notes

- `venv/` folder should be in `.gitignore`
- Always activate before working
- Each project gets its own venv
- Terminal prompt shows `(venv)` when active

---

## Dependency Management

### requirements.txt (Production)

**Format:**

```txt
package_name>=version    # Version or newer
package_name==version    # Exact version
package_name             # Latest version
```

**Example:**

```txt
pygame>=2.5.0
requests==2.31.0
pillow
```

**Install:**

```bash
pip install -r requirements.txt
```

### requirements-dev.txt (Development)

**Purpose:** Tools needed for development but not production

**Example:**

```txt
# Testing
pytest>=7.4.0
pytest-cov>=4.1.0

# Linting
pylint>=3.0.0
flake8>=6.1.0
black>=23.12.0

# Type checking
mypy>=1.7.0
```

**Install:**

```bash
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### Generate requirements.txt

```bash
# From current environment
pip freeze > requirements.txt

# Better: manually curate for only direct dependencies
```

---

## Testing with Pytest

### Why Test?

✅ Catch bugs early  
✅ Ensure code works as expected  
✅ Safe refactoring  
✅ Documentation through examples  
✅ Confidence in changes

### Basic Test Structure

```python
import pytest
from settings import Settings

class TestSettings:
    """Group related tests in a class."""

    def test_settings_initialization(self):
        """Test description - what you're testing."""
        # Arrange - setup
        settings = Settings()

        # Act - execute (if needed)
        width = settings.screen_width

        # Assert - verify
        assert width == 1200
        assert settings.screen_height == 800
```

### Test Discovery

Pytest automatically finds:

- Files: `test_*.py` or `*_test.py`
- Classes: `Test*`
- Functions: `test_*`

**Example:**

```
tests/
├── __init__.py
├── test_settings.py    ✅ Found
├── test_ship.py        ✅ Found
└── ship_tests.py       ❌ Not found (wrong pattern)
```

### Fixtures (Reusable Setup)

```python
@pytest.fixture
def game():
    """Create a mock game instance."""
    return MockGame()

def test_ship_initialization(game):
    """Pytest automatically passes fixture."""
    ship = Ship(game)
    assert ship.screen == game.screen
```

**Benefits:**

- Reusable setup code
- Fresh instance per test
- Automatic cleanup
- Clear dependencies

### Assertions

```python
# Equality
assert value == expected
assert value != unexpected

# Boolean
assert condition
assert not condition

# Membership
assert item in collection
assert item not in collection

# Comparisons
assert value > 0
assert value <= 100

# Type checking
assert isinstance(obj, ClassName)

# Exceptions
with pytest.raises(ValueError):
    function_that_raises()
```

### Running Tests

```bash
# Run all tests
pytest

# Run specific file
pytest tests/test_ship.py

# Run specific test
pytest tests/test_ship.py::TestShip::test_ship_initialization

# With coverage
pytest --cov=.

# Verbose output
pytest -v

# Stop on first failure
pytest -x
```

### Mock Objects

**Purpose:** Fake objects for testing without real dependencies

```python
class MockGame:
    """Fake game for testing Ship."""
    def __init__(self):
        pygame.init()
        self.screen = pygame.display.set_mode((800, 600))

def test_ship(self):
    mock_game = MockGame()
    ship = Ship(mock_game)  # Test with fake game
    assert ship.screen == mock_game.screen
```

**Benefits:**

- Faster tests (no real game initialization)
- Isolated tests (focus on one component)
- Control test conditions

---

## Code Quality Tools

### Overview

| Tool       | Purpose                | Speed  | Auto-fix |
| ---------- | ---------------------- | ------ | -------- |
| **Black**  | Code formatting        | Fast   | ✅ Yes   |
| **Flake8** | Style checker          | Fast   | ❌ No    |
| **Pylint** | Comprehensive analyzer | Slow   | ❌ No    |
| **MyPy**   | Type checker           | Medium | ❌ No    |

### Black - Code Formatter

**What it does:**

- Auto-formats code to consistent style
- No configuration needed
- "Opinionated" - one way to format

**Usage:**

```bash
# Check formatting
black --check .

# Auto-format
black .

# Format specific file
black ship.py
```

**Example:**

```python
# Before
def move_ship(  x,y,  speed ):
    return x+speed,y

# After (Black formatted)
def move_ship(x, y, speed):
    return x + speed, y
```

### Flake8 - Style Checker

**What it does:**

- Checks PEP 8 style guide
- Finds syntax errors
- Reports code smells
- Fast and focused

**Common errors:**

- `E501` - Line too long
- `F401` - Unused import
- `F841` - Unused variable
- `W291` - Trailing whitespace

**Usage:**

```bash
flake8 .
flake8 ship.py --show-source
```

### Pylint - Comprehensive Analyzer

**What it does:**

- Code quality analysis
- Style checking
- Bug detection
- Complexity measurement
- Generates quality score (0-10)

**Message types:**

- `C` - Convention (style)
- `R` - Refactor (structure)
- `W` - Warning (potential bug)
- `E` - Error (likely bug)
- `F` - Fatal (prevents running)

**Usage:**

```bash
pylint *.py
pylint ship.py --fail-under=8.0
```

**Common messages:**

- `C0111` - Missing docstring
- `R0903` - Too few public methods
- `W0612` - Unused variable
- `E1101` - No member (attribute doesn't exist)

### MyPy - Type Checker

**What it does:**

- Checks type hints
- Catches type-related bugs
- Optional gradual typing

**Example:**

```python
# Without type hints
def move(x, y):
    return x + y

move(10, "20")  # ❌ Runtime error, MyPy can't catch

# With type hints
def move(x: int, y: int) -> int:
    return x + y

move(10, "20")  # ❌ MyPy catches this!
```

**Usage:**

```bash
mypy .
mypy ship.py --strict
```

---

## Configuration Files

### File Format Overview

| Format   | Extension            | Used By                | Syntax                     |
| -------- | -------------------- | ---------------------- | -------------------------- |
| **INI**  | `.ini`, `.cfg`, `rc` | Pylint, Flake8, Pytest | `[section]` `key=value`    |
| **TOML** | `.toml`              | Black, MyPy, Poetry    | `[section]` `key = value`  |
| **YAML** | `.yml`, `.yaml`      | GitHub Actions         | `key: value` (indentation) |

### pytest.ini

```ini
[pytest]
testpaths = tests               # Where to find tests
python_files = test_*.py        # Test file pattern
python_classes = Test*          # Test class pattern
python_functions = test_*       # Test function pattern
addopts =
    --verbose                   # Detailed output
    --cov=.                     # Measure coverage
    --cov-report=html           # HTML coverage report
    --cov-report=term-missing   # Show missing lines
```

### .pylintrc

```ini
[MASTER]
ignore=tests,venv,.venv         # Skip these folders

[MESSAGES CONTROL]
disable=
    C0111,                      # missing-docstring
    R0903,                      # too-few-public-methods

[FORMAT]
max-line-length=100             # Max chars per line

[BASIC]
good-names=i,j,k,x,y,dx,dy     # Allow short names

[DESIGN]
max-args=5                      # Max function parameters
max-locals=15                   # Max local variables
```

### .flake8

```ini
[flake8]
max-line-length = 100           # Max chars per line
exclude =
    .git,
    __pycache__,
    venv,
    .venv,
    tests
ignore = E203, W503             # Ignore these errors

# Per-file ignores
per-file-ignores =
    __init__.py:F401            # Allow unused imports in __init__.py
```

### pyproject.toml

```toml
[tool.black]
line-length = 100
target-version = ['py314']      # Python version
exclude = '''
/(
    \.git
  | \.venv
  | __pycache__
)/
'''

[tool.mypy]
python_version = "3.14"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = false   # Type hints optional
ignore_missing_imports = true   # Don't error on missing stubs

[[tool.mypy.overrides]]
module = "pygame.*"
ignore_missing_imports = true   # Ignore pygame type issues
```

### .gitignore

```gitignore
# Python
__pycache__/
*.py[cod]
*$py.class

# Virtual environments
venv/
.venv/
ENV/

# IDE
.vscode/
.idea/

# OS
.DS_Store

# Testing
.pytest_cache/
htmlcov/
.coverage

# Distribution
build/
dist/
*.egg-info/
```

---

## CI/CD Pipeline

### What is CI/CD?

**CI (Continuous Integration):**

- Automatically test code on every commit
- Catch bugs early
- Ensure code quality

**CD (Continuous Deployment):**

- Automatically deploy passing code
- Faster releases
- Consistent process

### GitHub Actions Workflow

**File:** `.github/workflows/ci.yml`

```yaml
name: CI Pipeline

# When to run
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

# What to run
jobs:
  lint:
    name: Linting
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.14"

      - name: Install dependencies
        run: |
          pip install -r requirements-dev.txt

      - name: Run Black
        run: black --check .

      - name: Run Flake8
        run: flake8 .

      - name: Run Pylint
        run: pylint *.py --fail-under=8.0

  test:
    name: Testing
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.14"

      - name: Install system dependencies
        run: |
          sudo apt-get update
          sudo apt-get install -y python3-pygame xvfb

      - name: Install Python dependencies
        run: |
          pip install -r requirements.txt
          pip install -r requirements-dev.txt

      - name: Run tests
        run: |
          xvfb-run -a pytest

  build:
    name: Build Check
    runs-on: ubuntu-latest
    needs: [lint, test]
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.14"

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Check imports
        run: |
          python -c "import alien_invasion"
```

### YAML Syntax Basics

```yaml
# Comments start with #
key: value                  # String value
number: 42                  # Number
boolean: true               # Boolean (true/false)

# Lists
items:
  - first
  - second
  - third

# Or inline
items: [first, second, third]

# Nested objects
person:
  name: John
  age: 30

# Multi-line strings
script: |
  line 1
  line 2
  line 3
```

### Workflow Breakdown

**Triggers:**

```yaml
on:
  push: # On push to...
    branches: [main] # ...main branch
  pull_request: # On PR to...
    branches: [main] # ...main branch
```

**Jobs:**

```yaml
jobs:
  job_name:
    runs-on: ubuntu-latest # VM to run on
    needs: [other_job] # Wait for other jobs
    steps: # Sequential steps
      - name: Step name
        run: command
```

**Steps:**

- `uses:` - Use pre-built action
- `run:` - Run shell command
- `with:` - Pass parameters
- `name:` - Description

### Pipeline Flow

```
Push to GitHub
    ↓
GitHub Actions triggers
    ↓
┌─────────────────┐
│   Lint Job      │  ← Check code style
│  - Black        │
│  - Flake8       │
│  - Pylint       │
└─────────────────┘
    ↓
┌─────────────────┐
│   Test Job      │  ← Run tests
│  - pytest       │
│  - coverage     │
└─────────────────┘
    ↓
┌─────────────────┐
│   Build Job     │  ← Verify imports
│  - Check import │
└─────────────────┘
    ↓
All pass? ✅ → Merge allowed
Any fail? ❌ → Fix required
```

---

## Project Automation

### Makefile

**Purpose:** Define shortcuts for common commands

**Basic syntax:**

```makefile
.PHONY: target_name

target_name:
	command 1
	command 2
```

**⚠️ IMPORTANT:** Must use TAB (not spaces) for indentation!

**Example Makefile:**

```makefile
.PHONY: install test lint format clean run all

install:
	pip install -r requirements.txt
	pip install -r requirements-dev.txt

test:
	pytest

test-coverage:
	pytest --cov=. --cov-report=html --cov-report=term

lint:
	flake8 .
	pylint *.py

format:
	black .

type-check:
	mypy .

clean:
	find . -type d -name __pycache__ -exec rm -rf {} +
	find . -type f -name '*.pyc' -delete
	rm -rf .pytest_cache
	rm -rf htmlcov
	rm -rf .coverage

run:
	python alien_invasion.py

all: clean install lint test
```

**Usage:**

```bash
make install        # Install dependencies
make test           # Run tests
make lint           # Check code quality
make format         # Auto-format code
make clean          # Remove generated files
make run            # Run the game
make all            # Full pipeline
```

### Development Workflow

```bash
# 1. Initial setup (once)
git clone <repo>
cd alien_invasion
python3 -m venv venv
source venv/bin/activate
make install

# 2. Daily workflow
source venv/bin/activate    # Activate environment
git checkout -b feature     # Create feature branch

# 3. Write code
# Edit files...

# 4. Format and check
make format                 # Auto-format with Black
make lint                   # Check style
make test                   # Run tests

# 5. Fix issues and repeat
# Fix any errors...
make lint
make test

# 6. Commit and push
git add .
git commit -m "Add feature"
git push origin feature

# 7. Create PR on GitHub
# Pipeline runs automatically
# Merge when all checks pass ✅
```

---

## Quick Reference Commands

### Virtual Environment

```bash
python3 -m venv venv                    # Create
source venv/bin/activate                # Activate (Mac/Linux)
venv\Scripts\activate                   # Activate (Windows)
deactivate                              # Deactivate
```

### Package Management

```bash
pip install package                     # Install package
pip install -r requirements.txt         # Install from file
pip freeze > requirements.txt           # Generate requirements
pip list                                # List installed packages
pip show package                        # Package details
```

### Testing

```bash
pytest                                  # Run all tests
pytest tests/test_ship.py              # Run specific file
pytest -v                              # Verbose output
pytest --cov=.                         # With coverage
pytest -x                              # Stop on first failure
```

### Linting

```bash
black .                                # Auto-format
black --check .                        # Check only
flake8 .                              # Style check
pylint *.py                           # Full analysis
mypy .                                # Type check
```

### Git

```bash
git status                             # Check status
git add .                              # Stage all changes
git commit -m "message"                # Commit
git push origin branch                 # Push to remote
git pull                               # Pull changes
git checkout -b feature                # Create branch
```

### Make Commands

```bash
make install                           # Install dependencies
make test                              # Run tests
make lint                              # Check code
make format                            # Format code
make clean                             # Clean generated files
make run                               # Run application
make all                               # Full pipeline
```

---

## Best Practices Summary

### Code Quality

✅ Always use virtual environments  
✅ Format code with Black  
✅ Check with Flake8 and Pylint  
✅ Write tests for new features  
✅ Run tests before committing  
✅ Keep requirements.txt updated

### Git

✅ Use meaningful commit messages  
✅ Don't commit generated files  
✅ Create `.gitignore` early  
✅ Use feature branches  
✅ Review code before merging

### Testing

✅ Test one thing per test  
✅ Use descriptive test names  
✅ Use fixtures for setup  
✅ Mock external dependencies  
✅ Aim for >80% coverage

### CI/CD

✅ Run linting before tests  
✅ Test on multiple Python versions  
✅ Block merges on failures  
✅ Keep pipelines fast (<5 min)  
✅ Cache dependencies

---

## Common Issues & Solutions

### Issue: Import errors in tests

**Solution:** Make sure test files are in `tests/` directory with `__init__.py`

### Issue: Tests pass locally but fail in CI

**Solution:** Check Python version, missing system dependencies, or file paths

### Issue: Linters disagree

**Solution:** Configure tools consistently (same line length, ignore conflicting rules)

### Issue: Black and Flake8 conflict

**Solution:** Ignore E203 and W503 in Flake8 config

### Issue: Pygame tests fail in CI

**Solution:** Use `xvfb-run` for headless display

### Issue: Virtual environment activated but pip installs globally

**Solution:** Deactivate and reactivate, check `which pip`

---

## Resources

### Documentation

- [Python Official Docs](https://docs.python.org/3/)
- [Pygame Docs](https://www.pygame.org/docs/)
- [Pytest Docs](https://docs.pytest.org/)
- [Black Docs](https://black.readthedocs.io/)
- [GitHub Actions Docs](https://docs.github.com/actions)

### Style Guides

- [PEP 8 - Style Guide](https://pep8.org/)
- [PEP 257 - Docstring Conventions](https://www.python.org/dev/peps/pep-0257/)

### Tools

- [Real Python - Testing](https://realpython.com/pytest-python-testing/)
- [Pylint Messages](https://pylint.pycqa.org/en/latest/messages/messages_list.html)
- [Flake8 Error Codes](https://flake8.pycqa.org/en/latest/user/error-codes.html)

---

## Next Steps

1. ✅ Set up virtual environment
2. ✅ Create requirements files
3. ✅ Add linting configuration
4. ⬜ Write comprehensive tests
5. ⬜ Set up GitHub Actions pipeline
6. ⬜ Add pre-commit hooks
7. ⬜ Implement continuous deployment

---

**Last Updated:** December 20, 2025  
**Project:** Alien Invasion Game  
**Python Version:** 3.14
