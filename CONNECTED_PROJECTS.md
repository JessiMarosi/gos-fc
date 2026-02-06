# GOS-FC  Connected Projects (Repository Map)

## Purpose

This document links GOS-FC to the related Bytelock / GOS ecosystem projects it depends on conceptually.

This is **not** a runtime integration map.
It is a **semantic dependency map** describing the upstream systems whose outputs may be compiled by GOS-FC.

GOS-FC remains a compiler:
- it ingests declared artifacts
- it enforces deterministic compilation rules
- it outputs replayable bundles or explicit failure reports

It does not replace the systems listed here.

---

## Core Dependency Systems

### 1) DADS  Decision Accountability Documentation System
**Role:** Primary input source (the source language for the compiler)

**Provides:**
- decision records
- rationale at time of decision
- explicit risk acceptance
- named approvers and authority context
- append-only history

**Why it matters:**
Without DADS-style records, GOS-FC has nothing meaningful to compile.

---

### 2) ChainLogic-DCCS  Digital Chain of Custody & Custody Sequencing
**Role:** Custody + authority integrity substrate

**Provides:**
- authority transitions across people/roles
- custody of decisions and evidence artifacts
- provenance across systems and time
- non-repudiable sequencing (anti-tamper knowing)

**Why it matters:**
GOS-FC cannot compile defensible accountability unless custody and authority transitions are provable.

---

### 3) Cairn  Assumption-Tracked Reasoning Ledger
**Role:** Assumptions + reasoning persistence

**Provides:**
- explicit assumptions treated as true at the time
- supporting evidence links
- review/revalidation triggers
- long-horizon reasoning memory

**Why it matters:**
Accountability collapses when assumptions disappear.
Cairn turns assumptions into compile-time dependencies instead of institutional folklore.

---

## Supporting Systems (Conditional / Optional)

### 4) Axiom Forge
**Role:** Obligation discipline for high-stakes technical claims (optional but strengthening)

**Provides:**
- formal obligation declarations
- explicit analytic shape of claims
- deterministic failure when assumptions are missing

**Why it matters:**
When decisions rest on technical claims (safe, equivalent, meets X), Axiom Forge strengthens the proof discipline feeding the compiler.

---

### 5) Veridion
**Role:** Integrity state validation (supporting)

**Provides:**
- declared valid system states
- transition constraints
- integrity drift validation signals

**Why it matters:**
GOS-FC asks: Can accountability be reconstructed?
Veridion asks: Was the system actually in the state people believed it was?
These must align when both are present.

---

### 6) DeepSee-Live
**Role:** Context preservation (optional)

**Provides:**
- time-ordered contextual events
- correlated operational context
- replayable investigation views

**Why it matters:**
Not required for compilation, but reduces ambiguity and prevents post-hoc narrative invention when present.

---

### 7) StrataVault
**Role:** Data boundary enforcement (supporting)

**Provides:**
- layered access rules for evidence
- provenance for evidence access
- boundary-aware handling of sensitive artifacts

**Why it matters:**
Forensic artifacts are only credible if boundaries were enforced and access was justified.

---

## One-Sentence Memory Anchor

GOS-FC is the forensic compiler that proves whether the outputs of DADS, ChainLogic, Cairn, and related integrity systems can be assembled into a defensible institutional truth  or must fail deterministically.

---

## Non-Integration Statement

Linking these projects does not imply:
- runtime coupling
- shared telemetry
- enforcement authority
- monitoring or surveillance
- detection or attribution capability

GOS-FC remains compilation-only by doctrine.
