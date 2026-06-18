# P6 — Cascade Analysis
### RE Assistant Tool — Post-Correction Consistency Check

> **Method:** Cascade effect analysis across FRs, obstacle resolutions, soft goals, and NFRs  
> **Input:** SESL v1.1 changes applied in P5  
> **Output:** 4 cascades identified → corrections applied → SESL v1.2 produced  
> **Status:** All internally resolvable cascades closed. 3 items remain open pending stakeholder input.

---

## What Was Checked

| Check | Description |
|-------|-------------|
| **FR → FR** | Does the change to FR-X affect expected behaviour of FR-Y? |
| **FR → Obstacle resolution** | Does the change contradict the resolution it was derived from? |
| **FR → Soft goals** | Does any change risk violating SG1 (no hallucination), SG2 (German legal compliance), SG3 (no redundancy), SG4 (pre-educated LLM behaviour)? |
| **FR → NFRs** | Does any change affect NFR-01 (hallucination gate) or NFR-03 (legal)? |

---

## Cascades Found

---

### Cascade 1

| Field | Content |
|-------|---------|
| **Affected** | FR-02 |
| **Effect** | FR-01's trigger was rewritten from "pause exceeding silence threshold" to "no participant is actively contributing." However, FR-02's Precondition and Trigger still used the term **"pause point"** — a concept no longer defined anywhere in the specification. The two requirements used different vocabulary for the same event boundary. If "pause point" and "no participant actively contributing" are read independently, they admit different interpretations and could be tested inconsistently. |
| **Action** | Replace "pause point" in FR-02's Precondition and Trigger with "the point at which no participant is actively contributing to the conversation," to align terminology with FR-01's updated language. |
| **Applied in** | v1.2 |

---

### Cascade 2

| Field | Content |
|-------|---------|
| **Affected** | OB-L1b obstacle resolution |
| **Effect** | The obstacle resolution for OB-L1b still read *"at most one to two suggestions should be surfaced per conversational turn."* FR-02 was updated specifically to remove this numeric ceiling because it was flagged as an unsupported assumption. The obstacle resolution layer now contradicted the requirement it produced: the resolution said ≤2; the requirement said minimum-necessary. Any tester or architect reading the obstacle document first would re-derive the numeric ceiling. |
| **Action** | Update the OB-L1b obstacle resolution text to match FR-02's current phrasing — replace the "at most one to two" clause with the minimum-necessary formulation. The resolution should be the parent, not a contradiction, of the requirement it generated. |
| **Applied in** | v1.2 — logged in FR-02 change note; obstacle register updated |

---

### Cascade 3

| Field | Content |
|-------|---------|
| **Affected** | FR-04 |
| **Effect** | FR-03 now referenced **"Transcript Segment (see Glossary)"** and FR-05 referenced **"Verifiable Anchor (see Glossary)"**, but FR-04 still used the raw term **"transcript excerpt"** without a Glossary reference. The Glossary defines a Transcript Segment at speaker-turn granularity. If "excerpt" in FR-04 was interpreted at a different granularity — a sentence, a phrase, a paragraph — the inline display it renders may not correspond to the Transcript Segment that FR-03 and FR-05 used to anchor the same item. This created a traceability gap at the point of human review. |
| **Action** | Align FR-04's use of "transcript excerpt" with the Glossary. Add "Transcript Excerpt" as a fourth Glossary entry defined as a displayable portion of a Transcript Segment. Replace "transcript excerpt" with "Transcript Excerpt (see Glossary)" throughout FR-04. |
| **Applied in** | v1.2 |

---

### Cascade 4

| Field | Content |
|-------|---------|
| **Affected** | SG2 (German legal compliance) and NFR-03 (legal) |
| **Effect** | The Glossary definition of Authorised RE stated: *"Authorisation is managed outside the system; the system is responsible only for verifying that an authorised identity is associated with each approval action."* This means the system trusts the identity presented at write time without independently confirming that the identity holds current authorisation. Under German documentation integrity standards (grounded in Q2 and Q6 of the interview), the audit log alone may be insufficient — if an unauthorised person presents a valid identity token and commits to the baseline, the log records it as a legitimate act. The legal exposure is not from the log being absent; it is from the system having no independent authorisation check. |
| **Action** | Add either: (a) a new NFR requiring that the system verify the RE's authorisation status against a managed access control record at the point of each write operation, or (b) a new FR (FR-07) covering this as a functional gate. This is an open design decision but must be resolved before the specification can be considered legally complete under German standards. |
| **Applied in** | ⚠️ NOT YET RESOLVED — requires Prof. Femmer decision. Carried forward as Open Item 3. |

---

## Soft Goal and NFR Cross-Check

| | FR-01 change | FR-02 change | FR-03 change | FR-05 change | FR-06 / Glossary change |
|---|---|---|---|---|---|
| SG1 — no hallucination | No effect | No effect | ✅ Strengthened | ✅ Strengthened | No effect |
| SG2 — German legal | No effect | No effect | No effect | No effect | ⚠️ Risk — Cascade 4 |
| SG3 — no redundancy | No effect | Minor risk* | No effect | No effect | No effect |
| SG4 — pre-educated LLM | No effect | No effect | No effect | No effect | No effect |
| NFR-01 — hallucination gate | No effect | No effect | ✅ Strengthened | ✅ Strengthened | No effect |
| NFR-03 — legal | No effect | No effect | No effect | No effect | ⚠️ Risk — Cascade 4 |

*Minor risk on SG3: removing "ranked by coverage gap severity" without a replacement ranking criterion leaves prioritisation logic unspecified. Without a defined order, redundant suggestions from adjacent thematic clusters could be surfaced in adjacent turns. Acceptable for now given the open elicitation item, but should be resolved at the same time as the suggestion ceiling.

---

## Changes Applied to Produce SESL v1.2

| FR | Cascade | Fields Changed |
|----|---------|----------------|
| FR-02 | Cascade 1 | Precondition, Trigger, Step 5, Exception — "pause point" replaced throughout |
| FR-02 | Cascade 2 | Change note — OB-L1b resolution update logged |
| FR-04 | Cascade 3 | Description, Steps 1–2, Postcondition, Exception — "Transcript Excerpt (see Glossary)" |
| Glossary | Cascade 3 | Fourth entry added — Transcript Excerpt |

---

## Open Items Carried Forward

| # | Cascade | Item | Owner |
|---|---------|------|-------|
| 1 | — | FR-02: Numeric suggestion ceiling — value not confirmed by Prof. Femmer | Prof. Femmer |
| 2 | — | FR-01: Active-contribution detection criteria — not confirmed by Prof. Femmer | Prof. Femmer |
| 3 | Cascade 4 | FR-06 / NFR-03: Authorisation gap — requires new NFR or FR-07 to address German legal compliance (SG2) | Prof. Femmer + RE |

---

## Links

- Previous stage: [P5 — CPRE Quality Review](P5_CPRE_Quality_Review_RE_Assistant_Tool.md)
- Corrected specification: [SESL v1.2](../sesl/SESL_Requirements_RE_Assistant_v1.2.md)
- Next session handover: [P0 — Session Brief](P0_Session_Brief_RE_Assistant_Tool.md)
