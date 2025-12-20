# CI/CD Troubleshooting Guide

## Common GitHub Actions Issues

### Issue: Pygame Installation Fails

**Error:**

```
RuntimeError: Unable to run "sdl-config". Please make sure a development version of SDL is installed.
```

**Cause:** Pygame requires SDL (Simple DirectMedia Layer) system libraries that aren't installed on Ubuntu runners by default.

**Solution:** ✅ **Already Fixed in Your Workflows**

The workflows now install SDL dependencies before installing Pygame:

```yaml
- name: Install SDL dependencies
  run: |
    sudo apt-get update
    sudo apt-get install -y \
      libsdl2-dev \
      libsdl2-image-dev \
      libsdl2-mixer-dev \
      libsdl2-ttf-dev \
      libfreetype6-dev \
      libportmidi-dev \
      libjpeg-dev \
      python3-dev
```

**What these packages do:**

- `libsdl2-dev` - Main SDL library (required)
- `libsdl2-image-dev` - Image loading (PNG, JPG)
- `libsdl2-mixer-dev` - Audio mixing
- `libsdl2-ttf-dev` - TrueType font rendering
- `libfreetype6-dev` - Font rendering support
- `libportmidi-dev` - MIDI support
- `libjpeg-dev` - JPEG image support
- `python3-dev` - Python development headers

---

## Other Common Issues

### Issue: Linter Fails But Passes Locally

**Possible Causes:**

1. Different Python versions
2. Missing files in git
3. Different line endings (CRLF vs LF)

**Solutions:**

```bash
# Check Python version matches CI
python --version  # Should be 3.14

# Ensure all files are committed
git status

# Fix line endings (if on Windows)
git config core.autocrlf input
```

---

### Issue: Import Errors in CI

**Error:**

```
ModuleNotFoundError: No module named 'pygame'
```

**Solutions:**

1. Ensure `pygame` is in `requirements.txt`
2. Check SDL dependencies are installed (see above)
3. Verify cache isn't corrupted (re-run workflow)

---

### Issue: Cache Issues

**Symptom:** Old dependencies being used

**Solution:**

```yaml
# In your workflow, cache is enabled:
cache: "pip"
# To bust cache, you can:
# 1. Update requirements.txt
# 2. Manually clear cache in GitHub Settings
# 3. Change cache key
```

---

### Issue: Workflow Doesn't Trigger

**Check:**

1. Workflow file is in `.github/workflows/`
2. File has `.yml` extension
3. Branch name matches trigger
4. YAML syntax is valid

**Test locally:**

```bash
# Install act (GitHub Actions locally)
brew install act  # Mac
# or
curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash

# Run workflow locally
act push
```

---

### Issue: Permission Denied

**Error:**

```
permission denied while trying to connect to the Docker daemon
```

**Cause:** Runner doesn't have Docker permissions (rare for standard workflows)

**Solution:** Use `sudo` for Docker commands or ensure proper permissions

---

## Debugging Tips

### 1. View Detailed Logs

In GitHub:

1. Go to Actions tab
2. Click failed workflow
3. Click failed job
4. Expand failed step
5. Read full error message

### 2. Add Debug Output

```yaml
- name: Debug environment
  run: |
    echo "Python version:"
    python --version
    echo "Pip version:"
    pip --version
    echo "Installed packages:"
    pip list
    echo "Environment variables:"
    env | sort
```

### 3. Check Specific Files

```yaml
- name: Check files exist
  run: |
    ls -la
    cat requirements.txt
    cat .pylintrc
```

### 4. Run Steps Independently

Comment out later steps to isolate issues:

```yaml
# - name: Run linters
#   run: black --check .
```

---

## Pygame-Specific Issues

### Issue: Display Errors

**Error:**

```
pygame.error: No available video device
```

**Cause:** Pygame needs a display, but CI has none

**Solution:** Use headless mode (xvfb) for tests

```yaml
# For running the actual game (not needed for linting):
- name: Run with virtual display
  run: |
    sudo apt-get install -y xvfb
    xvfb-run python alien_invasion.py
```

**Note:** Your current workflow only lints code, doesn't run the game, so this isn't needed yet.

---

### Issue: Audio Errors

**Error:**

```
pygame.error: No available audio device
```

**Solution:** Initialize pygame without audio in CI

```python
# In your test setup:
import os
os.environ['SDL_AUDIODRIVER'] = 'dummy'
pygame.init()
```

---

## Workflow File Issues

### Issue: YAML Syntax Error

**Symptoms:** Workflow doesn't appear or fails immediately

**Check:**

```bash
# Install yamllint
pip install yamllint

# Check syntax
yamllint .github/workflows/ci.yml
```

**Common mistakes:**

```yaml
# ❌ Wrong indentation
steps:
- name: Test
run: echo "test"

# ✅ Correct indentation
steps:
  - name: Test
    run: echo "test"

# ❌ Missing quotes
python-version: 3.14

# ✅ With quotes (safer)
python-version: "3.14"
```

---

## Performance Issues

### Issue: Workflow is Slow

**Optimizations already in your workflows:**

1. **Pip caching:** ✅ Enabled

   ```yaml
   cache: "pip"
   ```

2. **Apt caching:** Could add

   ```yaml
   - name: Cache apt packages
     uses: actions/cache@v3
     with:
       path: /var/cache/apt
       key: apt-${{ runner.os }}
   ```

3. **Parallel jobs:** ✅ Already separate (lint + build)

4. **Fail fast:** ✅ Using `continue-on-error: false`

---

## Monitoring

### Check Workflow Status

```bash
# Using GitHub CLI
gh workflow list
gh run list
gh run view <run-id>

# View logs
gh run view <run-id> --log
```

### Status Badges

Already in your README:

```markdown
![CI](https://github.com/USERNAME/REPO/workflows/Python%20Linting%20Pipeline/badge.svg)
```

---

## Getting Help

### When to Ask for Help

1. Error persists after trying solutions
2. Unclear error messages
3. Workflow-specific GitHub Actions issues

### Where to Get Help

- **GitHub Actions docs:** https://docs.github.com/actions
- **Pygame docs:** https://www.pygame.org/docs/
- **Your learning notes:** `docs/development/LEARNING_NOTES.md`
- **Stack Overflow:** Tag with `github-actions` and `pygame`

---

## Quick Fixes Checklist

When workflow fails:

- [ ] Check error message carefully
- [ ] SDL dependencies installed? (✅ now fixed)
- [ ] All files committed?
- [ ] Python version correct?
- [ ] requirements.txt up to date?
- [ ] YAML syntax valid?
- [ ] Try re-running workflow
- [ ] Check previous successful runs for changes

---

## Prevention

### Before Pushing

```bash
# 1. Run linters locally
black .
flake8 .
pylint *.py
mypy .

# 2. Ensure all files committed
git status

# 3. Test imports
python -c "import alien_invasion; import settings; import ship"

# 4. Push
git push
```

### CI/CD Best Practices

1. ✅ **Test locally first** - Run linters before pushing
2. ✅ **Small commits** - Easier to debug failures
3. ✅ **Clear messages** - Help identify what changed
4. ✅ **Monitor regularly** - Check Actions tab
5. ✅ **Fix quickly** - Don't let failures pile up

---

**Last Updated:** December 20, 2025  
**Status:** SDL dependencies added to workflows ✅
