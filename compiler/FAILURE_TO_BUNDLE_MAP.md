# GOS-FC Failure  Bundle Map (Deterministic Routing Table)

This document maps canonical GOS-FC failure conditions to:

- the bundle section that would have contained the required artifact(s),
- the upstream contract area responsible for producing them, and
- the compilation stage where the failure must be surfaced.

This is a **routing table**, not a narrative.
GOS-FC does not infer missing inputs.

---

## Canonical Bundle Sections (Reference)

- Manifest
- Metadata
- Decisions
- Authority & Custody
- Assumptions
- Evidence
- Transitions
- Supersessions
- Context (optional)
- Integrity (optional)
- Failures (present on failure)
- Checksums

---

## Upstream Contract Origins (Reference)

- DADS (Decisions / rationale / approvals)
- ChainLogic-DCCS (Authority transitions / custody)
- Cairn (Assumptions / reasoning memory)
- Axiom Forge (optional: obligations)
- Veridion (optional: integrity states)
- DeepSee-Live (optional: context)
- StrataVault (optional: boundary + access provenance)

---

## Routing Table

> NOTE:
> Failure IDs below are canonical *classes* intended to align with FAILURE_CATALOG.md.
> Exact codes may be refined, but the mapping categories must remain stable.

---

### F-DEC-001  Missing DecisionRecord

- **Trigger:** A materially consequential action exists without a DecisionRecord.
- **Pipeline Stage:** Decision ingestion / decision graph construction
- **Expected Bundle Section:** Decisions
- **Upstream Origin:** DADS
- **Deterministic Output:** Failures (missing dependency list)
- **Primary Diagnostic:** Decision provenance gap

---

### F-AUTH-001  Missing AuthorityGrant

- **Trigger:** Decision references an approver or role without a time-bounded authority grant.
- **Pipeline Stage:** Authority resolution
- **Expected Bundle Section:** Authority & Custody
- **Upstream Origin:** ChainLogic-DCCS (or governance registry feeding it)
- **Deterministic Output:** Failures (unresolved authority dependency)

---

### F-AUTH-002  Ambiguous Authority Scope

- **Trigger:** Multiple authority grants plausibly apply, or scope cannot be resolved deterministically.
- **Pipeline Stage:** Authority type checking
- **Expected Bundle Section:** Authority & Custody
- **Upstream Origin:** ChainLogic-DCCS
- **Deterministic Output:** Failures (type failure)

---

### F-ASM-001  Missing AssumptionDeclaration

- **Trigger:** A decision requires an assumption (safety, equivalence, reversibility, etc.) that was never declared.
- **Pipeline Stage:** Assumption binding
- **Expected Bundle Section:** Assumptions
- **Upstream Origin:** Cairn
- **Deterministic Output:** Failures (missing assumption inventory)

---

### F-ASM-002  Unbounded / Non-Time-Bound Assumption

- **Trigger:** Assumption lacks a declaration timestamp, scope, or expiration semantics.
- **Pipeline Stage:** Assumption temporal validation
- **Expected Bundle Section:** Assumptions
- **Upstream Origin:** Cairn
- **Deterministic Output:** Failures (temporal validity failure)

---

### F-EVD-001  Missing EvidenceArtifact

- **Trigger:** Decision asserts evidence exists, but no evidence artifact is present at decision-time.
- **Pipeline Stage:** Evidence dependency resolution
- **Expected Bundle Section:** Evidence
- **Upstream Origin:** DADS (references) + evidence system(s)
- **Deterministic Output:** Failures (unresolved evidence dependency)

---

### F-EVD-002  Post-Outcome Evidence Used as Justification

- **Trigger:** Evidence timestamp occurs after the decision but is treated as supporting decision-time epistemic state.
- **Pipeline Stage:** Evidence time-binding
- **Expected Bundle Section:** Evidence
- **Upstream Origin:** Any (evidence-producing systems)
- **Deterministic Output:** Failures (time-binding violation)

---

### F-CUS-001  Broken Custody Chain

- **Trigger:** Artifact custody cannot be traced across time, systems, or custodians.
- **Pipeline Stage:** Custody verification
- **Expected Bundle Section:** Authority & Custody
- **Upstream Origin:** ChainLogic-DCCS
- **Deterministic Output:** Failures (custody integrity failure)

---

### F-TRN-001  Missing TransitionRecord

- **Trigger:** Role/authority/system state transition occurred without explicit TransitionRecord.
- **Pipeline Stage:** Transition normalization
- **Expected Bundle Section:** Transitions
- **Upstream Origin:** ChainLogic-DCCS (and operational transition feeders)
- **Deterministic Output:** Failures (missing transition dependency)

---

### F-SUP-001  Silent Mutation Detected (No SupersessionRecord)

- **Trigger:** Artifact appears altered without a supersession record.
- **Pipeline Stage:** Mutation / supersession enforcement
- **Expected Bundle Section:** Supersessions
- **Upstream Origin:** Any (but custody systems must surface)
- **Deterministic Output:** Failures (mutation violation)

---

### F-IRR-001  Irreversibility Marker Triggered

- **Trigger:** Reconstruction requires fabrication (authority/assumption/intent must be invented).
- **Pipeline Stage:** Terminal validity check
- **Expected Bundle Section:** Failures (and optionally Integrity)
- **Upstream Origin:** Emergent condition (not a single system)
- **Deterministic Output:** Failures (irreversibility declaration)

---

### F-CHK-001  Checksum / Integrity Marker Mismatch

- **Trigger:** Bundle contents do not match integrity markers.
- **Pipeline Stage:** Bundle sealing / verification
- **Expected Bundle Section:** Checksums
- **Upstream Origin:** Bundle generation process
- **Deterministic Output:** Failures (bundle integrity failure)

---

## Canonical Rule

A failure code is not a finding.
It is a deterministic statement of which **required dependency** is missing, ambiguous, time-invalid, or non-attributable.

GOS-FC outputs failure to prevent institutions from substituting legibility for truth.
