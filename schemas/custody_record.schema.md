# GOS-FC  Schema Draft: CustodyRecord (Conceptual)

## Status

DRAFT (conceptual schema; non-executable)

This file defines the required structure and invariants for `CustodyRecord` artifacts in GOS-FC.

If a future machine-readable schema contradicts this file, the contradiction must be resolved explicitly.
This document is authoritative unless amended.

---

## Type

- `artifact_type`: `CustodyRecord`

---

## Purpose

A `CustodyRecord` preserves chain-of-custody for artifacts (especially EvidenceArtifacts), ensuring that:

- possession/control is explicit
- transfers are time-stamped
- integrity continuity is explainable
- later claims about evidence handling are constrained by records

Custody is not inferred.
If custody is required and not recorded, accountability fails.

---

## Required Fields (Conceptual)

### Identity and Timing
- `id` (string, unique, stable)
- `recorded_at` (timestamp; when this custody record was created/issued)
- `transfer_time` (timestamp; when custody transfer took effect)
- `supersedes` (null or list of CustodyRecord ids; supersession only)

### Subject Artifact
- `artifact_ref` (string; id of the artifact whose custody is being recorded)
- `artifact_type` (string; must correspond to a defined artifact type)

### Transfer Parties
- `from_entity` (object)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

- `to_entity` (object)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

### Transfer Reason and Authorization
- `transfer_reason` (string; why custody moved)
- `authorizing_authority_refs` (list of AuthorityGrant ids; may be empty only if explicitly declared empty)
- `authorization_assertion` (string; explicit statement of authority basis or explicit "No authority grant asserted")

### Handling Controls
- `handling_controls` (list; may be empty but must exist)
  - Examples: sealed storage, access logging, write-blocking, restricted folder, witness presence (fictional)
- `access_restrictions` (list; may be empty but must exist)

### Integrity Continuity
- `integrity_reference_before` (string or null; expected hash/check prior to transfer)
- `integrity_reference_after` (string or null; hash/check after transfer)
- `integrity_check_method` (string or null)
- `integrity_checked_at` (timestamp or null)
- `integrity_checked_by` (string or null)

### Notes and Limitations
- `limitations` (list; may be empty but must exist)

---

## Forbidden Patterns (Compile-Time Failures)

### F-CU-001  Implied Custody
If custody is asserted without an explicit CustodyRecord, compilation fails where custody is required.

**Failure artifact:** `CUSTODY_GAP`

---

### F-CU-002  Time-Ambiguous Transfers
If `transfer_time` is missing or disputed, compilation fails.

**Failure artifact:** `TIME_UNBOUND_ARTIFACT`

---

### F-CU-003  Unexplained Custody Gaps
If custody cannot be represented as a continuous chain across required time windows, compilation fails.

**Failure artifact:** `CUSTODY_GAP`

---

### F-CU-004  Integrity Discontinuity Without Explanation
If integrity references change without recorded justification and method, compilation fails.

**Failure artifact:** `PROVENANCE_UNVERIFIED`

---

### F-CU-005  Silent Mutation
CustodyRecords may not be edited in place. Corrections require supersession.

**Failure artifact:** `MUTATION_DETECTED` or `SUPERSESSION_MISSING`

---

## Deterministic Minimal Valid Example (Fictional)

Illustrative only.

- `id`: CU-0003
- `artifact_ref`: EV-LOG-0007
- `transfer_time`: 2026-01-01T11:05:00Z
- `from_entity`: "On-duty Operator"
- `to_entity`: "Compliance Evidence Custodian"
- `handling_controls`: ["sealed_storage", "access_logged"]

---

## Canonical Rule

If custody matters, it must be recorded.

If it is not recorded, accountability fails deterministically.

