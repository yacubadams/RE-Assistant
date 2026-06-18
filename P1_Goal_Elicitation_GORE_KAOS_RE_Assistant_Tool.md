# P1 — Goal Elicitation
### RE Assistant Tool — GORE / KAOS Notation

> **Method:** GORE / KAOS AND-Decomposition  
> **Input:** Stakeholder interview — Prof. H. Femmer  
> **Output:** Root goal G0 + three functional sub-goals G1, G2, G3

---

## Constraints Applied

- Goals derived **only** from what the stakeholder stated — nothing invented
- Each goal cites the exact interview quote that supports it
- Goals are **stakeholder-level outcomes**, not system features
- All three sub-goals must together **fully satisfy G0** (AND-logic)

---

## G0 — Root Goal

| Field | Content |
|-------|---------|
| **Goal ID** | G0 |
| **Type** | Root Goal |
| **Statement** | RE teams can conduct elicitation interviews that reliably capture all required information and produce complete, trustworthy specifications without unnecessary delay. |
| **Source** | *"To guide and support the stakeholder's RE in supporting asking right questions (interview) that will save time, from interview to specification takes forever and this tool should fix it."* (Q8) |
| **Source** | *"All the required elements to be ticked off and that the result is reliable… feels relaxed knowing that the interview went perfectly and the required informations are gathered and documented properly."* (Q1) |

---

## AND-Decomposition — G1, G2, G3

> G0 is satisfied **if and only if all three sub-goals below are simultaneously achieved**.  
> This is KAOS AND-logic: no single sub-goal is sufficient alone.

---

### G1 — Guided Elicitation During the Interview

| Field | Content |
|-------|---------|
| **Goal ID** | G1 |
| **Type** | Functional Goal |
| **Statement** | RE teams are guided, at the right moments during an interview, to ask complete and relevant questions so that no required information is missed during elicitation. |
| **Source** | *"The tool is like a mentor or consultant, who could at the right moment, give recommendations to ask follow up questions."* (Q9) |
| **Source** | *"Guide and support the stakeholder's RE in supporting asking right questions."* (Q8) |

---

### G2 — Complete, Traceable Interview Summary

| Field | Content |
|-------|---------|
| **Goal ID** | G2 |
| **Type** | Functional Goal |
| **Statement** | Every conducted interview produces a sufficiently detailed, non-redundant summary and a traceable record that is valuable to the entire RE team and traceable to the source conversation. |
| **Source** | *"The summary of the interview are sometimes insufficient, so the tool needs to create as much of a summary that are valuable to the whole team."* (Q9) |
| **Source** | *"Suggestions always have references to which part of conversation/discussion being made."* (Q9) |

---

### G3 — Trustworthy, Grounded Specification Baseline

| Field | Content |
|-------|---------|
| **Goal ID** | G3 |
| **Type** | Functional Goal |
| **Statement** | All information accepted into the specification is verifiably grounded in what was actually stated during the interview, so that no invented or unconfirmed requirements enter the documentation baseline. |
| **Source** | *"People approve hallucination requirements; this has to be avoided, this is the worst case. This will jeopardize the 'main goals'."* (Q9) |
| **Source** | *"The result is reliable."* (Q1) |

---

## AND-Completeness Check

| Sub-Goal | Contribution to G0 |
|----------|--------------------|
| **G1** | Ensures the **right input** is gathered during the interview |
| **G2** | Ensures that input is **fully and usefully documented** |
| **G3** | Ensures the resulting documentation is **trustworthy and grounded** |
| **Verdict** | G1 + G2 + G3 are necessary and jointly sufficient to satisfy G0. Removing any one leaves G0 unsatisfied. |
