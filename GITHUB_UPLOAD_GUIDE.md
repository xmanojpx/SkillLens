# GitHub Upload Guide for SkillLens

This guide will walk you through uploading your SkillLens project to GitHub at https://github.com/xmanojpx/SkillLens

## Prerequisites

✅ All repository files have been prepared:
- `LICENSE` (MIT License)
- `CONTRIBUTING.md` (Contribution guidelines)
- `README.md` (Updated with badges and correct URLs)
- `.gitignore` (Enhanced to exclude unnecessary files)

## Step-by-Step Upload Process

### 1. Initialize Git Repository (if not already done)

Open PowerShell in the `f:\SkilLens` directory and run:

```powershell
cd f:\SkilLens
git init
```

### 2. Configure Git (if first time)

```powershell
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

### 3. Add Remote Repository

```powershell
git remote add origin https://github.com/xmanojpx/SkillLens.git
```

### 4. Review Files to be Committed

Check what will be included (respecting .gitignore):

```powershell
git status
```

**Files that WILL be committed:**
- All source code (`backend/app/`, `frontend/src/`)
- Configuration files (`.env.example`, `docker-compose.yml`, Dockerfiles)
- Documentation (all `.md` files including new `LICENSE` and `CONTRIBUTING.md`)
- Requirements files (`requirements.txt`, `package.json`)
- Batch scripts (`.bat` files)
- Research folder
- Database migration scripts
- Test files (`backend/tests/`)

**Files that will be EXCLUDED (via .gitignore):**
- `backend/venv/` (virtual environment)
- `backend/uploads/` (user uploads)
- `backend/__pycache__/` (Python cache)
- `frontend/node_modules/` (npm packages)
- `.env` (actual environment variables)
- `*.log` files
- Test output files (`*_test_results.txt`, `*_output.txt`)
- `demo.html` and `demo.py`

### 5. Stage All Files

```powershell
git add .
```

### 6. Create Initial Commit

```powershell
git commit -m "Initial commit: SkillLens AI-Powered Career Intelligence Platform

- Complete FastAPI backend with AI/ML features
- Next.js 14 frontend with TailwindCSS
- Research foundation with 100-student survey
- Docker support for easy deployment
- Comprehensive documentation and setup guides"
```

### 7. Push to GitHub

```powershell
# For first push
git push -u origin main

# If the branch is named 'master' instead
git branch -M main
git push -u origin main
```

**Note:** If you encounter authentication issues, you may need to:
- Use a Personal Access Token (PAT) instead of password
- Generate one at: https://github.com/settings/tokens
- Use it as your password when prompted

### 8. Add Repository Description on GitHub

After pushing, visit https://github.com/xmanojpx/SkillLens and:

1. Click the ⚙️ gear icon next to "About"
2. Add description:
   ```
   AI-Powered Career Intelligence & Workforce Readiness Platform using Semantic NLP, Knowledge Graphs, and Adaptive AI Agents
   ```
3. Add topics (tags):
   - `artificial-intelligence`
   - `machine-learning`
   - `career-development`
   - `nlp`
   - `knowledge-graph`
   - `fastapi`
   - `nextjs`
   - `python`
   - `typescript`
   - `education`

4. Add website (if you have one deployed)

### 9. Verify Upload

Check that all important files are visible on GitHub:
- ✅ README.md displays with badges
- ✅ LICENSE file is recognized
- ✅ Source code is properly organized
- ✅ .env file is NOT visible (only .env.example)
- ✅ No log files or test outputs

## Troubleshooting

### Issue: "Repository not found"
**Solution:** Make sure the repository exists on GitHub. Create it first at https://github.com/new

### Issue: "Authentication failed"
**Solution:** Use a Personal Access Token instead of password:
1. Go to https://github.com/settings/tokens
2. Generate new token (classic)
3. Select scopes: `repo` (full control)
4. Use token as password when pushing

### Issue: "Large files rejected"
**Solution:** Check for large files:
```powershell
git ls-files | ForEach-Object { Get-Item $_ } | Where-Object { $_.Length -gt 50MB }
```
Add large files to `.gitignore` if they shouldn't be committed.

### Issue: "Files I don't want are being committed"
**Solution:** 
1. Add them to `.gitignore`
2. Remove from staging: `git reset HEAD <file>`
3. Or remove from git entirely: `git rm --cached <file>`

## Post-Upload Checklist

- [ ] Repository is visible at https://github.com/xmanojpx/SkillLens
- [ ] README displays correctly with badges
- [ ] LICENSE is recognized by GitHub
- [ ] No sensitive data (API keys, passwords) is visible
- [ ] Repository description and topics are added
- [ ] All documentation links work correctly

## Next Steps

After successful upload, consider:

1. **Enable GitHub Actions** for CI/CD
2. **Set up branch protection** for main branch
3. **Create issues** for planned features
4. **Add collaborators** if working with a team
5. **Star your own repo** to show it's active 😊

---

**Congratulations! Your SkillLens project is now on GitHub!** 🎉
