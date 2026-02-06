# GOS-FC  Artifact Types (Conceptual Type System)

## Purpose

This document defines the **canonical artifact types** that GOS-FC accepts as inputs.

It is a **type system**, not a schema definition and not an implementation.

Every artifact consumed by GOS-FC must:
- declare exactly one type defined here
- satisfy the conceptual requirements of that type
- fail compilation if required elements are absent

Undefined artifact types are rejected.

---

## Design Rule

Artifacts define **what was asserted at a point in time**.

They do not:
- explain outcomes
- infer intent
- summarize narratives
- resolve ambiguity

Each artifact type has a narrow, enforceable responsibility.

---

## Artifact Type Index

1. DecisionRecord
2. AuthorityGrant
3. AssumptionDeclaration
4. EvidenceArtifact
5. CustodyRecord
6. TransitionRecord
7. SupersessionRecord
8. IrreversibilityMarker
9. FailureArtifact (output-only)

---

## 1. DecisionRecord

### Responsibility
Captures an explicit commitment to a course of action.

### Must Declare
- unique decision identifier
- decision timestamp
- decision statement (what was chosen)
- decision-maker identity
- referenced authority grant(s)
- referenced assumption declaration(s)
- alternatives considered (may be empty but must be explicit)

### Prohibitions
- post-hoc rationale
- inferred decisions
- decisions implied by action alone

---

## 2. AuthorityGrant

### Responsibility
Defines who is allowed to decide what, when, and under what constraints.

### Must Declare
- grant identifier
- granter identity
- grantee identity
- scope of authority
- start time
- end time (or explicit non-expiration)
- constraints and limitations

### Prohibitions
- role-based inference
- emergency authority without explicit declaration
- open-ended scope without bounds

---

## 3. AssumptionDeclaration

### Responsibility
Declares a condition treated as true at decision time without proof.

### Must Declare
- assumption identifier
- assumption statement
- declaring entity
- declaration time
- scope of applicability
- review or expiration condition

### Prohibitions
- implicit assumptions
- assumptions introduced after outcomes are known

---

## 4. EvidenceArtifact

### Responsibility
Provides factual material available at decision time.

### Must Declare
- evidence identifier
- artifact type (log, snapshot, document, attestation, etc.)
- creation time
- availability time
- producing system or entity
- integrity reference (hash or equivalent)

### Prohibitions
- time-ambiguous evidence
- evidence introduced post-decision to justify a decision

---

## 5. CustodyRecord

### Responsibility
Preserves chain-of-custody for artifacts.

### Must Declare
- custody record identifier
- artifact identifier
- from-entity
- to-entity
- transfer time
- custody justification

### Prohibitions
- implied custody
- undocumented transfers

---

## 6. TransitionRecord

### Responsibility
Records changes in authority, role, responsibility, or system state.

### Must Declare
- transition identifier
- subject of transition
- prior state
- new state
- effective time
- authorizing authority

### Prohibitions
- implicit transitions
- retroactive state changes

---

## 7. SupersessionRecord

### Responsibility
Replaces a prior artifact while preserving history.

### Must Declare
- supersession identifier
- superseded artifact identifier
- superseding artifact identifier
- reason for supersession
- supersession time

### Prohibitions
- silent overwrite
- deletion without replacement

---

## 8. IrreversibilityMarker

### Responsibility
Declares the moment after which truth can no longer be preserved without loss.

### Must Declare
- marker identifier
- triggering condition
- declaration time
- declaring authority
- scope of irreversibility

### Prohibitions
- retroactive declaration
- informal recognition without record

---

## 9. FailureArtifact (Output Only)

### Responsibility
Documents deterministic compilation failure.

### Must Declare
- failure identifier
- failed pipeline stage
- unresolved dependency list
- violated rule identifiers
- reproducibility reference

### Prohibitions
- narrative explanation
- blame assignment
- probability or inference

---

## Canonical Enforcement Rule

If an artifact:
- lacks required conceptual fields
- violates type prohibitions
- blurs responsibility boundaries

Then compilation must fail.

---

## Relationship to Schemas

Future YAML/JSON schemas must:
- conform to these type definitions
- not weaken requirements
- not introduce optionality where this document requires presence

If a schema contradicts this file, **this file is authoritative**.

