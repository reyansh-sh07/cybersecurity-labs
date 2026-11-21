# SQL Injection — UNION Attack: Finding a Column Containing Text

**Platform:** PortSwigger Web Security Academy  
**Category:** SQL Injection  
**Status:** Solved

## Objective

Identify which column in the original query can display text, then use a UNION query to return the lab-provided string.

## My Approach

I inspected the HTTP requests in Burp Suite and identified the category filter as a potential SQL injection point. I appended a single quote (`'`) and observed a server error. I then tried a doubled single quote (`''`); the page loaded normally. Comparing the responses suggested that the category value was being interpreted within a SQL string context, so I continued testing.

I used UNION-based tests to determine the original query's column count, then tested which column could display text. The lab supplied the marker string `dVsymE`. I found that the second column was text-compatible, returned the marker in that position, and the lab was solved.

## Why It Worked

A UNION-based injection must match the original query's column count and use compatible data types. The result becomes visible only when a returned value is placed in a column the application renders and that accepts the value's type.

## What I Learned

- Column-count discovery and data-type discovery are separate steps.
- A successful UNION structure does not mean every column can display text.
- A supplied marker string makes it easier to identify the text-compatible output position.
- Verify the impact with the lab completion state.

## How I Would Prevent It

- Use parameterized queries / prepared statements.
- Avoid concatenating request parameters into SQL.
- Allowlist valid category values.
- Restrict database privileges and avoid detailed error responses.

## Evidence

Burp screenshots are intentionally omitted. The lab-completion screenshot will be linked when its image file is available in the repository.
