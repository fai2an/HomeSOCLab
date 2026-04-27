# HomeSOCLab
Built a Home SOC Lab using VMware, Ubuntu Server  and Kali Linux. Deployed Wazuh SIEM v4.7.5,  configured endpoint monitoring, simulated SSH  brute force attack using ncrack, detected it  in real-time and mapped to MITRE ATT&amp;CK T1110.  Generated official security incident report.

I built this project to get hands-on experience with security monitoring. As someone looking to break into cybersecurity, I wanted to understand how a SOC analyst actually detects attacks, not just read about So I built my own mini SOC on my laptop.

# What I Built

A small lab with two virtual machines:

**Ubuntu Server** — runs Wazuh SIEM (the defender)
**Kali Linux** — plays two roles simultaneously:
  - Attacker — launches brute force attack against Ubuntu using ncrack
  - Agent — has Wazuh Agent installed which monitors and reports its own attacking activity back to the Wazuh Manager on Ubuntu

This dual role is what makes the lab interesting. Kali was essentially reporting its own attack to the victim. In a real SOC environment these would 
be separate machines , the attacker would be an external threat actor while the agent would be installed only on company endpoints being monitored.

I combined both roles in Kali to keep the lab running on a single 8GB laptop.

I deployed Wazuh on Ubuntu, connected Kali as a monitored endpoint, then attacked my own lab using ncrack to simulate a real SSH brute force attack.
Wazuh detected it. I analyzed it.

# Why I Did This

I kept seeing "SIEM" in Blue Team learning materials like tutorials and courses . Instead of just watching tutorials, I decided to build it myself. This project taught me more in one weekend than months of passive learning.

## Lab Setup

| Machine | Role | Specs |
|---------|------|-------|
| Ubuntu Server 25.04 | Wazuh Manager | 2GB RAM, 20GB |
| Kali Linux 2025.3 | Attacker + Agent | 2GB RAM, 80GB |

Both VMs run on VMware Workstation on a single 
Windows laptop with 8GB RAM.

# What I Did Step by Step

**1. Set up the virtual lab**
Installed VMware, created two VMs and configured them to talk to each other on an isolated network.

**2. Deployed Wazuh SIEM**
Installed Wazuh Manager, Indexer and Dashboard on Ubuntu Server. Accessed the dashboard from my Windows browser.

**3. Connected Kali as an endpoint**
Installed Wazuh Agent on Kali and connected it to the Manager. 
Seeing "Active Agent: 1" on the dashboard for the first time was genuinely exciting.

**4. Simulated a brute force attack**
Created a password wordlist and used ncrack to hammer Ubuntu's SSH service with login attempts.

**5. Watched Wazuh detect it live**
Within seconds the dashboard lit up with alerts. Wazuh correctly identified it as a brute force attack and mapped it to MITRE ATT&CK T1110.


# What Wazuh Detected

| Rule | What Happened | Severity |
|------|--------------|----------|
| 5712 | Brute force confirmed | Level 10 |
| 5551 | Multiple failed logins | Level 10 |
| 5758 | Max auth attempts exceeded | Level 8 |
| 5710 | SSH login — wrong username | Level 5 |
| 5503 | Login failed | Level 5 |


# MITRE ATT&CK Mapping

The attack mapped to:
- **T1110 — Brute Force**
- **Tactic — Credential Access**

This was my first time seeing MITRE ATT&CK in action rather than just reading about it.

# What I Learned

- How a SIEM collects and analyzes logs in real time
- What brute force attacks look like from a defender's perspective
- How Wazuh rules work and what triggers them
- The difference between alert severity levels
- How to identify false positives (Rule 521 — 
  Kali's tools triggered a rootkit alert!)
- Why timezone differences matter in log analysis
- How MITRE ATT&CK maps to real attacks

## Challenges I Faced

- Ubuntu 25.04 wasn't officially supported by Wazuh — fixed using --ignore-check flag
- Wazuh Agent kept pointing to wrong Manager IP — fixed using sed command

I'm including these because troubleshooting is a big part of real SOC work.

---

## Files in This Repo

- `wazuh_report.pdf` — Official security events report exported from Wazuh
- `screenshots/` — Dashboard evidence of detection
- `HomeSOCLab_Guide.pdf` — A guide on how to setup the lab written by me

## Tools Used

VMware Workstation · Ubuntu Server · Kali Linux · 
Wazuh 4.7.5 · ncrack · OpenSSH
