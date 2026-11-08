# Wazuh SIEM Home Lab
## Overview
Built a 3-VM SOC lab (Wazuh SIEM, RHEL agent, Kali attacker) on VMware Fusion
to practise detection and alert triage.

The story of the lab: Kali (the attacker) attacks rhel-agent (the victim). The Wazuh agent on rhel-agent
sends the logs to wazuh-server. You watch the alerts appear in the dashboard from your Mac, then write your
own detection rules and document everything.

## Architecture
<img width="845" height="131" alt="Screenshot 2026-11-08 at 14 20 55" src="https://github.com/user-attachments/assets/74f2566c-73cb-4b01-8277-c34b01d40815" />

## Tools
Wazuh 4.14, RHEL 9, Kali Linux, Hydra, VMware Fusion, MITRE ATT&CK
## What I did
1. Deployed Wazuh all-in-one on RHEL 9 and enrolled a Linux agent
2. Simulated 4 attacks: SSH brute force, new user, sudo use, file changes
3. Investigated each alert in the dashboard (rule ID, level, source IP, MITRE)
4. Enabled real-time File Integrity Monitoring
5. Wrote 3 custom rules (100010-100011- 100012)
