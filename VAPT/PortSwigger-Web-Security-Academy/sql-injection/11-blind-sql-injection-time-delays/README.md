# Lab 11 — Blind SQL Injection with Time Delays

**Category:** SQL Injection  
**Technique:** Blind SQL Injection — Time-Based  
**Status:** Solved

## Objective

Exploit a blind SQL injection vulnerability by causing a database-side time delay and use the delay as evidence that the injected expression is being executed.

## My Approach

1. I inspected the HTTP requests sent by the application to identify a potential SQL injection point.
2. I found that the `TrackingId` cookie was vulnerable to SQL injection.
3. Since the application did not return useful query results or visibly change its response based on the injected query, I used a database time-delay function to test the injection.
4. I injected a PostgreSQL `pg_sleep()` call that caused a 10-second delay in the response.
5. The delay confirmed that the injected expression was being executed and the lab was solved.

Payload used in the lab:

```sql
TrackingId=x'||pg_sleep(10)--
```

## Key Learning

- Time-based blind SQL injection uses response timing as a signal when the application does not provide visible data or errors.
- The delay can confirm query execution without relying on response content.
- The `pg_sleep()` function is PostgreSQL-specific.

## Remediation

- Use parameterized queries / prepared statements.
- Treat tracking cookies as untrusted input.
- Avoid executing user-controlled data as part of SQL statements.
