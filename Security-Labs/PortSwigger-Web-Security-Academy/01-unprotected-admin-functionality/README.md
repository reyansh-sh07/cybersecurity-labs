# Unprotected Admin Functionality

**Platform:** PortSwigger Web Security Academy  
**Category:** Access control  
**Status:** Solved  
**Environment:** Intentionally vulnerable, isolated Web Security Academy lab

## Objective

Identify an exposed administrative interface and use the lab's access-control weakness to delete the user `carlos`, as required by the challenge.

## Vulnerability Overview

The application exposed administrative functionality at a predictable URL without enforcing authentication or authorization. The path was disclosed in the publicly accessible `robots.txt` file.

This is a **broken access control** issue: a user who should not have administrative privileges can directly access privileged functionality.

> Important distinction: `robots.txt` is a crawler directive, not an access-control mechanism. Its `Disallow` entry does not protect a URL. The vulnerability exists because the application fails to enforce access controls on the administrative endpoint.

## Methodology

### 1. Application reconnaissance

- Browsed the storefront and observed how application URLs changed between pages.
- Inspected product links and experimented with an invalid `product_id` value to understand how the application handled identifiers.
- Reviewed the account page as part of the initial application mapping.

These were exploratory checks; they did not establish a separate product-level or account-level vulnerability.

### 2. Discovering the administrative path

Requested the site's `/robots.txt` file and found the following directive:

```text
User-agent: *
Disallow: /administrator-panel
```

This revealed the path `/administrator-panel` as a location that crawlers were asked not to visit.

### 3. Testing access control

Navigated directly to `/administrator-panel` without authenticating as an administrator. The application displayed the administrative user-management page.

This confirmed that the administrative functionality was accessible without the required access control.

### 4. Verifying impact

Used the exposed user-management functionality to delete the lab's designated user, `carlos`.

PortSwigger displayed the lab's success confirmation, verifying that the challenge objective had been completed.

## Finding

| Attribute | Result |
|---|---|
| Weakness | Missing access control on administrative functionality |
| Discovery | Administrative path disclosed by `robots.txt` |
| Affected functionality | `/administrator-panel` |
| Access required | No administrator authentication in the lab |
| Demonstrated impact | Unauthenticated access to user-management functionality and deletion of `carlos` |
| Lab result | Solved |

## Security Impact

If a comparable weakness existed in a real application, an unauthenticated user could potentially reach privileged administrative operations. Depending on the functionality exposed, this could lead to unauthorized account changes, deletion of user data, or broader administrative compromise.

The exact impact depends on the actions available through the exposed interface.

## Root Cause

The application relied on the administrative URL being difficult to discover instead of enforcing access control on the server.

A hidden or unlinked URL is not a security boundary. Every privileged endpoint must independently verify that the requester is authenticated and authorized.

## Recommended Remediation

1. **Enforce authentication:** Require a valid authenticated session before serving administrative functionality.
2. **Enforce authorization server-side:** Verify the user's administrative role or permission on every privileged request, including direct URL access.
3. **Use deny-by-default access control:** Grant access only to explicitly authorized roles.
4. **Do not rely on obscurity:** Treat `robots.txt`, hidden links, and unpredictable URLs as non-security controls.
5. **Add security tests and logging:** Test direct access to administrative routes with unauthenticated and low-privilege accounts, and log denied or suspicious administrative requests.

## Key Takeaways

- `robots.txt` can reveal paths that a site owner does not want crawlers to visit.
- A `Disallow` directive does not prevent a person from requesting that path.
- Administrative routes must enforce authorization on the server, not merely hide navigation links.
- Directly requesting a privileged URL is an important access-control test.
- A successful exploit should be verified through an observable result, not just inferred from a page loading.

## Evidence

The lab was completed in PortSwigger Web Security Academy. Screenshots are not included in this repository commit; the write-up records the discovery, access-control test, impact verification, and successful lab result without publishing the supplied captures.

## Scope and Ethics

Testing was performed only against the intentionally vulnerable PortSwigger Web Security Academy lab environment. The demonstrated user deletion was part of the challenge objective and was not performed against a real service.
