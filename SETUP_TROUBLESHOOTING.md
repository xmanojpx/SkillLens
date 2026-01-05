# Complete Setup Guide - Step by Step

## Current Issue: Database Connection Failed

The error shows that the PostgreSQL user `skilllens` doesn't have permission to connect to the database.

---

## Solution: Follow These Steps

### Step 1: Create Database

```powershell
cd f:\SkilLens
.\create_database.bat
```

**What it does:**
- Creates database `skilllens`
- Creates user `skilllens` with password `skilllens`
- Grants all necessary permissions
- Tests the connection

**When prompted:** Enter your PostgreSQL `postgres` user password

---

### Step 2: Verify Database

```powershell
# Navigate to PostgreSQL bin
cd "C:\Program Files\PostgreSQL\18\bin"

# Test connection (password: skilllens)
.\psql.exe -U skilllens -d skilllens

# You should see:
# skilllens=#

# Type \q to exit
\q
```

---

### Step 3: Initialize Schema

```powershell
cd f:\SkilLens\backend

# Make sure venv is activated
.\venv\Scripts\Activate.ps1

# Run simple init script
python init_db_simple.py
```

**Expected output:**
```
Connecting to PostgreSQL...
Executing SQL statements...
  [1/50] ✓
  [2/50] ✓
  ...
✅ Database initialized successfully!
```

---

### Step 4: Start Backend

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

### Step 5: Test

Open browser: http://localhost:8000/docs

Or run tests:
```powershell
cd f:\SkilLens\backend
python test_migration.py
```

---

## Alternative: Manual Database Creation

If the script doesn't work, create the database manually:

### Using psql:

```powershell
cd "C:\Program Files\PostgreSQL\18\bin"

# Connect as postgres
.\psql.exe -U postgres

# In psql, run:
CREATE DATABASE skilllens;
CREATE USER skilllens WITH PASSWORD 'skilllens';
GRANT ALL PRIVILEGES ON DATABASE skilllens TO skilllens;
\c skilllens
GRANT ALL ON SCHEMA public TO skilllens;
ALTER DATABASE skilllens OWNER TO skilllens;
\q
```

### Using pgAdmin:

1. Open pgAdmin
2. Connect to PostgreSQL server
3. Right-click "Databases" → Create → Database
   - Name: `skilllens`
   - Owner: Create new role `skilllens` (password: `skilllens`)
4. Right-click `skilllens` database → Query Tool
5. Run:
   ```sql
   GRANT ALL ON SCHEMA public TO skilllens;
   ALTER DATABASE skilllens OWNER TO skilllens;
   ```

---

## Verify Everything Works

### Check 1: Database Connection
```powershell
cd "C:\Program Files\PostgreSQL\18\bin"
.\psql.exe -U skilllens -d skilllens -c "SELECT 1;"
```

Should return: `1`

### Check 2: Schema Initialized
```powershell
.\psql.exe -U skilllens -d skilllens -c "\dt"
```

Should show 13 tables

### Check 3: Backend Starts
```powershell
cd f:\SkilLens\backend
uvicorn app.main:app --reload
```

Should start without errors

### Check 4: API Works
Open: http://localhost:8000/health

Should return:
```json
{
  "status": "healthy",
  "message": "SkillLens API is running"
}
```

---

## Quick Commands

```powershell
# Create database
.\create_database.bat

# Initialize schema
cd backend
python init_db_simple.py

# Start server
uvicorn app.main:app --reload

# Test
python test_migration.py
```

---

## Common Issues

### Issue: Password authentication failed

**Solution:** The password for user `skilllens` should be `skilllens`. If you set a different password, update `.env`:
```
DATABASE_URL=postgresql+asyncpg://skilllens:YOUR_PASSWORD@localhost:5432/skilllens
```

### Issue: Database does not exist

**Solution:** Run `.\create_database.bat` first

### Issue: Permission denied

**Solution:** Grant permissions:
```sql
GRANT ALL ON SCHEMA public TO skilllens;
ALTER DATABASE skilllens OWNER TO skilllens;
```

### Issue: PostgreSQL service not running

**Solution:**
```powershell
net start postgresql-x64-18
```

---

**Follow these steps in order and everything should work!** 🚀
