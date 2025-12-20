# Quick Start: GitHub Actions CI/CD

## 🚀 Deploy Your Pipeline in 3 Steps

### Step 1: Check Your Files

Make sure these files exist:

```bash
.github/workflows/ci.yml                    ✅
.github/workflows/python-versions.yml       ✅
.gitignore                                   ✅
requirements.txt                             ✅
requirements-dev.txt                         ✅
.pylintrc                                    ✅
.flake8                                      ✅
pyproject.toml                               ✅
```

### Step 2: Commit and Push

```bash
# Check status
git status

# Add workflow files
git add .github/

# Add updated .gitignore
git add .gitignore

# Commit
git commit -m "Add GitHub Actions CI/CD pipeline with linting"

# Push to GitHub
git push origin main
```

### Step 3: Verify in GitHub

1. Go to your repository on GitHub
2. Click the **"Actions"** tab at the top
3. You should see "Python Linting Pipeline" running
4. Click on it to watch the progress
5. Wait for ✅ green checkmark

---

## ✅ Success Indicators

Your pipeline is working if you see:

1. **Actions tab shows workflow**

   - "Python Linting Pipeline" listed
   - Recent runs visible

2. **Green checkmarks**

   - All jobs completed
   - All steps passed

3. **On commits**

   - Small ✅ next to commit hash
   - Shows "All checks have passed"

4. **On pull requests**
   - Checks section at bottom
   - All required checks passing

---

## 🧪 Test Your Pipeline

### Create a Test PR

```bash
# Create a new branch
git checkout -b test-pipeline

# Make a small change
echo "# Test" >> README.md

# Commit and push
git add README.md
git commit -m "Test CI/CD pipeline"
git push origin test-pipeline

# Create PR on GitHub
# Watch the pipeline run automatically!
```

---

## 📊 What Happens When You Push

```
Your Computer                 GitHub                   GitHub Actions
     │                           │                            │
     │  git push                 │                            │
     ├──────────────────────────>│                            │
     │                           │                            │
     │                           │  Trigger workflow          │
     │                           ├───────────────────────────>│
     │                           │                            │
     │                           │                    Run: Black
     │                           │                            │
     │                           │                    Run: Flake8
     │                           │                            │
     │                           │                    Run: Pylint
     │                           │                            │
     │                           │                    Run: MyPy
     │                           │                            │
     │                           │  ✅ All passed             │
     │                           │<───────────────────────────┤
     │                           │                            │
     │  See results              │                            │
     │<──────────────────────────┤                            │
```

---

## 🔍 View Pipeline Details

### In GitHub UI:

1. **Actions Tab**

   - All workflow runs
   - Status: ✅ Success, ❌ Failure, 🟡 Running

2. **Click a Workflow Run**

   - See all jobs (lint, build)
   - Duration of each job
   - Which steps passed/failed

3. **Click a Job**

   - Detailed logs for each step
   - Exact error messages
   - Line-by-line output

4. **On Commits**
   - Hover over ✅ or ❌
   - Click "Details" to see full logs

---

## ❌ If Something Fails

### Check the Logs

```
Actions → Click failed run → Click failed job → Read error
```

### Common Issues:

**1. Import Error**

```
ModuleNotFoundError: No module named 'pygame'
```

**Fix:** Add to `requirements.txt`

**2. Linting Error**

```
F401 'sys' imported but unused
```

**Fix:** Remove unused import

**3. Formatting Error**

```
would reformat ship.py
```

**Fix:** Run `black .` locally

**4. Type Error**

```
error: Argument 1 has incompatible type
```

**Fix:** Check type hints

---

## 🎯 Daily Workflow

```bash
# 1. Start work
git checkout -b feature-name

# 2. Write code
vim ship.py

# 3. Format locally
black .

# 4. Check locally
flake8 .
pylint *.py

# 5. Commit
git add .
git commit -m "Add feature"

# 6. Push
git push origin feature-name

# 7. Create PR on GitHub

# 8. Wait for CI ✅

# 9. Merge when green
```

---

## 📈 Pipeline Metrics

### Typical Run Times:

- **Lint Job:** ~60-90 seconds

  - Black: ~5 seconds
  - Flake8: ~3 seconds
  - Pylint: ~30 seconds
  - MyPy: ~20 seconds

- **Build Job:** ~30 seconds

  - Import checks: ~5 seconds

- **Total:** ~2 minutes per run

### Free Tier Usage:

- 2,000 minutes/month (private repos)
- Unlimited (public repos)
- Your usage: ~300-600 min/month
- **Status:** Well within free tier ✅

---

## 🏆 Best Practices

### DO:

✅ Push to feature branches  
✅ Create PRs for review  
✅ Wait for CI before merging  
✅ Fix failures quickly  
✅ Run linters locally first

### DON'T:

❌ Push directly to main  
❌ Merge with failing checks  
❌ Ignore linting errors  
❌ Skip local testing  
❌ Commit without formatting

---

## 🔔 Next Steps

After your pipeline is running:

1. **Add Status Badge**

   ```markdown
   ![CI](https://github.com/USERNAME/REPO/workflows/Python%20Linting%20Pipeline/badge.svg)
   ```

2. **Write Tests**

   - Add pytest tests
   - Update pipeline to run tests

3. **Add Coverage**

   - Measure test coverage
   - Report to Codecov

4. **Branch Protection**
   - Require passing checks
   - Require PR reviews
   - Prevent direct pushes to main

---

## 📚 Resources

- [GitHub Actions Docs](https://docs.github.com/actions)
- [Python Actions Setup](https://github.com/actions/setup-python)
- [Workflow Syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)

---

## ✨ Your Pipeline is Ready!

**Files created:**

- ✅ `.github/workflows/ci.yml`
- ✅ `.github/workflows/python-versions.yml`
- ✅ Updated `.gitignore`

**Next command:**

```bash
git add .github/ .gitignore
git commit -m "Add GitHub Actions CI/CD pipeline"
git push origin main
```

**Then visit:** `https://github.com/YOUR_USERNAME/alien_invasion/actions`

🎉 **You now have a professional CI/CD pipeline!**
