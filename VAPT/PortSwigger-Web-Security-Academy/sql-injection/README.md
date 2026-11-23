# SQL Injection

PortSwigger Web Security Academy labs focused on SQL injection vulnerabilities.

## Labs

| Lab | Vulnerability focus | Status |
|---|---|---|
| [01 — WHERE Clause Allowing Retrieval of Hidden Data](./01-sql-injection-where-clause-hidden-data/) | SQL injection / hidden data retrieval | Solved |
| [02 — Login Bypass](./02-sql-injection-login-bypass/) | SQL injection / authentication bypass | Solved |
| [03 — UNION Attack: Determining the Number of Columns](./03-sql-injection-union-column-count/) | UNION-based SQL injection / column enumeration | Solved |
| [04 — UNION Attack: Finding a Column Containing Text](./04-sql-injection-union-finding-text-column/) | UNION-based SQL injection / data-type discovery | Solved |
| [05 — Database Version Disclosure (Oracle)](./05-sql-injection-database-version-oracle/) | UNION-based SQL injection / database fingerprinting | Solved |
| [06 — Database Version Disclosure (MySQL / Microsoft)](./06-sql-injection-database-version-mysql-microsoft/) | UNION-based SQL injection / database fingerprinting | Solved |
| [07 — UNION Attack: Retrieving Data from Other Tables](./07-sql-injection-union-retrieving-data-from-other-tables/) | UNION-based SQL injection / data extraction from another table | Solved |
| [08 — UNION Attack: Retrieving Multiple Values in a Single Column](./08-sql-injection-union-retrieving-multiple-values-single-column/) | UNION-based SQL injection / concatenating multiple values into one column | Solved |
| [09 — Blind SQL Injection with Conditional Responses](./09-blind-sql-injection-conditional-responses/) | Blind SQL injection / Boolean response oracle | Solved |
| [10 — Blind SQL Injection with Visible Error-Based Responses](./10-blind-sql-injection-visible-error-based/) | Blind SQL injection / error-based data extraction | Solved |
| [11 — Blind SQL Injection with Time Delays](./11-blind-sql-injection-time-delays/) | Blind SQL injection / time-based confirmation | Solved |

## Focus Areas

- Identify and test request parameters that influence SQL queries.
- Use quote-based probes and compare responses to guide testing.
- Practice UNION-based SQL injection in authorized labs.
- Understand column counts, data types, and DBMS-specific syntax.
- Explore blind SQL injection techniques using Boolean responses, errors, and timing signals.
- Document methodology, impact, and remediation.

All exercises are performed in authorized training labs.
