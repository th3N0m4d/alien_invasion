# Alien Invasion 🚀

A Space Invaders-style game built with Python and Pygame.

## 📖 Documentation

Full documentation is available in the [`docs/`](docs/) directory:

- **[Getting Started](docs/README.md)** - Documentation overview
- **[Learning Notes](docs/development/LEARNING_NOTES.md)** - Complete learning guide
- **[Linter Setup](docs/development/LINTER_SETUP.md)** - Code quality configuration
- **[CI/CD Setup](docs/development/CICD_SETUP.md)** - GitHub Actions pipeline

## 🎮 Quick Start

### Prerequisites

- Python 3.10 or higher
- pip (Python package manager)

### Installation

```bash
# Clone the repository
git clone <your-repo-url>
cd alien_invasion

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Mac/Linux
# venv\Scripts\activate   # On Windows

# Install dependencies
pip install -r requirements.txt
```

### Run the Game

```bash
python alien_invasion.py
```

## 🛠️ Development

### Install Development Dependencies

```bash
pip install -r requirements-dev.txt
```

### Code Quality

```bash
# Format code
black .

# Check style
flake8 .

# Analyze code
pylint *.py

# Type check
mypy .
```

### Project Structure

```
alien_invasion/
├── docs/                  # Documentation
│   ├── development/      # Technical docs
│   └── guides/           # Quick guides
├── .github/workflows/    # CI/CD pipelines
├── images/               # Game assets
├── alien_invasion.py     # Main game file
├── ship.py              # Ship class
├── settings.py          # Game settings
└── README.md            # This file
```

## 🤝 Contributing

See our [Contributing Guide](CONTRIBUTING.md) for details on:

- Code style guidelines
- Development workflow
- How to submit changes

## 📋 Features

- [x] Ship rendering and positioning
- [x] Screen setup and configuration
- [ ] Ship movement
- [ ] Bullets
- [ ] Alien fleet
- [ ] Collision detection
- [ ] Scoring system
- [ ] Game states (start, play, game over)

## 🧪 Testing

Testing setup coming soon!

## 📄 License

This project is part of a Python learning journey.

## 🙏 Acknowledgments

Built following Python Crash Course principles.
