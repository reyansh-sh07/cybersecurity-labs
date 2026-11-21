# SQL Injection — WHERE Clause Allowing Retrieval of Hidden Data

**Platform:** PortSwigger Web Security Academy  
**Category:** SQL Injection  
**Status:** Solved  
**Difficulty:** Apprentice

## Objective

Test whether the shop's product-category filter was vulnerable to SQL injection, then use the weakness to reveal products that were hidden by the normal filter.

## My Approach

I started by looking at how the product category filter worked and checked the request in Burp Suite. This helped me identify the `category` parameter as the input that controlled which products were displayed.

To see how the application handled that input, I first added a single quote (`'`) to the category value. This was a simple first probe: if the value was being inserted into a SQL query without proper handling, the quote could interfere with the query's syntax.

After that, I tested a boolean-based condition:

```
Pets' OR 1=1--
```

The logic behind this was straightforward. `1=1` is always true, so adding it with `OR` can make the filter match rows regardless of the original category condition. The `--` marker comments out the rest of the SQL statement in the context of this lab, helping prevent the remaining query from interfering with the test.

The response showed products outside the normal category results, including hidden items. That indicated the input was changing the database query's logic rather than being handled strictly as a category value. The lab's completion message then confirmed the exercise was solved.

## Why It Worked

The vulnerable filter likely used the category value directly in a SQL `WHERE` clause. A simplified example might look like this:

```sql
SELECT * FROM products
WHERE category = 'Pets' AND released = 1
```

If user input is concatenated into a query, SQL syntax can escape the intended value and alter the condition. In this test, the always-true condition changes how the database evaluates the filter, allowing the application to return additional rows.

This is an illustrative query—not the application's confirmed source code.

## What I Took Away

- A quote can be a useful initial probe for checking how input interacts with a database query.
- Request parameters can be inspected and tested directly with Burp, rather than relying only on the browser interface.
- Boolean logic is central to understanding many SQL injection techniques: changing the condition can change which records are returned.
- SQL injection may produce a normal-looking response instead of an obvious database error.
- I want to keep practising how to form and interpret tests independently, instead of depending on hints.

## How I Would Prevent It

- Use parameterized queries or prepared statements so input is treated as data, not executable SQL.
- Avoid building SQL statements by concatenating user-controlled values.
- Validate category values against an allowlist of valid categories.
- Use a database account with only the permissions the application requires.
- Add tests for database-backed filters and other user-controlled query features.

## Evidence

Evidence images will be added manually to the `evidence/` folder. The placeholders below show where each screenshot belongs.

### Request and injection test

<!-- Add a sanitized Burp Repeater screenshot here. Redact session cookies and the unique lab hostname. -->

### Lab completion

<!-- Add the PortSwigger completion screenshot here. -->

## Scope

This exercise was completed in PortSwigger Web Security Academy's intentionally vulnerable training environment.
