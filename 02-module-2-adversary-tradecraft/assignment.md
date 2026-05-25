---
slug: module-2-adversary-tradecraft
id: fdlghjbfgcvx
type: challenge
title: 'Module 2: Adversary Tradecraft'
teaser: The ransomware kill chain mapped stage by stage — from initial access to encryption
  — with MITRE ATT&CK context and defender inflection points at each step.
notes:
- type: text
  contents: |-
    # 322 – Ransomware Ecosystem

    **Module 2 — Adversary Tradecraft**

    Welcome to Module 2. Where Module 1 mapped the ecosystem, this module puts you in the SOC chair: how do operators actually move through an environment, what telemetry catches them, and which detections produce signal versus noise?

    **This track has no virtual machines or hands-on labs.**
    Your work in each challenge is to read the content, engage with the detection logic, and answer the discussion questions.

    **Estimated module time:** 3–4 hours
    **Challenges in this track:** 8
tabs:
- id: nq4kalspfeaf
  title: Challenge
  type: service
  hostname: bookworm
  port: 8080
difficulty: ""
timelimit: 7200
lab_config:
  default_layout_sidebar_size: 0
enhanced_loading: null
---

# ⚠️ Before You Begin

> [!WARNING]
> **The reading content in the left column and the lab challenge in the right column work together.** Complete the reading of the instructions first, then complete the lab challenge before pressing **Next** at the bottom of this page.

---

# Task 1: Introduction & Kill Chain Overview

In this module you will complete **Module 2 of the Ransomware Ecosystem course** by reading and reflecting on **adversary tradecraft across the full kill chain** and engaging with **detection content, telemetry sources, and actor profiles relevant to your environment**.

## Task 1.1 – How to Use This Module

Module 1 mapped what ransomware is and the ecosystem around it. This module is for the engineer in the SOC chair: how do operators actually move through an environment, what telemetry catches them, and which detections produce signal versus noise?

The structure follows the kill chain end to end. For each phase you get:

**1 )** **The dominant techniques** — with MITRE ATT&CK identifiers and the specific tooling operators use in 2025–2026

**2 )** **The data sources** — where each technique surfaces in your telemetry

**3 )** **Example detection content** — Sigma rules and KQL/SPL/EQL queries you can adapt for your environment

**4 )** **Discussion questions** — operational prompts that connect the technique to your specific detection posture

> [!IMPORTANT]
> Read this module once linearly. Then take any single section and ask: *"Where in our environment would this signal land, who sees it first, and how long until that person can act?"* Detections without that operational answer are just decoration. Bring the gaps into Module 6's tabletop scenarios.

## Task 1.2 – The Kill Chain at a Glance

Modern ransomware operations follow a recognizable kill chain that maps cleanly to MITRE ATT&CK. Each stage in this module corresponds to one challenge.

| Stage | Challenge | What attackers do | Detection window |
|---|---|---|---|
| Initial Access | 2 | Phishing, valid creds, edge CVEs, help-desk vishing | Hours to days before dwell |
| Execution & Persistence | 3 | LOLBins, remote-access tools, scheduled tasks | Minutes to hours after access |
| Credential Access | 4 | LSASS dumping, Kerberoasting, DCSync, ADCS abuse | Minutes to hours post-execution |
| Discovery & Lateral Movement | 5 | AdFind, BloodHound, PsExec, Impacket | Hours to days during dwell |
| Defense Evasion | 6 | BYOVD, VSS deletion, log clearing, GPO tampering | Minutes before encryption |
| Exfiltration | 7 | Rclone, MEGAsync, custom binaries | Days before encryption |
| Encryption & Impact | 7 | Mass file rename, hypervisor targeting | Minutes — the ransom event |
| Actor Profiles & Assessment | 8 | Operational profiles for 2025–2026 active clusters | Reference |

**1 )** Encryption is the **last** stage. By the time the ransom note appears, the attacker has typically been present for days to weeks. Every earlier stage is a detection opportunity.

**2 )** The kill chain is not always linear. Credential access and discovery often happen in parallel. Defense evasion can begin as soon as the attacker has a foothold.

**3 )** The brand of ransomware matters less than the behavior. A detection portfolio built around kill-chain stages catches all brands — including new ones.

> [!NOTE]
> This module references MITRE ATT&CK technique IDs throughout. If you are not familiar with ATT&CK, the ATT&CK Navigator at attack.mitre.org/matrices/enterprise is a useful companion. You do not need to use it to follow this module — all techniques are explained in plain language.

## Task 1.3 – What Changed in 2023–2026

The kill chain above is not new. What changed in the 2023–2026 period matters to your detection investment:

**1 )** **Edge-device exploitation became the dominant initial-access vector.** VPN appliances, remote-management platforms, and file-transfer tools generated more confirmed ransomware intrusions than phishing in this period. Patching velocity on internet-facing systems is now the single highest-leverage preventive control.

**2 )** **Infostealer logs commoditised valid-credential access.** Credentials stolen by Redline, Lumma, Vidar, and StealC are sold on Russian Market within hours of harvest. An employee infected at home provides an IAB with a foothold into the corporate environment the next morning. Device Trust / Conditional Access policies that reject logins from unmanaged devices are the direct mitigation.

**3 )** **Adversary-in-the-middle phishing kits defeated push MFA at scale.** Evilginx, EvilProxy, and Tycoon relay authentication challenges live, harvesting session cookies in real time. Phishing-resistant MFA (FIDO2/passkeys) is the only categorical mitigation.

**4 )** **The affiliate model separated initial access from the locker.** A single affiliate may use LockBit one week and RansomHub the next. Detection content keyed to brand-specific locker IOCs misses this. Kill-chain behavioral detection does not.

> [!TIP]
> Keep these four shifts in mind as you read the subsequent challenges. Each one points to a specific gap in a traditional perimeter-focused detection portfolio.

---

# Task 2: Initial Access

In this module you will complete **Initial Access** by reading and reflecting on **the six dominant methods ransomware operators use to enter an environment** and engaging with **the telemetry and detection logic that catches each one**.

## Task 2.1 – The Six Initial-Access Families

Initial access is where the kill chain begins and where ransomware operators have the widest selection of methods. Six families dominate post-2023.

**1 )** **Phishing with malicious attachments or links**

HTML smuggling, ISO/IMG containers carrying LNK + DLL, OneNote with embedded scripts, malicious XLLs, and password-protected ZIPs holding stealers or loaders. The shift to non-Office containers directly reflects Microsoft's macro-blocking defaults from 2022 onward — operators adapted within weeks.

**2 )** **Valid accounts purchased from infostealer logs**

Browser-stored passwords, session cookies, and saved MFA tokens harvested by Redline, Lumma, Vidar, and StealC, then resold on Russian Market. Adversary-in-the-middle phishing kits (Evilginx, EvilProxy, Tycoon) defeat push-MFA in real time by relaying authentication challenges live and capturing the resulting session cookie.

**3 )** **Exploitation of public-facing applications**

The 2023–2026 wave of edge-device CVEs produced more confirmed ransomware intrusions than any other vector in that period:

- Citrix NetScaler: CVE-2023-3519 (unauthenticated RCE), CVE-2023-4966 "Citrix Bleed" (session token leak)
- Fortinet FortiOS / FortiManager: multiple critical auth-bypass CVEs
- Ivanti Connect Secure: CVE-2023-46805 / CVE-2024-21887 (auth bypass + RCE chain)
- Cisco ASA: CVE-2023-20269 (credential brute-force)
- ScreenConnect: CVE-2024-1709 (auth bypass)
- MOVEit Transfer: CVE-2023-34362 (SQL injection → RCE)
- GoAnywhere MFT, Confluence, ConnectWise

**4 )** **External remote services**

RDP and VPN endpoints exposed to the internet, accessed with reused or credential-sprayed passwords. Brute-forcing and credential-stuffing remain effective against any service without rate limits and MFA.

**5 )** **Trusted relationships**

Compromise of MSPs and SaaS providers to reach downstream customers — Kaseya (2021) and ConnectWise (2024) are the landmark cases. Federated identity (SAML, OIDC) trust chains are increasingly abused once an identity provider is compromised.

**6 )** **Social engineering of help desks and SSO**

Scattered Spider's signature technique: vishing the help desk, constructing a plausible identity verification, requesting an MFA reset, recovering an authenticator app. Requires no technical exploit — only a convincing caller. Microsoft's Octo Tempest write-up is the best public characterisation.

## Task 2.2 – Detection Priorities for Initial Access

| Technique | ATT&CK | Why ransomware uses it | Telemetry to watch |
|---|---|---|---|
| Phishing — malicious attachment | T1566.001 | ZIP/IMG/ISO containers with LNK+DLL or HTML+JS smuggling | Email gateway, EDR file-write events, MOTW propagation |
| Phishing — link | T1566.002 | AitM proxy URLs harvesting session cookies and MFA tokens | Email URL reputation, Conditional Access sign-in logs |
| Valid accounts | T1078 | IAB-sourced credentials, infostealer logs, password-spray results | Risky sign-in events, impossible travel, Tor / VPS auth |
| Exploit public-facing app | T1190 | Citrix Bleed-class auth bypass; web shell drop on edge appliance | WAF, NGFW IPS, edge-device telemetry, vendor IOC Sigma rules |
| External remote services | T1133 | RDP/VPN access with valid credentials | Authentication logs, geolocation anomalies, off-hours patterns |
| Trusted relationship | T1199 | Compromise via SaaS or MSP; federated SSO abuse | OAuth grants, federated identity logs, partner-tenant sign-ins |

> [!NOTE]
> The telemetry column describes where the signal lands — not necessarily where it is reviewed. The operational question is whether someone with authority to act sees that signal within a useful time window.

## Task 2.3 – Sample Detection: AitM Cookie-Replay Sign-In

Adversary-in-the-middle sign-ins produce a characteristic pattern: a successful authentication with no compliant registered device, often from an unusual geography. The Sigma rule below targets this in Azure AD / Entra sign-in logs.

```yaml
title: Suspicious Token Replay — Off-Geo and No Device
logsource:
  product: azure
  service: signinlogs
detection:
  selection:
    AuthenticationDetails|contains: "satisfied"
    DeviceDetail.IsCompliant: false
    DeviceDetail.TrustType: ""
    Status.errorCode: 0
  filter_known:
    UserPrincipalName|endswith: "@trusted.contractor.example"
  condition: selection and not filter_known
fields:
  - UserPrincipalName
  - IPAddress
  - Location
  - DeviceDetail.OperatingSystem
level: high
```

**What this catches:** A session-cookie replay from an attacker who completed AitM phishing. The real user authenticated; the attacker replayed the harvested cookie from a different device and IP. The absence of a compliant or registered device is the discriminating signal.

**What it misses:** Attackers using enrolled devices (via MDM bypass or device-code phishing) will not trigger this rule. Complement with sign-in velocity and impossible-travel rules.

> [!IMPORTANT]
> This rule requires Conditional Access sign-in logs shipped to your SIEM. If Device Compliance state is not populated in your logs, the rule will not fire. Verify the field mapping in your environment before deployment.

## Task 2.4 – Discussion Questions

Write your answers in your team discussion document before moving to Challenge 3.

**Q1.** Three of the six initial-access families (valid accounts, edge-device exploitation, trusted relationships) do not require the victim to click anything. What does that tell you about the role of security awareness training as a *primary* control for ransomware initial access in 2025–2026? What role should it play?

**Q2.** Your organisation uses a VPN appliance from a major vendor. A critical auth-bypass CVE is published on a Tuesday. Your patching SLA for internet-facing systems is 30 days. How long does your organisation actually have before active exploitation begins — and what does that gap mean for compensating controls while the patch is applied?

**Q3.** The AitM detection above requires Conditional Access sign-in logs shipped to your SIEM, Device Compliance state populated in logs, and a known-good filter list. Map those three requirements against your current environment. Which is missing, and what is the remediation?

---

# Task 3: Execution & Persistence

In this module you will complete **Execution & Persistence** by reading and reflecting on **how ransomware operators establish and maintain their foothold** and engaging with **the EDR telemetry and detection content that surfaces these behaviours**.

## Task 3.1 – Three Execution Patterns

The execution stage is where adversaries are most likely to leave noisy footprints — and most likely to disable the agents that would record them. Three patterns dominate.

**1 )** **Living-off-the-Land Binaries and Scripts (LOLBins / LOLBAS)**

Operators prefer tools already present on the victim system. Native Windows binaries produce less EDR noise than dropped executables and are harder to block without impacting legitimate administration.

Common LOLBins in ransomware intrusions:

- `PowerShell.exe` — cradles, IEX, encoded commands, AMSI bypass, Cobalt Strike beacon scripts
- `WMIC.exe` — remote process execution, persistence via `__EventFilter`
- `mshta.exe` — HTA-based payload execution
- `regsvr32.exe` — COM scriptlet execution (Squiblydoo)
- `certutil.exe` — base64 decode and download
- `bitsadmin.exe` — background download and execute
- `msbuild.exe`, `InstallUtil.exe`, `rundll32.exe` — code execution via trusted build tools
- `conhost.exe` — used as a parent-process spoofing target

The [LOLBAS Project](https://lolbas-project.github.io) tracks 200+ such binaries with known abuse patterns.

**2 )** **Legitimate Remote-Access Tools**

Operators install commercial remote-management software alongside or instead of malware so their traffic looks like normal IT activity. CISA has flagged this pattern in multiple ransomware advisories.

Commonly observed: AnyDesk, ScreenConnect, Atera, Splashtop, TeamViewer, NinjaRMM, Action1, MeshCentral.

The detection challenge: these tools are legitimate in many environments. Context matters — AnyDesk on a sysadmin laptop is normal; AnyDesk installed silently on a finance workstation at 02:00 by an unusual parent process is not.

**3 )** **Persistence Mechanisms**

Operators establish redundant persistence so that a single EDR alert or reboot does not end the intrusion:

- Scheduled tasks (T1053.005) — often masquerading as Windows system tasks
- Run keys (T1547.001) — `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
- Services (T1543.003) — installed via PsExec, Impacket, or custom binaries
- WMI subscriptions (T1546.003) — `__EventFilter` + `__EventConsumer` pairs
- Startup folder drops, IFEO debugger hijacks, COR_PROFILER, GPO modifications

## Task 3.2 – EDR Telemetry for Execution & Persistence

| Technique | ATT&CK | Why ransomware uses it | Telemetry to watch |
|---|---|---|---|
| PowerShell | T1059.001 | Cradles, encoded commands, AMSI bypass, beacon scripts | PowerShell ScriptBlock logs (EID 4104), Module logs (EID 4103) |
| WMI / WMIC | T1047 | Remote process execution, `__EventFilter` persistence | WMI-Activity log, Process Create with wmic.exe parent |
| Scheduled task | T1053.005 | Persistence + impersonation, masquerades as Windows tasks | Security EID 4698/4702, Task Scheduler operational log |
| Service install | T1543.003 | PsExec / Impacket smbexec / atexec, custom service binaries | System EID 7045, Sysmon EID 6 |
| Remote access tooling | T1219 | Atera, AnyDesk, ScreenConnect installed by attacker | Software-installed events, EDR new-binary first-seen |
| IFEO / Image File Execution Options | T1546.012 | Debugger key hijack, often pointing at cmd.exe | Registry writes to `HKLM\...\Windows NT\CurrentVersion\IFEO` |

> [!IMPORTANT]
> PowerShell ScriptBlock logging (EID 4104) is not enabled by default in Windows. If it is not enabled and shipping to your SIEM, an entire category of execution telemetry is invisible. Verify this is active in your environment before reading further.

## Task 3.3 – Hunt Query: Newly-Installed Remote-Access Tools

The following KQL query targets Microsoft Defender for Endpoint telemetry and hunts for remote-access tool binaries dropped on hosts that are not in the IT-Admin machine group — a high-signal indicator that an attacker installed a remote-management agent for persistence.

```kql
DeviceFileEvents
| where Timestamp > ago(7d)
| where FileName matches regex @"(?i)(anydesk|screenconnect|atera|splashtop|teamviewer|ninja|meshagent|connectwisecontrol).*\.(exe|msi)"
| join kind=leftouter (
    DeviceInfo
    | summarize arg_max(Timestamp, *) by DeviceId
    | project DeviceId, MachineGroup
  ) on DeviceId
| where MachineGroup !contains "IT-Admin"
| project Timestamp, DeviceName, MachineGroup, FileName, FolderPath,
          InitiatingProcessFileName, InitiatingProcessAccountName
| order by Timestamp desc
```

**What this catches:** Remote-access tool installers dropped on non-admin endpoints, particularly useful for identifying attacker-installed persistence tools during active intrusions or in retrospective threat hunting.

**Tuning notes:**
- Expand the regex pattern to include tools specific to your environment
- The `MachineGroup` filter requires your MDE device groups to be configured consistently
- Run this query as a scheduled detection rule, not just an ad-hoc hunt

> [!TIP]
> If you do not use MDE, the same logic translates to any EDR with file-creation telemetry. The key fields are: filename (regex match on known RAT names), host group or role, and initiating process. A PowerShell or cmd.exe parent process installing AnyDesk is a near-certain indicator.

## Task 3.4 – Discussion Questions

Write your answers in your team discussion document before moving to Challenge 4.

**Q1.** LOLBins are legitimate Windows tools. Blocking them often breaks administrative workflows. For your environment, identify **two LOLBins** from Task 01 that are used legitimately by your IT or security teams. For each one, describe what a detection rule would look like that catches attacker use but does not alert on legitimate use. What context (parent process, user account, time of day, destination host) would discriminate the two?

**Q2.** Scheduled tasks, Run keys, services, and WMI subscriptions are all persistence mechanisms. If an attacker establishes all four redundantly, what is the minimum number of independent detection rules required to guarantee at least one fires? Does your SIEM currently have that coverage?

**Q3.** The remote-access tool hunt query above requires Device Groups to be consistently configured in MDE and ScriptBlock logging to be enabled. Identify **one data-quality prerequisite** in your own environment that a detection rule depends on but that has never been formally validated. What would a validation test look like?

---

# Task 4: Credential Access & Privilege Escalation

In this module you will complete **Credential Access & Privilege Escalation** by reading and reflecting on **the techniques ransomware affiliates use to acquire domain-level credentials** and engaging with **the detection priorities that catch each one**.

## Task 4.1 – Why Credential Access Determines Blast Radius

Once inside, ransomware affiliates need higher privileges to disable defences, reach backup infrastructure, and deploy the locker at scale across the domain. The techniques below are not theoretical — they appear in post-incident forensics reports from virtually every major ransomware intrusion in the 2022–2026 period.

The relationship between credential access and blast radius is direct: an affiliate with only a standard user account can encrypt one workstation. An affiliate with Domain Admin or equivalent can encrypt every endpoint, every server, and every backup simultaneously.

> [!IMPORTANT]
> The single highest-impact countermeasure in this entire kill chain is LSASS protection — PPL plus Credential Guard. If your environment does not have both enabled and verified, the rest of this challenge describes attacks that will succeed against you with commodity tooling.

## Task 4.2 – The Credential Access Toolkit

**1 )** **LSASS Dumping (T1003.001)**

The Local Security Authority Subsystem Service (LSASS) holds credential material in memory — NTLM hashes, Kerberos tickets, and in older configurations, cleartext passwords. Dumping LSASS is the fastest path from local access to domain credentials.

Common tools: Mimikatz (`sekurlsa::logonpasswords`), ProcDump (`procdump -ma lsass.exe`), comsvcs.dll MiniDump (`rundll32 comsvcs.dll MiniDump <PID> lsass.dmp full`), NanoDump (in-memory variant designed to evade LSASS protections).

Mitigations: LSASS PPL (Protected Process Light) via registry or Credential Guard via Hyper-V VSM. The Microsoft Defender ASR rule "Block credential stealing from LSASS" provides a software-based layer where hardware virtualisation is unavailable.

**2 )** **Kerberoasting (T1558.003)**

Any authenticated domain user can request a Kerberos service ticket (TGS) for any service principal name (SPN). The ticket is encrypted with the service account's password hash and can be brute-forced offline. Service accounts with weak passwords and high privilege (backup agents, SQL services, legacy application accounts) are the primary targets.

Tools: GetUserSPNs (Impacket), Rubeus (`kerberoast`), ADFind.

Detection signal: Security EID 4769 (Kerberos service ticket requested) with Encryption Type `0x17` (RC4-HMAC). Modern environments should see few RC4 TGS requests — a spike is high-confidence.

**3 )** **AS-REP Roasting (T1558.004)**

Accounts configured with "Do not require Kerberos pre-authentication" return an AS-REP encrypted with the account's password hash — without the attacker needing to authenticate first. Less common than Kerberoasting but trivially automated.

**4 )** **DCSync (T1003.006)**

Mimikatz `lsadump::dcsync` impersonates a Domain Controller and requests password hashes for all accounts via the MS-DRSR protocol. Requires `Replicating Directory Changes` and `Replicating Directory Changes All` permissions — typically held by Domain Admins, MSOL accounts, and Azure AD Connect sync accounts.

Detection: Security EID 4662 with Object Type `19195a5b-6da0-11d0-afd3-00c04fd930c9` (Domain Naming Context) and the Replicating Directory Changes GUIDs.

**5 )** **ADCS Misconfiguration Abuse (T1649)**

Active Directory Certificate Services misconfigurations (ESC1–ESC8, documented by SpecterOps) allow privilege escalation from a standard domain user to Domain Admin via certificate template abuse. Certify and Certipy are the standard enumeration and exploitation tools.

Particularly dangerous because ADCS is often deprioritised in hardening programmes — it is complex, widely deployed, and misconfigurations are common in environments that have not run a formal PKI audit.

**6 )** **Browser Credential Harvesting and MFA Fatigue**

Stolen browser credential stores (Chrome, Edge, Firefox) are extracted post-compromise using standard tooling. Password manager databases are targeted explicitly when present.

MFA fatigue (push-bombing): repeated MFA push notifications sent until a tired or distracted user approves one. No longer mitigated by push-based MFA alone — phishing-resistant MFA (FIDO2/passkeys) is the categorical fix.

## Task 4.3 – Detection Priorities

| Technique | ATT&CK | Why ransomware uses it | Telemetry to watch |
|---|---|---|---|
| LSASS access | T1003.001 | Mimikatz / ProcDump / comsvcs.dll MiniDump | Sysmon EID 10 (ProcessAccess on lsass.exe with access mask 0x1010/0x1410); EDR LSASS monitor |
| Kerberoasting | T1558.003 | TGS-REQ for SPNs returning RC4-HMAC tickets | Security EID 4769 with Ticket Encryption Type 0x17 and unusual user-to-SPN ratios |
| AS-REP roasting | T1558.004 | AS-REP request without pre-auth for target accounts | Security EID 4768 with Pre-Authentication Type 0 |
| DCSync | T1003.006 | Mimikatz lsadump replicating from a DC | Security EID 4662 with Replicating Directory Changes GUIDs from non-DC source |
| ADCS abuse | T1649 | Certipy/Certify enrolling on misconfigured templates | Certification Services EID 4886/4887; Locksmith / PKIView audit output |

> [!TIP]
> EID 4769 with Encryption Type 0x17 is one of the highest-fidelity Kerberoasting signals available in standard Windows logging — no Sysmon required. If your SIEM is not alerting on this, add it today. The false-positive rate in a modern environment is very low because RC4 TGS tickets should be rare.

## Task 4.4 – Discussion Questions

Write your answers in your team discussion document before moving to Challenge 5.

**Q1.** Your organisation has 200 service accounts. You have never audited which ones have SPNs registered, what their password age is, or whether they have elevated privileges. Walk through the steps of a Kerberoasting attack against your environment as it exists today. What would the attacker be able to access? What is the remediation priority order?

**Q2.** LSASS PPL and Credential Guard are the primary mitigations for LSASS dumping. Check your organisation's endpoint baseline: are both enabled on servers? On workstations? If not, what is the operational barrier to enabling them, and how would you remediate it?

**Q3.** ADCS misconfigurations (ESC1–ESC8) are exploitable in a large proportion of enterprise Active Directory environments. If your organisation has an internal PKI, when was the last time it was audited for ESC-class misconfigurations? What tool would you use to check, and who owns the remediation?

---

# Task 5: Discovery & Lateral Movement

In this module you will complete **Discovery & Lateral Movement** by reading and reflecting on **the tools ransomware affiliates use to map your environment and spread through it** and engaging with **Sigma detection content and lateral-movement alert priorities**.

## Task 5.1 – Why Discovery Is the Noisiest Stage

Discovery is one of the most reliably noisy stages of a ransomware intrusion. Affiliates need to map your environment quickly — often in the first one to two hours of dwell — and the tooling is well-known, widely detected, and leaves clear artefacts.

The challenge is not detection sensitivity. It is detection **speed**: discovery tools run, complete their work, and are deleted in minutes. If your SIEM alert takes four hours to surface, the attacker is already on the domain controllers.

> [!NOTE]
> This is the stage where dwell time directly determines blast radius. An affiliate who maps the environment in two hours and deploys the locker the same day produces a different outcome than one who is detected during the discovery phase and contained before lateral movement begins.

## Task 5.2 – Domain Enumeration Tools

**1 )** **AdFind**

The most commonly observed domain-enumeration tool in post-incident forensics, explicitly referenced in Conti playbooks. AdFind queries Active Directory directly via LDAP.

Signature command lines:
```
adfind.exe -f objectcategory=person
adfind.exe -f objectcategory=computer
adfind.exe -sc trustdmp
adfind.exe -subnets -f objectcategory=subnet
adfind.exe -dcmodes
```

**2 )** **BloodHound / SharpHound**

BloodHound ingests Active Directory relationships and graphs attack paths to high-value targets. SharpHound is the C# ingestor that collects the raw data. Virtually every sophisticated ransomware affiliate uses BloodHound to find the shortest path to Domain Admin.

**3 )** **ADRecon, PingCastle**

ADRecon produces detailed Active Directory configuration reports. PingCastle generates a risk-scored AD health check. Both are used for enumeration and also appear in legitimate AD audits — context (who ran it, from where, at what time) is the discriminating factor.

**4 )** **Network Scanning**

SoftPerfect Network Scanner, Advanced IP Scanner, and NetScan are commonly observed for host discovery. These are legitimate tools available freely online and produce clear network-scan telemetry in NetFlow and NGFW logs.

## Task 5.3 – Lateral Movement

**1 )** **PsExec (T1569.002)**

The classic lateral-movement tool. Creates a remote service (PSEXESVC) on the target host and executes commands over SMB. Leaves EID 7045 (new service installed) on the target. Detectable and widely known — still used because it works reliably in Windows environments.

**2 )** **Impacket Suite**

The Python Impacket library provides multiple lateral-movement modules used extensively in ransomware intrusions:

- `wmiexec` — remote command execution via WMI, semi-interactive shell
- `smbexec` — SMB-based command execution without dropping a binary
- `atexec` — command execution via Task Scheduler
- `dcomexec` — DCOM-based remote execution

All produce network-authentication events and process-creation telemetry.

**3 )** **Cobalt Strike**

Despite years of law-enforcement attention and the 2022 source-code leak, Cobalt Strike beacons remain among the most commonly observed second-stage frameworks in ransomware intrusions. Operation Morpheus (July 2024) disrupted unlicensed infrastructure but did not eliminate use.

Cracked and leaked versions are widely traded. Detection should target behavioural indicators (named pipes, beacon jitter patterns, default certificate hashes) rather than licence status.

**4 )** **Sliver, Brute Ratel, Mythic, Havoc**

Post-Morpheus alternatives increasingly appearing in ransomware intrusions, particularly where operators are avoiding Cobalt Strike signatures. Detection content exists for all four — Sigma rules are available in the SigmaHQ repository.

**5 )** **RDP and WinRM**

Lateral movement via RDP (T1021.001) and WinRM / PowerShell Remoting (T1021.006) using stolen credentials. Produces EID 4624 (type 10 for RDP, type 3 for network) and can be chained across the environment.

## Task 5.4 – Detection Content

**Sigma rule — AdFind discovery patterns (Conti/FIN12 style):**

```yaml
title: AdFind Discovery Patterns (Conti/FIN12 style)
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    Image|endswith: \adfind.exe
  args:
    CommandLine|contains:
      - "objectcategory=person"
      - "objectcategory=computer"
      - "trustdmp"
      - "subnets"
      - "dcmodes"
  condition: selection and args
falsepositives:
  - Legitimate AD audits performed by IT
level: high
```

**Lateral-movement priorities to alert on:**

**1 )** PsExec (PSEXESVC) service installs (EID 7045) across multiple endpoints in a short window — especially from a source host that is not an authorised management server.

**2 )** EID 4624 logon type 3 (network) chains showing the same account authenticating to many machines in rapid succession.

**3 )** Cobalt Strike named-pipe patterns (`\\.\pipe\msagent_*`, `\\.\pipe\status_*`, `\\.\pipe\postex_*`) on hosts that are not legitimate C2 infrastructure.

**4 )** WinRM / PowerShell remoting (ports 5985/5986) originating from sources that do not normally use it — particularly workstations remoting to servers.

**5 )** BloodHound / SharpHound collection artefacts: `BloodHound.zip`, `SharpHound.exe`, LDAP queries with the characteristic BloodHound session-collection filter string.

> [!TIP]
> The AdFind Sigma rule above will produce false positives in environments with active IT AD-audit programmes. Work with your AD team to establish a known-good list of authorised sources and times, then filter. A high-privilege account running AdFind from a workstation at 03:00 is not your AD team.

## Task 5.5 – Discussion Questions

Write your answers in your team discussion document before moving to Challenge 6.

**Q1.** Discovery tools like AdFind and BloodHound complete their work in minutes and are then deleted. If your average SIEM-to-analyst alert time is four hours, discovery will always be complete before anyone investigates. What architectural changes (automated alerting, SOAR playbooks, EDR auto-containment) would be required to make the detection useful rather than forensic?

**Q2.** PsExec is used by legitimate IT teams and by ransomware affiliates. Describe a detection rule for PsExec lateral movement that would fire on attacker use but not on your IT team's normal administrative activity. What contextual fields (source host, destination host, account, time) are required?

**Q3.** Cobalt Strike has been detected for years and still appears in almost every major ransomware intrusion. What does this tell you about the relationship between detection capability and attacker behaviour? If you could reliably detect Cobalt Strike beacons in your environment tomorrow, what would sophisticated actors do differently the week after?

---

# Task 6: Defense Evasion

In this module you will complete **Defense Evasion** by reading and reflecting on **how ransomware operators disable defences before detonating the locker** and engaging with **high-confidence detection content for the most common evasion techniques**.

## Task 6.1 – Why Evasion Comes Before Encryption

Most ransomware operations attempt to disable the EDR before encrypting. The logic is straightforward: a modern EDR with behavioural detection will fire on mass file-rename activity within seconds. If the agent is running when encryption starts, automated isolation can limit damage to a fraction of the environment.

Operators know this. Defense evasion is therefore not optional — it is a prerequisite for a successful detonation.

The good news: evasion techniques are noisy. Killing an EDR agent, deleting shadow copies, and clearing event logs are all high-fidelity signals. If these fire and are acted on, the operator has failed.

> [!IMPORTANT]
> The detection rules in this challenge are among the highest-value in the entire module. Shadow-copy deletion and boot-configuration tampering have near-zero false-positive rates in production environments. If your SIEM does not alert on them, you are missing a near-certain pre-encryption signal.

## Task 6.2 – Five Defense-Evasion Families

**1 )** **Bring Your Own Vulnerable Driver (BYOVD)**

An attacker drops a legitimately signed but known-vulnerable kernel driver, exploits the driver to execute code in kernel space, and uses that access to terminate or unhook security agents. Because the driver is legitimately signed, many defences do not block it at load time.

The [LOLDrivers project](https://www.loldrivers.io) maintains a public catalogue of drivers with known abuse patterns — over 1,000 entries as of 2025. Operators including AvosLocker, Cuba, BlackByte, BlackCat, and Akira have all deployed BYOVD variants.

Mitigations: Microsoft Defender ASR rule "Block abuse of exploited vulnerable signed drivers," Windows Defender Application Control (WDAC) deny-list, Hypervisor-Protected Code Integrity (HVCI).

**2 )** **Tampering via Legitimate Features**

Group Policy modifications to disable Windows Defender:
```
HKLM\SOFTWARE\Policies\Microsoft\Windows Defender
DisableAntiSpyware = 1
```

Removing tenant-level XDR onboarding, disabling Real-Time Protection, adding exclusions for the directories containing the locker. These are low-noise operations that exploit the same administrative interfaces your IT team uses.

**3 )** **Service Control Against Security Products**

```
net stop <EDR service name>
sc stop <EDR service name>
taskkill /f /im <EDR agent process>
```

Typically attempted, often unsuccessful against modern self-protecting agents — but worth alerting on regardless. An attacker attempting to stop your EDR is a clear declaration of intent.

**4 )** **Volume Shadow Copy and Recovery Destruction**

The most reliable pre-encryption signal. Operators delete shadow copies to prevent victim recovery without paying. Commands observed in almost every major intrusion:

```
vssadmin delete shadows /all /quiet
wmic shadowcopy delete
wbadmin delete catalog -quiet
bcdedit /set {default} bootstatuspolicy ignoreallfailures
bcdedit /set {default} recoveryenabled No
```

These commands have near-zero legitimate use in production environments outside of specific, authorised maintenance windows.

**5 )** **Log Clearing**

Increasingly observed immediately before encryption — operators clear Windows event logs to hinder forensic investigation:

```
wevtutil cl System
wevtutil cl Security
wevtutil cl Application
```

Also: stopping the EventLog service, unloading the Sysmon driver. Log clearing is both a forensic countermeasure and a high-confidence indicator that encryption is imminent.

## Task 6.3 – High-Confidence Sigma Detection

The following rule covers all three VSS/recovery-destruction command variants in a single detection:

```yaml
title: Volume Shadow Copy and Recovery Deletion
logsource:
  category: process_creation
  product: windows
detection:
  selection_vss:
    Image|endswith: \vssadmin.exe
    CommandLine|contains|all:
      - 'delete'
      - 'shadows'
  selection_wmic:
    Image|endswith: \wmic.exe
    CommandLine|contains: 'shadowcopy delete'
  selection_wbadmin:
    Image|endswith: \wbadmin.exe
    CommandLine|contains|all:
      - 'delete'
      - 'catalog'
      - '-quiet'
  selection_bcdedit:
    Image|endswith: \bcdedit.exe
    CommandLine|contains:
      - 'bootstatuspolicy ignoreallfailures'
      - 'recoveryenabled No'
  condition: 1 of selection_*
level: critical
tags:
  - attack.impact
  - attack.t1490
```

**Additional high-priority rules to implement:**

- Alert on `wevtutil cl` with any log name argument
- Alert on EventLog service stop (`sc stop eventlog`, `net stop eventlog`)
- Alert on Sysmon driver unload (`fltMC unload SysmonDrv`)
- Alert on known LOLDriver hashes from the LOLDrivers catalogue (available as a YARA rule set)
- Alert on `net stop` / `taskkill` targeting named EDR agent processes

> [!TIP]
> The bcdedit detection above fires on both the `bootstatuspolicy` and `recoveryenabled` variants. In a production environment these commands are almost never run legitimately outside of imaging workflows. Set this rule to **critical** severity with automated alerting — not a daily digest.

## Task 6.4 – Discussion Questions

Write your answers in your team discussion document before moving to Challenge 7.

**Q1.** Shadow-copy deletion has near-zero false-positive rate. Yet in many organisations, the detection rule exists but the alert sits in a queue and is investigated hours later — by which point encryption is complete. Map the alert-to-action chain in your organisation for a **critical** SIEM alert at 03:00 on a Saturday. How many minutes from alert fire to analyst action? Is that fast enough?

**Q2.** BYOVD exploits legitimately signed drivers, which means many traditional defences (application allowlisting keyed to signing) do not stop it. Microsoft's HVCI (Hypervisor-Protected Code Integrity) is the hardware-based mitigation. What percentage of your endpoint fleet supports HVCI? What is the plan for endpoints that do not?

**Q3.** Log clearing immediately before encryption is both a forensic countermeasure and an encryption-imminent signal. If an attacker clears your Windows Security log, what forensic evidence is lost? What sources outside the cleared log would allow you to reconstruct the intrusion timeline?

---

# Task 7: Exfiltration & Encryption/Impact

In this module you will complete **Exfiltration & Encryption/Impact** by reading and reflecting on **how ransomware operators stage and exfiltrate data before encrypting** and engaging with **the detection content for both stages**.

## Task 7.1 – Why Exfiltration Comes First

Modern ransomware almost always exfiltrates before encrypting. The sequence matters: exfiltration is the leverage that makes double extortion work. If you detect and contain the intrusion after exfiltration but before encryption, you still face a data-leak extortion — even if you recover cleanly from backups.

The detection priority for exfiltration is therefore not just about catching data theft — it is about identifying the intrusion while there is still time to prevent the encryption event entirely.

> [!IMPORTANT]
> The detection question for exfiltration is not *"did data leave?"* — for most enterprises, something is always leaving. The question is: *"did large volumes leave from a server tier, to a destination category we don't normally use, in a compressed time window?"* That framing produces useful signal without drowning in noise.

## Task 7.2 – Exfiltration Tools and Techniques

**1 )** **Rclone**

The single most-observed exfiltration tool across ransomware intrusions 2022–2026. Conti playbooks included rclone configurations almost verbatim. Rclone is a legitimate open-source cloud-sync utility — its presence in an enterprise environment is not inherently malicious.

Indicators of attacker use:
- `rclone.exe` (or a renamed copy with a different filename) executed from an unusual path
- Command lines containing `copy`, `sync`, `--ignore-existing`, `--no-check-certificate`, `--transfers`
- Outbound connections to MEGA, Backblaze B2, Google Drive, OneDrive, S3, Azure Blob Storage
- High byte-volume outbound from server-tier hosts in a short window

**2 )** **MEGAsync**

GUI-based MEGA client. Signature: outbound connections to `mega.nz` and `api.mega.io`. Often dropped alongside rclone as a backup exfiltration channel.

**3 )** **FileZilla and WinSCP**

Older standby tools still appearing in intrusions. SFTP/FTP connections to attacker-controlled servers. Watch for first-seen instances on server-tier hosts.

**4 )** **Cloud Storage Abuse**

Direct uploads to attacker-controlled buckets — S3, Azure Blob, Google Drive — particularly common where outbound DLP is weak or where legitimate use of those services makes detection harder.

**5 )** **Custom Go / Rust Exfiltrators**

BlackCat, Royal, and Akira have all deployed custom binaries for exfiltration. These do not match known-tool signatures. Detection relies on behavioural indicators: new first-seen binary on a server, high outbound byte-volume, unusual destination categories.

**6 )** **Cobalt Strike SMB / DNS Channels**

Smaller-volume staging over C2 channels — used for credential material, configuration files, and targeted high-value documents rather than bulk data.

## Task 7.3 – Exfiltration Detection (KQL)

The following query targets Microsoft Defender for Endpoint network telemetry and hunts for outbound burst activity from server-tier hosts to known cloud-storage destinations:

```kql
DeviceNetworkEvents
| where Timestamp > ago(24h)
| where ActionType == "ConnectionSuccess"
| where InitiatingProcessAccountDomain != ""
| where DeviceName endswith "-srv"
| where RemoteUrl matches regex
    @"(mega\.nz|backblazeb2|amazonaws\.com|blob\.core\.windows\.net|file\.io|anonfiles|gofile\.io)"
| summarize
    Bytes = sum(toint(ReportId)),
    Conns = count()
    by DeviceName, RemoteUrl, bin(Timestamp, 1h)
| where Conns > 50
| order by Conns desc
```

**What this catches:** Sustained outbound connections from server-named hosts to cloud-storage endpoints — high-confidence for rclone, MEGAsync, or custom exfiltrator activity.

**Tuning notes:**
- The `endswith "-srv"` filter assumes a consistent server naming convention; adapt to your environment
- Add legitimate cloud-storage destinations your organisation uses to an exclusion list
- The `Conns > 50` threshold is a starting point — tune based on your baseline outbound volume
- Run as a scheduled detection rule with a 1-hour look-back on a 15-minute cadence

> [!TIP]
> If you do not have MDE, the equivalent query translates to NetFlow data with destination IP/domain enrichment. The key signal is byte-volume from server-tier hosts to non-business cloud destinations in a compressed window — the specific SIEM implementation varies but the logic does not.

## Task 7.4 – Encryption & Impact

By the time encryption fires, the operator has already won — almost. The few minutes between locker launch and full impact are where automated EDR isolation can save large fractions of the file-share.

**1 )** **Mass File Rename / Extension-Add Bursts**

The most universal encryption signature. Every modern locker renames or re-extensions files as it encrypts them. EDRs benchmarked on this signal will alert within seconds of the locker starting.

Detection: EDR file-rename telemetry with a rate threshold (e.g., >100 renames per minute from a single process) across multiple directories.

**2 )** **Read–Rewrite–Delete Patterns at High Entropy**

Even fast lockers leave the pattern of reading a file, writing high-entropy bytes, and renaming. Entropy-based detection in modern EDRs targets this at the I/O layer.

**3 )** **Ransom Note File Drops**

Common names: `ReadMe.txt`, `HOW_TO_DECRYPT.txt`, `restore-files.txt`, `!!!IMPORTANT!!!.txt`, and locker-specific variants. Easy to alert on with a file-creation rule matching known ransom-note filenames.

**4 )** **Linux / ESXi Targeting**

Akira, BlackCat, Royal, and Babuk-fork families target hypervisor infrastructure directly. Standard kill chain on ESXi:

```bash
vim-cmd vmsvc/getallvms          # enumerate VMs
esxcli vm process kill --type=force --world-id=<ID>   # terminate VMs
```

Then encrypt VMDK files. Direct compromise of vSphere management interfaces (vCenter, ESXi host UI) via credential reuse or CVE exploitation is now standard operating procedure for sophisticated affiliates.

> [!NOTE]
> ESXi and hypervisor infrastructure are frequently excluded from EDR coverage and backup validation. If your VMware infrastructure is not in scope for your EDR deployment and your VMDK backups have not been tested for recovery, hypervisor encryption will be total and recovery will depend entirely on whether you have air-gapped or immutable copies.

## Task 7.5 – Discussion Questions

Write your answers in your team discussion document before moving to Challenge 8.

**Q1.** Exfiltration is the leverage behind double extortion. If your organisation detects and stops the locker before encryption but after 500 GB has already been exfiltrated, what is your incident-response decision tree? Do you pay to suppress the leak? What legal, regulatory, and reputational factors influence that decision? Who makes it?

**Q2.** The KQL query above detects rclone-style exfiltration to known cloud-storage destinations. A sophisticated affiliate could use a custom domain over HTTPS to an attacker-controlled server not on any blocklist. What compensating detection would catch that? (Hint: think about volume, timing, and process context rather than destination.)

**Q3.** Your ESXi infrastructure runs 80% of your production workloads. The last time VMDK backups were tested for recovery was 18 months ago. The vCenter admin account uses the same password as the domain admin. Map the blast radius of a hypervisor-targeting ransomware attack in your environment and identify the three highest-priority remediations.

---

# Task 8: Actor Profiles & Self-Assessment

In this module you will complete **Actor Profiles & Self-Assessment** by reading and reflecting on **the operationally relevant profiles of active ransomware clusters** and engaging with **a structured self-assessment of your detection portfolio against the full kill chain**.

## Task 8.1 – How to Use Actor Profiles

Module 1 covered ransomware brands in terms of genealogy and history. This challenge asks a different question: how does each actor cluster **behave operationally**, and what does that tell your detection content?

> [!NOTE]
> The brand matters less than the behaviour. An affiliate who moves from LockBit to RansomHub changes their ransom note, not their kill chain. A detection portfolio built around kill-chain stages catches all brands — including new ones that appear after this module was written. Use these profiles to calibrate your detection priorities, not to build brand-specific IOC lists.

## Task 8.2 – Active Cluster Profiles (2025–2026)

**Scattered Spider / UNC3944 / Octo Tempest**

- English-speaking, native-fluent help-desk vishing. Active in MGM/Caesars (September 2023), repeated SaaS provider compromises through 2024–2025.
- **Initial access:** vishing identity verification at help desks; SIM-swap targeting; targeting identity providers (Okta, Azure AD, Duo).
- **Lateral movement:** heavy use of legitimate admin tools, federated identity abuse, AWS/Azure cloud pivot.
- **Distinctive capability:** no malware required for initial access — pure social engineering. Traditional perimeter defences are largely irrelevant.
- **Detection priorities:** help-desk identity-verification audit logs; Conditional Access for unusual SaaS sign-ins; Okta / Azure AD admin-action alerts; SaaS-to-SaaS OAuth grant monitoring.

---

**LockBit (post-Operation Cronos)**

- Disrupted in February 2024 (Operation Cronos — NCA-led with Europol and FBI) but never fully eliminated. Public builder leak in 2022 means the locker code is widely available to smaller crews.
- Affiliate ecosystem fragmented across LockBit successor instances, Akira, and RansomHub.
- **TTPs:** heavy LotL, PsExec lateral movement, custom data-exfil binaries, varied initial access depending on affiliate.
- **Post-Cronos reality:** the brand is degraded but the affiliate talent pool dispersed into other operations. Detections keyed to LockBit-specific IOCs miss the dispersed affiliates.

---

**ALPHV / BlackCat → RansomHub**

- Rust-based locker. Exit-scammed in February 2024 after the Change Healthcare attack, taking the $22M ransom payment and leaving the affiliate unpaid. Affiliate base flowed largely into RansomHub.
- **Distinctive features:** searchable data-leak site (DLS), Linux/ESXi-aware locker, advanced anti-analysis techniques.
- **Detection priorities:** Rust-compiled binary first-seen events on workstations and servers; ESXi command-line activity (vim-cmd, esxcli) outside normal maintenance windows.

---

**Akira**

- Active 2023–present. Heavy exploitation of edge devices (Cisco ASA, Fortinet, Citrix) and infostealer-sourced credentials for initial access.
- Linux/ESXi support. Distinctive retro-aesthetic ransom note and DLS branding.
- Observed in healthcare, education, and critical infrastructure sectors.
- **Detection priorities:** edge-device exploit telemetry; Cisco ASA authentication anomalies; new first-seen binaries on ESXi hosts.

---

**Black Basta**

- Conti successor operation. Significant share of 2023–2024 attacks. Used Qakbot loader until that ecosystem was disrupted in August 2023 (Operation Duck Hunt), then shifted to DarkGate and Pikabot.
- Late-2024 internal leaks gave defenders unusual visibility into affiliate playbooks and tooling — similar to the Conti leaks of 2022.
- **Detection priorities:** DarkGate / Pikabot loader IOCs; mass-exploitation of ConnectWise ScreenConnect (CVE-2024-1709) in early 2024 campaigns.

---

**Qilin**

- Originally branded as Agenda; rebranded Qilin. Cross-platform locker.
- Synnovis / NHS pathology laboratories (June 2024) was the highest-profile victim — caused cancellation of thousands of appointments and blood-transfusion disruption across London NHS trusts.
- Affiliate program with explicit willingness to target healthcare.
- **Detection priorities:** standard kill-chain coverage; no distinctive unique TTPs but high-impact against healthcare infrastructure.

---

**The Current Long Tail: Play, Medusa, INC, RansomHub, Cactus, Hunters International, BianLian**

Each has distinctive branding and some variation in TTPs, but the operational defensive posture is similar across the cluster. Detect on kill-chain stages; the brand matters less than the behaviour. RansomHub in particular absorbed significant affiliate talent from ALPHV/BlackCat and LockBit post-disruption and is now one of the most active operations.

## Task 8.3 – Self-Assessment Checklist

Score each control as **Strong**, **Partial**, or **Absent** in your environment. Aim for at least **Strong** in the bolded rows before any others.

**Initial Access**

- [ ] **Phishing-resistant MFA (FIDO2/passkeys) on email and all remote-access services**
- [ ] Conditional Access policies rejecting sign-ins from unmanaged devices
- [ ] KEV-prioritised patching SLA of ≤14 days on internet-facing services
- [ ] Email URL rewrite and click-time reputation scanning
- [ ] AitM-aware sign-in anomaly detection (device compliance, impossible travel)

**Execution & Persistence**

- [ ] **PowerShell ScriptBlock logging (EID 4104) enabled and shipping to SIEM**
- [ ] Alerts on first-seen remote-access tool binaries on non-admin hosts
- [ ] Alerts on new services (EID 7045), scheduled tasks (EID 4698), and Run-key writes on servers

**Credential Access**

- [ ] **LSASS PPL and/or Credential Guard enabled on servers and workstations**
- [ ] **Kerberoasting alert: EID 4769 with Encryption Type 0x17**
- [ ] DCSync alert: EID 4662 with Replicating Directory Changes GUIDs from non-DC source
- [ ] ADCS audit completed (Certify / Certipy / Locksmith) with ESC-class findings remediated

**Discovery**

- [ ] Alerts on AdFind, BloodHound/SharpHound, network-scan tooling (SoftPerfect, Advanced IP Scanner)
- [ ] Process-creation telemetry (Sysmon or EDR) covering all servers

**Lateral Movement**

- [ ] **Alerts on PsExec / Impacket patterns from non-management sources**
- [ ] Alerts on anomalous EID 4624 type-3 authentication chains
- [ ] Cobalt Strike named-pipe and beacon-pattern detection

**Defense Evasion**

- [ ] **Critical alert: VSS / shadow-copy deletion (vssadmin, wmic, wbadmin)**
- [ ] **Critical alert: bcdedit bootstatuspolicy / recoveryenabled modification**
- [ ] Alert on wevtutil log-clearing and EventLog service stop
- [ ] BYOVD mitigation: Defender ASR rule for vulnerable driver abuse + HVCI where supported

**Exfiltration**

- [ ] **Alert on rclone / MEGAsync on server-tier hosts**
- [ ] NetFlow or proxy alert on outbound burst volume to cloud-storage categories from servers
- [ ] DLP coverage for sensitive data categories on email and web gateways

**Impact**

- [ ] **EDR auto-isolation on mass file-rename / encryption-rate threshold**
- [ ] Backup admin-plane uses phishing-resistant MFA with no shared credentials with production
- [ ] Immutable or air-gapped backup copies tested for recovery within the last 90 days
- [ ] ESXi/vSphere in EDR scope or equivalent hypervisor-layer monitoring

> [!NOTE]
> This checklist maps directly to Module 6's tabletop scenarios. Before running those scenarios, score yourself here and pick a scenario tuned to your weakest 2–3 control areas. A gap in the **bolded** rows means a ransomware affiliate with commodity tooling can likely complete the kill chain without being stopped.

## Task 8.4 – Module Wrap-Up

**What this module covered:**

**1 )** The six initial-access families dominant in 2025–2026 — and why edge-device exploitation and credential theft now outpace phishing as entry vectors.

**2 )** Execution and persistence via LOLBins, legitimate remote-access tools, and redundant persistence mechanisms — with EDR telemetry requirements.

**3 )** Credential access techniques that convert a foothold into domain-level control — LSASS, Kerberoasting, DCSync, and ADCS abuse.

**4 )** Discovery and lateral movement — why speed of detection matters more than sensitivity at this stage.

**5 )** Defense evasion — the pre-encryption stage where operators go quiet before the detonation. Shadow-copy deletion is your last reliable warning.

**6 )** Exfiltration — data already gone before the locker fires, and the detection content that catches bulk transfers from server-tier hosts.

**7 )** Actor profiles — calibrated for detection priority, not brand recognition.

**What comes next:**

- **Module 3** covers the negotiation playbook — what happens after the locker fires and you are facing an extortion demand.
- **Module 6** (Tabletop Scenarios) is the operational test of the detection content in this module. Bring your self-assessment scores into those scenarios.

> [!TIP]
> Take your three lowest-scored rows from the self-assessment checklist and write one sentence for each: *"The reason this is Absent/Partial in our environment is ___."* Those three sentences are your action plan. Bring them to your next security team review.

# 🏁 End of Lab

**Module 2 — Adversary Tradecraft** is complete.

You have covered the full ransomware kill chain from initial access through impact, with detection content at each stage and operational profiles for the most active 2025–2026 clusters.

Proceed to **Module 3: Negotiation Playbook** when ready.
