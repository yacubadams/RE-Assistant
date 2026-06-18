# Prompt Chain — P1 to P6
### RE Assistant Tool — Full Methodology Prompt Reference

> **Purpose:** This file documents every prompt used to produce the requirements artifacts in this repository, in execution order. It exists so the pipeline can be reproduced, audited, or extended in future sessions.  
> **Note:** P0 is not a prompt — it is an output (the session handover brief). The prompts begin at P1.  
> **Input to all prompts:** [Henning Femmer Interview — 13 May 2026](../Henning_Femmer_Interview_13_May_2026.md)

---

## How to Use This File

Each prompt below can be pasted directly into a new session with the appropriate input attached. The pipeline is designed to be resumable: start from the P0 Session Brief and continue from the last completed stage.

---

---

## P0 — Session Resume

> **What this is:** P0 is not a pipeline stage — it is the prompt you use to open a new session and restore context before continuing work. It is always the first thing pasted when resuming. The P0 brief itself is produced at the end of P6 and updated at the end of every subsequent session.

**Role and task:**

```
# ROLE
You are a Requirements Engineer resuming a formal requirements engineering pipeline.

# CONTEXT
The following is the P0 Session Brief from the previous session.
Read it carefully before doing anything else.
It tells you what version the specification is at, what is frozen,
what changed last session, and what still needs to be resolved.

# RULES
  - Do not modify any element marked as FROZEN without explicit instruction
  - Do not close any open item without stakeholder confirmation
  - Do not proceed to the next pipeline stage until the current task is confirmed complete
  - Treat the interview transcript as the sole source of truth —
    no requirement may be introduced that cannot be traced to a direct quote from it

# P0 SESSION BRIEF
[paste the current P0_Session_Brief_RE_Assistant_Tool.md content here]

# INTERVIEW TRANSCRIPT (source of truth)
[paste Henning_Femmer_Interview_13_May_2026.md content here]

# CURRENT SESL SPECIFICATION
[paste SESL_Requirements_RE_Assistant_v1.2.md content here]

# TASK
Confirm you have read the brief, then state:
  1. The current SESL version
  2. The three open items and their owners
  3. What the recommended next task is
  — and wait for instruction before proceeding.
```

**Output:** Oriented session — ready to continue from where the previous session ended.

---

---

## P1 — Goal Elicitation

**Role and task:**

```
# ROLE
You are a Requirements Engineer applying GORE/KAOS notation.

# TASK
From the interview transcript below, identify:
  1. Root goal G0   — what the stakeholder ultimately wants to achieve
  2. Functional sub-goals G1, G2, G3 using AND-decomposition of G0

# CONSTRAINTS
  - Do NOT invent goals. Only derive from what the stakeholder said.
  - Each goal must cite the exact quote from the interview that supports it.
  - Goals are stakeholder-level outcomes — not system features or solutions.
  - All three sub-goals must together fully satisfy G0 (AND-logic).

# OUTPUT FORMAT
  G0 — Root Goal:       [goal statement]   Source: [interview quote]
  G1 — Functional Goal: [goal statement]   Source: [interview quote]
  G2 — Functional Goal: [goal statement]   Source: [interview quote]
  G3 — Functional Goal: [goal statement]   Source: [interview quote]

interview transcript:
[paste interview transcript here]
```

**Output:** [P1 — Goal Elicitation](P1_Goal_Elicitation_GORE_KAOS_RE_Assistant_Tool.md)

---

---

## P2 — Leaf Goal Decomposition

**Role and task:**

```
# ROLE
You are applying KAOS AND-refinement (goal decomposition).

# TASK P2
For each functional sub-goal below, perform AND-decomposition into exactly 2 operational leaf goals.

# CRITERIA FOR A VALID LEAF GOAL (KAOS definition)
  - Assignable to ONE specific agent (system OR human — not both)
  - Observable: you can verify whether it was achieved or not
  - Specific enough that a single system requirement addresses it
  - Still a GOAL (what must hold) — not a feature (how it is built)

# CONSTRAINTS
  - Produce exactly 2 leaf goals per sub-goal
  - Assign each: "Assigned to: system"  or  "Assigned to: human actor"

# OUTPUT FORMAT
  G1: L1a — [leaf goal statement]   Assigned to: system
      L1b — [leaf goal statement]   Assigned to: system

[paste G0, G1, G2, G3 output from P1 here]
```

**Output:** [P2 — Leaf Goal Decomposition](P2_Leaf_Goal_Decomposition_KAOS_RE_Assistant_Tool.md)

---

---

## P3 — Obstacle Analysis

**Role and task:**

```
# ROLE
You are performing KAOS obstacle analysis for an LLM-powered system.

# TASK P3
For each leaf goal, identify the most critical obstacle that could prevent it from
being achieved in this system context.

# FOCUS AREAS — LLM-specific failure modes to prioritise
  - Hallucination:       LLM generates output not grounded in the conversation
  - Wrong timing:        LLM interrupts the interview at an inappropriate moment
  - Over-generation:     LLM produces too many suggestions causing overload
  - Conflict:            LLM surfaces two contradicting recommendations
  - Artifact violation:  LLM modifies data it should not touch (legal risk)

# FOR EACH OBSTACLE, PROVIDE
  Obstacle:   [what could go wrong]
  Resolution: [what the system must do to prevent or counter this]
  Priority:   CRITICAL / HIGH / MEDIUM

# CONSTRAINT
  Do not invent obstacles unrelated to this system context.

# INPUT
[paste L1a, L1b, L2a, L2b, L3a, L3b output from P2 here]
```

**Output:** [P3 — Obstacle Analysis](P3_Obstacle_Analysis_KAOS_RE_Assistant_Tool.md)

---

---

## P4 — SESL Formal Requirements Authoring

**Role and task:**

```
# ROLE
You are writing formal requirements in SESL format.

# TASK P4
Convert each obstacle resolution into one formal SESL requirement.

# SESL FORMAT (use exactly this structure for every requirement)
  FR-[ID]: [Requirement name]
  Goal source:    [G0 → Gx → Lxa]
  Obstacle:       [OBx name]
  Priority:       High / Medium / Low
  ---
  Description:    The system shall [main behaviour statement]
  Precondition:   [what must be true before this executes]
  Trigger:        [event that starts this requirement]
  Main behaviour: 1. [step]   2. [step]   3. [step]  ...
  Postcondition:  [state after successful execution]
  Exception:      [what happens if execution fails]

# RULES
  - Every requirement must trace to a specific leaf goal
  - No implementation details (no model names, API names, internal logic)

[paste full P3 output here]
```

**Output:** [SESL v1.0 → v1.2](../sesl/SESL_Requirements_RE_Assistant_v1.2.md)

---

---

## P5 — CPRE Quality Review

**Role and task:**

```
# ROLE
You are a senior Requirements Engineer performing a quality review.

# TASK P5
Review the SESL specification below against CPRE quality criteria.

# QUALITY CRITERIA TO CHECK
  1. COMPLETE:     Every leaf goal (L1a–L3b) has at least one FR
  2. TRACEABLE:    Every FR links to a specific goal AND an obstacle
  3. CONSISTENT:   No two requirements contradict each other
  4. UNAMBIGUOUS:  Each requirement has exactly one valid interpretation
  5. TESTABLE:     Each requirement has a measurable acceptance condition

# HALLUCINATION CHECK — flag any FR that:
  - Cannot be traced to a stakeholder-stated goal
  - Contains assumptions not supported by interview data

# OUTPUT FORMAT
  Per criterion:   PASS / FAIL  +  one-line explanation
  Per flagged FR:  FR-ID  +  issue description  +  suggested correction

# INPUT
[paste current SESL specification here]
```

**Output:** [P5 — CPRE Quality Review](P5_CPRE_Quality_Review_RE_Assistant_Tool.md)

---

---

## P6 — Cascade Analysis and Correction

**Role and task:**

```
# ROLE
You are checking for cascading effects after specification changes.

# TASK P6
The following changes were applied to the SESL specification in this session.
Check whether any change has cascading effects on other parts of the specification.

# WHAT TO CHECK
  1. Other FRs — does the change to FR-X affect expected behaviour of FR-Y?
  2. Obstacle resolutions — does the change contradict the resolution it was derived from?
  3. Soft goals — does any change risk violating:
       SG1 (no hallucination)
       SG2 (German legal compliance)
       SG3 (no redundancy)
       SG4 (pre-educated LLM behaviour)
  4. NFRs — does any change affect NFR-01 (hallucination gate) or NFR-03 (legal)?

# OUTPUT FORMAT
  For each cascade found:
    Affected: [FR-ID or NFR-ID or SG label]
    Effect:   [description of the cascade]
    Action:   [what needs to change]

  If no cascades: "No cascading effects identified — specification is internally consistent."

# THEN — write the P0 brief for the next session:
  Summarise: current version, what is frozen, what changed, what is still open.

# INPUT — changes applied this session
[paste list of changes from P5 here]

# CURRENT SPECIFICATION
[paste current SESL version here]
```

**Output:** [P6 — Cascade Analysis](P6_Cascade_Analysis_RE_Assistant_Tool.md)  
**Also produces:** [P0 — Session Brief](P0_Session_Brief_RE_Assistant_Tool.md)

---

---

## Pipeline Execution Summary

| Stage | Prompt Input | Output File | SESL Version |
|-------|-------------|-------------|--------------|
| P1 | Interview transcript | P1_Goal_Elicitation | — |
| P2 | P1 output | P2_Leaf_Goal_Decomposition | — |
| P3 | P2 output | P3_Obstacle_Analysis | — |
| P4 | P3 output | SESL_Requirements v1.0 | v1.0 |
| P5 | SESL v1.0 | P5_CPRE_Quality_Review | → v1.1 corrections issued |
| P6 | P5 changes + SESL v1.1 | P6_Cascade_Analysis + P0_Session_Brief | → v1.2 |
| P7 (next) | P0 brief + stakeholder confirmation | TBD | → v1.3 planned |

---

## Resuming the Pipeline (Next Session)

To continue from where this session ended:

1. Paste the [P0 Session Brief](P0_Session_Brief_RE_Assistant_Tool.md) at the start of the new session
2. Conduct elicitation with Prof. Femmer to confirm the three open items
3. Use the P6 prompt structure above to apply confirmed values and run a new cascade check
4. Produce SESL v1.3 as the baseline release candidate
