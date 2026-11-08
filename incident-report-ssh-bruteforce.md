# Incident Report: SSH Brute-Force Activity

## Summary

An SSH brute-force attack targeted the `rhel-agent` host, attempting to authenticate as `testuser` and potentially other accounts. The attack resulted in a successful SSH login after repeated failed password-authentication attempts.

## Timeline

| Event | Time (UTC) |
| --- | --- |
| First failed SSH authentication | `Nov 8, 2026 @ 12:58:33.497` |
| Brute-force alert generated | `Nov 8, 2026 @ 12:60:21.465` |
| Successful SSH login | ``Nov 8, 2026 @ 13:02:53.375`` |

> Replace the placeholder values with the corresponding timestamps from the authentication logs and alerting system. Use one timezone consistently throughout the report.

## Affected Asset

| Field | Value |
| --- | --- |
| Host | `rhel-agent` |
| Target user | `testuser` |
| Service | SSH |

## Indicators

| Indicator | Value |
| --- | --- |
| Attacker IP | `172.16.21.138` |
| Usernames tried | `testuser` |
| Failed attempts | `6` |
| Successful authentication | `testuser` from `172.16.21.138` |

## Analysis

This is a **true positive**. Multiple unsuccessful SSH authentication attempts from `KALI_IP`, followed by a successful login to the same affected asset, are consistent with password-guessing activity that culminated in valid-account access.

The activity maps to the following MITRE ATT&CK techniques:

| Technique | Rationale |
| --- | --- |
| [T1110 – Brute Force](https://attack.mitre.org/techniques/T1110/) | Repeated authentication attempts were made against SSH accounts. |
| [T1078 – Valid Accounts](https://attack.mitre.org/techniques/T1078/) | The successful SSH session used valid credentials for `testuser`. |

## Response

1. Block ``172.16.21.138` at the firewall, security group, and any relevant network controls.
2. Reset the `testuser` password and invalidate any active sessions or credentials associated with that account.
3. Review `/etc/passwd`, `/etc/shadow`, identity-provider records, and SSH authorized keys for newly created or altered accounts.
4. Review `sudo` logs and command history for activity performed by `testuser` after the successful login.
5. Preserve relevant authentication, SSH, firewall, and endpoint logs for investigation; determine whether any lateral movement or persistence occurred.

## Recommendations

- Disable SSH password authentication and require SSH key-based authentication.
- Add multi-factor authentication (MFA) for SSH access.
- Restrict SSH access by source IP, ideally through a VPN or bastion host allowlist.
- Enable and tune `fail2ban` (or equivalent controls) to rate-limit and block repeated failed SSH logins.
- Alert on successful SSH logins that follow repeated failures, especially from the same source IP.
