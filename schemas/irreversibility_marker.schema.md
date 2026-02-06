# GOS-FC  Schema Draft: IrreversibilityMarker (Conceptual)

## Status

DRAFT (conceptual schema; non-executable)

This file defines the required structure and invariants for `IrreversibilityMarker` artifacts in GOS-FC.

If a future machine-readable schema contradicts this file, the contradiction must be resolved explicitly.
This document is authoritative unless amended.

---

## Type

- `artifact_type`: `IrreversibilityMarker`

---

## Purpose

An `IrreversibilityMarker` declares the moment after which **truth preservation becomes non-recoverable** without loss.

It is the point where:
- explanations are required before evidence is secured
- memory and narrative begin competing with artifacts
- later reinterpretation becomes possible

GOS-FC uses this marker to enforce a hard boundary:

> After irreversibility, missing artifacts cannot be reconstructed into accountability.

---

## Required Fields (Conceptual)

### Identity and Timing
- `id` (string, unique, stable)
- `recorded_at` (timestamp; when the marker record was created/issued)
- `effective_at` (timestamp; the irreversibility threshold moment itself)
- `supersedes` (null or list of IrreversibilityMarker ids; supersession only)

---

### Declaring Authority (Hard Requirement)
- `declaring_entity` (object)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

- `authorizing_authority_refs` (list of AuthorityGrant ids; must be non-empty)
- `authorization_assertion` (string; explicit statement of why this entity is authorized to declare irreversibility)

---

### Trigger Definition (Hard Requirement)
- `trigger_class` (enum; disclosure_pressure | evidence_decay | custody_break | narrative_commitment | decision_commitment | other)
- `trigger_statement` (string; explicit condition that caused irreversibility)
- `trigger_scope` (string or structured scope; what systems/decisions it applies to)

---

### Dependencies That Must Be Frozen (Hard Requirement)
- `required_artifact_classes` (list; must be non-empty)
  - Examples: DecisionRecord, AuthorityGrant, AssumptionDeclaration, EvidenceArtifact, CustodyRecord, TransitionRecord, SupersessionRecord
- `frozen_as_of` (timestamp; the time at which required artifacts must be considered locked for compilation)

**Rule:**  
`frozen_as_of` must be **** `effective_at` (truth is locked at or before the threshold, never after).

---

### Declared Consequences
- `consequence_statement` (string; what becomes impossible after this point)
- `expected_failure_modes` (list; may be empty but must exist)
  - Examples: NARRATIVE_UNDERCONSTRAINED, TRUTH_LOCK_MISSING, TIME_INVALID_EVIDENCE, CUSTODY_GAP

---

### Evidence Binding (When Applicable)
- `evidence_refs` (list of EvidenceArtifact ids; may be empty but must exist)
- `evidence_assertion` (string; explicit statement tying evidence availability to irreversibility time)

---

## Forbidden Patterns (Compile-Time Failures)

### F-IR-001  Retroactive Irreversibility
If `recorded_at` occurs after `effective_at` by an unjustified gap (marker created to legitimize later narrative), compilation fails.

**Failure artifact:** `RETROACTIVE_IRREVERSIBILITY_MARKER`

---

### F-IR-002  Truth Lock After the Fact
If `frozen_as_of` is later than `effective_at`, compilation fails.

**Failure artifact:** `TRUTH_LOCK_MISSING`

---

### F-IR-003  Unauthorized Marker
If the marker lacks valid authority references, compilation fails.

**Failure artifact:** `AUTHORITY_NOT_FOUND` or `AUTHORITY_SCOPE_MISMATCH`

---

### F-IR-004  Vague Trigger
If the trigger cannot be stated in a falsifiable way, compilation fails.

**Failure artifact:** `VALUE_OUT_OF_RANGE` (trigger invalid)

---

### F-IR-005  Silent Mutation
IrreversibilityMarkers may not be edited in place. Corrections require supersession.

**Failure artifact:** `MUTATION_DETECTED` or `SUPERSESSION_MISSING`

---

## Deterministic Minimal Valid Example (Fictional)

Illustrative only.

- `id`: IR-0001
- `recorded_at`: 2026-01-01T12:00:00Z
- `effective_at`: 2026-01-01T11:50:00Z
- `trigger_class`: "narrative_commitment"
- `trigger_statement`: "External explanation requested before sealed evidence bundle existed."
- `required_artifact_classes`: ["DecisionRecord","AuthorityGrant","AssumptionDeclaration","EvidenceArtifact","CustodyRecord"]
- `frozen_as_of`: 2026-01-01T11:50:00Z

---

## Canonical Rule

If irreversibility is reached, GOS-FC must **refuse reconstruction**.

After this point, the only valid outputs are:
- compiled accountability (if dependencies were already satisfied), or
- deterministic failure describing what was missing.

