# GOS-FC  Schema Draft: EvidenceArtifact (Conceptual)

## Status

DRAFT (conceptual schema; non-executable)

This file defines the required structure and invariants for `EvidenceArtifact` inputs to GOS-FC.

If a future machine-readable schema contradicts this file, the contradiction must be resolved explicitly.
This document is authoritative unless amended.

---

## Type

- `artifact_type`: `EvidenceArtifact`

---

## Purpose

An `EvidenceArtifact` represents factual material that was **available at decision time**.

Evidence exists to constrain decisions, not to justify them after the fact.

If evidence was not available when a decision was made, it cannot satisfy that decisions dependencies.

---

## Required Fields (Conceptual)

### Identity and Classification
- `id` (string, unique, stable)
- `evidence_kind` (enum; e.g., log, snapshot, document, measurement, attestation, configuration_state)
- `description` (string; what this artifact is, without interpretation)

### Creation and Availability (Hard Requirement)
- `created_at` (timestamp; when the artifact came into existence)
- `available_at` (timestamp; when it became accessible to decision-makers)

**Rule:**  
`available_at` must be **** the `effective_at` of any DecisionRecord that references this artifact.

---

### Source and Provenance
- `producing_entity` (object)
  - `name` (string)
  - `role` (string)
  - `organization_unit` (string; may be fictional)
  - `identifier` (string; local id; must not be a real person identifier in this public repo)

- `origin_system` (string; system or process that produced the artifact)
- `collection_method` (string; how the artifact was obtained)

---

### Integrity Anchors
- `integrity_reference` (string; hash, checksum, or equivalent)
- `integrity_method` (string; e.g., SHA-256, signed attestation)
- `integrity_verified_at` (timestamp or null)
- `integrity_verified_by` (string or null)

---

### Context and Scope
- `context_scope` (string or structured scope; what this evidence applies to)
- `limitations` (list; known gaps or limits; may be empty but must exist)

---

## Explicit Null Semantics

If no evidence was available at decision time, that condition must be declared explicitly:

- `evidence_kind`: "none"
- `description`: "No evidence artifacts available at decision time"
- `limitations`: ["Decision relied on judgment without evidence artifacts"]

Silence is not equivalent to none.

---

## Forbidden Patterns (Compile-Time Failures)

### F-EV-001  Time-Invalid Evidence
If `available_at` is later than the decision it is used to justify, compilation fails.

**Failure artifact:** `TIME_INVALID_EVIDENCE`

---

### F-EV-002  Post-Hoc Justification
If an EvidenceArtifact is introduced solely to rationalize a completed decision, compilation fails.

**Failure artifact:** `POST_HOC_EVIDENCE`

---

### F-EV-003  Ambiguous Availability
If `available_at` is missing, inferred, or disputed, compilation fails.

**Failure artifact:** `TIME_UNBOUND_ARTIFACT`

---

### F-EV-004  Integrity-Free Evidence
If `integrity_reference` is missing or unverifiable for evidence that requires integrity guarantees, compilation fails.

**Failure artifact:** `PROVENANCE_UNVERIFIED`

---

### F-EV-005  Silent Mutation
EvidenceArtifacts may not be altered in place. Corrections require supersession.

**Failure artifact:** `MUTATION_DETECTED` or `SUPERSESSION_MISSING`

---

## Deterministic Minimal Valid Example (Fictional)

Illustrative only.

- `id`: EV-OBS-002
- `evidence_kind`: "operator_observation"
- `description`: "Operator observed parameter value exceeding expected range on control interface."
- `created_at`: 2026-01-01T09:58:00Z
- `available_at`: 2026-01-01T09:58:00Z
- `origin_system`: "Operations-Control-UI"
- `integrity_reference`: "hash:abc123..."

---

## Canonical Rule

Evidence must constrain decisions **before** they are made.

If evidence arrives after the decision, it can explain history  but it cannot compile accountability.

