# GOS-FC Canonical Forensic Bundle Specification

## Purpose

This document defines the **entire forensic bundle format** produced by the
Governance Operating Substrate  Forensic Compiler (GOS-FC).

A bundle is a **single, self-contained accountability artifact** representing
exactly one compilation attempt.

That attempt results in either:
- a successfully compiled accountability record, or
- a deterministic compilation failure.

There are no partial successes.

---

## Bundle Invariants

1. The bundle is append-only.
2. No artifact may be silently overwritten.
3. Corrections require explicit supersession.
4. All records are time-bound.
5. The bundle must be interpretable without any external system.
6. Missing information is recorded explicitly, never inferred.
7. Failure artifacts are first-class outputs.

---

## Manifest Section

This section uniquely identifies the compilation run.

Required fields:
- Bundle Identifier
- Compilation Timestamp
- Compiler Version
- Run Mode (success | failure)

The manifest is immutable once written.

---

## Metadata Section

Metadata provides contextual information only.

Optional fields may include:
- Institution identifier (fictional or real)
- Scenario identifier
- Jurisdictional scope

Metadata must not assert authority, intent, correctness, or justification.

---

## Decision Records Section

Contains decision records.

Each decision must declare:
- Decision identifier
- Decision timestamp
- Decision scope
- Referenced approver(s)

Undocumented decisions must not appear.

---

## Authority and Custody Section

### Authority Grants

Each authority grant must declare:
- Grantor
- Recipient
- Scope
- Duration

Implicit authority is forbidden.

### Custody Records

Each custody record must declare:
- Custodian
- Transfer event
- Timestamp
- Artifact reference

Broken custody chains create provenance gaps.

---

## Assumptions Section

Each assumption must declare:
- Assumption identifier
- Declaration timestamp
- Scope
- Expiration (if applicable)

Undeclared assumptions must not be reconstructed.

---

## Evidence Section

Each evidence artifact must declare:
- Evidence identifier
- Availability timestamp
- Attribution reference
- Relationship to decision timing

Evidence created after a decision must be marked as post-outcome evidence
and carries no justificatory weight.

---

## Transition Records Section

Each transition must declare:
- Role changes
- Authority changes
- System state transitions
- Timestamps

Unrecorded transitions create irreversible provenance gaps.

---

## Supersession Records Section

Each supersession must declare:
- Superseding artifact
- Superseded artifact
- Reason for correction
- Timestamp

Silent mutation is forbidden.

---

## Context Section (Optional)

Contains non-authoritative contextual data.

Context may inform understanding but must never justify decisions.

---

## Integrity Section (Optional)

Contains integrity state declarations.

Integrity does not imply authorization or correctness.

---

## Failure Section (Required on Failure)

If compilation fails, this section must enumerate:
- Failure codes
- Missing decisions
- Missing authority grants
- Missing assumptions
- Evidence timing violations
- Authority ambiguities
- Irreversibility markers

Failure output is terminal.

---

## Checksums Section

Contains integrity markers covering the entire document.

Any modification invalidates the bundle.

---

## Canonical Rule

If accountable truth cannot be proven from this document alone,
accountability does not exist.
