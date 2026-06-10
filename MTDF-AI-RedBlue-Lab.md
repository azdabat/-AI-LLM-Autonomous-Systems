# MTDF AI Red/Blue Lab
### Ala Dabat | 2026 | Minimum Truth Detection Framework

> *"The closed loop between attack simulation and detection engineering is where security maturity is built. Not in theory. In telemetry."*

---

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
[![Framework](https://img.shields.io/badge/Framework-MTDF-blue)](https://azdabat.github.io/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules/index.html)
[![Status](https://img.shields.io/badge/Status-Active%20Research-orange)]()

---

## What This Is

This repository documents the research and architecture for an **AI-augmented purple team lab** — a closed-loop pipeline where:

- **Attack simulation** generates ground-truth telemetry against a controlled lab environment
- **LLM-assisted analysis** identifies technique primitives and minimum truth anchors from that telemetry
- **MTDF Copilot** generates schema-precise, 4-phase KQL detection rules following the Minimum Truth Detection Framework
- **Validation** confirms detection fires against the same telemetry before any rule is committed to production

Every rule in the [MTDF production repository](https://github.com/azdabat/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules) goes through this pipeline. This repository documents how that pipeline is built and operated.

---

## The Core Principle

The MTDF doctrine applied to purple team operations:

> Start with the minimum truth required for the attack to exist.  
> Simulate it. Capture it. Detect it. Validate it.  
> Everything else is noise.

Attack simulation without detection validation is red team theatre.  
Detection engineering without attack simulation is blue team guesswork.  
The closed loop is the only honest methodology.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    MTDF AI RED/BLUE LAB                         │
│                                                                 │
│  ┌─────────────────┐         ┌─────────────────────────────┐   │
│  │   ATTACK LAYER  │         │      DETECTION LAYER        │   │
│  │                 │         │                             │   │
│  │  Empire C2      │         │  MTDF Copilot               │   │
│  │  Atomic Red Team│────────►│  (Claude API +              │   │
│  │  Kali tooling   │         │   AnythingLLM)              │   │
│  │                 │         │                             │   │
│  └────────┬────────┘         └──────────────┬──────────────┘   │
│           │                                 │                  │
│           │ Telemetry                        │ KQL Rules        │
│           ▼                                 ▼                  │
│  ┌─────────────────┐         ┌─────────────────────────────┐   │
│  │  Lab Environment│         │  ADX-Docker Validation      │   │
│  │  Windows VM     │         │                             │   │
│  │  Sysmon         │────────►│  Rule fires? → Receipt      │   │
│  │  MDE/Sentinel   │         │  Rule misses? → Refine      │   │
│  └─────────────────┘         └─────────────────────────────┘   │
│                                                                 │
│              ◄──── CLOSED LOOP ────►                           │
└─────────────────────────────────────────────────────────────────┘
```

### Components

| Component | Role | Status |
|-----------|------|--------|
| **Empire C2** | Attack simulation + telemetry generation | 🟡 Lab only |
| **Atomic Red Team** | Validated atomic test execution | 🟡 Lab only |
| **Kali Linux** | Offensive tooling + recon simulation | 🔴 Planned |
| **MTDF Copilot** | LLM-assisted rule generation | ✅ Operational |
| **ADX-Docker** | Rule validation against telemetry | ✅ Operational |
| **AnythingLLM** | Orchestration + RAG knowledge base | ✅ Operational |
| **Claude API** | LLM engine (Anthropic) | ✅ Operational |

---

## The Detection Factory Loop

Every MTDF production rule follows this exact path:

```
STEP 1 ── TECHNIQUE SELECTION
          Follow the MTDF Tier 1 → Tier 2 → Tier 3 roadmap
          One technique at a time

STEP 2 ── ATTACK SIMULATION
          Execute technique in isolated lab environment
          Capture all generated telemetry (JSON/EVTX)
          
STEP 3 ── TELEMETRY PROCESSING
          Split raw telemetry by detection surface
          Load signal files into MTDF Copilot workspace
          Discard noise baseline (does not go to LLM)
          
STEP 4 ── RULE GENERATION
          MTDF Copilot analyses telemetry
          Identifies minimum truth anchor
          Declares anchoring strategy + skeleton
          Generates 4-phase KQL rule — MDE + Sentinel variants
          
STEP 5 ── VALIDATION
          Run rule against telemetry in ADX-Docker
          Confirm detection fires on attack traffic
          Confirm no false positive on clean baseline
          Screenshot the hit → this is the receipt
          
STEP 6 ── COUSIN RULE GENERATION
          Identify adjacent technique surfaces
          Generate cousin rules for full ecosystem coverage
          Repeat Steps 4-5 for each cousin
          
STEP 7 ── COMMIT WITH RECEIPT
          Rule + receipt committed to production repository
          Maturity status updated in MTDF roadmap
          Technique marked complete on God Mode matrix
```

---

## Lab Environment

### Minimum Viable Lab

```
Host Machine (Windows 11)
├── Ollama — local LLM fallback (qwen2.5-coder:7b, mtdf-copilot)
├── AnythingLLM — orchestration + workspace
├── ADX-Docker — Kusto validation environment
└── Claude API — primary LLM engine

Isolated VM Network (192.168.56.0/24)
├── Windows 10/11 VM — attack target
│   ├── Sysmon (full configuration)
│   ├── Windows Event Forwarding enabled
│   └── MDE sensor (if available) OR EVTX export
└── Kali Linux VM — attack platform
    ├── Empire C2
    ├── Atomic Red Team
    └── Standard Kali tooling
```

### Critical Isolation Requirements

**The lab network must be completely isolated from production.**

- No route between lab VMs and production infrastructure
- Empire C2 listeners bound to lab network range only
- Telemetry exported as files — never sent directly to production SIEM
- API keys for Claude/AnythingLLM on host machine — never inside VMs

---

## MTDF Copilot Integration

The MTDF Copilot is the detection engineering engine at the centre of this pipeline.

### Architecture

```
Telemetry Files (split by surface)
         │
         ▼
AnythingLLM Workspace
├── System Prompt (MTDF doctrine — lean 200 words)
├── Skeleton A — Substrate-First MDE
├── Skeleton B — Intent-First MDE  
├── Skeleton C — Substrate-First Sentinel
├── Schema Reference — MDE tables + fields
├── Schema Reference — Sentinel tables + fields
└── Framework README — cousin tables, ecosystem maps
         │
         ▼
Claude Sonnet API (Anthropic)
         │
         ▼
4-Phase KQL Rule Output
├── PHASE 1: Minimum Truth
├── PHASE 2: Native Enrichment
├── PHASE 3: Convergence Scoring
└── PHASE 4: Hunter Directive
```

### Prompt Patterns

**Standard rule generation:**
```
T[XXXX.XXX]. [substrate] + [primitive]. MDE rule. [Intent-First/Substrate-First]. Skeleton [A/B/C].
```

**Telemetry-driven generation:**
```
Analyse the embedded telemetry. Identify Empire execution 
fingerprints. Declare anchoring strategy. Generate production 
rule for MDE targeting T[XXXX.XXX].
```

**Cousin rule generation:**
```
Identify all cousin techniques to T[XXXX.XXX]. List each 
cousin, its minimum truth anchor, and skeleton. Generate 
the first cousin rule.
```

---

## Validated Output — T1003.001

The first production-candidate rule generated through this pipeline:

**Technique:** OS Credential Dumping: LSASS Memory via comsvcs.dll MiniDump  
**Anchoring:** Intent-First — rundll32 is ubiquitous; the malicious primitive is the explicit `MiniDump` export call targeting lsass  
**Variants:** MDE (DeviceProcessEvents) + Sentinel (SecurityEvent EID 4688 + Sysmon EID 1)

**Key engineering decisions:**

**Dual primitive coverage** — named export (`comsvcs,MiniDump`) AND ordinal form (`comsvcs,#24`). The ordinal form is deliberate export name obfuscation used by sophisticated operators. Most published rules miss it.

**Sentinel dual ingestion path** — SecurityEvent EID 4688 (Windows native auditing) and WindowsEvent Sysmon EID 1 handled separately with documented field availability differences. `IsElevatedSession` intentionally absent from Sentinel variant — SecurityEvent does not expose integrity level natively. `IsOrdinalForm` (+15 points) structurally fills that scoring gap.

**Minimum fire path documented** — Base 55 + IsElevatedSession 15 + IsSuspectOutputPath 10 = 80 ≥ 75 threshold. The rule fires on a bare, non-obfuscated dump to a staging path running as SYSTEM — the realistic attacker minimum.

**Scoring decision table:**

| Signal | Points | Rationale |
|--------|--------|-----------|
| Base score | +55 | Primitive is explicit — MiniDump + lsass is irreducible |
| IsElevatedSession | +15 | SeDebugPrivilege required; System/High integrity confirms capability |
| IsOrdinalForm (#24) | +15 | Deliberate obfuscation of export name — no legitimate use |
| IsSuspectOutputPath | +10 | Dump staged to exfil-friendly path |
| IsUnsignedParent | +10 | Unsigned parent breaks trusted chain |
| IsOfficeParent | +15 | Macro → rundll32 is a confirmed kill-chain pattern |
| IsScriptParent | +10 | Script engine spawning rundll32 for dumping |
| IsEncodedPayload | +20 | Obfuscation of the dump command itself |
| Management tool parent | -15 | Soft reduction only — analyst still sees the alert |

**Status:** Production-candidate. Pending ADX-Docker receipt.

**Rule files:**
- `rules/credential-access/T1003.001_rundll32_comsvcs_MDE.kql`
- `rules/credential-access/T1003.001_rundll32_comsvcs_Sentinel.kql`

---

## Roadmap

### Phase 1 — Detection Factory (Active)

Work through the MTDF Tier 1 baseline pack using the closed loop:

| Technique | MITRE | Anchoring | Status |
|-----------|-------|-----------|--------|
| LSASS Memory Dump (comsvcs) | T1003.001 | Intent-First | 🟡 ADX pending |
| LSASS Memory Dump (Task Mgr) | T1003.001 | Intent-First | 🔴 Queued |
| LSASS Memory Dump (ProcDump) | T1003.001 | Intent-First | 🔴 Queued |
| Registry Run Key Persistence | T1547.001 | Intent-First | 🔴 Queued |
| Scheduled Task CLI | T1053.005 | Intent-First | 🔴 Queued |
| Scheduled Task Silent (TaskCache) | T1053.005 | Substrate-First | 🔴 Queued |
| PowerShell Abuse | T1059.001 | Intent-First | 🔴 Queued |
| WMI Permanent Subscription | T1546.003 | Substrate-First | 🔴 Queued |
| LOLBins Proxy Execution | T1218 | Intent-First | 🔴 Queued |

### Phase 2 — Kali Integration (Planned Q3 2026)

LLM-assisted offensive tooling integrated with the detection pipeline:

**Objective:** Natural language → attack execution → telemetry capture → detection generation in a single workflow. The LLM drives both sides of the purple team exercise.

**Research questions:**
- Can an LLM accurately translate technique descriptions into Kali tool commands?
- Does LLM-assisted attack execution produce telemetry distinguishable from manual operation?
- Can the same LLM that generated the attack identify its own minimum truth anchor in the resulting telemetry?

**Scope — Phase 2 research:**
- Recon simulation (Nmap, Responder, BloodHound) → detection rule generation
- Exploitation chain documentation → kill chain mapping
- Post-exploitation telemetry → cousin rule identification
- All activity confined to isolated lab network

### Phase 3 — Autonomous Validation Loop (Planned Q4 2026)

Fully automated pipeline:
```
LLM generates attack scenario
        │
        ▼
Atomic Red Team executes in lab
        │
        ▼
Telemetry captured automatically
        │
        ▼
MTDF Copilot generates detection rule
        │
        ▼
ADX validates rule fires
        │
        ▼
If miss → LLM refines rule → repeat
If hit  → commit to production
```

### Phase 4 — AI Threat Modelling (Parallel Track)

STRIDE-based threat modelling of the pipeline itself and of AI systems at Sky scale:

- Threat model of the MTDF Copilot agentic pipeline
- AI-specific threat class detection rules (prompt injection, model abuse, emergent behaviour)
- KQL detections for LLM system anomalies in enterprise environments

---

## AI & LLM Security Research

This lab is also the practical foundation for original research into **AI system security** — applying detection engineering principles to the threats that emerge when LLMs become enterprise infrastructure.

### The Five AI Threat Classes — Detection Engineering Perspective

| Threat Class | Minimum Truth Anchor | Anchoring Strategy | Detection Surface |
|-------------|---------------------|-------------------|------------------|
| **Prompt Injection** | Instruction pattern in external data processed by model | Intent-First | Input/output logging; anomalous output format |
| **Model Abuse** | Query patterns inconsistent with defined use case | Intent-First | Usage telemetry; rate anomalies |
| **Data Leakage** | Model output containing system prompt or RAG content | Substrate-First | Output content classification |
| **Insecure Tool Execution** | Tool call with parameters outside defined scope | Intent-First | Tool audit log; scope violation detection |
| **Emergent Agent Behaviour** | Action sequence deviating from expected decision tree | Substrate-First | Agent reasoning chain audit trail |

**Full research:** [AI_Threat_Classes_Detection_View.md](./threat-models/AI_Threat_Classes_Detection_View.md)

### STRIDE Applied to This Pipeline

The MTDF Copilot pipeline is itself a case study in AI system security. Full STRIDE analysis published at:

[STRIDE_LLM_Agent_Pipeline.md](./threat-models/STRIDE_LLM_Agent_Pipeline.md)

Key findings:
- Highest risk boundary: document ingestion → vector DB (indirect prompt injection via telemetry files)
- Critical control: real production telemetry must never be sent to external APIs
- Novel threat: RAG poisoning through adversarially crafted knowledge base documents

---

## Repository Structure

```
MTDF-AI-RedBlue-Lab/
│
├── README.md                              ← This file
│
├── threat-models/
│   ├── STRIDE_LLM_Agent_Pipeline.md      ← STRIDE of MTDF Copilot
│   ├── AI_Threat_Classes_Detection_View.md
│   └── RAG_Pipeline_Attack_Surface.md
│
├── copilot/
│   ├── MTDF_Lean_SystemPrompt.txt        ← Workspace system prompt
│   ├── MTDF_Skeleton_A_SubstrateFirst.txt
│   ├── MTDF_Skeleton_B_IntentFirst.txt
│   ├── MTDF_Skeleton_C_Sentinel.txt
│   ├── MTDF_Schema_MDE.txt
│   └── MTDF_Schema_Sentinel.txt
│
├── rules/
│   ├── credential-access/
│   │   ├── T1003.001_rundll32_comsvcs_MDE.kql
│   │   └── T1003.001_rundll32_comsvcs_Sentinel.kql
│   ├── persistence/
│   ├── execution/
│   └── lateral-movement/
│
├── receipts/
│   └── [ADX validation screenshots — added as rules are validated]
│
├── telemetry-methodology/
│   ├── Telemetry_Split_Methodology.md   ← How to split raw telemetry
│   └── Signal_vs_Noise_Classification.md
│
└── lab-setup/
    ├── Lab_Architecture.md              ← VM network isolation setup
    ├── Sysmon_Config.md                 ← Sysmon deployment guide
    └── Empire_Lab_Setup.md             ← C2 lab configuration
```

---

## Relationship to MTDF

This repository is the **operational execution layer** of the Minimum Truth Detection Framework ecosystem:

| Repository | Role |
|------------|------|
| [MTDF Core](https://github.com/azdabat/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules) | Doctrine, validated composite rules, ADX receipts |
| [MTDF AI Red/Blue Lab](.) | Attack simulation, LLM-assisted rule generation, closed loop validation |
| [ATLAS](https://github.com/azdabat/ATLAS-ATTACK-ECOSSYSTEM) | Strategic ecosystem map, cousin relationships |
| [God Mode Roadmap](https://azdabat.github.io/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules/MTDF-Composite-Roadmap.html) | Interactive coverage matrix |

---

## Safety & Scope

All offensive tooling and attack simulation in this repository is:

- Executed exclusively in an isolated lab network (no route to production)
- Documented for defensive detection engineering purposes
- Never tested against systems without explicit authorisation
- Compliant with responsible disclosure principles

This repository does not publish exploit code, working payloads, or operational attack infrastructure. It documents the **telemetry signatures and detection logic** that emerge from attack simulation — the blue team artefact of every red team action.

---

## Status

| Component | Status |
|-----------|--------|
| MTDF Copilot pipeline | ✅ Operational |
| First production rule (T1003.001) | 🟡 ADX validation pending |
| Telemetry split methodology | ✅ Complete |
| STRIDE threat model | 🟡 In Progress |
| Kali integration research | 🔴 Planned Q3 2026 |
| Autonomous validation loop | 🔴 Planned Q4 2026 |

---

## Attribution & Licence

**Copyright (c) 2026 Ala Dabat. All Rights Reserved.**

Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

- Attribution required — credit Ala Dabat
- Non-commercial use only
- ShareAlike — derivatives under same licence

Part of the [Minimum Truth Detection Framework](https://azdabat.github.io/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules/index.html)

---

*"The minimum truth of purple team operations: an attack that cannot be detected might as well not have happened. A detection that has never been tested against a real attack might as well not exist. The closed loop is the only honest methodology."*

*— Ala Dabat, 2026*
