# 07 — UNION Attack: Retrieving Data from Other Tables

## Objective

Use a UNION-based SQL injection vulnerability in the product-category filter to retrieve data from another database table.

## My Approach

1. Intercepted the site's HTTP requests using Burp Suite and inspected the parameters for a potential SQL injection point.
2. Identified the `category` filter as the parameter to test.
3. Appended a single quote (`'`) to the value and observed a server error. Then tested a pair of quotes (`''`), after which the page loaded normally. This behavior suggested that the input was being handled within a string context in the SQL query and that the parameter was a viable injection point.
4. Used a UNION-based query to determine the number of columns returned by the original query and to identify which columns accepted text.
5. Confirmed that the query returned two columns and that both could accept text.
6. Used the UNION injection to query the `username` and `password` fields from the `users` table.

The lab was completed when the response exposed the requested data from the other table.

## Vulnerability

The application incorporated user-controlled category input into a SQL query without safely parameterizing it. This allowed the input to alter the query structure and combine the original result with data retrieved from another table.

## Key Learning

- A quote-based probe can help identify whether input is being placed inside a SQL string.
- UNION-based injection requires matching the original query's column count and compatible data types.
- If a query is injectable, its impact may extend beyond the original table or feature to other accessible tables.

## Remediation

- Use parameterized queries / prepared statements.
- Do not concatenate untrusted input into SQL query strings.
- Apply least-privilege permissions to database accounts.
- Avoid returning database errors or sensitive query results to users.

## Evidence

Lab completion screenshot pending integration.
