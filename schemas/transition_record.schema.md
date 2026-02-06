# GOS-FC  Schema Draft: TransitionRecord (Conceptual)

## Status

DRAFT (conceptual schema; non-executable)

This file defines the required structure and invariants for `TransitionRecord` artifacts in GOS-FC.

If a future machine-readable schema contradicts this file, the contradiction must be resolved explicitly.
This document is authoritative unless amended.

---

## Type

- `artifact_type`: `TransitionRecord`

---

## Purpose

A `TransitionRecord` captures an explicit change in:

- authority
- role or responsibility
- custody context
- system state relevant to governance

Transitions are where ambiguity is introduced if not recorded.
If a transition occurred and is not recorded, accountability fails.

---

## Required Fields (Conceptual)

### Identity and Timing
- `id` (string, unique, stable)
- `recorded_at` (timestamp; when this transition record was created/issued)
- `effective_at` (timestamp; when the transition took effect)
- `supersedes` (null or list of TransitionRecord ids; supersession only)

---

### Transition Subject
- `transition_subject` (enum; authority | role | responsibility | system_state | custody_context)
- `subject_identifier` (string; id or name of the subject being transitioned)

---

### Prior and New State (Hard Requirement)
- `prior_state` (string or structured object; must be explicit)
- `new_state` (string or structured object; must be explicit)

---

### Parties Involved
- `from_entity` (object; entity relinquishing state, if applicable)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

- `to_entity` (object; entity assuming state, if applicable)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

---

### Authorization (Hard Requirement)
- `authorizing_authority_refs` (list of AuthorityGrant ids; must be non-empty)
- `authorization_assertion` (string; explicit statement of why this authority applies)

---

### Scope and Impact
- `transition_scope` (string or structured scope; what systems/processes are affected)
- `impact_summary` (string; governance-relevant impact, not narrative)

---

### Constraints and Conditions
- `constraints` (list; may be empty but must exist)
- `conditions` (list; triggering or boundary conditions; may be empty but must exist)

---

### Evidence Binding
- `evidence_refs` (list of EvidenceArtifact ids; may be empty only if explicitly declared empty)
- `evidence_assertion` (string; explicit statement tying evidence availability to transition time)

---

### Review and Revalidation
- `review_required` (boolean)
- `review_by` (timestamp or null)
- `review_triggers` (list; may be empty but must exist)

---

## Explicit Null Semantics

If a transition does not involve certain elements (e.g., no evidence artifacts), that absence must be declared explicitly.

Silence is not equivalent to none.

---

## Forbidden Patterns (Compile-Time Failures)

### F-TR-001  Implicit Transition
If a state change is inferred from behavior or outcome without a TransitionRecord, compilation fails.

**Failure artifact:** `TRANSITION_UNRECORDED`

---

### F-TR-002  Time-Ambiguous Transition
If `effective_at` is missing, inferred, or disputed, compilation fails.

**Failure artifact:** `TIME_UNBOUND_ARTIFACT`

---

### F-TR-003  Unauthorized Transition
If `authorizing_authority_refs` is empty or invalid, compilation fails.

**Failure artifact:** `AUTHORITY_NOT_FOUND` or `AUTHORITY_SCOPE_MISMATCH`

---

### F-TR-004  Retroactive Normalization
If a TransitionRecord is created to legitimize an earlier unrecorded transition, compilation fails.

**Failure artifact:** `RETROACTIVE_TRANSITION`

---

### F-TR-005  Silent Mutation
TransitionRecords may not be edited in place. Corrections require supersession.

**Failure artifact:** `MUTATION_DETECTED` or `SUPERSESSION_MISSING`

---

## Deterministic Minimal Valid Example (Fictional)

Illustrative only.

- `id`: TR-ROLE-0002
- `transition_subject`: "role"
- `subject_identifier`: "On-Call Operations Lead"
- `prior_state`: "Standby"
- `new_state`: "Active Authority Holder"
- `effective_at`: 2026-01-01T09:45:00Z
- `authorizing_authority_refs`: ["AG-MGMT-002"]

---

## Canonical Rule

If something changed and it mattered, it must have a TransitionRecord.

If it does not, accountability fails deterministically.

