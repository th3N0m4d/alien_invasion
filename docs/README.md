# Documentation

Welcome to the Alien Invasion project documentation!

## 📚 Table of Contents

### Development Documentation

- **[Learning Notes](development/LEARNING_NOTES.md)** - Comprehensive guide covering all topics learned

  - Python errors & fixes
  - Git best practices
  - Virtual environments
  - Testing with pytest
  - Code quality tools
  - CI/CD pipelines

- **[Linter Setup](development/LINTER_SETUP.md)** - Python linting configuration

  - Black, Flake8, Pylint, MyPy setup
  - Configuration files explained
  - Usage examples
  - Common issues

- **[CI/CD Setup](development/CICD_SETUP.md)** - Complete GitHub Actions guide
  - Pipeline overview
  - Workflow configuration
  - YAML syntax
  - Troubleshooting

### Quick Start Guides

- **[CI/CD Quick Start](guides/QUICKSTART_CICD.md)** - Deploy pipeline in 3 steps

  - Fast setup
  - Testing instructions
  - Daily workflow

- **[GitHub vs CircleCI](guides/GITHUB_VS_CIRCLECI.md)** - Platform comparison
  - Detailed analysis
  - Feature comparison
  - Cost breakdown

## 🚀 Quick Links

### For New Contributors

1. Start with [Learning Notes](development/LEARNING_NOTES.md)
2. Set up linters: [Linter Setup](development/LINTER_SETUP.md)
3. Deploy CI/CD: [Quick Start](guides/QUICKSTART_CICD.md)

### For Development

- Check code quality: See [Linter Setup](development/LINTER_SETUP.md)
- Understand pipeline: See [CI/CD Setup](development/CICD_SETUP.md)
- Choose CI platform: See [Comparison](guides/GITHUB_VS_CIRCLECI.md)

## 📖 Documentation Structure

```
docs/
├── README.md                          # This file
├── development/                       # Technical documentation
│   ├── LEARNING_NOTES.md             # Complete learning guide
│   ├── LINTER_SETUP.md               # Linting configuration
│   └── CICD_SETUP.md                 # CI/CD detailed guide
└── guides/                            # Quick reference guides
    ├── QUICKSTART_CICD.md            # Fast CI/CD setup
    └── GITHUB_VS_CIRCLECI.md         # Platform comparison
```

## 🛠️ Project Setup

### Prerequisites

- Python 3.10+
- pip
- Git

### Installation

```bash
# Clone repository
git clone <repo-url>
cd alien_invasion

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # Mac/Linux

# Install dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### Running the Game

```bash
python alien_invasion.py
```

### Development Workflow

```bash
# Format code
black .

# Check style
flake8 .
pylint *.py

# Type check
mypy .

# Run all checks
# (Once Makefile is added)
```

## 📝 Contributing

1. Read [Learning Notes](development/LEARNING_NOTES.md) to understand the project
2. Follow linting guidelines in [Linter Setup](development/LINTER_SETUP.md)
3. Ensure CI/CD pipeline passes (see [CI/CD Setup](development/CICD_SETUP.md))

## 🔗 External Resources

- [Python Official Docs](https://docs.python.org/3/)
- [Pygame Documentation](https://www.pygame.org/docs/)
- [GitHub Actions Docs](https://docs.github.com/actions)
- [PEP 8 Style Guide](https://pep8.org/)

## 📧 Questions?

Check the [Learning Notes](development/LEARNING_NOTES.md) for detailed explanations of common topics.

---

**Last Updated:** December 20, 2025
