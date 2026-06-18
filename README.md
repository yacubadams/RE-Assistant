# RE Assistant Tool — Requirements Artifact Repository

> **Stakeholder:** Prof. H. Femmer  
> **Users:** Requirements Engineering (RE) Teams  
> **Method:** GORE / KAOS Goal Modelling + SESL Formal Specification  
> **Current SESL Version:** `v1.2` — Cascade Corrections Applied

---

## What Is This?

This repository contains the full requirements engineering artifact chain for the **RE Assistant Tool** — an LLM-powered system designed to guide Requirements Engineers during elicitation interviews, produce traceable summaries, and ensure no hallucinated or ungrounded requirements enter the specification baseline.

The artifacts were produced using a formal pipeline (P1–P6) applying GORE/KAOS goal modelling, KAOS obstacle analysis, and SESL-format requirements authoring, followed by CPRE quality review and cascade correction.

---

## Artifact Index

| File | Stage | Contents |
|------|-------|----------|
| [P0 — Session Brief](pipeline/P0_Session_Brief_RE_Assistant_Tool.md) | P0 | Handover brief for next session — frozen elements, open items, recommended task |
| [P1 — Goal Elicitation](pipeline/P1_Goal_Elicitation_GORE_KAOS_RE_Assistant_Tool.md) | P1 | Root goal G0 + functional sub-goals G1, G2, G3 via GORE/KAOS AND-decomposition |
| [P2 — Leaf Goal Decomposition](pipeline/P2_Leaf_Goal_Decomposition_KAOS_RE_Assistant_Tool.md) | P2 | Operational leaf goals L1a–L3b assigned to system / human actors |
| [P3 — Obstacle Analysis](pipeline/P3_Obstacle_Analysis_KAOS_RE_Assistant_Tool.md) | P3 | LLM-specific obstacle analysis for all six leaf goals |
| [SESL v1.2 — Formal Requirements](sesl/SESL_Requirements_RE_Assistant_v1.2.md) | P4–P6 | Six formal requirements FR-01–FR-06 + Glossary |

---

## Pipeline Overview

```
Interview Transcript (Prof. H. Femmer)
        │
        ▼
P1 — Goal Elicitation          G0 → G1 / G2 / G3
        │
        ▼
P2 — Leaf Goal Decomposition   L1a, L1b / L2a, L2b / L3a, L3b
        │
        ▼
P3 — Obstacle Analysis         OB-L1a through OB-L3b
        │
        ▼
P4 — SESL Authoring            FR-01 through FR-06
        │
        ▼
P5 — CPRE Quality Review       Ambiguity, testability, hallucination flags
        │
        ▼
P6 — Cascade Correction        v1.1 → v1.2 (internally consistent)
        │
        ▼
P7 (next session) — Stakeholder confirmation of 3 open items → v1.3
```

---

## Version History

| Version | Stage | Key Changes |
|---------|-------|-------------|
| `v1.0` | P4 | Initial SESL authoring — FR-01 to FR-06 |
| `v1.1` | P5 | CPRE review: mechanism assumptions removed, numeric ceiling removed, Glossary created (3 entries) |
| `v1.2` | P6 | Cascade corrections: FR-02 terminology aligned, FR-04 Glossary reference added, 4th Glossary entry added |
| `v1.3` | P7 (planned) | Stakeholder confirmation items — suggestion ceiling, contribution detection criteria, authorisation gap |

---

## Open Items (Pending Before v1.3)

| # | Requirement | Item | Owner |
|---|-------------|------|-------|
| 1 | FR-02 | Numeric ceiling for suggestions per delivery opportunity not yet confirmed | Prof. Femmer |
| 2 | FR-01 | Criteria for detecting when no participant is actively contributing not yet confirmed | Prof. Femmer |
| 3 | FR-06 / NFR-03 | Authorisation gap — system trusts presented identity without independent check. May violate German legal compliance (SG2). Requires new NFR or FR-07. | Prof. Femmer + RE |

---

## Methodology Reference

| Term | Description |
|------|-------------|
| **GORE** | Goal-Oriented Requirements Engineering |
| **KAOS** | Keep All Objects Satisfied — goal/obstacle modelling framework |
| **SESL** | Structured English Specification Language |
| **CPRE** | Certified Professional for Requirements Engineering quality criteria |
| **AND-decomposition** | All sub-goals must hold for the parent goal to be satisfied |
| **Leaf goal** | An operational goal assignable to a single agent (system or human) |
| **Obstacle** | A condition that prevents a goal from being achieved |
