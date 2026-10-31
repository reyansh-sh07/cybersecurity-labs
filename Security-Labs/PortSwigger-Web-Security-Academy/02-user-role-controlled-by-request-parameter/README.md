# User Role Controlled by Request Parameter

**Platform:** PortSwigger Web Security Academy  
**Topic:** Access Control — Privilege Escalation  
**Difficulty:** Apprentice  
**Status:** Solved

## What was I trying to do?

The goal of this lab was to access the admin panel and delete the user `carlos`.

Unlike the previous lab, this time the description already gave me the admin panel path: `/admin`. It also provided login credentials for a normal user:

- **Username:** `wiener`
- **Password:** `peter`

The interesting part was that the lab mentioned a *forgeable cookie*. I didn't know exactly how that worked yet, so I started by logging in and inspecting what the browser was storing.

## How I approached the lab

### 1. I logged in with the provided credentials

I used the credentials from the lab description to log in as `wiener`.

Since the admin panel was already identified as `/admin`, I tried opening it. The application showed that the admin interface was only available to an administrator.

So, the URL itself wasn't the main problem. I needed to understand how the application decided whether I was an administrator or a normal user.

### 2. I inspected the browser's Network tab

I opened Developer Tools and checked the Network tab.

While inspecting the requests, I noticed a cookie named `Admin` with the value `false`. That caught my attention because it looked like the application might be using this cookie to decide the user's role.

At first, I tried to change the value from the Network tab. I then realised that simply inspecting or trying to edit the request details there wasn't changing the cookie stored by my browser.

### 3. I checked the Storage tab

I searched for where the browser stores cookies and found them under the **Storage** tab in Developer Tools.

There I could see the `Admin` cookie. I changed its value from:

```text
Admin=false
```

to:

```text
Admin=true
```

I left the session cookie alone. The important thing I was testing was whether changing the role-related cookie would affect the application's access decision.

### 4. I refreshed the page and checked the admin access

After changing the cookie, I refreshed the page and tried the admin panel again.

This time, the application treated my account as an administrator and allowed me to access the admin functionality.

That was the key finding: the application was trusting a role value that I could change in my own browser.

### 5. I deleted the requested user

Once I had access to the admin panel, I found the user `carlos` and used the Delete option.

The application confirmed the deletion, and PortSwigger displayed the lab completion message.

**Lab solved.**

## What actually went wrong?

The application was using a client-side cookie named `Admin` to decide whether the user had administrator privileges.

A browser cookie is stored on the client side, which means the user can inspect and modify it. The server should never treat a value supplied by the browser as proof that someone is an administrator.

In this lab, changing `Admin=false` to `Admin=true` was enough to gain access to the admin functionality. The application did not properly verify the user's role on the server.

This is an example of **Broken Access Control** and **vertical privilege escalation**.

- **Broken Access Control:** the application does not correctly enforce who can access a feature.
- **Vertical privilege escalation:** a normal user gains permissions meant for a more privileged user, such as an administrator.
- **Cookie tampering:** changing a cookie value to test whether the application trusts it.

## What I learned from this lab

1. Developer Tools can help me understand what information the browser sends to an application.
2. The Network tab helps inspect requests and responses, while the Storage tab lets me inspect browser-stored cookies.
3. A cookie value stored in the browser can be modified by the user, so it should not be trusted as proof of authorization.
4. A normal user should not become an administrator just by changing a value such as `Admin=false` to `Admin=true`.
5. Access control must be checked by the server, not decided only by a client-controlled value.

## How could this be prevented?

If I were securing an application like this, I would:

- Keep the user's role and permissions in trusted server-side session data.
- Avoid trusting client-controlled cookie values for authorization decisions.
- If cookies contain security-sensitive data, protect their integrity with a secure signing or integrity-validation mechanism. Signing alone is not a replacement for proper authorization checks.
- Verify the user's permissions on the server for every admin-only page and action.
- Test the application by changing client-side values and confirming that unauthorized users still cannot access privileged functionality.

## My takeaway

At first, I was looking at the Network tab and trying to change the value from there. I didn't immediately realise that the browser's Storage tab was where I could edit the cookie itself.

Once I found the `Admin` cookie, the idea became much clearer. I changed its value, refreshed the page, and saw that the application was treating my normal account as an administrator.

The biggest thing I learned here is that **a value coming from the browser cannot be trusted just because the application created it**. The server must independently verify what the user is allowed to do.

## Lab Evidence

The evidence folder is intended to contain the following screenshots. Cookie and session values must be redacted before publishing.

- `01-lab-description.png` — lab objective, admin panel path, and provided test credentials.
- `02-users-before-deletion.png` — the user list before deleting `carlos`.
- `03-admin-cookie-storage.png` — the `Admin` cookie in the browser Storage tab. Session-cookie values are redacted.
- `04-admin-access-check.png` — the admin access check before the cookie change.
- `05-lab-solved.png` — successful lab completion and deletion confirmation.

## Scope

This exercise was performed in the intentionally vulnerable PortSwigger Web Security Academy lab. The cookie modification and user deletion were performed only inside that authorized lab environment.
