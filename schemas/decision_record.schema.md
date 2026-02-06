# GOS-FC  Schema Draft: DecisionRecord (Conceptual)

## Status

DRAFT (conceptual schema; non-executable)

This file defines the required structure and invariants for `DecisionRecord` artifacts in GOS-FC.

If a future machine-readable schema contradicts this file, the contradiction must be resolved explicitly.
This document is authoritative unless amended.

---

## Type

- `artifact_type`: `DecisionRecord`

---

## Purpose

A `DecisionRecord` captures an explicit commitment to a course of action **at decision time**.

It exists to make the following reconstructable without interpretation:

- what was decided
- when it was decided
- who decided
- under what authority
- under which declared assumptions
- what evidence was available at the time

---

## Required Fields (Conceptual)

### Identity and Versioning
- `id` (string, unique, stable)
- `issued_at` (timestamp, when this record was created/issued)
- `effective_at` (timestamp, when the decision took effect; may equal `issued_at`)
- `supersedes` (null or list of DecisionRecord ids; supersession only, never mutation)

### Decision Content
- `decision_statement` (string; the commitment in plain language)
- `decision_scope` (string or structured scope; what domain/system/process is affected)
- `decision_class` (enum; e.g., routine | emergency | exception | risk_acceptance)
- `decision_constraints` (list; explicit constraints, can be empty but must exist)

### Actor / Signer
- `decider` (object)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

### Authority Binding (Hard Requirement)
- `authority_refs` (list of AuthorityGrant ids; must be non-empty)
- `authority_assertion` (string; explicit statement of why referenced authority grants apply)

### Assumption Binding (Hard Requirement)
- `assumption_refs` (list of AssumptionDeclaration ids; may be empty only if explicitly declared empty)
- `assumption_assertion` (string; either:
  - "No assumptions declared" (explicit) OR
  - statement tying assumptions to the decision)

### Evidence Binding (Decision-Time Only)
- `evidence_refs` (list of EvidenceArtifact ids; may be empty only if explicitly declared empty)
- `evidence_assertion` (string; either:
  - "No evidence artifacts available at decision time" OR
  - statement tying evidence to decision time)

### Alternatives and Rationale Discipline
- `alternatives_considered` (list; may be empty but must exist)
- `rationale_at_time` (string; must be contemporaneous; must not reference later outcomes)

### Review / Revalidation
- `review_required` (boolean)
- `review_by` (timestamp or null)
- `review_triggers` (list; may be empty but must exist)

---

## Forbidden Patterns (Compile-Time Failures)

### F-DR-001  Implicit Authority
A DecisionRecord that omits `authority_refs` or relies on role proximity, urgency, or common understanding fails compilation.

**Failure artifact:** `AUTHORITY_NOT_FOUND` or `AUTHORITY_AMBIGUOUS`

---

### F-DR-002  Post-Hoc Rationale
If `rationale_at_time` references outcomes, later discoveries, later investigations, or hindsight framing, compilation fails.

**Failure artifact:** `POST_HOC_RATIONALE`

---

### F-DR-003  Missing Assumption Declaration (When Assumptions Are Used)
If the decision asserts conditions treated as true but provides no `assumption_refs`, compilation fails.

**Failure artifact:** `ASSUMPTION_MISSING`

---

### F-DR-004  Time-Invalid Evidence
If evidence referenced was not available at decision time (availability later than `effective_at`), compilation fails.

**Failure artifact:** `TIME_INVALID_EVIDENCE`

---

### F-DR-005  Silent Mutation
A DecisionRecord may not be edited in place. Any correction must create a new record and populate `supersedes`.

**Failure artifact:** `MUTATION_DETECTED` or `SUPERSESSION_MISSING`

---

## Deterministic Minimal Valid Example (Fictional)

This is illustrative only; it is not a claim about any real incident.

- `id`: DR-0001
- `issued_at`: 2026-01-01T10:00:00Z
- `effective_at`: 2026-01-01T10:00:00Z
- `decision_statement`: "Revert parameter P to baseline and freeze changes pending review."
- `authority_refs`: ["AG-OPS-001"]
- `assumption_refs`: ["AS-OPS-004"]
- `evidence_refs`: ["EV-OBS-002"]

---

## Canonical Rule

A DecisionRecord is admissible only if it can be compiled **without interpretation**.

If it requires a human to fill in what they meant, it fails.

