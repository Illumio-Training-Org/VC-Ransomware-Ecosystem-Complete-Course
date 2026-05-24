---
slug: module-3-negotiation-playbook
id: zmicy9z5dloi
type: challenge
title: 'Module 3: Negotiation Playbook'
teaser: The pay/no-pay decision, negotiation anatomy, the operator playbook, sanctions
  screening, and the professional negotiator — everything that happens in the chat
  window.
notes:
- type: text
  contents: |-
    # 322 – Ransomware Ecosystem

    **Module 3 — Negotiation Playbook**

    Welcome to Module 3. Modules 1 and 2 mapped the ecosystem and the tradecraft. This module puts you in the most uncomfortable seat in any ransomware response: the negotiation room.

    The material is drawn from Coveware case data, the ContiLeaks chats, and OFAC/FinCEN advisories. Treat the scripts and frameworks as study material — in a real incident, engage external IR and breach counsel before opening any chat.

    **This track has no virtual machines or hands-on labs.**
    Your work in each challenge is to read the content, engage with the decision frameworks, and answer the discussion questions.

    **Estimated module time:** 3–4 hours
    **Challenges in this track:** 6
difficulty: ""
timelimit: 7200
lab_config:
  default_layout_sidebar_size: 0
enhanced_loading: null
---

# Task 1: Introduction & The Pay/No-Pay Decision

In this module you will complete **Module 3 of the Ransomware Ecosystem course** by reading and reflecting on **negotiation tactics, decision frames, and legal constraints** and engaging with **the decisions your organization must make before, during, and after a ransomware extortion event**.

## Task 1.1 – Ground Rules

Negotiation is the most uncomfortable part of any ransomware response. The technical work has decision branches. The legal work has decision branches. Negotiation has all of them — plus a counterparty who is actively manipulating the room.

Before continuing, three ground rules apply to everything in this module:

**1 )** **This is study material, not authority to negotiate.** In any real incident: engage external IR and breach counsel before opening the chat, screen for OFAC sanctions before discussing payment, and document every decision with timestamps.

**2 )** **Specific firms, fee structures, and outcomes change quarter to quarter.** Re-check Coveware's most recent quarterly before relying on any number in this module for a real decision.

**3 )** **The negotiation does not exist in isolation.** Legal, regulatory, insurance, and communications decisions run in parallel. The playbook in this module only makes sense when it is connected to those other tracks.

> [!IMPORTANT]
> The ContiLeaks chats revealed that ransomware operators run scripted playbooks of their own. Knowing the script makes the chat less unsettling — and knowing what operators expect from the victim's side is the difference between a professional negotiation and one that leaks information or concedes leverage unnecessarily.

## Task 1.2 – The Six Inputs to the Pay/No-Pay Decision

Most incident playbooks rush past the pay/no-pay decision. This module slows it down. The decision has six inputs — and the order in which you consider them reveals how mature the program is.

**1 )** **Recovery posture** — Do you have known-good backups? When did you last successfully restore the systems that matter? Backup confidence is the single biggest determinant of bargaining power. If you can rebuild without paying, the negotiation becomes purely about leak suppression — a very different and often weaker argument for payment.

**2 )** **Leak risk** — What was actually exfiltrated? Customer PII, employee data, source code, contracts, embarrassing internal communications? Some categories trigger mandatory regulatory notification regardless of payment, which collapses the leverage of payment to suppress the leak — you must disclose anyway.

**3 )** **Sanctions** — Is the operator on the OFAC SDN list, or so closely associated with a designated entity that any payment carries strict-liability sanctions risk? Evil Corp, certain LockBit-linked individuals, sanctioned mixers, and sanctioned exchanges are all live concerns. This is non-negotiable — it comes before any discussion of payment amounts.

**4 )** **Regulatory** — What disclosures are required, to whom, and on what timeline? SEC 4-day disclosure (public companies), CIRCIA 72-hour reporting, NIS2, GDPR, HIPAA, sector-specific obligations. These exist whether you pay or not.

**5 )** **Insurance** — Does your policy cover ransom payment? With what sub-limit, coinsurance, and prior-approval requirements? Are there panel-firm requirements for the IR firm and negotiator? Is there a misrepresentation risk that could void coverage entirely?

**6 )** **Reputation and trust** — Customers, employees, partners, regulators, the press. Even a paid ransom does not buy silence in 2026. Operators leak data anyway, exit-scam, or hold it for re-extortion. Plan for disclosure regardless of whether you pay.

> [!NOTE]
> Input 3 (sanctions) is the only one that can create criminal liability. Process it first, before any other input influences the decision. If the screen returns a positive result mid-negotiation, the entire decision tree changes.

## Task 1.3 – Decision Frames Worth Writing Down Before You Need Them

These four decision frames represent the range of documented organizational postures. The right one for your organization depends on backup maturity, regulatory exposure, and board risk appetite — but the key word is **before**: write it down before the incident, not during.

**1 )** **"We will not pay if we have known-good backups and no high-sensitivity exfil."**
The most defensible default for organizations with mature recovery programs. Preserves resources, avoids sanctions risk, and reduces moral-hazard concerns.

**2 )** **"We will not pay sanctioned actors under any circumstance."**
The legal default. No further discussion required once a sanctions match is confirmed.

**3 )** **"We will pay only with insurance approval, breach-counsel direction, and board sign-off."**
The process-bound default for organizations that won't categorically refuse but need governance guardrails to prevent panic payments.

**4 )** **"We will pay to suppress a data leak only when payment is materially likely to suppress it."**
The honest version. Market evidence is mixed — operators have re-extorted, exit-scammed, and leaked data after receiving payment. Treat suppression as low-probability and price the option accordingly.

> [!TIP]
> If your organization does not have a documented decision frame for ransomware payment, that is the highest-priority output of completing this module. The questions in Challenge 6 (Decision Aids) will help you build it.

## Discussion Questions — The Pay/No-Pay Decision

Write your answers in your team discussion document before moving to Challenge 2.

**Q1.** Walk through the six inputs for a hypothetical attack on your organization. Which input is hardest to evaluate quickly — and why? What would you need to know within the first four hours of detection to make a credible initial assessment of each one?

**Q2.** The module states that backup confidence is the single biggest determinant of bargaining power. If you genuinely cannot recover from backups within an acceptable time window, does that change your pre-incident security investment priorities? What does the negotiation playbook tell you about where to invest?

**Q3.** Which of the four decision frames most closely matches your organization's current undocumented default? What would it take to make that frame explicit, written, and pre-approved by the board?

---

# Task 2: Anatomy of a Ransomware Negotiation

In this module you will complete **Anatomy of a Ransomware Negotiation** by reading and reflecting on **the four phases of a live negotiation** and engaging with **the decisions, timelines, and human dynamics that determine the outcome**.

> [!NOTE]
> A standard negotiation runs in four phases. The boundaries blur in practice — phases overlap, operators accelerate or stall, and your internal decision chain rarely moves at the speed the operator demands. The structure here helps anchor decisions so they are made deliberately, not reactively.

## Task 2.1 – Phase 1: Discovery and Stabilization (T+0 to T+24h)

The first 24 hours set the conditions for everything that follows. Errors made here — scope overclaims accepted as fact, premature chat opens, uncontrolled executive contact — are hard to walk back.

**1 )** **Confirm scope of encryption and exfiltration.** The ransom note typically claims more than was actually taken. Verify against your own EDR telemetry, DLP logs, and outbound-proxy records before accepting the operator's claims.

**2 )** **Engage external IR firm and breach counsel before opening the chat.** Counsel directs the engagement under attorney-client privilege. This is not optional — it is the single most important structural decision in the first hour.

**3 )** **Identify the ransomware brand** from the ransom note: wallpaper, file extension, Tor URL pattern, and leak-site branding. The brand matters for two reasons: it determines sanctions-screening priorities, and it tells experienced IR firms what historical payment-vs-walkaway ratios and operator behaviour patterns to expect.

**4 )** **Open a controlled chat.** Most IR firms run the chat from sandboxed infrastructure under a generic identity ("company representative") to avoid:
- Identifying executives or providing leverage points
- Confirming insurance coverage (a major concession)
- Providing revenue or valuation data the operator didn't already have

**5 )** **Gate all further communication on legal review**, including OFAC screening of the operator and the destination wallet before any payment discussion.

> [!IMPORTANT]
> Do not confirm the existence of cyber insurance in the chat. Operators calibrate demands to ability-to-pay — and "we have a $5M policy" is an anchor that will cost you more than the information appears to be worth.

## Task 2.2 – Phase 2: Information Gathering and Proof of Life (T+24h to T+72h)

Phase 2 is intelligence collection under time pressure. The operator is also collecting intelligence on you.

**1 )** **Request a sample decryptor.** Most operators decrypt 2–3 small, non-sensitive files free as proof of life. Verify they decrypt cleanly without modification. This also confirms the operator actually holds the decryption key — not all claims are genuine.

**2 )** **Request a file tree of stolen data.** Operators usually share a directory listing or a small archive of "interesting" files to demonstrate exfil. Use this to:
- Corroborate the scope you estimated from internal telemetry
- Identify whether high-sensitivity categories (PII, financial, source code, regulated health data) were actually taken
- Assess the plausibility of the operator's claimed exfil volume

**3 )** **Probe operator identity.** Negotiators trained on actor profiles can often identify the specific affiliate by chat patterns, language quirks, phrasing, and tooling references. This feeds sanctions screening and helps anticipate pressure tactics.

**4 )** **Stall the operator's clock deliberately.** Requests for time to consult lawyers, accountants, and the board are routine and expected. Operators grant 24–48h extensions regularly — deadlines are anchoring tactics, not hard limits. Use the time to complete OFAC screening, validate backup options, and align your internal decision chain.

> [!TIP]
> Tempo in Phase 2 is a signal. Responding within minutes implies panic. Responding within 24 hours implies process. Operators read tempo the same way a poker player reads betting cadence — controlled pace signals strength.

## Task 2.3 – Phase 3: Bargaining (T+72h to T+10d)

Phase 3 is where the financial negotiation happens. Most of the concessions — on both sides — occur here.

**1 )** **Opening counter-offer.** Common pattern: open at 5–15% of the demand, with a documented justification. Revenue claims, ability-to-pay documentation, and business-interruption budget figures all support the counter. Avoid round numbers — "$847,000" reads as the result of analysis; "$1,000,000" reads as a starting bid.

**2 )** **Establish credibility with documentation.** Operators expect victims to claim poverty. Supporting a low counter with financial evidence (redacted financials, business-interruption cost analysis) moves the negotiation faster than bare assertions.

**3 )** **Use the operator's own incentives.** Affiliates split proceeds with operators and are penalized for low-yield deals — but they are also rewarded for fast payment. Speed has value to them. Offering faster payment in exchange for a lower amount is a legitimate lever.

**4 )** **Be willing to walk.** The clearest signal of bargaining power is genuine willingness to end the negotiation. Operators read this in chat tempo, in language register, and in the questions asked. A victim who asks "what is your lowest price?" is signalling they intend to pay. A victim who asks "what do you deliver post-payment?" is signalling they are evaluating the option.

**5 )** **Keep humans in the loop.** Most successful negotiations are not transcripts of rapid-fire haggling — they are measured professional exchanges where each side maps the other's red lines. Emotional escalation, panic language, or executive involvement in the chat itself are concessions.

> [!NOTE]
> Operators routinely accept **30–70% reductions** from the opening demand when victim posture is credible. The Coveware quarterlies consistently show median payments well below the average — the distribution is heavy-tailed, meaning a small number of large payments pull the average up significantly.

## Task 2.4 – Phase 4: Settlement, Payment, and Aftermath

Settlement is not the end — it is the beginning of the highest-risk phase for re-extortion.

**1 )** **Settlement terms.** Before payment, require a written commitment covering:
- Decryption tools (single archive with documented hashes)
- Deletion of all stolen data, with a certificate of deletion
- A list of file paths and hashes of the stolen data set
- A written promise not to re-extort, resell, or republish — including under any successor brand

None of these commitments are legally enforceable. Treat them as evidence of intent and a contractual baseline for any future dispute, not as guarantees.

**2 )** **Payment process.** Payment typically routes via crypto exchange through a designated escrow at the IR firm, with sanctions and AML screening of the destination wallet completed immediately before transfer. The wallet is screened again at payment time — sanctions lists are updated dynamically.

**3 )** **Post-payment decryptor validation.** Decryptors are reverse-engineered before being deployed on production systems. Operator-provided decryption tools have repeatedly contained bugs that destroy data. Validate on isolated copies of representative files first.

**4 )** **Publish-back risk.** Operators have re-extorted victims after payment (Change Healthcare 2024 is the canonical case), sold the data to other operators, or leaked it anyway. Your post-incident communications plan must assume this is possible — plan for disclosure regardless of payment.

> [!IMPORTANT]
> The ALPHV/BlackCat exit scam after Change Healthcare illustrates a specific risk: the **operator absconded with the ransom before providing the decryptor to the affiliate**, who then re-extorted the data under the RansomHub brand. The victim paid twice. This is not hypothetical — build it into your decision model.

## Discussion Questions — Negotiation Anatomy

**Q1.** Phase 1 says to engage breach counsel before opening the chat. In your organization, how long does it actually take to get breach counsel on a call at 02:00 on a Saturday? Is the retainer number in your documented runbook, and is the runbook accessible outside the corporate network?

**Q2.** Phase 3 recommends opening at 5–15% of the demand with financial documentation. What financial documentation could your organization produce within 48 hours that would credibly support a low counter-offer? Who owns those documents, and do they know they might be needed in an incident?

**Q3.** The publish-back risk in Phase 4 means paying the ransom does not guarantee data suppression. Given that disclosure may be required regardless of whether you pay, how does that change the financial calculus of the pay/no-pay decision for your organization?

---

# Task 3: The Operator Playbook

In this module you will complete **The Operator Playbook** by reading and reflecting on **how ransomware operators script and run their side of the negotiation** and engaging with **how to recognize and respond to each tactic**.

> [!NOTE]
> The ContiLeaks chats and other internal disclosures show operators run standardized, supervised playbooks. Knowing the script in advance is the most direct way to reduce the psychological pressure of the chat — and to avoid making concessions the operator didn't earn.

## Task 3.1 – Standard Operator Opening

The opening sequence is remarkably consistent across brands. Recognizing each element as a scripted move — rather than a novel threat — is the first de-escalation tool.

**1 )** **"You have been compromised."** Establishes the brand, posts a Tor link, demands login. Often accompanied by a wallpaper change and a file dropped in every encrypted directory.

**2 )** **Initial demand.** Anchored at 1–5% of estimated annual revenue. The operator has looked up your organization on Dun & Bradstreet, ZoomInfo, your own annual report, and — in many cases — internal financial documents they exfiltrated. The number is not random.

**3 )** **Deadline.** Typically 72 hours or 7 days to "avoid leak." Deadlines are routinely extended when the victim engages professionally. Treat them as anchoring tactics, not hard limits.

**4 )** **Proof-of-life offer.** Free decryption of 2–3 small files. Occasionally a sample of stolen files to demonstrate exfil scope.

> [!TIP]
> The opening demand is an anchor, not a floor. Operators research your revenue to set a number that feels achievable to you while maximizing their expected value. Your first job is to break the anchor — with documentation, not emotion.

## Task 3.2 – Operator Pressure Tactics

Pressure tactics are deployed when the victim is moving slowly, appears to be recovering without paying, or has pushed back harder than the operator expected. Each tactic is designed to produce urgency and emotional response.

**1 )** **Selective leaks during negotiation.** Posting screenshots of stolen documents in the chat or to the leak site to demonstrate that exfil is real and that the deadline is credible. The files chosen are usually the most embarrassing or legally sensitive ones the operator found.

**2 )** **Direct contact with executives.** Calls or emails to CEOs, board members, and — in some documented cases — family members. This is distressing by design. Prepare your principals in advance: they should not engage, they should route all contact to the IR firm, and they should document every attempt.

**3 )** **Customer, partner, or regulator notification threats.** "We will notify the SEC / your customers / your regulators." In many cases your mandatory disclosure obligations mean you must self-disclose anyway — which neutralizes this as leverage. Knowing your disclosure obligations before the incident removes the power of this tactic.

**4 )** **DDoS attack.** The third-extortion layer: targeting remaining accessible infrastructure to disrupt recovery efforts and increase business-interruption costs while negotiation is ongoing.

**5 )** **Media engagement.** Some operators (LockBit, ALPHV, and successors) have engaged journalists directly, publishing internal data or communicating with reporters to increase public pressure. Expect press calls. Have holding statements drafted and the communications lead in the incident bridge from hour one.

> [!IMPORTANT]
> The direct-contact tactic targeting executives and family members is documented in multiple incidents. Brief your C-suite before an incident — not during — on what to say (nothing) and where to route contact (the IR firm). An unprepared executive who engages the operator directly is one of the highest-risk events in a negotiation.

## Task 3.3 – Operator Concession Patterns

Operators are not trying to maximize a single negotiation — they are managing a portfolio of simultaneous chats and optimizing expected value across all of them. This creates predictable concession patterns you can use.

**1 )** **Routine reductions of 30–70%** when victim posture is credible. Financial documentation, recovery optionality, and a professional negotiation tone are the inputs. Emotional desperation, rapid responses, and premature disclosure of insurance coverage are the inputs that suppress reductions.

**2 )** **Speed discounts.** Operators consistently offer better terms for fast payment (24–72 hours). The affiliate needs cash; the operator needs throughput. A credible offer of same-day or next-day payment in exchange for a meaningful reduction is a legitimate lever.

**3 )** **Component splitting.** Some operators separate the demand into "decryption only" and "data deletion" components. If recovery from backups is feasible — so encryption is not the primary problem — but exfil scope is unacceptable, paying only the data-suppression component may be negotiable. Treat the deletion commitment as low-probability of being honored (see Challenge 2, Phase 4).

**4 )** **Operators rarely walk away from an active professional negotiation.** They monetize what they can, when they can. A victim who stays in the chat, engages professionally, and moves at a controlled pace almost always gets a better outcome than one who panics, goes silent, or makes rapid capitulations.

## Discussion Questions — The Operator Playbook

**Q1.** The opening demand is anchored to estimated revenue. If an operator has exfiltrated your internal financials, they may know your actual financial position better than a D&B estimate suggests. How does that change the information asymmetry in Phase 3 bargaining — and what does it imply for what documents you protect most carefully before an incident?

**Q2.** The direct-contact pressure tactic targets executives and family members. Does your organization have a documented protocol for what to do when a C-suite executive receives a direct call from a ransomware operator? If not, draft a one-paragraph brief that could be handed to any senior leader right now.

**Q3.** Operators offer speed discounts for rapid payment. In your organization, what is the actual fastest you could authorize and execute a cryptocurrency payment — accounting for board approval, insurance sign-off, legal review, and the mechanics of the transaction? Map that timeline against the "24–72 hour" speed-discount window.

---

# Task 4: Sanctions Screening

In this module you will complete **Sanctions Screening** by reading and reflecting on **the OFAC strict-liability framework for ransomware payments** and engaging with **the practical screening process your organization must run before authorizing any payment**.

> [!IMPORTANT]
> OFAC screening is non-optional and non-discretionary. The September 2021 OFAC advisory makes clear that paying a sanctioned actor is a strict-liability violation — willfulness and intent are not required for liability. Banks, insurers, IR firms, crypto exchanges, and the victim organization itself are all explicitly subject to this risk.

## Task 4.1 – The Legal Framework

**1 )** **Strict liability** means the government does not need to prove you knew the actor was sanctioned. If a sanctioned actor received payment that you authorized, you are potentially liable — regardless of intent. This is why OFAC screening must occur before any payment discussion, not after settlement.

**2 )** **The October 2020 OFAC advisory** first put ransomware payment facilitators (negotiators, IR firms, insurers) explicitly on notice. The September 2021 update reinforced this and added detail on the licensing pathway available when payment is necessary despite a positive screen.

**3 )** **Who is subject to the risk:**
- The victim organization authorizing payment
- The IR firm facilitating the transaction
- The ransomware negotiator managing the chat
- The cyber insurer reimbursing the ransom payment
- The crypto exchange executing the transfer
- Legal counsel who approves the transaction

**4 )** **The licensing pathway.** If OFAC screening returns a positive match and payment is nonetheless necessary (e.g., no backup recovery possible, imminent risk to life-safety systems), the OFAC advisory describes a specific licensing process. This requires legal counsel experienced with OFAC, takes time, and is not guaranteed to result in authorization. Engage counsel immediately if this situation arises.

> [!NOTE]
> The existence of the licensing pathway does not mean payment to a sanctioned actor is always prohibited. It means it requires explicit OFAC authorization first — not after the fact. Operating without authorization and then seeking retroactive approval is not a recognized defense.

## Task 4.2 – Practical Screening Steps

Run these steps before any payment discussion — not before payment execution, but before the conversation moves to amounts.

**1 )** **Identify the brand and any associated individuals** from open-source threat intelligence. The IR firm's threat-intel team does this as a standard intake step. Resources: CISA joint advisories, FBI FLASH alerts, OFAC's own press releases on designations.

**2 )** **Cross-check the brand and individuals against the OFAC SDN list and Cyber-related Designations.** The SDN list is searchable at ofac.treas.gov. Cyber-related Designations are a sub-list maintained specifically for cybercrime actors and infrastructure.

**3 )** **Cross-check the destination crypto wallet** against OFAC-published wallet addresses. OFAC publishes specific wallet addresses in many designation notices — these are directly searchable.

**4 )** **Run a blockchain analytics screen** (Chainalysis, TRM Labs, Elliptic) for clustering with sanctioned addresses. A wallet that has transacted with a sanctioned wallet — even indirectly — may carry exposure. Blockchain analytics tools map these relationships automatically.

**5 )** **Document the screening findings** before authorizing payment. The documentation is your primary defense if the screen later proves to have been incomplete. Record: who ran the screen, what tools were used, what was checked, what was found, and who reviewed the results.

**6 )** **If the screen is positive or ambiguous** — escalate to OFAC via the licensing process before payment. Do not proceed on the assumption that ambiguity resolves in your favor.

**7 )** **Screen again at payment time.** Sanctions lists are updated dynamically. A clean screen on day 1 of the negotiation does not guarantee a clean screen on day 10 when payment is ready to execute. Run the blockchain analytics check again immediately before transfer.

> [!TIP]
> Your IR firm and ransomware negotiator should have OFAC screening built into their standard workflow — ask for this explicitly when engaging them. If they cannot describe their screening process, that is a qualification question worth asking before the incident, not during.

## Task 4.3 – Key Designations to Know (Mid-2026)

The OFAC sanctions list relevant to ransomware payments grows regularly. The following are the most operationally significant designations as of mid-2026. Always re-screen against the current SDN list at payment time — this list changes.

| Entity / Individual | Designation Date | Why It Matters |
|---|---|---|
| Evil Corp / Maksim Yakubets | Dec 2019 | Most significant criminal cyber designation; multiple affiliated brands |
| Evgeniy Bogachev (GameOver Zeus) | Dec 2016 | Long-standing designation; affiliated infrastructure remains active |
| Suex OTC | Sep 2021 | First crypto exchange designated; established the exchange-designation model |
| Garantex | Apr 2022; seized Mar 2025 | Major OTC desk used for ransomware laundering |
| Tornado Cash | Aug 2022 | Mixer designation; established legal basis for sanctioning smart-contract protocols |
| Sinbad mixer | Nov 2023 | Successor to Blender.io; used by North Korean actors |
| ChipMixer (infrastructure seized) | Mar 2023 | Mixer used across criminal ecosystem |
| Hydra Market | Apr 2022 | Russian dark-web market; ransomware laundering nexus |
| Certain LockBit-affiliated individuals | 2024 (Operation Cronos indictments) | Designated following infrastructure seizure |

> [!IMPORTANT]
> Evil Corp is the most complex designation for victims to navigate. Evil Corp brands have included Dridex, BitPaymer, WastedLocker, Hades, Phoenix CryptoLocker, PayloadBIN, and Macaw Locker. An organization that receives a ransom note and identifies the operator as Evil Corp-affiliated must treat payment as presumptively prohibited until OFAC counsel has reviewed the specific circumstances.

## Discussion Questions — Sanctions Screening

**Q1.** The strict-liability framework means intent is irrelevant — a good-faith but incomplete screening process that misses a sanctioned actor still creates liability. What does that mean for the quality threshold of your screening process? What would a "defensible" screening process look like documented as a checklist?

**Q2.** Your organization is mid-negotiation when the IR firm's blockchain analytics screen flags that the destination wallet has transacted with a Garantex-linked address. Garantex was sanctioned in 2022 and seized in 2025. Describe the next five steps your organization takes — and who owns each one.

**Q3.** Cyber insurers are themselves subject to OFAC sanctions risk when reimbursing ransom payments. How does that affect the prior-approval process your insurer requires before you can pay? Have you confirmed with your insurer what their OFAC screening process looks like and what happens if they identify a positive match after you've already paid?

---

# Task 5: Communication Scripts & The Professional Negotiator

In this module you will complete **Communication Scripts & The Professional Negotiator** by reading and reflecting on **the four key communication moments in a ransomware negotiation** and engaging with **when and why to use a professional negotiator rather than managing the chat internally**.

## Task 5.1 – Opening Message

The opening message sets the tone for the entire negotiation. Its purpose is to: establish that you are professional and process-bound, begin intelligence collection (proof of life, exfil scope, demand confirmation), and avoid making any concession — explicit or implicit.

```
Subject: <case ID from ransom note>

This is a representative authorized to communicate on behalf of the
affected organization. We are evaluating the situation and will need
some time to verify what has been accessed.

Before we discuss any next steps, please:

1. Demonstrate decryption capability by decrypting the three files
   referenced in the attached list (no sensitive content).
2. Provide a directory listing of any data you claim to have.
3. Confirm the demand amount and the deadline you require.

We will respond within 24 hours of receiving the above.
```

**What this message does:**

**1 )** Establishes a generic, professional identity — not an executive, not a named individual, not a panicked responder.

**2 )** Requests three things before engaging on any financial discussion: proof of decryption, exfil scope, and demand confirmation.

**3 )** Sets a response cadence (24 hours) that communicates process without appearing slow.

**4 )** Makes no concessions: no confirmation of insurance, no revenue information, no emotional language.

> [!NOTE]
> The "three files" proof-of-life request should be genuine — small, non-sensitive files that you actually want decrypted, whose successful decryption you can verify. Do not request files that contain data you don't want the operator to open again.

## Task 5.2 – Pushing Back on the Demand

This message is sent after OFAC screening is complete, the exfil scope is partially validated, and internal alignment on the counter-offer amount is confirmed.

```
We have reviewed your demand. We are not in a position to consider
the amount as stated. The figure does not reflect the actual financial
condition of the organization, and any payment we make would be
subject to extensive internal review and external (legal, insurance)
authorization that we cannot accelerate beyond a defined process.

If you wish to continue the conversation, please respond with a figure
that reflects your assessment of what we can realistically authorize.
We are prepared to share documentation supporting our position if a
constructive figure is offered.
```

**What this message does:**

**1 )** Rejects the opening demand without offering a counter — forcing the operator to move first, which provides intelligence on their actual floor.

**2 )** References internal process constraints ("legal, insurance authorization") without confirming policy existence or limits.

**3 )** Offers documentation in exchange for a constructive counter — setting up the Phase 3 bargaining dynamic where your credibility comes from evidence, not assertions.

> [!TIP]
> Waiting for the operator to move first after this message is psychologically difficult. Most organizations want to fill the silence. Resist the impulse — silence after a professional pushback is a signal of strength, not weakness. The operator will respond.

## Task 5.3 – Walk-Away Message

The walk-away message is used when: backup recovery is viable, the operator has not moved to an acceptable figure, or a sanctions complication makes payment prohibited. It must be sent only after internal alignment — sending it and then re-engaging undermines all future credibility.

```
We have completed our internal evaluation. We are not in a position
to make a payment at this time. We understand the consequences you
have outlined. Should circumstances change in the future, we may
re-engage; otherwise this is our final response in this channel.
```

**What this message does:**

**1 )** Closes the negotiation without hostility or explanation — giving the operator no information to use as leverage and no emotional hook to pull.

**2 )** Leaves a narrow re-engagement door ("should circumstances change") without implying likelihood. Some IR teams omit this; include it only if there is genuine possibility of re-engagement.

**3 )** Does not explain the reason for walking away. Explanations are leverage.

> [!IMPORTANT]
> A walk-away message is only credible if you are actually prepared to accept the consequences — data published, decryptor not provided, re-extortion attempt. Sending this message and then re-engaging within 48 hours without a material change in circumstances collapses your bargaining position permanently.

## Task 5.4 – Post-Settlement Validation Request

This message is sent after agreeing on terms, before any payment is executed.

```
Before we treat this matter as concluded, we require:

1. The decryptor (single archive, with documented hashes).
2. Written confirmation that all stolen data has been deleted from
   your infrastructure and any third-party storage.
3. A list of file paths and hashes of the stolen data set.
4. Confirmation that you will not re-extort, resell, or republish
   the data, including under any successor brand.

We will treat the matter as closed only after the above is provided.
```

**What this message does:**

**1 )** Creates a written record of what the operator committed to deliver, which is the baseline for any future dispute or re-extortion defense.

**2 )** Requests the decryptor before payment where possible — though many operators insist on payment first. The hash documentation allows you to verify the decryptor has not been tampered with.

**3 )** Explicitly covers successor brands — directly addressing the re-extortion risk documented in the Change Healthcare incident.

> [!NOTE]
> None of these commitments are legally enforceable against a criminal organization. Their value is: (1) as a psychological anchor for the operator against re-extortion, (2) as evidence in any future regulatory defense that you took reasonable steps, and (3) as documentation for your insurer that the settlement was structured professionally.

## Task 5.5 – The Role of the Professional Negotiator

Most enterprises do not — and should not — run their own negotiations. A professional negotiator brings four things an internal team cannot replicate under pressure.

**1 )** **Threat intelligence.** The negotiator has chat history with the same brand — sometimes the same affiliate. They know which deadlines are real, which pressure tactics are scripted, and what historical settlements looked like for similar victims. This context changes every decision in Phases 2 and 3.

**2 )** **Calibrated tempo and language.** Tone, pacing, and language calibrated to the operator's expectations and the specific affiliate's style. Amateur negotiations either capitulate too fast (panic payments, accepting the first counter) or escalate emotionally (which operators read as instability and exploit).

**3 )** **Compliance built into the workflow.** OFAC screening, AML review, and insurer prior-approval are standard steps in a professional firm's process — not afterthoughts. The compliance infrastructure exists before the incident.

**4 )** **Insulation from internal pressure.** The negotiator does not work for the victim's executive team. CEO panic, board impatience, CFO anxiety about business interruption — none of these pressures reach the chat. An internal negotiator absorbs all of them simultaneously.

**Leading firms:** Coveware, GroupSense, Kivu Consulting, Arete, Mandiant (Google), Unit 42 (Palo Alto Networks).

**Insurance panel requirements:** Most cyber-insurance carriers maintain approved panels of negotiators. Using an off-panel firm may result in the negotiation fees — and sometimes the ransom payment itself — being excluded from coverage. Confirm panel requirements with your insurer before an incident, and ensure your retainer firm is on the panel.

## Discussion Questions — Scripts and Negotiators

**Q1.** The opening message makes no concessions and requests three things before engaging on financial terms. In your organization, who would draft and send this message — and do they have the authority to do so without executive approval at 03:00 on a Sunday morning? What is the governance gap if that person is unavailable?

**Q2.** The walk-away message is only credible if you are prepared to accept the consequences. Before you can send it with credibility, you need to know: Can you actually recover without paying? What is the realistic recovery timeline? Who has verified this? Answer these questions for your organization right now — not during an incident.

**Q3.** Professional negotiators cost money before and during an incident (retainer fees plus per-incident fees). Estimate the cost of a professional negotiation retainer versus the cost of an unguided internal negotiation that results in paying 70% of the opening demand rather than 25%. At what ransom demand size does the professional negotiator clearly pay for itself?

---

# Task 6: Decision Aids, Reading the Chat & After-Action

In this module you will complete **Decision Aids, Reading the Chat & After-Action** by reading and reflecting on **the governance tools, chat-reading skills, and after-action discipline that complete the negotiation playbook** and engaging with **the pre-incident questions your organization must answer now**.

## Task 6.1 – RACI for the Payment Decision

The payment decision is one of the most consequential decisions an organization makes during a ransomware event — and one of the most commonly made without pre-established authority. The RACI below is a starting point. Tune it to your governance structure, but the critical word is **before**: establish this before the incident, documented and board-acknowledged.

| Decision | CEO | CFO | CISO | Board |
|---|---|---|---|---|
| Engage IR firm + breach counsel | A | C | R | I |
| Open negotiation channel | A | C | R | I |
| Authorize payment up to insurance sub-limit | A | R | C | I |
| Authorize payment above insurance sub-limit | R | R | C | A |
| Authorize public disclosure | A | C | C | I |
| Walk away from negotiation | A | R | R | I |

**R = Responsible, A = Accountable, C = Consulted, I = Informed**

**1 )** The most common governance gap is the "above sub-limit" row — where board accountability is required but board availability at 02:00 on a holiday weekend has not been tested. Who has board authority in an emergency? Is that documented?

**2 )** The walk-away decision is frequently the hardest. Under pressure from business interruption costs and board impatience, organizations that intended to walk often don't. Pre-committing the walk-away criteria in writing — and naming the people who hold that authority — is the only reliable mitigation.

> [!IMPORTANT]
> If your RACI does not exist before the incident, it will be improvised during the incident — under time pressure, with incomplete information, and with a counterparty actively trying to shorten your decision window. An improvised RACI almost always produces worse outcomes than a pre-established one.

## Task 6.2 – Pre-Incident Questions to Settle Now

These nine questions are the minimum set your organization must answer — in writing, with named owners — before a ransomware event. If you cannot answer all nine right now, each unanswered question is a gap worth closing before your next tabletop or insurance renewal.

**1 )** Who has ultimate authority to authorize payment? Is that documented and board-acknowledged?

**2 )** What is the maximum we will pay — in absolute dollars and as a percentage of the opening demand — before walking away regardless of other factors?

**3 )** Under what circumstances do we walk away regardless of demand size? (No backups? OFAC match? Public company disclosure already triggered?) Write the conditions explicitly.

**4 )** Which IR firm and breach counsel are on retainer? Have we tested the after-hours contact path — not the business-hours number, the 02:00 Saturday number?

**5 )** Which negotiation firm do we use? Are they on our insurance carrier's approved panel?

**6 )** What is the board notification path during an active incident? How long does it realistically take to reach a quorum for a payment decision above the insurance sub-limit?

**7 )** What is the regulatory disclosure timeline and who drafts the initial wording? (SEC 4-day clock starts at materiality determination — who makes that call, and when?)

**8 )** Who handles media contact? Are holding statements pre-approved for the most likely scenarios? Is the communications lead in the incident bridge from hour one?

**9 )** How do we handle direct contact attempts to executives or family members by the operator? Is there a documented protocol, and have executives been briefed on it?

> [!TIP]
> Answering these nine questions is the highest-value tabletop exercise a security program can run that doesn't require a vendor or a war-game scenario. It surfaces governance gaps that no technical control can compensate for. Run it with the CISO, GC, CFO, and one board member present.

## Task 6.3 – Reading the Chat

Negotiation chat reads like a bureaucratic transaction in many cases. The psychological pressure comes from the stakes, not from any sophistication in the operator's language. Several patterns are worth recognizing so they don't produce unintended responses.

**1 )** **Translation lag and grammar errors.** Many operations are run through translators. Awkward phrasing, unusual idioms, and grammatical inconsistencies are not signs of ignorance — they are artifacts of the translation layer. Don't read them as weakness or as an opportunity to take liberties with the professional tone of your responses.

**2 )** **Affiliate hand-off.** A mid-negotiation change in tone, language register, or responsiveness can indicate the affiliate has handed the conversation to a more senior negotiator — or to the operator directly. Don't read this as escalation by default; it often means the operator has decided the deal is worth closing and is taking personal interest.

**3 )** **Time-zone clues.** Response patterns often align with Moscow time (UTC+3) for Russian-operated brands. Off-pattern responses — late-night messages in Moscow time, weekend silence — can signal a different shift, a different team, or a deadline that is less real than it appears.

**4 )** **Cultural references and language register.** Scattered Spider's English-speaking crews are immediately distinguishable from translated Russian crews — the register is native, the idioms are contemporary, and the vishing-style social pressure is harder to deflect. The IR firm's threat-intel team reads this for you; it feeds actor identification and sanctions screening priority.

**5 )** **Anchoring.** Round numbers and aggressive deadlines are almost always opening anchors, not final positions. A $10M demand with a 72-hour deadline is not a reflection of your organization's value to the operator — it is a negotiating starting position. Counter with documentation, not emotion.

## Task 6.4 – After-Action: What to Capture

Whether you paid or not, the after-action capture is the highest-leverage investment you can make for the next incident. Institutional memory fades within weeks; the after-action discipline preserves it.

**1 )** **Full chat transcript.** The IR firm typically preserves this. Ensure it is transferred to your legal team under privilege and retained per your document-retention policy. It is evidence in any future regulatory inquiry.

**2 )** **Every decision point.** Who decided, when, what they decided, and what evidence they had at the time. This is your defense in a regulatory inquiry — and the basis for improving the playbook.

**3 )** **Time-to-decision measurements:**
- Detection to incident declaration
- Incident declaration to IR firm on the bridge
- Chat open to first counter-offer
- Settlement in principle to payment execution

These timelines tell you where the decision chain slowed down — and whether the slowdowns were governance gaps, technical gaps, or staffing gaps.

**4 )** **Operator behavior anomalies.** What the operator did that differed from the playbook — and what it correlated with. This feeds threat intelligence for the next engagement.

**5 )** **Post-payment delivery.** What the operator actually delivered, whether the decryptor worked, and whether the data deletion commitment was honored (or evidence emerged that it was not).

**6 )** **Playbook gaps identified.** Specific changes to the IR plan, the RACI, the communication templates, or the pre-incident questions that this event revealed were missing or wrong.

> [!NOTE]
> The after-action document is privileged work product when created under legal direction. Structure it that way from the start — not as a technical incident report, but as a legal memorandum describing the facts, decisions, and rationale. This protects it from discoverability in third-party litigation arising from the incident.

## Discussion Questions — Decision Aids, Chat & After-Action

**Q1.** Work through the nine pre-incident questions in Task 02 for your organization right now. Mark each one as: **Answered and documented**, **Answered but not documented**, or **Not answered**. For each "not answered" or "not documented" — who owns closing that gap, and by what date?

**Q2.** The RACI in Task 01 shows that authorizing payment above the insurance sub-limit requires board accountability. What is the realistic fastest you can reach a board decision in an emergency — accounting for time zones, personal schedules, and quorum requirements? If the answer is "more than 4 hours," what does that mean for your maximum ransom threshold at the CISO/CFO level?

**Q3.** The after-action in Task 04 requires capturing every decision point with timestamps, evidence, and decision-maker names. Who currently owns this documentation role in your IR plan? If no one is explicitly named, add this role to your IR plan as your first post-module action item.

**Q4.** Module 6 (Tabletop Scenario Library) includes Scenario 4 ("The Sanctioned Actor") and Scenario 6 ("The Public Records Crisis") which explicitly exercise this negotiation playbook. Before closing this module — identify one scenario from this module that represents your organization's highest-probability negotiation challenge, and commit to running a tabletop exercise against it within the next quarter.

# 🏁 End of Training Lab

**Module 3 — Ransomware Ecosystem — Negotiation Playbook — Complete**

You have completed the Negotiation Playbook module. The frameworks in this module are only valuable if they are tested before they are needed. The single highest-leverage action after completing this module is to answer the nine pre-incident questions in Task 02 — in writing, with named owners — and schedule a tabletop exercise that walks your incident team through at least one of the scenarios in Challenge 1's discussion questions.

The next module in this series is **Module 4: Recovery Architecture** — backup design, immutable storage, isolated recovery environments, and IT/OT recovery sequencing.
