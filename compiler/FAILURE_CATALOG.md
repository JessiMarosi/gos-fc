# GOS-FC  Failure Catalog (Canonical)

## Purpose

This document defines the canonical failure types emitted by GOS-FC.

Failures are not exceptions.
They are **deliverable artifacts**.

Every failure type below must be:
- deterministic
- reproducible
- explainable in terms of violated rules and missing dependencies
- non-narrative (no blame, no intent, no causality inference)

If a situation is covered by this catalog, the compiler must emit the specified failure.
If no failure applies, the catalog must be amended explicitly (not patched ad-hoc in code).

---

## Failure Artifact Structure (Conceptual)

Every failure output must contain:

- `failure_id` (unique)
- `failure_type` (one of the canonical types below)
- `pipeline_stage` (from compiler/PIPELINE.md)
- `trigger_summary` (13 sentences, non-narrative)
- `violated_rules` (list of rule IDs)
- `unresolved_dependencies` (list of missing/invalid refs)
- `time_context` (key timestamps used in evaluation)
- `reproducibility` (input bundle hash/manifest id in later phases)

---

## Severity (Non-Scoring)

Severity is non-numeric and non-probabilistic:

- `BLOCKER`  compilation cannot proceed
- `GATE`  compilation may proceed only if explicitly allowed by doctrine (rare)
- `WARN`  emitted only when explicitly allowed; must not enable compilation if dependencies are missing

Default is `BLOCKER`.

---

## Canonical Failures

### FC-0001  INVALID_INPUT_FORMAT
**Severity:** BLOCKER  
**Stage:** 0 (Intake)  
**Trigger:** Artifact cannot be parsed, normalized, or identified.

**Examples:**
- missing artifact_type
- malformed timestamps
- invalid encoding

**Must include:**
- offending artifact id/path (if available)
- parse error details (non-sensitive)

---

### FC-0002  UNKNOWN_ARTIFACT_TYPE
**Severity:** BLOCKER  
**Stage:** 01  
**Trigger:** `artifact_type` not defined in schemas/ARTIFACT_TYPES.md.

---

### FC-0003  DUPLICATE_ID
**Severity:** BLOCKER  
**Stage:** 0  
**Trigger:** Two artifacts share the same `id`.

---

### FC-0101  SCHEMA_VIOLATION
**Severity:** BLOCKER  
**Stage:** 1 (Structural Validation)  
**Trigger:** Artifact violates its conceptual schema requirements.

**Must include:**
- missing required fields
- invalid enum values
- type mismatches

---

### FC-0201  TIME_UNBOUND_ARTIFACT
**Severity:** BLOCKER  
**Stage:** 2 (Time Binding)  
**Trigger:** A required time field is missing/ambiguous/disputed.

**Examples:**
- EvidenceArtifact missing `available_at`
- TransitionRecord missing `effective_at`

---

### FC-0202  TIME_INVALID_EVIDENCE
**Severity:** BLOCKER  
**Stage:** 2  
**Trigger:** Evidence referenced by a DecisionRecord was not available at decision time.

**Rule:**  
If `EvidenceArtifact.available_at` > `DecisionRecord.effective_at`  fail.

---

### FC-0203  POST_HOC_RATIONALE
**Severity:** BLOCKER  
**Stage:** 2  
**Trigger:** Decision rationale references later outcomes, investigations, or hindsight framing.

---

### FC-0204  POST_HOC_ASSUMPTION
**Severity:** BLOCKER  
**Stage:** 2  
**Trigger:** AssumptionDeclaration is created after decision time and used to justify a prior decision.

---

### FC-0205  POST_HOC_EVIDENCE
**Severity:** BLOCKER  
**Stage:** 2  
**Trigger:** EvidenceArtifact introduced to rationalize an already executed decision rather than constrain it.

---

### FC-0301  AUTHORITY_NOT_FOUND
**Severity:** BLOCKER  
**Stage:** 3 (Authority Resolution)  
**Trigger:** A DecisionRecord/Transition/Supersession references no AuthorityGrant, or referenced grants do not exist.

---

### FC-0302  AUTHORITY_SCOPE_MISMATCH
**Severity:** BLOCKER  
**Stage:** 3  
**Trigger:** Referenced AuthorityGrant exists but does not authorize the decision/transition action within scope constraints.

---

### FC-0303  AUTHORITY_AMBIGUOUS
**Severity:** BLOCKER  
**Stage:** 3  
**Trigger:** Multiple plausible authority paths exist and the record does not bind which one applies.

**Note:**  
Ambiguity is not resolved by heuristics.

---

### FC-0304  EMERGENCY_AUTHORITY_UNDECLARED
**Severity:** BLOCKER  
**Stage:** 3  
**Trigger:** Actions treated as emergency occur without an explicit emergency-authorizing AuthorityGrant.

---

### FC-0305  RETROACTIVE_AUTHORITY_GRANT
**Severity:** BLOCKER  
**Stage:** 3  
**Trigger:** AuthorityGrant is issued after the relevant decision/transition and used to legitimize earlier actions.

---

### FC-0401  ASSUMPTION_MISSING
**Severity:** BLOCKER  
**Stage:** 4 (Assumption Closure)  
**Trigger:** Decision depends on assumptions not declared via AssumptionDeclaration.

---

### FC-0402  ASSUMPTION_UNSCOPED
**Severity:** BLOCKER  
**Stage:** 4  
**Trigger:** AssumptionDeclaration lacks enforceable scope or applicability boundaries.

---

### FC-0403  ASSUMPTION_CONTRADICTION
**Severity:** BLOCKER  
**Stage:** 4  
**Trigger:** Two or more in-scope assumptions conflict and conflict is not resolved by explicit supersession.

---

### FC-0404  ASSUMPTION_UNREVIEWABLE
**Severity:** BLOCKER  
**Stage:** 4  
**Trigger:** Assumption has indefinite life without review triggers/requirements.

---

### FC-0501  CUSTODY_GAP
**Severity:** BLOCKER  
**Stage:** 5 (Custody Integrity)  
**Trigger:** Required custody chain is missing links across the needed time window.

---

### FC-0502  TRANSITION_UNRECORDED
**Severity:** BLOCKER  
**Stage:** 5  
**Trigger:** A governance-relevant transition is implied but no TransitionRecord exists.

---

### FC-0503  PROVENANCE_UNVERIFIED
**Severity:** BLOCKER  
**Stage:** 5  
**Trigger:** Integrity or provenance cannot be established where required (missing hashes, missing verification, unexplained changes).

---

### FC-0601  MUTATION_DETECTED
**Severity:** BLOCKER  
**Stage:** 6 (Mutation & Supersession)  
**Trigger:** Artifact content differs across versions without explicit supersession chain.

---

### FC-0602  SUPERSESSION_MISSING
**Severity:** BLOCKER  
**Stage:** 6  
**Trigger:** A correction/replacement occurs without a SupersessionRecord linking old  new.

---

### FC-0603  SUPERSESSION_CHAIN_BROKEN
**Severity:** BLOCKER  
**Stage:** 6  
**Trigger:** Supersession lineage is incomplete, cyclic, or references non-existent artifacts.

---

### FC-0701  TRUTH_LOCK_MISSING
**Severity:** BLOCKER  
**Stage:** 7 (Narrative Constraint Gate)  
**Trigger:** Irreversibility is reached but required artifact classes were not frozen at or before threshold.

---

### FC-0702  NARRATIVE_UNDERCONSTRAINED
**Severity:** BLOCKER  
**Stage:** 7  
**Trigger:** The available artifacts permit multiple incompatible explanations because binding records are missing.

**Important:**  
GOS-FC does not choose narratives; it fails.

---

### FC-0703  COMPETING_EXPLANATIONS_UNBOUND
**Severity:** BLOCKER  
**Stage:** 7  
**Trigger:** Two or more official/operative explanations exist, and the record cannot force convergence using decision-time artifacts.

---

### FC-0704  RETROACTIVE_IRREVERSIBILITY_MARKER
**Severity:** BLOCKER  
**Stage:** 7  
**Trigger:** IrreversibilityMarker is created after the threshold in a way that enables narrative laundering.

---

### FC-0801  FAILURE_BUNDLE_EMIT
**Severity:** WARN (output marker, not a cause)  
**Stage:** 8 (Emit)  
**Trigger:** Any BLOCKER failure occurred; compiler emits a deterministic failure bundle.

---

## Canonical Non-Goals (Failure Catalog)

The compiler must never emit failures that:
- assign blame
- infer intent
- claim causality
- score risk probability
- declare legality/ethics

Failures are structural: missing bindings, missing dependencies, invalid time/authority/custody.

---

## Amendment Rule

This catalog may be amended only by:
- explicit commit changing this file
- a new failure ID
- a deterministic trigger definition
- mapping to pipeline stage(s)

No implementation may introduce an unnamed failure type.

