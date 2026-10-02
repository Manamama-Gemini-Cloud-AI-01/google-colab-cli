# Project Workflow: Upstream Sync & Local Installation

## Why Rebasing Fails Here
*Do not rebase (`git rebase main`) across the 18 divergent commits on `fix-auth-browser-open`.* Rebasing replays every historical commit one by one, causing cascading, repeated merge conflicts in documentation (`docs/`), changelogs, tests, and dependency pins (`pyproject.toml`).

---

## The Correct Sync Procedure (Single-Step Merge & Strategic Resolution)

To update your branch with upstream `main` while preserving your browser-opening fix and avoiding rebase hell:

### Step 1: Sync Local Main
```bash
git checkout main
git pull origin main
```

### Step 2: Merge Main into Feature Branch
```bash
git checkout fix-auth-browser-open
git merge main
```

### Step 3: Handle Conflicts Explicitly
If conflicts occur during `git merge main`:
1. **Preserve our functional code fix** in `src/colab_cli/commands/automation.py`.
2. **Accept upstream (`theirs`)** for all documentation, logs, tests, and dependency files using Git's built-in strategy:
   ```bash
   git checkout --theirs docs/ tests/ pyproject.toml CHANGELOG.md
   git add .
   git commit -m "Merge branch 'main' into fix-auth-browser-open (accepting upstream docs/tests/deps)"
   ```
   *(Or resolve file-by-file using `git checkout --theirs <file>` for any non-code file).*

---

## Development & Validation
- **Run Tests:** `pytest -v`
- **Environment:** Android / Termux
