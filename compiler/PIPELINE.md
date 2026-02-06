# GOS-FC  Compiler Pipeline (Deterministic)

## Purpose

This document defines the deterministic compilation pipeline for GOS-FC.

GOS-FC compiles governance/accountability inputs into replayable forensic artifacts **or fails deterministically** with explicit diagnostics.

This pipeline is not an implementation.
It is the authoritative specification of compilation stages, required inputs, and failure semantics.

---

## Core Principle

GOS-FC never best-efforts accountability.

If required inputs are missing, ambiguous, time-invalid, or non-superseded:
- compilation halts
- failure is produced as an artifact

Failure is a deliverable.

---

## Inputs (Conceptual)

GOS-FC accepts declared artifacts such as:

- Decision Records
- Authority Grants and Authority Scopes
- Assumption Declarations
- Evidence Artifacts (time-bound, attributable)
- Custody / Transition Records
- Supersession Chains (no silent mutation)

All inputs must be explicit.
Silence is an unresolved dependency.

---

## Outputs

### On Success
- Replayable decision timeline
- Authority resolution graph
- Assumption dependency graph
- Evidence binding map (what was available when)
- Hash-sealed forensic bundle (fictional in this repo)
- Deterministic verification manifest (fictional in this repo)

### On Failure
- Failure report (deterministic)
- Missing dependency list
- Ambiguity diagnostics (authority/type failures)
- Time-invalid evidence diagnostics
- Mutation/supersession violations
- Minimal reproducible failure bundle (fictional)

---

## Pipeline Stages

### Stage 0  Intake & Normalization (Non-Interpretive)
**Goal:** Load declared artifacts into a canonical internal representation without interpretation.

**Deterministic rules:**
- Unknown artifact types are rejected unless schema-defined.
- Duplicate identifiers are rejected.
- Missing required fields fail immediately.

**Failure artifacts:**
- INVALID_INPUT_FORMAT
- UNKNOWN_ARTIFACT_TYPE
- DUPLICATE_ID

---

### Stage 1  Structural Validation (Schemas)
**Goal:** Validate shape, required fields, and allowable values.

**Deterministic rules:**
- Schema violations halt compilation.
- Close enough is forbidden.

**Failure artifacts:**
- SCHEMA_VIOLATION
- REQUIRED_FIELD_MISSING
- VALUE_OUT_OF_RANGE

---

### Stage 2  Time Binding (Decision-Time Truth Conditions)
**Goal:** Enforce that evidence and declarations existed at the relevant decision times.

**Deterministic rules:**
- Evidence introduced after decision time cannot satisfy a dependency.
- Post-hoc rationale cannot satisfy decision rationale requirements.
- Time-unknown artifacts fail if time is required.

**Failure artifacts:**
- TIME_INVALID_EVIDENCE
- POST_HOC_RATIONALE
- TIME_UNBOUND_ARTIFACT

---

### Stage 3  Authority Resolution (Type-Checking)
**Goal:** Resolve every decision to an explicit authority grant with valid scope at that time.

**Deterministic rules:**
- No implicit authority.
- No role-proximity inference.
- No emergency without an emergency declaration or grant.

**Failure artifacts:**
- AUTHORITY_NOT_FOUND
- AUTHORITY_SCOPE_MISMATCH
- AUTHORITY_AMBIGUOUS
- EMERGENCY_AUTHORITY_UNDECLARED

---

### Stage 4  Assumption Closure (Dependency Checking)
**Goal:** Ensure that all assumptions relied upon are explicitly declared and bound.

**Deterministic rules:**
- Undeclared assumptions halt compilation.
- Assumptions must be reviewable and time-scoped.

**Failure artifacts:**
- ASSUMPTION_MISSING
- ASSUMPTION_UNSCOPED
- ASSUMPTION_CONTRADICTION

---

### Stage 5  Custody Integrity (Provenance Checking)
**Goal:** Ensure evidence and records have traceable custody and transitions.

**Deterministic rules:**
- If custody is required and absent, compilation fails.
- If transitions are implied but not recorded, compilation fails.

**Failure artifacts:**
- CUSTODY_GAP
- TRANSITION_UNRECORDED
- PROVENANCE_UNVERIFIED

---

### Stage 6  Mutation & Supersession Enforcement (Immutability)
**Goal:** Enforce that corrections occur only by explicit supersession.

**Deterministic rules:**
- Silent overwrite is forbidden.
- Any correction must reference the prior record.
- Supersession chains must be complete.

**Failure artifacts:**
- MUTATION_DETECTED
- SUPERSESSION_MISSING
- SUPERSESSION_CHAIN_BROKEN

---

### Stage 7  Narrative Constraint Check (Anti-Laundering Gate)
**Goal:** Ensure the compiled artifact set does not permit multiple incompatible explanations to coexist due to missing bindings.

**Deterministic rules:**
- GOS-FC does not choose narratives.
- If records cannot force convergence, compilation fails.
- Disputed later becomes a failure signal unless contemporaneous artifacts resolve it.

**Failure artifacts:**
- NARRATIVE_UNDERCONSTRAINED
- COMPETING_EXPLANATIONS_UNBOUND
- TRUTH_LOCK_MISSING

---

### Stage 8  Bundle Emit (Success or Failure)
**Goal:** Emit a replayable bundle if compiled; otherwise emit a failure bundle.

**Deterministic rules:**
- Outputs are reproducible from the same inputs.
- Failure bundles include only what is required to reproduce the failure.

**Output artifacts:**
- SUCCESS_BUNDLE
- FAILURE_BUNDLE

---

## Determinism Guarantees

GOS-FC must guarantee:

- identical inputs produce identical outputs
- failures are stable and reproducible
- diagnostics enumerate missing dependencies explicitly
- no inference, guessing, or probability enters compilation

---

## Scenario Alignment (Validation Target)

This pipeline must surface deterministic failure for the fictional scenario where:

- authority is blurred
- assumptions are missing
- evidence is delayed/partial
- custody is incomplete
- narratives diverge after the irreversibility threshold

If GOS-FC can be made to compile anyway, the system is invalid.

