# SSL and Secure Licence Validation
## How NSFW Manager Protects Its One Network Request

NSFW Manager makes a single outbound network request: licence validation at startup. This page explains how that request is secured.

---

## Why Security Matters for Licence Validation

The validation request contains your email address, licence key, and a machine identifier. If transmitted over an unencrypted connection, anyone who can monitor your network traffic could intercept your licence key and potentially use it on another machine.

NSFW Manager uses multiple layers of protection to prevent this.

---

## HTTPS / TLS 1.2+

All communication with https://api.nsfwmanager.com/ uses HTTPS with TLS 1.2 or higher. This encrypts the entire request and response, preventing anyone monitoring network traffic from reading the contents.

NSFW Manager validates the server's TLS certificate before sending any data:
- Certificate chain is verified against trusted root CAs
- Hostname must match the certificate
- Certificate must not be expired

If certificate validation fails for any reason, the request is aborted and no data is sent. NSFW Manager falls back to using cached licence data.

---

## HMAC-SHA256 Payload Signing

In addition to TLS encryption, the server signs its responses using HMAC-SHA256. NSFW Manager verifies this signature locally before accepting the response.

This protects against:
- **Fake licence servers:** An attacker who intercepts DNS or routes traffic to a different server cannot produce a valid HMAC signature for the response
- **Tampered responses:** A modified response (for example, changing "expired" to "valid") would have an invalid signature and be rejected
- **Replay attacks:** Old valid responses cannot be replayed to trick the application into accepting a revoked licence

---

## What the Request Contains

The validation request includes:
- Email address
- Licence key
- A SHA-256 hashed machine identifier (the raw hardware data is hashed before sending)
- Application major version number

It does not include any file names, folder paths, detection results, or personal data beyond the licence information.

---

## Related Pages

- [Privacy](./privacy.md) — complete overview of what NSFW Manager does and does not transmit
- [Licence Security](./licence-security.md) — anti-tamper protections for the local licence cache
