# 2FA Simple Bypass

**Platform:** PortSwigger Web Security Academy  
**Topic:** Authentication — Multi-Factor Authentication (2FA) Bypass  
**Status:** Solved

## Objective

Access the lab account belonging to `carlos` without completing the second authentication factor.

## Methodology

### 1. Complete a normal login and 2FA flow

I started the lab and logged in with the credentials provided for `wiener`. I entered the supplied 2FA code and confirmed that the normal authentication flow allowed me to access the account.

### 2. Start a login as Carlos

I logged out and then submitted the credentials provided for `carlos`. The application accepted the primary credentials and displayed the 2FA verification page.

### 3. Refresh the 2FA page

While on the 2FA page, I refreshed the browser. I had intended to inspect the resulting requests in Burp Suite, but the application instead allowed me to reach Carlos's account page without completing the 2FA challenge.

The account page showed the username `carlos`, and the lab displayed its solved state.

## Finding

The application did not consistently enforce completion of the second authentication factor before granting access to the protected account.

The refresh itself was not necessarily the underlying vulnerability. It caused the browser to make another request, and the application failed to require successful 2FA verification before serving the protected account page.

## Security impact

An attacker who knows a victim's primary credentials may be able to bypass the second authentication factor and access the victim's account. This defeats the additional protection that 2FA is intended to provide.

## Root cause

The observed behaviour is consistent with an authentication-state or access-control enforcement flaw: the server allowed access to an authenticated resource before the 2FA step had been successfully completed.

The server must maintain a distinct, incomplete-authentication state after password verification and must not authorize protected resources until the second factor has been validated.

## How to prevent this vulnerability

- Treat password verification and 2FA verification as separate authentication stages.
- Do not issue a fully authenticated session until all required factors have been validated.
- Enforce the authentication state server-side on every protected request.
- Ensure refreshing, replaying, or directly requesting a protected URL cannot skip the 2FA step.
- Regenerate session identifiers after successful authentication and 2FA completion.
- Test alternate request paths, page refreshes, and direct navigation during the pending-2FA state.

## What I learned

1. A successful password check must not be treated as completion of multi-factor authentication.
2. Authentication state must be enforced on the server, not inferred from which page the browser displays.
3. Refreshing a page can generate a new request and expose weaknesses in how the server validates the current authentication state.
4. Protected resources should be tested directly while the 2FA challenge is still pending.

## Evidence

The lab-completion screenshot belongs in the `evidence/` directory.

Save the redacted screenshot as:

`01-lab-solved-redacted.png`

![PortSwigger 2FA simple bypass lab completion](./evidence/01-lab-solved-redacted.png)

Before uploading, redact the unique lab-instance URL from the browser address bar. Do not publish session cookies, authentication tokens, or other secrets.

## Scope

This exercise was performed only in the intentionally vulnerable PortSwigger Web Security Academy lab environment.
