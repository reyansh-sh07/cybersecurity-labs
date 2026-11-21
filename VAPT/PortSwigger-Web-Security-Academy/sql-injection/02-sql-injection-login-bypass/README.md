# SQL Injection — Login Bypass

## Objective

The goal of this lab was to bypass the login mechanism by exploiting SQL injection in the username field. I wanted to see whether the input was being inserted into a database query without proper handling, and whether that could change how the query behaved.

## My Approach

I started by reading the lab description and focused on the login form, especially the username field. Since it was likely being used in a database query, I first tested whether a single quote (`'`) would break the input or change the application's response.

After identifying the injection point, I tried a login-bypass payload using the administrator username and SQL comment syntax. The idea was to make the database treat the rest of the query—including the password check—as a comment, rather than part of the condition it needed to evaluate.

I then used Burp Suite Repeater to inspect and send the request, so I could see how the server responded to the modified input. Once the bypass worked, I verified that I had reached the account as the administrator and confirmed that the lab was marked as solved.

## Why This Worked

The application was building its SQL query using user input without safely separating data from SQL syntax. The quote could change the structure of the query, while the comment marker could cause the remaining part of the query to be ignored.

In other words, the input was no longer being treated only as a username—it was influencing the query's logic.

## What I Learned

- Login forms can be vulnerable when user input is directly included in SQL queries.
- A small change in input can alter the meaning of a database query if input is not handled safely.
- Burp Repeater helps isolate and test a request without repeatedly interacting with the form.
- A successful login bypass is a practical example of how SQL injection can affect authentication, not just data retrieval.

## How I Would Prevent It

- Use **parameterized queries / prepared statements** so user input is handled as data, not executable SQL.
- Avoid building SQL queries by concatenating input strings.
- Use safe database access patterns consistently across the application.
- Return generic authentication errors and avoid exposing unnecessary details that help attackers test inputs.

## Evidence

I will add the screenshots manually to the `evidence/` folder and link them here once uploaded.

<!--
### 01 — Login bypass attempt / request
![Login bypass request](./evidence/01-login-bypass-request.png)

### 02 — Successful administrator login
![Administrator account after bypass](./evidence/02-administrator-login-confirmed.png)
-->
