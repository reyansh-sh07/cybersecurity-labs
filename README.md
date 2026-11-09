# Cybersecurity Portfolio

A practical record of my cybersecurity learning, hands-on exercises, and independent investigations. This repository is organised by the type of security work being practised.

## Portfolio Areas

| Area | Focus |
|---|---|
| [Foundations](./Foundations/) | Linux, networking, operating systems, and other core concepts. |
| [VAPT](./VAPT/) | Vulnerability assessment and penetration-testing labs in authorised environments. |
| [SOC](./SOC/) | Security monitoring, log analysis, incident investigation, and detection-oriented projects. |

## VAPT

The VAPT section contains my completed PortSwigger Web Security Academy labs. These are guided exercises in a deliberately vulnerable training environment, not evidence of professional penetration-testing engagements.

| # | Lab | Topic | Status |
|---|---|---|---|
| 01 | [Unprotected Admin Functionality](./VAPT/PortSwigger-Web-Security-Academy/01-unprotected-admin-functionality/) | Broken access control | Solved |
| 02 | [User Role Controlled by Request Parameter](./VAPT/PortSwigger-Web-Security-Academy/02-user-role-controlled-by-request-parameter/) | Broken access control / privilege escalation | Solved |
| 03 | [User ID Controlled by Request Parameter](./VAPT/PortSwigger-Web-Security-Academy/03-user-id-controlled-by-request-parameter/) | IDOR / horizontal privilege escalation | Solved |
| 04 | [User Role Can Be Modified in User Profile](./VAPT/PortSwigger-Web-Security-Academy/04-user-role-can-be-modified-in-user-profile/) | Mass assignment / vertical privilege escalation | Solved |
| 05 | [Password Reset Broken Logic](./VAPT/PortSwigger-Web-Security-Academy/05-password-reset-broken-logic/) | Password-reset logic flaw / account takeover | Solved |
| 06 | [2FA Simple Bypass](./VAPT/PortSwigger-Web-Security-Academy/06-2fa-simple-bypass/) | Multi-factor authentication bypass | Solved |
| 07 | [SQL Injection — WHERE Clause Allowing Retrieval of Hidden Data](./VAPT/PortSwigger-Web-Security-Academy/07-sql-injection-where-clause-hidden-data/) | SQL injection / hidden data retrieval | Solved |

## SOC

The SOC section contains defensive-security investigations and scenario-based exercises. Projects clearly distinguish completed work from ongoing practice.

- [SOC01 — SSH Authentication Log Investigation](./SOC/SOC01-authentication-log-investigation/)
- [SOC02 — Compromised Windows Analysis](./SOC/SOC02-compromised-windows-analysis/)
- [SOC03 — Phishing Unfolding (in progress)](./SOC/SOC03-phishing-unfolding-simulator/)

## Foundations

Foundational exercises remain in the [Foundations](./Foundations/) directory.

## Documentation and Responsible Practice

Each project should explain its objective, environment, data sources, methodology, observations, findings, limitations, and lessons learned. Evidence is included only when it is useful and safe to publish.

All testing is limited to authorised labs or controlled environments. Synthetic datasets are explicitly labelled and are never represented as real incidents. Sensitive credentials, tokens, and private information should not be committed.
