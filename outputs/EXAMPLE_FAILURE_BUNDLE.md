# GOS-FC  Example Failure Bundle (Fictional)

## Status

EXAMPLE (fictional demonstration)

This file is a fully fictionalized, Oldsmar-inspired validation run.
It is not a claim about any real organization, person, system, vendor, or incident.

Purpose: demonstrate deterministic compilation failure and anti-narrative-laundering behavior.

---

## Scenario Summary (Fictional)

A municipal operations team experiences an anomalous parameter change in a control interface.

Key properties (intentionally adversarial):
- ambiguous authority boundaries
- missing decision-time artifacts
- delayed evidence arrival
- custody gaps
- post-hoc narrative pressure
- irreversibility threshold occurs before evidence is sealed

Evaluation criterion:
- Not success.
- Whether GOS-FC fails loudly and deterministically rather than compiling false clarity.

---

## Input Bundle (Fictional Artifact Inventory)

### Present Artifacts
- DecisionRecord: DR-0001
- EvidenceArtifact: EV-OBS-0001 (operator observation)
- TransitionRecord: TR-ROLE-0001
- AuthorityGrant: AG-OPS-001 (limited operations authority)

### Missing / Incomplete Artifacts (Intentional)
- AssumptionDeclaration: (none provided)
- CustodyRecord: (none provided for EV-OBS-0001)
- EvidenceArtifact: (no sealed snapshot/log bundle at decision time)
- SupersessionRecord: (none; later corrections attempted by overwrite)
- IrreversibilityMarker: IR-0001 is present but recorded after threshold (retroactive)

---

## Compile Attempt

### Stage 0  Intake & Normalization
Result: PASS  
Notes: All artifacts parse and declare known types.

---

### Stage 1  Structural Validation (Schemas)
Result: PASS  
Notes: Required conceptual fields exist in all supplied artifacts.

---

### Stage 2  Time Binding (Decision-Time Truth Conditions)
Result: FAIL (BLOCKER)

Triggered failures:
- FC-0201 TIME_UNBOUND_ARTIFACT
- FC-0202 TIME_INVALID_EVIDENCE (conditional)
- FC-0205 POST_HOC_EVIDENCE (conditional)

Reasoning (non-narrative):
- DR-0001 references a system log export not present as an EvidenceArtifact.
- EV-OBS-0001 exists at decision time, but no additional decision-time evidence bundle exists.
- A later log export (received after the decision) is offered as justification; it cannot satisfy decision-time dependencies.

Unresolved dependencies:
- EvidenceArtifact: EV-LOG-0001 (required, missing)
- EvidenceArtifact: EV-SNAPSHOT-0001 (required, missing)

---

### Stage 3  Authority Resolution (Type Checking)
Result: FAIL (BLOCKER)

Triggered failures:
- FC-0302 AUTHORITY_SCOPE_MISMATCH
- FC-0303 AUTHORITY_AMBIGUOUS

Reasoning (non-narrative):
- DR-0001 action implies a configuration freeze across multiple domains.
- AG-OPS-001 permits limited operational reversion but does not grant cross-domain freeze authority.
- Record does not bind a higher authority grant to resolve scope.

Unresolved dependencies:
- AuthorityGrant: AG-MGMT-0001 (required for freeze scope, missing)
- AuthorityGrant: AG-EMERG-0001 (if decision is emergency-class, missing)

---

### Stage 4  Assumption Closure (Dependency Checking)
Result: FAIL (BLOCKER)

Triggered failures:
- FC-0401 ASSUMPTION_MISSING

Reasoning (non-narrative):
- Decision relies on assumed conditions:
  - operator observation is accurate
  - parameter baseline is correct
  - rollback is safe
- No AssumptionDeclaration artifacts exist, and no explicit no assumptions statement exists.

Unresolved dependencies:
- AssumptionDeclaration: AS-0001..AS-000N (missing)

---

### Stage 5  Custody Integrity (Provenance Checking)
Result: FAIL (BLOCKER)

Triggered failures:
- FC-0501 CUSTODY_GAP
- FC-0503 PROVENANCE_UNVERIFIED

Reasoning (non-narrative):
- EV-OBS-0001 is referenced but has no custody record.
- No integrity reference continuity exists for decision-relevant evidence.

Unresolved dependencies:
- CustodyRecord: CU-0001 (EV-OBS-0001 custody, missing)
- Integrity verification metadata (missing)

---

### Stage 6  Mutation & Supersession (Immutability)
Result: FAIL (BLOCKER)

Triggered failures:
- FC-0601 MUTATION_DETECTED
- FC-0602 SUPERSESSION_MISSING

Reasoning (non-narrative):
- A corrected decision statement appears later without a SupersessionRecord linking DR-0001  DR-0001A.
- Silent overwrite is forbidden.

Unresolved dependencies:
- SupersessionRecord: SR-0001 (missing)
- DecisionRecord: DR-0001A (if correction exists, must be explicit)

---

### Stage 7  Narrative Constraint Gate (Anti-Laundering)
Result: FAIL (BLOCKER)

Triggered failures:
- FC-0701 TRUTH_LOCK_MISSING
- FC-0702 NARRATIVE_UNDERCONSTRAINED
- FC-0704 RETROACTIVE_IRREVERSIBILITY_MARKER

Reasoning (non-narrative):
- Competing explanations become possible because decision-time evidence and assumptions are missing.
- An IrreversibilityMarker is introduced after threshold to lock truth retroactively.
- GOS-FC refuses reconstruction after irreversibility when dependencies were not frozen.

Unresolved dependencies:
- IrreversibilityMarker recorded at/near threshold with valid authority (missing)
- Frozen artifact class set as-of threshold (missing)

---

## Final Output

### Compilation Result
**FAILURE_BUNDLE_EMIT**

### Canonical Failure Summary
Accountability cannot be compiled because:

- evidence was not bound at decision time
- authority scope cannot be proven
- assumptions were not declared
- custody/provenance continuity is absent
- corrections were attempted without supersession
- irreversibility was reached without truth-lock prerequisites

GOS-FC refuses narrative convergence.
It emits missing dependencies and halts.

---

## Emitted Failure Artifact (Conceptual)

- `failure_type`: FC-0702 NARRATIVE_UNDERCONSTRAINED
- `severity`: BLOCKER
- `pipeline_stage`: 7
- `violated_rules`:
  - ER-G-001, ER-G-002, ER-G-003
- `unresolved_dependencies`:
  - EV-LOG-0001, EV-SNAPSHOT-0001
  - AG-MGMT-0001
  - AS-0001..AS-000N
  - CU-0001
  - SR-0001
  - IR-0001 (time-valid, authority-valid)

---

## What This Demonstrates

- GOS-FC does not pick a story.
- It does not smooth ambiguity.
- It forces truth conditions to be satisfied **before** irreversibility.
- If they are not, it fails loudly and deterministically.

That is the point.

