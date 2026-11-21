# SQL Injection — Database Version Disclosure (Oracle)

## Objective

Identify the database platform and retrieve its version information through a UNION-based SQL injection in the product-category filter.

## My Approach

I started by inspecting the category-filter request in Burp Suite Repeater. To check how the input was handled, I appended a single quote (`'`) to the category value and observed an error response. I also tried a doubled single quote (`''`) as a comparison. The response behavior from these probes suggested that the category value was affecting the SQL query's syntax, so I continued testing the filter.

I then used UNION-based tests to determine the query's column count and which columns could display text. Once the output structure was understood, I looked up the Oracle-specific syntax in a SQL injection cheat sheet and used Oracle's `v$version` view to retrieve version details.

The version information appeared in the page response, and the lab completion banner confirmed that the lab was solved.

## Why It Worked

The category value was being included unsafely in the SQL statement. UNION allows a query to append results from another SELECT statement, provided the number of columns and their data types are compatible with the original query.

Oracle exposes database version information through the `v$version` view. Because the injected query could return data in a column rendered by the page, the database details became visible in the response.

## What I Learned

- Quote probes are useful starting points, but their results need to be interpreted in context.
- UNION-based SQL injection requires the appended query to match the original query's column count and compatible types.
- SQL syntax and system views vary by database, so identifying the database platform matters.
- The application's response can reveal information even when the page looks like a normal product listing.

## How I Would Prevent It

- Use parameterized queries / prepared statements.
- Avoid building SQL statements through string concatenation.
- Restrict the application's database permissions to only what it needs.
- Handle database errors safely and avoid returning detailed SQL error messages.

## Evidence

### UNION query and version response

![Burp Repeater showing the UNION query and Oracle version response](./evidence/01-burp-union-oracle-version.png)

### Lab completion

![PortSwigger lab marked as solved](./evidence/02-lab-solved.png)
