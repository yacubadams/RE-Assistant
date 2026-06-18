# P5 — CPRE Quality Review
### RE Assistant Tool — Requirements Quality Assessment

> **Method:** CPRE (Certified Professional for Requirements Engineering) Quality Criteria  
> **Input:** SESL v1.0 — FR-01 to FR-06 (output of P4)  
> **Output:** Quality findings that drove SESL v1.1 changes  
> **Reviewer:** Requirements Engineer  
> **Result:** 4 corrective actions issued → applied in v1.1 and v1.2

---

## Quality Criteria Assessment

---

### 1. COMPLETE

**Result: ✅ PASS**

Every leaf goal has exactly one FR assigned: L1a→FR-01, L1b→FR-02, L2a→FR-03, L2b→FR-04, L3a→FR-05, L3b→FR-06. No leaf goal is unaddressed.

---

### 2. TRACEABLE

**Result: ✅ PASS**

Every FR carries a full goal chain (G0→Gx→Lxa) and a named obstacle reference. The path from interview quote to obstacle to FR is unbroken across all six requirements.

---

### 3. CONSISTENT

**Result: ✅ PASS with note**

No two requirements directly contradict each other. FR-01 (when to deliver suggestions) and FR-02 (how many to deliver) operate at different levels of the same pipeline and compose correctly. FR-05 (pre-queue gate) and FR-06 (write gate) govern distinct pipeline stages without overlap.

**Note:** The interaction between FR-01 and FR-02 is not explicitly specified. FR-01 triggers on a pause; FR-02 governs what is surfaced at that pause. A downstream requirement should define what happens when FR-02's consolidation is not complete by the time FR-01's pause window closes — the exception clauses currently handle this implicitly but it is not stated as a shared contract between the two.

---

### 4. UNAMBIGUOUS

**Result: ❌ FAIL**

Four ambiguities identified:

| FR | Term | Ambiguity |
|----|------|-----------|
| FR-01 | "configured silence threshold" | Who configures it, at what granularity, and within what permitted range is undefined |
| FR-02 | "one to two suggestions" | Upper bound is imprecise — "one to two" permits one or two; "maximum of two" is the correct deterministic phrasing |
| FR-03 / FR-05 | "identifiable transcript segment" / "verifiable anchor" | No definition of segment granularity — a word, a sentence, a speaker turn, and a paragraph are all valid candidates and lead to different acceptance conditions |
| FR-06 | "authorised RE" | Authorisation is never defined — no role, permission model, or authentication mechanism is referenced anywhere in the specification |

---

### 5. TESTABLE

**Result: ⚠️ PARTIAL PASS**

FR-02 through FR-06 each have postconditions that are directly observable and falsifiable. FR-01 is the weak point: its postcondition states the suggestion must be delivered "without overlapping any active speech turn," which is measurable in principle, but the acceptance test cannot be written without a concrete threshold value. As long as the threshold is undefined, FR-01 cannot be formally tested to a pass/fail verdict.

---

## Hallucination Check

Three items were identified where the specification introduced content not traceable to a stakeholder-stated goal or interview quote.

---

### Flag 1 — FR-01: Implementation mechanism smuggled into a goal-level requirement

**Issue:** "configured silence threshold" and "silence" as the mechanism for detecting a turn boundary are not stated in the interview. The stakeholder said the tool should act at the *"right moment"* — but silence detection is one possible mechanism, not a stated requirement. Introducing it at the SESL layer conflates the goal (right-moment delivery) with a design decision (silence as proxy for turn boundary).

**Suggested correction:**

| | |
|---|---|
| **Replace** | *"The system detects a conversational pause that exceeds the configured silence threshold"* |
| **With** | *"The system detects that no participant is actively contributing to the conversation"* |

This keeps the requirement at goal level and leaves mechanism decisions to the design phase.

---

### Flag 2 — FR-02: Specific numeric limit not stated by stakeholder

**Issue:** "a maximum of two" is a number introduced by the specification author, not Prof. Femmer. The interview states *"might recommend so many suggestions that might be too much"* and calls for no unnecessary repetition — but no numeric ceiling was ever given. The number two is a design assumption, not a requirement.

**Suggested correction:**

Remove the numeric bound from the requirement body and replace with a testable but stakeholder-grounded statement:

> *"The system shall surface the minimum number of suggestions necessary to address the highest-priority coverage gaps identified at the current pause point, suppressing all redundant or lower-priority candidates."*

If a numeric ceiling is needed, it must be elicited from Prof. Femmer explicitly and added as a separate constraint with a source quote.

---

### Flag 3 — FR-02: "Coverage gap severity" is an assumed ranking concept

**Issue:** "ranked by coverage gap severity" introduces an ordering mechanism — severity — that the stakeholder never defined or requested. The interview mentions that the tool should *"connect the pattern of the issue"* and avoid repetition, but does not specify severity as a ranking dimension. This is a design concept presented as a requirement.

**Suggested correction:**

| | |
|---|---|
| **Replace** | *"ranked by coverage gap severity"* |
| **With** | *"ranked such that the most critical unaddressed topics in the current session are addressed first"* |

This preserves the intent of prioritisation without committing to an undefined scoring model.

---

## Summary Table

| Criterion | Result | Critical Issues |
|-----------|--------|-----------------|
| Complete | ✅ PASS | None |
| Traceable | ✅ PASS | None |
| Consistent | ✅ PASS with note | FR-01/FR-02 interaction contract missing |
| Unambiguous | ❌ FAIL | 4 undefined terms across FR-01, FR-02, FR-03/FR-05, FR-06 |
| Testable | ⚠️ PARTIAL PASS | FR-01 not fully testable without threshold definition |
| Hallucination check | ⚠️ 3 FLAGS | FR-01 mechanism assumption, FR-02 numeric assumption, FR-02 severity concept |

---

## Corrective Actions Issued

| # | Action | Target | Applied in |
|---|--------|--------|------------|
| Action 1 | Elicit numeric suggestion limit and threshold from Prof. Femmer — do not specify in requirements without source quote | FR-02 | v1.1 — ceiling removed; remains open pending stakeholder confirmation |
| Action 2 | Define "transcript segment" granularity as a shared Glossary term referenced by FR-03 and FR-05 | FR-03, FR-05, Glossary | v1.1 — Glossary created; terms referenced |
| Action 3 | Add role/permission definition that FR-06 can reference for "authorised RE" | FR-06, Glossary | v1.1 — Glossary entry added |
| Action 4 | Revise FR-01 trigger to remove the silence mechanism assumption | FR-01 | v1.1 — trigger rewritten |

---

## What Changed as a Result of This Review

See [SESL v1.2](../sesl/SESL_Requirements_RE_Assistant_v1.2.md) for the full corrected specification.  
See [P6 — Cascade Analysis](P6_Cascade_Analysis_RE_Assistant_Tool.md) for downstream effects of these corrections.

| FR | Change applied in v1.1 |
|----|------------------------|
| FR-01 | Trigger and Step 1 rewritten — silence mechanism assumption removed |
| FR-02 | Numeric ceiling removed; "coverage gap severity" replaced with minimum-necessary phrasing |
| FR-03 | "Identifiable transcript segment" → "Transcript Segment (see Glossary)" |
| FR-05 | "Verifiable anchor" → "Verifiable Anchor (see Glossary)" |
| FR-06 | "Authorised RE" → "Authorised RE (see Glossary)" |
| Glossary | New section created — Transcript Segment, Verifiable Anchor, Authorised RE |
