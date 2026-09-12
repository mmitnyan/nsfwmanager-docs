# Licence Security
## How NSFW Manager Protects Licence Keys and Validates User Access

This document explains how NSFW Manager handles licence validation, what mechanisms protect against tampering, and what restrictions apply in trial mode.

---

## How Licence Validation Works

When NSFW Manager starts, it sends a validation request to the licence server at https://api.nsfwmanager.com/api/license/validate.php. The request includes:

- Your email address
- Your licence key
- A hashed hardware identifier (machine ID)
- The application's major version number

The server checks that the key is valid, active, not expired, and bound to the correct machine. It returns the licence status and expiry date.

No file names, file content, scan results, or folder paths are ever transmitted in this request. While using NSFW Manager without an active paid licence, a separate minimal anonymous telemetry ping is also sent at startup — see [Privacy](./privacy.md#anonymous-telemetry-ping-trial--unlicensed-use-only) for exactly what it contains.

---

## Locally Cached Licence

After a successful validation, the licence data is cached locally in your user profile. NSFW Manager uses this cache on subsequent startups to avoid requiring a network connection on every launch.

The cached file is protected by an HMAC-SHA256 signature. Any manual modification to the file breaks the signature, and NSFW Manager falls back to trial mode.

---

## Machine Binding

On first activation, the application computes a hardware fingerprint using a SHA-256 hash of machine-specific identifiers. This fingerprint is sent to the server and stored against your licence.

Every subsequent validation checks that the machine fingerprint matches the stored value. If it does not match (for example, if someone copies the licence file to a different machine), validation fails and trial mode applies.

To transfer your licence to a new machine, use the web portal to unregister the current machine before activating on the new one.

---

## Anti-Tamper Protections

**HMAC signature on local cache:** The cached licence file cannot be modified without invalidating the signature. Editing the file to change the expiry date or trial status causes the check to fail immediately.

**Machine ID verification in offline mode:** Even when offline, the machine ID stored in the cached licence is verified against the current hardware. Copying a licence file between machines is detected.

**Anti-clock-rollback:** If the system clock is moved back by more than 2 hours, NSFW Manager detects the discrepancy and reverts to trial mode. This prevents extending grace periods or trial resets by manipulating system time.

---

## Trial Mode Restrictions

When no valid licence is present, NSFW Manager operates in **trial (demo) mode**.

**Trial mode allows:**
- Unlimited scanning
- Full preview of results
- Full Configuration Panel access
- Quarantine Manager viewer

**Trial mode limits:**
- File actions (delete, move, quarantine, open directory) are limited to **5 per session**
- Once the limit is reached, action buttons are greyed out until the next application restart
- The counter resets on every launch

**Trial mode also sends:** a minimal anonymous telemetry ping at each startup (hashed random ID, app version, Windows version, licence tier). See [Privacy](./privacy.md) for details. This stops as soon as a paid licence is activated.

---

## Offline Access for Activated Users

There is an important distinction between two offline scenarios:

**Offline activation (new, never-activated key):** Not supported. First activation requires an internet connection to register the machine and verify the key against the server.

**Offline use (already activated):** Fully supported. If the licence server is unreachable at startup, NSFW Manager uses the locally cached licence data and grants full access for up to 30 days since the last successful online check. A warning is shown but functionality is not restricted.

This design ensures that legitimate paying users are never locked out by temporary network issues, travel, or server maintenance.

---

## Licence States at Startup

| State | Behaviour |
|---|---|
| Valid, server reachable | Full access; local cache updated |
| Valid, server unreachable | Full access via cached data (up to 30 days) |
| Grace period (expired 1–15 days ago) | Full access; renewal reminder shown |
| Expired more than 15 days ago | Trial mode (5 actions/session) |
| Revoked or suspended | Trial mode; contact support message shown |

---

## Related Pages

- [Licence and Activation](../getting-started/licence.md) — user-facing activation guide
- [SSL and Secure Communication](./ssl.md) — how the validation request is protected in transit
- [Privacy](./privacy.md) — complete list of what is and is not transmitted
