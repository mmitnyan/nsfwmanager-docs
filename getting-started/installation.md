# Installation Guide
## How to Install NSFW Manager on Windows

NSFW Manager is distributed as a **Windows MSI installer**. The installation process is silent, requires no administrator rights, and completes in under a minute.

---

## System Requirements

- Windows 10 or Windows 11 (64-bit)
- Approximately 500 MB of free disk space
- Optional: a dedicated GPU for faster scanning (see [Performance Troubleshooting](../troubleshooting/performance.md))

---

## Download

Download the installer from:

**https://download.nsfwmanager.com/**

You will receive a file named 
sfwmanager-<version>-x64.msi.

---

## Installation Steps

### 1. Double-click the MSI file

The installer runs silently:
- No UAC prompt
- No administrator password required
- No configuration needed during installation

### 2. Installation location

NSFW Manager installs into:

`
%LOCALAPPDATA%\NsfwManager\
`

This is a folder inside your Windows user profile (C:\Users\<yourname>\AppData\Local\NsfwManager\). No files are written to C:\Program Files\ or anywhere that requires admin access.

### 3. Configuration and data

Your settings, licence, logs, and quarantine data are stored in:

`
%APPDATA%\NsfwManager\
`

This is separate from the application files so that updating NSFW Manager never disturbs your configuration.

### 4. Launch

After installation, find NSFW Manager in your Start Menu under NsfwManager. On first launch, the Configuration Panel opens automatically to let you set your scan directory, quarantine directory, and move directory.

---

## Antivirus and Security

NSFW Manager is distributed as a code-signed MSI. If your antivirus flags it, this is a false positive from the AI detection model files bundled with the installer (AI model weights sometimes trigger heuristic malware scanners). You can safely whitelist the installer and the application.

NSFW Manager connects to the internet for licence validation at startup, and — while used without an active paid licence — also sends a minimal anonymous telemetry ping (no files, filenames, or personal data; see [Privacy](../security/privacy.md)). No files are ever uploaded.

---

## Troubleshooting

### Installer errors 1925 or 1303
These errors occur only with older per-machine installers and should not appear with current versions. If they do, see [Windows Installer Errors 1925 and 1303](../troubleshooting/windows-errors-1925-1303.md).

### Silent installation
For automated deployment, see [Installation Model: No Admin Required](../architecture/msi-per-user.md#silent-installation).
