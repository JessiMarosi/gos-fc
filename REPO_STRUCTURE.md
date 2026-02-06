# GOS-FC  Repository Structure (Canonical)

## Purpose

This document defines the authoritative structure of the GOS-FC repository.

It specifies:
- what belongs in each directory
- what must not be placed there
- the phase order in which directories may be populated

If content appears outside its defined location, it is a structural violation.

---

## Root Directory

The repository root contains **doctrine and orientation only**.

Permitted files:
- README.md
- BOUNDARIES.md
- NON_GOALS.md
- THREAT_MODEL.md
- GLOSSARY.md
- CONNECTED_PROJECTS.md
- REPO_STRUCTURE.md
- LICENSE

The root must not contain:
- code
- scripts
- schemas
- data
- operational artifacts
- scenario evidence

---

## RESEARCH/

### Purpose
Contains **public, cited research** used to inform fictional scenario construction.

### Permitted Content
- publicly reported facts (with citations)
- structured timelines of what is publicly known
- explicitly labeled open questions
- source lists

### Prohibited Content
- conclusions about intent or blame
- operational detail
- speculative attribution
- fictional narrative
- recommendations or judgments

### Required Discipline
- facts, hypotheses, and open questions must be separated
- every factual claim must be cited

---

## SCENARIO/

### Purpose
Contains **fully fictionalized validation scenarios** inspired by public facts.

### Permitted Content
- fictional timelines
- fictional decision records
- fictional authority structures
- fictional evidence sets
- adversarial accountability attacks
- explicit mapping to public inspiration sources

### Prohibited Content
- claims about real individuals
- claims about real systems
- operational detail that could cause harm
- unlabeled blending of fact and fiction

All content here must be clearly labeled as fictional.

---

## schemas/

### Purpose
Contains draft schemas for governance artifacts.

### Permitted Content
- YAML or JSON schema drafts
- record type definitions
- validation constraints

### Prohibited Content
- executable code
- real data
- production configurations

Schemas define *shape*, not implementation.

---

## compiler/

### Purpose
Contains conceptual compiler logic descriptions.

### Permitted Content
- rule descriptions
- compilation phases
- failure condition definitions
- deterministic processing plans

### Prohibited Content
- executable compilers
- automation
- live integrations

This directory remains largely descriptive in early phases.

---

## outputs/

### Purpose
Contains **fictional sample outputs** and deterministic failure examples.

### Permitted Content
- fictional forensic bundles
- example failure reports
- illustrative replay artifacts

### Prohibited Content
- real incident data
- real organizational artifacts
- sensitive material

Outputs exist to demonstrate behavior, not results.

---

## tools/

### Purpose
Contains helper scripts for validation and demonstration **in later phases only**.

### Permitted Content
- PowerShell scripts for schema validation
- bundle verification helpers
- deterministic checking tools

### Prohibited Content
- surveillance tooling
- monitoring agents
- operational automation

No tools may be added before research and scenario phases are complete.

---

## Phase Ordering Rules

Content must be added in this order:

1. Doctrine (root files)
2. Repository structure
3. Research
4. Scenario
5. Schemas
6. Compiler descriptions
7. Example outputs
8. Tools

Skipping phases is not permitted.

---

## Canonical Rule

Structure is a form of governance.

If content cannot be placed cleanly within this structure, it does not belong in GOS-FC.

