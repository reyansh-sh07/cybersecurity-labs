# Cybersecurity Portfolio

A practical record of my cybersecurity learning, hands-on exercises, and independent investigations. This repository is organised by the type of security work being practised.

## Portfolio Areas

| Area | Focus |
|---|---|
| [Foundations](./Foundations/) | Linux, networking, operating systems, and other core concepts. |
| [VAPT](./VAPT/) | Vulnerability assessment and penetration-testing labs in authorised environments. |
| [SOC](./SOC/) | Security monitoring, log analysis, incident investigation, and detection-oriented projects. |

## VAPT

The VAPT section contains hands-on PortSwigger Web Security Academy labs organised by vulnerability category. These are training exercises in deliberately vulnerable environments, not evidence of professional penetration-testing engagements.

- [Broken Access Control — 4 labs](./VAPT/PortSwigger-Web-Security-Academy/broken-access-control/)
- [SQL Injection — 1 lab](./VAPT/PortSwigger-Web-Security-Academy/sql-injection/)

Authentication-related labs are currently retained at the PortSwigger academy directory root and will be categorised separately later.

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
