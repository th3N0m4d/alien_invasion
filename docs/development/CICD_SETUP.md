# GitHub Actions CI/CD Setup Guide

**Project:** Alien Invasion Game  
**CI/CD Platform:** GitHub Actions  
**Date:** December 20, 2025

---

## Why GitHub Actions?

### ✅ Chosen: GitHub Actions

| Feature            | GitHub Actions        | CircleCI         |
| ------------------ | --------------------- | ---------------- |
| **Integration**    | Native to GitHub      | External service |
| **Setup**          | 1 YAML file           | Account + config |
| **Free Tier**      | 2,000 min/month       | 6,000 min/month  |
| **Public Repos**   | Unlimited             | Unlimited        |
| **Learning Curve** | Easy                  | Medium           |
| **Maintenance**    | Low                   | Medium           |
| **Best For**       | Small-medium projects | Enterprise       |

**Decision:** GitHub Actions is perfect for your project because:

- ✅ No additional account needed
- ✅ Simpler setup and debugging
- ✅ Direct integration with GitHub
- ✅ Free for public repos
- ✅ Large community and marketplace

---

## Pipeline Overview

### Main Pipeline (`.github/workflows/ci.yml`)

Runs on every push and pull request:

```
Push Code → GitHub
     ↓
Run Linting Job
     ├─ Checkout code
     ├─ Setup Python 3.14
     ├─ Install dependencies
     ├─ Black (formatting)
     ├─ Flake8 (style)
     ├─ Pylint (quality)
     └─ MyPy (types)
     ↓
Run Build Job (if linting passes)
     ├─ Verify imports
     └─ Check project structure
     ↓
✅ All Passed → Merge allowed
❌ Any Failed → Fix required
```

---

## Workflow Files

### 1. `ci.yml` - Main Pipeline

**Location:** `.github/workflows/ci.yml`

**Triggers:**

- Push to `main` or `develop` branches
- Pull requests to `main` or `develop`

**Jobs:**

1. **lint** - Runs all linters
2. **build** - Verifies imports (runs only if lint passes)

**Key Features:**

```yaml
# Caches pip dependencies for faster runs
cache: 'pip'

# Fails if any linter fails
continue-on-error: false

# Requires Pylint score ≥ 9.0
pylint *.py --fail-under=9.0

# Jobs run sequentially
needs: lint  # build waits for lint
```

---

### 2. `python-versions.yml` - Compatibility Check

**Location:** `.github/workflows/python-versions.yml`

**Triggers:**

- Weekly (every Sunday)
- Manual trigger via GitHub UI

**Purpose:**
Tests your code against multiple Python versions:

- Python 3.10
- Python 3.11
- Python 3.12
- Python 3.13
- Python 3.14

**Matrix Strategy:**
Runs 5 jobs in parallel, one per Python version.

---

## How to Enable

### Step 1: Push Workflows to GitHub

```bash
# Make sure you're in your project
cd /Users/I547821/Documents/Development/alien_invasion

# Add workflow files
git add .github/workflows/

# Commit
git commit -m "Add GitHub Actions CI/CD pipeline"

# Push to GitHub
git push origin main
```

### Step 2: Verify in GitHub

1. Go to your GitHub repository
2. Click **"Actions"** tab
3. You should see workflows running
4. Click on a workflow to see details

---

## Understanding the Pipeline

### YAML Syntax Breakdown

```yaml
# Workflow name (appears in GitHub UI)
name: Python Linting Pipeline

# When to run
on:
  push: # On push events
    branches: [main] # Only these branches
  pull_request: # On PRs
    branches: [main]

# Jobs to execute
jobs:
  job_name:
    name: Display Name
    runs-on: ubuntu-latest # VM to use

    steps:
      # Use a pre-built action
      - name: Step name
        uses: actions/checkout@v4

      # Run shell commands
      - name: Install deps
        run: |
          pip install package
          python script.py

      # Conditional execution
      - name: Success message
        if: success()
        run: echo "Done!"
```

---

## Key GitHub Actions Concepts

### 1. Triggers (`on:`)

```yaml
# Push to specific branches
on:
  push:
    branches: [ main, develop ]

# Pull requests
on:
  pull_request:
    branches: [ main ]

# Scheduled (cron)
on:
  schedule:
    - cron: '0 0 * * 0'  # Every Sunday

# Manual trigger
on:
  workflow_dispatch:

# Multiple triggers
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 1'  # Every Monday
```

---

### 2. Jobs

```yaml
jobs:
  # Job 1
  lint:
    name: Linting
    runs-on: ubuntu-latest
    steps:
      - run: echo "Linting..."

  # Job 2 (waits for Job 1)
  build:
    name: Build
    runs-on: ubuntu-latest
    needs: lint # Dependency
    steps:
      - run: echo "Building..."
```

**Job dependencies:**

- Jobs run in **parallel** by default
- Use `needs:` to create dependencies
- Failed job stops dependent jobs

---

### 3. Steps

```yaml
steps:
  # Use pre-built action
  - name: Checkout code
    uses: actions/checkout@v4

  # Run single command
  - name: Install package
    run: pip install pygame

  # Run multiple commands
  - name: Setup
    run: |
      pip install package1
      pip install package2
      python setup.py

  # Conditional step
  - name: On success
    if: success()
    run: echo "Passed!"

  # Continue on error
  - name: Optional check
    run: some-command
    continue-on-error: true
```

---

### 4. Matrix Strategy

Run same job with different configurations:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.10", "3.11", "3.12"]

    steps:
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Test Python ${{ matrix.python-version }}
        run: python --version
```

**Result:** 3 jobs run in parallel (one per version)

---

### 5. Caching

Speed up workflows by caching dependencies:

```yaml
- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: "3.14"
    cache: "pip" # Cache pip packages
```

**Benefits:**

- Faster workflow runs
- Reduced package downloads
- Lower bandwidth usage

**First run:** ~2-3 minutes (installs everything)  
**Cached runs:** ~30 seconds (uses cache)

---

## Viewing Pipeline Results

### In GitHub UI

1. **Repository → Actions tab**

   - See all workflow runs
   - Filter by workflow, branch, status

2. **Click a workflow run**

   - See all jobs
   - View logs for each step
   - Download artifacts

3. **On Pull Requests**
   - See checks at bottom of PR
   - Red ❌ = failed
   - Green ✅ = passed
   - Yellow ⚠️ = running

---

## Status Badges

Add a badge to your README to show build status:

```markdown
# Alien Invasion

![CI](https://github.com/YOUR_USERNAME/alien_invasion/workflows/Python%20Linting%20Pipeline/badge.svg)

A space invaders-style game built with Pygame.
```

**Result:**  
![CI](https://img.shields.io/badge/build-passing-brightgreen)

---

## Common Pipeline Scenarios

### Scenario 1: Push to Main Branch

```
Developer pushes code
     ↓
GitHub Actions triggered
     ↓
Runs lint job:
  ├─ Black ✅
  ├─ Flake8 ✅
  ├─ Pylint ✅
  └─ MyPy ✅
     ↓
Runs build job:
  └─ Import check ✅
     ↓
✅ All passed - code is good!
```

---

### Scenario 2: Pull Request with Errors

```
Developer creates PR
     ↓
GitHub Actions triggered
     ↓
Runs lint job:
  ├─ Black ✅
  ├─ Flake8 ❌ (unused import found)
  └─ Stops here
     ↓
❌ Checks failed - cannot merge
     ↓
Developer fixes issue
     ↓
Pipeline re-runs automatically
     ↓
✅ Now passes - can merge
```

---

### Scenario 3: Weekly Compatibility Check

```
Sunday at midnight (UTC)
     ↓
Scheduled workflow runs
     ↓
Tests 5 Python versions in parallel:
  ├─ Python 3.10 ✅
  ├─ Python 3.11 ✅
  ├─ Python 3.12 ✅
  ├─ Python 3.13 ✅
  └─ Python 3.14 ✅
     ↓
✅ Compatible with all versions!
```

---

## Troubleshooting

### Issue: Workflow doesn't run

**Check:**

- Workflow file is in `.github/workflows/`
- File has `.yml` or `.yaml` extension
- YAML syntax is valid
- Branch name matches trigger

---

### Issue: Linter fails in CI but passes locally

**Causes:**

1. Different Python version
2. Different dependency versions
3. Missing files in git

**Solution:**

```bash
# Check Python version
python --version

# Verify dependencies match
pip freeze > requirements.txt

# Ensure all files committed
git status
```

---

### Issue: Pipeline is slow

**Solutions:**

1. Enable pip caching (already done)
2. Reduce matrix size
3. Run fewer linters
4. Use faster runners (paid)

**Current setup:**

- ✅ Pip caching enabled
- ✅ Only essential linters
- Typical run time: 1-2 minutes

---

## Advanced Features (Future)

### 1. Pre-commit Hooks

Run linters before git commit:

```bash
# Install pre-commit
pip install pre-commit

# Create .pre-commit-config.yaml
# Auto-runs Black, Flake8 on commit
```

---

### 2. Code Coverage Reports

Track test coverage:

```yaml
- name: Generate coverage
  run: pytest --cov --cov-report=xml

- name: Upload to Codecov
  uses: codecov/codecov-action@v3
```

---

### 3. Deployment

Auto-deploy when tests pass:

```yaml
- name: Deploy to production
  if: github.ref == 'refs/heads/main'
  run: ./deploy.sh
```

---

### 4. Notifications

Send alerts to Slack/Discord:

```yaml
- name: Notify on failure
  if: failure()
  uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
```

---

## Cost Comparison

### GitHub Actions

**Free Tier (Public Repos):**

- ✅ Unlimited minutes
- ✅ Unlimited storage

**Free Tier (Private Repos):**

- 2,000 minutes/month
- 500 MB storage

**Your Usage Estimate:**

- ~1-2 min per run
- ~10 runs per day
- ~300 runs/month
- **Total:** ~300-600 min/month
- **Cost:** FREE ✅

---

### CircleCI

**Free Tier:**

- 6,000 minutes/month
- 1 concurrent job

**Your Usage:**

- Would also be FREE
- But requires separate account

**Verdict:** GitHub Actions is simpler with no extra account

---

## Quick Reference

### Enable Pipeline

```bash
git add .github/workflows/
git commit -m "Add CI/CD pipeline"
git push origin main
```

### View Results

```
GitHub → Repository → Actions tab
```

### Manual Trigger

```
Actions → python-versions.yml → Run workflow
```

### Status Badge

```markdown
![CI](https://github.com/USERNAME/REPO/workflows/Python%20Linting%20Pipeline/badge.svg)
```

---

## Best Practices

1. ✅ **Keep workflows fast** (< 5 minutes)
2. ✅ **Use caching** (pip, npm, etc.)
3. ✅ **Fail fast** (stop on first error)
4. ✅ **Run on PRs** (catch issues before merge)
5. ✅ **Use matrix testing** (test multiple versions)
6. ✅ **Add status badges** (show build status)
7. ✅ **Monitor failures** (fix quickly)

---

## Current Setup Summary

### ✅ Configured

- **Main pipeline** (`ci.yml`)

  - Runs on push/PR to main/develop
  - Checks: Black, Flake8, Pylint, MyPy
  - Verifies imports
  - Takes ~1-2 minutes

- **Compatibility check** (`python-versions.yml`)
  - Runs weekly
  - Tests Python 3.10-3.14
  - Manual trigger available

### ⬜ Future Enhancements

- Add pytest when tests are written
- Add code coverage reporting
- Add pre-commit hooks
- Add deployment automation

---

## Next Steps

1. **Push workflows to GitHub**

   ```bash
   git add .github/
   git commit -m "Add GitHub Actions CI/CD"
   git push
   ```

2. **Verify in GitHub Actions tab**

   - Check that workflows appear
   - Ensure first run passes

3. **Add status badge to README**

   - Copy badge markdown
   - Add to top of README.md

4. **Make a change and test**
   - Create a PR
   - Watch pipeline run
   - See checks on PR

---

**Setup Date:** December 20, 2025  
**Platform:** GitHub Actions  
**Status:** ✅ Ready to deploy  
**Estimated Cost:** FREE
