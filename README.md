# AI & LLM Security Research
### Ala Dabat | 2026 | Part of the Minimum Truth Detection Framework

> *"The attack surface doesn't stop at the endpoint. As AI systems become infrastructure, the threat model must evolve with them."*

---

## Overview

This repository documents original security research at the intersection of **AI/LLM systems** and **detection engineering** — applying the same adversarial rigour that underpins the [Minimum Truth Detection Framework](https://azdabat.github.io/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules/index.html) to a new and rapidly evolving threat surface.

The work here spans three connected disciplines:

**1. AI System Threat Modelling**
Applying STRIDE methodology to LLM-based and agentic architectures — identifying risks introduced by autonomy, tool use, memory, orchestration layers, and RAG pipelines. This is not theoretical — every threat model here is derived from systems I have built and attacked myself.

**2. LLM-Augmented Detection Engineering**
The MTDF Copilot — an agentic detection engineering pipeline powered by Claude (Anthropic API) and AnythingLLM — produces schema-precise, 4-phase KQL detection rules against real Empire telemetry. This pipeline is itself a case study in AI system security.

**3. AI-Augmented Red & Blue Team Operations**
Research into LLM integration with offensive security tooling (Kali Linux ecosystem) and defensive detection pipelines — exploring how AI accelerates both attack simulation and detection validation, and what the security implications of that acceleration are.

---

## Repository Structure

```
AI-LLM-Security/
│
├── threat-models/
│   ├── STRIDE_LLM_Agent_Pipeline.md          ← STRIDE analysis of agentic pipeline
│   ├── AI_Threat_Classes_Detection_View.md   ← 5 AI threat classes, detection lens
│   └── RAG_Pipeline_Attack_Surface.md        ← RAG-specific threats and mitigations
│
├── mtdf-copilot/
│   ├── Architecture.md                       ← Pipeline design and trust boundaries
│   ├── Detection_Factory_Workflow.md         ← End-to-end rule generation workflow
│   └── Prompt_Engineering_Doctrine.md        ← System prompt design principles
│
├── red-blue-ai/
│   ├── LLM_Kali_Integration_Research.md      ← AI-augmented offensive security
│   ├── AI_Purple_Team_Methodology.md         ← LLM-assisted purple team operations
│   └── Detection_Validation_Pipeline.md      ← Automated detection testing with AI
│
└── receipts/
    └── [ADX validation screenshots]
```

---

## Section 1 — AI System Threat Modelling

### Why This Matters Now

Enterprise AI adoption is accelerating faster than security frameworks can adapt. STRIDE was designed for traditional software systems — deterministic, predictable, auditable. LLM-based systems introduce threat classes that have no precedent in classical application security:

- **Non-deterministic outputs** — the same input produces different outputs across runs
- **Instruction-data conflation** — the model cannot natively distinguish between instructions and data it processes
- **Emergent behaviour** — multi-step agent chains produce outcomes that were not explicitly programmed and cannot be fully predicted
- **Indirect attack surfaces** — the attack vector is often data the system *reads*, not input the attacker *types*

The security industry is still catching up. Most published AI threat models are vendor whitepapers. This repository documents threat models derived from **systems I have built and operated** — grounded in operational reality, not theoretical frameworks.

### STRIDE Applied to an LLM Agent Pipeline

The MTDF Copilot is an agentic detection engineering system with the following trust boundary map:

```
[User Input]
     │
     ▼
[AnythingLLM Orchestrator] ──► [Vector DB / RAG Engine]
     │                               │
     │                         [Embedded Documents]
     │                         - Telemetry files (attack data)
     │                         - Skeleton templates
     │                         - Framework knowledge base
     ▼
[Anthropic Claude API] ──► [KQL Rule Output]
     │
[External Network Boundary]
```

**Key threats identified at each boundary:**

| Boundary | Highest Risk Threat | Class | Mitigation |
|----------|-------------------|-------|-----------|
| User → Orchestrator | System prompt override | Spoofing / Tampering | Prompt hardening, output validation |
| Document → RAG | **Indirect prompt injection** | Tampering | Input sanitisation before embedding |
| RAG → Model context | RAG poisoning | Tampering | Document provenance verification |
| Orchestrator → API | Sensitive data exfiltration | Information Disclosure | Never embed real production telemetry |
| API → Output | Malicious KQL generation | Tampering | Human review before deployment |
| Tool execution | Unintended agent actions | Elevation of Privilege | Least-privilege tool access |

**Full STRIDE analysis:** [STRIDE_LLM_Agent_Pipeline.md](./threat-models/STRIDE_LLM_Agent_Pipeline.md)

---

### The Five AI-Specific Threat Classes

| Class | Description | Detection Angle |
|-------|-------------|----------------|
| **Prompt Injection** | Attacker-controlled input overrides model instructions | Monitor for instruction patterns in data inputs; anomalous output format deviation |
| **Model Abuse** | Jailbreaking, capability extraction, harmful output generation | Output content classification; rate limiting on unusual query patterns |
| **Data Leakage** | System prompt extraction, training data memorisation, cross-session leakage | Output filtering for known-sensitive strings; session isolation monitoring |
| **Insecure Tool Execution** | Manipulated inputs trigger dangerous tool calls | Tool call audit logging; human-in-the-loop for irreversible actions |
| **Emergent Agent Behaviour** | Multi-step chains produce unintended outcomes | Agent action scope constraints; reasoning chain audit trail |

**Full analysis with detection engineering perspective:** [AI_Threat_Classes_Detection_View.md](./threat-models/AI_Threat_Classes_Detection_View.md)

---

## Section 2 — MTDF Copilot

### What It Is

An agentic KQL detection rule generation pipeline that takes a MITRE technique prompt and produces a schema-precise, 4-phase detection rule following the Minimum Truth Detection Framework doctrine.

**Architecture:**
- **Engine:** Claude Sonnet (Anthropic API) via AnythingLLM
- **Knowledge base:** Framework README, skeleton templates, MDE/Sentinel schema references
- **Telemetry:** Empire C2 framework attack data, split by detection surface
- **Output:** Production-candidate KQL rules for Microsoft Defender XDR and Microsoft Sentinel

### First Validated Output — T1003.001

**Technique:** OS Credential Dumping: LSASS Memory via comsvcs.dll MiniDump

**Anchoring Strategy:** Intent-First — rundll32.exe is ubiquitous; the malicious primitive is the explicit `MiniDump` export call targeting lsass in the command line.

**Key engineering decisions:**
- Dual primitive coverage: named export (`MiniDump`) AND ordinal form (`#24`) — catches deliberate obfuscation
- Sentinel variant handles dual ingestion paths: SecurityEvent EID 4688 (native auditing) and WindowsEvent Sysmon EID 1 — with documented field availability differences
- `IsElevatedSession` intentionally absent from Sentinel variant — SecurityEvent does not expose integrity level natively; `IsOrdinalForm` (+15) fills the scoring gap
- Minimum fire path calculated and documented: Base 55 + IsElevatedSession 15 + IsSuspectOutputPath 10 = 80 ≥ 75

**Rule files:**
- [T1003.001_rundll32_comsvcs_MDE.kql](./rules/credential-access/T1003.001_rundll32_comsvcs_MDE.kql)
- [T1003.001_rundll32_comsvcs_Sentinel.kql](./rules/credential-access/T1003.001_rundll32_comsvcs_Sentinel.kql)

---

## Section 3 — AI-Augmented Red & Blue Team Operations

### The Vision

The convergence of LLMs with offensive security tooling represents both the most significant capability multiplier and the most significant risk amplifier in modern security operations.

**On the offensive side:**
LLMs integrated with Kali Linux tooling can accelerate:
- Attack path generation and hypothesis formation
- Automated exploitation chain documentation
- Natural language to command translation (recon → exploitation → post-ex)
- Purple team scenario generation from threat intelligence

**On the defensive side:**
LLMs integrated with detection pipelines can accelerate:
- Detection rule generation from telemetry (the MTDF Copilot)
- Alert triage and contextual enrichment
- Threat hunt hypothesis generation
- Incident timeline reconstruction

**The security paradox:**
The same capability that makes an AI-augmented red team faster makes an AI-augmented attacker faster. Understanding both sides is the only way to build defences that hold.

### Research Direction — LLM + Kali Integration

**Phase 1 — Documented (in progress):**
- LLM-assisted reconnaissance automation
- Natural language to Nmap/Metasploit/Burp command translation
- AI-generated attack path documentation for purple team scenarios
- Automated Empire module selection based on target environment profile

**Phase 2 — Planned:**
- Closed-loop purple team pipeline: LLM generates attack → executes in lab → detection fires → LLM analyses gap → generates cousin rule
- AI-assisted MITRE ATT&CK coverage gap analysis
- LLM-driven threat simulation against detection rule set

**Phase 3 — Research horizon:**
- Autonomous adversary emulation with detection feedback loop
- LLM-based variant generation for evasion testing
- AI threat model validation through automated attack simulation

### The Security Implications of AI-Augmented Offensive Tooling

This is not just a capability question — it is a threat model question.

When LLMs are integrated with offensive tooling, new risks emerge:

**Prompt injection in offensive pipelines** — if an AI-augmented red team tool processes target system outputs (banner grabs, error messages, web responses), those outputs could contain prompt injection payloads designed to manipulate the AI's next action.

**Capability amplification** — a less-skilled attacker with AI assistance can execute attacks previously requiring senior expertise. The defender's assumption that attack complexity implies a sophisticated adversary breaks down.

**Attribution complexity** — AI-generated attack patterns may not match known threat actor TTPs, complicating attribution and potentially evading threat-intel-based detections.

**Detection implications** — traditional behavioural detections built on human attacker patterns may need to model AI-assisted attack velocity and sequencing, which differs from manual operation.

---

## Research Philosophy

This work operates at the intersection of three principles from the MTDF:

**Minimum Truth applies to AI systems too.**
Every AI threat has an irreducible condition that must be true for it to be real. Prompt injection requires the model to conflate instruction and data. Insecure tool execution requires the agent to have capability beyond its scope. Start with the minimum truth. Everything else is reinforcement.

**The rule is the sensor. The incident is the narrative.**
AI security detections should be independent surface sensors — one rule per threat class, correlated at the incident layer. A monolithic "AI security" detection that tries to catch everything catches nothing reliably.

**Adversary-informed engineering.**
You cannot build defences for threats you haven't modelled from the attacker's perspective. Every detection in this repository is built by someone who understands how the attack is constructed.

---

## Status

| Research Area | Status |
|--------------|--------|
| STRIDE — LLM Agent Pipeline | 🟡 In Progress |
| AI Threat Classes — Detection View | 🟡 In Progress |
| MTDF Copilot — Architecture | ✅ Complete |
| MTDF Copilot — First Rule (T1003.001) | ✅ Production-candidate |
| LLM + Kali Integration — Phase 1 | 🔴 Planned |
| Purple Team AI Pipeline | 🔴 Planned |
| Autonomous Detection Validation | 🔴 Planned |

---

## Attribution & Licence

All original research and detection logic: **Ala Dabat (2026)**

Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)
Attribution required · Non-commercial · ShareAlike

Part of the [Minimum Truth Detection Framework](https://github.com/azdabat/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules)

---

*"The minimum truth of AI security is this: the model cannot distinguish between instructions and data. Everything else — prompt injection, RAG poisoning, emergent behaviour, insecure tool execution — is reinforcement of that single architectural reality."*

*— Ala Dabat, 2026*

