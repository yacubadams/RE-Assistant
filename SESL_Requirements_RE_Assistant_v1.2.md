# SESL — Formal Requirements Specification `v1.2`
### RE Assistant Tool — FR-01 to FR-06

> **Format:** SESL (Structured English Specification Language)  
> **Version:** `v1.2` — Cascade Corrections Applied  
> **Goal traceability:** G0 → G1 / G2 / G3 → L1a–L3b → FR-01–FR-06  
> **Stakeholder:** Prof. H. Femmer | **Users:** Requirements Engineering Team

---

## Version History

| Version | Changes |
|---------|---------|
| `v1.0` | Initial SESL authoring (P4) |
| `v1.1` | CPRE review (P5): mechanism assumptions removed, numeric ceiling removed, Glossary created |
| `v1.2` | Cascade corrections (P6): FR-02 terminology aligned, FR-04 Glossary reference added, 4th Glossary entry added |

---

## Glossary

> All capitalised terms below are defined here. Where a requirement references a term, it defers to this definition.

---

### Transcript Segment

| Field | Content |
|-------|---------|
| **Definition** | A contiguous utterance contributed by a single identified speaker within the session transcript, bounded by either a speaker change or a natural conversational pause. Each Transcript Segment is uniquely referenceable by speaker identity and its sequential position within the session. |
| **Source** | *"Suggestions always have references to which part of conversation/discussion being made."* (Q9) — operationalises *"which part"* at speaker-turn granularity. |

---

### Transcript Excerpt *(added v1.2 — Cascade 3)*

| Field | Content |
|-------|---------|
| **Definition** | The displayable portion of one or more Transcript Segments presented inline to the RE during review. A Transcript Excerpt must correspond to at least one complete Transcript Segment and must contain sufficient content to allow the RE to verify whether it supports the associated summary item or requirement candidate. |
| **Source** | *"Suggestions always have references to which part of conversation/discussion being made."* (Q9) — FR-04 operationalises the inline display of this reference at the point of RE review. |

---

### Verifiable Anchor

| Field | Content |
|-------|---------|
| **Definition** | A Transcript Segment, or set of contiguous Transcript Segments, whose stated content directly supports a requirement candidate or summary item — without inference, extrapolation, or elaboration beyond what was explicitly expressed by a participant. |
| **Source** | *"People approve hallucination requirements; this has to be avoided, this is the worst case."* (Q9) — operationalises the boundary between stated content and inferred content. |

---

### Authorised RE

| Field | Content |
|-------|---------|
| **Definition** | A member of the Requirements Engineering team operating under Prof. Femmer's RE framework who has been explicitly granted write access to the documentation baseline by the designated project authority. Authorisation is managed outside the system; the system is responsible only for verifying that an authorised identity is associated with each approval action. |
| **Source** | *"He has a team that consists of a bunch of Requirement Engineers, they are the users who conduct elicitation during the RE process."* (Q7). *"Cannot alter automatically."* (Q6) |

---

### ⚠️ Open Elicitation Items

| # | Requirement | Item | Owner |
|---|-------------|------|-------|
| 1 | FR-02 | Numeric ceiling for suggestions per delivery opportunity — not yet confirmed | Prof. Femmer |
| 2 | FR-01 | Criteria for determining when no participant is actively contributing — not yet confirmed | Prof. Femmer |
| 3 | FR-06 / NFR-03 | Authorisation verification gap — may violate German legal compliance (SG2). Requires new NFR or FR-07. | Prof. Femmer + RE |

---

---

## G1 — Guided Elicitation During the Interview

---

### FR-01 — Turn-Boundary Gated Suggestion Delivery

> **Goal source:** G0 → G1 → L1a | **Obstacle:** OB-L1a — Wrong Timing | **Priority:** 🔴 High

> **v1.1 change:** Trigger and Step 1 revised to remove silence-detection mechanism assumption (P5 Action 4).

| Field | Content |
|-------|---------|
| **Description** | The system shall surface follow-up question suggestions only when no participant is actively contributing to the conversation, and shall withdraw any pending suggestion immediately if active contribution resumes. |
| **Precondition** | A live interview session is active and the live feedback feature is enabled. |
| **Trigger** | The system detects that no participant is actively contributing to the conversation. |
| **Postcondition** | The suggestion has been delivered to the RE without overlapping any active contribution by any participant. |
| **Exception** | If the system cannot reliably determine whether participants are actively contributing, it withholds all suggestions until active contribution status can be confirmed. |

**Main Behaviour:**

1. The system continuously monitors the live session stream for active contribution by any participant.
2. When the system determines that no participant is actively contributing, it evaluates whether an unaddressed coverage gap exists in the conversation so far.
3. If a gap is identified, the system surfaces the corresponding suggestion to the RE interface.
4. If no gap is identified, the system takes no action and resumes monitoring.
5. If any participant resumes active contribution before the suggestion is acknowledged, the system withdraws the suggestion immediately.

---

### FR-02 — Suggestion Consolidation and Rate Limiting

> **Goal source:** G0 → G1 → L1b | **Obstacle:** OB-L1b — Over-generation | **Priority:** 🔴 High

> **v1.1 change:** Numeric ceiling and 'coverage gap severity' removed; minimum-necessary phrasing applied (P5 Actions 1 & 4).  
> **v1.2 change:** 'Pause point' replaced throughout with 'point at which no participant is actively contributing' to align with FR-01 (Cascade 1). OB-L1b obstacle resolution updated in register (Cascade 2).

| Field | Content |
|-------|---------|
| **Description** | The system shall consolidate thematically related follow-up suggestions into grouped prompts and surface the minimum number of suggestions necessary to address the highest-priority unaddressed topics at the current point at which no participant is actively contributing, suppressing all redundant or lower-priority candidates. |
| **Precondition** | At least one follow-up suggestion candidate has been generated for the current point at which no participant is actively contributing to the conversation. |
| **Trigger** | The system has identified one or more suggestion candidates at a confirmed point at which no participant is actively contributing to the conversation, and is preparing to deliver them to the RE interface. |
| **Postcondition** | Only the minimum number of suggestions necessary to address the highest-priority gaps are simultaneously visible to the RE. All deferred suggestions are retained for subsequent delivery. |
| **Exception** | If clustering cannot be completed before active contribution resumes, the system surfaces only the single highest-priority candidate and defers all others to the next eligible delivery opportunity. |

**Main Behaviour:**

1. The system collects all suggestion candidates generated for the current delivery opportunity.
2. The system clusters candidates by thematic similarity and suppresses redundant variants within each cluster, retaining one representative per cluster.
3. The system ranks remaining clusters such that the most critical unaddressed topics in the current session are surfaced first.
4. The system selects the highest-priority cluster or clusters and surfaces a single consolidated suggestion per selected cluster to the RE interface.
5. All remaining lower-priority suggestions are deferred and reconsidered at the next point at which no participant is actively contributing.

---

---

## G2 — Complete, Traceable Interview Summary

---

### FR-03 — Transcript-Grounded Summary Generation

> **Goal source:** G0 → G2 → L2a | **Obstacle:** OB-L2a — Hallucination | **Priority:** 🔴 High

> **v1.1 change:** 'Identifiable transcript segment' now references Glossary definition (P5 Action 2).

| Field | Content |
|-------|---------|
| **Description** | The system shall generate interview summary items exclusively from content present in the verified session transcript, and shall exclude any summary clause that cannot be mapped to at least one Transcript Segment (see Glossary). |
| **Precondition** | The interview session has concluded and a complete, verified transcript of the session is available to the system. |
| **Trigger** | The RE initiates summary generation for a completed interview session. |
| **Postcondition** | Every item in the presented draft summary has a confirmed mapping to a Transcript Segment. No ungrounded item is present in the draft. |
| **Exception** | If the transcript is incomplete or unavailable, the system halts summary generation entirely and notifies the RE that a complete verified transcript is required before the process can proceed. |

**Main Behaviour:**

1. The system accepts the verified session transcript as the sole input source for summary generation.
2. For each candidate summary item produced, the system verifies that it maps to at least one Transcript Segment (see Glossary).
3. Candidates with a confirmed mapping are included in the draft summary.
4. Candidates without a confirmed mapping are excluded from the draft summary and logged as ungrounded items.
5. The system presents the draft summary to the RE, with the log of excluded items visible and clearly marked as withheld.

---

### FR-04 — Inline Traceability Link Verification

> **Goal source:** G0 → G2 → L2b | **Obstacle:** OB-L2b — False Citation | **Priority:** 🟠 High

> **v1.2 change:** 'Transcript excerpt' replaced with 'Transcript Excerpt (see Glossary)' throughout; fourth Glossary entry added to align granularity with FR-03 and FR-05 (Cascade 3).

| Field | Content |
|-------|---------|
| **Description** | The system shall display the cited Transcript Excerpt (see Glossary) inline alongside every summary item at the point of RE review, and shall block commitment of any summary item whose inline Transcript Excerpt is disputed or unresolvable. |
| **Precondition** | A draft summary with candidate traceability links has been generated and is presented to the RE for review. |
| **Trigger** | The RE opens a summary item for review or initiates a commitment action on a summary item. |
| **Postcondition** | Every committed summary item carries a verified, RE-confirmed traceability link to a supporting Transcript Excerpt (see Glossary). No item lacking a confirmed link is present in the committed output. |
| **Exception** | If a traceability link cannot be resolved to a displayable Transcript Excerpt (see Glossary), the system automatically treats the item as unverified and blocks its commitment without requiring RE intervention. |

**Main Behaviour:**

1. For each summary item, the system renders the linked Transcript Excerpt (see Glossary) inline and adjacent to the summary item text, without requiring the RE to navigate to the full transcript.
2. The RE reviews the inline Transcript Excerpt to confirm it supports the content of the summary item.
3. If the RE confirms the mapping, the item is marked as verified and becomes eligible for commitment.
4. If the RE identifies a mismatch, the RE marks the item as disputed.
5. The system blocks commitment of any disputed item until the traceability link has been corrected or the item has been removed from the draft.

---

---

## G3 — Trustworthy, Grounded Specification Baseline

---

### FR-05 — Pre-Queue Traceability Hard Gate

> **Goal source:** G0 → G3 → L3a | **Obstacle:** OB-L3a — Hallucination (requirement candidate) | **Priority:** 🔴 High

> **v1.1 change:** 'Verifiable anchor' now references Glossary definition (P5 Action 2).

| Field | Content |
|-------|---------|
| **Description** | The system shall verify that every requirement candidate has a Verifiable Anchor (see Glossary) in the session transcript before placing it in the RE approval queue, and shall permanently withhold any candidate that fails this verification. |
| **Precondition** | A verified session transcript is available and one or more requirement candidates have been generated from it. |
| **Trigger** | The system prepares to populate the RE approval queue following candidate generation. |
| **Postcondition** | The RE approval queue contains only candidates with confirmed Verifiable Anchors. No ungrounded candidate is reachable via the approval interface. |
| **Exception** | If Verifiable Anchor verification cannot be completed for any candidate, the entire approval queue is withheld and the RE is notified that verification is pending. No partial queue population is permitted. |

**Main Behaviour:**

1. For each generated requirement candidate, the system checks whether a Verifiable Anchor (see Glossary) exists within the session transcript.
2. Candidates with a confirmed Verifiable Anchor are placed in the RE approval queue with their anchor reference attached and visible.
3. Candidates without a confirmed Verifiable Anchor are permanently withheld from the queue and recorded in a separate ungrounded candidates log.
4. The RE approval queue is populated exclusively with Verifiable-Anchor-confirmed candidates.
5. The ungrounded candidates log is made available to the RE for transparency, clearly marked as ineligible for approval.

---

### FR-06 — Explicit Approval-Gated Baseline Write

> **Goal source:** G0 → G3 → L3b | **Obstacle:** OB-L3b — Artifact Violation | **Priority:** 🔴 High

> **v1.1 change:** 'Authorised RE' now references Glossary definition (P5 Action 3).

| Field | Content |
|-------|---------|
| **Description** | The system shall treat all existing specification artifacts as read-only at all times and shall only append a requirement candidate to the documentation baseline upon receipt of an explicit, fully logged approval action from an Authorised RE (see Glossary). |
| **Precondition** | One or more requirement candidates are present in the RE approval queue. All existing specification artifacts are in a read-only state. |
| **Trigger** | An Authorised RE (see Glossary) performs an explicit approval action on a specific requirement candidate. |
| **Postcondition** | The approved candidate is appended to the baseline. A permanent, auditable log entry exists for the approval action. No existing artifact has been altered in any way. |
| **Exception** | If the log entry cannot be created prior to the write operation — for any reason — the write is aborted in full and the Authorised RE is notified. No partial writes to the baseline are permitted under any condition. |

**Main Behaviour:**

1. The system receives an explicit approval action from an Authorised RE for an identified requirement candidate.
2. The system creates a log entry recording the Authorised RE identity, the candidate identifier, and the timestamp of the approval action, before any write operation begins.
3. The system appends the approved candidate to the documentation baseline as a new entry.
4. The system confirms to the RE that the candidate has been committed and that the log entry has been created.
5. All pre-existing baseline artifacts remain unmodified throughout; only new entries are appended.

---

## Requirements Traceability Matrix

| FR | Goal Chain | Obstacle | Failure Mode | Priority |
|----|------------|----------|--------------|----------|
| FR-01 | G0 → G1 → L1a | OB-L1a Wrong Timing | Wrong Timing | 🔴 High |
| FR-02 | G0 → G1 → L1b | OB-L1b Over-generation | Over-generation | 🔴 High |
| FR-03 | G0 → G2 → L2a | OB-L2a Hallucination | Hallucination | 🔴 High |
| FR-04 | G0 → G2 → L2b | OB-L2b False Citation | Hallucination (citation) | 🟠 High |
| FR-05 | G0 → G3 → L3a | OB-L3a Hallucination | Hallucination (candidate) | 🔴 High |
| FR-06 | G0 → G3 → L3b | OB-L3b Artifact Violation | Artifact Violation | 🔴 High |
