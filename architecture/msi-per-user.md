# MSI Per‑User Installation  
## How NSFW Manager Installs Without Administrator Privileges

This document explains how NSFW Manager uses a **per‑user MSI installation model**, why this approach was chosen, and how it improves compatibility, reliability, and user experience on modern Windows systems. It also describes the differences between per‑user and per‑machine MSI packages and how this affects installation behavior.

---

## 📌 Overview

NSFW Manager installs using a **per‑user MSI**.  
This means:

- No administrator rights required  
- No UAC elevation prompt  
- Silent installation works correctly  
- All files are stored in user‑writable directories  
- No privileged registry keys are used  
- No system‑wide changes are made  

This installation model is ideal for desktop applications that do not require system‑level components.

---

# 🟦 Why Per‑User MSI?

Earlier versions of NSFW Manager used a **per‑machine MSI**, which caused:

- Windows Installer error **1925** (insufficient privileges)  
- Windows Installer error **1303** (cannot write to Program Files)  
- Silent install failures (`msiexec /quiet`)  
- UAC elevation prompts  
- Installation failures during the `InstallFinalize` phase  

These issues occurred because per‑machine MSIs require administrator rights and write to protected locations.

Switching to a **per‑user MSI** eliminates all of these problems.

---

# 🟩 Installation Paths (Per‑User)

NSFW Manager installs entirely under the user profile:

### Application files
%LOCALAPPDATA%\NsfwManager\


### Configuration, logs, and runtime data
%APPDATA%\Roaming\NsfwManager\
%LOCALAPPDATA%\NsfwManager\logs\


### Quarantine directory
%LOCALAPPDATA%\NsfwManager\Quarantine\


### Default move‑to directory
%USERPROFILE%\Documents\MyPrivatePictures\


These locations are **always writable** by the current user, ensuring reliable installation and operation.

---

# 🟦 No Administrator Rights Required

Per‑user MSI avoids:

- Writing to `C:\Program Files\`  
- Writing to `C:\ProgramData\`  
- Writing to HKLM registry keys  
- Creating system‑wide shortcuts  
- Running privileged CustomActions  

Because the installer never touches protected areas, Windows does not require elevation.

### Benefits

- Works in corporate environments  
- Works on locked‑down machines  
- Works under standard user accounts  
- Works with silent installs  
- No UAC prompt  
- No admin password required  

---

# 🟧 Silent Installation Support

Silent installation is fully supported:
msiexec /i nsfwmanager.msi /quiet /L*V install.log


Expected behavior:

- No UAC prompt  
- No privilege errors  
- Installation completes successfully  
- Log ends with **Return value 1** (success)

This is essential for:

- automated deployments  
- enterprise environments  
- script‑based installations  
- software management tools

---

# 🟦 No Privileged CustomActions

The MSI contains **no deferred CustomActions** that require elevation.

This avoids:

- rollback failures  
- commit‑phase crashes  
- Windows Installer error **1603**  
- permission issues during file operations  

All actions performed by the installer are safe for per‑user context.

---

# 🟩 Start Menu Shortcuts (Per‑User)

Shortcuts are created under:
%APPDATA%\Microsoft\Windows\Start Menu\Programs\NsfwManager\


This ensures:

- no access to `C:\ProgramData\Microsoft\Windows\Start Menu\`  
- no elevation required  
- shortcuts are visible only to the current user  
- compatibility with Windows 10 and 11

---

# 🟦 Registry Usage

NSFW Manager uses only **HKCU** (Current User) registry keys when needed.

It does **not** write to:

- HKLM  
- system‑wide COM registrations  
- privileged installer keys  

This keeps the installation lightweight and safe.

---

# 🟩 Uninstallation

Uninstallation also requires **no administrator rights**.

Windows removes:

- the application directory under `%LOCALAPPDATA%`  
- user configuration under `%APPDATA%`  
- Start Menu shortcuts under `%APPDATA%`  
- MSI registration under HKCU  

No privileged cleanup is needed.

---

# 📁 Log Locations

Installation‑related issues can be diagnosed using:
%APPDATA%\Roaming\NsfwManager\logs\NsfwManager.log
%LOCALAPPDATA%\NsfwManager\logs\startup.log
%LOCALAPPDATA%\NsfwManager\logs\execution.log


These logs help identify:

- directory access issues  
- MSI initialization problems  
- engine loading errors  
- video decoding failures  

---

# 📌 Summary

NSFW Manager uses a **per‑user MSI** to ensure:

- no administrator rights required  
- no UAC elevation  
- no Program Files access  
- no privileged registry writes  
- no deferred CustomActions  
- full silent install support  
- reliable installation on all Windows environments  

This installation model is stable, secure, and fully compatible with modern Windows systems.

---









