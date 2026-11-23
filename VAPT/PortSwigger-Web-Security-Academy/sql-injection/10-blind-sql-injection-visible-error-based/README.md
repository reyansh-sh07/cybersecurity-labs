# Lab 10 — Blind SQL Injection with Visible Error-Based Responses

**Category:** SQL Injection  
**Technique:** Blind SQL Injection — Error-Based  
**Status:** Solved

## Objective

Exploit a blind SQL injection vulnerability in the `TrackingId` cookie to leak the administrator's username and password through database error messages, then log in as the administrator.

## How I Approached It

1. I inspected the HTTP requests sent by the application using Burp Suite and looked for user-controlled values that might be included in a SQL query.
2. I focused on the `TrackingId` cookie and tested it with a single quote (`'`) and a doubled quote (`''`). The responses helped confirm that the cookie value was being handled inside a SQL string and that my input could affect the query.
3. The lab was based on **visible error-based SQL injection**, so I used a `CAST()` expression to intentionally trigger a database conversion error and make the database reveal information through the error response.
4. I first queried the `users` table to reveal the administrator username. Since the query could return multiple rows, I added `LIMIT 1` to retrieve one value at a time.
5. I repeated the same approach to extract the administrator password.
6. Finally, I used the recovered credentials to log in as `administrator` and confirm that the lab was solved.

## Payloads Used

I started by testing whether a conversion error could be triggered:

```sql
TrackingId=ogAZZfxtOKUELbuJ' AND CAST((SELECT 1) AS int)--
```

Then I used the error response to retrieve the administrator username (limiting the result to one row):

```sql
TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
```

After identifying the username as `administrator`, I extracted the password using the same pattern:

```sql
TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

The database error response exposed the selected value, which I used to log in as the administrator.

## Key Learning

- Blind SQL injection can still leak data when normal query results are not shown.
- Database errors can become an unintended information channel if they expose query details or returned values.
- `CAST()` can cause conversion errors that reveal information in a vulnerable application.
- Limiting a subquery to one row makes the extraction predictable when multiple records exist.

## Remediation

- Use parameterized queries / prepared statements instead of concatenating untrusted input into SQL.
- Do not expose raw database errors to users; log detailed errors server-side.
- Treat cookie values as untrusted input and validate them appropriately.

---

All testing was performed in the authorized PortSwigger Web Security Academy lab environment.
