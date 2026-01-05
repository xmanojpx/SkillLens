# SkillLens Setup - Step by Step Guide

## ✅ PostgreSQL is Already Installed!

Since PostgreSQL is installed on your C drive, follow these simple steps:

---

## Step 1: Run Setup Script

```powershell
cd f:\SkilLens
.\setup_local.bat
```

**What it does:**
- Finds PostgreSQL automatically (checks C:\Program Files\PostgreSQL)
- Creates Python virtual environment
- Installs all dependencies
- Creates database and user
- Initializes database schema

**When prompted for password:**
- Enter the password you set when installing PostgreSQL
- This is the password for the `postgres` user

---

## Step 2: Start Backend

```powershell
.\start_backend.bat
```

The server will start on: http://localhost:8000

---

## Step 3: Test It

Open browser: http://localhost:8000/docs

Or run automated tests:
```powershell
.\run_tests.bat
```

---

## 🔧 If Setup Script Doesn't Find PostgreSQL

### Option A: Manual Database Creation

```powershell
# 1. Open Command Prompt or PowerShell
# 2. Navigate to PostgreSQL bin folder
cd "C:\Program Files\PostgreSQL\16\bin"

# 3. Create database (enter postgres password when prompted)
.\psql.exe -U postgres -c "CREATE DATABASE skilllens;"
.\psql.exe -U postgres -c "CREATE USER skilllens WITH PASSWORD 'skilllens';"
.\psql.exe -U postgres -c "GRANT ALL PRIVILEGES ON DATABASE skilllens TO skilllens;"
```

### Option B: Using pgAdmin

1. Open pgAdmin (installed with PostgreSQL)
2. Connect to PostgreSQL server
3. Right-click "Databases" → Create → Database
   - Name: `skilllens`
4. Right-click "Login/Group Roles" → Create → Login/Group Role
   - Name: `skilllens`
   - Password: `skilllens`
   - Privileges: Can login
5. Right-click `skilllens` database → Properties → Security
   - Add `skilllens` user with all privileges

---

## Step 4: Initialize Database Schema

```powershell
cd f:\SkilLens\backend

# Create virtual environment (if not exists)
python -m venv venv

# Activate it
.\venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt

# Initialize database
python scripts\init_db.py --seed
```

**Expected output:**
```
INFO:root:Connecting to PostgreSQL...
INFO:root:Creating database tables...
INFO:root:✅ Database initialized successfully!
```

---

## Step 5: Start Server

```powershell
cd f:\SkilLens\backend
.\venv\Scripts\Activate.ps1
uvicorn app.main:app --reload
```

**Expected output:**
```
INFO:     Uvicorn running on http://0.0.0.0:8000
INFO:     Application startup complete.
```

---

## Step 6: Verify

### Check API Docs
Open: http://localhost:8000/docs

### Check Health
Open: http://localhost:8000/health

Should show:
```json
{
  "status": "healthy",
  "message": "SkillLens API is running",
  "database": "PostgreSQL"
}
```

### Run Tests
```powershell
cd f:\SkilLens\backend
.\venv\Scripts\Activate.ps1
python test_migration.py
```

All 9 tests should pass! ✅

---

## 🎯 Quick Reference

### Start Backend
```powershell
.\start_backend.bat
```

### Run Tests
```powershell
.\run_tests.bat
```

### Connect to Database
```powershell
cd "C:\Program Files\PostgreSQL\16\bin"
.\psql.exe -U skilllens -d skilllens
```

### Check Database Tables
```sql
\dt
-- Should show 13 tables
```

### View Data
```sql
SELECT COUNT(*) FROM users;
SELECT COUNT(*) FROM resumes;
SELECT COUNT(*) FROM readiness_scores;
```

---

## 🐛 Troubleshooting

### PostgreSQL Service Not Running

```powershell
# Check service status
Get-Service -Name postgresql*

# Start service
net start postgresql-x64-16
```

### Can't Find psql.exe

Add PostgreSQL to PATH:
```powershell
# Temporary (current session only)
$env:Path += ";C:\Program Files\PostgreSQL\16\bin"

# Permanent (run as Administrator)
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Program Files\PostgreSQL\16\bin", "Machine")
```

### Database Connection Error

Check `.env` file has correct DATABASE_URL:
```
DATABASE_URL=postgresql+asyncpg://skilllens:skilllens@localhost:5432/skilllens
```

### Port 8000 Already in Use

```powershell
# Find what's using port 8000
netstat -ano | findstr :8000

# Kill the process (replace PID)
taskkill /PID <PID> /F

# Or use different port
uvicorn app.main:app --reload --port 8001
```

---

## ✅ Success Checklist

- [ ] PostgreSQL service is running
- [ ] Database `skilllens` created
- [ ] User `skilllens` created
- [ ] Python virtual environment created
- [ ] Dependencies installed
- [ ] `.env` file configured
- [ ] Database initialized (13 tables)
- [ ] Backend starts without errors
- [ ] http://localhost:8000/docs loads
- [ ] All tests pass

---

## 🚀 You're Ready!

Once everything is set up:

1. **Test Authentication**: Register a user at `/api/auth/register`
2. **Upload Resume**: Use `/api/resume/upload`
3. **Calculate Score**: Use `/api/scoring/readiness`
4. **View Data**: Check database with psql

**Your PostgreSQL migration is complete and working!** 🎉
