---
slug: module-6-tabletop-scenarios
id: 6fl3ij8pkxha
type: challenge
title: 'Module 6: Tabletop Scenario Library'
teaser: Six decision-forcing scenarios set at Northwind Logistics — late-night detection,
  social engineering, insurance questions, OFAC exposure, supply-chain compromise,
  and data leak.
notes:
- type: text
  contents: |-
    # Module 6: Tabletop Scenario Library

    Six self-contained tabletop scenarios. One fictional organization. The practical deliverable of the Ransomware Ecosystem series.

    Each scenario runs 60–120 minutes, solo or facilitator-led. They are designed to probe process, governance, decision-making, and runbook completeness — the places where real incidents are won or lost.

    **The first time your team encounters a real ransomware incident should not be the first time they encounter a ransomware incident.**
tabs:
- id: yrayuulqoua8
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

# Task 1: Introduction & Anchor Organization

In this module you will complete the **Tabletop Scenario Library** by working through **six realistic ransomware scenarios** built on a single fictional anchor organization — Northwind Logistics. Each scenario is self-contained and designed to surface specific gaps in process, governance, and decision authority.

> [!NOTE]
> This module is the practical deliverable of the Ransomware Ecosystem series. The experience is significantly richer if participants have read Modules 1–5 before running the scenarios — but each scenario can be run cold. The scenarios deliberately probe different parts of the program: technical response, negotiation, legal and regulatory obligations, insurance, identity, supply chain, and communications.

## Task 1.1 – How to Use This Module

**1 )** **Solo (self-paced).** For each inject: read it, write down — in under three minutes — the action you would take, the decision you would make, and the next person you would call. Move to the next inject. At the end, compare your decision log to the discussion guide and facilitator notes. Note where you skipped a question because you didn't have the answer.

**2 )** **Group (facilitator-led).** Pre-assign roles before the session. Pre-read: Module 1, the relevant technical/process module, and the scenario itself. Strict timekeeping — each inject is announced; participants have 5–10 minutes to commit to a response. A note-taker captures the decision log. Hot-wash: 30-minute discussion using the discussion guide and facilitator notes. Written after-action review distributed within five business days.

**3 )** **Difficulty calibration.**
- **Easy mode:** Facilitator answers tactical questions when asked ("what does our backup architecture look like?"). For first-time participants or when the goal is to teach the runbook.
- **Standard mode:** Facilitator answers only what the scenario inject explicitly states. Participants work with the gaps.
- **Hard mode:** Facilitator introduces unannounced complications — a participant becomes unavailable mid-scenario, a partner statement contradicts internal telemetry, an inject is delivered with deliberate ambiguity.

**4 )** **Demonstrating learning.**
- **Per-scenario AAR (After-Action Review).** 1–2 pages covering what happened, decisions made, and what would change. The single highest-leverage artifact.
- **Updated runbook entries.** Specific changes to the IR runbook attributable to the scenario.
- **Detection content.** A Sigma rule, hunt query, or alert tuned in response to a scenario gap.
- **Cross-module synthesis paper.** Take three scenarios and identify the cross-cutting weakness across all three. ~5 pages.
- **Capstone.** Run all six scenarios over a quarter; produce an end-of-program improvement plan with cost and prioritization.

> [!IMPORTANT]
> These scenarios test process, governance, decision-making, and runbook completeness. They do not test hands-on EDR/SIEM operation, live forensics on a compromised image, negotiation against a real adversary, or real recovery time from real backups. Those are valuable next steps — this module is the bridge.

## Task 1.2 – Anchor Organization: Northwind Logistics

All six scenarios use the same fictional organization. Read this profile first — every inject schedule references it.

**1 )** **Company overview.** Northwind Logistics is a regional third-party logistics (3PL) provider headquartered in Cleveland, Ohio. Approximately 1,200 employees across 14 facilities in OH, PA, IN, MI, KY, and TN. Mix of warehousing, cross-dock, and e-commerce fulfillment for mid-market manufacturers and a handful of national retailers. Annual revenue approximately $240M.

**2 )** **IT and security profile.** 350 servers (250 Windows, 100 Linux), approximately 1,400 endpoints. Microsoft 365 / Azure AD / Entra ID. Veeam backup with on-premises repository — immutable retention not configured, despite the architecture supporting it. Fortinet edge firewalls and SD-WAN. CrowdStrike Falcon on Windows endpoints; Defender for Servers on most but not all Windows servers; no EDR on Linux. Single Active Directory forest, 2 domain controllers per region. Help-desk operates 24x5 with on-call. Cyber insurance with $5M ransomware sub-limit, $250k retention. **No IR retainer** — a deliberate gap in this profile.

**3 )** **Customer SLAs.** Strict same-day shipping for two retail customers; 45-minute cross-dock turnaround for one automotive customer. Downtime penalties run $80k–$250k per day per affected customer.

**4 )** **Governance.** CEO Maria Alvarez (formerly COO at a competitor). CFO Doug Park. CIO Priya Shah (recently hired, six months tenure). CISO position is open — security reports through a Director of Security to the CIO. General Counsel Erin Brooks. Board cyber-risk subcommittee meets quarterly.

> [!TIP]
> The deliberate gaps in Northwind's profile — no IR retainer, no EDR on Linux, immutable backup not configured, CISO position open, CIO only six months in role — are the pressure points the scenarios are designed to exploit. Notice when a scenario's difficulty is amplified by one of these gaps.

## Task 1.3 – Common Scenario Structure & Roles

Every scenario in this module follows the same structure:

**1 )** **Pre-brief.** 1–2 paragraphs of context describing the state of the world before the incident begins.

**2 )** **Inject schedule.** 6–10 timed events that drive the scenario forward, each with a stated decision focus.

**3 )** **Decision points.** Specific moments where the team must commit to a named action — not "it depends," not "we'd discuss it," but an actual decision and rationale.

**4 )** **Discussion guide.** Open-ended questions that probe gaps and assumptions. These are the questions you should still be arguing about after the hot-wash.

**5 )** **Facilitator notes.** What to listen for; common wrong turns; what "good" looks like.

**6 )** **Deliverables for participants.** The tangible outputs that demonstrate learning — runbook entries, protocols, templates, decision logs.

**Roles to assign at the table:**

- **Incident Commander** — typically the Director of Security or CIO
- **Communications lead** — Comms / IR / GC support
- **IT operations lead** — infrastructure, backup, restore decisions
- **Identity / endpoint lead** — EDR, Active Directory, SaaS
- **Legal lead** — General Counsel or breach counsel proxy
- **Executive sponsor** — CEO or CFO
- **Optional:** HR lead, customer-success lead, BCP lead, third-party partner liaison

---

# Task 2: Scenario 1 — The 03:00 Detonation

In this scenario you will work through **The 03:00 Detonation** — a mass-encryption event that begins at night with a degraded leadership chain, a compromised backup, and escalating customer SLA exposure.

> [!NOTE]
> **Scenario premise:** A LockBit-successor brand has been resident in your environment for nine days. Detonation begins on a Saturday morning while half the leadership team is at a wedding. The Director of Security is on-call. The CIO is in Vermont. The CEO is in airplane mode.

## Task 2.1 – Pre-Brief

It is 02:55 on Saturday. The Cleveland NOC sees CPU and disk-IO spikes on the file servers in the northeast region. By 03:08, ticket volume from "can't open file" errors is climbing in the global queue. The on-call infrastructure engineer (a contractor on the night shift) escalates to the Director of Security at 03:14.

By 03:25, ten of the fourteen facilities are reporting widespread file-share unavailability. EDR on Windows endpoints has begun firing high-severity "mass file rename" alerts. Linux file servers — which do not have EDR — are showing similar IO patterns from telemetry but no alerting.

The CIO is at her sister's wedding in Vermont. The CEO is on a personal phone in airplane mode in transit. The General Counsel is reachable but has just sat down to a four-hour brunch with her in-laws.

## Task 2.2 – Inject Schedule

| Time | Inject | Decision Focus |
|---|---|---|
| T+0 | EDR fires "mass file rename / high entropy I/O" across multiple Windows servers. Director of Security paged. | Initial assessment, escalation chain |
| T+10m | First ransom note appears on a workstation in the Pittsburgh facility. Note brands itself "Akira" and provides a Tor URL. | Confirm scope. Identify lead. Stand up bridge. |
| T+25m | Linux file server (no EDR) confirmed encrypting via SMB out-of-band telemetry. Pittsburgh and Detroit warehouses cannot ship. | Decide on isolation strategy. Authority to take prod offline. |
| T+40m | Veeam backup job alerts "target unreachable." Backup admin laptop also unresponsive on RDP. | Backup integrity? Catalog accessible? Tape vault? |
| T+1h 15m | First customer call. Automotive customer's 06:00 cross-dock requires 45-min turnaround, currently 0% functional. | Customer comms. SLA exposure. Who has authority to declare? |
| T+2h | EDR confirms ransom note files dropped to approximately 280 servers. Approximately 60% of Windows file shares appear encrypted. Linux server scope unclear. | Declared incident. Engage external IR? Insurance notification? |
| T+3h | CIO reachable from Vermont. CEO not reachable. CFO available. Deputy GC available. | Decision authority. Authority to authorize emergency spend. |
| T+4h 30m | Operator chat URL accessed. Initial demand: $4.8M. Threat to leak "15TB of customer data and contracts" within 96h. | Engage breach counsel. Open negotiation? Sanctions screen? |
| T+8h | Two retail customers escalate. National news has not picked it up yet. CEO reachable, asks: "do we pay or not?" | Materiality? Disclosure timing? Insurance carrier engagement? |
| T+24h | Backup vault confirmed corrupted in Tier 2 — immutable was not configured. Tier 1 backups encrypted. Tape last refreshed 6 weeks ago — restore validation pending. | Recovery plan with degraded backup. RTO conversation with CEO. |

## Task 2.3 – Decision Points

Work through each of these before moving to the discussion guide. Write a committed answer — not "it depends."

**1 )** Who is the Incident Commander? At what minute is that determined?

**2 )** When do you isolate parts of the network? What is the threshold to take down the SD-WAN entirely?

**3 )** When do you call the cyber-insurance carrier? Is the after-hours number known and accessible outside corporate email?

**4 )** Do you engage external IR? Off-panel or on-panel? At what hour does that conversation start?

**5 )** Do you open a negotiation channel? If so, who types in it, under what supervision?

**6 )** When do you tell customers? What do you tell them? Who is authorized to commit to a service-recovery timeline?

**7 )** If Tier 2 backups are gone, what is the recovery path — and how long?

**8 )** Is this material for SEC purposes? When do you make the materiality determination?

## Task 2.4 – Discussion Guide

Probe these areas as the scenario unfolds. Do not accept easy answers.

**Q1.** How long did it take to identify the Incident Commander? Was the substitution chain documented before this incident?

**Q2.** What part of the response broke because the CIO and GC were unavailable? Could the substitution chain have been better documented?

**Q3.** Who has authority to authorize external spend on a Saturday morning? Who has authority to take production offline during off-hours?

**Q4.** What customer comms script did you reach for? Who approved it? In what time?

**Q5.** When did the immutable-backup gap become visible to leadership? Could it have been surfaced before the incident — and what would that have taken?

## Task 2.5 – Facilitator Notes

**Common wrong turn 1:** The team conflates "we have backups" with "we have a recovery plan." Push them to articulate the IRE design, the catalog location, and the validation process.

**Common wrong turn 2:** Opening the negotiation channel before legal is engaged. Do not allow this. The OFAC and privilege exposures are real.

**Common wrong turn 3:** Assuming customer comms can wait. The two retail customers are reading the news; the automotive customer is on the phone in the first hour.

> [!TIP]
> Listen for: someone naming the materiality determination process for SEC purposes, the carrier-notification clause, and the immutable-backup retention question. **"Good" looks like:** clear IC by T+30m, isolation decision authority defined, IR retainer (or panel firm) called by T+1h, customer-facing holding statement by T+90m.

## Task 2.6 – Deliverables for Participants

**1 )** Updated runbook entry for after-hours escalation, with named substitutions for IC and General Counsel.

**2 )** Decision log of the scenario — minute-by-minute decisions with rationale documented.

**3 )** Backup architecture review note: identify what would have been different if Tier 2 immutability had been configured, and the cost and effort to do so.

**4 )** After-action review of customer-comms timing and content — what was said, when, by whom, and what should change.

---

# Task 3: Scenario 2 — The Help-Desk Call

In this scenario you will work through **The Help-Desk Call** — a social-engineering-driven identity compromise that escalates from a single MFA reset into cloud-identity privilege escalation and mailbox exfiltration. You have not been encrypted — yet.

> [!NOTE]
> **Scenario premise:** A Scattered Spider-style caller convinces a third-party-managed help desk to reset MFA for an employee. The attacker's next moves — inbox-rule creation, remote-access tool deployment, and cloud-admin role escalation — happen in real time while the actual employee is in a meeting.

## Task 3.1 – Pre-Brief

It is Wednesday at 14:50. A help-desk technician (employed via a third-party MSP) takes a call from a man identifying himself as Greg Mancini, regional manager of the Pittsburgh facility. He says his laptop was stolen from his car at lunch; he needs his MFA reset and to get back into Outlook before a 16:00 customer call. The caller is articulate, knows Greg's manager's name (which is on LinkedIn), gives the building address (also public), and seems genuinely stressed. The technician resets MFA after a knowledge-based question.

By 15:25, the SOC observes anomalous Outlook web sign-ins from a residential IP range in another time zone. By 15:40 the CrowdStrike sensor on Greg's actual — still-online — laptop has flagged unusual remote-access tool installation. Greg himself walks back from a meeting at 16:05 wondering why he has 47 push-MFA prompts.

## Task 3.2 – Inject Schedule

| Time | Inject | Decision Focus |
|---|---|---|
| T+0 | SOC sees suspicious sign-in to Greg's Microsoft 365 account from an unfamiliar IP. Conditional Access flagged "medium-risk" but did not block. | Investigate sign-in |
| T+15m | Inbox-rule creation event: forwarding rule on Greg's mailbox to an external address. | Determine compromise scope |
| T+30m | Greg returns to desk; reports MFA prompts and discovers MFA was reset. Help-desk ticket reviewed and reset call confirmed. | Identify identity-verification failure |
| T+45m | EDR on Greg's laptop detects ScreenConnect installation. The actual Greg did not install it. | Endpoint compromise — but laptop was not the access source |
| T+1h | Threat intel correlation: caller's voice and patter match a documented Scattered Spider-style social engineering pattern. | Brand identification; expected next moves |
| T+1h 30m | AzureAD audit log shows new global admin role assigned to a recently-created service principal. | Privilege escalation in cloud identity |
| T+2h | AWS console alert: new IAM access key created on a backup-storage account. Investigation suggests federated SSO from compromised identity. | Cloud lateral movement; potential backup compromise |
| T+3h | EDR isolation triggered. Conditional Access tightened. New service principal disabled. Hunt query identifies four other suspicious sign-ins in the past 24h. | Containment; broader scope check |
| T+6h | Forensic timeline complete. No file encryption observed yet. Exfiltration of approximately 30 mailboxes confirmed via OAuth-grant abuse. | Encryption-vs-pure-extortion decision |

## Task 3.3 – Decision Points

**1 )** When is the help-desk identity-verification process treated as the root cause? Who owns fixing it?

**2 )** Do you treat this as a ransomware incident, or as a data-extortion-only incident? What changes in your response posture?

**3 )** Do you suspend SSO from federated apps temporarily? What is the cost vs. containment tradeoff?

**4 )** Do you proactively notify the third-party MSP that runs the help desk — or wait for them to ask?

**5 )** Do you notify customers whose mailboxes are confirmed to have been exfiltrated?

## Task 3.4 – Discussion Guide

**Q1.** How does your help desk currently verify identity? What questions cannot be answered by someone with only public information about the employee?

**Q2.** What is the time between an MFA reset and a high-risk sign-in alert in your environment? Who sees the alert first?

**Q3.** Do you have a documented process for revoking all sessions for a single user? How long does it take to execute?

**Q4.** What is your protocol when a third-party-managed help desk performs a sensitive identity action — such as an MFA reset — without a supervisor review?

**Q5.** If exfiltration is confirmed but encryption has not fired, is that better, worse, or just different? How does it change your notification obligations?

## Task 3.5 – Facilitator Notes

**Key challenge:** This scenario is harder than it sounds. Many teams do not realize the help desk is a control surface — they treat it as a service desk. Push them to articulate the process change.

**The MSP question:** The organization outsources the help desk to manage cost; it cannot outsource the accountability for an identity-verification failure. The MSP's contract should specify who owns the process — and what happens when that process fails.

> [!TIP]
> **"Good" looks like:** Identity-verification protocol redesign with named callbacks, video verification, or ticket-driven workflow — not knowledge-based questions. MSP contract amendment. SOC detection content that alerts on MFA-reset to high-risk sign-in within 15 minutes. Documented session-revoke runbook that can be executed in under 10 minutes.

## Task 3.6 – Deliverables for Participants

**1 )** New help-desk identity-verification protocol: specific steps, specific verification methods, and named escalation path for high-sensitivity actions (MFA reset, password change, device unlock).

**2 )** Updated MSP contract addendum — or a list of clauses to negotiate — covering identity-verification standards, incident-notification SLA, and accountability for verification failures.

**3 )** SOC detection content: new alert on MFA reset followed by sign-in from an unfamiliar IP within one hour.

**4 )** After-action review of the data-exfiltration-only response posture — what differs from a full-encryption incident and what stays the same.

---

# Task 4: Scenario 3 — The Insurance Question

In this scenario you will work through **The Insurance Question** — a coverage challenge that surfaces mid-incident when the carrier identifies a potential misrepresentation on the renewal application. The negotiation is progressing; the payment is nearly approved; and now everything stops.

> [!NOTE]
> **Scenario premise:** Northwind has been encrypted for 36 hours. The IR firm is engaged (off-panel — the carrier has been informed). Negotiation has brought the demand from $4.8M to $1.6M. Payment is pending carrier approval. Then the broker calls with a question about the MFA attestation on last year's application.

## Task 4.1 – Pre-Brief

Northwind has been encrypted for 36 hours. Recovery is in progress. The IR firm has been engaged off-panel — the usual consultancy was unavailable; the carrier has been informed. The negotiation team has the operator at $1.6M, down from $4.8M. The CFO is preparing to authorize payment subject to carrier approval.

At 15:30 on day three, the cyber broker calls the General Counsel. "Quick question — the carrier is reviewing the application from last year's renewal. There's an attestation about MFA on remote-access services. The carrier's adjuster says CrowdStrike's logs show RDP from external IPs in the past six months without MFA challenges — three workstations, finance team, all managed by the third-party MSP. Was that disclosed at renewal?"

The CFO does not remember signing the application personally. The CIO is new — six months tenure — and was not in role at last renewal. The application was likely signed by the prior CIO, who has since left.

## Task 4.2 – Inject Schedule

| Time | Inject | Decision Focus |
|---|---|---|
| T+0 | Broker call: carrier raises application accuracy question regarding MFA on RDP. | Engage breach counsel; pause payment process |
| T+30m | Internal review: application from last renewal located. Question 4.7 states MFA is enforced on all remote access. | Determine if statement was true at the time of signing |
| T+1h | Tech review: CrowdStrike logs show three finance workstations with RDP from external IPs prior to a Conditional Access policy change six weeks ago. The change followed a routine internal audit that found the gap. | Was the audit reported? When? To whom? |
| T+2h | Insurance broker requests written response within 48h or carrier may issue a reservation-of-rights letter. | Deadline tightening; counsel-led drafting begins |
| T+3h | Operator escalates: deadline reduced from 96h to 48h with sample data leaked to the leak site as proof of seriousness. | Payment timeline pressure now compounded by coverage uncertainty |
| T+4h | Coverage review with breach counsel: $5M sub-limit; $250k retention; payment requires carrier pre-approval per policy. | Identify exposures if reservation is issued |
| T+6h | Internal accounting: cash on hand for $1.6M ransom plus IR plus recovery without insurance is approximately $3.2M; available without breaching debt covenants. | Self-fund vs. wait for insurance answer |
| T+12h | CFO and CEO discuss possibility of self-funding. CEO defers to GC and breach counsel. | Governance: who decides; rationale must be documented |
| T+24h | Breach counsel drafts a response to the carrier explaining the MFA gap, the audit finding, the corrective action, and arguing the misrepresentation was not material. | Carrier response strategy |

## Task 4.3 – Decision Points

**1 )** Do you pause the negotiation while the insurance question is resolved? What signal does that send to the operator — and what is the cost of the delay?

**2 )** If the carrier issues a reservation of rights, does Northwind self-fund the ransom? At what board level is that decision made?

**3 )** Who reviews and approves the response to the carrier? Counsel only? CFO co-sign? Both jointly?

**4 )** If application accuracy is challenged, what is the defense — was the statement "substantially true" at the time? Was it materially misrepresented?

**5 )** How do you change the renewal-application process going forward so this situation cannot recur?

## Task 4.4 – Discussion Guide

**Q1.** Who reviews cyber-insurance applications in your organization today? Is there a sign-off chain? Is technical truth verified against actual control state before signing?

**Q2.** What controls were stated as "in place" on your most recent application that you are not 100% confident about today?

**Q3.** If you self-fund this ransom, what does that do to next year's renewal underwriting and premium?

**Q4.** How would you have spotted the RDP gap proactively? What detection or hygiene control would have surfaced it before the incident and before the renewal?

**Q5.** What is the operational cost of pausing negotiation for 24–48 hours? How does the operator read a delay?

## Task 4.5 – Facilitator Notes

**This scenario is uncomfortable on purpose.** Many organizations have at least one item on their renewal application that is optimistic relative to reality. The lesson is the application-truth process, not rationalization of the specific gap.

**Common wrong turn:** Trying to litigate the question with the broker rather than designing a careful written response under counsel. The broker is not the decision-maker; the carrier's adjuster is. Counsel's response letter is the product.

**Common wrong turn:** Assuming the operator will hold position while internal politics play out. Operators read pace; this delay will cost bargaining leverage.

> [!TIP]
> **"Good" looks like:** Pause and engage counsel before answering the broker. Honest internal review of whether the statement was accurate at signing. Quantified self-fund vs. coverage decision with rationale. A concrete future-state remediation of the application review process — named owner, annual cadence, technical verification step.

## Task 4.6 – Deliverables for Participants

**1 )** New cyber-insurance application sign-off process: who validates each technical control statement before the application is signed, and what evidence they review.

**2 )** Re-attestation calendar: a schedule of controls that were attested to on the last application, reviewed on a defined cadence to confirm continued accuracy.

**3 )** Decision log for the self-fund-vs-wait analysis: amounts, governance chain, rationale, and outcome.

**4 )** Post-incident broker conversation framing: what to say at next renewal about the MFA gap, the corrective action, and the claim history.

---

# Task 5: Scenario 4 — The Sanctioned Actor

In this scenario you will work through **The Sanctioned Actor** — a payment halt triggered by a positive OFAC sanctions screen at the moment payment is queued and approved. The question is not whether to pay; it is what to do next.

> [!NOTE]
> **Scenario premise:** Northwind has been encrypted for four days. Recovery is partial (~40% restored). The negotiation has been productive — $900k from a $3.5M opening, carrier-approved, funds queued. At 11:15 on day five, the IR firm's blockchain analytics team flags the destination wallet as attribution-tied to Evil Corp affiliates via a sanctioned mixer. Seventeen hours remain on the operator's deadline.

## Task 5.1 – Pre-Brief

Northwind has been encrypted for four days. Recovery is partial — approximately 40% of file shares restored from backup; critical financial systems still offline. The negotiation has been productive: $900k from a $3.5M opening, with a verbal commitment from the operator to provide decryptors and "delete" stolen data on receipt of payment. The carrier has approved the payment in principle. The board is briefed. Funds are queued at the IR firm's escrow account.

At 11:15 on day five, the IR firm's blockchain analytics team flags the destination wallet. The wallet has received fragments of cryptocurrency from a mixer that was OFAC-sanctioned in November 2024, and the clustering analysis suggests the wallet itself is part of an attribution cluster previously linked to Evil Corp affiliates. The firm's compliance team will not authorize the transaction without a specific OFAC license.

The deadline the operator has communicated is 04:00 the next morning — approximately 17 hours from now.

## Task 5.2 – Inject Schedule

| Time | Inject | Decision Focus |
|---|---|---|
| T+0 | IR firm's compliance team flags wallet clustering; payment paused. | Sanctions screen result; halt payment |
| T+30m | Internal call: breach counsel, IR firm, CFO. Review of OFAC enforcement guidelines and penalty mitigation factors. | Disclosure and mitigation strategy |
| T+1h | Breach counsel: voluntary self-disclosure to OFAC and law enforcement is a mitigating factor. Filing path discussed. | Decide on self-disclosure |
| T+1h 30m | Operator messages: "24h left. Confirm wire received or we leak." | Timeline pressure; should we inform the operator? |
| T+2h | Decision: file voluntary self-disclosure with OFAC; do not pay. Engage FBI via IC3. | Walk-away posture |
| T+3h | Counsel drafts a message to the operator that does not commit to payment but does not close the channel. Goal: keep negotiation open while next steps are decided. | Operator chat language |
| T+4h | FBI engagement: provides IOCs and wallet info; offers to assist if operator brand has known weaknesses. | Federal cooperation |
| T+8h | Decision finalized: do not pay. Operator informed payment cannot be processed for legal reasons; offer to discuss alternative resolution not involving sanctioned channels. Operator unlikely to engage but counsel directs the message. | Alternative paths |
| T+12h | Operator does not respond. Internal preparation for leak assumed. | Comms posture for leak |
| T+17h | Deadline passes. Operator posts a sample on the leak site within 30 minutes; full data leak threatened in 7 days. | Leak response |

## Task 5.3 – Decision Points

**1 )** Who decides not to pay once the sanctions screen is positive? Is this CEO authority, board authority, or counsel-determined?

**2 )** Do you tell the operator the legal reason for non-payment, or stall with a different explanation?

**3 )** Do you self-disclose to OFAC voluntarily? What are the specific considerations, and who owns the filing?

**4 )** Do you engage the FBI? At what hour — and does engagement help or complicate the operational posture?

**5 )** How do you frame the eventual data leak to customers and the press — particularly the fact that no payment was made?

## Task 5.4 – Discussion Guide

**Q1.** How is sanctions screening currently structured in your incident response plan? Is wallet-level clustering analysis included — or only brand-level screening against the SDN list?

**Q2.** What is the decision authority chain when a payment cannot legally be made but the demand would otherwise be worth paying? Does your IR runbook address this?

**Q3.** What does "voluntary self-disclosure" to OFAC actually involve in practice? Has your breach counsel ever filed one? Have you discussed the mechanics before this scenario?

**Q4.** How does FBI engagement help or hurt the operational posture? What information do you share, and what do you hold back?

**Q5.** What is the customer-comms shape when data is leaked through no choice of yours — and how is that message different from the message when you chose not to pay for financial reasons?

## Task 5.5 – Facilitator Notes

**Stress that this is not hypothetical.** Wallet attribution to sanctioned entities has occurred in multiple recent matters. The sanctions exposure is a real operational risk that fires at the payment step — after negotiation is complete and approval is in hand.

**Common wrong turn:** Trying to find a workaround — a third-party intermediary, a different wallet, a cryptocurrency tumbler. All such structures are themselves sanctions risks. Counsel should redirect immediately.

**Common wrong turn:** Silence with the operator. Counsel-directed measured language is better — operator behavior on the leak is partially shaped by their perception of the target's professionalism and resolve.

> [!TIP]
> **"Good" looks like:** Clear walk-away decision made by named authority. Voluntary self-disclosure filed with OFAC. FBI engagement via IC3. Comms plan that frames non-payment as a legal posture — not a financial one — and does not create the impression of bad faith in the negotiation.

## Task 5.6 – Deliverables for Participants

**1 )** Sanctions-screening procedure document — specific tools, specific steps, wallet-level clustering analysis requirement, documentation template. Tested in advance, not improvised at payment time.

**2 )** OFAC voluntary self-disclosure template, drafted in coordination with breach counsel, with named owner and filing path.

**3 )** Customer comms language for a "data leaked, no payment made" outcome — differentiated from payment-made and payment-declined-for-financial-reasons scenarios.

**4 )** Decision tree for when payment is legally impossible: what alternatives exist, who decides, and what the documentation requirements are.

---

# Task 6: Scenario 5 — The Supply Chain Compromise

In this scenario you will work through **The Supply Chain Compromise** — a third-party-origin incident where your MSP's ransomware event has become your identity compromise. The challenge is acting on your own telemetry before your partner's public relations team controls the narrative.

> [!NOTE]
> **Scenario premise:** Your help-desk MSP has been encrypted. Their administrator account in your Microsoft 365 tenant is actively making changes — while their CEO's email says "suspended operations." The data exfiltration to an unfamiliar AWS bucket has been running for 41 minutes. The gap between the partner's public statement and what your telemetry shows is the core tension of this scenario.

## Task 6.1 – Pre-Brief

It is Tuesday at 09:25. The CIO receives an urgent email from the help-desk MSP's CEO: "We are experiencing a security incident. Out of an abundance of caution we have suspended all customer-facing operations and are investigating. We will provide further updates as soon as possible."

Within five minutes, the SOC notices unusual activity in the Microsoft 365 tenant: the MSP-owned global admin account is actively making changes — password resets, role assignments to new service principals, and outbound data transfer events to an unfamiliar AWS bucket. The MSP's CEO email says "suspended"; their privileged account is anything but.

## Task 6.2 – Inject Schedule

| Time | Inject | Decision Focus |
|---|---|---|
| T+0 | MSP CEO email arrives. SOC observes MSP global-admin activity in your tenant simultaneously. | Trust the email or the telemetry? |
| T+5m | Conditional Access: MSP global-admin account locked. New service principal disabled. Active sessions revoked. | Containment of partner identity |
| T+15m | Hunt: identify all MSP-managed identities in tenant. Six service accounts, two named admin accounts, one application registration with broad scopes. | Full scope of MSP access surface |
| T+30m | Forensic snapshot of recent MSP-attributed actions: 412 password resets executed in 19 minutes; approximately 40 mailboxes accessed; OAuth grant created for a third-party data-export app. | Damage assessment |
| T+1h | MSP CEO call back. They confirm ransomware on their internal infrastructure but "do not believe" customer environments are affected. Your evidence shows otherwise. | Information asymmetry with the partner |
| T+2h | Coverage call: cyber insurer confirms event qualifies; engages panel IR firm. Forensics pivot to MSP-side analysis. | External investigation scope |
| T+4h | Internal: 412 user accounts have new passwords; help desk is overwhelmed with self-service password-reset requests from locked-out users. | User-facing cost of containment |
| T+8h | Customer comms decision point: do you tell your customers that an MSP-side incident has touched your tenant? When? In what language? | Disclosure timing |
| T+24h | Public statement from MSP names you and three other clients as "affected." MSP CEO had not pre-notified you before issuing the statement. | Disclosure-coordination breakdown |
| T+48h | Forensic conclusion: approximately 70GB exfiltrated from your tenant — customer data, employee data, internal documents. | Notification obligations triggered |

## Task 6.3 – Decision Points

**1 )** How quickly do you act on telemetry that contradicts your partner's public statement? Who makes that call?

**2 )** How long does it take to disable all MSP-managed identities in your environment? Is a runbook for this practiced?

**3 )** Do you self-disclose to customers before or after the MSP names you publicly? What are the risks of each timing?

**4 )** How do you handle 412 simultaneous user password resets without grinding the help desk to a halt?

**5 )** What does your MSP contract say about coordination of public statements and incident notification to the customer?

## Task 6.4 – Discussion Guide

**Q1.** How many third-party identities have privileged access to your environment today? When were they last reviewed — and by whom?

**Q2.** What is your detection content for partner-identity anomalies? How is it different from internal-identity anomaly detection?

**Q3.** If a partner's public statement contradicts your telemetry, who decides what is true and how to act on it?

**Q4.** What would a "break-glass" partner-identity revocation look like if you had to disable all MSP access in five minutes? Is that possible in your current architecture?

**Q5.** How do you handle the customer disclosure when the data path went partner → you → customer? Who has the notification obligation, and in what order?

## Task 6.5 – Facilitator Notes

**Core challenge:** This scenario surfaces the third-party access surface, which is often inventoried but rarely exercised. The gap between what the MSP's CEO says and what the telemetry shows is intentional — it forces the team to decide whether to trust the partner or trust the data.

**Common wrong turn:** Deferring to the partner's public statement instead of acting on telemetry. The partner's lawyers and PR team are not optimizing for your timeline.

**Common wrong turn:** Underestimating the user-facing cost of mass password reset. The help desk becomes a bottleneck; the user-facing disruption from containment may exceed the disruption from the incident itself.

> [!TIP]
> **"Good" looks like:** Telemetry-led action by T+10m. Full partner-identity disable runbook executed by T+30m. Pre-emptive customer comms before the MSP's public statement names you. MSP contract review initiated post-incident to add disclosure-coordination language and incident-notification SLA.

## Task 6.6 – Deliverables for Participants

**1 )** Inventory of third-party privileged identities with named owners, access scopes, and last-review dates.

**2 )** Break-glass partner-identity revocation runbook: how to disable all identities associated with a specific partner in under 10 minutes, with named executor and approval authority.

**3 )** Detection content: alert on partner-identity behavior anomalies — mass actions, off-hours activity, new service-principal creation.

**4 )** MSP contract review checklist: incident-notification SLA, public-statement coordination requirement, indemnification language, and minimum security controls for MSP-managed accounts.

---

# Task 7: Scenario 6 — The Public Records Crisis

In this scenario you will work through **The Public Records Crisis** — the aftermath of a data leak where no ransom was paid, the data is public, and every decision from here is about communication, governance, and trust rather than technical containment.

> [!NOTE]
> **Scenario premise:** Northwind paid no ransom 14 days ago. The encryption was contained; recovery declared at 96% by day nine. The operator's wallet was tied to a sanctioned cluster (see Scenario 4). The team has been bracing for the leak. On Friday at 14:45 ET, it arrives — 4GB of employee SSNs, payroll records, customer ACH instructions, and executive emails. KrebsOnSecurity has it at 15:08.

## Task 7.1 – Pre-Brief

Northwind paid no ransom 14 days ago. The encryption was contained; backups restored cleanly; recovery declared at 96% by day nine. The negotiation went nowhere — the operator's wallet was attribution-tied to a sanctioned cluster. The team has been bracing for the leak.

On Friday at 14:45 ET, the operator's leak site posts the first batch — a 4GB archive labeled "Northwind_Initial." The archive contains approximately 11,000 employee social security numbers, payroll records, customer ACH and wire instructions, and sample executive emails. The KrebsOnSecurity reporter posts at 15:08; The Record's reporter at 15:14; a regional newspaper picks up at 15:30.

The CEO is mid-Atlantic on a flight to a family wedding in Italy. The CFO is offsite at a customer meeting. The General Counsel is one hour into a root canal. The CIO is available. The Director of Security is available. The PR firm on retainer has a junior associate on weekend duty.

## Task 7.2 – Inject Schedule

| Time | Inject | Decision Focus |
|---|---|---|
| T+0 | Leak site posts 4GB sample. Tor link circulating in security community. | Confirm contents; engage forensics |
| T+15m | First reporter inquiry. Holding statement requested by 17:00. | Authority to speak; statement language |
| T+30m | Internal: GC reachable in approximately 45 minutes. CIO in command. CEO not reachable for 4–6 hours. | Substitution chain validated |
| T+45m | Forensics confirms data is genuine; 11,000 employee records is approximately the entire current workforce. | Employee notification trigger |
| T+1h | Press release embargo offered to two major outlets in exchange for accuracy review. PR firm advises against. | Comms strategy |
| T+1h 30m | Three customer companies call in succession. They have customer-facing exposure of their own data via Northwind's records. | Customer comms; B2B trust impact |
| T+2h | GC available; reviews holding statement. State AGs in 12 states will require notification within 30 days. HR considers offering credit monitoring. | Regulatory matrix; notification scope |
| T+3h | Employee Slack lights up. Several employees have already been contacted by reporters at their homes. | Internal comms |
| T+4h | CEO reachable from layover. Asks: "do we want to do a video to employees or a written note?" | Tone setting |
| T+8h | First copycat scam: phishing emails impersonating Northwind HR are sent to employees offering "free credit monitoring." | Secondary exploitation |
| T+24h | Class action complaint filed. Two state AGs send formal inquiries. | Litigation posture |
| T+72h | Major customer requests in writing whether their data has been part of the leak. Northwind's contracts include a notification clause. | Contract obligations |

## Task 7.3 – Decision Points

**1 )** Who is authorized to speak to the press at T+15m? Is the holding statement pre-approved or written ad hoc?

**2 )** When and how do you notify 11,000 current employees? Email? Text? In-person at facilities?

**3 )** Do you offer credit monitoring? What duration, what provider, and at what cost? Who can authorize a $1M-class spend without the CEO?

**4 )** How do you handle the customer-data dimension — customer notifications, contract triggers, B2B trust?

**5 )** What is the SEC materiality posture if you have not already disclosed under the 8-K rule?

## Task 7.4 – Discussion Guide

**Q1.** What does your existing holding-statement library cover? Was it tested in the last 12 months?

**Q2.** Who can authorize a $1M-class spend on credit monitoring without CEO sign-off? Is that delegated authority documented?

**Q3.** How do you coordinate employee, customer, and press messaging on the same day — with the same factual content?

**Q4.** What is the protocol when reporters reach employees at their homes? Is there an internal communications channel employees can use when facing media calls?

**Q5.** How does a publicly documented data leak affect next year's cyber-insurance renewal — and what do you say to the underwriter?

## Task 7.5 – Facilitator Notes

**This is the longest scenario and the one most teams handle worst** — because it is not technical. The decisions are about communication, governance, and trust. Technical responders are often out of their depth here, and that is intentional.

**Common wrong turn:** Waiting for the CEO. CEOs often appreciate clear, decisive substitute leadership in their absence; it is the unanswered phone call that erodes trust — not the decision made without them.

**Common wrong turn:** Trying to keep the leak quiet. The data is published on a publicly accessible leak site. Controlling the narrative is the goal, not suppressing information that is already out.

> [!TIP]
> **"Good" looks like:** Holding statement out by T+90m. Employee notification by end of day. Customer outreach proactive within 48 hours. Credit-monitoring offer launched within a week. CEO video for employees on Saturday. Class-action response framework in place with counsel before the complaint is filed.

## Task 7.6 – Deliverables for Participants

**1 )** Updated holding-statement library with versions for ransomware-leak scenarios — encryption-only, exfiltration-only, and combined — with approval chain documented.

**2 )** Employee notification template plus protocol for who sends it, when, via what channel, and what escalation path exists for employees contacted directly by press.

**3 )** Customer notification template differentiated by contract tier — customers with contractual notification clauses versus customers without.

**4 )** Credit-monitoring decision matrix: when offered, what provider, what duration, cost model, and who has authority to approve at each spend threshold.

**5 )** Class-action and regulator-inquiry response framework with counsel — holding language, document-preservation protocol, and designated response lead.

---

## Task 7.7 – Facilitator Appendix: Running and Calibrating Scenarios

**Running a scenario solo (self-paced):**

If you are running these alone, the practice is quieter but still useful. For each inject: read it, write down in under three minutes the action you would take, the decision you would make, and the next person you would call. Move to the next inject. At the end, compare your decision log against the discussion guide and facilitator notes. Note where you skipped a question because you did not have the answer.

**Running a scenario as a group:**

- Pre-read: Module 1, the relevant technical/process module, and the scenario itself — for all participants
- Pre-assigned roles with printed role cards distributed at the start
- Strict timekeeping: each inject is announced; participants have 5–10 minutes to commit to a response
- A note-taker captures the decision log throughout
- Hot-wash: 30-minute discussion using the discussion guide and facilitator notes
- Written after-action review distributed within five business days

**Calibrating difficulty:**

- **Easy mode:** Facilitator answers tactical questions when asked ("what does our backup architecture look like?"). Used for first-time participants or when the goal is to teach the runbook.
- **Standard mode:** Facilitator answers only what the scenario inject explicitly states. Participants work with the gaps.
- **Hard mode:** Facilitator introduces unannounced complications — a participant becomes "unavailable" mid-scenario, a partner statement contradicts internal telemetry, an inject is delivered with deliberate ambiguity.

**What this module does not test:**

These scenarios are tabletops. They test process, governance, decision-making, and runbook completeness. They do not test hands-on EDR/SIEM operation under duress, live forensics on a compromised image, negotiation against a real adversary, or real recovery time from real backups. Those are valuable next steps — Module 6 is the bridge between this course's research and a fuller program of red-team exercises, purple-team detection engineering, and live recovery rehearsals.

> [!NOTE]
> The single most important output of completing all six scenarios is not any one deliverable — it is the identification of the gaps your team could not answer. Those unanswered questions are the highest-priority items for your IR program backlog.

# 🏁 End of Training Lab

**Module 6 — Ransomware Ecosystem — Tabletop Scenario Library — Complete**

You have completed the Ransomware Ecosystem series. All six modules are now done:

- **Module 1:** Ransomware Research & Foundations
- **Module 2:** Adversary Tradecraft
- **Module 3:** Negotiation Playbook
- **Module 4:** Recovery Architecture
- **Module 5:** Legal & Regulatory Playbook
- **Module 6:** Tabletop Scenario Library

The highest-leverage next step: schedule a tabletop using the scenario that most closely maps to your organization's highest-probability risk — with breach counsel, the CISO, and at least one executive in the room. The scenario you are least confident about running is the one you need most.
