# GOS-FC  Schema Draft: SupersessionRecord (Conceptual)

## Status

DRAFT (conceptual schema; non-executable)

This file defines the required structure and invariants for `SupersessionRecord` artifacts in GOS-FC.

If a future machine-readable schema contradicts this file, the contradiction must be resolved explicitly.
This document is authoritative unless amended.

---

## Type

- `artifact_type`: `SupersessionRecord`

---

## Purpose

A `SupersessionRecord` replaces a prior artifact **without erasing history**.

Supersession is the only admissible mechanism for:
- correcting errors
- refining statements
- updating constraints
- replacing incomplete records

If a record is changed without supersession, that is mutation  and compilation must fail.

---

## Required Fields (Conceptual)

### Identity and Timing
- `id` (string, unique, stable)
- `recorded_at` (timestamp; when this supersession record was created/issued)
- `effective_at` (timestamp; when the supersession takes effect)
- `supersedes` (null or list of SupersessionRecord ids; supersession records themselves may be superseded)

---

### Supersession Linkage (Hard Requirement)
- `superseded_artifact_id` (string; id of the artifact being replaced)
- `superseded_artifact_type` (string; must be a defined artifact type)
- `superseding_artifact_id` (string; id of the replacing artifact)
- `superseding_artifact_type` (string; must be a defined artifact type)

**Rule:**  
Supersession may only occur between compatible types unless explicitly allowed.
Example: DecisionRecord  DecisionRecord is allowed.
DecisionRecord  AuthorityGrant is not allowed.

---

### Reason and Scope
- `supersession_reason` (string; explicit reason for replacement)
- `supersession_scope` (string or structured scope; what part of the prior artifact is being corrected/replaced)
- `supersession_class` (enum; correction | completion | clarification | constraint_update | reissue)

---

### Authorization (Hard Requirement)
- `authorizing_authority_refs` (list of AuthorityGrant ids; must be non-empty)
- `authorization_assertion` (string; explicit statement of why this authority applies)

---

### Evidence Binding (When Applicable)
- `evidence_refs` (list of EvidenceArtifact ids; may be empty but must exist)
- `evidence_assertion` (string; explicit statement tying evidence availability to supersession time)

---

### Integrity Expectations
- `integrity_reference_superseded` (string or null)
- `integrity_reference_superseding` (string or null)
- `integrity_method` (string or null)

---

### Impact Declaration
- `impact_statement` (string; what changes materially as a result of supersession)
- `non_changes` (list; what is explicitly unchanged; may be empty but must exist)

---

## Forbidden Patterns (Compile-Time Failures)

### F-SR-001  Silent Overwrite
If an artifact content changes without a SupersessionRecord linking old  new, compilation fails.

**Failure artifact:** `MUTATION_DETECTED` or `SUPERSESSION_MISSING`

---

### F-SR-002  Broken Lineage
If an artifact claims to supersede something but no SupersessionRecord exists (or the chain is incomplete), compilation fails.

**Failure artifact:** `SUPERSESSION_CHAIN_BROKEN`

---

### F-SR-003  Unauthorized Supersession
If `authorizing_authority_refs` is empty or invalid, compilation fails.

**Failure artifact:** `AUTHORITY_NOT_FOUND` or `AUTHORITY_SCOPE_MISMATCH`

---

### F-SR-004  Retroactive Narrative Laundering
If supersession is used to replace contemporaneous records with narrative summaries (or to remove ambiguity by rewriting history), compilation fails.

**Failure artifact:** `NARRATIVE_UNDERCONSTRAINED` or `TRUTH_LOCK_MISSING`

---

### F-SR-005  Type-Incompatible Replacement
If supersession crosses incompatible artifact types without an explicit allowed mapping, compilation fails.

**Failure artifact:** `VALUE_OUT_OF_RANGE` or `UNKNOWN_ARTIFACT_TYPE` (depending on enforcement stage)

---

## Deterministic Minimal Valid Example (Fictional)

Illustrative only.

- `id`: SR-0002
- `recorded_at`: 2026-01-02T10:00:00Z
- `effective_at`: 2026-01-02T10:00:00Z
- `superseded_artifact_id`: DR-0001
- `superseding_artifact_id`: DR-0001A
- `supersession_reason`: "Correction: authority reference omitted in original record."
- `authorizing_authority_refs`: ["AG-COMPLIANCE-001"]

---

## Canonical Rule

Corrections must preserve history.

If history is rewritten, accountability fails deterministically.

