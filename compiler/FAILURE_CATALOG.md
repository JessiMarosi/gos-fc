# GOS-FC  Failure Catalog (Canonical)

## Purpose

This document defines the **canonical failure types** emitted by GOS-FC.

Failures are not exceptions.
They are **first-class, deliverable forensic artifacts**.

Every failure defined here is:
- deterministic (same inputs  same failure)
- reproducible (replayable from the input bundle)
- explainable in structural terms (rules violated, dependencies missing)
- non-narrative (no blame, no intent, no causality inference)

If a situation is covered by this catalog, the compiler **must** emit the specified failure.
If no failure applies, the catalog must be amended explicitly  not patched ad hoc in implementation.

---

## How to Read This Catalog

Each failure entry answers five questions:

1. **What structural condition is violated?**
2. **At which pipeline stage is the violation detected?**
3. **Why compilation cannot safely proceed past this point**
4. **What explicit dependencies are missing or invalid**
5. **Why inference or reconstruction is forbidden**

Failures do **not** explain what happened.
They explain **why accountable truth cannot be compiled**.

---

## Failure Artifact Structure (Conceptual)

Every emitted failure artifact must include:

- `failure_id`  unique identifier for this failure instance
- `failure_type`  one of the canonical types below
- `pipeline_stage`  stage identifier from `compiler/PIPELINE.md`
- `trigger_summary`  concise, non-narrative description (13 sentences)
- `violated_rules`  list of rule identifiers
- `unresolved_dependencies`  missing, invalid, or ambiguous references
- `time_context`  timestamps used for evaluation
- `reproducibility`  input bundle or manifest reference

---

## Severity (Non-Scoring)

Severity is **non-numeric** and **non-probabilistic**.

- `BLOCKER`  compilation must halt
- `GATE`  compilation may proceed only if explicitly allowed by doctrine (rare)
- `WARN`  informational marker; must never enable compilation if dependencies are missing

Default severity is `BLOCKER`.

---

## Canonical Failures

### FC-0001  INVALID_INPUT_FORMAT
**Severity:** BLOCKER  
**Stage:** 0 (Intake)  
**Trigger:** Artifact cannot be parsed, normalized, or identified.

**Examples:**
- missing `artifact_type`
- malformed timestamps
- invalid encoding

**Must include:**
- offending artifact identifier or path (if available)
- non-sensitive parse error details

---

### FC-0002  UNKNOWN_ARTIFACT_TYPE
**Severity:** BLOCKER  
**Stage:** 0  
**Trigger:** `artifact_type` is not defined in `schemas/ARTIFACT_TYPES.md`.

---

### FC-0003  DUPLICATE_ID
**Severity:** BLOCKER  
**Stage:** 0  
**Trigger:** Two artifacts share the same identifier.

---

### FC-0101  SCHEMA_VIOLATION
**Severity:** BLOCKER  
**Stage:** 1 (Structural Validation)  
**Trigger:** Artifact violates its conceptual schema.

**Must include:**
- missing required fields
- invalid enum values
- type mismatches

---

### FC-0201  TIME_UNBOUND_ARTIFACT
**Severity:** BLOCKER  
**Stage:** 2 (Time Binding)  
**Trigger:** Required time field is missing, ambiguous, or disputed.

---

### FC-0202  TIME_INVALID_EVIDENCE
**Severity:** BLOCKER  
**Stage:** 2  
**Trigger:** Evidence referenced by a decision was not available at decision time.

**Rule:**  
If `EvidenceArtifact.available_at` > `DecisionRecord.effective_at`  fail.

---

### FC-0203  POST_HOC_RATIONALE
**Severity:** BLOCKER  
**Stage:** 2  
**Trigger:** Decision rationale references outcomes, investigations, or hindsight unavailable at decision time.

---

### FC-0204  POST_HOC_ASSUMPTION
**Severity:** BLOCKER  
**Stage:** 2  
**Trigger:** Assumption is declared after decision time and used retroactively.

---

### FC-0205  POST_HOC_EVIDENCE
**Severity:** BLOCKER  
**Stage:** 2  
**Trigger:** Evidence is introduced to justify an already executed decision rather than constrain it.

---

### FC-0301  AUTHORITY_NOT_FOUND
**Severity:** BLOCKER  
**Stage:** 3 (Authority Resolution)  
**Trigger:** No AuthorityGrant exists for a referenced decision or transition.

---

### FC-0302  AUTHORITY_SCOPE_MISMATCH
**Severity:** BLOCKER  
**Stage:** 3  
**Trigger:** Referenced AuthorityGrant does not authorize the action within declared scope.

---

### FC-0303  AUTHORITY_AMBIGUOUS
**Severity:** BLOCKER  
**Stage:** 3  
**Trigger:** Multiple plausible authority paths exist and none are explicitly bound.

**Note:**  
Ambiguity is not resolved heuristically.

---

### FC-0304  EMERGENCY_AUTHORITY_UNDECLARED
**Severity:** BLOCKER  
**Stage:** 3  
**Trigger:** Emergency actions occur without explicit emergency authority.

---

### FC-0305  RETROACTIVE_AUTHORITY_GRANT
**Severity:** BLOCKER  
**Stage:** 3  
**Trigger:** Authority is granted after the fact to legitimize earlier actions.

---

### FC-0401  ASSUMPTION_MISSING
**Severity:** BLOCKER  
**Stage:** 4 (Assumption Closure)  
**Trigger:** Decision depends on undeclared assumptions.

---

### FC-0402  ASSUMPTION_UNSCOPED
**Severity:** BLOCKER  
**Stage:** 4  
**Trigger:** Assumption lacks enforceable scope.

---

### FC-0403  ASSUMPTION_CONTRADICTION
**Severity:** BLOCKER  
**Stage:** 4  
**Trigger:** In-scope assumptions conflict without explicit resolution.

---

### FC-0404  ASSUMPTION_UNREVIEWABLE
**Severity:** BLOCKER  
**Stage:** 4  
**Trigger:** Assumption persists indefinitely without review or expiration.

---

### FC-0501  CUSTODY_GAP
**Severity:** BLOCKER  
**Stage:** 5 (Custody Integrity)  
**Trigger:** Required custody chain is incomplete.

---

### FC-0502  TRANSITION_UNRECORDED
**Severity:** BLOCKER  
**Stage:** 5  
**Trigger:** Governance-relevant transition lacks a TransitionRecord.

---

### FC-0503  PROVENANCE_UNVERIFIED
**Severity:** BLOCKER  
**Stage:** 5  
**Trigger:** Integrity or provenance cannot be established.

---

### FC-0601  MUTATION_DETECTED
**Severity:** BLOCKER  
**Stage:** 6 (Mutation & Supersession)  
**Trigger:** Artifact content changes without explicit supersession.

---

### FC-0602  SUPERSESSION_MISSING
**Severity:** BLOCKER  
**Stage:** 6  
**Trigger:** Correction occurs without a SupersessionRecord.

---

### FC-0603  SUPERSESSION_CHAIN_BROKEN
**Severity:** BLOCKER  
**Stage:** 6  
**Trigger:** Supersession lineage is incomplete, cyclic, or invalid.

---

### FC-0701  TRUTH_LOCK_MISSING
**Severity:** BLOCKER  
**Stage:** 7 (Narrative Constraint Gate)  
**Trigger:** Irreversibility is reached before required artifacts are frozen.

---

### FC-0702  NARRATIVE_UNDERCONSTRAINED
**Severity:** BLOCKER  
**Stage:** 7  
**Trigger:** Available artifacts permit multiple incompatible explanations.

**Important:**  
GOS-FC does not choose narratives; it fails.

---

### FC-0703  COMPETING_EXPLANATIONS_UNBOUND
**Severity:** BLOCKER  
**Stage:** 7  
**Trigger:** Multiple official explanations exist without binding records.

---

### FC-0704  RETROACTIVE_IRREVERSIBILITY_MARKER
**Severity:** BLOCKER  
**Stage:** 7  
**Trigger:** IrreversibilityMarker is introduced after threshold to enable reconstruction.

---

### FC-0801  FAILURE_BUNDLE_EMIT
**Severity:** WARN (output marker, not a cause)  
**Stage:** 8 (Emit)  
**Trigger:** One or more BLOCKER failures occurred; compiler emits failure bundle.

---

## Canonical Non-Goals (Failure Semantics)

The compiler must never emit failures that:
- assign blame
- infer intent
- claim causality
- score risk
- declare legality or ethics

Failures describe **structural impossibility**, not wrongdoing.

---

## Amendment Rule

This catalog may be amended only by:
- explicit commit modifying this file
- introduction of a new failure identifier
- deterministic trigger definition
- explicit mapping to pipeline stage(s)

No implementation may introduce unnamed or implicit failure types.
