# Upgrading NSFW Manager
## What Happens to Your Data When You Install a New Version

NSFW Manager uses a **per-user installer**. Upgrading is as simple as running the new installer — there is nothing to uninstall first.

---

## How to Upgrade

1. Download the new version's `.msi` installer from the official download page
2. Run it — the installer detects the existing installation automatically
3. The old version is removed and the new version is installed in its place
4. All your settings, licence, and quarantined files are preserved

The installer does not support downgrading. If you attempt to install an older version while a newer one is present, the installer will stop with an error message. To downgrade, uninstall the current version first, then install the older one.

---

## What Is Preserved During Upgrade

NSFW Manager stores your data in two locations that are completely separate from the application binaries:

| Data | Location | Preserved on upgrade |
|---|---|---|
| Settings (NsfwManager.ini) | `%APPDATA%\NsfwManager\` | Yes — always |
| Licence (NsfwManager.lic) | `%APPDATA%\NsfwManager\` | Yes — no re-activation needed |
| Quarantine sessions | `%LOCALAPPDATA%\NsfwManager\Quarantine\` (default) | Yes — not managed by the installer |
| Scan cache (scan_cache.db) | `%LOCALAPPDATA%\NsfwManager\` | Yes — survives unless engine changes |

**Why settings and licence survive:** They are stored in `%APPDATA%`, which is a user data folder that the installer does not touch. The MSI only manages files it explicitly registered during install — runtime-created files are never removed.

**Why quarantine survives:** The quarantine folder (`Quarantine\`) is created at runtime, not during installation. The MSI uninstall step removes only files it installed; it does not touch folders your data created.

---

## Scan Cache After an Upgrade

The scan cache stores AI scores per file to avoid re-scanning unchanged content. After an upgrade:

- If the AI engine models are unchanged between versions, the cache is fully reused — previously scanned files skip the AI step immediately on the next scan
- If the engine models have changed (typically in major version bumps), the cached scores are automatically invalidated on first scan. Files are re-scanned and the cache is rebuilt progressively. This is transparent — you will notice the first scan is slower than usual

You do not need to clear the cache manually after upgrading. The cache validation logic detects model changes automatically.

---

## Licence After Upgrade

Your licence does not need to be re-entered after an upgrade. The licence file (`NsfwManager.lic`) is stored in `%APPDATA%\NsfwManager\` and is preserved across all upgrades.

**Annual subscriptions:** If your subscription expired between the old version and the new one, you will be prompted to renew when NSFW Manager starts, but this is a billing issue rather than an upgrade issue.

**Lifetime licences:** A Lifetime licence covers a specific **major version** (the first digit of the version number). If you upgrade from major version 2 to major version 3, the Lifetime licence for v2 does not cover v3. The Licence Management dialog will display the covered major version for your key.

---

## Re-Running the First-Run Configuration

Upgrading does not re-trigger the first-run configuration dialog. Your existing directories (scan folder, quarantine folder, engines, thresholds) are loaded from the saved configuration.

If you want to reset to defaults or reconfigure from scratch: open **Configuration** and adjust settings manually, or delete `%APPDATA%\NsfwManager\NsfwManager.ini` before launching the new version (this will trigger the first-run wizard).

---

## Uninstalling

To fully remove NSFW Manager:

1. Go to **Settings → Apps** (Windows 11) or **Control Panel → Programs** (Windows 10)
2. Find **NSFW Manager** and select Uninstall

The uninstaller removes the application binaries from `%LOCALAPPDATA%\NsfwManager\`. It does **not** delete:

- Your settings (`%APPDATA%\NsfwManager\`)
- Your licence (`%APPDATA%\NsfwManager\NsfwManager.lic`)
- Your quarantine sessions (if stored at the default location)
- The scan cache

To remove these, delete the `%APPDATA%\NsfwManager\` and `%LOCALAPPDATA%\NsfwManager\` folders manually after uninstalling.

---

## Related Pages

- [Installation](./installation.md) — fresh install steps and path reference
- [Licence and Activation](./licence.md) — trial, paid, and Lifetime licence details
- [Licence Management Panel](../ui/licence-panel.md) — viewing your current licence status
