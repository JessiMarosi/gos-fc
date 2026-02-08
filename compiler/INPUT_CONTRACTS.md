# GOS-FC Input Contracts

This document defines the **explicit input contracts** between
the Governance Operating Substrate Forensic Compiler (GOS-FC)
and upstream systems in the GOS ecosystem.

GOS-FC does not coordinate, orchestrate, or control upstream systems.
It **only ingests their outputs** and enforces deterministic compilation rules.

If required inputs are missing, ambiguous, or unbound,
GOS-FC fails deterministically.

---

## Contract Principles

1. GOS-FC **never infers** missing information.
2. All inputs are treated as **claims**, not truths.
3. Authority, assumptions, and evidence must be **explicitly declared**.
4. Silence is not neutral  silence is a missing dependency.
5. Inputs are validated structurally, temporally, and semantically.

---

## Upstream Systems and Expected Inputs

### 1. DADS  Decision Accountability Documentation System

**Role:** Primary source of decision intent and rationale.

**Required Artifacts:**
- DecisionRecord
- Decision timestamp
- Declared decision scope
- Named approver(s)

**GOS-FC Refuses To Infer:**
- Why a decision was made if not stated
- Whether a decision was justified
- Whether a decision was correct

**Failure Modes:**
- Missing DecisionRecord  Compilation failure
- Ambiguous approver  Authority resolution failure

---

### 2. ChainLogic-DCCS  Digital Chain of Custody System

**Role:** Authority and custody integrity across time and systems.

**Required Artifacts:**
- AuthorityGrant
- CustodyRecord
- TransitionRecord

**GOS-FC Refuses To Infer:**
- Authority continuity
- Implicit delegation
- Informal custody transfer

**Failure Modes:**
- Broken custody chain  Integrity failure
- Authority scope mismatch  Type failure

---

### 3. Cairn  Assumption-Tracked Reasoning Ledger

**Role:** Preservation of assumptions treated as true at decision time.

**Required Artifacts:**
- AssumptionDeclaration
- Assumption timestamp
- Supporting references (if any)

**GOS-FC Refuses To Infer:**
- Which assumptions were active
- Whether assumptions were reasonable
- Whether assumptions remained valid over time

**Failure Modes:**
- Undeclared assumption  Missing dependency failure
- Expired or unbounded assumption  Temporal validity failure

---

### 4. Axiom Forge (Optional)

**Role:** Formal obligation discipline for high-stakes claims.

**Required Artifacts (when used):**
- Obligation declaration
- Formal claim structure
- Dependency references

**GOS-FC Refuses To Infer:**
- Logical equivalence
- Claim completeness
- Mathematical correctness

**Failure Modes:**
- Unbound obligation  Obligation resolution failure

---

### 5. Veridion (Optional)

**Role:** Integrity state validation of systems and artifacts.

**Required Artifacts:**
- Declared integrity state
- Integrity transition markers

**GOS-FC Refuses To Infer:**
- Actual system state
- Undeclared drift
- Implicit validity

**Failure Modes:**
- Integrity mismatch  State validation failure

---

### 6. DeepSee-Live (Optional)

**Role:** Contextual observability and event correlation.

**Required Artifacts:**
- Time-ordered context events
- Correlation identifiers

**GOS-FC Refuses To Infer:**
- Causality
- Intent
- Narrative meaning

**Failure Modes:**
- Context-only evidence  Non-authoritative input

---

### 7. StrataVault (Optional)

**Role:** Data boundary and access provenance enforcement.

**Required Artifacts:**
- Access boundary declarations
- Evidence access records

**GOS-FC Refuses To Infer:**
- Data legitimacy
- Access justification
- Boundary correctness

**Failure Modes:**
- Boundary violation  Trust surface failure

---

## Contract Enforcement

All inputs are validated during the **ingestion phase**
as defined in:

- compiler/PIPELINE.md
- compiler/ENFORCEMENT_RULES.md
- compiler/FAILURE_CATALOG.md

Violations halt compilation.

---

## Canonical Rule

> GOS-FC compiles accountability only from what is explicitly provided.
>  
> Anything not declared does not exist to the compiler.

This is non-negotiable.
