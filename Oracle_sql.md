# Create New PDB (Fresh Start)

### Step-by-Step Guide

#### 1. Connect as SYSDBA

**Using SQLPlus:**
```cmd
sqlplus / as sysdba
```

**Using SQL Developer:**
- Connection Name: `SYS_Connection`
- Username: `sys`
- Password: (your Oracle installation password)
- Role: `SYSDBA`
- Connection Type: `Basic`
- Hostname: `localhost`
- Port: `1521`
- SID: `orcl` (your CDB name)

---

#### 2. Create New PDB

```sql
-- Create the PDB
CREATE PLUGGABLE DATABASE django_dev
  ADMIN USER django_admin IDENTIFIED BY "StrongPassword123!"
  ROLES = (DBA)
  FILE_NAME_CONVERT = (
    'F:\rokon\oradata\ORCL\pdbseed\',
    'F:\rokon\oradata\ORCL\django_dev\'
  );
```

**Parameters Explained:**
- `django_dev`: Your new database name (change as needed)
- `django_admin`: Admin username for this PDB
- `StrongPassword123!`: Choose a strong password
- `FILE_NAME_CONVERT`: Adjust path based on your Oracle data directory

**Find Your Data Directory:**
```sql
SELECT name, value 
FROM v$parameter 
WHERE name = 'db_create_file_dest';
```

---

#### 3. Open the PDB

```sql
-- Open immediately
ALTER PLUGGABLE DATABASE django_dev OPEN;

-- Auto-open on restart
ALTER PLUGGABLE DATABASE django_dev SAVE STATE;
```

---

#### 4. Verify Creation

```sql
-- Check all PDBs
SELECT pdb_id, pdb_name, status, open_mode 
FROM dba_pdbs;

-- Check running PDBs
SELECT name, open_mode 
FROM v$pdbs;
```

**Expected Output:**
```
NAME          OPEN_MODE
------------- ----------
PDB$SEED      READ ONLY
NEWPDB        READ WRITE
DJANGO_DEV    READ WRITE  ← Your new database
```

---

#### 5. Grant Privileges to Admin User

```sql
-- Connect to the new PDB
ALTER SESSION SET CONTAINER = django_dev;

-- Grant necessary privileges
GRANT CONNECT, RESOURCE, DBA TO django_admin;
GRANT CREATE SESSION TO django_admin;
GRANT CREATE TABLE TO django_admin;
GRANT CREATE SEQUENCE TO django_admin;
GRANT CREATE VIEW TO django_admin;
GRANT UNLIMITED TABLESPACE TO django_admin;

-- Verify grants
SELECT * FROM dba_sys_privs WHERE grantee = 'DJANGO_ADMIN';
```

---

#### 6. Test Connection

```sql
-- Exit and reconnect with new credentials
EXIT;
```

```cmd
sqlplus django_admin/StrongPassword123!@localhost:1521/django_dev
```

**If successful:**
```sql
SELECT * FROM DUAL;
-- Should return: X
```

---

## Django Configuration

### Update `.env` file:

```env
# Oracle Database Configuration
ORACLE_NAME=localhost:1521/django_dev
ORACLE_USER=django_admin
ORACLE_PASSWORD=StrongPassword123!
USE_ORACLE=True
```

### Update `settings.py`:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.oracle',
        'NAME': os.environ.get('ORACLE_NAME', 'localhost:1521/django_dev'),
        'USER': os.environ.get('ORACLE_USER', 'django_admin'),
        'PASSWORD': os.environ.get('ORACLE_PASSWORD', 'StrongPassword123!'),
    }
}
```

---
