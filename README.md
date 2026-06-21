# Medication Reconciliation System — Installation Guide

## What You Get
- Full web application running on your hospital server
- All staff access from any computer on the network via browser
- Centralized database — one source of truth
- Login system with 3 roles: Admin / Pharmacist / Viewer
- Excel + PDF export that works 100%
- Notifications for incomplete records

---

## Requirements
- Windows Server or any Windows PC that stays ON
- Python 3.9 or newer (free download: python.org)
- No internet required after setup (except for Chart.js CDN — can be made offline too)

---

## Step 1 — Install Python
1. Go to https://python.org/downloads
2. Download Python 3.12 (or newer)
3. Run installer → CHECK "Add Python to PATH" ✓
4. Click Install Now

---

## Step 2 — Copy Files
Copy the entire `medrecon` folder to your server, for example:
```
C:\MedRecon\
```

---

## Step 3 — Install Dependencies
Open **Command Prompt** (cmd) as Administrator:
```cmd
cd C:\MedRecon
pip install -r requirements.txt
```

---

## Step 4 — Run the Application
```cmd
cd C:\MedRecon
python app.py
```

You will see:
```
==================================================
  Medication Reconciliation System
  URL: http://localhost:5000
  Login: admin / Admin@2026
==================================================
```

---

## Step 5 — Access from Other Computers

### Find your server's IP address:
Open Command Prompt and type: `ipconfig`
Look for **IPv4 Address** — example: `192.168.1.50`

### Access from any computer on the network:
```
http://192.168.1.50:5000
```

---

## Step 6 — Run Automatically on Windows Startup

### Option A: Simple (Task Scheduler)
1. Open **Task Scheduler** → Create Basic Task
2. Name: `MedRecon Server`
3. Trigger: When the computer starts
4. Action: Start a program
   - Program: `python`
   - Arguments: `C:\MedRecon\app.py`
   - Start in: `C:\MedRecon`
5. Check "Run whether user is logged on or not"

### Option B: Windows Service (More reliable)
Install NSSM (Non-Sucking Service Manager):
```cmd
nssm install MedRecon python C:\MedRecon\app.py
nssm start MedRecon
```

---

## Default Login Credentials
| Username | Password    | Role  |
|----------|-------------|-------|
| admin    | Admin@2026  | Admin |

⚠️ **Change the admin password immediately after first login!**

---

## User Roles
| Role       | Can View | Can Add/Edit | Can Delete | Admin Panel |
|------------|----------|--------------|------------|-------------|
| Admin      | ✓        | ✓            | ✓          | ✓           |
| Pharmacist | ✓        | ✓            | ✓          | ✗           |
| Viewer     | ✓        | ✗            | ✗          | ✗           |

---

## Adding Users (Admin only)
1. Login as admin
2. Go to Admin → User Management
3. Click "+ Add User"
4. Fill name, username, password, role
5. Click Save

---

## Backup Your Data
The database is stored in: `C:\MedRecon\medrecon.db`

**Backup:** Copy this file to a safe location daily (or set up Windows Task Scheduler to auto-copy).

---

## Firewall (if other computers can't connect)
Open **Windows Defender Firewall** → Advanced Settings → Inbound Rules → New Rule:
- Type: Port
- Port: 5000
- Allow the connection

---

## Troubleshooting
| Issue | Solution |
|-------|----------|
| "python is not recognized" | Re-install Python with "Add to PATH" checked |
| "Port 5000 in use" | Change port in app.py: `app.run(port=5001)` |
| Other computers can't connect | Check firewall, use server's IP not `localhost` |
| Database error | Delete `medrecon.db` and restart (loses data) |

---

## File Structure
```
C:\MedRecon\
├── app.py              ← Main application (run this)
├── requirements.txt    ← Python packages
├── medrecon.db         ← Database (auto-created)
├── templates/
│   ├── login.html      ← Login page
│   └── app.html        ← Main application
└── static/             ← CSS/JS files (if any)
```
