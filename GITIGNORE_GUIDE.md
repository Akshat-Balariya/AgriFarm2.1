# .gitignore Configuration Guide

## Overview

The `.gitignore` files have been configured for your entire AgriGo project to:
- ✅ **Prevent committing secrets** - `.env` files with API keys
- ✅ **Avoid build artifacts** - `build/`, `node_modules/`, compiled code
- ✅ **Exclude large files** - Virtual environments, dependencies
- ✅ **Skip IDE files** - `.idea/`, `.vscode/` project configurations
- ✅ **Keep repository clean** - Logs, temp files, OS files

---

## Files Updated/Created

### Root Directory
**File**: `.gitignore`
- Covers entire project
- Python, Node.js, Android, IDE, OS files
- Most comprehensive rules

### Server Folder
**File**: `server/.gitignore`
- Python venv and dependencies
- Node.js node_modules
- Flask/Django temporary files
- Database files

### Client Folder (Android)
**File**: `client/.gitignore`
- Android Studio & Gradle cache
- Build outputs (APK, AAB)
- IDE configuration files
- NDK build artifacts

### Website Folder
**File**: `agrinova-ui-main/.gitignore`
- Node modules and lock files
- Vite build output
- TypeScript build info
- Environment files

---

## What Gets Ignored

### 🚫 Secrets & Sensitive Files
```
.env                    ← Local environment variables with API keys
.env.local              ← Local environment overrides
.env.*.local            ← Environment-specific secrets
*.pem, *.key, *.cert    ← SSL certificates and private keys
credentials.json        ← Firebase/Google credentials
```

### 🚫 Dependencies & Virtual Environments
```
venv/                   ← Python virtual environment
env/                    ← Python environment
node_modules/           ← Node.js dependencies
__pycache__/            ← Python compiled cache
.gradle/                ← Gradle cache
```

### 🚫 Build Artifacts
```
build/                  ← Build output directory
dist/                   ← Distribution files
*.apk, *.aab            ← Android packages
*.egg-info/             ← Python package info
.pytest_cache/          ← Test cache
```

### 🚫 IDE & Editor Files
```
.idea/                  ← IntelliJ/Android Studio config
.vscode/                ← VS Code config
*.iml                   ← IntelliJ module file
.sublime-workspace      ← Sublime Text workspace
```

### 🚫 OS & Temporary Files
```
.DS_Store               ← macOS files
Thumbs.db               ← Windows thumbnails
*.log                   ← Log files
*.tmp, *.bak            ← Temporary files
```

---

## ✅ What Gets Tracked (Examples)

### 📝 DO Commit These
```
.env.example            ← Template (no real secrets!)
.gitignore              ← This file
Procfile                ← Render deployment config
render.yaml             ← Deployment blueprint
build.gradle.kts        ← Build configuration
package.json            ← Dependencies list
requirements.txt        ← Python dependencies
src/                    ← All source code
README.md               ← Documentation
```

### 🚫 DON'T Commit These
```
.env                    ← Real secrets
.env.production         ← Production secrets
node_modules/           ← Dependencies (recreate from package.json)
build/                  ← Build output (recreate on compile)
venv/                   ← Virtual env (recreate from requirements.txt)
```

---

## Best Practices

### ✅ DO

1. **Use `.env.example` for templates**
   ```bash
   # Copy to create your local .env
   cp .env.example .env
   # Then edit .env with real values
   ```

2. **Commit `package.json` and `requirements.txt`**
   - Team members can recreate dependencies
   - No need to commit node_modules or venv

3. **Add new secrets to `.gitignore`**
   - If you create a new secret file, add it immediately:
   ```bash
   echo "secrets.json" >> .gitignore
   ```

4. **Use different `.env` per environment**
   ```
   .env                        ← Local development
   .env.production             ← Production (never commit!)
   .env.example                ← Safe template (DO commit)
   ```

### 🚫 DON'T

1. **Never commit `.env` with real secrets**
2. **Never commit `node_modules/` or `venv/`**
3. **Never commit build output (`build/`, `dist/`)**
4. **Never commit IDE configuration (`.idea/`, `.vscode/`)**
5. **Never commit database files or locks**

---

## Common Issues & Solutions

### Problem: "Accidentally committed `.env`"

**Solution - Remove from history:**
```bash
# If not yet pushed
git reset HEAD .env
rm .env
git add .gitignore
git commit --amend

# If already pushed (more complex)
git filter-branch --tree-filter 'rm -f .env' HEAD
git push --force-with-lease
```

### Problem: "I need to ignore a new file type"

**Solution - Add to `.gitignore`:**
```bash
echo "*.backup" >> .gitignore
git add .gitignore
git commit -m "Ignore backup files"
```

### Problem: "File is ignored but I need to track it"

**Solution - Use `!` to unignore:**
```gitignore
# In .gitignore:
*.log           # Ignore all logs
!important.log  # Except this one
```

---

## Checking What's Ignored

### See ignored files
```bash
git status --ignored
```

### Check if specific file is ignored
```bash
git check-ignore -v filename
```

### Force add ignored file (dangerous!)
```bash
git add -f filename
```

---

## Per-Folder Configuration

### `/` (Root)
- High-level rules
- Covers all subfolders
- Most comprehensive

### `/server/`
- Python & Node.js specific
- Flask/backend rules
- Database files

### `/client/`
- Android Studio specific
- Gradle & build rules
- SDK configuration

### `/agrinova-ui-main/`
- Node.js & Vite specific
- Build output rules
- TypeScript rules

---

## Render Deployment Note

When deploying to Render:
- ✅ `.env.example` is tracked (used as template)
- ✅ `Procfile` is tracked (required by Render)
- ✅ `render.yaml` is tracked (deployment config)
- 🚫 Real `.env` is never committed (secrets in Render dashboard)

---

## Summary

| File | Ignore? | Why |
|------|---------|-----|
| `.env` | ✅ YES | Contains real secrets |
| `.env.example` | ❌ NO | Safe template for team |
| `node_modules/` | ✅ YES | Huge, recreate from package.json |
| `venv/` | ✅ YES | Huge, recreate from requirements.txt |
| `build/` | ✅ YES | Regenerated on compile |
| `src/` | ❌ NO | Your actual source code |
| `.idea/` | ✅ YES | IDE preferences, not shared |
| `README.md` | ❌ NO | Project documentation |
| `package.json` | ❌ NO | Dependency declarations |
| `requirements.txt` | ❌ NO | Python dependencies |

---

## Quick Reference Commands

```bash
# Check current ignored files
git status --ignored

# Add a file to .gitignore
echo "*.backup" >> .gitignore

# Remove file from tracking but keep locally
git rm --cached filename

# Check if file is ignored
git check-ignore -v myfile.txt

# See what would be committed
git status

# Dry run to see what would be deleted
git clean -n
```

---

## Files Configured

✅ **Root `.gitignore`** - Main comprehensive configuration
✅ **`server/.gitignore`** - Python & Node.js specific
✅ **`client/.gitignore`** - Android Studio specific  
✅ **`agrinova-ui-main/.gitignore`** - Vite & React specific

All are ready to use!

---

**Remember**: Commit `.env.example`, not `.env`. Your team needs the template, not your secrets!

