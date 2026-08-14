# Per-User MSI: No Admin Rights Required
## How NSFW Manager Installs Cleanly on Any Windows User Account

NSFW Manager uses a **per-user MSI** installer. This means the entire installation happens inside your own user profile — no system-wide changes, no administrator access needed, no UAC prompt.

---

## Why Per-User Installation

Earlier versions of NSFW Manager shipped with a traditional per-machine installer. This caused problems for many users:

- Windows Installer errors 1925 and 1303 appeared when the user did not have administrator rights
- Silent installation (msiexec /quiet) failed
- UAC elevation prompts appeared even on machines where the user was a local admin

A per-user installer eliminates all of these problems because it never tries to write to protected system locations. The application files go into your user profile, which you always have full write access to.

---

## Where Files Are Stored

| Content | Location |
|---|---|
| Application files | %LOCALAPPDATA%\NsfwManager\ |
| Configuration and licence | %APPDATA%\NsfwManager\ |
| Logs | %APPDATA%\NsfwManager\logs\ |
| Start Menu shortcuts | %APPDATA%\Microsoft\Windows\Start Menu\Programs\NsfwManager\ |

All of these locations are in your user profile and are always writable without elevated privileges.

---

## Benefits for Different Users

**Home users:** Install without being prompted for a password or admin approval. Updates install the same way.

**Corporate / managed machines:** Per-user install works on locked-down machines where users cannot install to C:\Program Files\. IT administrators do not need to pre-authorize the installation.

**Multi-user machines:** Each user has their own copy of the application with their own configuration, licence, and quarantine data. One user's settings do not affect another's.

---

## Silent Installation

NSFW Manager supports fully silent installation for automated deployments:

`
msiexec /i nsfwmanager-<version>-x64.msi /quiet /L*V install.log
`

Expected behavior:
- No UAC prompt
- No dialog boxes
- Installation completes silently
- Log file is written to install.log

This is useful for software management tools, IT deployment scripts, or organizations rolling NSFW Manager out to multiple machines.

---

## Uninstallation

Uninstalling also requires no administrator rights. You can uninstall NSFW Manager from Windows Settings → Apps, or by re-running the MSI installer and choosing Remove.

Your configuration, scan cache, quarantine data, and licence file are not removed by the uninstaller. If you want to remove those as well, delete the %APPDATA%\NsfwManager\ and %LOCALAPPDATA%\NsfwManager\ folders manually after uninstalling.

---

## Troubleshooting Installer Errors

If you see **Windows Installer error 1925 or 1303**, see [Windows Installer Errors 1925 and 1303](../troubleshooting/windows-errors-1925-1303.md).
