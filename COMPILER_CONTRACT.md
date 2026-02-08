# COMPILER_CONTRACT  Closed Compiler Surface (Baseline Freeze)

Status: **CLOSED SURFACE (Baseline Freeze)**
Repo: **gos-fc (public)**
Scope: **compiler surface only** (inputs  deterministic pipeline  failure semantics  output bundle)

## 1) Purpose

This document freezes the GOS-FC compiler surface at a compiler-complete baseline.

GOS-FC exists to:
- compile accountability inputs into audit-grade, replayable artifacts **or**
- fail deterministically with explicit missing-dependency diagnostics.

GOS-FC does **not** judge outcomes, detect attacks, assign blame, infer intent, or automate enforcement.

Anchor doctrine: **It refuses to let institutions pretend that legibility is the same as truth.**

## 2) What Compiler Surface Means

The compiler surface is the minimum stable contract required to support:
- integration with upstream systems (DADS, ChainLogic-DCCS, Cairn, etc.),
- deterministic compilation behavior (same inputs  same output/failure),
- canonical failures (explicit, stable semantics),
- a canonical output bundle (stable structure and meaning).

The compiler surface is **documentation-defined** in this repository (no executable compiler code is required for the surface to be binding).

## 3) Frozen Surface Inventory (Normative)

The following files collectively define the compiler surface and are treated as **normative** (contract-binding):

### 3.1 Inputs (Integration Interface)
- `compiler/INPUT_CONTRACTS.md`
- `schemas/*` (conceptual schemas defining artifact types and required fields)

### 3.2 Pipeline + Enforcement (Deterministic Stage Semantics)
- `compiler/PIPELINE.md`
- `compiler/ENFORCEMENT_RULES.md`

### 3.3 Failures (Canonical Failure Semantics)
- `compiler/FAILURE_CATALOG.md`

### 3.4 Output Bundle Contract (Single-Document Spec)
- `outputs/BUNDLE_FORMAT.md`
- `outputs/EXAMPLE_FAILURE_BUNDLE.md` (fictional validation example)

### 3.5 Deterministic Join (Failure  Bundle Routing)
- `compiler/FAILURE_TO_BUNDLE_MAP.md`

### 3.6 Boundary Doctrine (Safety + Non-Goals)
- `BOUNDARIES.md`
- `NON_GOALS.md`
- `THREAT_MODEL.md`
- `GLOSSARY.md` (binding terms)

Note: `CANONICAL_RUN.md` is the demonstration-first entrypoint and is compatible with this freeze, but it is not itself the compiler surface contract.

## 4) Compatibility Promise

From this freeze forward, the project maintains the following stability expectations:

### 4.1 Determinism
Given the same normative inputs (as defined by the surface), the compilation outcome must be stable:
- identical output bundle structure and meaning, **or**
- identical failure code(s) and the same required-missing dependency meaning.

### 4.2 Failure Semantics Stability
Existing canonical failure identifiers and their meanings are stable and must not be silently reinterpreted.

### 4.3 Output Bundle Stability
The canonical bundle sections and their meaning are stable. Section names, ordering rules (if specified), and what belongs where are stable.

## 5) What Counts as a Breaking Change

Any of the following is a **breaking change** to the frozen compiler surface:

### 5.1 Inputs / Schemas
- adding new **required** fields to an existing schema
- changing field meaning (semantic reinterpretation)
- renaming schema types or identifiers used as normative references
- changing required upstream dependencies for a previously valid artifact set

### 5.2 Pipeline / Enforcement
- changing stage ordering or stage meaning in a way that changes outcomes
- changing enforcement rules such that the same inputs now fail (or pass) differently without a versioned contract change

### 5.3 Failures
- renaming or deleting existing failure codes
- changing the meaning of an existing failure code
- merging/splitting failure codes in a way that changes how prior cases would classify

### 5.4 Output Bundle
- renaming bundle sections
- changing section meaning or required contents
- changing how failures map to sections (routing) without a versioned contract revision

### 5.5 Failure-to-Bundle Map
- changing routing semantics for an existing failure without a versioned contract revision

## 6) What Is Allowed Without Breaking the Freeze

These are allowed as non-breaking changes (still require discipline, but do not redefine the surface):

- clarifying wording, adding examples, tightening prose **without** changing meaning
- adding OPTIONAL (non-required) fields to schemas (clearly marked optional)
- adding new fictional scenarios/examples that do not introduce new normative requirements
- adding new research notes that remain clearly separated from normative contracts
- adding internal reasoning in private channels (not committed)  explicitly out of scope for the public repo

## 7) Required Change Process (Formal Revision Only)

Any proposed expansion of the compiler surface must follow a formal revision process:

1. Create a dedicated proposal document (e.g., `RFC_COMPILER_SURFACE_<topic>.md`) that includes:
   - the change
   - why it is needed
   - impacted normative files
   - compatibility impact (breaking vs non-breaking)
   - migration notes for upstream systems

2. Update this contract with:
   - a new contract revision identifier (see Section 8)
   - an explicit list of normative deltas

3. Only then may normative surface files be changed.

No drive-by additions to schemas, failures, or bundle format are permitted while this contract is in CLOSED state.

## 8) Contract Revision Identifier

Contract revision is tracked by git history and the explicit status line at the top of this file.

If/when a formal revision occurs, this file must be updated to include:
- Status: **REVISED SURFACE (vNext)** or similar
- a concise Normative Changes section listing exactly what changed

Until then, this baseline remains **CLOSED SURFACE (Baseline Freeze)**.

## 9) Public Repository Safety Constraints (Binding)

Because this repository is public:
- do not introduce sensitive content
- keep validation artifacts fictionalized and scrubbed
- do not name real persons/systems as factual participants in validation narratives
- any Oldsmar-inspired content must remain: public facts (cited) + explicitly fictional scenario

## 10) We Are Here Marker

Compiler closure achieved.
Demonstration-first entrypoint achieved.
Inputs, pipeline, failures, bundle format, and failure routing are frozen under this contract.

Next work must strengthen the compiler proof, not broaden the surface area.
