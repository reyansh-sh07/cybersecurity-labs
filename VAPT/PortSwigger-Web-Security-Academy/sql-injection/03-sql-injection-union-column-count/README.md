# SQL Injection — UNION Attack: Determining the Number of Columns

**Platform:** PortSwigger Web Security Academy  
**Category:** SQL Injection  
**Status:** Solved

## Objective

Determine how many columns the original SQL query returns by testing a UNION-based injection in the product-category filter.

## My Approach

I started by examining the requests sent by the application in Burp Suite and looked for a parameter that might be incorporated into a SQL query. The category filter stood out, so I tested how it handled a single quote (`'`). The server returned an error, and comparing the behavior with a doubled quote (`''`) helped indicate that the input was being interpreted within a SQL string context.

With that initial clue, I moved on to UNION-based testing. I varied the number of selected values in the injected query until I found a structure that the application accepted. The successful test used three `NULL` values, indicating that the original query returned three columns. The lab's completion banner confirmed the result.

## Why It Worked

The vulnerable category parameter was inserted into the SQL query without safe parameterization. UNION requires the appended SELECT statement to return the same number of columns as the original query. Using `NULL` values is a useful initial technique because they can be compatible with many column data types.

## What I Learned

- Start by identifying parameters that influence database queries.
- Use controlled input probes and compare responses instead of jumping straight to complex payloads.
- UNION-based testing requires matching the original query's column count.
- `NULL` values can help test column compatibility without knowing each column's data type.
- A successful response is evidence of behavior; confirm the finding with the lab's completion state.

## How I Would Prevent It

- Use parameterized queries / prepared statements.
- Never build SQL query strings by concatenating user input.
- Allowlist valid category values where appropriate.
- Avoid exposing detailed database errors to users.

## Evidence

Burp request screenshots are intentionally omitted. Completion evidence will be added here when the screenshot is available in the repository.
