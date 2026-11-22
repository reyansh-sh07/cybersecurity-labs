# Lab 09 — Blind SQL Injection with Conditional Responses

**Category:** SQL Injection  
**Technique:** Blind SQL Injection — Conditional Responses  
**Status:** Solved

## Objective

Use a blind SQL injection vulnerability to determine the administrator's password by observing whether the application returns a recognizable response when an injected SQL condition is true.

## My Approach

1. I inspected the HTTP requests sent by the application using Burp Suite and looked for a request field that could be tested for SQL injection.
2. I identified the `TrackingId` cookie as the injection point.
3. I tested how the input behaved with single-quote and doubled-single-quote probes (`'` and `''`). The response behavior suggested that the cookie value was being incorporated into a SQL string context.
4. Since the application did not display query results or SQL errors, I shifted to Boolean conditions and used the application's conditional response as an oracle. A recognizable **“Welcome back”** message indicated that the injected condition evaluated to true.
5. I used a condition with `SUBSTRING()` to test the administrator password one character at a time. By changing the character being tested, I could infer the password from the application's true/false response behavior.

Example condition:

```sql
TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1)
FROM users WHERE username='administrator')='a
```

This tests whether the first character of the administrator's password is `a`. The response provides a Boolean signal rather than revealing the query result directly.

6. After determining the password, I logged in as the administrator and confirmed that the lab was solved.

## Key Learning

- Blind SQL injection can still be exploited even when database output and error messages are hidden.
- Conditional application behavior can act as a Boolean oracle.
- `SUBSTRING()` can be used to infer a value character by character.
- The vulnerable input was the analytics `TrackingId` cookie.

## Remediation

- Use parameterized queries / prepared statements instead of concatenating untrusted input into SQL.
- Treat analytics cookies and other client-controlled values as untrusted input.
- Avoid exposing application responses that reveal whether sensitive database conditions are true or false.
