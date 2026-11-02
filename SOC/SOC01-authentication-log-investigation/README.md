# SOC01 — SSH Authentication Log Investigation

<em>First self-designed SOC investigation using a self-generated synthetic dataset.</em>

## Project Disclosure

**This is a learning simulation, not a real security incident.** I created the authentication-log dataset myself using synthetic SSH log entries, then investigated it in Ubuntu. No production server, real organisation, or real user account was involved.

This was my first SOC-style investigation project. I designed the exercise and used GPT as a learning assistant while developing the dataset, exploring Linux commands, and reviewing my initial analysis. The observations and conclusions below are based on the displayed synthetic log sample.

## Scenario

A Linux server's SSH authentication log contains a mixture of successful and failed login attempts. The task is to identify unusual authentication patterns, prioritise source IP addresses for investigation, and decide what additional evidence a SOC analyst should collect before concluding that an account has been compromised.

## Objectives

- Inspect SSH authentication events in a Linux log file.
- Distinguish successful authentication from failed attempts.
- Count failed attempts by username.
- Correlate activity by source IP and account.
- Identify suspicious sequences, including a successful login following repeated failures.
- Form a cautious incident assessment and recommend follow-up actions.

## Environment and Tools

- Ubuntu terminal (WSL environment)
- Bash
- `grep`, `awk`, `sort`, `uniq`, and `wc`
- Synthetic file: `auth.log`

## Dataset

The sample contains **18 SSH authentication events**.

The source addresses use documentation-only IPv4 ranges (including `192.0.2.0/24`, `198.51.100.0/24`, and `203.0.113.0/24`). They are placeholders, not real attacker or victim infrastructure.

The sample includes successful public-key authentication, successful password authentication, and failed password attempts against several usernames.

## Investigation Workflow

### 1. Create and inspect the log file

I created a working directory, wrote the synthetic SSH events into `auth.log`, and inspected the contents in the terminal.

### 2. Establish the dataset size

```bash
wc -l auth.log
```

**Observed output:** 18 lines.

### 3. Count failed attempts by username

```bash
grep "Failed password" auth.log | awk '{print $(NF-5)}' | sort | uniq -c
```

This extracts the username field from the matching SSH log entries, sorts the values, and counts each distinct username.

Observed counts:

| Username | Failed attempts |
|---|---:|
| deploy | 5 |
| root | 3 |
| admin | 2 |
| guest | 2 |
| oracle | 1 |
| test | 1 |
| **Total** | **14** |

### 4. Review successful password authentications

```bash
grep "Accepted password" auth.log
```

The sample shows successful password authentication for `deploy` from `203.0.113.77` and for `backup` from `192.0.2.45`.

Successful public-key authentication for `alice` is a separate event type and is not returned by the `Accepted password` filter.

### 5. Pivot on the deploy account

```bash
grep "deploy" auth.log
```

This reveals repeated failed password attempts for `deploy`, followed by a successful password login from the same source address, `203.0.113.77`, at 08:16:02. A later failed attempt for the same account is also present.

## Key Findings

### Finding 1 — Highest-priority source: 203.0.113.77

The source generated failed attempts against multiple usernames, including `admin`, `root`, `test`, `deploy`, and `oracle`. Most importantly, the log records a successful password authentication for `deploy` after repeated failures from this source.

**Assessment:** This is the strongest lead in the sample and is consistent with password guessing followed by a successful login. The log alone does not prove that the login was unauthorised or that the account was compromised.

**Priority:** High — validate the `deploy` login and investigate post-authentication activity.

### Finding 2 — Repeated failed attempts from 198.51.100.25

This source generated failed attempts for `guest` and `root`. No successful authentication from this address appears in the supplied sample.

**Assessment:** The pattern warrants review, but failed attempts alone do not establish a successful compromise.

**Priority:** Medium — check for additional events and whether the source is expected.

### Finding 3 — Successful backup login from 192.0.2.45

The log records a successful password login for `backup` from this address.

**Assessment:** A successful login is not automatically malicious. Its legitimacy cannot be determined without knowing whether this source and account are expected for backup operations.

**Priority:** Verify against the server's authorised backup schedule and source inventory.

### Finding 4 — Alice's public-key logins

The sample contains successful public-key authentication events for `alice` from `192.0.2.10`.

**Assessment:** No suspicious indicator is established by these entries alone. Confirm that the account, key, and source are expected.

## Incident Assessment

The leading hypothesis is password guessing against several accounts, with the `deploy` account requiring immediate validation because a successful password login followed repeated failures from the same source.

This remains a **hypothesis**, not a confirmed compromise. The dataset is small and synthetic, and it does not include process execution, privilege changes, command history, network connections, or file-integrity evidence.

## Recommended Follow-up

1. Validate whether the `deploy` login at 08:16:02 was expected.
2. Review subsequent SSH sessions and process activity for the `deploy` account.
3. Check authentication records for privilege escalation, such as `sudo` activity.
4. Review changes to SSH keys, user accounts, scheduled tasks, and sensitive files.
5. Correlate the source addresses with firewall, VPN, and network telemetry.
6. Verify whether the `backup` login matches an authorised backup host and schedule.
7. Preserve relevant logs and timestamps before containment actions.
8. If corroborating evidence indicates compromise, follow the organisation's incident-response process to contain the affected account or host.

Do not block an address or declare an account compromised solely from this small sample without considering operational context and corroborating evidence.

## Limitations

- The dataset is synthetic and contains only 18 events.
- There is no asset inventory or authorised-source baseline.
- There are no process, endpoint, firewall, VPN, or network-flow logs.
- The sample cannot establish attribution, intent, or actual compromise.
- The commands demonstrate basic log triage, not a complete SIEM investigation.

## What I Learned

- How SSH authentication events represent successful and failed logins.
- How to use `grep`, `awk`, `sort`, `uniq`, and `wc` for basic log analysis.
- Why counting failures is useful but insufficient without correlating source, account, and time.
- Why a successful login after repeated failures deserves more attention than a failed attempt alone.
- Why successful authentication must be validated against expected behaviour.
- How to distinguish observed evidence from an investigative hypothesis.

## Evidence

The two terminal screenshots selected for this project should be stored in `evidence/`:

- `01-synthetic-authentication-log.png` — creation and inspection of the synthetic SSH log.
- `02-cli-investigation-and-username-counts.png` — log line count, failed-attempt counts, successful logins, and the `deploy` pivot.

Screenshots of conversational feedback and planning are not included as technical evidence. Their useful lessons are incorporated into this report.

## Scope and Ethics

This exercise was performed locally using a self-created synthetic dataset. It did not involve scanning, accessing, or testing any external system. The project is intended for education and portfolio demonstration only.
