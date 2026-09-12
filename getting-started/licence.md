# Licence and Activation
## Trial Mode, Paid Licences, and How Activation Works

NSFW Manager uses a licence system that keeps full scanning capability available to everyone while limiting certain file actions in trial mode. This page explains what each mode can do, how to activate a paid licence, and how the system behaves when you are offline.

---

## Trial Mode

When no valid licence is present, NSFW Manager operates in **trial mode**.

**What trial mode allows:**
- Unlimited scanning of any number of files
- Full preview of detected files in the Properties Panel
- Full use of the Configuration Panel
- Full use of the Quarantine Manager viewer

**What trial mode limits:**
- File actions — delete, move, quarantine, and opening a file's directory — are limited to **5 per session**
- Once you reach 5 actions, the action buttons are greyed out until you restart the application
- The counter resets each time you launch the application

**Why 5 actions per session:** The trial is designed to let you evaluate the full detection workflow — scan a folder, review the results, and perform a few actions to understand how quarantine and delete work — without providing enough unlocked capacity to use the product as a full tool indefinitely. Five actions is enough to verify that everything works as expected for your use case.

**Scanning is never limited:** You can scan folders of any size in trial mode. The limitation is only on what you can do with the results.

**Anonymous telemetry while in trial:** As long as no active paid licence is present, NSFW Manager also sends a minimal anonymous ping at each startup — a hashed random ID, the app version, your Windows version, and your licence tier (always "trial"). No files, filenames, or personal data are ever included. This stops automatically once you activate a paid licence. See [Privacy](../security/privacy.md) for full details.

---

## Paid Licence

A paid licence removes the 5-action limit entirely. All file actions are unlimited for the duration of the licence period.

**Licence types:**
- **Annual subscription:** Renews automatically each year
- **Lifetime licence:** One-time purchase covering a specific major version of NSFW Manager

---

## How to Activate

1. Open Configuration → Licence (or Help → Licence from the menu)
2. Enter your email address and your 64-character licence key
3. Click Validate
4. The application verifies your key with the licence server
5. If validation succeeds, the licence is stored locally and full access is granted immediately — no restart required

Your licence key is sent to you by email when you purchase. If you have lost your key, you can recover it from the licence portal using your email and password.

---

## Machine Binding

Each licence key is bound to a specific machine on first activation. The application computes a unique identifier from your hardware and registers it with the licence server. Subsequent licence checks verify that you are on the same machine.

**Why machine binding exists:** It prevents one licence key from being used simultaneously on many different computers. One licence = one machine at a time.

**If you need to move your licence to a new machine:** Log in to the licence portal and unregister your current machine. You can then activate on the new machine.

---

## Offline Behaviour

NSFW Manager validates your licence at startup by contacting the licence server. Two separate situations arise when that contact fails:

**Already-activated users (network temporarily unavailable):**
Your licence data is cached locally after each successful validation. If the server is unreachable, NSFW Manager uses the cached data. Full access continues for up to **30 days** after the last successful online validation. A warning is shown but access is not restricted.

**Grace period (licence recently expired):**
If your paid licence has expired within the last **15 days**, NSFW Manager maintains full access and shows a renewal reminder. After 15 days past expiry, trial mode applies.

**Important distinction:** Offline *activation* (activating a new, never-used key without internet access) is not supported. If you are activating for the first time, you need an internet connection. Offline *use* for an already-activated licence is fully supported for 30 days.

---

## Security Mechanisms

The licence system includes several protections against tampering:

**HMAC-SHA256 verification:** The locally cached licence file is signed. Any manual modification to the file breaks the signature and causes NSFW Manager to fall back to trial mode.

**Machine binding check:** In offline mode, the machine identifier is still verified against the cached licence. Copying the licence file from one machine to another causes verification to fail.

**Anti-clock-rollback:** If your system clock is set back by more than 2 hours, NSFW Manager detects the rollback and reverts to trial mode. This prevents extending a grace period by manipulating the system time.

---

## Related Pages

- [Privacy](../security/privacy.md) — what data is sent during licence validation
- [Licence Security](../security/licence-security.md) — technical details of the security model
- [SSL and Secure Communication](../security/ssl.md) — how the validation request is protected
