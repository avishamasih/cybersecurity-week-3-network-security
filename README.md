# Week 3 – Advanced Cybersecurity: Network Security Assessment & Traffic Investigation

## Project Title
Week 3 Cybersecurity Internship Practical Assessment – Network Security Assessment & Traffic Investigation

## Objective
This project documents a hands-on network security assessment performed as part of Week 3 of the Cybersecurity Internship program at **DG Interns Hub**. The objective was to discover live hosts and exposed services on an authorized lab network, perform basic and advanced Nmap scans, capture and analyze live traffic with Wireshark, correlate scan activity with packet-level evidence, identify and classify security findings, and apply and verify security hardening measures.

All work was performed strictly within an isolated, authorized virtual lab environment. No external or public IP addresses were scanned or tested at any point.

## Lab Environment

| VM Name | OS | Role | IP Address |
|---|---|---|---|
| Kali Linux | Kali Linux 2026.2 | Analyst / attacker machine (Nmap + Wireshark) | 10.0.2.15 |
| Ubuntu-Target | Ubuntu Server 26.04.1 LTS | Target machine | 10.0.2.3 |
| Gateway | VirtualBox NAT router | Network gateway | 10.0.2.2 |

Virtualization platform: **Oracle VirtualBox** (NAT network, 10.0.2.0/24)

## Tools Used
- **Oracle VirtualBox** – Virtualization platform for the isolated lab VMs
- **Kali Linux 2026.2** – Analyst/attacker machine, pre-loaded with Nmap and Wireshark
- **Ubuntu Server 26.04.1 LTS** – Target machine
- **Nmap** – Host discovery, port scanning, service/version detection, OS detection, NSE vulnerability scripts
- **Wireshark 4.6.6** – Live packet capture and protocol-level traffic analysis
- **draw.io (diagrams.net)** – Network topology diagram
- **GitHub** – Version control and documentation hosting

## Tasks Completed
- [x] Task 7 – Network Discovery & Basic Nmap Scanning
- [x] Task 8 – Advanced Nmap Security Assessment (-sV, -O, -sS, -sU, --script vuln)
- [x] Task 9 – Wireshark Network Traffic Capture
- [x] Task 10 – Nmap + Wireshark Investigation
- [x] Task 11 – Advanced Network Traffic Investigation
- [x] Task 12 – Vulnerability Assessment
- [x] Task 13 – Security Hardening & Before/After Testing
- [x] Task 14 – Final Mini Security Assessment

## Key Findings

| # | Finding | Risk |
|---|---|---|
| 1 | SMB (port 445) exposed on gateway | High |
| 2 | RPC endpoint mapper (port 135) exposed | Medium |
| 3 | Unidentified service on port 7070 | Medium |
| 4 | Ambiguous UDP services (67, 137, 4500) | Low |
| 5 | Vulnerability scan inconclusive (NAT gateway limitation) | Low |

During the Nmap SYN scan, Wireshark captured a distinct burst of TCP SYN and RST,ACK packets between Kali and the gateway (4,005 packets / 237 KB in the busiest conversation) — a clear network-level signature of port scanning activity.

## Security Improvements
As a controlled hardening demonstration on the Kali Linux machine's SSH service:
1. **Installed and enabled UFW** host-based firewall (`sudo apt install ufw -y`, `sudo ufw enable`)
2. **Added a firewall deny rule** for port 22 (`sudo ufw deny 22`)
3. **Stopped and disabled the SSH service** entirely (`sudo systemctl stop ssh`, `sudo systemctl disable ssh`)

## Before / After Results

| Security Issue | Before | After |
|---|---|---|
| Host firewall | UFW not installed / inactive | UFW active and enabled on startup |
| SSH port 22 | Open (Nmap confirmed) | Deny rule added at firewall level |
| SSH service | Running and exposed | Stopped & disabled — all 1000 scanned ports show no open services |

**Result:** A follow-up Nmap scan confirmed *"All 1000 scanned ports on 10.0.2.15 are in ignored states"* — the demonstrated SSH exposure was fully eliminated.

## Recommendations
1. Restrict SMB (445) and RPC (135) access to trusted internal hosts only
2. Investigate and disable any unidentified service (e.g. port 7070) that isn't justified
3. Prefer disabling unused services over relying on firewall rules alone
4. Enable a host-based firewall by default, with a deny-by-default inbound posture
5. Re-run scans after every configuration change to verify hardening took effect

## Conclusion
This assessment covered the full lifecycle of a basic network security assessment: discovery, enumeration, traffic analysis, vulnerability identification, and verified hardening. Nmap and Wireshark proved complementary — Nmap efficiently identified what was exposed, while Wireshark revealed how that exposure looked at the protocol level, valuable both for understanding attacker behavior and for verifying defensive controls.

## Repository Structure
```
Week-3-Cybersecurity/
├── 01-Nmap/
│   ├── Scans/
│   └── Screenshots/
├── 02-Wireshark/
│   ├── PCAP/
│   └── Screenshots/
├── 03-Vulnerability-Assessment/
├── 04-Hardening/
│   ├── Before/
│   └── After/
├── Network-Diagram/
├── Week-3-Report.pdf
├── Week-3-Presentation.pptx
└── README.md
```

---
**Author:** Avisha Masih
**Role:** Cybersecurity Intern
**Organization:** DG Interns Hub
**Submission Date:** 8th September 2026
