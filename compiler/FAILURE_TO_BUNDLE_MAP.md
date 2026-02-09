# GOS-FC Failure  Bundle Map (Deterministic Routing Table)

This document maps **canonical GOS-FC failure codes** to:

- the bundle section that would have contained the required artifact(s),
- the upstream contract origin responsible for producing them (or feeding them),
- the pipeline stage where the failure must be surfaced.

This is a **routing table**, not a narrative.
GOS-FC does not infer missing inputs.

---

## Canonical Bundle Sections (Reference)

- Manifest
- Metadata
- Decision Records
- Authority and Custody
- Assumptions
- Evidence
- Transition Records
- Supersession Records
- Context (optional)
- Integrity (optional)
- Failure (required on failure)
- Checksums

---

## Upstream Contract Origins (Reference)

- DADS  decisions / rationale / approvals / references
- ChainLogic-DCCS  authority grants / custody / transitions
- Cairn  assumptions / reasoning memory
- AxiomForge (optional)  obligations discipline
- Veridion (optional)  integrity state declarations
- DeepSee-Live (optional)  contextual observability
- StrataVault (optional)  governed boundaries + access provenance

---

## Routing Rules (Deterministic)

1. Each failure code maps to one **primary bundle section** where the missing/invalid dependency belongs.
2. All failures are emitted in the **Failure** section, but this map identifies the *target* section that would have satisfied the dependency.
3. Upstream origin indicates which system is expected to produce the dependency **or** provide the authoritative feed that enables it.
4. This map is **normative for routing**: if a code exists in `compiler/FAILURE_CATALOG.md`, its routing must be explicit here (or explicitly marked N/A with justification).

---

## Routing Table (Canonical)

### Stage 0  Intake

#### FC-0001  INVALID_INPUT_FORMAT
- **Primary Bundle Section:** Manifest (run context) + Failure
- **Upstream Origin:** Any (source artifact producers)
- **Deterministic Output:** Failure (parse/normalization diagnostics)

#### FC-0002  UNKNOWN_ARTIFACT_TYPE
- **Primary Bundle Section:** Failure
- **Upstream Origin:** Any (source artifact producers)
- **Deterministic Output:** Failure (unknown type; cannot route)

#### FC-0003  DUPLICATE_ID
- **Primary Bundle Section:** Failure
- **Upstream Origin:** Any (source artifact producers)
- **Deterministic Output:** Failure (identifier collision)

---

### Stage 1  Structural Validation

#### FC-0101  SCHEMA_VIOLATION
- **Primary Bundle Section:** Failure
- **Upstream Origin:** Any (artifact producers); schemas are defined in `schemas/`
- **Deterministic Output:** Failure (missing/invalid fields)

---

### Stage 2  Time Binding

#### FC-0201  TIME_UNBOUND_ARTIFACT
- **Primary Bundle Section:** (Depends on artifact class) + Failure
  - Decision Records / Authority and Custody / Assumptions / Evidence / Transition Records / Supersession Records
- **Upstream Origin:** Any (the producer of the time-bound artifact)
- **Deterministic Output:** Failure (missing/ambiguous time binding)

#### FC-0202  TIME_INVALID_EVIDENCE
- **Primary Bundle Section:** Evidence
- **Upstream Origin:** Evidence-producing systems; DADS provides references
- **Deterministic Output:** Failure (evidence not available at decision time)

#### FC-0203  POST_HOC_RATIONALE
- **Primary Bundle Section:** Decision Records
- **Upstream Origin:** DADS
- **Deterministic Output:** Failure (rationale contaminated by hindsight)

#### FC-0204  POST_HOC_ASSUMPTION
- **Primary Bundle Section:** Assumptions
- **Upstream Origin:** Cairn
- **Deterministic Output:** Failure (assumption declared after decision time)

#### FC-0205  POST_HOC_EVIDENCE
- **Primary Bundle Section:** Evidence
- **Upstream Origin:** Evidence-producing systems
- **Deterministic Output:** Failure (post-outcome evidence misused as constraint)

---

### Stage 3  Authority Resolution

#### FC-0301  AUTHORITY_NOT_FOUND
- **Primary Bundle Section:** Authority and Custody
- **Upstream Origin:** ChainLogic-DCCS (or governance registry feeding it)
- **Deterministic Output:** Failure (missing AuthorityGrant dependency)

#### FC-0302  AUTHORITY_SCOPE_MISMATCH
- **Primary Bundle Section:** Authority and Custody
- **Upstream Origin:** ChainLogic-DCCS
- **Deterministic Output:** Failure (grant exists; scope does not authorize action)

#### FC-0303  AUTHORITY_AMBIGUOUS
- **Primary Bundle Section:** Authority and Custody
- **Upstream Origin:** ChainLogic-DCCS
- **Deterministic Output:** Failure (multiple plausible authority paths; none bound)

#### FC-0304  EMERGENCY_AUTHORITY_UNDECLARED
- **Primary Bundle Section:** Authority and Custody
- **Upstream Origin:** ChainLogic-DCCS
- **Deterministic Output:** Failure (emergency class actions require explicit grant)

#### FC-0305  RETROACTIVE_AUTHORITY_GRANT
- **Primary Bundle Section:** Authority and Custody
- **Upstream Origin:** ChainLogic-DCCS
- **Deterministic Output:** Failure (authority granted after-the-fact)

---

### Stage 4  Assumption Closure

#### FC-0401  ASSUMPTION_MISSING
- **Primary Bundle Section:** Assumptions
- **Upstream Origin:** Cairn
- **Deterministic Output:** Failure (missing assumption inventory)

#### FC-0402  ASSUMPTION_UNSCOPED
- **Primary Bundle Section:** Assumptions
- **Upstream Origin:** Cairn
- **Deterministic Output:** Failure (scope not enforceable)

#### FC-0403  ASSUMPTION_CONTRADICTION
- **Primary Bundle Section:** Assumptions + Supersession Records (if resolved by supersession)
- **Upstream Origin:** Cairn (assumptions) + any producer of supersession records
- **Deterministic Output:** Failure (conflict without explicit resolution)

#### FC-0404  ASSUMPTION_UNREVIEWABLE
- **Primary Bundle Section:** Assumptions
- **Upstream Origin:** Cairn
- **Deterministic Output:** Failure (indefinite assumption without review/expiration)

---

### Stage 5  Custody Integrity

#### FC-0501  CUSTODY_GAP
- **Primary Bundle Section:** Authority and Custody
- **Upstream Origin:** ChainLogic-DCCS
- **Deterministic Output:** Failure (custody chain incomplete)

#### FC-0502  TRANSITION_UNRECORDED
- **Primary Bundle Section:** Transition Records
- **Upstream Origin:** ChainLogic-DCCS (and operational transition feeders)
- **Deterministic Output:** Failure (missing transition dependency)

#### FC-0503  PROVENANCE_UNVERIFIED
- **Primary Bundle Section:** Integrity (optional) + Authority and Custody + Failure
- **Upstream Origin:** ChainLogic-DCCS (custody/provenance) + Veridion (optional integrity states)
- **Deterministic Output:** Failure (cannot establish provenance/integrity where required)

---

### Stage 6  Mutation & Supersession

#### FC-0601  MUTATION_DETECTED
- **Primary Bundle Section:** Supersession Records
- **Upstream Origin:** Any (artifact producers) + custody systems must surface
- **Deterministic Output:** Failure (mutation without explicit supersession)

#### FC-0602  SUPERSESSION_MISSING
- **Primary Bundle Section:** Supersession Records
- **Upstream Origin:** Any (artifact producers responsible for correction discipline)
- **Deterministic Output:** Failure (correction without SupersessionRecord)

#### FC-0603  SUPERSESSION_CHAIN_BROKEN
- **Primary Bundle Section:** Supersession Records
- **Upstream Origin:** Any (artifact producers)
- **Deterministic Output:** Failure (invalid lineage)

---

### Stage 7  Narrative Constraint Gate

#### FC-0701  TRUTH_LOCK_MISSING
- **Primary Bundle Section:** Failure (and optionally Integrity)
- **Upstream Origin:** Emergent condition (not a single upstream system)
- **Deterministic Output:** Failure (irreversibility reached without required freezes)

#### FC-0702  NARRATIVE_UNDERCONSTRAINED
- **Primary Bundle Section:** Failure
- **Upstream Origin:** Emergent condition (record is insufficient across multiple domains)
- **Deterministic Output:** Failure (multiple incompatible explanations remain possible)

#### FC-0703  COMPETING_EXPLANATIONS_UNBOUND
- **Primary Bundle Section:** Failure
- **Upstream Origin:** Emergent condition (competing explanations exist; bindings missing)
- **Deterministic Output:** Failure (cannot force convergence without decision-time artifacts)

#### FC-0704  RETROACTIVE_IRREVERSIBILITY_MARKER
- **Primary Bundle Section:** Failure
- **Upstream Origin:** Any producer attempting retroactive locking; governance must bind at threshold
- **Deterministic Output:** Failure (retroactive truth-lock attempt)

---

### Stage 8  Emit

#### FC-0801  FAILURE_BUNDLE_EMIT
- **Primary Bundle Section:** Failure
- **Upstream Origin:** Bundle generation process
- **Deterministic Output:** Failure (output marker; not a cause)

---

## Canonical Rule

A failure code is not a finding.
It is a deterministic statement of which **required dependency** is missing, ambiguous, time-invalid, or non-attributable.

GOS-FC outputs failure to prevent institutions from substituting legibility for truth.
