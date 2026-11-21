# SQL Injection — WHERE Clause Allowing Retrieval of Hidden Data

**Platform:** PortSwigger Web Security Academy  
**Category:** SQL Injection  
**Status:** Solved  
**Difficulty:** Apprentice

## Objective

Check whether the product-category filter could be manipulated with SQL injection, then use the flaw to reveal products that the application normally hides.

## My Approach

I began with the category filter and inspected its request in Burp Suite. The `category` parameter stood out because it controlled which products appeared on the page, making it a logical place to start testing.

I first appended a single quote (`'`) to the category value and observed how the application responded. I then tried a doubled single quote (`''`) as a comparison, to see how the input behaved when quote characters were present. These probes, along with the response behavior, suggested that the parameter was interacting with the underlying SQL query and was worth investigating further.

Next, I tested a boolean condition. The idea was to make the original category condition match more broadly by adding an always-true expression. When the response displayed products beyond the normal category results—including hidden products—it showed that the filter's query logic had been altered. The lab completion banner confirmed the result.

## Why It Worked

The category filter was vulnerable because user input was able to affect the structure of the SQL query. A simplified version of the intended query might be:

```sql
SELECT * FROM products
WHERE category = 'Pets' AND released = 1
```

If the application builds this query by directly concatenating user input, a tester may be able to change how the `WHERE` condition is evaluated. In this lab, the altered condition returned additional rows instead of limiting results to the selected category and released products.

This is an illustrative query, not the application's confirmed source code.

## What I Took Away

- Start by identifying which request parameter controls the page's behavior.
- Quote-based probes can reveal useful changes in how input is handled, but they should be interpreted with the response—not treated as proof by themselves.
- Boolean logic can change the result set when input is improperly included in a SQL query.
- SQL injection can expose data that the application intended to hide.

## How I Would Prevent It

- Use parameterized queries or prepared statements so input is treated as data, not SQL.
- Avoid constructing queries by concatenating user-controlled strings.
- Validate category values against an allowlist of valid categories.
- Use a database account with only the permissions the application needs.
- Test database-backed filters for injection vulnerabilities.

## Evidence

I'll add the screenshots manually to the `evidence/` folder and place them below.

### Request and injection test

<!-- Add sanitized Burp screenshot here. Redact cookies and the unique lab hostname. -->

### Lab completion

<!-- Add PortSwigger completion screenshot here. -->

## Scope

This exercise was completed in PortSwigger Web Security Academy's intentionally vulnerable training environment.
