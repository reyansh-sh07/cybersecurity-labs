# SQL Injection — Database Version Disclosure (Oracle)

## Objective

The objective of this lab was to use SQL injection to identify the database type and retrieve its version information. This helped me practise using a UNION-based query to extract data from a database in a controlled PortSwigger environment.

## My Approach

I started by testing the category filter in Burp Suite Repeater. I added a single quote (`'`) to the category value to see whether the application would return an error. It did, which suggested that the input was being included in the SQL query in a way that could affect its syntax.

Next, I used UNION-based testing to understand the query's column structure and determine which column could display text. Once I had a compatible column, I used Oracle's `v$version` view to retrieve database version information.

The query returned the database details in the response, and the lab was marked as solved.

## Why This Worked

The category value was being handled unsafely as part of the SQL statement. By introducing SQL syntax into that input, I was able to alter the original query and append a second SELECT statement with UNION.

Oracle provides the `v$version` view for version information. The UNION query allowed the application's response to display data from that view, rather than only the normal product-category results.

## What I Learned

- A single quote can be a useful first test for SQL injection when the input is placed inside a quoted SQL string.
- UNION-based injection depends on matching the original query's column count and compatible data types.
- Databases have their own system views and syntax, so identifying the database type matters during testing.
- Error responses and normal page output can both provide clues about how a query is structured.

## How I Would Prevent It

- Use parameterized queries / prepared statements so input is treated as data, not SQL syntax.
- Avoid constructing SQL statements through string concatenation.
- Restrict database permissions so application accounts can access only what they need.
- Handle database errors safely and avoid returning detailed SQL errors to users.

## Evidence

### UNION query and database version response

![Burp Repeater showing the UNION query and Oracle version response](./evidence/01-burp-union-oracle-version.png)

### Lab completion

![PortSwigger lab marked as solved](./evidence/02-lab-solved.png)
