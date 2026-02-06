# GOS-FC  Schema Draft: AssumptionDeclaration (Conceptual)

## Status

DRAFT (conceptual schema; non-executable)

This file defines the required structure and invariants for `AssumptionDeclaration` artifacts in GOS-FC.

If a future machine-readable schema contradicts this file, the contradiction must be resolved explicitly.
This document is authoritative unless amended.

---

## Type

- `artifact_type`: `AssumptionDeclaration`

---

## Purpose

An `AssumptionDeclaration` captures a condition treated as true **without proof** at decision time.

Assumptions are not failures.
**Undeclared assumptions are.**

This artifact exists to ensure assumptions:
- are explicit
- are time-scoped
- can be challenged, reviewed, or invalidated
- do not disappear after outcomes are known

---

## Required Fields (Conceptual)

### Identity and Timing
- `id` (string, unique, stable)
- `declared_at` (timestamp; when the assumption was declared)
- `effective_from` (timestamp; when the assumption applies)
- `effective_until` (timestamp or explicit null with justification)
- `supersedes` (null or list of AssumptionDeclaration ids; supersession only)

### Assumption Content
- `assumption_statement` (string; clear, testable claim treated as true)
- `assumption_scope` (string or structured scope; what decisions/systems it applies to)
- `declaring_entity` (object)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

### Basis and Awareness
- `basis` (string; why this assumption is believed reasonable at the time)
- `known_uncertainties` (list; may be empty but must exist)
- `known_risks_if_false` (list; may be empty but must exist)

### Review and Expiration Discipline
- `review_required` (boolean)
- `review_by` (timestamp or null)
- `review_triggers` (list; may be empty but must exist)
- `invalidated_by` (list of conditions; may be empty but must exist)

---

## Explicit Null Semantics

If no assumptions are being made for a decision, that condition must be declared explicitly:

- `assumption_statement`: "No assumptions declared for this decision"
- `known_uncertainties`: []
- `known_risks_if_false`: []

Silence is not equivalent to none.

---

## Forbidden Patterns (Compile-Time Failures)

### F-AS-001  Implicit Assumptions
If a decision relies on conditions treated as true but no AssumptionDeclaration exists, compilation fails.

**Failure artifact:** `ASSUMPTION_MISSING`

---

### F-AS-002  Post-Hoc Assumptions
If an assumption is declared after outcomes are known and used to justify a prior decision, compilation fails.

**Failure artifact:** `POST_HOC_ASSUMPTION`

---

### F-AS-003  Unscoped Assumptions
If `assumption_scope` is vague or undefined, compilation fails.

**Failure artifact:** `ASSUMPTION_UNSCOPED`

---

### F-AS-004  Perpetual Assumptions Without Review
If `effective_until` is null and `review_required` is false or missing, compilation fails.

**Failure artifact:** `ASSUMPTION_UNREVIEWABLE`

---

### F-AS-005  Silent Mutation
AssumptionDeclarations may not be edited in place. Corrections require supersession.

**Failure artifact:** `MUTATION_DETECTED` or `SUPERSESSION_MISSING`

---

## Deterministic Minimal Valid Example (Fictional)

Illustrative only.

- `id`: AS-OPS-004
- `declared_at`: 2026-01-01T09:00:00Z
- `assumption_statement`: "Human operators will observe and reverse anomalous parameter changes within 60 seconds."
- `assumption_scope`: "Operations-Control / Anomaly Response"
- `review_required`: true
- `review_by`: 2026-01-31T00:00:00Z

---

## Canonical Rule

If an assumption mattered, it had to be declared **before** it mattered.

If it wasnt declared, accountability fails.

