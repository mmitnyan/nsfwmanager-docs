# Licence Management Panel
## Activating, Reviewing, and Recovering Your Licence

The **Licence Management** dialog lets you activate a paid licence, check your current licence status, and recover a lost key. Open it from **Help → Licence** in the menu bar.

---

## Current Status Section

The top section of the dialog always shows your current licence state with a color-coded indicator.

| Status text | Color | Meaning |
|---|---|---|
| Valid licence – no restrictions | Green | Active paid licence, server confirmed |
| Valid licence (offline mode) | Orange | Active licence, server temporarily unreachable — using cached data |
| Licence expired – grace period (X day(s) remaining) | Orange | Licence recently expired; full access continues during the 15-day grace period |
| Trial mode – X action(s) remaining this session | Orange | No valid licence; trial counter shows how many file actions remain |
| Licence expired – demo mode (X action(s) remaining) | Red | Licence expired beyond the grace period; trial mode applies |
| Trial limit reached – actions are disabled | Red | All 5 trial actions have been used for this session |

Below the status line, a detail line shows additional information when relevant:

- **Active paid licence:** licence type (Annual Subscription or Lifetime), renewal or expiry date, and covered major version (for Lifetime licences)
- **Offline mode:** a warning that the server could not be reached and offline grace is in effect
- **Grace period:** the exact number of days before full restrictions apply, with a renewal reminder
- **Expired licence:** the date on which the licence expired

---

## Activate a Licence Section

This section provides the form to enter and validate a new licence.

**E-mail:** The email address associated with your purchase.

**Key:** The 64-character hexadecimal key delivered to you when you purchased. The field is validated before submission — if the key is not exactly 64 characters, an error is shown immediately without a network request.

The fields are pre-filled with your current licence data if any has been previously entered. This makes re-validation or updating an expired licence faster.

**Validate button:** Sends your email and key to the licence server for verification. The button is temporarily greyed out during the request. On success, the status section updates immediately and the dialog closes. On failure, an error message explains the reason (invalid key, account not found, network error, etc.).

---

## Import Purchased Licence

The **Import purchased licence** button opens a secondary **Recover Licence** dialog. Use this if:

- You have lost your 64-character key and cannot find the purchase email
- You know your account email and portal password but do not have the key at hand

The Recover Licence dialog asks for your **email** and **portal password**. On submission, NSFW Manager calls the licence recovery API to retrieve your key. If credentials are valid and an active licence is found, the key is fetched and activation proceeds automatically — you do not need to copy and paste the key manually.

**Error cases:**
- Wrong email or password: "Authentication failed. Check your e-mail and password."
- No active licence on the account: "No active licence found for this account."

---

## Licence Portal Button

The orange **Licence Portal** button opens `login.nsfwmanager.com` in your browser, pre-filled with your current language and email. The portal is where you:

- Purchase a new licence or renew an existing one
- Manage machine registrations (transfer your licence to a new machine by unregistering the current one)
- View your purchase history and invoice

**Why machine transfer goes through the portal, not the dialog:** Unregistering a machine is a server-side operation that requires verifying your account credentials. It is intentionally not available directly in the application to prevent accidental de-registration.

---

## Contact Support Button

Opens the support form in your browser, pre-filled with your language and email. Use this if:

- Validation fails with an error you cannot resolve
- Your account shows no active licence but you have a purchase record
- You need to report a billing or licence issue

---

## What Happens After Successful Activation

When Validate succeeds:
- The status section immediately updates to "Valid licence – no restrictions" in green
- The licence data is saved locally (protected by HMAC signature against tampering)
- All file action restrictions are lifted for the remainder of the session and all future sessions while the licence remains valid
- No restart is required

---

## Related Pages

- [Licence and Activation](../getting-started/licence.md) — full guide to trial mode, paid plans, and offline behaviour
- [Licence Security](../security/licence-security.md) — anti-tamper protections and machine binding details
- [Privacy](../security/privacy.md) — what data is sent during validation
