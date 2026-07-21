# CyberDefenders Microsoft Cloud Investigations Writeups

### A structured series of CyberDefenders Premium (BlueYard) Microsoft Cloud forensics and threat hunting challenges focused on isolating identity breaches, privilege escalation, and hybrid cloud compromise. Designed around real-world SOC workflows, each project documents the analytical process required to identify, investigate, and scope security events through timeline reconstruction, Kusto Query Language (KQL) analysis, and MITRE ATT&CK mapping.

## Topics span identity-based attacks, service principal abuse, and developer perimeter compromise. Investigations leverage Microsoft 365 Unified Audit Logs (UAL), Entra ID telemetry, and Azure Activity Logs to trace attack vectors across the Microsoft stack, including password spraying, Entra ID privilege escalation, and multi-region cloud pivoting.

---

## Investigations

| # | Investigation | Platform | Focus | Status |
|---|--------------|----------|-------|--------|
| 01 | AzureSpray | CyberDefenders | Azure AD Sign-in Logs, Sentinel Analytics, Password Spraying, Smart Lockout bypass | In Progress |
| 02 | DynamicEscalate | CyberDefenders | M365 UAL, Exchange Message Traces, Entra ID Privilege Escalation, KQL | Planned |
| 03 | Rogue Azure | CyberDefenders | Entra ID, Azure Storage Blobs, Service Principal abuse, Data exfiltration, KQL | Planned |
| 04 | CursorJack | CyberDefenders | Hybrid Forensics, MCP/Deeplink abuse, Developer workstation compromise, Multi-region pivoting | Planned |

---

## Report Structure

Each investigation follows a consistent format:

```text
Overview        — scenario background and scope
Methodology     — step-by-step analytical approach
Attack Chain    — chronological event reconstruction
IOCs            — IPs, hashes, domains, file names
ATT&CK Mapping  — tactic and technique identification
Investigation   — evidence, artifacts, KQL queries, and answers
```

---

## Platform

[CyberDefenders CyberRange Challenges](https://cyberdefenders.org/)
