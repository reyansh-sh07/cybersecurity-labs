# User ID Controlled by Request Parameter

**Platform:** PortSwigger Web Security Academy  
**Topic:** Access Control — Horizontal Privilege Escalation  
**Difficulty:** Apprentice  
**Status:** Solved

## What was the objective?

The goal was to access the account belonging to `carlos`, retrieve his API key, and submit it as the lab solution.

The lab description explicitly identified a **horizontal privilege escalation** vulnerability on the user account page. It also provided credentials for a normal account:

- **Username:** `wiener`
- **Password:** `peter`

![PortSwigger lab description and objective](./evidence/01-lab-description.jpg)

## How I approached the lab

### 1. I identified the type of vulnerability

The lab description already gave an important clue: the issue involved horizontal privilege escalation.

Horizontal privilege escalation happens when one user gains access to another user's resources or data at the same privilege level. In this case, I needed to access Carlos's account while logged in as Wiener.

### 2. I logged in using the provided credentials

I logged in with the supplied test credentials:

```text
Username: wiener
Password: peter
```

After logging in, I opened the account page. The URL looked like this:

```text
/my-account?id=wiener
```

The `id` parameter immediately stood out. It appeared to identify which user's account information the application should display.

### 3. I tested whether the user ID was properly restricted

Since the account identifier was supplied in the URL, I tested whether changing it would let me access another user's account.

I changed:

```text
/my-account?id=wiener
```

to:

```text
/my-account?id=carlos
```

The application displayed Carlos's account information, including the API key required by the lab.

This showed that the application was accepting a user-controlled identifier without properly checking whether the logged-in user was authorized to access the requested account.

### 4. I retrieved the API key and completed the lab

I used the API key displayed on Carlos's account page as the lab solution. PortSwigger then displayed the successful completion message.

**Lab solved.**

![Successful lab completion and Carlos account page; API key redacted](./evidence/02-carlos-account-access-redacted.jpg)

## What actually went wrong?

The application used the `id` request parameter to select an account, but it did not enforce ownership or authorization checks for that account.

Changing the identifier from `wiener` to `carlos` therefore exposed another user's private account data.

This is an example of:

- **Broken Access Control:** the application failed to enforce the intended access restrictions.
- **Horizontal Privilege Escalation:** one ordinary user accessed another ordinary user's account.
- **Insecure Direct Object Reference (IDOR):** a user-controlled object identifier was accepted without a sufficient authorization check.

## What I learned from this lab

1. URL parameters can control which records or resources an application retrieves.
2. A valid identifier does not mean the current user is authorized to access the corresponding resource.
3. I should test object identifiers with another test account in an authorized lab environment.
4. Authentication and authorization are different: being logged in does not automatically grant access to every user's data.
5. The server must verify resource ownership or permissions on every request.

## How could this be prevented?

- Derive the current user's identity from the authenticated server-side session instead of trusting a client-supplied user ID for self-service account pages.
- Enforce object-level authorization on every request that accesses or modifies a user's data.
- Use deny-by-default access-control rules.
- Test with multiple accounts to verify that one user cannot read or modify another user's resources.
- Treat unpredictable identifiers as defense-in-depth only; they do not replace authorization checks.

## My takeaway

The lab description gave me the vulnerability category, so I focused on understanding how the account page selected a user. After logging in as `wiener`, I noticed the `id=wiener` parameter in the URL. That was the key clue.

Changing the ID to `carlos` exposed his account information and allowed me to retrieve the API key required to solve the lab.

The main lesson is that **the server must check whether the authenticated user is allowed to access the specific account or object requested**. Simply changing an ID in the URL should never grant access to another user's data.

## Lab Evidence

The screenshots in the `evidence/` directory document the lab objective and successful completion. The API key shown on the account page has been redacted before publication.

## Scope

This exercise was performed only in the intentionally vulnerable PortSwigger Web Security Academy lab, using the test accounts and environment provided for the exercise.
