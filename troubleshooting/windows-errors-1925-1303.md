# Windows Installer Errors 1925 and 1303  
## Understanding and Resolving Installation Failures

This document explains the Windows Installer errors **1925** and **1303**, why they occur when installing NSFW Manager, and how the MSI package was updated to avoid these issues. These errors are common when installing Win32 applications using MSI on modern Windows systems.

---

## 📌 Overview

During installation, Windows Installer may return:

- **Error 1925** – insufficient privileges  
- **Error 1303** – insufficient permissions to write to a directory  
- **Error 1603** – generic fatal error (often caused by 1925/1303)  

These errors typically appear in the MSI log during the **InstallFinalize** phase.

---

## 🔍 Error 1925 — Insufficient Privileges

### **Log Example**
Error 1925. You do not have sufficient privileges to complete this installation for all users of the machine.
Log on as administrator and then retry this installation.



### **Cause**

Error 1925 occurs when:

- The MSI is configured as **per-machine** (`ALLUSERS=1`)
- The installation writes to **Program Files**, **ProgramData**, or **HKLM**
- The installer is run **without elevation**
- Silent installs (`/quiet`) do **not** trigger UAC elevation

In this situation, Windows Installer attempts to perform privileged actions without admin rights, causing **InstallFinalize** to fail.

### **Impact**

- Installation fails with **Return value 3**  
- Windows Installer reports **1603** as the final error  
- The MSI appears valid, but cannot complete the commit phase

---

## 🔍 Error 1303 — Insufficient Permissions to Write to Directory

### **Log Example**

Error 1303. The installer has insufficient privileges to access this directory.


### **Cause**

Error 1303 occurs when the MSI attempts to write to protected locations such as:

- `C:\Program Files\...`
- `C:\ProgramData\...`
- Directories owned by SYSTEM or TrustedInstaller

If the MSI is not elevated, Windows denies access.

### **Impact**

- Files cannot be written  
- Shortcuts cannot be created  
- CustomActions may fail  
- Installation aborts during InstallFinalize

---

## 🔥 Why These Errors Happened in Early Versions of NSFW Manager

Earlier MSI builds were configured as:

- **per-machine installation**
- writing to **Program Files**
- creating shortcuts in **ProgramData**
- storing configuration in **HKLM**
- using **deferred CustomActions**

This required **administrator elevation**, but silent installs (`msiexec /quiet`) do not trigger UAC prompts.

As a result:

- GUI installation worked (because UAC prompt appeared)
- Silent installation failed (because no elevation occurred)

---

## 🟩 Final Fix Implemented in NSFW Manager

NSFW Manager now uses a **per-user MSI**, which installs into:

- `%LOCALAPPDATA%\NsfwManager\`
- `%APPDATA%\Roaming\NsfwManager\`
- User-writable directories only

### ✔ No Program Files  
### ✔ No ProgramData  
### ✔ No HKLM  
### ✔ No privileged CustomActions  
### ✔ No elevation required  
### ✔ Silent install works correctly

This eliminates errors **1925**, **1303**, and the resulting **1603**.

---

## 🧪 How to Verify the Fix

### Silent install command:
msiexec /i nsfwmanager.msi /quiet /L*V install.log



### Expected results:

- No UAC prompt  
- No privilege errors  
- No directory access errors  
- InstallFinalize completes successfully  
- Application installs under the user profile  
- Log contains **Return value 1** (success)

---

## 📁 Log Locations for Troubleshooting

If installation issues occur, check:
%APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
%LOCALAPPDATA%\NsfwManager\logs\startup.log
%LOCALAPPDATA%\NsfwManager\logs\execution.log


These logs help identify:

- permission issues  
- directory access failures  
- MSI initialization problems  
- engine loading errors  

---

## 📌 Summary

Errors **1925** and **1303** are caused by Windows Installer attempting privileged operations without elevation.  
By switching NSFW Manager to a **per-user MSI**, all privileged operations were removed, making installation:

- simpler  
- safer  
- compatible with silent installs  
- fully functional without admin rights  

This resolves the installation failures seen in earlier versions.

---




