---
slug: module-1-foundations-and-ecosystem
id: sgyjvkqntewd
type: challenge
title: 'Module 1: Foundations & Ecosystem Map'
teaser: A deep-research briefing on ransomware origins, attack anatomy, landmark incidents,
  and the dual criminal/defender ecosystem.
notes:
- type: text
  contents: |-
    # 322 – Ransomware Ecosystem

    **Module 1 — Foundations & Ecosystem Map**

    Welcome to the first module of the Ransomware Ecosystem self-paced course. This briefing is designed for security professionals who already understand enterprise security fundamentals and are ready to engage with ransomware as an adversary tradecraft, economic, and governance problem.

    **This track has no virtual machines or hands-on labs.**
    Your work in each challenge is to read the content, engage with the material, and answer the discussion questions.

    **Estimated module time:** 3–4 hours
    **Challenges in this track:** 8
tabs:
- title: Challenge
  type: service
  hostname: bookworm
  port: 8080
difficulty: ""
timelimit: 7200
lab_config:
  default_layout_sidebar_size: 0
enhanced_loading: null
---

# Task 1: Introduction & Executive Summary

In this module you will complete **Module 1 of the Ransomware Ecosystem course** by reading and reflecting on **the executive summary and course foundations** and engaging with **ransomware as an economic, technical, and governance problem**.

## Task 1.1 – Read the Executive Summary

Ransomware has evolved from a single floppy-disk experiment in 1989 into a multi-billion-dollar criminal industry that has restructured how organizations think about cyber risk, insurance, incident response, and law enforcement cooperation across borders.

What began as opportunistic file-encrypting malware now operates as a **layered service economy**: separate specialists handle initial access, malware development, encryption, negotiation, laundering, and public-relations leverage on victim shaming sites.

Three observations frame everything that follows:

**1 )** **Ransomware is an economic phenomenon as much as a technical one.** Design choices on both sides are shaped by margin, friction, and risk.

**2 )** **The line between cybercrime and statecraft has blurred.** Nation-state actors borrow ransomware playbooks for disruption, and criminal groups operate from jurisdictions that decline to extradite them.

**3 )** **The most effective controls are unglamorous.** Phishing-resistant MFA, tested immutable backups, segmentation, and EDR with humans behind it are decisive — but the harder problem is governance, not technology.

> [!NOTE]
> This module assumes you already understand the basics of enterprise security. Each challenge in this track builds on the last, so read linearly the first time. On a second pass, pick an incident from Challenge 4 and trace its threads through Challenge 6 (attacker industry) and Challenge 7 (defender industry).

## Task 1.2 – Understand the Course Structure

This module covers **eight challenges**, each mapping to a major section of the research briefing:

| Challenge | Topic | Estimated Time |
|---|---|---|
| 1 | Introduction & Executive Summary | 20 min |
| 2 | Origins & Evolution | 25 min |
| 3 | Attack Anatomy & Detection | 25 min |
| 4 | Notable Incidents | 40 min |
| 5 | Mitigation Framework | 25 min |
| 6 | The Attacker-Side Industry | 35 min |
| 7 | The Defender-Side Industry | 40 min |
| 8 | Economic Snapshot & Strategic Questions | 30 min |

**1 )** Each challenge ends with **discussion questions**. These are not graded. Write your answers in a shared document, a personal notebook, or a team discussion thread.

**2 )** The discussion questions at the end of each challenge are **seeds for tabletop exercises**. Treat each one as a scenario brief — the harder you engage, the more useful the rest of the course becomes.

**3 )** Footnotes and source references appear in Appendix B of the original briefing. All figures are orders of magnitude — re-verify against primary sources before using them in a deliverable.

> [!TIP]
> If you are completing this module as part of a cohort, assign one team member per discussion prompt to lead a 5-minute debrief before moving to the next challenge.

## Task 1.3 – Review Key Terminology

Before continuing, familiarize yourself with the key terms used throughout this module. You will encounter all of them in the challenges ahead.

**1 )** **Ransomware operator** — The criminal group that brands and runs the operation (e.g., LockBit, ALPHV/BlackCat). Builds and maintains the locker, leak site, and payment infrastructure.

**2 )** **Affiliate** — The contractor who actually breaks in and deploys the locker under a Ransomware-as-a-Service (RaaS) model. Receives 70–80% of any paid ransom.

**3 )** **Initial Access Broker (IAB)** — A separate criminal who sells the pre-established network foothold the affiliate uses to enter the victim environment.

**4 )** **Infostealer** — Commodity malware that harvests browser credentials, cookies, and MFA tokens from infected hosts. The "logs" are sold on dark-web marketplaces and feed IABs.

**5 )** **Data Leak Site (DLS)** — A Tor-hosted site operated by the ransomware brand to publish stolen data from non-paying victims. The threat of posting is used as negotiation leverage.

**6 )** **Double extortion** — Encrypting the victim's data AND threatening to publish exfiltrated data. Introduced by Maze in 2019. Backups alone no longer guarantee consequence-free recovery.

**7 )** **RaaS (Ransomware-as-a-Service)** — The dominant operating model: operator provides the platform, affiliates provide the intrusion labor, proceeds are split.

**8 )** **BYOVD (Bring Your Own Vulnerable Driver)** — Defense-evasion technique in which an attacker loads a legitimately signed but vulnerable kernel driver to disable EDR/AV products.

**9 )** **MFA fatigue** — Repeatedly issuing push-notification MFA prompts until the user approves one. No longer mitigated by push-based MFA alone.

**10 )** **CIS rule** — Hard-coded instruction in many ransomware lockers that refuses to encrypt systems whose locale or keyboard layout indicates a Russian or CIS-region user.

> [!NOTE]
> A full glossary covering all 25+ key terms appears in Appendix A of the original research briefing. The terms above are the ones most critical for completing the challenges in this track.

---

# Task 2: Origins & Evolution of Ransomware

In this module you will complete **Origins & Evolution of Ransomware** by reading and reflecting on **35 years of ransomware history** and engaging with **the economic and technical forces that shaped each era**.

## Task 2.1 – The First Ransomware and the Dormant Decade (1989–2012)

**1 )** **1989 — The AIDS Trojan (PC Cyborg)**

In December 1989, Dr. Joseph L. Popp mailed approximately 20,000 floppy disks labelled *"AIDS Information — Introductory Diskettes"* to subscribers of *PC Business World* magazine and attendees of a World Health Organization AIDS conference. The disks contained a real questionnaire program — and a Trojan that counted system boots.

After 90 reboots, the malware hid directories on the C: drive and obscured filenames using simple symmetric substitution. It then displayed a ransom note demanding $189 (annual licence) or $378 (lifetime) sent to a P.O. box in Panama for *"PC Cyborg Corporation."*

The AIDS Trojan was crude — its "encryption" was reversible without paying — but it **established the template**: a coercive demand, a payment channel that resisted attribution, and a victim population large enough that some recipients would pay.

**2 )** **1990s–2012 — The Dormant Decade**

Through the 1990s and early 2000s, ransomware was a curiosity. The primary obstacle was **payment**: without anonymous fund collection, attackers were easy to trace.

Workarounds emerged over time:
- Western Union transfers, premium-rate SMS, and pre-paid vouchers (Paysafecard, Ukash) — high-friction but functional
- Russian families (most notably **GpCode**) experimented with strong asymmetric encryption between 2004 and 2010
- Early GpCode variants used weak implementations researchers could break; later versions moved to correctly implemented 1024-bit RSA

**"Police locker" scams** (Reveton, Urausy, 2011–2013) impersonated the FBI or Metropolitan Police, accused victims of viewing illegal content, and demanded a "fine." These locked the screen but didn't encrypt files — validating the social-engineering model before the encryption technology matured.

> [!NOTE]
> The economic obstacle of payment is the single most important reason ransomware was rare before Bitcoin. When you see an attacker ecosystem innovation, ask first: *what friction did this remove?*

## Task 2.2 – CryptoLocker and the Bitcoin Era (2013–2016)

**1 )** **September 2013 — CryptoLocker**

CryptoLocker combined three properties that defined the next decade of ransomware:

- **Hybrid encryption**: AES-256 + RSA-2048, with the private key held only on a remote C2 server
- **Bitcoin payment channel**: pseudonymous, global, automatable
- **A working payment portal**: decryption keys were actually delivered when victims paid

The operation extracted an estimated **$3 million** before *Operation Tovar* — an FBI-led multinational action in June 2014 — disrupted the GameOver Zeus botnet that distributed it and recovered the key database.

**2 )** **The imitator wave (2014–2016)**

CryptoLocker's success seeded a generation of imitators: CryptoWall, TorrentLocker, TeslaCrypt, Locky, Cerber, CTB-Locker, Petya. Distribution shifted from botnet droppers to large-scale phishing campaigns delivering malicious Office documents, then to exploit kits (Angler, Nuclear, RIG) targeting browser and Flash vulnerabilities.

The criminal economics improved with each iteration: better key management, faster encryption, and increasingly polished payment portals.

> [!IMPORTANT]
> Bitcoin was the underrated enabler. Earlier ransomware was bottlenecked by payment plumbing. Bitcoin gave attackers a global, pseudonymous channel they could automate. The same property — a public, permanent ledger — later became the lever defenders and law enforcement used to map the criminal economy and seize funds.

## Task 2.3 – Big Game Hunting and the RaaS Era (2016–Present)

**1 )** **2016–2019 — The Pivot to Enterprise Targets**

Around 2016, sophisticated operators recognized that one $5 million payment from a hospital, manufacturer, or city was easier than ten thousand $300 payments from individuals.

- **SamSam** (two Iranian nationals later indicted by the DOJ) targeted exposed RDP and unpatched JBoss servers, manually attacking organizations one at a time — hitting the City of Atlanta and the Colorado Department of Transportation
- **Ryuk** (Wizard Spider) weaponized the Trickbot and Emotet banking-trojan ecosystems as initial-access platforms, bringing *"big game hunting"* — deliberate selection of high-revenue victims — into the mainstream

The 2017 events — **WannaCry** (North Korea) and **NotPetya** (Russia/GRU) — were not classic criminal ransomware. Both caused devastating collateral damage and compressed years of organizational learning into a single news cycle, convincing boards worldwide that ransomware was no longer an IT problem.

**2 )** **2019–Present — RaaS and Double Extortion**

In late 2019, **Maze** introduced *double extortion*: exfiltrate sensitive data before encrypting, then threaten public release if the ransom is not paid.

This single innovation reshaped the threat. Backups — the canonical mitigation — **no longer guaranteed recovery without consequences**. Even fully restored victims faced a separate decision about whether to pay to suppress data leaks.

Maze also formalized the **RaaS model**: the operator builds and maintains the locker, leak site, and payment infrastructure; affiliates do the breaking-in, splitting the proceeds 70–80% to the affiliate and 20–30% to the operator.

**3 )** The post-Maze ecosystem added **triple extortion** (DDoS or harassment of customers) and **quadruple extortion** (regulator notification, stockholder pressure). After the **Conti leaks** of February 2022 — when a Ukrainian member inside the operation dumped two years of internal chats — the world learned exactly how corporate these operations had become: HR functions, dev sprint planning, OSINT teams, payroll, and employee onboarding documents.

> [!TIP]
> The RaaS model is the key structural insight of this entire course. Every control decision you make should be evaluated against *which role in the RaaS supply chain it disrupts* — and whether disrupting that role changes the criminal math.

## Discussion Questions — Origins & Evolution

Consider the following questions before moving to Challenge 3. Write your answers in your team discussion document.

**Q1.** The AIDS Trojan in 1989 established the ransomware template, but the market didn't take off until Bitcoin arrived in 2013. Identify two other *enabling technologies or conditions* — beyond Bitcoin — that unlocked ransomware growth in the 2013–2019 period. For each one, explain the friction it removed.

**Q2.** The shift from individual targets to *big game hunting* (2016–2019) was driven by economics, not technology. What would have to change in the economics of ransomware today for attackers to shift back to high-volume, low-value targets? What defensive implications would that shift have for your organization?

**Q3.** Double extortion made backup-only defenses insufficient. If triple and quadruple extortion layers continue to expand, what is the logical next layer of coercion attackers might add — and what defensive response would that require?

---

# Task 3: Anatomy of a Modern Ransomware Attack

In this module you will complete **Anatomy of a Modern Ransomware Attack** by reading and reflecting on **the ransomware kill chain and encryption mechanics** and engaging with **pre-encryption detection signals and your organization's detection posture**.

## Task 3.1 – The Kill Chain at a Glance

Modern ransomware operations follow a recognizable kill chain that maps cleanly to MITRE ATT&CK. Understanding each stage gives defenders a shared vocabulary for detection engineering and tabletop discussion.

**1 )** **Initial Access** — How attackers get in

- Phishing with malicious attachments or links
- Valid credentials purchased from infostealer logs
- Exploitation of internet-exposed services: RDP without MFA, VPN appliances with known CVEs (Citrix NetScaler, Fortinet, Ivanti, Cisco ASA)
- Supply-chain compromise
- Social engineering of help desks (Scattered Spider's signature technique)

**2 )** **Execution and Persistence** — Establishing a foothold

- Living-off-the-land binaries (LOLBins): PowerShell, WMIC, certutil, regsvr32, mshta
- Legitimate remote-access tools: AnyDesk, ScreenConnect, Atera, Splashtop, TeamViewer
- Web shells, scheduled tasks, run keys, modifications to existing services

**3 )** **Privilege Escalation & Credential Access** — Gaining the keys to the kingdom

- Mimikatz, LSASS dumping via comsvcs.dll or ProcDump
- Kerberoasting, AS-REP roasting, ADCS abuse
- Stored credentials in browsers and password managers
- Unpatched local privilege escalation CVEs

**4 )** **Discovery & Lateral Movement** — Mapping and spreading

- AdFind, BloodHound, SoftPerfect Network Scanner, ADRecon
- Lateral movement via SMB, WMI, PsExec, Impacket (wmiexec/smbexec/atexec), RDP
- Remote-management tooling and SSO infrastructure abuse

**5 )** **Defense Evasion** — Blinding the defenders

- Disabling Windows Defender via Group Policy
- Killing security agents using **BYOVD** (Bring Your Own Vulnerable Driver)
- Clearing event logs, modifying boot loaders, manipulating firewall rules
- Deleting volume shadow copies: `vssadmin delete shadows /all /quiet`, `wbadmin delete`, `bcdedit /set safeboot`

**6 )** **Exfiltration** — Staging data for the leverage play

- Rclone, MegaSync, FileZilla, custom Go binaries
- Data staged and uploaded to attacker-controlled cloud storage
- Volumes commonly range from hundreds of gigabytes to multiple terabytes

**7 )** **Encryption** — The ransom event

- Hybrid scheme: AES-128/256 (CTR or CBC mode) for file content
- The per-file AES key is encrypted with the operator's asymmetric public key (RSA-2048/4096 or Curve25519)
- The matching private key is held only by the operator
- Fast modes: intermittent encryption (first N MB, or every Nth block) maximizes throughput on large file shares

**8 )** **Extortion** — The negotiation

- Ransom note dropped in each directory and displayed at login
- Negotiation via a Tor-hosted victim portal
- Failure to pay triggers posting to a Data Leak Site (DLS)

> [!NOTE]
> Encryption is stage 7 of 8. By the time the ransom note appears, the attacker has typically been in the environment for **days to weeks**. Every earlier stage represents a detection opportunity that, if missed, brings the attacker closer to the point of no return.

## Task 3.2 – Encryption Mechanics

The **hybrid encryption scheme** is dictated by performance:

**1 )** Symmetric algorithms (AES) encrypt at rates approaching disk speed — a multi-terabyte file share can be locked in hours.

**2 )** Asymmetric algorithms (RSA) are too slow for large files but excel at protecting small things — like a per-file symmetric key.

**3 )** The result: the operator never has to expose the private key, and the victim cannot recover files without paying (or without a law-enforcement decryptor release).

**Two historical pitfalls** have given defenders openings:
- **Weak random number generation** in early families (CryptXXX, some Petya variants) — produced recoverable keys
- **Offline key generation** — keys embedded in the binary or memory, extractable before use

These are the exception. Modern, well-engineered families are designed assuming defenders will reverse-engineer the binary.

> [!IMPORTANT]
> If a decryptor has been released for a ransomware family, check **NoMoreRansom.org** before paying. Law enforcement operations (Operation Cronos — LockBit; Hive infiltration) have resulted in free decryptors being distributed to thousands of victims.

## Task 3.3 – Detection-Friendly Behaviors

Even sophisticated operations leave high-confidence signals. The detection engineering community has converged on a tractable set of behaviors that, in combination, produce few false positives.

**1 )** **Mass shadow-copy deletion**
- `vssadmin delete shadows /all /quiet`
- `wmic shadowcopy delete`
- Win32_ShadowCopy WMI calls

**2 )** **Boot configuration tampering**
- `bcdedit /set {default} bootstatuspolicy ignoreallfailures`
- `bcdedit /set {default} recoveryenabled No`

**3 )** **Service control against security products**
- `net stop`, `sc stop`, `taskkill` against EDR/AV processes
- Loading unsigned or vulnerable drivers (BYOVD)

**4 )** **Anomalous file I/O entropy** — Bursts of high-entropy writes across many files. EDRs have shifted from signature-based to behavioral telemetry on this exact pattern.

**5 )** **Anomalous authentication patterns**
- Surge of Kerberos ticket requests (Kerberoasting)
- Unusual RDP lateral movement across subnets
- Off-hours service-account logons

**6 )** **Legitimate remote-management tools in unexpected contexts**
- AnyDesk on a sysadmin's laptop: normal
- AnyDesk on a finance workstation at 02:00: not normal

**7 )** **Mass file-rename operations** — Adding a single extension across thousands of files simultaneously

**8 )** **Outbound transfers to rclone-friendly cloud endpoints** — From servers with no business reason to upload large volumes of data

> [!TIP]
> Sigma rules and the published ransomware playbooks from Microsoft, CrowdStrike, and SentinelOne are good starting points for operationalizing detection against behaviors 1–8. Most of these signals surface in EDR telemetry, SIEM, or network flow data — not in the endpoint logs alone.

## Task 3.4 – Discussion Challenge: Pre-Encryption Detection

> *"If your detection stack only fires on encryption itself, the attacker has already won."*

This is the **core detection insight** of this challenge. Encryption is the final step of a multi-week operation. Every one of the eight stages in the kill chain is a detection opportunity.

**Work through the following questions.** Write your answers in your discussion document before moving on.

**Q1.** Select **any ransomware family** you are aware of — or pick one from the list in Challenge 6 (LockBit, ALPHV/BlackCat, Akira, Black Basta, Scattered Spider/ALPHV). Identify **three behaviors** from the detection list above (Task 03) that would occur **hours or days before encryption** in a typical attack by that group.

**Q2.** For each of the three behaviors you identified: **Where in your environment** would that signal surface? Name the specific tool, log source, or telemetry feed that would generate it (e.g., EDR telemetry, Windows Security event log, SIEM correlation rule, network flow data, email gateway).

**Q3.** **Who is on call at 03:00** to act on each of those signals in your organization? Is there a documented escalation path from the alert to a human who can make a containment decision within 30 minutes? If not, what is the gap — tooling, staffing, or process?

> [!IMPORTANT]
> Question 3 is the most important question in this challenge. Most organizations have some version of behaviors 1–8 detectable in their environment. The failure mode is almost never *"we had no signal"* — it is *"the signal fired but no one acted on it in time."* The answer to Q3 is the starting point for your detection program.

---

# Task 4: Notable Incidents & What They Taught Us

In this module you will complete **Notable Incidents & What They Taught Us** by reading and reflecting on **eleven landmark ransomware case studies** and engaging with **the practical lessons that shaped the modern defensive playbook**.

> [!NOTE]
> Each incident below is presented with its core lesson called out explicitly. When you read each case, ask yourself: *Does my organization have the gap this incident exposed? What would the outcome have been for us?*

## Task 4.1 – The 2017 Wakeup Call

**1 )** **WannaCry — May 2017**

WannaCry exploited **EternalBlue** — a Windows SMBv1 vulnerability stolen from the U.S. NSA and dumped publicly by the Shadow Brokers in April 2017. Microsoft had patched it (MS17-010) in March, but unpatched and end-of-life systems were everywhere.

WannaCry infected an estimated **200,000+ systems across 150 countries** within days, including 80 NHS trusts in the UK — surgeries were cancelled and ambulances diverted. The U.S., UK, and others attributed the operation to North Korea's Lazarus Group.

> **Lesson:** Vulnerable services facing the internet are a strategic problem, not a maintenance backlog. A wormable exploit combined with a network-reachable RCE on millions of unpatched hosts is the worst-case scenario. The patching SLA debate that followed reset many enterprise vulnerability management programs.

**2 )** **NotPetya — June 2017**

NotPetya weaponized the **auto-update mechanism of M.E.Doc**, a Ukrainian accounting package, to propagate a wiper that posed as ransomware. The malware combined EternalBlue, EternalRomance, and a Mimikatz-style credential harvester to spread within networks at exceptional speed.

Maersk's global shipping operations, FedEx/TNT, Merck, Mondelez, Saint-Gobain, and Reckitt Benckiser were among the worst-hit. **Aggregate damages exceeded $10 billion.** The attack was attributed to Sandworm — a unit of Russia's GRU.

> **Lesson:** Supply-chain trust is a top-tier risk. The "war exclusion" in cyber-insurance policies is unreliable — the 2023 Merck v. ACE American appellate ruling found NotPetya did not qualify for the war exclusion, a decision that reverberated through the insurance industry.

## Task 4.2 – 2021: The Year of Critical Infrastructure

**1 )** **Colonial Pipeline — May 2021**

The DarkSide affiliate gained access via a single **legacy VPN account with a reused password and no MFA**. Encryption hit corporate IT; Colonial proactively shut down pipeline operations, cutting 45% of the U.S. East Coast's fuel supply. The company paid roughly **75 BTC (~$4.4 million)**; the FBI subsequently recovered ~63.7 BTC by tracing the wallet.

> **Lesson:** A single weak credential on a legacy access path can produce nation-impacting consequences. Colonial Pipeline drove the Biden administration's Executive Order 14028 and TSA pipeline security directives, plus permanent board-level attention to OT.

**2 )** **JBS Foods — May 2021**

REvil (Sodinokibi) compromised the world's largest meat processor, halting beef and pork production in the U.S. and Australia. JBS paid an **$11 million ransom** to expedite recovery and limit data-leak risk. The interruption affected food supply for several days.

> **Lesson:** Food and agriculture are critical infrastructure in practice even where they are not formally designated as such. The episode accelerated CISA/USDA sector engagement.

**3 )** **Kaseya VSA — July 2021**

REvil exploited zero-day vulnerabilities in Kaseya VSA — a remote-management platform used by Managed Service Providers — to push the locker through MSPs to their downstream customers. An estimated **800–1,500 small businesses** across multiple countries were encrypted. REvil demanded a **$70 million universal decryptor**.

> **Lesson:** MSPs are force-multipliers for both productivity and risk. The MSP-targeting playbook — compromise the management platform once, deliver to all customers — is now standard. CISA's MSP/MSSP security guidance emerged directly from this period.

## Task 4.3 – The Criminal Industry Exposed

**1 )** **Conti Chats Leak — February 2022**

After Conti's leadership posted public support for the Russian invasion of Ukraine, a Ukrainian researcher embedded inside the operation dumped roughly **two years of internal Jabber chats and source code** (the "ContiLeaks"). The disclosure exposed:

- HR functions, finance, payroll, and performance management
- Dev sprint planning and malware development pipelines
- OSINT teams, negotiation specialists, and affiliate onboarding documents
- An estimated **$180M+ annual revenue** at peak

> **Lesson:** Ransomware operators run as businesses with growth, retention, and quality-control problems. Defensive pressure on *profit margins* — slower laundering, recovered ransoms, reputational damage — is more durable than pressure on infrastructure alone.

**2 )** **Costa Rica National Emergency — April 2022**

Conti compromised the Costa Rican Ministry of Finance and 26 other government bodies, paralyzing tax collection and customs. President Rodrigo Chaves declared a **national state of emergency** on May 8, 2022 — the first such declaration over a cyberattack.

> **Lesson:** Ransomware can destabilize a national economy. The U.S. State Department's $10 million Rewards-for-Justice bounty for Conti leadership followed shortly after.

## Task 4.4 – Modern High-Impact Incidents

**1 )** **LockBit and Operation Cronos (2019–2024)**

At its peak, LockBit was the most prolific RaaS operation — publicly listed victims approaching **2,000**, a self-serve affiliate builder, a public bug-bounty for its own malware, and an explicit no-CIS rule.

On February 19–20, 2024, an international coalition led by the UK National Crime Agency **seized LockBit's infrastructure** (Operation Cronos), defaced the leak site, indicted operators including alleged administrator Dmitry Khoroshev, and released free decryptors.

> **Lesson:** Law-enforcement disruption can meaningfully reduce a brand's value to affiliates even without arresting the principals. A coordinated multi-jurisdiction operation, paired with public attribution and decryptor release, is now an established playbook (see also: Hive, REvil).

**2 )** **MGM Resorts and Caesars Entertainment — September 2023**

A young English-speaking subgroup called **Scattered Spider** (UNC3944/Octo Tempest) gained access to MGM via a **vishing call to the IT help desk** impersonating an employee, then deployed ALPHV/BlackCat ransomware.

- Caesars reportedly paid ~$15 million
- MGM declined to pay; their week-long outage cost an estimated **$100 million in lost revenue** plus ~$10 million in remediation

> **Lesson:** English-speaking, social-engineering-led affiliates are difficult to defend against with technical controls alone. Identity-verification protocols at the help desk — callbacks, ticket-driven workflows, the "three knowns" — became urgent priorities across the Fortune 500.

**3 )** **Change Healthcare — February 2024**

ALPHV/BlackCat compromised Change Healthcare, a UnitedHealth Group subsidiary that processes roughly **one in three U.S. medical claims**. UnitedHealth's CEO testified that initial access used **valid credentials on a Citrix portal with no MFA**.

The outage paralyzed pharmacy benefit and claims-processing systems for weeks. The company paid an approximately **$22 million ransom** — which the ALPHV affiliate then absconded with after ALPHV staged an exit scam, leaving the affiliate to re-extort the data under the new **RansomHub** brand. Total impact exceeded **$2 billion**.

> **Lesson:** The cost of a single MFA gap on one SaaS portal is now measured in billions, not millions. Concentration risk in healthcare clearinghouses is itself a systemic-risk problem regulators will address.

**4 )** **Synnovis / NHS London — June 2024**

The **Qilin** RaaS encrypted Synnovis, a pathology-services joint venture serving major NHS hospitals in south-east London. Blood-test capacity collapsed; thousands of operations and outpatient appointments were postponed. Patient data was published after the ransom was not paid.

> **Lesson:** Critical UK public services remain high-value targets. Recovery measured in weeks-to-months is a realistic planning assumption — not a worst case.

## Discussion Questions — Notable Incidents

**Q1.** Across the eleven incidents in this challenge, **three root causes** appear repeatedly: no MFA on a remote access path, unpatched internet-exposed services, and a flat/unsegmented network. Pick the single most impactful incident for your sector and trace exactly which root cause was decisive. Would your organization have had that gap closed at the time of the incident?

**Q2.** Colonial Pipeline (corporate IT encrypted → operational technology shut down proactively) and Change Healthcare (clearinghouse encrypted → entire U.S. medical claims ecosystem disrupted) both illustrate **concentration and dependency risk**. Identify the equivalent single-point-of-failure in your organization or sector. What is the downstream blast radius if that system is encrypted for two weeks?

**Q3.** The Conti leaks revealed a professional criminal organization with **HR, payroll, and dev sprints**. Given that level of organizational maturity, what does it mean to think of ransomware defense as a purely technical problem? What governance, organizational, or economic pressure would more meaningfully raise the cost for a Conti-scale operation?

---

# Task 5: Defense in Depth: The Mitigation Framework

In this module you will complete **Defense in Depth: The Mitigation Framework** by reading and reflecting on **the controls that consistently appear in post-incident reviews** and engaging with **a budget-constrained prioritization challenge for your organization**.

> [!NOTE]
> The controls below are the ones that appear repeatedly in post-incident reviews as either the missing piece (when absent) or the determining piece (when present). They map to the CISA #StopRansomware Guide and the NIST CSF 2.0 Govern–Identify–Protect–Detect–Respond–Recover loop.

## Task 5.1 – Identity is the New Perimeter

The majority of ransomware incidents in 2021–2026 began with a credential compromise. Identity is now the primary attack surface.

**1 )** **Phishing-resistant MFA on every internet-facing service**

Push-notification MFA is no longer sufficient against MFA-fatigue attacks and adversary-in-the-middle phishing kits (Evilginx, EvilProxy, Tycoon 2FA). Move to **FIDO2/WebAuthn** or smart cards for privileged accounts.

**2 )** **Conditional access policies** that:
- Block legacy authentication protocols
- Restrict access from impossible-travel locations
- Require compliant, managed devices for high-risk applications

**3 )** **Privileged Access Management (PAM)** with just-in-time, just-enough-access for domain admin, cloud admin, and break-glass accounts. Tier-0 isolation in Active Directory.

**4 )** **Identity provider-side detection** for password spray, token theft, anomalous OAuth consents, and dormant-account reactivation.

**5 )** **Help-desk identity verification protocols** that cannot be defeated by knowledge of public information.

> [!IMPORTANT]
> The Scattered Spider / MGM incident proved that the help desk is an identity-verification chokepoint. A vishing call that impersonates a legitimate employee and passes basic knowledge checks is sufficient to bypass technical MFA controls. The protocol must require verification *that cannot be provided by an attacker who has done OSINT* — not just name, employee ID, and department.

## Task 5.2 – Reduce the Attack Surface

**1 )** **Internet-exposed services inventory, refreshed continuously.** RDP, SMB, legacy VPN, and management interfaces must not be reachable from the internet without strong intermediation.

**2 )** **Patching cadence keyed to the CISA Known Exploited Vulnerabilities (KEV) catalog.** Edge-device CVEs (Citrix, Fortinet, Ivanti, Cisco) deserve emergency-class SLAs — these are the most commonly exploited initial-access vectors in 2023–2026.

**3 )** **Application allow-listing and macro restrictions.** Office macros from the internet should not execute in 2026. HTML smuggling, ISO/IMG containers, OneNote, LNK, and XLL are the current delivery vectors — email and web filtering should be tuned accordingly.

**4 )** **DNS filtering** against newly-registered domains and known malicious infrastructure. Major vendors publish daily IOC feeds.

> [!TIP]
> The KEV catalog is free and updated daily. If you do not have a process that ingests KEV and produces a patching ticket within 48 hours for internet-facing assets, that is a gap worth closing before any other project on this list.

## Task 5.3 – Detect and Contain In Flight

**1 )** **EDR/XDR on every endpoint** — servers included — with auto-response enabled for high-confidence behaviors. Test the response playbook against your own production gold images quarterly.

**2 )** **Detection engineering against the high-confidence behaviors from Challenge 3** (mass shadow-copy deletion, boot-config tampering, service control against security products, anomalous file I/O entropy). Sigma rules and the Microsoft/CrowdStrike/SentinelOne ransomware playbooks are good starting points.

**3 )** **Network segmentation** so that a compromise of one workstation cannot directly reach:
- Domain controllers
- Backup servers
- Virtualization management (VMware vCenter, Hyper-V)
- SAN/NAS management interfaces

> [!IMPORTANT]
> Many post-incident reviews find that a **flat network was the primary force multiplier**. The attacker's access to one endpoint shouldn't imply access to everything.

**4 )** **24/7 monitoring with a documented response runbook.** Ransomware operators preferentially detonate on **Friday nights, holiday evenings, and the first day of long weekends** — exactly when staffing is thinnest. If you do not staff this internally, retain an MDR provider.

## Task 5.4 – Plan to Recover

**1 )** **3-2-1 backups**, with at least one copy **immutable** (object-lock, WORM, hardware-enforced) and at least one copy **offline or air-gapped**. The attacker's first move on discovering backups is to delete or encrypt them — assume they will try.

**2 )** **Tested restore procedures.** Run an end-to-end restore from backup in a non-production environment on a regular cadence. Untested backups have a non-zero real-world failure rate.

**3 )** **Documented incident response plan** covering:
- Legal notification and breach-counsel engagement
- Communications: internal, customer, press
- Regulator notification timelines by jurisdiction
- An explicit *"to-pay-or-not-to-pay"* decision tree with named decision-makers

**4 )** **Tabletop exercises** — at least annually for the executive team, twice annually for the technical team.

**5 )** **IR retainer placed before the incident.** The hours after detection are not the time to procure professional services. The retainer also gives you access to the firm's threat-intel context about active campaigns targeting your sector.

> [!NOTE]
> The IR retainer is dual-use: it gives you a response resource and it signals to insurers that you have a mature incident response posture. Some cyber-insurance carriers offer meaningful premium discounts for documented retainers with tier-1 IR firms.

## Task 5.5 – Discussion Challenge: Budget-Constrained Prioritization

This is the **core decision challenge** of this module. There is no single correct answer — the quality of your response reflects the quality of your assumptions.

**The scenario:** You are the CISO of a 1,000-person organization. You have a single annual budget cycle and can fund **only six of the controls** from Tasks 01–04. Your organization currently has:
- No MFA on VPN remote access
- An EDR product installed but not configured for auto-response
- No MDR — a 9-to-5 internal team only
- 3-2-1 backups on-site but not tested in 18 months and not immutable
- A flat network with no segmentation between workstations and servers
- No IR retainer

**Work through the following questions in your discussion document:**

**Q1.** Which **six controls** would you fund first? Rank them in priority order and give a one-sentence justification for each choice.

**Q2.** What does your prioritized list **assume about your threat actor**? A Scattered Spider-style social-engineering crew requires a different answer than a nation-state targeting edge devices. Be explicit about the threat model your six choices encode.

**Q3.** Which of your six selected controls would be **the first to fall short** under a Scattered Spider-style vishing attack against your help desk — and what would you add if you had a seventh budget line?

**Q4.** If you could only implement **one** control from your list of six in the next 30 days, which would it be and why?

> [!TIP]
> Budget prioritization exercises are most useful when they force explicit trade-offs rather than "we should do everything." The constraints in this scenario are realistic for the mid-market. If your organization is larger, adjust the scenario to your actual gap list — the exercise structure is the same.

---

# Task 6: The Attacker-Side Industry

In this module you will complete **The Attacker-Side Industry** by reading and reflecting on **the criminal supply chain that produces ransomware operations** and engaging with **the resilience of that supply chain to disruption**.

> [!NOTE]
> Ransomware is a market. Like other markets, it has produced specialization, intermediaries, branding, reputation systems, and financial plumbing. Understanding the structure of the criminal side is prerequisite to evaluating where defensive pressure is actually durable.

## Task 6.1 – The RaaS Market

**RaaS is the dominant operating model for criminal ransomware.** The operator brand provides the locker, leak site, victim chat portal, payment infrastructure, and reputation. Affiliates are independent contractors who supply the intrusion labor.

| Role | What They Do | Typical Fee / Split |
|---|---|---|
| RaaS operator | Builds locker, leak site, payment portal; runs brand reputation; vets affiliates | 20–30% of paid ransom |
| Affiliate | Buys access or breaks in directly; performs lateral movement, exfiltration, and detonation | 70–80% of paid ransom |
| Initial Access Broker (IAB) | Sells pre-compromised access on forums (XSS, Exploit). Pricing tiers by victim revenue, sector, and access type | $500–$100,000+ per access |
| Infostealer operator | Distributes Redline / Lumma / Vidar / StealC; sells "logs" (credentials, cookies, MFA tokens) on Russian Market and similar | $10–$50 per log |
| Negotiator | Runs the victim chat; uses staged proof-of-life, deadlines, and selective leak disclosure | Salary or % of payment |
| Money-laundering service | Mixers, chain-hopping bridges, OTC desks; converts crypto to cash | 5–25% laundering fee |
| Bulletproof hosting | Tolerant infrastructure for C2, leak sites, and payment portals | Monthly subscription |

**1 )** RaaS brands **compete for affiliate talent** the way SaaS companies compete for developers. The competitive levers are:
- Encryption performance and compatibility (Windows, Linux/ESXi, NAS appliances)
- Quality of the affiliate portal: dashboards, ticketing, on-call support
- Reputation for paying out reliably (no exit scams)
- Legal risk: indictments, infrastructure seizures, decryptors released by law enforcement
- PR: leak-site polish, media engagement, and victim-shaming theatrics that increase pressure to pay

## Task 6.2 – A Short Genealogy of RaaS Brands

Brand history is messy because operators rebrand to escape law enforcement attention or recover from breaches.

**1 )** **CryptoLocker → CryptoWall → Locky → Cerber** — The first wave of crypto-ransomware (2013–2016), distributed largely by exploit kits and email campaigns.

**2 )** **GandCrab → REvil/Sodinokibi** — GandCrab pioneered the modern RaaS model in 2018 and publicly "retired" in 2019. Operators quickly re-emerged as REvil. REvil hit JBS, Kaseya, and others before being disrupted by U.S. and Russian action in 2021–2022.

**3 )** **Maze → Egregor → Sekhmet** — Maze (2019–2020) introduced double extortion and shaped industry practice. When Maze publicly "closed" in late 2020, affiliates migrated to Egregor and successor brands.

**4 )** **Ryuk → Conti → Black Basta / Karakurt / Royal / BlackSuit** — The Wizard Spider lineage. Conti dissolved after the February 2022 leaks; members rebranded into multiple successor families.

**5 )** **DarkSide → BlackMatter → ALPHV/BlackCat → RansomHub** — The Colonial Pipeline operators rebranded to BlackMatter, then ALPHV/BlackCat (the first Rust-based major locker). After Change Healthcare in early 2024, ALPHV staged an exit scam; affiliates and access flowed into RansomHub.

**6 )** **LockBit (1.0 / 2.0 / 3.0 / Green)** — The most prolific brand of 2022–2023. Disrupted by Operation Cronos in February 2024 but never fully eliminated.

**7 )** **Current cluster (mid-2026):** Akira, Black Basta, Play, Medusa, Qilin, INC, Cactus, Hunters International, RansomHub, BianLian — a long-tail market where smaller brands compete on niche features and some have shifted to data-extortion-only operations (no encryption at all).

> [!TIP]
> The genealogy above illustrates the key structural insight: **operators rebrand, but affiliates and IABs are persistent**. A law-enforcement takedown that seizes infrastructure but doesn't arrest the affiliate base simply moves the talent to the next brand within weeks.

## Task 6.3 – Initial Access Brokers & the Access Marketplace

**IABs are arguably the most important specialization in the modern ecosystem.** They convert the noisy work of finding a foothold into a tradable inventory item, lowering the skill floor for affiliates who no longer need to be skilled intruders.

**1 )** **Pricing factors:**
- **Sector:** Manufacturing, healthcare, and professional services command premiums
- **Estimated revenue:** Brokers cite D&B or ZoomInfo to substantiate "$500M+ revenue" claims that justify higher prices
- **Access type:** Domain admin via Cobalt Strike beacon > VPN with valid AD credentials > RDP with local admin > web shell on a single host
- **Geography:** English-speaking jurisdictions price higher; CIS targets typically not listed at all

**2 )** **Observed price ranges (2025–2026):**
- Single RDP foothold: a few hundred dollars
- Domain administrator access at a multinational: well over $100,000

> [!NOTE]
> IAB listings have shifted from open advertising on forums (XSS, Exploit, BreachForums) toward private brokered sales. The move toward privacy reflects law-enforcement pressure on the forums themselves, not reduced activity.

## Task 6.4 – Infostealers: The Credential Supply Chain

**Infostealers** — Redline, Vidar, Raccoon, Lumma, StealC, RisePro, Atomic Stealer (macOS) — are mass-distributed commodity malware that harvest browser credentials, cookies, autofill data, cryptocurrency wallets, MFA tokens, and FTP/SSH keys.

**1 )** Collected files ("logs") are sold on dedicated marketplaces: Genesis Market (seized April 2023), Russian Market, and successors.

**2 )** **Logs are particularly dangerous because they often include session cookies** — a buyer can replay an authenticated session without needing the user's password or MFA at all.

**3 )** A surprising fraction of corporate compromises trace back to an **infostealer infection on an employee's personal device** used to access corporate SaaS over BYOD. The personal device was outside the enterprise security perimeter entirely.

**4 )** Infostealer logs are the upstream feedstock for IABs, who package, deduplicate, and resell them at higher margins.

## Task 6.5 – Laundering Infrastructure & Cryptocurrency

Bitcoin and other cryptocurrencies enabled the modern ransomware market — but the public ledger is also their structural weakness. Every payment ultimately has to be cashed out, and each laundering layer has its own friction and enforcement risk.

**1 )** **Mixers / tumblers** — Blend funds across many users to break the chain of custody. U.S. Treasury sanctioned Tornado Cash (August 2022) and Sinbad (November 2023); ChipMixer infrastructure was seized (March 2023). Each takedown shifts traffic to the next service.

**2 )** **Chain hopping** — Conversion across blockchains (BTC → ETH → Monero → back) using cross-chain bridges. Privacy coins, especially Monero, remain the preferred laundering layer when the pipeline can absorb conversion losses.

**3 )** **OTC desks and illicit exchanges** — Garantex (sanctioned April 2022; seized March 2025) and Suex were canonical examples: higher fees, lower compliance friction.

**4 )** **Cash-out via no/low-KYC rails** — Crypto ATMs, in-person trades, prepaid cards, and gift-card laundering at the retail tail of the pipeline.

**Blockchain analytics firms** (Chainalysis, TRM Labs, Elliptic) have made this layer increasingly tractable for law enforcement. Confirmed ransomware payments tracked were approximately **$1.0 billion in 2023 → $1.1 billion in 2024**, with 2025 trending higher.

## Task 6.6 – Negotiation Tactics, Geopolitics & the CIS Rule

**1 )** **Negotiation playbook** — Operators run standardized, scripted chats:
- Opening demand calibrated to victim annual revenue (commonly 1–5%)
- Proof-of-life decryption of a small file set
- Staged release of stolen data on the DLS if pressure is needed
- Willingness to discount substantially when the victim demonstrates inability to pay
- DLS theatrics: countdown timers, sample data drops, PR-style write-ups to attract media and regulator attention

**2 )** **Triple/quadruple extortion layers:**
- DDoS of victim infrastructure during negotiation
- Harassing calls to executives or customers
- Threats to notify regulators
- Threats to short the victim's stock

**3 )** **The CIS rule** — The single most important political fact about ransomware: most prolific brands operated from Russia and other CIS jurisdictions, which have not extradited nationals to the U.S. or its allies. The CIS rule — the locker checks the system locale and refuses to run on CIS-region systems — is hard-coded in the malware itself. This is an unusual artifact: a political instruction embedded inside crime software.

**4 )** **Geopolitical layers:**
- **North Korea** (Lazarus and subgroups): ransomware as direct regime revenue — WannaCry, Maui, H0lyGh0st
- **Iran** (MuddyWater, MOIS-aligned clusters): ransomware-style operations for disruption and coercion
- **China** (APT41): rare use of ransomware-like wipers in some operations
- **Criminal operators outside CIS**: recent indictments have named individuals in Ukraine, Belarus, Latvia — enforcement depends on cooperation that is uneven

## Task 6.7 – Discussion Challenge: Supply Chain Resilience

> *"If a single ransomware brand were eliminated tomorrow — operators arrested, infrastructure seized, decryptor released — how long would the affiliate base take to reconstitute under a new brand?"*

This is the central strategic question for evaluating law-enforcement disruption as a defensive lever.

**Work through the following in your discussion document:**

**Q1.** Select a recently disrupted RaaS brand from the genealogy in Task 02 (LockBit, ALPHV/BlackCat, REvil, or Hive). **Trace the path of reconstitution:** Which RaaS competitors absorbed the affiliates? Which IABs continued to sell to the new buyers? Which laundering services routed the proceeds? How long did the disruption actually reduce attack volume?

**Q2.** The genealogy shows that **operators rebrand but affiliates persist**. Given this, which layer of the supply chain is the most structurally vulnerable to sustained disruption — operators, affiliates, IABs, or money launderers? Defend your answer with evidence from at least two incidents in this challenge or Challenge 4.

**Q3.** **Blockchain analytics** (Chainalysis, TRM Labs) and **OFAC sanctions** are the primary mechanisms for applying financial pressure on the laundering layer. What are the limits of these mechanisms, and under what conditions would they be most effective at reducing the economics of ransomware operations?

**Q4.** The Conti leaks revealed that sophisticated operators have **performance management, onboarding, and dev sprints**. If you were designing a defensive operation to raise the *operating cost* of a RaaS brand (not just the legal risk) — what three levers would you pull, and why?

---

# Task 7: The Defender-Side Industry

In this module you will complete **The Defender-Side Industry** by reading and reflecting on **the commercial and institutional ecosystem that has grown up to fight ransomware** and engaging with **how these categories interact and where they fall short**.

> [!NOTE]
> The single most striking fact about this section: ransomware has *created* entire product and service categories that didn't exist as standalone businesses a decade ago. XDR, MDR, ransomware negotiation, blockchain analytics, and cyber insurance with active loss prevention are all, in part, ransomware-era inventions.

## Task 7.1 – EDR and XDR

**Endpoint Detection and Response (EDR)** emerged in the early 2010s and accelerated dramatically after WannaCry/NotPetya. **Extended Detection and Response (XDR)** folds in network, identity, email, and cloud telemetry. Together they are the cornerstone of every credible ransomware defense — and the absence of EDR is a near-universal red flag in cyber-insurance underwriting.

**1 )** **Leading vendors:** CrowdStrike Falcon, Microsoft Defender for Endpoint, SentinelOne Singularity, Palo Alto Cortex XDR, Sophos Intercept X, Trend Micro Vision One, Trellix, Cybereason, ESET Inspect, Bitdefender GravityZone.

**2 )** **Buying pattern:** Per-endpoint per-month subscription with tiered modules (basic EDR, threat hunting, identity protection, exposure management).

**3 )** **Where EDR fails:**
- Unmanaged devices and BYOD/contractor laptops
- OT/ICS environments and legacy systems that cannot run a modern agent
- Network appliances (firewalls, VPN concentrators, switches)
- These are **exactly** the systems attackers preferentially compromise

> [!IMPORTANT]
> The **CrowdStrike outage of July 2024** — a defective Falcon sensor content update that bricked roughly 8.5 million Windows hosts globally, costing affected industries an estimated $5 billion+ — was not a security event, but it illustrates the systemic concentration risk created by the EDR market. When a small number of agents run on the world's endpoints, a bad update is a single point of failure with no equivalent in the pre-EDR era.

## Task 7.2 – Managed Detection and Response (MDR)

Most organizations cannot staff a 24/7 SOC. MDR providers run the SOC for them — typically with a vendor-specific or vendor-agnostic EDR underneath, plus their own analytic content, threat hunting, and on-call response capability.

**1 )** **Leading firms:** Arctic Wolf, Expel, Red Canary, Huntress, eSentire, Sophos MDR, CrowdStrike Falcon Complete, Rapid7 MDR, Secureworks Taegis MDR, Trustwave.

**2 )** **Pricing:** Per-endpoint or per-user per-month, with tiered response inclusions. Average enterprise spend per endpoint is materially higher than DIY EDR.

**3 )** **Trade-offs:** MDR providers vary widely in:
- Quality and freshness of detection content
- Analyst response time SLAs (minutes vs. hours matters enormously)
- Willingness to take containment action without customer approval

> [!TIP]
> Before signing an MDR contract, ask for SLA evidence and request a live tabletop demonstration of their response workflow. The quality difference between tier-1 and tier-3 MDR providers is not visible in the marketing materials — it shows in the runbook.

## Task 7.3 – Threat Intelligence Vendors

Threat intelligence as a market exists in part *because of* ransomware. The sub-categories are distinct:

**1 )** **Tactical intel** — IOCs, YARA rules, Sigma rules. Point-in-time, high volume, low shelf life.

**2 )** **Strategic / actor-attribution reporting** — Campaign analysis, adversary profiles, sector-specific threat briefings.
- Leading providers: Mandiant (Google), CrowdStrike, Microsoft MSTIC, Unit 42 (Palo Alto), SecureWorks CTU, Talos (Cisco), Symantec, IBM X-Force, ESET, Kaspersky.

**3 )** **Dark-web monitoring** — Forum postings, leak-site scraping, infostealer-log alerts. Flags when your organization or credentials appear in criminal infrastructure.
- Leading providers: Recorded Future, KELA, Flashpoint, Group-IB, Intel 471, Digital Shadows (ReliaQuest), ZeroFox.

**4 )** **Brand and executive protection** — Domain typosquatting monitoring, executive personal-info exposure alerts, social media impersonation detection.

> [!NOTE]
> The most valuable threat-intel subscription for a mid-market organization is often **dark-web monitoring for credential exposure** — because infostealer logs hitting Russian Market with your employees' cookies are frequently the upstream event for an IAB sale that precedes the ransomware deployment by weeks.

## Task 7.4 – Incident Response Firms and Ransomware Negotiators

**1 )** **Incident Response firms** are engaged either through a pre-placed retainer or emergency procurement after detection.

Key firms: Mandiant (Google), CrowdStrike Services, Secureworks, Palo Alto Unit 42, Kroll, Sygnia, Aon Cyber Solutions, Stroz Friedberg (Aon), Coveware, Kivu Consulting.

Roles in a ransomware engagement: forensic investigation, evidence preservation, attacker eviction, recovery support, ransom negotiation (sometimes), and regulatory notification support.

**2 )** **Ransomware negotiators** are a specialized sub-category. Leading firms: Coveware, Kivu Consulting, GroupSense, Soverance.

The negotiation process (drawn from the Conti playbook and Coveware case data):
- Opening demand: typically 1–5% of victim annual revenue
- Proof-of-life decryption of a small file set
- Price reduction of 40–80% is common with experienced negotiators
- **OFAC sanctions screening** is now a mandatory step — paying a sanctioned actor is a strict-liability violation for the victim, negotiator, and insurer

> [!IMPORTANT]
> The October 2020 and September 2021 OFAC advisories made explicit that paying a sanctioned ransomware actor (Evil Corp, certain LockBit- or ALPHV-linked individuals, sanctioned mixers) carries strict-liability sanctions risk. Sanctions screening mid-negotiation is now standard — if the threat actor turns out to be on the OFAC list, the negotiation branch changes significantly.

## Task 7.5 – Cyber Insurance

Cyber insurance has been one of the most consequential commercial responses to ransomware — both as a financial backstop and as a market mechanism for driving security hygiene adoption.

**1 )** **Major carriers:** Beazley, Chubb, AIG, Hiscox, Travelers, Munich Re, Allianz, Zurich, Coalition, At-Bay, Resilience, Cowbell, Corvus (Travelers).

**2 )** **Coverage components:**
- First-party: IR costs, business interruption, ransom payment, data recovery
- Third-party: regulatory fines (where insurable), liability
- Active risk services: continuous attack-surface monitoring, free EDR/MDR for lower-tier customers

**3 )** **Underwriting evolution:** The application questionnaire has become a *de facto* industry security standard. Carriers commonly require:
- MFA on remote access and email
- EDR on every endpoint
- Immutable backups
- Network segmentation
- EOL software bans
- Vulnerability management programs
- Tabletop testing

**4 )** **Premium dynamics:** Rates rose 100–300% in 2021–2022, eased in 2023–2024, then began rising again as breach severity increased. Sub-limits and coinsurance for ransomware events are now standard.

**5 )** **Policy disputes:** The Merck v. ACE American (NJ, May 2023) ruling that NotPetya was not excluded by the war exclusion prompted insurers to add explicit "cyber war" exclusions — the wording of which remains actively contested.

**6 )** **Public-policy debate:** AXA France (May 2021) announced it would no longer reimburse ransom payments. North Carolina and Florida (2022) prohibited state and local government entities from paying ransoms. Australia (2024 Cyber Security Act) introduced mandatory ransomware-payment reporting.

## Task 7.6 – Legal, Regulatory & Government Response

**1 )** **U.S. SEC Cybersecurity Disclosure Rule (effective Dec 2023)** — Public companies must disclose material cybersecurity incidents on Form 8-K within four business days of materiality determination.

**2 )** **CIRCIA (2022)** — Covered critical-infrastructure entities must report covered cyber incidents to CISA within 72 hours and ransom payments within 24 hours. Final implementing rules are phasing in.

**3 )** **EU NIS2 (transposed 2024–2025)** — Expanded scope, mandatory 24/72-hour incident reporting, executive accountability provisions.

**4 )** **EU DORA (effective January 2025)** — Financial sector and ICT providers; detailed incident reporting and resilience testing requirements.

**5 )** **OFAC and FinCEN** — Sanctions designations; strict-liability payment-facilitation risk for victims, negotiators, and insurers.

**6 )** **Government and law enforcement operations (key milestones):**
- **CISA / FBI** — Joint advisories on RaaS families; StopRansomware.gov; JCDC public-private coordination
- **Hive disruption (FBI, Jan 2023)** — ~$130M in ransom payments averted; covert infiltration before seizure
- **Operation Cronos (NCA/Europol, Feb 2024)** — LockBit seizure, indictments, free decryptors
- **Operation Endgame (Europol, May 2024)** — Loader/dropper botnet infrastructure (IcedID, SystemBC, Pikabot, Smokeloader, Bumblebee, Trickbot)
- **NoMoreRansom.org** — Free decryptors distributed to tens of thousands of victims

**7 )** **Counter Ransomware Initiative (CRI)** — Multilateral cooperation framework, 60+ countries; policy, sanctions, and joint operational support.

## Task 7.7 – Backup, Recovery & Standards

**1 )** **Backup vendors:** Veeam, Rubrik, Cohesity, Commvault, Druva — now market "ransomware recovery" as a primary use case.

Key engineered features: immutable storage (object-lock, hardware WORM), anomaly detection on backup data (entropy spikes), air-gapped vault tiers, orchestrated recovery with malware scanning.

**2 )** **Common architectural failures:**
- Backup admin credentials reused with production AD
- Backup target on a domain-joined Windows file share
- Backups not tested for restore
- Immutable retention windows shorter than attacker dwell time

**3 )** **Key standards and frameworks:**
- **NIST CSF 2.0 (Feb 2024)** — Adds a Govern function; re-emphasizes supply-chain risk
- **CIS Controls v8.1** — Prioritized control list with explicit ransomware overlays; widely cited in insurance questionnaires
- **MITRE ATT&CK** — The de facto vocabulary for adversary techniques and detection engineering
- **MITRE D3FEND** — The defensive counterpart, mapping countermeasures to ATT&CK techniques
- **ISO/IEC 27001 and 27035** — International standards for ISMS and incident management

## Task 7.8 – Discussion Challenge: Incident Response Ecosystem Mapping

> *"Pick any single notable incident from Challenge 4 and identify the defender-side categories from this challenge that would have been engaged in the response."*

**Work through the following in your discussion document:**

**Q1.** Select **one incident** from Challenge 4 (WannaCry, Colonial Pipeline, MGM Resorts, Change Healthcare, or Synnovis). For each defender category below, state whether it was **engaged**, **absent**, or **unknown** in that incident's response — and explain why its presence or absence shaped the outcome.

| Defender Category | Engaged / Absent / Unknown | Impact on Outcome |
|---|---|---|
| EDR/XDR | | |
| MDR | | |
| Threat intelligence | | |
| Incident response firm | | |
| Ransomware negotiator | | |
| Cyber insurance | | |
| Legal / breach counsel | | |
| Government / law enforcement | | |
| Backup and recovery | | |

**Q2.** As the CISO of that organization, which **two defender categories** would you have wanted fully operational *six months before the incident* — and what would each have cost you annually? (Rough orders of magnitude are acceptable — the point is to connect the categories to real budget decisions.)

**Q3.** The CrowdStrike outage of July 2024 was not a security event, but it demonstrated that **concentration risk in the EDR market** is itself a systemic vulnerability. How do you balance the security benefits of consolidating onto a single EDR platform against the concentration risk that consolidation creates? What governance or architectural decisions mitigate both risks simultaneously?

---

# Task 8: Economic Snapshot & Strategic Questions

In this module you will complete **Economic Snapshot & Strategic Questions** by reading and reflecting on **the numbers that frame the ransomware market** and engaging with **six scenario-based strategic questions that test your organization's readiness**.

> [!NOTE]
> All figures are current as of mid-2026 and drawn from primary sources (Chainalysis, Coveware, IBM, Munich Re). Treat them as orders of magnitude — re-verify against the cited sources before using any number in a deliverable. These figures *will* drift; the structural observations they support are more durable than the specific values.

## Task 8.1 – Economic Snapshot: Numbers Worth Knowing

**1 )** **Confirmed ransomware payments (crypto-traceable)**

| Year | Confirmed Payments | Source |
|---|---|---|
| 2022 | ~$567 million | Chainalysis |
| 2023 | ~$1.0 billion | Chainalysis |
| 2024 | ~$1.1 billion | Chainalysis |
| 2025 | Trending higher | Chainalysis (est.) |

These are confirmed figures — actual payments are higher, as attribution continues to improve retroactively.

**2 )** **Attack economics**

| Metric | Order of Magnitude | Source |
|---|---|---|
| Average ransom paid | Six- to seven-figure range; volatile by quarter and victim profile | Coveware |
| Median ransom paid | Materially below the average — the distribution is heavy-tailed | Coveware |
| Share of victims who pay | Declining trend; well under half in recent quarters | Coveware |
| Average operational downtime | Two to three weeks; full restoration longer | Coveware / Sophos |
| Average total cost of a ransomware breach (excl. ransom) | Multi-million USD; healthcare and finance higher | IBM Cost of a Data Breach Report |

**3 )** **Market context**

| Metric | Order of Magnitude | Source |
|---|---|---|
| Global cyber-insurance gross written premium | Roughly $15B+ and growing, high U.S. concentration | Munich Re / Marsh / NAIC |
| EDR/XDR market annual revenue | Tens of billions across vendors | Vendor financials / Gartner |
| Victim organizations posted on data-leak sites | Several thousand per year, growing year-over-year | ransomware.live / ransomwatch |

**Two structural observations:**

**1 )** The share of victims who pay is *decreasing* while total payments are roughly flat-to-up — indicating that the population of paying victims is shifting toward **larger, harder-hit organizations**. The "spray and pray" era is over; the economics now favor big game hunting.

**2 )** The cyber-insurance market is consolidating around a smaller set of carriers with **sophisticated underwriting** — a strongly positive force for security maturity in the mid-market. Insurance questionnaires are now one of the most effective mechanisms for getting security controls adopted in organizations that wouldn't otherwise prioritize them.

> [!IMPORTANT]
> The declining payment rate is not primarily a sign that organizations are better defended — it is partly a sign that **insurers and boards have changed their payment calculus**, and that some organizations have genuinely improved backup posture. The attacker response has been to move up-market to larger victims where the ransom math still works.

## Task 8.2 – Strategic Scenario Questions

These six questions close Module 1. They are the seeds of the tabletop scenario library that will be built in later modules. **Do not treat them as rhetorical** — each one should produce a concrete, organization-specific answer.

Work through each question in your discussion document. Where your answer is *"I don't know"* or *"we haven't documented this,"* that gap is the starting point for a work item.

---

**Scenario 1 — Middle-of-the-Night Incident**

> *You receive a call at 02:00 on a Saturday. An English-speaking crew has social-engineered your help desk and is actively moving through your environment. Ransomware deployment is estimated to be 2–4 hours away.*

**Q1a.** Who is in the incident bridge right now — by name and role? Who has authority to engage breach counsel?

**Q1b.** How long does it take to get your IR retainer firm on the call? Is the retainer number in your runbook? Is the runbook accessible outside the corporate network?

**Q1c.** What is the first containment action — and who has the authority to execute it without a committee decision at 02:00?

---

**Scenario 2 — Backup Failure**

> *Your backups were encrypted alongside production. The immutability claim in your backup vendor contract has a clause you didn't read. Recovery from your last-known-good backup will take six weeks.*

**Q2a.** What is your actual recovery time objective in practice — not the number in the DR plan — and which business processes run on Tier-2 hardware (manual, paper, degraded) while you restore?

**Q2b.** Which business processes have **no** Tier-2 fallback? For those, what is the customer, regulatory, and financial impact of a six-week outage?

**Q2c.** When did you last run a full restore test end-to-end in a non-production environment? What was the result?

---

**Scenario 3 — The Ransom Decision**

> *The threat actor is demanding $10 million. Your negotiator has worked them to $2 million. Your insurance carrier will cover up to $1.5 million. Recovery from backup is possible but will take four weeks.*

**Q3a.** Who decides whether to pay — by name and title? Is that decision-making authority documented in your IR plan?

**Q3b.** Is the payment mechanism rehearsed? Who holds the cryptocurrency wallet? Has your team ever executed a crypto payment under operational pressure?

**Q3c.** At what point in the negotiation does your legal team conduct OFAC sanctions screening on the threat actor? What changes to your response if the screening flags a match?

---

**Scenario 4 — The Data Leak**

> *The threat actor publishes 200GB of customer data on their leak site. The data includes PII, health records, and contractual information. Media coverage begins within hours.*

**Q4a.** What is your **notification stack** — regulators, customers, business partners, employees, the press — and in what order do they receive notification?

**Q4b.** Which jurisdictions does your customer data span? Which of those jurisdictions have mandatory notification timelines (GDPR 72 hours, SEC 4 business days, HIPAA, state breach laws)?

**Q4c.** Who owns the press statement? Is the communications team in the incident bridge from hour one, or are they brought in later? What is the consequence of the difference?

---

**Scenario 5 — The OFAC Complication**

> *Mid-negotiation, your sanctions screening reveals that the threat actor group is linked to an OFAC-designated entity. The decryptor has not yet been received. Your insurance carrier is asking for a compliance opinion before authorizing any payment.*

**Q5a.** What changes for your organization — operationally, legally, and financially — when an OFAC link is confirmed mid-negotiation?

**Q5b.** Have you tested this branch of the runbook in a tabletop? If not, this is the first tabletop scenario you should run with breach counsel and your IR retainer before your next policy renewal.

**Q5c.** If payment is blocked, what is your recovery path? Does your answer to Scenario 2 change the calculus?

---

**Scenario 6 — The Insurance Dispute**

> *Your cyber-insurance carrier is declining coverage on the claim. They allege that the MFA controls stated on your application were not actually deployed on the compromised Citrix portal — which is true.*

**Q6a.** What is your legal position? Who drafted your insurance application, and who signed it? Was there a technical attestation process?

**Q6b.** What is the financial exposure if the claim is declined — IR costs, business interruption, ransom payment, regulatory fines, legal fees?

**Q6c.** What process change would prevent this outcome on your next policy renewal? Who owns the security-controls attestation process in your organization?

> [!IMPORTANT]
> Scenario 6 is not hypothetical. Change Healthcare's breach exposed that a single unchecked Citrix portal with no MFA — despite MFA being a standard requirement in virtually every enterprise cyber-insurance policy — could result in both a catastrophic breach and a coverage dispute. The gap between the stated control posture on the application and the actual control posture in production is one of the most common failure modes in cyber-insurance claims.

## Task 8.3 – Module Summary & What Comes Next

**What you covered in Module 1:**

**1 )** **Origins & Evolution** — 35 years of ransomware history, from the AIDS Trojan to the modern RaaS supply chain.

**2 )** **Attack Anatomy** — The eight-stage kill chain, hybrid encryption mechanics, and the detection signals that fire before the ransom note.

**3 )** **Notable Incidents** — Eleven case studies, each with a concrete lesson for the defensive playbook.

**4 )** **Mitigation Framework** — The controls that consistently appear in post-incident reviews as decisive: phishing-resistant MFA, patching against KEV, EDR with auto-response, segmentation, immutable backups, tested IR plans, and retainers.

**5 )** **The Attacker Industry** — The criminal supply chain: RaaS operators, affiliates, IABs, infostealers, bulletproof hosting, cryptocurrency laundering, and the geopolitical safe harbors that protect operators.

**6 )** **The Defender Industry** — The commercial and institutional response: EDR/XDR, MDR, threat intel, IR firms, negotiators, cyber insurance, legal and regulatory obligations, and government law-enforcement operations.

**7 )** **Economics & Strategy** — The numbers that frame the market, and six scenario-based strategic questions that test organizational readiness.

**Suggested next research modules:**

| Module | Topic |
|---|---|
| Module 2 | Adversary Tradecraft Deep Dive — MITRE ATT&CK techniques, telemetry sources, and detection content |
| Module 3 | Negotiation Playbook — Opening, pace, leverage, sanctions screening, and exit conditions |
| Module 4 | Recovery Architecture — Backup design, immutable storage, isolated recovery, and IT/OT recovery sequencing |
| Module 5 | Legal & Regulatory Playbook — SEC, CIRCIA, NIS2, DORA, GDPR, and HIPAA matrices |
| Module 6 | Tabletop Scenario Library — One-page briefs, inject schedules, decision points, and after-action guides |

> [!TIP]
> The six strategic questions in Task 02 are the first six scenarios in the Module 6 tabletop library. If your team completes this module together, use the answers you wrote to those questions as the pre-brief for your next tabletop exercise. The gaps you identified *are the exercise*.

# 🏁 End of Training Lab

**Module 1 — Ransomware Ecosystem — Complete**

You have completed the Foundations & Ecosystem Map module. The research briefing that underpins this module is a living document — figures and brand names will change. The structural observations (ransomware as an economic phenomenon, the resilience of the affiliate base to operator disruption, the unglamorous nature of the decisive controls) are the durable takeaways.

Return to this material before your next tabletop exercise, insurance renewal, or board presentation on cyber risk.
