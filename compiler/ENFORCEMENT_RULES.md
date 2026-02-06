# GOS-FC  Enforcement Rules (Pipeline  Failures Map)

## Purpose

This document binds the compilation pipeline stages (compiler/PIPELINE.md) to the canonical failures (compiler/FAILURE_CATALOG.md).

It prevents:
- best effort accountability
- failure softening
- stage skipping
- narrative laundering via permissive diagnostics

If an implementation violates this file, it violates doctrine.

---

## Global Enforcement Axioms

### ER-G-001  Deterministic Halt on BLOCKER
If any `BLOCKER` failure is triggered, compilation must halt and emit a failure bundle.

No partial success is allowed.

### ER-G-002  No Inference
The compiler must not infer:
- intent
- causality
- likelihood
- blame
- most plausible narrative

If the artifact set permits multiple incompatible explanations, it must fail.

### ER-G-003  No Silent Coercion
The compiler may not:
- auto-fill missing fields
- substitute unknown
- normalize away ambiguity
- accept post-hoc artifacts as decision-time dependencies

### ER-G-004  Single-Source Authority
PIPELINE.md + FAILURE_CATALOG.md + schemas/* are authoritative.
Implementation must match them.

### ER-G-005  Stage Order Is Fixed
Stages run in order. Earlier stage failures block later evaluation.
(Example: if schema fails, authority resolution is not attempted.)

---

## Stage-by-Stage Enforcement

### Stage 0  Intake & Normalization
**Allowed failures:**
- FC-0001 INVALID_INPUT_FORMAT
- FC-0002 UNKNOWN_ARTIFACT_TYPE
- FC-0003 DUPLICATE_ID

**Mandatory rules:**
- Reject unknown types immediately.
- Reject duplicate IDs immediately.

**Must not do:**
- Accept partially parsed artifacts.
- Guess missing artifact_type.

---

### Stage 1  Structural Validation (Schemas)
**Allowed failures:**
- FC-0101 SCHEMA_VIOLATION

**Mandatory rules:**
- Every artifact must satisfy its conceptual schema.
- Required fields are required. No defaults.

**Must not do:**
- Coerce types (e.g., string  timestamp).
- Treat missing lists as empty without explicit declaration.

---

### Stage 2  Time Binding (Decision-Time Truth Conditions)
**Allowed failures:**
- FC-0201 TIME_UNBOUND_ARTIFACT
- FC-0202 TIME_INVALID_EVIDENCE
- FC-0203 POST_HOC_RATIONALE
- FC-0204 POST_HOC_ASSUMPTION
- FC-0205 POST_HOC_EVIDENCE

**Mandatory rules:**
- EvidenceArtifact.available_at must be <= DecisionRecord.effective_at when referenced.
- Post-hoc rationale/assumptions/evidence cannot satisfy dependencies.
- If required time fields are missing, fail.

**Must not do:**
- Accept approximate time as satisfying binding.
- Allow later evidence to justify earlier decisions.

---

### Stage 3  Authority Resolution (Type Checking)
**Allowed failures:**
- FC-0301 AUTHORITY_NOT_FOUND
- FC-0302 AUTHORITY_SCOPE_MISMATCH
- FC-0303 AUTHORITY_AMBIGUOUS
- FC-0304 EMERGENCY_AUTHORITY_UNDECLARED
- FC-0305 RETROACTIVE_AUTHORITY_GRANT

**Mandatory rules:**
- Every decision and governed transition must bind to explicit AuthorityGrant refs.
- Authority must be valid in time and scope at the moment of use.
- If multiple authority paths exist and the record does not bind one, fail.

**Must not do:**
- Infer authority from role/title.
- Treat it was urgent as authority.

---

### Stage 4  Assumption Closure (Dependency Checking)
**Allowed failures:**
- FC-0401 ASSUMPTION_MISSING
- FC-0402 ASSUMPTION_UNSCOPED
- FC-0403 ASSUMPTION_CONTRADICTION
- FC-0404 ASSUMPTION_UNREVIEWABLE

**Mandatory rules:**
- If assumptions matter, they must be declared explicitly.
- Contradictory assumptions fail unless resolved by supersession.

**Must not do:**
- Treat obvious assumptions as implicit.
- Allow indefinite assumptions without review discipline.

---

### Stage 5  Custody Integrity (Provenance Checking)
**Allowed failures:**
- FC-0501 CUSTODY_GAP
- FC-0502 TRANSITION_UNRECORDED
- FC-0503 PROVENANCE_UNVERIFIED

**Mandatory rules:**
- Where custody is required, chain must be continuous across the needed window.
- Governance-relevant transitions must be recorded.
- Integrity continuity must be explainable.

**Must not do:**
- Assume custody continuity.
- Accept unverifiable artifacts as binding evidence.

---

### Stage 6  Mutation & Supersession (Immutability)
**Allowed failures:**
- FC-0601 MUTATION_DETECTED
- FC-0602 SUPERSESSION_MISSING
- FC-0603 SUPERSESSION_CHAIN_BROKEN

**Mandatory rules:**
- No artifact may be silently overwritten.
- Corrections must be explicit via SupersessionRecord.
- Supersession lineage must be complete and acyclic.

**Must not do:**
- Prefer newer artifacts without lineage.
- Permit cleanup edits as non-material.

---

### Stage 7  Narrative Constraint Gate (Anti-Laundering)
**Allowed failures:**
- FC-0701 TRUTH_LOCK_MISSING
- FC-0702 NARRATIVE_UNDERCONSTRAINED
- FC-0703 COMPETING_EXPLANATIONS_UNBOUND
- FC-0704 RETROACTIVE_IRREVERSIBILITY_MARKER

**Mandatory rules:**
- If irreversibility is reached and dependencies are not frozen, fail.
- If artifacts do not force convergence, fail.
- Do not choose among narratives.

**Must not do:**
- Output a best narrative.
- Treat later reporting suggests as a binding input.

---

### Stage 8  Emit
**Allowed failures:**
- FC-0801 FAILURE_BUNDLE_EMIT (output marker only)

**Mandatory rules:**
- Emit failure bundle for any BLOCKER.
- Failure bundle must include:
  - the triggered failure types
  - violated rules
  - unresolved dependencies
  - time context used
  - minimal reproducibility reference (later phase)

---

## Strictness Defaults

- All failures defined as BLOCKER remain BLOCKER.
- No implementation may downgrade severity.
- WARN is allowed only when explicitly described as output marker.

---

## Amendment Rule

This file may be amended only by explicit commit.

No code may introduce:
- new stage behavior
- new failure mappings
- permissive bypasses

without updating this file first.

