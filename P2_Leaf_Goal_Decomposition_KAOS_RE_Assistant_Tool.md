# P2 — Leaf Goal Decomposition
### RE Assistant Tool — KAOS AND-Refinement

> **Method:** KAOS AND-Refinement  
> **Input:** Functional sub-goals G1, G2, G3 from P1  
> **Output:** Six operational leaf goals L1a–L3b assigned to system or human actors

---

## KAOS Leaf Goal Criteria

Each leaf goal below satisfies all four KAOS criteria:

| Criterion | Definition |
|-----------|------------|
| **Assignable** | Assigned to exactly one agent: system or human actor |
| **Observable** | Can be verified — it either holds or it does not |
| **Specific** | Specific enough that a single system requirement addresses it |
| **Goal (not feature)** | States *what must hold* — not *how it is built* |

---

## G1 — RE teams are guided at the right moments to ask complete and relevant questions

---

### L1a — System

| Field | Content |
|-------|---------|
| **Leaf Goal ID** | L1a |
| **Statement** | The system detects gaps and unaddressed topics in the live conversation and surfaces contextually relevant follow-up question suggestions at the appropriate moment during the session. |
| **Assigned to** | **System** |
| **AND-Check** | L1a ensures the right suggestions exist at the right time. Together with L1b (RE acts on them), G1 is fully satisfied. |

---

### L1b — Human Actor

| Field | Content |
|-------|---------|
| **Leaf Goal ID** | L1b |
| **Statement** | The RE selects and poses the appropriate follow-up questions to the interviewee based on the suggestions presented during the session. |
| **Assigned to** | **Human Actor (RE)** |
| **AND-Check** | L1b ensures suggestions are acted upon. Together with L1a (system surfaces them), G1 is fully satisfied. |

> **AND-check G1:** L1a alone is insufficient — suggestions that are never acted on do not improve elicitation. L1b alone is insufficient — the RE cannot select questions the system never surfaced. Both must hold.

---

## G2 — Every interview produces a detailed, non-redundant, traceable summary

---

### L2a — System

| Field | Content |
|-------|---------|
| **Leaf Goal ID** | L2a |
| **Statement** | The system generates a consolidated interview summary that groups related topics and eliminates redundant items, covering all substantive discussion points raised during the session. |
| **Assigned to** | **System** |
| **AND-Check** | L2a ensures the summary is complete and non-redundant. Together with L2b (traceability), G2 is fully satisfied. |

---

### L2b — System

| Field | Content |
|-------|---------|
| **Leaf Goal ID** | L2b |
| **Statement** | The system links every item in the generated summary to the specific transcript segment from which it was derived, so that any team member can verify its origin. |
| **Assigned to** | **System** |
| **AND-Check** | L2b ensures every summary element is traceable. Together with L2a (completeness), G2 is fully satisfied. |

> **AND-check G2:** A complete summary with no traceability links cannot be verified. A fully linked but incomplete summary misses content. Both must hold.

---

## G3 — All accepted information is verifiably grounded in interview statements

---

### L3a — System

| Field | Content |
|-------|---------|
| **Leaf Goal ID** | L3a |
| **Statement** | The system withholds any requirement candidate from the documentation baseline if it cannot be directly traced to a statement made in the interview transcript. |
| **Assigned to** | **System** |
| **AND-Check** | L3a enforces a traceability gate at system level. Together with L3b (human approval), G3 is fully satisfied. |

---

### L3b — Human Actor

| Field | Content |
|-------|---------|
| **Leaf Goal ID** | L3b |
| **Statement** | The RE explicitly approves each traceable requirement candidate before it is committed to the specification baseline. |
| **Assigned to** | **Human Actor (RE)** |
| **AND-Check** | L3b enforces a human accountability gate. Together with L3a (system gate), G3 is fully satisfied. Neither alone is sufficient. |

> **AND-check G3:** A traceable but unapproved item must not enter the baseline (L3b gate needed). An approved but untraceable item must not enter the baseline (L3a gate needed). Both gates must hold simultaneously.

---

## Agent Assignment Summary

| Leaf Goal | Assigned To |
|-----------|-------------|
| L1a | System |
| L1b | Human Actor (RE) |
| L2a | System |
| L2b | System |
| L3a | System |
| L3b | Human Actor (RE) |
