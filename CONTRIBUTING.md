# Contributing to Alien Invasion

Thank you for your interest in contributing! This document provides guidelines for contributing to the project.

## 📋 Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Code Style](#code-style)
- [Development Workflow](#development-workflow)
- [Submitting Changes](#submitting-changes)
- [Documentation](#documentation)

---

## Getting Started

### Prerequisites

- Python 3.10 or higher
- Git
- Basic understanding of Pygame (helpful but not required)

### Resources

Before contributing, familiarize yourself with:

- **[Learning Notes](docs/development/LEARNING_NOTES.md)** - Comprehensive project guide
- **[Linter Setup](docs/development/LINTER_SETUP.md)** - Code quality standards
- **[CI/CD Setup](docs/development/CICD_SETUP.md)** - Automated testing

---

## Development Setup

### 1. Fork and Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR_USERNAME/alien_invasion.git
cd alien_invasion
```

### 2. Create Virtual Environment

```bash
# Create virtual environment
python3 -m venv venv

# Activate it
source venv/bin/activate  # Mac/Linux
# venv\Scripts\activate   # Windows
```

### 3. Install Dependencies

```bash
# Install runtime dependencies
pip install -r requirements.txt

# Install development dependencies
pip install -r requirements-dev.txt
```

### 4. Create a Branch

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Or a bug fix branch
git checkout -b fix/bug-description
```

---

## Code Style

### We Use Automated Linters

All code must pass:

- **Black** - Code formatter
- **Flake8** - Style checker
- **Pylint** - Code analyzer (score ≥ 9.0)
- **MyPy** - Type checker

### Running Linters Locally

```bash
# Format code automatically
black .

# Check style
flake8 .

# Analyze code quality
pylint *.py

# Type check
mypy .
```

### Style Guidelines

1. **Line Length:** Maximum 100 characters
2. **Indentation:** 4 spaces (no tabs)
3. **Quotes:** Use double quotes for strings
4. **Imports:**
   - Standard library first
   - Third-party libraries second
   - Local imports last
   - Alphabetically sorted within groups

```python
# Good
import sys
from typing import Tuple

import pygame

from settings import Settings
from ship import Ship
```

5. **Naming Conventions:**

   - Classes: `PascalCase`
   - Functions/methods: `snake_case`
   - Constants: `UPPER_SNAKE_CASE`
   - Private: `_leading_underscore`

6. **Docstrings:**
   ```python
   def move_ship(dx: int, dy: int) -> None:
       """Move the ship by the given offset.

       Args:
           dx: Horizontal movement delta
           dy: Vertical movement delta
       """
       self.x += dx
       self.y += dy
   ```

---

## Development Workflow

### 1. Write Code

```bash
# Make your changes
vim ship.py
```

### 2. Format and Check

```bash
# Auto-format
black .

# Check style
flake8 .
pylint *.py

# Type check
mypy .
```

### 3. Test Locally

```bash
# Run the game
python alien_invasion.py

# When tests are added:
# pytest
```

### 4. Commit Changes

```bash
# Stage changes
git add .

# Commit with clear message
git commit -m "Add ship movement controls"
```

**Commit Message Guidelines:**

- Use present tense ("Add feature" not "Added feature")
- Use imperative mood ("Move cursor" not "Moves cursor")
- First line ≤ 50 characters
- Detailed description if needed (after blank line)

**Examples:**

```bash
# Good
git commit -m "Add left/right ship movement"

git commit -m "Fix ship boundary detection

- Prevent ship from moving off screen
- Add boundary collision checks
- Update ship position validation"

# Bad
git commit -m "fixed stuff"
git commit -m "WIP"
```

### 5. Push Changes

```bash
# Push to your fork
git push origin feature/your-feature-name
```

---

## Submitting Changes

### Creating a Pull Request

1. **Push your branch** to your fork
2. **Go to the original repository** on GitHub
3. **Click "New Pull Request"**
4. **Select your branch**
5. **Fill out the PR template:**

```markdown
## Description

Brief description of changes

## Type of Change

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing

How did you test these changes?

## Checklist

- [ ] Code follows style guidelines
- [ ] All linters pass
- [ ] Documentation updated
- [ ] Tests added (if applicable)
```

### PR Review Process

1. **Automated checks run** (GitHub Actions)
   - All linters must pass
   - Build must succeed
2. **Code review** by maintainer

   - Code quality
   - Design patterns
   - Documentation

3. **Changes requested** (if needed)

   - Address feedback
   - Push updates to same branch
   - PR updates automatically

4. **Approval and merge**
   - Squash and merge (typically)
   - Delete feature branch

---

## CI/CD Pipeline

All pull requests automatically run:

### Linting Job

- ✅ Black formatting check
- ✅ Flake8 style check
- ✅ Pylint quality check (≥ 9.0)
- ✅ MyPy type check

### Build Job

- ✅ Import verification
- ✅ Syntax validation

**Your PR must pass all checks before merge!**

See [CI/CD Setup](docs/development/CICD_SETUP.md) for details.

---

## Documentation

### When to Update Docs

Update documentation when:

- Adding new features
- Changing existing behavior
- Adding dependencies
- Updating setup process
- Fixing important bugs

### Documentation Structure

```
docs/
├── README.md              # Documentation index
├── development/           # Technical docs
│   ├── LEARNING_NOTES.md # Learning guide
│   ├── LINTER_SETUP.md   # Code quality
│   └── CICD_SETUP.md     # CI/CD guide
└── guides/                # Quick references
    ├── QUICKSTART_CICD.md
    └── GITHUB_VS_CIRCLECI.md
```

### Writing Good Documentation

- Use clear, concise language
- Include code examples
- Keep formatting consistent
- Update table of contents
- Add links between related docs

---

## Questions?

- Check [Learning Notes](docs/development/LEARNING_NOTES.md) for common topics
- Review [Linter Setup](docs/development/LINTER_SETUP.md) for code quality questions
- See [CI/CD Setup](docs/development/CICD_SETUP.md) for pipeline issues

---

## Recognition

Contributors will be acknowledged in the project!

---

**Thank you for contributing!** 🎉
