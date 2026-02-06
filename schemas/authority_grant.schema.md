# GOS-FC  Schema Draft: AuthorityGrant (Conceptual)

## Status

DRAFT (conceptual schema; non-executable)

This file defines the required structure and invariants for `AuthorityGrant` artifacts in GOS-FC.

If a future machine-readable schema contradicts this file, the contradiction must be resolved explicitly.
This document is authoritative unless amended.

---

## Type

- `artifact_type`: `AuthorityGrant`

---

## Purpose

An `AuthorityGrant` is the only admissible mechanism for establishing decision authority.

Authority is not inferred from:
- job titles
- proximity
- urgency
- common understanding
- historical practice
- vendor involvement

If it is not granted explicitly, it does not exist.

---

## Required Fields (Conceptual)

### Identity and Versioning
- `id` (string, unique, stable)
- `issued_at` (timestamp; when the grant record was created/issued)
- `effective_from` (timestamp; when the authority becomes valid)
- `effective_until` (timestamp or explicit `null` with an explicit non-expiration justification)
- `supersedes` (null or list of AuthorityGrant ids; supersession only)

### Parties
- `granter` (object)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

- `grantee` (object)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

### Scope (Hard Requirement)
- `scope` (object; must be explicit and bounded)
  - `domain` (string; what system/process area)
  - `actions_permitted` (list of strings; must be non-empty)
  - `actions_prohibited` (list of strings; may be empty but must exist)
  - `objects_covered` (list of strings; what assets/interfaces/processes are in scope)
  - `objects_excluded` (list of strings; may be empty but must exist)

### Constraints (Hard Requirement)
- `constraints` (list; must exist, may be empty only if explicitly stated)
- `approval_requirements` (list; may be empty but must exist)
- `escalation_requirements` (list; may be empty but must exist)

### Justification and Traceability
- `grant_basis` (string; why this authority is granted)
- `grant_evidence_refs` (list of EvidenceArtifact ids; may be empty but must exist)
- `revocation_conditions` (list; must exist)

---

## Emergency Authority Discipline (Explicit, Not Implied)

Emergency authority is not a feeling.
It is a declared, time-bounded grant.

If a grant supports emergency actions, it must include:

- `emergency` (object)
  - `is_emergency_grant` (boolean)
  - `trigger_conditions` (list; must be non-empty if true)
  - `maximum_duration` (duration or timestamp bound)
  - `mandatory_post_event_review` (boolean; must be true if emergency)
  - `required_review_window` (duration; if emergency)

If `is_emergency_grant` is false or absent:
- emergency actions are not authorized by this grant.

---

## Forbidden Patterns (Compile-Time Failures)

### F-AG-001  Implicit Authority
If authority is asserted without an AuthorityGrant id, compilation fails.

**Failure artifact:** `AUTHORITY_NOT_FOUND`

---

### F-AG-002  Unbounded Scope
If `actions_permitted` is vague (e.g., all actions) or `scope.domain` is undefined, compilation fails.

**Failure artifact:** `AUTHORITY_SCOPE_MISMATCH` or `AUTHORITY_AMBIGUOUS`

---

### F-AG-003  Retroactive Grants
If `issued_at` occurs after `effective_from` by an unjustified or suspicious gap, or if a grant is created after an incident to legitimize past decisions, compilation fails.

**Failure artifact:** `RETROACTIVE_AUTHORITY_GRANT` (or `TIME_INVALID_EVIDENCE` where applicable)

---

### F-AG-004  Emergency Without Declaration
If actions are classified as emergency but no emergency grant exists, compilation fails.

**Failure artifact:** `EMERGENCY_AUTHORITY_UNDECLARED`

---

### F-AG-005  Silent Mutation
AuthorityGrants may not be edited in place. Corrections require supersession.

**Failure artifact:** `MUTATION_DETECTED` or `SUPERSESSION_MISSING`

---

## Deterministic Minimal Valid Example (Fictional)

Illustrative only; not a claim about any real entity.

- `id`: AG-OPS-001
- `issued_at`: 2026-01-01T08:00:00Z
- `effective_from`: 2026-01-01T08:00:00Z
- `effective_until`: 2026-12-31T23:59:59Z
- `scope.domain`: "Operations-Control"
- `actions_permitted`: ["adjust_parameter", "revert_change", "freeze_changes_pending_review"]
- `constraints`: ["No permanent configuration change without second approver"]

---

## Canonical Rule

If authority must be inferred, accountability has already failed.

Authority must be provable, scoped, and time-valid  or compilation must halt.

