# P3 — Obstacle Analysis
### RE Assistant Tool — KAOS Obstacle Analysis

> **Method:** KAOS Obstacle Analysis — LLM-specific failure modes  
> **Input:** Leaf goals L1a–L3b from P2  
> **Output:** Six obstacles with resolutions and priorities

---

## LLM Failure Modes Covered

| Failure Mode | Description |
|--------------|-------------|
| **Hallucination** | LLM generates output not grounded in the conversation |
| **Wrong Timing** | LLM interrupts the interview at an inappropriate moment |
| **Over-generation** | LLM produces too many suggestions causing cognitive overload |
| **Conflict** | LLM surfaces two contradicting recommendations *(addressed structurally by FR-02 clustering — not triggered as a standalone obstacle)* |
| **Artifact Violation** | LLM modifies data it should not touch — legal risk under German law |

---

## G1 Obstacles — Guided Elicitation During the Interview

---

### OB-L1a — Wrong Timing

| Field | Content |
|-------|---------|
| **Leaf Goal** | L1a — System detects gaps and surfaces suggestions at the appropriate moment |
| **Failure Mode** | Wrong Timing |
| **Priority** | 🔴 CRITICAL |
| **Obstacle** | The system interrupts the interview at an inopportune moment — e.g. while the interviewee is mid-sentence — breaking the natural conversational flow and causing the RE to lose the thread of a critical answer. |
| **Resolution** | The system must only surface suggestions during natural pauses in conversation, never during active speech. Live feedback is default-on but must be suppressible per session. |
| **Grounding** | *"The tool is like a mentor or consultant, who could at the right moment, give recommendations."* (Q9) — the qualifier *"right moment"* is load-bearing. |
| **Resolved by** | FR-01 — Turn-Boundary Gated Suggestion Delivery |

---

### OB-L1b — Over-generation

| Field | Content |
|-------|---------|
| **Leaf Goal** | L1b — RE selects and poses follow-up questions based on suggestions |
| **Failure Mode** | Over-generation |
| **Priority** | 🔴 CRITICAL |
| **Obstacle** | The system surfaces so many suggestions simultaneously that the RE cannot process them during a live session, causing cognitive overload and resulting in the suggestions being ignored entirely — making G1 unachievable even though L1a was satisfied. |
| **Resolution** | The system must cluster thematically related suggestions and surface the minimum necessary to address the highest-priority gaps, suppressing all redundant or lower-priority candidates. |
| **Grounding** | *"The tool which was empowered by the intelligence layer might recommend so many suggestions that might be too much, so the tool needs to be able to connect the pattern of the issue, so that there will be no unnecessary repetition."* (Q9) |
| **Resolved by** | FR-02 — Suggestion Consolidation and Rate Limiting |

---

## G2 Obstacles — Complete, Traceable Interview Summary

---

### OB-L2a — Hallucination (Summary)

| Field | Content |
|-------|---------|
| **Leaf Goal** | L2a — System generates a consolidated, non-redundant summary |
| **Failure Mode** | Hallucination |
| **Priority** | 🔴 CRITICAL |
| **Obstacle** | The LLM elaborates or infers beyond what was actually said, inserting plausible but unstated points — producing a document that reads as complete and detailed but contains fabricated content that no participant actually raised. |
| **Resolution** | Every summary item must map to at least one identifiable Transcript Segment. Items without a confirmed mapping must be flagged and excluded from the draft summary. |
| **Grounding** | *"People approve hallucination requirements; this has to be avoided, this is the worst case. This will jeopardize the 'main goals'."* (Q9) |
| **Resolved by** | FR-03 — Transcript-Grounded Summary Generation |

---

### OB-L2b — Hallucination (Citation-Level)

| Field | Content |
|-------|---------|
| **Leaf Goal** | L2b — System links every summary item to the specific transcript segment |
| **Failure Mode** | Hallucination (citation-level) |
| **Priority** | 🟠 HIGH |
| **Obstacle** | The system produces citation references that appear syntactically valid but point to the wrong transcript segment — creating false traceability that passes human review undetected. |
| **Resolution** | Traceability links must be verifiable by the RE at the point of review: each link must surface the cited Transcript Excerpt inline so the reviewer can confirm the mapping without navigating to the full transcript. |
| **Grounding** | *"Suggestions always have references to which part of conversation/discussion being made."* (Q9) — the reference itself must be correct, not merely present. |
| **Resolved by** | FR-04 — Inline Traceability Link Verification |

---

## G3 Obstacles — Trustworthy, Grounded Specification Baseline

---

### OB-L3a — Hallucination (Requirement Candidate)

| Field | Content |
|-------|---------|
| **Leaf Goal** | L3a — System withholds any requirement candidate that cannot be traced to an interview statement |
| **Failure Mode** | Hallucination (requirement candidate) |
| **Priority** | 🔴 CRITICAL |
| **Obstacle** | The LLM generates a requirement candidate that sounds contextually plausible but is an inference not grounded in any interview statement. Because it is domain-consistent, it passes the RE's attention and enters the baseline as an approved, ungrounded requirement. |
| **Resolution** | Traceability must be enforced as a hard gate, not a soft warning. A candidate with no Verifiable Anchor must be architecturally blocked from reaching the approval queue. Grounding verification occurs before presentation to the RE, not after. |
| **Grounding** | *"People approve hallucination requirements; this has to be avoided, this is the worst case. This will jeopardize the 'main goals'."* (Q9) |
| **Resolved by** | FR-05 — Pre-Queue Traceability Hard Gate |

---

### OB-L3b — Artifact Violation

| Field | Content |
|-------|---------|
| **Leaf Goal** | L3b — RE explicitly approves each candidate before it is committed to the baseline |
| **Failure Mode** | Artifact Violation |
| **Priority** | 🔴 CRITICAL |
| **Obstacle** | The system automatically writes approved or unapproved requirement candidates into existing specification artifacts — bypassing the human approval gate and altering legally protected documentation without authorisation. |
| **Resolution** | All existing specification artifacts must be read-only at all times. Write operations to the baseline must be triggered exclusively by an explicit, logged human approval action. No background process may modify committed artifacts. |
| **Grounding** | *"Nothing is automatically changed in the current or previous artifacts that were stored, there is a legal aspect to it. Cannot alter automatically."* (Q6) |
| **Resolved by** | FR-06 — Explicit Approval-Gated Baseline Write |

---

## Obstacle Summary Table

| Obstacle ID | Leaf Goal | Failure Mode | Priority | Resolved by |
|-------------|-----------|--------------|----------|-------------|
| OB-L1a | L1a | Wrong Timing | 🔴 CRITICAL | FR-01 |
| OB-L1b | L1b | Over-generation | 🔴 CRITICAL | FR-02 |
| OB-L2a | L2a | Hallucination | 🔴 CRITICAL | FR-03 |
| OB-L2b | L2b | Hallucination (citation-level) | 🟠 HIGH | FR-04 |
| OB-L3a | L3a | Hallucination (requirement candidate) | 🔴 CRITICAL | FR-05 |
| OB-L3b | L3b | Artifact Violation | 🔴 CRITICAL | FR-06 |
