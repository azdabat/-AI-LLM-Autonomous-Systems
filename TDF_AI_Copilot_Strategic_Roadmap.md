# MTDF AI Copilot — Strategic Roadmap & Vision
### Ala Dabat | 2026
### From Detection Factory to Autonomous Security Intelligence

> *"The framework is the doctrine. The LLM is the execution engine. The roadmap is the proof that detection engineering can scale without losing rigour."*

---

## Executive Summary

The Minimum Truth Detection Framework began as a detection engineering methodology — a set of architectural principles for building high-fidelity, scale-safe KQL rules that survive production reality. 

The MTDF AI Copilot transforms that methodology into an **intelligent, automated detection factory** — a system that can ingest attack telemetry, reason from doctrine, generate production-candidate rules, identify coverage gaps, and scale across every layer of the modern attack surface including areas that traditional SOC tooling was never designed to cover.

This document defines the full roadmap: where the Copilot is today, where it is going, and how it evolves from a rule generation assistant into a comprehensive security intelligence platform covering ATT&CK gaps, cousin technique ecosystems, granular threat hunting, bespoke data lakes, and niche system detection that falls outside standard SOC visibility.

---

## Current State — What Is Built

### The Foundation (Operational)

**MTDF Copilot v2** is operational with:

- Claude Sonnet API as the LLM engine via AnythingLLM
- Lean 200-word system prompt encoding MTDF doctrine
- Three skeleton templates (Substrate-First MDE, Intent-First MDE, Substrate-First Sentinel)
- MDE and Sentinel schema reference anchors preventing field hallucination
- Full framework README as RAG knowledge base — cousin tables, ecosystem maps, scoring doctrine
- Empire C2 telemetry split methodology — 9 detection surface groups
- First production-candidate rule: T1003.001 LSASS Memory Dump (MDE + Sentinel variants)

**What it can do today:**
- Take a technique name or raw telemetry and generate a schema-correct 4-phase KQL rule
- Identify cousin techniques from the framework README and propose adjacent sensors
- Declare anchoring strategy and skeleton before generating any KQL
- Produce technique-specific reinforcement signals beyond the generic skeleton
- Generate both MDE and Sentinel variants in a single session
- Document scoring rationale in a human-readable decision table

**What it cannot do yet:**
Everything else on this roadmap.

---

## The Strategic Vision

### Three Layers of Intelligence

The Copilot evolves through three distinct capability layers:

```
LAYER 3 — AUTONOMOUS INTELLIGENCE
         Self-directed coverage gap analysis
         Bespoke data lake integration
         Niche system detection generation
         Granular threat hunting hypothesis engine
                    ▲
LAYER 2 — ECOSYSTEM COVERAGE
         Full ATT&CK cousin gap elimination
         Cross-tactic correlation rules
         Threat actor profile-driven generation
         Custom detection packs per vertical
                    ▲
LAYER 1 — DETECTION FACTORY (Current)
         Technique → rule generation
         Telemetry → minimum truth identification
         Cousin rule suggestion
         MDE + Sentinel schema compliance
```

Each layer builds on the previous. Layer 1 must be complete and validated before Layer 2 begins. This document covers all three.

---

## LAYER 1 ROADMAP — Detection Factory Completion

### Objective
Complete the Tier 1 and Tier 2 composite rule sets with ADX-validated receipts for every technique. This is the foundation everything else rests on.

### Tier 1 — Baseline Pack (Priority Order)

| # | Technique | MITRE | Anchoring | Copilot Prompt Pattern | Telemetry Source | Status |
|---|-----------|-------|-----------|----------------------|-----------------|--------|
| 1 | LSASS via comsvcs.dll | T1003.001 | Intent-First | `T1003.001. rundll32 + comsvcs. MDE. Skeleton B.` | Empire Invoke-Mimikatz | 🟡 ADX pending |
| 2 | LSASS via Task Manager | T1003.001 | Substrate-First | `T1003.001. Task Manager dump. MDE. Skeleton A.` | Manual lab | 🔴 Queued |
| 3 | LSASS via ProcDump | T1003.001 | Intent-First | `T1003.001. ProcDump -ma lsass. MDE. Skeleton B.` | Atomic T1003.001 | 🔴 Queued |
| 4 | Registry Run Key | T1547.001 | Intent-First | `T1547.001. Run key write + payload. MDE. Skeleton B.` | Atomic T1547.001 | 🔴 Queued |
| 5 | Scheduled Task CLI | T1053.005 | Intent-First | `T1053.005. schtasks /create. MDE. Skeleton B.` | Atomic T1053.005 | 🔴 Queued |
| 6 | TaskCache Silent | T1053.005 | Substrate-First | `T1053.005. TaskCache registry write. MDE. Skeleton A.` | Empire persistence | 🔴 Queued |
| 7 | PowerShell Abuse | T1059.001 | Intent-First | `T1059.001. -Enc + IEX + VirtualAlloc. MDE. Skeleton B.` | Empire stager | 🔴 Queued |
| 8 | WMI Subscription | T1546.003 | Substrate-First | `T1546.003. scrcons + vbscript.dll. MDE. Skeleton A.` | Empire Invoke-WMI | 🔴 Queued |
| 9 | LOLBins Proxy Exec | T1218 | Intent-First | `T1218. rundll32 + mshta + regsvr32. MDE. Skeleton B.` | Atomic T1218.* | 🔴 Queued |
| 10 | OAuth Consent Abuse | T1621 | Intent-First | `T1621. high-risk scope grant. Sentinel AuditLogs.` | Synthetic | 🔴 Queued |

### Tier 2 — Composite Correlation Pack

| # | Technique | MITRE | Notes | Status |
|---|-----------|-------|-------|--------|
| 11 | SMB Lateral Movement | T1021.002 | + WMI cousin immediately | 🔴 Queued |
| 12 | WMI Remote Execution | T1021.003 | Cousin to SMB | 🔴 Queued |
| 13 | BYOVD Driver Staging | T1068 | Substrate-First, temporal detection | 🔴 Queued |
| 14 | DLL Sideloading | T1574.002 | Correlation required — both events mandatory | 🔴 Queued |
| 15 | Pass-the-Hash | T1550.002 | Network logon type 3 anchor | 🔴 Queued |
| 16 | Kerberoasting | T1558.003 | TGS request volume + RC4 | 🔴 Queued |
| 17 | DCSync | T1003.006 | Cousin to LSASS dump | 🔴 Queued |
| 18 | Defense Evasion (AV Tamper) | T1562.001 | fltmc + WinDefend stop | 🔴 Queued |
| 19 | Shadow Copy Deletion | T1490 | Ransomware precursor anchor | 🔴 Queued |
| 20 | Archive Staging + Exfil | T1560.001 | Pre-exfil compression | 🔴 Queued |

---

## LAYER 2 ROADMAP — Full ATT&CK Cousin Gap Coverage

### The Cousin Gap Problem

The MTDF Cousin Technique Doctrine establishes that modern adversaries shift seamlessly between adjacent substrates to achieve the same operational goal. A detection rule that covers SMB lateral movement but not WMI lateral movement provides false coverage confidence.

The Copilot's next evolution is **systematic cousin gap elimination** — using the framework's ecosystem tables and the LLM's reasoning to identify every uncovered adjacent surface and generate the missing sensor.

### Cousin Gap Analysis — The Copilot Prompt

For every completed Tier 1/2 rule, the Copilot runs this analysis:

```
Technique: T[XXXX.XXX] — [name]
Primary rule: COMPLETE

Identify all cousin techniques sharing this adversary goal.
For each cousin:
1. State the substrate difference
2. Declare anchoring strategy
3. Identify the minimum truth anchor
4. State which skeleton applies
5. Flag whether it requires different telemetry
6. Generate the cousin rule

Then identify any ATT&CK sub-techniques not yet covered
by any sensor in the MTDF ecosystem for this tactic.
```

### Known Cousin Gaps by Tactic

**Credential Access — T1003 family**
```
T1003.001 LSASS via comsvcs     → PRIMARY (built)
T1003.001 LSASS via Task Manager → COUSIN (missing)
T1003.001 LSASS via ProcDump    → COUSIN (missing)
T1003.001 LSASS via nanodump    → COUSIN (missing — kernel-level)
T1003.002 SAM extract           → COUSIN (missing)
T1003.003 NTDS.dit via VSS      → COUSIN (missing)
T1003.004 LSA Secrets           → COUSIN (missing)
T1003.006 DCSync                → COUSIN (missing)
T1003.007 Proc memory read      → COUSIN (missing — fileless)
```
**Gap score: 1/9 covered. 8 cousin rules needed.**

**Lateral Movement — T1021 family**
```
T1021.001 RDP                   → MISSING
T1021.002 SMB/Windows Admin Share → MISSING
T1021.003 DCOM                  → MISSING
T1021.004 SSH                   → MISSING (Linux estate)
T1021.005 VNC                   → MISSING
T1021.006 WinRM                 → MISSING
T1021.007 Cloud Services        → MISSING
```
**Gap score: 0/7 covered. Full ecosystem missing.**

**Execution — T1059 family**
```
T1059.001 PowerShell            → QUEUED
T1059.002 AppleScript           → N/A (macOS)
T1059.003 Windows Command Shell → MISSING
T1059.004 Unix Shell            → MISSING (Linux estate)
T1059.005 VBScript              → MISSING
T1059.006 Python                → MISSING
T1059.007 JavaScript            → MISSING
T1059.008 Network Device CLI    → MISSING (network estate)
T1059.009 Cloud API             → MISSING (cloud estate)
```
**Gap score: 0/7 covered (Windows). Cross-platform gaps significant.**

**Persistence — T1547 family**
```
T1547.001 Registry Run Keys     → QUEUED
T1547.002 Authentication Packages → MISSING
T1547.003 Time Providers        → MISSING
T1547.004 Winlogon Helper DLL   → MISSING
T1547.005 Security Support Provider → MISSING
T1547.006 Kernel Modules        → MISSING (Linux)
T1547.009 Shortcut Modification → MISSING
T1547.012 Print Processors      → MISSING
T1547.014 Active Setup          → MISSING
T1547.015 Login Items           → N/A (macOS)
```
**Gap score: 0/9 covered (Windows). 9 cousin rules needed.**

### Copilot-Driven Gap Elimination Workflow

```
WEEK 1-2: Complete Tier 1 rules + immediate cousins
           Each Tier 1 rule generates 2-3 cousin rules in the same session

WEEK 3-4: Tier 2 composite rules + cousin pairs

MONTH 2:  Systematic gap analysis per tactic
           Copilot generates gap report → prioritise by threat actor usage
           Generate missing sensors in batches

MONTH 3+: Cross-tactic correlation rules
           Sensor chains that connect cousin detections at incident layer
```

---

## LAYER 2 — Threat Actor Profile-Driven Generation

### The Vision

Beyond technique coverage, the Copilot evolves to generate **threat actor profile packs** — detection sets tuned to the specific TTPs of named adversaries relevant to Sky's industry vertical (media, broadcast, telecommunications).

**Example prompt pattern:**
```
Generate a detection pack for APT29 (Cozy Bear) targeting
a media/broadcast organisation. Focus on their known TTPs:
- SUNBURST-style supply chain compromise
- FOGGYWEB webshell deployment
- MagicWeb authentication bypass
For each TTP: minimum truth anchor, anchoring strategy,
MDE rule, Sentinel rule, cousin techniques.
```

**Threat actor profiles to build:**
- APT29 — nation-state, supply chain focus
- Lazarus Group — financial motivation, crypto theft
- LockBit 3.0 — ransomware operator TTPs
- TA505 — initial access broker patterns
- Custom profile: Sky-specific threat landscape

---

## LAYER 3 ROADMAP — Granular Threat Hunting & Bespoke Data Lakes

### The Problem the Copilot Solves at Layer 3

Standard SOC tooling covers the standard estate — Windows endpoints, Azure AD, M365, network perimeter. But modern enterprise environments contain entire categories of systems that generate rich security telemetry and receive essentially zero detection coverage:

- **OT/SCADA systems** — operational technology, industrial control systems
- **Custom application logs** — bespoke business applications with proprietary log formats
- **Broadcast/media infrastructure** — encoding systems, playout servers, contribution networks
- **CDN and streaming infrastructure** — content delivery, DRM systems
- **IoT device fleets** — smart building systems, access control, cameras
- **Legacy systems** — mainframes, proprietary protocols, end-of-life platforms
- **Cloud-native workloads** — container orchestration, serverless, service mesh telemetry
- **Developer tooling** — CI/CD pipeline security, code repository anomalies
- **AI/ML infrastructure** — model training environments, inference APIs, vector databases

The Copilot's Layer 3 capability generates **bespoke detection logic for any structured telemetry source** — not just MDE and Sentinel tables.

### The Bespoke Data Lake Prompt Pattern

```
I have a custom telemetry source with the following schema:
[paste field list and sample events]

This data comes from: [system description]
Threat actors targeting this system typically use: [TTPs]
The environment context is: [description]

Apply MTDF doctrine to this schema:
1. Identify the minimum truth anchor for [technique]
2. Declare anchoring strategy (Substrate-First or Intent-First)
3. Generate a 4-phase detection rule using only the available fields
4. Map to the closest MITRE ATT&CK technique
5. Produce a HunterDirective appropriate for the analyst team
   monitoring this system
```

### Bespoke Detection Domains — Sky Context

For Sky specifically, the following domains represent high-value detection gaps:

**Broadcast Infrastructure**
```
Schema: Playout server logs, encoder telemetry, contribution uplink events
Threats: Content tampering, broadcast signal injection, DRM bypass
MTDF anchor: Substrate-First — unauthorised modification of encoding parameters
Coverage gap: Zero published detection rules for broadcast infrastructure
```

**CDN / Streaming**
```
Schema: CDN access logs, streaming manifest requests, DRM token events
Threats: Content scraping at scale, DRM circumvention, session hijacking
MTDF anchor: Intent-First — request patterns inconsistent with legitimate streaming
Coverage gap: Minimal published detection logic for streaming-specific TTPs
```

**AI/ML Infrastructure**
```
Schema: Model inference logs, training job events, vector DB access logs
Threats: Prompt injection, model extraction, training data poisoning
MTDF anchor: Intent-First — query patterns implying capability extraction
Coverage gap: No established detection framework for ML infrastructure threats
```

**CI/CD Pipeline Security**
```
Schema: GitHub Actions logs, Jenkins events, artifact repository access
Threats: Supply chain injection, secrets exfiltration, pipeline tampering
MTDF anchor: Substrate-First — unexpected pipeline modification events
Coverage gap: Emerging area with minimal published detection coverage
```

### Granular Threat Hunting — The Copilot as Hypothesis Engine

Layer 3 also includes the Copilot's evolution into a **threat hunting hypothesis engine** — generating structured hunt hypotheses from threat intelligence, actor profiles, and coverage gap analysis.

**Hunt hypothesis generation prompt:**
```
Based on the MTDF framework coverage gaps and the following
threat intelligence: [CTI summary]

Generate 5 threat hunting hypotheses for the next hunt cycle.
For each hypothesis:
1. The adversary behaviour being hunted
2. The MITRE technique(s) involved
3. The data source required
4. The minimum truth condition to confirm the hypothesis
5. The KQL hunt query skeleton
6. What absence of evidence tells us (negative hunt value)
```

**Hunt output format — the MTDF Hunt Card:**
```
HUNT CARD: [ID]
Hypothesis: [One sentence — what adversary behaviour are we hunting?]
Technique: T[XXXX.XXX]
Data Source: [Table/log source]
Minimum Truth: [Irreducible condition]
Anchoring: [Substrate-First / Intent-First]
Hunt Query: [KQL skeleton]
Confidence if found: [HIGH/MEDIUM/LOW with rationale]
Negative value: [What does not finding this tell us?]
Priority: [CRITICAL/HIGH/MEDIUM based on threat landscape]
```

---

## LAYER 3 — Fine-Tuning the Copilot

### The Fine-Tuning Roadmap

As the detection factory produces validated rules, those rules become training data for a fine-tuned model that internalises MTDF doctrine rather than reading it from context on every query.

**Phase 1 — Training data accumulation (now)**
Every validated rule is a training example:
```
Input:  Technique + anchoring strategy + telemetry context
Output: 4-phase KQL rule with correct structure and scoring
```
Target: 100 validated rules = minimum viable training dataset

**Phase 2 — LoRA fine-tuning (50+ rules)**
Fine-tune a local 7b model on the validated rule corpus:
- Base model: Qwen2.5-coder:7b or CodeLlama-7b
- Fine-tuning method: LoRA (Low-Rank Adaptation) — efficient, runs on consumer GPU
- Training data: Input/output pairs from validated Copilot sessions
- Target: Model that generates MTDF-compliant KQL without needing skeleton documents in context

**Phase 3 — Specialised vertical models (100+ rules)**
Separate fine-tuned models for different detection domains:
- `mtdf-windows` — Windows endpoint detection
- `mtdf-cloud` — Azure AD, M365, cloud-native
- `mtdf-network` — Network traffic, DNS, proxy
- `mtdf-bespoke` — Custom/niche system detection

**Phase 4 — RAG + Fine-tune hybrid (full production)**
Fine-tuned base model + dynamic RAG for:
- Current threat intelligence injection
- New telemetry schema ingestion
- Real-time MITRE ATT&CK updates
- Threat actor profile packs

### The Training Data Pipeline

```
ADX-Docker validates rule → Receipt captured
        │
        ▼
Training pair created:
{
  "input": {
    "technique": "T1003.001",
    "anchoring": "Intent-First",
    "skeleton": "B",
    "telemetry_context": "[relevant fields from Empire data]"
  },
  "output": "[full validated 4-phase KQL rule]"
}
        │
        ▼
Stored in /training-data/validated/
        │
        ▼
At 50 pairs: first LoRA fine-tuning run
At 100 pairs: evaluate fine-tuned vs base model
At 200 pairs: vertical specialisation begins
```

---

## Coverage Metrics — How We Measure Progress

### ATT&CK Coverage Matrix

The [God Mode Roadmap](https://azdabat.github.io/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules/MTDF-Composite-Roadmap.html) tracks visual coverage. The Copilot adds quantitative metrics:

**Coverage score per tactic:**
```
Tactic Coverage Score = 
  (Rules built / Total techniques in tactic) × 
  (Cousin coverage / Total cousin surfaces) × 
  (Validated rules / Total rules built)

Current scores:
Credential Access:  1/17 = 6%  (1 rule, 8 cousins missing)
Execution:          0/9  = 0%  (Tier 1 queued)
Persistence:        0/19 = 0%  (Tier 1 queued)
[All other tactics: 0%]

Target by end of Layer 1: ≥ 60% on Tier 1 tactics
Target by end of Layer 2: ≥ 80% across all Windows tactics
Target by Layer 3:        Coverage extends to non-standard estates
```

### Quality Metrics Per Rule

Every committed rule is scored against:

| Metric | Measure | Target |
|--------|---------|--------|
| Schema compliance | Zero hallucinated field names | 100% |
| Phase completeness | All 4 phases present and populated | 100% |
| ADX validation | Receipt exists | Required for production |
| Minimum fire path | Documented with calculation | Required |
| Cousin identification | At least 1 cousin identified | Required |
| HunterDirective quality | Answers why/what/next specifically | Required |

---

## Integration Points — The Full Ecosystem

The Copilot does not exist in isolation. It integrates with every layer of the MTDF ecosystem:

```
THREAT INTELLIGENCE (MISP lab)
         │ feeds technique prioritisation
         ▼
MTDF COPILOT
         │ generates rules
         ▼
ADX-DOCKER VALIDATION
         │ produces receipts
         ▼
GITHUB PRODUCTION REPOSITORY
         │ public portfolio
         ▼
GOD MODE ROADMAP (GitHub Pages)
         │ visual coverage matrix
         ▼
SOC DEPLOYMENT
         │ MDE custom detections / Sentinel analytics rules
         ▼
ALERT TRIAGE
         │ HunterDirective guides analyst
         ▼
INCIDENT LAYER
         │ cousin sensors stitched via entity keys
         ▼
THREAT HUNT CYCLE
         │ gaps identified → back to Copilot
         ▼
NEW TECHNIQUE PRIORITISATION
         └──────────────────────────► back to top
```

---

## The Niche System Problem — Why This Matters

### What Standard SOC Misses

Every enterprise has a tier of systems that generate security-relevant telemetry and receive essentially zero detection engineering attention. The reasons are consistent:

- Log formats are proprietary or poorly documented
- The systems are managed by teams outside security
- Threat models for these systems don't exist in public frameworks
- Detection engineers don't have the domain knowledge to write rules

This is where the Copilot's Layer 3 capability has the highest impact. Given any structured log format and a description of the system, the Copilot can:

1. Apply MTDF doctrine to identify what minimum truth looks like in that schema
2. Generate detection logic that follows the same rigorous 4-phase structure
3. Produce HunterDirectives appropriate for the analysts who monitor that system
4. Map the detections to the closest available MITRE technique

**The result:** Detection coverage that scales beyond the standard estate into every corner of the environment that generates logs — regardless of whether a public detection rule has ever been written for that system.

### The Prompt That Unlocks This

```
You are the MTDF Copilot operating in BESPOKE MODE.

I am providing you with a non-standard telemetry source.
Apply MTDF doctrine to this schema as if it were a standard
EDR table. The core principles do not change:
- Find the minimum truth anchor
- Declare anchoring strategy
- Enrich natively from available fields
- Score convergence
- Output a HunterDirective

Schema: [field list]
Sample event: [example log entry]
System: [what this system does]
Threat scenario: [what we are trying to detect]

Generate the detection rule.
```

---

## Summary — The Three-Horizon Vision

| Horizon | Capability | Timeline | Measure of Success |
|---------|-----------|----------|-------------------|
| **Layer 1** | Detection factory — Tier 1+2 complete | Now → 3 months | 20 validated rules with receipts |
| **Layer 2** | Full ATT&CK cousin coverage + actor profiles | 3-6 months | 80% tactic coverage, 5 actor packs |
| **Layer 3** | Bespoke data lakes + fine-tuned models + hunt engine | 6-12 months | Non-standard estate coverage, LoRA fine-tune running |

**The north star:**

A detection engineering system where any analyst — regardless of experience level — can describe an attack scenario in plain English and receive a production-candidate, MTDF-compliant detection rule covering that technique and all its cousin surfaces, validated against real telemetry, with a HunterDirective that tells them exactly what to do when it fires.

That system exists. It is being built. This roadmap is how it gets finished.

---

## Attribution & Licence

**Copyright (c) 2026 Ala Dabat. All Rights Reserved.**

Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

Part of the [Minimum Truth Detection Framework](https://azdabat.github.io/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules/index.html)

---

*"Detection engineering that only covers the standard estate provides a false sense of security. The adversary does not restrict themselves to systems that have published detection rules. Neither should we."*

*— Ala Dabat, 2026*
