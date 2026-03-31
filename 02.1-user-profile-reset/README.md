# 02.1 – Reset Local User Profile (testuser)

## Objective

Automatically reset a local user profile (`testuser`) to default by deleting it and forcing Windows to recreate it on next login.

---

## Key Concepts

### CIM (Win32_UserProfile)

System-level representation of user profiles. Required for clean profile removal.

### Filesystem

Actual user data:

```
C:\Users\testuser
```

---

## Why Simple Deletion Is Not Enough

| Method               | Result             |
| -------------------- | ------------------ |
| Delete folder only   | Broken profile     |
| Delete registry only | Orphan data        |
| Delete via CIM only  | Incomplete cleanup |

Correct approach:
→ CIM + Registry (optional) + Filesystem

Registry cleanup is optional because removing the profile via Win32_UserProfile (CIM) automatically deletes the corresponding ProfileList entry.

---

## Script Location

```
C:\Scripts\reset-testuser.ps1
```

---

## PowerShell Script

```powershell
$ErrorActionPreference = 'Stop'

$UserName = 'testuser'
$ProfilePath = "C:\Users\$UserName"
$LogFile = "C:\Scripts\reset-testuser.log"

Start-Transcript -Path $LogFile -Append

try {
    Write-Host "==== START: $(Get-Date) ===="

    $Profile = Get-CimInstance Win32_UserProfile | Where-Object {
        $_.LocalPath -ieq $ProfilePath
    }

    if ($Profile) {
        if ($Profile.Loaded) {
            throw "Profile is loaded. Cannot delete."
        }

        Remove-CimInstance -InputObject $Profile
    }

    Start-Sleep -Seconds 2

    if (Test-Path $ProfilePath) {
        cmd.exe /c "rd /s /q `"$ProfilePath`""
    }

    if (Test-Path $ProfilePath) {
        throw "Profile folder still exists."
    }

    Write-Host "Profile successfully removed."
}
catch {
    Write-Host "ERROR: $($_.Exception.Message)"
}
finally {
    Stop-Transcript
}
```

---

## Manual Test

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File "C:\Scripts\reset-testuser.ps1"
```

---

## Verification

```powershell
Get-Content "C:\Scripts\reset-testuser.log"
```

Expected:

- Profile found  
- Profile removed  
- Folder deleted  

---

## Functional Test

1. Login as `testuser`  
2. Create file on Desktop/Documents/Downloads and change wallpaper  
3. Logout  
4. Restart the machine (script executes at startup)  
5. Login again  

Expected:

- Clean profile  
- Desktop reset  
- Files removed  

---

## Key Learnings

- User profile ≠ just a folder  
- Must handle:
  - CIM (system layer)
  - Filesystem (data)
- Profiles cannot be deleted while loaded  
- SYSTEM context ensures full access, administrator privileges are more limited 
- Logging is mandatory for debugging  

---

## Common Failures

| Issue                     | Cause                       |
| ------------------------- | --------------------------- |
| Profile not deleted       | User still logged in        |
| Script runs but no effect | Wrong path                  |
| No log file               | Script not executed         |
| Access denied             | Not running as SYSTEM/Admin |
