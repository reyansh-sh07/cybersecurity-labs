# Lab 10 — Blind SQL Injection with Visible Error-Based Responses

**Category:** SQL Injection  
**Technique:** Blind SQL Injection — Error-Based  
**Status:** Solved

## Objective

Exploit a blind SQL injection vulnerability by causing database errors that reveal information, then use the extracted administrator credentials to log in.

## My Approach

1. I inspected the application's HTTP requests using Burp Suite and looked for a place where user-controlled input was included in a SQL query.
2. I identified the `TrackingId` cookie as the injection point and tested it using quote-based probes (`'` and `''`) to understand how the input was handled.
3. Since the lab was based on visible error-based SQL injection, I used SQL expressions that triggered database errors when values were cast to an incompatible type.
4. I first extracted the administrator username, then used the same error-based approach to retrieve the administrator password from the `users` table.
5. I used the recovered credentials to log in as the administrator and confirm that the lab was solved.

## Key Learning

- Database error messages can leak sensitive data when raw errors are exposed.
- Error-based SQL injection can be used as an information extraction channel even when query results are not displayed normally.
- `CAST()` can trigger conversion errors that expose returned values in vulnerable applications.

## Remediation

- Use parameterized queries / prepared statements.
- Never expose raw database errors to users.
- Validate and handle untrusted cookie values as input.
