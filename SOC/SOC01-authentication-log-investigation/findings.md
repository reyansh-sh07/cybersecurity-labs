# SOC01 — Investigation Notes and Findings

**Dataset:** Self-generated synthetic SSH authentication log  
**Events:** 18  
**Environment:** Local Ubuntu terminal  
**Incident status:** Suspicious pattern identified; compromise not established

## Observed facts

- 14 failed password-authentication events appear in the sample.
- Failed attempts by username: deploy (5), root (3), admin (2), guest (2), oracle (1), test (1).
- Successful password authentication appears for deploy from 203.0.113.77.
- The deploy success follows multiple failed attempts for that account from the same source.
- A successful backup password login appears from 192.0.2.45.
- Successful public-key authentication appears for alice from 192.0.2.10.
- 198.51.100.25 produces failed guest and root attempts, with no success shown in this sample.

## Hypotheses

1. 203.0.113.77 may have performed password guessing, followed by a successful deploy login.
2. 198.51.100.25 may have performed username/password probing or password guessing.
3. The backup login may be legitimate, but requires validation against an approved backup source.

These are hypotheses only. The synthetic log does not prove attacker identity or account compromise.

## Prioritised investigation

| Priority | Lead | Next evidence to collect |
|---|---|---|
| High | deploy login from 203.0.113.77 | Session details, commands/processes, sudo events, key/account changes, outbound connections |
| Medium | guest/root failures from 198.51.100.25 | Broader authentication window, firewall/VPN logs, other hosts' logs |
| Verify | backup login from 192.0.2.45 | Backup schedule, host inventory, service-account ownership |
| Baseline | alice public-key logins | Approved SSH key and expected source host |

## Analyst conclusion

The deploy sequence is the highest-priority lead because it combines repeated failures with a subsequent successful password authentication from the same source. The correct next step is to validate the login and inspect post-authentication activity. The evidence provided is insufficient to declare a confirmed compromise.
