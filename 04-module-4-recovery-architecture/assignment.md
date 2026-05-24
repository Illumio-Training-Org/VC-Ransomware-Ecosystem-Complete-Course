---
slug: module-4-recovery-architecture
id: hzw8zrxndy9z
type: challenge
title: 'Module 4: Recovery Architecture'
teaser: Backup architecture, immutable storage, isolated recovery environments, identity
  recovery, and the RTO/RPO framework — building infrastructure that survives the
  attack.
notes:
- type: text
  contents: |-
    # 322 – Ransomware Ecosystem

    **Module 4 — Recovery Architecture**

    Welcome to Module 4. Module 3 covered the negotiation room. This module covers what happens when you decide not to pay — or when payment alone is not enough: getting back online.

    Recovery from ransomware is not recovery from a hardware failure. The attacker has been resident, backups are a primary target, and identity is likely compromised. Every architectural choice in this module flows from that reality.

    **This track has no virtual machines or hands-on labs.**
    Your work in each challenge is to read the content, engage with the design decisions, and answer the discussion questions.

    **Estimated module time:** 3–4 hours
    **Challenges in this track:** 6
difficulty: ""
timelimit: 7200
lab_config:
  default_layout_sidebar_size: 0
enhanced_loading: null
---

# Task 1: Introduction & The Recovery Problem Statement

In this module you will complete **Module 4 of the Ransomware Ecosystem course** by reading and reflecting on **recovery architecture for adversarial scenarios** and engaging with **the design decisions that determine whether recovery takes days or months**.

## Task 1.1 – The Premise

One sentence frames every architectural decision in this module:

> *If your recovery plan only works when the attacker doesn't reach the backup admin plane, you have a ransom-payment plan, not a recovery plan.*

**1 )** Design for the case where the attacker has **full domain admin** and **active access to your backup infrastructure** for a week before encryption fires.

**2 )** This is not a theoretical worst case. Mandiant M-Trends 2024 reports median ransomware dwell times in the days-to-weeks range — long enough for an attacker to inventory backup infrastructure, harvest credentials, and stage destructive actions against it.

**3 )** Most recovery failures are not failures to have backups. They are failures of **backup architecture** — the backup existed, but was domain-joined, used shared credentials, had no immutable tier, or had never been tested for full restoration.

> [!IMPORTANT]
> The dominant industry findings reinforce this: ransomware operators target backups as a first-class objective. Most incidents involve an attempt to compromise the backup repository. Immutable and air-gapped backups are the strongest single predictor of successful recovery without payment.

## Task 1.2 – Why Ransomware Recovery is Different

Recovery from ransomware has five characteristics that distinguish it from routine recovery and demand different architecture:

**1 )** **The attacker has been resident.** The malware did not arrive and encrypt immediately — it lived inside the network, moving laterally and escalating privileges. Any system it touched is potentially compromised. Any backup taken during the dwell period may contain persistence mechanisms.

**2 )** **Backups are a primary target.** This is not incidental. Operators delete shadow copies, hunt for backup servers, harvest backup admin credentials, and stage destructive actions against backup repositories before detonating the locker. The backup system is part of the attack surface.

**3 )** **Recovery time is dominated by validation, not restore.** Restoring bytes is fast on modern infrastructure. Verifying that what you restored is **clean** — no implants, no persistence, no attacker artifacts — is the time-consuming step that most RTO estimates ignore.

**4 )** **Identity is the recovery substrate.** Active Directory and identity providers are typically compromised in serious incidents. You cannot trust restored systems that authenticate against a compromised identity layer. Recovering identity safely is a precondition for recovering everything else.

**5 )** **Business decisions are entangled with technical decisions.** Manufacturing, healthcare, and OT recovery have safety constraints. Hospitals cannot restore clinical systems in an order that creates patient-safety hazards. Utilities cannot restore SCADA before the safety-instrumented systems are confirmed clean. Recovery sequencing is a business and safety problem, not just a technical one.

> [!NOTE]
> Average operational downtime from a significant ransomware event continues to be measured in weeks, not days. Full restoration to pre-incident state is sometimes measured in months. These are the planning parameters — not the aspirational RTO on paper, but the industry-observed reality for organizations without mature recovery architecture.

## Task 1.3 – What "Good" Recovery Architecture Looks Like

This module covers six areas. Each challenge maps to one:

| Challenge | Topic | The Core Question |
|---|---|---|
| 2 | Backup Architecture | Do you have a tier that survives privileged attacker access? |
| 3 | Immutable Storage | Is at least one copy technically impossible to delete during an attack? |
| 4 | Isolated Recovery Environment | Can you rebuild without touching the compromised production environment? |
| 5 | Recovery Sequencing & Identity | Do you know the right order — and can you recover identity first? |
| 6 | Restore Testing, RTO/RPO & Readiness | Have you actually tested it, and are the numbers real? |

**1 )** Each challenge ends with discussion questions. Write answers in your team discussion document.

**2 )** The pre-incident readiness checklist at the end of Challenge 6 is the output of this module. It identifies the gaps your recovery program has before you need to use it.

**3 )** Module 6 Scenario 1 ("The 03:00 Detonation") and Scenario 5 ("The Supply Chain Compromise") exercise recovery sequencing under time pressure. The first time you discover your IRE rehearsal was six months out of date should be in a tabletop, not the incident.

## Discussion Questions — The Recovery Problem Statement

**Q1.** The module premise is: design for the case where the attacker has full domain admin and active access to backup infrastructure for a week before encryption fires. Honestly assess your current recovery architecture against this premise. At what point in that scenario does your recovery plan fail?

**Q2.** Recovery time is dominated by validation, not restore. What is your current process for verifying that a restored system is clean before re-connecting it to production? If the answer is "we run an AV scan," what are the limits of that approach against a sophisticated attacker who has been resident for a week?

**Q3.** Identify the three business processes in your organization where a two-week outage would cause the most severe harm — financial, safety, regulatory, or reputational. Do those three processes have explicit, documented, and tested recovery paths that are independent of the rest of the environment?

---

# Task 2: Backup Architecture

In this module you will complete **Backup Architecture** by reading and reflecting on **the four-tier backup reference architecture for adversarial environments** and engaging with **the common design failures that turn backup investments into false confidence**.

## Task 2.1 – The 3-2-1 Baseline and Why It's Not Enough

**3-2-1** — three copies of data, on two different media, with at least one off-site — originated in a pre-ransomware threat model. It remains a useful starting point, but on its own it does not survive an attacker with privileged access.

**1 )** A 3-2-1 architecture with all three copies reachable via the same admin credentials, on domain-joined systems, and without immutability can be destroyed by an attacker with domain admin in under an hour.

**2 )** The failure mode is not conceptual — it is architectural. The number of copies is less important than the **isolation** between them and the **immutability** of at least one.

Augment 3-2-1 with three additional properties:

**1 )** **Immutability.** At least one copy on storage where deletion or modification is denied at the infrastructure level — by anyone, including a privileged administrator — for a defined retention window.

**2 )** **Air-gap or strong isolation.** At least one copy that is not reachable from the production network when not actively being written to or read from.

**3 )** **Separation of identity.** Backup infrastructure authenticated by a different identity provider — or at minimum a tier-0-isolated subset of AD with separate administrators and step-up MFA.

> [!IMPORTANT]
> The three augmentations are not independent. Immutability without identity separation can be defeated by compromising the backup admin account and disabling the lock. Identity separation without immutability still leaves backups vulnerable to privileged deletion. All three are required in combination.

## Task 2.2 – The Four-Tier Reference Architecture

A defensible 2026 backup architecture has four tiers. An attacker compromising one tier does not cascade into the others.

**1 )** **Tier 1 — Operational Backups**

Fast, on-prem or close-to-prem. Used for routine restores (deleted file, broken VM). Daily-to-hourly cadence. May be reachable from production for performance reasons — this tier is expected to be compromised in a serious incident.

**2 )** **Tier 2 — Resilient Backups (the critical tier)**

- **Immutable.** Object-locked, with retention long enough to exceed expected attacker dwell time (90+ days recommended).
- **Separate identity tier.** Backup management plane runs on privileged-access-workstation pattern, authenticated by a separate identity boundary.
- **Audited.** Any action that could reduce immutability — change retention, delete vault, alter roles — generates an alert.

**3 )** **Tier 3 — Vault / DR Copy**

- Air-gapped or strongly isolated, geographically separated.
- Tape, offline disk, or cloud account on completely separate credentials and billing.
- Restore from this tier must be tested at minimum annually.
- This is the tier you use if Tier 1 and Tier 2 are both destroyed.

**4 )** **Tier 4 — Critical-System Cold Copy (optional but increasingly common)**

A weekly or monthly capture of the absolute essentials: domain controllers, identity infrastructure, critical application databases, source code, financial systems. Stored on separate media with its own lifecycle, not referenced in the same documentation as Tier 1–3. This is the data you restore first, on the most isolated infrastructure, to stand up the skeleton of the environment.

> [!TIP]
> Think of the four tiers as concentric rings of difficulty for the attacker. Tier 1 is expected to fall. Tier 2 should survive domain admin compromise. Tier 3 should survive destruction of on-prem infrastructure entirely. Tier 4 is the last resort that you have absolute confidence in — even if you haven't touched it in a month.

## Task 2.3 – Common Architectural Failures

These seven failures appear repeatedly in post-incident reviews. Read this list as a gap assessment for your current architecture.

**1 )** **Backup management server domain-joined to production AD.**
A domain admin compromise equals backup compromise. The fix: move the backup admin tier to a separate identity boundary. This is the single most common failure pattern.

**2 )** **Backup target on a Windows file share.**
Default-deny ACLs on a file share are bypassed by an attacker with domain admin. Move backup targets to immutable object storage (S3 object lock, Azure Blob immutable, GCS bucket lock) or hardware WORM appliances.

**3 )** **Same admin credentials for production and backup.**
Separate them with a hard process boundary. Backup admin credentials should be stored in a separate credential vault, not in the same PAM system used for production.

**4 )** **Immutable retention period shorter than dwell time.**
If retention is 14 days but the attacker dwells for 21 days before encrypting, your "immutable" copy is already from a period of attacker residence. Extend retention to 90+ days to provide a high-confidence clean restore point.

**5 )** **Backups encrypted with keys stored in production identity.**
If the attacker compromises the production KMS or the admin account that controls encryption keys, the backup encryption is defeated. Move backup encryption keys to a separate KMS with M-of-N break-glass access.

**6 )** **No restore testing.**
A backup that has not been restored is a hypothesis. Industry surveys consistently find that significant proportions of organizations cannot fully restore from primary backups when actually required. Test quantitatively, not qualitatively.

**7 )** **Backup catalog stored in production.**
Without the catalog, valid backups become an unnavigable data set. Store the catalog separately — ideally in the same tier as Tier 2 or Tier 3 backups, where it is immutable and independently accessible.

> [!NOTE]
> Failures 1 and 7 are often invisible until an incident. Backup software dashboards typically show green status regardless of whether the architecture is actually resilient. The gap assessment at the end of this challenge is designed to surface these failures before the incident does.

## Discussion Questions — Backup Architecture

**Q1.** Map your current backup architecture against the four-tier reference. Which tiers exist, which are missing, and — most importantly — does any single compromise (domain admin, backup admin, cloud IAM) cascade across more than one tier? If yes, that is the gap that needs closing first.

**Q2.** Check failure pattern 4: what is the retention period of your immutable backup tier, and what is the longest documented attacker dwell time in an incident relevant to your sector? If retention is shorter than that dwell time, what is the remediation path and who owns it?

**Q3.** Failure pattern 6 (no restore testing) is the most common and most dangerous. When did your organization last perform a full restore of a critical application stack — database, application server, and dependencies — end-to-end? What was the result? If the answer is "never" or "more than a year ago," what would it take to schedule one in the next 60 days?

---

# Task 3: Immutable Storage

In this module you will complete **Immutable Storage** by reading and reflecting on **the mechanisms that make a backup technically impossible to delete or modify during an attack** and engaging with **the implementation decisions and failure modes specific to each mechanism**.

> [!NOTE]
> Immutability is the single highest-impact backup property against ransomware. Every other backup design decision is secondary to having at least one copy that cannot be deleted or overwritten — by anyone, including a privileged administrator — for a defined retention window. This challenge covers how to achieve that, and where each mechanism can fail.

## Task 3.1 – The Five Immutability Mechanisms

**1 )** **Cloud Object Lock (S3 / Azure Blob Immutable / GCS Bucket Lock)**

Cloud object stores expose a per-object or per-bucket retention period. During the lock window, delete and overwrite operations are denied at the API level — even by the account root or storage admin.

- **Strengths:** Strong isolation from on-prem compromise. Retention period is configurable. Governance mode (can be extended) vs. Compliance mode (cannot be shortened or removed, even by root) — use Compliance mode for ransomware protection.
- **Failure modes:** Account-level IAM compromise that allows an attacker to disable the lock or change retention settings. Root account access that bypasses IAM controls. Ensure root account has MFA, hardware token, and is never used for routine operations.

**2 )** **Hardware WORM Appliances**

Purpose-built write-once-read-many storage: Dell PowerProtect / Data Domain Retention Lock, HPE StoreOnce Catalyst, NetApp SnapLock, IBM Spectrum Protect Plus.

- **Strengths:** Strong on-prem option. Physical enforcement of write protection. Well-tested in regulated industries (financial, healthcare) with compliance audit trails.
- **Failure modes:** Management-plane compromise — the appliance admin console can typically modify retention settings if the admin account is compromised. Vendor-specific bypass CVEs in management firmware. Protect the management plane with separate credentials and step-up MFA.

**3 )** **Vendor-Native Immutability (Veeam, Rubrik, Cohesity, Commvault, Druva)**

Purpose-built backup platforms with immutable repository tiers built into the product.

- **Strengths:** Operationally convenient. Integrated with the backup workflow. Rubrik and Cohesity in particular have designed immutability as a core property, not an add-on.
- **Failure modes:** The most important question to ask: *can immutability be revoked from the same console that manages day-to-day backup operations?* If yes, a compromised backup admin account can revoke it. Verify that immutability configuration requires a separate, elevated identity action — not just admin access to the backup console.

**4 )** **Tape**

Physical tape rotated to off-site storage or a vault on a defined cadence.

- **Strengths:** Strongly air-gapped by nature. Survives a wide range of attack scenarios including complete destruction of on-prem infrastructure. The attacker cannot reach it over a network. Long retention at low cost.
- **Failure modes:** Operationally inconvenient — restore times are slow. Discipline-dependent: the rotation schedule must be followed rigorously. Tapes at the backup site are protected; tapes still in the drive or queued for rotation are not.

**5 )** **Air-Gapped Offline Disk**

Physical disk drives rotated to a vault on a defined cadence, mounted only for the copy window, disconnected and physically secured otherwise.

- **Strengths:** Faster restore than tape. Similar threat model — network-isolated when not in use. Manageable for organizations that cannot operationalize tape.
- **Failure modes:** Rotation discipline is everything. A disk left connected after the copy window is no longer air-gapped. Physical security of the vault matters — an attacker with physical access to the facility defeats this control.

## Task 3.2 – The One Hardening Step Every Implementation Requires

All five mechanisms share a single architectural dependency: the **management plane**.

Every immutable storage system has an administrative interface that can configure retention periods, alter user roles, disable locks, or delete vaults. If that management plane is accessible with the same credentials as the rest of the environment, immutability is conditional — it holds only until the attacker reaches the admin account.

**The single most important hardening step for any immutable storage implementation:**

**1 )** **Require step-up authentication** — ideally a separate identity tier — for any action that could reduce immutability:
- Change retention period
- Delete or disable a vault
- Alter admin user roles
- Disable or bypass lock settings

**2 )** **Audit those actions and alert on them.** Any attempt to reduce immutability during an incident is a high-confidence signal that an attacker has reached the backup management plane. Alert with high priority; treat as an active compromise indicator.

**3 )** **Test the enforcement.** With a test account that has backup admin rights, attempt to delete an object within the retention window. The attempt should fail at the API level. If it succeeds, the immutability implementation has a gap.

> [!IMPORTANT]
> Cloud object lock in Compliance mode is the strongest mechanism available to most organizations today — because it cannot be overridden by anyone, including the cloud account root, during the retention window. This is a technical guarantee, not a policy guarantee. Compare this to Governance mode, where a privileged account can override the lock. Use Compliance mode for your ransomware-recovery tier.

## Task 3.3 – Mechanism Selection for Your Environment

Use this framework to select the right mechanism — or combination — for your environment.

| Question | Recommended mechanism |
|---|---|
| Primarily cloud infrastructure? | Cloud object lock (Compliance mode) as Tier 2; consider cloud-to-cloud for Tier 3 |
| Primarily on-prem with a large backup vendor already deployed? | Vendor-native immutability (verify management-plane hardening) + tape or offline disk for Tier 3 |
| Regulated industry with compliance audit requirements? | Hardware WORM appliance (SnapLock, Data Domain Retention Lock) for documented compliance |
| Highest-assurance requirement, slow restore acceptable? | Tape rotation to off-site vault |
| Mixed cloud/on-prem? | Cloud object lock for Tier 2 (cloud-native systems) + hardware WORM or vendor-native for Tier 2 (on-prem systems) + tape for Tier 3 |

**1 )** No single mechanism is universally correct. The selection depends on your infrastructure model, operational capability, restore-time requirements, and regulatory environment.

**2 )** Mechanisms can be combined. Cloud object lock for operational cloud data + tape for the vault tier is a common and effective combination.

**3 )** The mechanism is less important than the **management-plane hardening**. A poorly hardened cloud object lock account is weaker than a well-hardened WORM appliance.

## Discussion Questions — Immutable Storage

**Q1.** For each immutable storage mechanism your organization currently uses (or is considering), answer: Can immutability be reduced or disabled from the same account used for routine backup operations? If yes, what is the remediation — and who owns it?

**Q2.** The module recommends alerting on any attempt to modify immutability settings as a high-confidence incident indicator. Does your SIEM or backup platform currently generate this alert? If not, what would it take to create it — and how would that alert be triaged at 02:00 on a weekend?

**Q3.** Cloud object lock in Compliance mode provides a technical guarantee that no one — including the account root — can delete the object during the retention window. What is the attack path that defeats this guarantee, and what compensating control addresses it?

---

# Task 4: Isolated Recovery Environment

In this module you will complete **Isolated Recovery Environment** by reading and reflecting on **the design and operational use of a clean-room recovery environment** and engaging with **the architectural properties that allow recovery to run in parallel with eradication**.

> [!NOTE]
> The Isolated Recovery Environment (IRE) — also called a clean room or isolated recovery environment — is a separate environment used to bring critical systems back online before reconnecting to compromised production infrastructure. Organizations that recovered fastest from major incidents had architectural separation that allowed reconstitution to happen in parallel with eradication. Maersk's NotPetya recovery is the canonical case study.

## Task 4.1 – Why the IRE Matters

Without an IRE, recovery from a significant ransomware event follows a sequential path: eradicate → rebuild → validate → restore → reconnect. Each step depends on the previous one being complete. Total time is the sum of all steps.

With an IRE, recovery and eradication happen in parallel. While the security team eradicates the attacker from production, the recovery team is already rebuilding critical systems in the clean environment. Cut-over occurs when both tracks are ready.

**1 )** **Maersk — NotPetya (June 2017).** Maersk lost nearly every domain controller globally. They rebuilt the AD forest in approximately 10 days using a single backup domain controller found in Ghana that happened to be offline when the wiper ran. The ability to stand up a clean identity environment — separate from production — was the enabling condition for their recovery. Without it, recovery would have taken weeks longer.

**2 )** **The core enabling property:** The IRE has its own identity provider. Systems restored into the IRE authenticate against clean infrastructure, not the compromised production AD. This means validation and testing can happen without touching the compromised environment.

**3 )** **The parallel recovery model:** While production is being eradicated and cleaned, the IRE is being populated with restored systems. Cut-over is a network plumbing operation — the IRE becomes the new production — rather than a sequential rebuild.

> [!IMPORTANT]
> The IRE is not a DR site. A DR site replicates production infrastructure and activates when production fails. The IRE is a clean-room environment built from scratch, from trusted media, with its own identity — specifically designed for the scenario where production is assumed to be fully compromised.

## Task 4.2 – IRE Design Properties

**1 )** **Separate physical or logical environment with its own identity provider.**

The IRE has its own AD forest or cloud identity tenant, provisioned from clean media. It shares no credentials, no trusts, and no administrative access with the production environment. This is the non-negotiable property — without it, the IRE is not isolated.

**2 )** **One-way data ingress only.**

Restored data flows from the immutable backup repository into the IRE. There is no return path during recovery. This prevents any attacker persistence that survived in the backup data from propagating back to the recovery environment or to other systems.

**3 )** **Hardened gold-image build pipeline, stored immutable.**

Operating system images, application installation packages, and configuration baselines are stored in the same immutable repository as backup data. Systems in the IRE are built from these clean images — not restored from potentially compromised production images.

**4 )** **Out-of-band communication.**

The IRE has its own administrative laptops, never connected to production. Communication among the recovery team — including bridges with the IR firm, legal, and executive stakeholders — runs through a separate communication channel (separate email tenant, personal devices, or a temporary cloud collaboration tenant).

Some organizations maintain a sealed cabinet with pre-configured IRE admin hardware, opened only on declared incident.

**5 )** **Documented entry and exit criteria for systems re-joining production.**

Before a system leaves the IRE and re-joins production, it must pass: a clean malware scan, a forensic artifact review, fresh credential issuance, and a defined observation period with monitoring enabled. The criteria are documented before the incident — not improvised during.

> [!TIP]
> If you have never built and exercised an IRE, start with a tabletop version: document what the IRE would look like for your three most critical systems, where the clean identity would come from, and what the cut-over steps would be. The documentation gap you find in that exercise is worth more than any technical control you could add.

## Task 4.3 – What You Actually Do in the IRE During an Incident

The IRE becomes operational when an incident is declared and the recovery track begins. The steps below are the standard sequence — adapt to your environment.

**1 )** **Stand up identity from clean media.**

Provision the clean AD forest or cloud identity tenant from the gold images and configuration baselines stored in the immutable repository. This is the first action — nothing else can be restored until there is a trusted identity layer to authenticate against.

**2 )** **Restore domain controllers, core infrastructure in priority order.**

DNS, DHCP, certificate authorities, secrets management, and any PKI infrastructure required for application authentication. The recovery sequencing challenge (Challenge 5) covers the detailed wave ordering.

**3 )** **Run forensic and malware sweeps against restored data before exposing it.**

Before restored data is accessible to any user or application in the IRE, it undergoes: AV/EDR scan with updated signatures, forensic artifact review by the IR firm, and hash verification against known-clean baselines where available.

This step is the primary reason recovery takes longer than raw restore time. Do not skip it. Restored data that contains attacker persistence will re-infect the IRE.

**4 )** **Validate application functionality with a small test cohort.**

Before cut-over, a subset of users accesses the restored applications in the IRE to confirm functionality. This catches configuration issues, data corruption, and missing dependencies before they affect the entire organization.

**5 )** **Cut over by network plumbing.**

When the IRE is validated and eradication of the production environment is sufficiently complete, cut-over is executed by network reconfiguration — routing production traffic to the IRE infrastructure. The IRE becomes the new production. The former production environment continues eradication, forensic preservation, and eventual decommissioning or rebuild.

## Discussion Questions — Isolated Recovery Environment

**Q1.** Your organization currently has no IRE. A critical ransomware event occurs and your production AD is fully compromised. Walk through the steps you would take to stand up a clean identity environment for recovery — where would the clean AD baseline come from, what hardware would you use, and how long would it realistically take? Identify the biggest gap in that plan.

**Q2.** The IRE requires out-of-band communication — separate laptops, a separate communication tenant, or personal devices. Does your organization have this capability today? If recovery team members need to coordinate using compromised email and collaboration infrastructure, what information is at risk, and how does that affect the recovery operation?

**Q3.** The forensic and malware sweep of restored data (Step 3) is the primary driver of recovery time beyond raw restore speed. In your environment, who would perform this sweep, what tools would they use, and what would their throughput be in terabytes per day? If you have 50TB of data to validate, what does that mean for your realistic recovery timeline?

---

# Task 5: Recovery Sequencing & Identity Recovery

In this module you will complete **Recovery Sequencing & Identity Recovery** by reading and reflecting on **the order in which systems must come back online** and engaging with **why identity recovery is the unsung bottleneck that most ransomware programs underweight**.

> [!NOTE]
> Sequencing is what separates a clean recovery from one that breaks the business twice. Restore the wrong system first and you introduce attacker persistence into your clean environment. Restore in the wrong order and you create application dependencies that block critical services. Sequence matters — and it must be documented before the incident.

## Task 5.1 – Recovery Wave 1: Identity and Core Infrastructure

Nothing else recovers until Wave 1 is complete. Every system that authenticates — which is every system — depends on this layer.

**1 )** **Active Directory / Entra ID restored from a known-clean point.**

The identity layer is the foundation. In a serious incident, existing AD cannot be trusted. Stand up clean identity in the IRE (Challenge 4) and validate before anything else authenticates against it.

**2 )** **DNS, DHCP, certificate authorities, KMS / secrets management.**

DNS is a dependency for almost every application. Certificate authorities are a dependency for any application using TLS mutual authentication or client certificates. KMS is a dependency for any application using encrypted data. These come before the applications that depend on them.

**3 )** **Out-of-band communication: email and chat back, even on a temporary tenant.**

If the incident has encrypted your production email and collaboration infrastructure, recovery coordination becomes blind. Establish a communication channel — a temporary Microsoft 365 tenant, a separate Google Workspace, or personal devices and consumer tools — before Wave 2 begins. Stakeholder updates, IR firm bridges, and regulator notifications all depend on this.

**4 )** **Privileged access workstations and break-glass accounts re-issued.**

Restore privileged access infrastructure before restoring the systems that depend on it. New PAM credentials, new break-glass accounts, new admin workstations — all provisioned from clean media and registered against the clean identity layer.

> [!IMPORTANT]
> Wave 1 is the longest wave in most significant ransomware recoveries, even though the data volume is small. Identity recovery is procedurally complex, requires rehearsal, and cannot be rushed without reintroducing the attacker's footholds. Do not cut corners on Wave 1 to accelerate Wave 2.

## Task 5.2 – Recovery Waves 2–4

**Wave 2 — Financial, Customer-Facing, and Regulated Systems**

**1 )** ERP, billing, payroll, and any system with explicit regulatory recovery-time obligations are the first application-layer systems to restore — because they carry the highest financial and regulatory impact from extended outage.

**2 )** Customer-facing portals are brought up under degraded service if necessary — limited functionality, read-only modes, or manual fallback processes — while full restoration is in progress.

**3 )** Regulator and customer notification timelines must be met regardless of internal recovery state. If SEC 4-day or CIRCIA 72-hour clocks are running, those notifications happen on the clock — not after recovery is complete.

**Wave 3 — Collaborative and Analytic Systems**

**1 )** File shares, internal collaboration platforms (Teams, SharePoint, Confluence), BI and analytics systems.

**2 )** These are high-volume but lower-criticality. Most organizations can operate for days without analytics or internal file shares. Restore them after regulated and customer-facing systems are stable.

**Wave 4 — OT / ICS (in mixed IT/OT environments)**

**1 )** Operational technology has different rules. **Safety-instrumented systems come back first** — because they are the last line of defense against physical hazard in manufacturing, utilities, and healthcare environments. OT restoration without SIS confirmation is a safety incident, not a recovery.

**2 )** Engineering workstations, historians, and HMIs require their own restoration paths. ICS-CERT and vendor-specific guidance for the exact systems deployed. Do not apply IT recovery procedures to OT without vendor validation.

**3 )** Physical-process recovery may have its own runbook entirely separate from IT recovery — manufacturing line restart procedures, hospital paper-based fallback protocols, utility load-balancing coordination with the grid operator. These are owned by operations, not IT.

> [!NOTE]
> Wave 4 is where IT-centric recovery programs most often cause secondary incidents. An IT team that restores an HMI without confirming the underlying SIS is clean can create a physical-safety hazard. If your organization includes OT, the Wave 4 runbook must be co-authored by OT engineering, not just IT security.

## Task 5.3 – Identity Recovery: The Unsung Problem

Most ransomware programs underweight identity recovery. The reason: it is procedurally complex, requires expertise that many organizations don't have in-house, and the consequences of getting it wrong are severe — a partially recovered AD with attacker persistence reinfects everything else that authenticates against it.

In a serious incident, **existing AD cannot be trusted**. The attacker likely has DCSync capability (dumps all password hashes), Golden Ticket (forges Kerberos tickets for any account), or persistent administrative footholds via scheduled tasks, service accounts, or GPO modifications. You cannot recover by resetting passwords alone.

**Three paths to recovery:**

**1 )** **Forest recovery from authoritative backup.**

Microsoft has published a detailed multi-day procedure for AD forest recovery. It involves seizing FSMO roles, restoring from backup in a specific order, and cleaning metadata. It works — but requires rehearsal. Almost no organization rehearses it before they need it.

- Recommended: download the Microsoft AD Forest Recovery Guide and read it before an incident. Identify who would execute it and how long it would take your team.

**2 )** **Stand up a fresh forest in the IRE and migrate.**

Increasingly common in modern recoveries. Build a clean greenfield AD or Entra ID tenant in the IRE, migrate users and groups, re-join systems, and retire the compromised forest. Modern tooling (Quest Migration Manager, ADMT, third-party AD recovery platforms) supports this.

- Trade-off: faster than forest recovery in many cases, but loses historical AD history and requires re-enrollment of managed devices.

**3 )** **Cloud-first reset (Entra ID / Azure AD as the recovery substrate).**

For organizations already heavy in cloud identity, lean on the cloud layer as the recovery substrate. Entra ID's conditional access, device compliance, and sign-in risk capabilities may provide a faster path to a trusted authentication layer than rebuilding on-prem AD.

- Trade-off: works well for cloud-native applications; on-prem legacy applications with Kerberos/NTLM dependencies may require a hybrid path anyway.

> [!IMPORTANT]
> Whichever path you choose: **practice it**. The hours of an active incident are not when you want to be reading the Microsoft forest-recovery whitepaper for the first time. A tabletop walkthrough of the identity recovery procedure — even without actually executing the steps — reveals dependencies and expertise gaps that cannot be discovered any other way.

## Discussion Questions — Sequencing & Identity Recovery

**Q1.** Map your three most critical business applications to the four recovery waves. For each application, identify: which Wave it belongs in, what identity dependencies it has (AD, Entra ID, SAML federation, service accounts), and what happens to your business if it is unavailable for the full duration of Waves 1 and 2.

**Q2.** Which of the three identity recovery paths is most appropriate for your organization — and have you practiced any version of it? If the answer is "no," identify the single biggest obstacle to rehearsing identity recovery in a test environment, and what it would take to remove that obstacle.

**Q3.** Wave 4 (OT/ICS recovery) requires co-authorship by OT engineering. If your organization includes OT environments, who owns the Wave 4 runbook today? If no one does, what is the risk if IT executes recovery steps on OT infrastructure without OT engineering involvement?

---

# Task 6: Restore Testing, RTO/RPO & Pre-Incident Readiness

In this module you will complete **Restore Testing, RTO/RPO & Pre-Incident Readiness** by reading and reflecting on **the testing discipline and planning math that determine whether your recovery architecture actually works** and engaging with **a gap assessment against the pre-incident readiness checklist**.

## Task 6.1 – Restore Testing: The Part Everyone Skips

> *A backup that has not been restored is a hypothesis, not a backup.*

**1 )** Industry surveys consistently find that significant proportions of organizations cannot fully restore from primary backups when actually required. The failure modes are predictable: expired encryption keys, catalog corruption, dependency on infrastructure that was encrypted alongside the backup target, or simply a backup process that had silently been failing for weeks before the incident.

**2 )** Restore testing is not a one-time event. It is a continuous program with four levels:

**Level 1 — Continuous (daily / weekly):** Sample restore of individual files or VMs from operational backups. Report restore success rate as a service-level metric visible to the CISO and reported to the board alongside availability metrics. A backup system with a 98% restore success rate has a 2% silent failure rate — which is the backup you will need during an incident.

**Level 2 — End-to-end (quarterly):** Restore a full critical application stack — database, application server, and dependencies — in a test environment. Validate application functionality, not just file presence. Measure actual restore time and compare against your stated RTO. Document discrepancies.

**Level 3 — Disaster-class (annually):** Exercise the full IRE. Declare a hypothetical compromise. Restore identity and one or two critical applications. Validate cut-over. Measure end-to-end time from declaration to validated service. Document the pain points — they are your remediation roadmap.

**Level 4 — Surprise (ad hoc):** The most valuable restore test is the one the team did not know was coming. Coordinate with leadership; test alerting paths and out-of-hours response without pre-notification to the recovery team. The gap between what the runbook says will happen and what actually happens is the risk.

> [!IMPORTANT]
> Level 2 and Level 3 testing are where most organizations find that their stated RTO is aspirational, not achievable. The first time this is discovered should be in a planned test, not during an active incident with stakeholders watching a recovery dashboard that is not moving.

## Task 6.2 – RTO/RPO: The Math Most Organizations Get Wrong

RTO (Recovery Time Objective) and RPO (Recovery Point Objective) numbers are easy to declare and hard to live up to. Two structural observations shape how to think about them.

**1 )** **Stated RTOs are almost always aspirational.**

Industry surveys and incident-response retrospectives consistently show actual restoration times exceeding stated RTOs by large multiples for significant ransomware events. The stated RTO is typically derived from a hardware-failure scenario — not from an adversarial scenario where:
- The attacker has been resident for weeks
- Identity must be rebuilt from scratch
- Every restored system requires forensic validation before reconnection
- The recovery team is operating under regulatory notification timelines simultaneously

Build recovery plans with margin. If your stated RTO is 48 hours, your architecture should be capable of achieving 24 hours — so that the overhead of adversarial recovery doesn't push you past the stated commitment.

**2 )** **Recovery is rarely uniform across systems.**

Some systems restore from immutable backups within hours. Others require painstaking reconstruction — identity, application dependencies, data validation. **The longest-pole system sets the practical RTO for any business process that touches it.** Identify your long-pole systems before an incident and design recovery specifically for them.

**3 )** **Separate "online RTO" from "business RTO."**

- **Online RTO:** Systems are technically restored and accessible.
- **Business RTO:** Business operations are actually restored — data is current, integrations are working, users can complete their workflows.

The gap between these two is where most stakeholder frustration lives during recovery. Communicate both explicitly. A system that is "online" but missing three weeks of transactional data is not operationally restored.

**4 )** **RPO math under adversarial conditions:**

RPO = time since last known-clean backup. In an adversarial scenario where the attacker has been resident for weeks, the last known-clean backup may be weeks old — not hours old. Your RPO under adversarial conditions is substantially worse than your RPO under routine conditions. Plan for this explicitly: which business processes have the lowest tolerance for data loss, and what is their adversarial RPO if the attacker dwelled for 21 days?

> [!TIP]
> The most useful framing for board communication: present three recovery scenarios — Best Case (ransomware detected early, clean backup available, no identity compromise), Likely Case (typical dwell time, partial backup compromise, identity rebuild required), and Worst Case (long dwell, backup tier 1 and 2 compromised, full IRE required). Each scenario has a different RTO and RPO. The board should approve the architecture against the Likely Case, not the Best Case.

## Task 6.3 – Pre-Incident Readiness Checklist

This checklist is the output of Module 4. Work through it as a gap assessment for your current recovery program. Each "no" or "unknown" is a remediation item.

**Backups**

**1 )** Tier 1 (operational), Tier 2 (immutable), and Tier 3 (vault) are all present and have been tested for restore within the last 90 days.

**2 )** Immutable retention period exceeds your sector's documented attacker dwell time (minimum 90 days recommended).

**3 )** Backup catalog is stored separately from the backup data and is independently accessible.

**4 )** Backup management credentials are separate from production credentials and require step-up MFA.

**5 )** Any attempt to modify immutability settings generates a high-priority alert.

**Identity Recovery**

**6 )** Forest recovery procedure is documented and has been rehearsed at least once in the last 12 months.

**7 )** Break-glass accounts exist, are isolated from production AD, have phishing-resistant MFA enrolled, and are audited.

**8 )** Clean AD or cloud-identity baselines are stored in the immutable backup tier and validated within the last 90 days.

**Isolated Recovery Environment**

**9 )** IRE architecture is documented — including clean identity source, one-way ingress path, and gold-image pipeline.

**10 )** The IRE has been exercised (even in tabletop form) within the last 12 months.

**11 )** Out-of-band communication capability exists and has been tested (separate email, personal devices, or temporary tenant).

**Runbooks and Sequencing**

**12 )** Recovery runbooks exist by system class (identity, financial, customer-facing, OT) with named owners and decision points identified.

**13 )** Recovery wave sequencing is documented and has been reviewed by OT engineering (if applicable).

**14 )** Entry and exit criteria for systems leaving the IRE and re-joining production are defined.

**Contracts and External Resources**

**15 )** IR retainer is in place with after-hours contact path tested.

**16 )** Recovery vendor professional services (Veeam, Rubrik, Cohesity, or equivalent) are on standby or on retainer.

**17 )** Cyber-insurance carrier and broker contacts are in the runbook with coverage limits documented.

**18 )** Breach counsel is pre-engaged.

**Regulatory**

**19 )** Notification matrices by jurisdiction are documented — timelines and responsible owners for SEC, CIRCIA, NIS2, GDPR, HIPAA, and sector-specific obligations.

> [!NOTE]
> Items 1–5 (backups) and 12–14 (runbooks and sequencing) are the highest-leverage items on this checklist. Organizations that have immutable backups with tested restore procedures and documented sequencing runbooks consistently recover faster and with lower total cost than those that do not — regardless of the sophistication of the attacker.

## Discussion Questions — Testing, RTO/RPO & Readiness

**Q1.** Work through the 19-item readiness checklist for your organization. Mark each item as: **Yes (tested)**, **Yes (not tested)**, **Partial**, or **No**. Count the gaps. Which three gaps carry the highest recovery risk — and who owns closing each one?

**Q2.** The module distinguishes "online RTO" from "business RTO." For your most critical business process, what is the gap between these two in a Likely Case recovery scenario — and how would you communicate that gap to the board in a way that leads to a budget decision rather than a reassurance?

**Q3.** Level 4 (surprise) restore testing is described as the most valuable because it tests what actually happens, not what the runbook says will happen. What organizational or cultural obstacles prevent your organization from running a surprise restore test — and what would it take to overcome them?

# 🏁 End of Training Lab

**Module 4 — Ransomware Ecosystem — Recovery Architecture — Complete**

You have completed the Recovery Architecture module. The pre-incident readiness checklist in Task 03 is the tangible output — a gap list that drives your remediation roadmap.

The single highest-leverage action after completing this module: run a Level 2 end-to-end restore test for your most critical application. Measure the actual restore time, compare it against your stated RTO, and document the gap. That gap is the most honest assessment of your current recovery readiness.

The next module in this series is **Module 5: Legal & Regulatory Playbook** — the SEC, CIRCIA, NIS2, DORA, GDPR, and HIPAA matrices your organization actually faces.
