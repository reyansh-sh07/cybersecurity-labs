# 08 — UNION Attack: Retrieving Multiple Values in a Single Column

## Objective

Use a UNION-based SQL injection to retrieve multiple values through a single text-compatible column by concatenating fields from another database table.

## My Approach

1. **Request inspection:** Inspected the application's HTTP requests in Burp Suite to locate a parameter that could be tested for SQL injection.
2. **Identifying the injection point:** Found the `category` filter and appended a single quote (`'`). The server returned an error. I then tested a pair of quotes (`''), and the page loaded normally. This suggested the value was being handled inside a SQL string context and indicated a potential injection point.
3. **UNION testing:** Used UNION-based testing to determine the number of columns returned by the original query.
4. **Column compatibility:** Confirmed that the query returned two columns and that only the second column was text-compatible.
5. **Concatenation:** Since the required username and password values needed to be retrieved through one text-compatible column, I concatenated them using a separator (`~`).
6. **Data retrieval:** The response returned the values in the `username~password` format. I used the retrieved password to log in and complete the lab.

The lab was completed after successfully retrieving the credentials and authenticating.

## Vulnerability

The application incorporated user-controlled category input into a SQL query without safely parameterizing it. The UNION injection allowed data from another table to be returned through a compatible result column. Concatenation made it possible to combine multiple fields into one output value.

## Key Learning

- Quote-based probes can help identify whether an input is being handled inside a SQL string context.
- UNION-based injection requires matching the original query's column count and compatible data types.
- When only one column can display text, multiple values can be concatenated into a single output.
- SQL concatenation syntax varies between database systems.

## Remediation

- Use parameterized queries / prepared statements.
- Avoid concatenating untrusted input into SQL query strings.
- Apply least-privilege permissions to database accounts.
- Do not expose sensitive query results or database errors to users.

## Evidence

Lab completion screenshot pending integration.
