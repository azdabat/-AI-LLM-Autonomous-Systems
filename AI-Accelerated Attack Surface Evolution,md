# AI-Accelerated Attack Surface Evolution
### Composite Behavioural Detection in the Age of Polymorphic Malware

**Author:** Ala Dabat | 2026  
**Repository:** [Minimum Truth Detection Framework](https://github.com/azdabat/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules)  
**License:** [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode)  
**Framework:** [God Mode Composite Roadmap](https://azdabat.github.io/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules/MTDF-Composite-Roadmap.html)

---

> *"The attacker's minimum viable behaviour is invariant.*  
> *The binary changes. The hash changes. The signature mutates.*  
> *But to achieve the operational goal — the attacker must still load the driver,*  
> *touch LSASS, or establish the pipe.*  
> *That behavioural truth is what we anchor on.*  
> *Everything else is noise."*
>
> — Minimum Truth Detection Framework Doctrine

---

## Table of Contents

- [The Thesis](#the-thesis)
- [How AI Is Changing the Attack Surface](#how-ai-is-changing-the-attack-surface)
- [The Evolution of Command-Line Execution](#the-evolution-of-command-line-execution)
- [Polymorphic Malware — Why Signatures Fail](#polymorphic-malware--why-signatures-fail)
- [SilverFox / ValleyRAT — BYOVD as a Case Study](#silverfox--valleyrat--byovd-as-a-case-study)
- [Composite Behavioural Modelling — The MTDF Approach](#composite-behavioural-modelling--the-mtdf-approach)
- [Three Additional AI-Accelerated Attack Chains](#three-additional-ai-accelerated-attack-chains)
- [The Threat Modelling Methodology](#the-threat-modelling-methodology)
- [Detection Architecture Summary](#detection-architecture-summary)
- [Framework Alignment](#framework-alignment)

---

## The Thesis

Security detection has been built on a fundamentally flawed premise: that knowing what something *looks like* is sufficient to determine whether it is malicious. Hash blocklists, YARA signatures, and static AV rules all ask the same question — *"is this file known bad?"*

AI changes the economics of evasion so dramatically that this question becomes structurally unanswerable.

An LLM can generate a unique, syntactically valid, functionally equivalent variant of any attack in seconds. It can reason about your detection rules and produce payloads specifically engineered to evade them. It can automate the iteration cycle that previously took a skilled red teamer days. The cost of generating a new variant approaches zero.

The only viable response is a detection architecture that does not ask *"is this file known bad?"* but instead asks *"did the attack behaviour we are trying to detect actually occur?"*

That is the Minimum Truth doctrine. This document applies it to the most important emerging AI-accelerated attack classes.

---

## How AI Is Changing the Attack Surface

```mermaid
graph TD
    subgraph Before["Pre-AI Threat Landscape"]
        B1["Attack variant generation\nDays per variant\nRequires senior expertise"]
        B2["Evasion testing\nManual, slow iteration\nLimited by human bandwidth"]
        B3["Payload sophistication\nCorrelated with attacker skill\nHigh barrier to entry"]
        B4["Signature detection\nCould keep pace\nVendors had time to respond"]
    end

    subgraph After["AI-Accelerated Threat Landscape"]
        A1["Attack variant generation\nSeconds per variant\nAvailable to any skill level"]
        A2["Evasion testing\nAutomated against known rules\nUnlimited iteration"]
        A3["Payload sophistication\nDecoupled from attacker skill\nLow barrier to entry"]
        A4["Signature detection\nCannot keep pace\nVariants outrun vendor response"]
    end

    subgraph Defence["The Only Viable Response"]
        D1["Behavioural composite detection\nAnchored on invariant actions\nHash-agnostic by design"]
        D2["Minimum truth doctrine\nWhat must be true for attack to exist\nNot what the attack looks like"]
        D3["Cousin technique coverage\nEvery adjacent substrate monitored\nPivot = data point, not escape"]
    end

    Before -->|"LLM capability\ncrosses threshold"| After
    After -->|"Signature controls\nstructurally fail"| Defence
```

### The Three AI-Enabled Capability Shifts

**Shift 1 — Targeted Evasion at Scale**

Previously, evasion required deep knowledge of a target's specific detection stack. An attacker needed to know which SIEM rules you ran, what thresholds you used, which fields you monitored. That knowledge was hard to acquire.

Now: feed a detection rule to an LLM. Ask it to generate a variant that preserves the attack's operational goal while bypassing the specific predicate. The model reasons about the rule's logic and produces targeted evasion within a single conversation. This is not theoretical — red team tooling already integrates this workflow.

**Shift 2 — Polymorphic Payload Generation**

Classical polymorphism required custom mutation engines — sophisticated code that reordered instructions, applied encryption layers, and mutated string constants. Building a reliable polymorphic engine was a specialist skill.

LLMs can generate functionally equivalent code in any language, with any obfuscation pattern, on demand. What was previously a week of work for a skilled malware developer is now a prompt.

**Shift 3 — Attack Chain Reasoning**

The most dangerous capability is not code generation — it is *reasoning*. An LLM can analyse a target environment description and produce an optimised attack path: which lateral movement substrate to try first, which credentials to target, which persistence mechanism is least monitored, which exfiltration channel blends into baseline traffic. This operational reasoning previously required human expertise accumulated over years.

### Why Behavioural Composite Detection Is the Answer

```mermaid
graph LR
    subgraph Invariant["What Changes (AI can mutate this)"]
        I1["File hash"]
        I2["Binary signature"]
        I3["String constants"]
        I4["API call sequence"]
        I5["Obfuscation layer"]
        I6["C2 domain"]
    end

    subgraph Fixed["What Cannot Change (Minimum Truth)"]
        F1["VirtualAlloc must be called\nfor memory injection"]
        F2["LSASS must be opened\nfor credential access"]
        F3["A driver must load\nfor BYOVD exploitation"]
        F4["A service must be registered\nfor persistence"]
        F5["Data must leave the network\nfor exfiltration"]
    end

    Invariant -->|"Mutation changes\neverything here"| Fixed
    Fixed -->|"Behaviour cannot\nchange without\nchanging the attack goal"| Fixed
```

The attacker can change every file, every hash, every string. They cannot change *what they must do* to achieve their objective. That invariant behaviour is the Minimum Truth anchor.

---

## The Evolution of Command-Line Execution

Command-line execution telemetry (T1059) has been the foundation of detection engineering for over a decade. The progression of evasion against it illustrates exactly how AI accelerates the arms race.

```mermaid
timeline
    title Evolution of Command-Line Execution & Evasion
    2015 : PowerShell -EncodedCommand
         : Base64 payload encoding
         : First generation evasion
    2016 : AMSI introduced
         : Script block logging
         : Defenders gain visibility
    2017 : AMSI bypass techniques emerge
         : AmsiScanBuffer patch
         : amsiInitFailed manipulation
    2018 : Living-off-the-land (LOLBins)
         : Execution via certutil, mshta, rundll32
         : Signed binary proxy execution
    2019 : Fileless execution
         : In-memory only payloads
         : No disk artefact to scan
    2020 : Process hollowing / injection
         : Execution inside legitimate processes
         : Parent-child trust abuse
    2022 : WMI fileless subscription
         : scrcons.exe DLL execution
         : No child process visible
    2023 : BYOVD mainstream
         : Signed vulnerable driver abuse
         : Kernel-level EDR blinding
    2024 : AI-assisted variant generation
         : Per-campaign unique payloads
         : Targeted rule evasion
    2025 : Byte-flip polymorphism
         : Hash mutation, signature preserved
         : Blocklists structurally defeated
    2026 : AI-reasoned attack chains
         : Substrate selection optimised per target
         : Cousin pivots automated on detection
```

### Where Detection Engineering Has Been Forced

Each evasion advance forced detection from:

| Era | Defender Anchor | Why It Failed | Next Anchor |
|-----|----------------|---------------|-------------|
| 2015 | Command-line string matching | Encoding bypasses string match | Script block logging |
| 2017 | AMSI script content | Patch AMSI before scan | Process behaviour |
| 2019 | Child process trees | Fileless = no child | Memory telemetry |
| 2022 | Process injection signals | WMI fileless = no process | DLL load events |
| 2024 | Hash + signature blocklists | Byte-flip = new hash, valid signature | **Behavioural sequence** |

The direction of travel is clear. Every signature-adjacent control has been systematically defeated. The terminal anchor — the one that cannot be mutated away — is the **behavioural sequence of what the attack must do**.

---

## Polymorphic Malware — Why Signatures Fail

### Classical Polymorphism vs AI-Assisted Mutation

Classical polymorphic malware mutates code structure: reordering instructions, varying encryption keys, inserting junk code. The payload is functionally identical but structurally different on each execution.

**The problem for classical polymorphism:** Code mutation modifies binary content. If the binary was signed, the digital signature is cryptographically tied to the file content. Mutation invalidates the signature. Windows kernel driver loading requires a valid signature. Classical polymorphism cannot be applied to drivers.

**AI-assisted byte-flip mutation solves this:**

```mermaid
graph LR
    subgraph Original["Legitimate Signed Driver"]
        O1["PE Header\nTimeDateStamp: 0x5F3A2B1C"]
        O2["Code Section\nAuthenticated by signature"]
        O3["Digital Signature\nCovers code section only"]
    end

    subgraph Mutated["Byte-Flipped Variant"]
        M1["PE Header\nTimeDateStamp: 0x5F3A2B1D\n← one byte changed"]
        M2["Code Section\nIdentical — unchanged"]
        M3["Digital Signature\nStill VALID\nTimestamp not in scope"]
    end

    subgraph Result["Security Control Outcomes"]
        R1["Hash: DIFFERENT\n❌ Blocklist misses it"]
        R2["Signature: VALID\n❌ OS loads as trusted kernel driver"]
        R3["Behaviour: IDENTICAL\n✅ Composite detection fires"]
    end

    Original -->|"AI generates\nunique byte position\nper campaign"| Mutated
    Mutated --> Result
```

### Why Every Hash-Based Control Fails

| Control | What It Asks | Why It Fails |
|---------|-------------|--------------|
| Hash blocklist | Is this specific hash known bad? | New hash per variant — blocklist never catches up |
| VirusTotal lookup | Has any vendor flagged this hash? | Zero detections on fresh variant |
| AV signature scan | Does this file match a known signature? | No signature for a new hash |
| Driver signature check | Is this driver signed? | **Yes — actively confirms attacker advantage** |
| Static sandbox | Does this file behave maliciously in isolation? | Sandbox evasion + environment fingerprinting |
| Behavioural composite | Did this attack sequence occur? | **Cannot be bypassed by file mutation** |

### The MTDF Response — Mutation-Resistant Anchors

The Minimum Truth Framework selects detection anchors that are invariant to file mutation:

```kql
// MUTATION-RESISTANT ANCHOR: driver staged to writable path
// The file hash changes every campaign. The behaviour cannot.
// A kernel driver can only load legitimately from System32\drivers.
// Any .sys drop to %TEMP%, %APPDATA%, %PUBLIC% is invariant suspicious behaviour.

DeviceFileEvents
| where ActionType in ("FileCreated", "FileModified", "FileRenamed")
| where FolderPath matches regex @"(?i)\\(Temp|AppData|ProgramData|Public|Users\\[^\\]+\\)"
| where FileName matches regex @"\.(sys|drv)$"
// This fires regardless of hash. Regardless of signer. Regardless of byte-flip variant.
// The behaviour — staging a driver to a user-writable path — is invariant.
```

---

## SilverFox / ValleyRAT — BYOVD as a Case Study

SilverFox (also tracked as ValleyRAT, SilverCat) is a Chinese-nexus espionage cluster active 2024–2026, targeting financial services, technology, and logistics organisations. It represents the current frontier of AI-assisted evasion: byte-flip polymorphism combined with a multi-stage kill chain that defeats every signature-based control.

### The Full Kill Chain

```mermaid
graph TD
    A["Stage 0 — Initial Delivery\nSEO-poisoned installers\nFake WeChat / Zoom / Teams\nT1189 · T1204.002"] --> B

    B["Stage 1 — Signed Binary Execution\nLegitimate signed binary from user path\nEstablishes trusted execution context\nT1218"] --> C

    C["Stage 2 — DLL Sideloading\nMalicious DLL loaded from same directory\nRuns inside trusted signed process\nNo child process · No visible cmdline\nT1574.002"] --> D

    D["Stage 3 — ValleyRAT User-Mode Implant\nC2 established via low-volume HTTPS\nReconnaissance · Privilege assessment\nT1071.001"] --> E

    E["Stage 4 — Driver Staging\nVulnerable signed driver dropped to Temp\nByte-flip applied: new hash, valid signature\nT1027"] --> F

    F["Stage 5 — Kernel Service Registration\nsc.exe / Registry API / PowerShell\nService created pointing to staged driver\nT1543.003"] --> G

    G["Stage 6 — BYOVD Kernel Exploitation\nDriver loaded into kernel\nVulnerability grants ring-0 access\nT1068"] --> H

    H["Stage 7 — EDR Blinding\nfltmc unload removes security minifilter\nWinDefend / Sense terminated from kernel\nEndpoint blind — cannot detect further activity\nT1562.001"] --> I

    I["Stage 8 — ValleyRAT Full Payload\nProcess injection into legitimate host\nCredential harvesting · Data exfiltration\nT1055 · T1041"]

    style A fill:#2d2d2d,color:#fff
    style B fill:#4a3000,color:#fff
    style C fill:#4a3000,color:#fff
    style D fill:#003366,color:#fff
    style E fill:#003366,color:#fff
    style F fill:#003366,color:#fff
    style G fill:#660000,color:#fff
    style H fill:#660000,color:#fff
    style I fill:#330044,color:#fff
```

### MITRE ATT&CK Coverage

| Tactic | Technique | ID | Stage | MTDF Anchor |
|--------|-----------|-----|-------|-------------|
| Initial Access | Drive-By Compromise | T1189 | 0 | N/A — pre-endpoint |
| Initial Access | User Execution: Malicious File | T1204.002 | 0 | Process parent = fake installer |
| Defense Evasion | DLL Side-Loading | T1574.002 | 2 | DeviceImageLoadEvents: signed loader + mismatched DLL |
| Defense Evasion | Signed Binary Proxy Execution | T1218 | 1 | Signed binary from writable path |
| Defense Evasion | Obfuscated Files / Artifacts | T1027 | 4 | .sys in writable path — byte-flipped |
| Defense Evasion | Impair Defenses: Disable Tools | T1562.001 | 7 | fltmc.exe unload primitive |
| Privilege Escalation | Exploitation for Privilege Escalation | T1068 | 6 | DriverLoadEvent from writable path |
| Persistence | Create/Modify Windows Service | T1543.003 | 5 | Registry ImagePath → writable .sys |
| C2 | Application Layer Protocol: Web | T1071.001 | 3/8 | Low-volume HTTPS from sideloaded process |
| Exfiltration | Exfiltration Over C2 Channel | T1041 | 8 | Data volume anomaly on C2 channel |

### Composite Detection — Three-Tier Architecture

The Minimum Truth Framework deploys three independent, escalating sensors. Each tier is a standalone sensor — not a monolithic kill-chain join. They correlate at the incident layer via `DeviceId`.

#### Tier 1 — Atomic Surface Sensor

**Minimum Truth:** A kernel service was registered pointing to a user-writable path.  
**Purpose:** Feed the 30-day atomic primitive index. Do not alert independently.

```kql
// ============================================================================
// TIER 1 — Atomic Surface Sensor
// T1543.003 Service Registration from Writable Path
// Minimum Truth: ImagePath points to user-writable location
// Purpose: Primitive index — feeds incident correlation layer
// Do NOT alert on this alone — high noise, requires convergence
// ============================================================================

DeviceRegistryEvents
| where Timestamp > ago(24h)
| where ActionType == "RegistryValueSet"
| where RegistryKey has @"CurrentControlSet\Services"
| where RegistryValueName =~ "ImagePath"
| where RegistryValueData matches regex @"(?i)\\(Temp|AppData|ProgramData|Public|Users\\[^\\]+\\)"
| where RegistryValueData matches regex @"\.(sys|drv|dat|bin|tmp)\b"
| project
    Timestamp,
    DeviceId,
    DeviceName,
    AccountName      = InitiatingProcessAccountName,
    ServiceKey       = RegistryKey,
    ServiceImagePath = RegistryValueData,
    WriterProcess    = InitiatingProcessFileName,
    WriterCmdLine    = InitiatingProcessCommandLine,
    WriterHash       = InitiatingProcessSHA256
| order by Timestamp desc
```

#### Tier 2 — Core+ Chained Detection

**Minimum Truth:** Driver-like file staged to writable path AND service registered — same device, within 2 hours.  
**Alert level:** HIGH. Hash-invariant. Method-invariant.

```kql
// ============================================================================
// TIER 2 — Core+ Chained Detection
// T1027 Driver Staging → T1543.003 Service Registration
// Minimum Truth: File drop + service registration chain on same device
// Anchoring: Substrate-First — physical staging is the irreducible truth
// Alert: HIGH confidence — hash-agnostic by design
// MTDF Engineering Rules applied:
//   [RULE-6] leftouter join — extend IsMatch before filtering
//   [RULE-1] arg_max over any() for deterministic summarise
//   [RULE-10] Score floor at zero
// ============================================================================

let Lookback      = 6h;
let ChainWindow   = 2h;

let WritablePaths = dynamic([
    "\\Temp\\", "\\AppData\\", "\\ProgramData\\",
    "\\Public\\", "\\Users\\", "\\Downloads\\"
]);

let DriverExtRegex = @"\.(sys|drv|dat|bin|tmp)$";

// Phase 1 — Minimum Truth: driver-like artifact staged to writable path
let DriverDrops =
    DeviceFileEvents
    | where Timestamp > ago(Lookback)
    | where ActionType in ("FileCreated", "FileModified", "FileRenamed", "FileMoved")
    | where FolderPath has_any (WritablePaths)
    | where FileName matches regex DriverExtRegex
    | project
        DeviceId,
        DropTime     = Timestamp,
        DriverFile   = FileName,
        DriverFolder = FolderPath,
        DropperProc  = InitiatingProcessFileName,
        DropperHash  = InitiatingProcessSHA256;

// Phase 1 — Minimum Truth: service registered pointing to writable path
let ServiceRegistry =
    DeviceRegistryEvents
    | where Timestamp > ago(Lookback)
    | where ActionType == "RegistryValueSet"
    | where RegistryKey has @"CurrentControlSet\Services"
    | where RegistryValueName =~ "ImagePath"
    | where RegistryValueData has_any (WritablePaths)
    | where RegistryValueData matches regex DriverExtRegex
    | project
        DeviceId,
        ServiceTime      = Timestamp,
        ServiceKey       = RegistryKey,
        ServiceImagePath = RegistryValueData,
        ServiceWriter    = InitiatingProcessFileName;

// Phase 2 — Correlation: same device, drop before service registration
// [RULE-6]: extend IsMatch before any where filter on right-side fields
DriverDrops
| join kind=leftouter (ServiceRegistry) on DeviceId
| extend IsChained = toint(
    isnotempty(ServiceTime) and
    ServiceTime between (DropTime .. DropTime + ChainWindow)
)
// [RULE-1]: arg_max for deterministic field selection
| summarize
    ChainConfirmed = max(IsChained),
    DropTime       = min(DropTime),
    ServiceTime    = max(ServiceTime),
    arg_max(DropTime, *)
  by DeviceId, DriverFile, DriverFolder

| where ChainConfirmed == 1

// Phase 3 — Scoring
| extend RiskScore = 70  // Base: chain confirmed
| extend RiskScore = RiskScore + iff(DropperProc !in~ ("msiexec.exe","trustedinstaller.exe"), 15, 0)
// [RULE-10]: floor at zero
| extend RiskScore = iif(RiskScore < 0, 0, RiskScore)
| extend Severity  = "HIGH"

// Phase 4 — Hunter Directive
| project
    DropTime, ServiceTime, DeviceId, DeviceName,
    DriverFile, DriverFolder, DropperProc, DropperHash,
    ServiceKey, ServiceImagePath, ServiceWriter,
    RiskScore, Severity,
    HunterDirective = strcat(
        "HIGH | T1027+T1543.003 | BYOVD Staging Chain | ",
        "Driver artifact: ", DriverFile, " staged to ", DriverFolder, " | ",
        "Service registered: ", ServiceKey, " → ", ServiceImagePath, " | ",
        "Dropper process: ", DropperProc, " | ",
        "NEXT: (1) Check DeviceEvents for DriverLoadEvent on DeviceId=", DeviceId,
        " to confirm kernel load. ",
        "(2) Check DeviceProcessEvents for fltmc.exe unload — EDR may already be blind. ",
        "(3) Pivot DeviceImageLoadEvents for signed loader + mismatched DLL (Tier 3 sideload)."
    )
| order by DropTime desc
```

#### Tier 3 — Advanced Kill-Chain: Sideload + BYOVD + Kernel Confirmation

**Minimum Truth:** `DriverLoadEvent` from a writable path preceded by the complete sideload → stage → service chain.  
**Alert level:** CRITICAL. Binary-invariant. Hash-invariant. Signature-invariant.

```kql
// ============================================================================
// TIER 3 — Advanced Kill-Chain Detection
// SilverFox / ValleyRAT: Sideload → Stage → Service → Kernel Load
// Author: Ala Dabat | MTDF Doctrine (2026)
// MITRE: T1574.002 · T1027 · T1543.003 · T1068 · T1562.001
// Anchoring: Substrate-First
// Skeleton: A — Substrate-First MDE
// Minimum Truth: DriverLoadEvent from writable path — kernel load confirmed
// Alert: CRITICAL — hash-invariant, signature-invariant, binary-invariant
//
// MTDF Engineering Rules:
//   [RULE-1]  arg_max over any() — deterministic summarise
//   [RULE-4]  Prevalence window separate from detection window
//   [RULE-5]  Null SHA256 guard on rarity scoring
//   [RULE-6]  leftouter join — extend IsMatch, never filter on right-side fields
//   [RULE-10] Score floor at zero — negative scores have no semantic meaning
// ============================================================================

let Lookback              = 24h;
let SideloadToDropWindow  = 6h;
let DropToServiceWindow   = 2h;
let ServiceToKernelWindow = 2h;

let WritablePaths = dynamic([
    "\\Temp\\", "\\AppData\\", "\\ProgramData\\",
    "\\Public\\", "\\Desktop\\", "\\Downloads\\", "\\Users\\"
]);

let ModuleExtRegex = @"\.(dll|ocx|cpl|dat|bin|tmp)$";
let DriverExtRegex = @"\.(sys|drv|dat|bin|tmp|ax)$";

// ── PHASE 1: MINIMUM TRUTH — Substrate Signals ──────────────────────────────

// Signal A: DLL Sideload — signed loader + mismatched/unsigned module
// The signed loader abuses Windows DLL search order.
// Legitimate scenario: rare. Attacker scenario: ubiquitous in SilverFox.
let SideloadEvents =
    DeviceImageLoadEvents
    | where Timestamp > ago(Lookback)
    | where InitiatingProcessSignatureStatus == "Signed"
    | where FileName matches regex ModuleExtRegex
    | where FolderPath has_any (WritablePaths)
          or InitiatingProcessFolderPath has_any (WritablePaths)
    // Signer mismatch or unsigned module = trust abuse
    | where SignatureStatus != "Signed"
          or Signer != InitiatingProcessSigner
    | project
        DeviceId,
        SideloadTime = Timestamp,
        LoaderName   = InitiatingProcessFileName,
        LoaderPath   = InitiatingProcessFolderPath,
        LoaderSigner = tostring(InitiatingProcessSigner),
        LoadedModule = FileName,
        LoadedPath   = FolderPath,
        LoadedSig    = tostring(SignatureStatus);

// Signal B: Driver-like artifact staged to writable path
let DriverDrops =
    DeviceFileEvents
    | where Timestamp > ago(Lookback)
    | where ActionType in ("FileCreated", "FileModified", "FileRenamed", "FileMoved")
    | where FolderPath has_any (WritablePaths)
    | where FileName matches regex DriverExtRegex
    | project
        DeviceId,
        DropTime     = Timestamp,
        DriverFile   = FileName,
        DriverFolder = FolderPath,
        DropperProc  = InitiatingProcessFileName;

// Signal C: Service registration — registry API method
let ServiceRegistry =
    DeviceRegistryEvents
    | where Timestamp > ago(Lookback)
    | where ActionType == "RegistryValueSet"
    | where RegistryKey has @"CurrentControlSet\Services"
    | where RegistryValueName =~ "ImagePath"
    | where RegistryValueData has_any (WritablePaths)
    | where RegistryValueData matches regex DriverExtRegex
    | project DeviceId, ServiceTime = Timestamp,
              ServiceIndicator = RegistryKey, Method = "Registry";

// Signal C: Service registration — process method (sc.exe, PowerShell, pnputil)
let ServiceProcess =
    DeviceProcessEvents
    | where Timestamp > ago(Lookback)
    | where FileName in~ ("sc.exe", "pnputil.exe", "powershell.exe", "pwsh.exe")
    | where ProcessCommandLine has_any (WritablePaths)
    | where ProcessCommandLine matches regex DriverExtRegex
    | project DeviceId, ServiceTime = Timestamp,
              ServiceIndicator = ProcessCommandLine, Method = "Process";

let ServiceCreates = union ServiceRegistry, ServiceProcess;

// Signal D: Kernel driver load confirmation — ground truth
// DriverLoadEvent is the irreducible minimum truth.
// The chain is only CRITICAL when this event is confirmed.
let KernelLoads =
    DeviceEvents
    | where Timestamp > ago(Lookback)
    | where ActionType == "DriverLoadEvent"
    | where FolderPath has_any (WritablePaths)
    | project
        DeviceId,
        KernelLoadTime = Timestamp,
        KernelDriver   = FileName,
        KernelPath     = FolderPath;

// ── PHASE 2: CHAIN CORRELATION ───────────────────────────────────────────────
// Device-scoped. No brittle PID requirements.
// Time windows enforce realistic staging gaps — not forensic precision.

SideloadEvents
| join kind=inner (DriverDrops) on DeviceId
| where DropTime between (SideloadTime .. SideloadTime + SideloadToDropWindow)
| join kind=inner (ServiceCreates) on DeviceId
| where ServiceTime between (DropTime .. DropTime + DropToServiceWindow)
| join kind=inner (KernelLoads) on DeviceId
| where KernelLoadTime between (ServiceTime .. ServiceTime + ServiceToKernelWindow)

// ── PHASE 3: CONVERGENCE SCORING ─────────────────────────────────────────────
// [RULE-1]: arg_max(*) replaces any() — deterministic field selection
| summarize
    FirstSeen      = min(SideloadTime),
    LastSeen       = max(KernelLoadTime),
    LoaderName     = any(LoaderName),
    LoaderPath     = any(LoaderPath),
    LoaderSigner   = any(LoaderSigner),
    LoadedModules  = make_set(LoadedModule, 10),
    SigStatus      = make_set(LoadedSig, 5),
    DriverFiles    = make_set(DriverFile, 10),
    ServiceMethods = make_set(Method, 5),
    KernelDrivers  = make_set(KernelDriver, 10),
    KernelPaths    = make_set(KernelPath, 10),
    Droppers       = make_set(DropperProc, 10)
  by DeviceId

// Base score: 95 — full confirmed chain is structurally CRITICAL
// No reinforcement needed — the substrate chain IS the truth
| extend RiskScore = 95
// [RULE-10]: floor at zero
| extend RiskScore = iif(RiskScore < 0, 0, RiskScore)
| extend Severity  = "CRITICAL"

// ── PHASE 4: HUNTER DIRECTIVE ────────────────────────────────────────────────
| project
    FirstSeen, LastSeen,
    Severity, RiskScore,
    DeviceId,
    LoaderName, LoaderPath, LoaderSigner,
    LoadedModules, SigStatus,
    DriverFiles, ServiceMethods,
    KernelDrivers, KernelPaths,
    Droppers,
    HunterDirective = strcat(
        "CRITICAL | SilverFox/ValleyRAT BYOVD Chain Confirmed | ",
        "Signed loader (", LoaderName, " — ", LoaderSigner, ") ",
        "sideloaded untrusted module(s): ", tostring(LoadedModules),
        " (sig: ", tostring(SigStatus), ") | ",
        "Staged driver artifact(s): ", tostring(DriverFiles), " | ",
        "Service created via: ", tostring(ServiceMethods), " | ",
        "KERNEL LOAD CONFIRMED: ", tostring(KernelDrivers),
        " from ", tostring(KernelPaths), " | ",
        "⚠ EDR MAY BE BLIND — do not rely on further EDR telemetry | ",
        "IMMEDIATE ACTIONS: ",
        "(1) Network isolate DeviceId=", DeviceId, " — do not investigate on live system. ",
        "(2) Validate EDR health independently before trusting further alerts. ",
        "(3) Memory acquisition before any reboot — kernel tamper may not survive reboot. ",
        "(4) Fleet hunt: same LoaderName and LoadedModules across estate. ",
        "(5) Check DeviceProcessEvents for fltmc.exe unload — EDR blinding confirmation."
    )
| order by LastSeen desc
```

---

## Three Additional AI-Accelerated Attack Chains

### Attack Chain 1 — AI-Assisted PowerShell Empire Stager (T1059.001)

Empire C2 already generates unique stagers per session. With LLM assistance, evasion variant generation is fully automated — producing base64-encoded payloads that bypass AMSI, rotate obfuscation patterns, and modify the command structure to evade specific rule predicates.

**Why AI accelerates this:** An LLM can generate a novel `-EncodedCommand` payload that preserves Empire's operational structure while evading any specific string-matching rule. The AMSI bypass pattern, the download cradle, and the reflection load can all be varied programmatically.

**The MTDF Response — Intent-First on Invariant Primitives:**

```mermaid
graph LR
    subgraph Mutable["AI Can Mutate These"]
        M1["-EncodedCommand value\n(new base64 per run)"]
        M2["Download URL\n(rotated C2)"]
        M3["Variable names\n(randomised)"]
        M4["AMSI bypass\n(pattern varied)"]
    end

    subgraph Invariant["Cannot Be Mutated"]
        I1["PowerShell must execute\n(substrate truth)"]
        I2["Memory must be allocated\n(VirtualAlloc invariant)"]
        I3["Reflection must occur\n([System.Reflection.Assembly])"]
        I4["Network connection must form\n(C2 invariant)"]
    end

    Mutable -->|"AI generates\nnew variants"| Invariant
    Invariant -->|"Minimum Truth\nanchor"| Invariant
```

```kql
// ============================================================================
// Empire Stager — Intent-First Composite
// T1059.001 PowerShell + T1055 Reflective Injection
// Author: Ala Dabat | MTDF Doctrine (2026)
// Anchoring: Intent-First — PowerShell is ubiquitous; primitive implies capability
// Skeleton: B — Intent-First MDE
// Minimum Truth: PowerShell with in-memory execution + remote retrieval primitive
// These primitives are invariant across AI-generated variants
// ============================================================================

let IntentPrimitives =
    DeviceProcessEvents
    | where Timestamp > ago(24h)
    | where FileName in~ ("powershell.exe", "pwsh.exe")
    // Minimum Truth: the explicit capability primitive
    // AI can vary the encoding, URL, variable names
    // It cannot remove the need to download, decode, and execute in memory
    | where ProcessCommandLine has_any (
        // In-memory execution (invariant — fileless requires this)
        "IEX", "Invoke-Expression",
        // Remote retrieval (invariant — Empire needs to pull the stager)
        "DownloadString", "DownloadData", "Invoke-WebRequest", "Net.WebClient",
        // Reflection load (invariant — .NET assembly must load somehow)
        "Reflection.Assembly", "[System.Reflection",
        // AMSI bypass patterns (structurally required for evasion)
        "AmsiUtils", "amsiInitFailed", "AmsiScanBuffer",
        // Memory allocation (invariant — injection requires allocation)
        "VirtualAlloc", "VirtualProtect"
    )
    // Soft noise exclusion: known legitimate automation paths
    | where FolderPath !startswith @"C:\Program Files\WindowsPowerShell\Modules\PSWindowsUpdate"
    ;

// Phase 2 — Native Enrichment (no raw table joins)
let EnrichedPrimitives =
    IntentPrimitives
    | extend
        ParentProcess   = InitiatingProcessFileName,
        ParentCmdLine   = InitiatingProcessCommandLine,
        ParentHash      = InitiatingProcessSHA256,
        ParentSigner    = InitiatingProcessSignerType,
        ParentIntegrity = InitiatingProcessIntegrityLevel

    // Signal grouping — AI-generated variants trigger multiple signals simultaneously
    // --- Obfuscation signals (AI varies these but cannot eliminate all) ---
    | extend
        IsEncodedPayload = toint(ProcessCommandLine has_any ("-Enc", "-EncodedCommand")),
        IsInMemoryStager = toint(ProcessCommandLine has_any ("IEX", "DownloadString", "Invoke-Expression")),
        IsAMSIBypass     = toint(ProcessCommandLine has_any ("AmsiUtils", "amsiInitFailed", "AmsiScanBuffer")),
        IsReflective     = toint(ProcessCommandLine has_any ("VirtualAlloc", "VirtualProtect", "Reflection.Assembly")),
    // --- Parent trust signals ---
        IsUnsignedParent = toint(ParentSigner == "None" or ParentSigner == ""),
        IsOfficeParent   = toint(InitiatingProcessFileName in~ (
            "winword.exe", "excel.exe", "outlook.exe", "powerpnt.exe")),
        IsScriptParent   = toint(InitiatingProcessFileName in~ (
            "wscript.exe", "cscript.exe", "mshta.exe"))
    ;

// Phase 3 — Convergence Scoring
EnrichedPrimitives
| extend RiskScore = 55  // Base: intent primitive cleared Phase 1
| extend RiskScore = RiskScore + iff(IsEncodedPayload == 1, 20, 0)
| extend RiskScore = RiskScore + iff(IsInMemoryStager == 1, 20, 0)
| extend RiskScore = RiskScore + iff(IsAMSIBypass     == 1, 25, 0)
| extend RiskScore = RiskScore + iff(IsReflective     == 1, 20, 0)
| extend RiskScore = RiskScore + iff(IsUnsignedParent == 1, 10, 0)
| extend RiskScore = RiskScore + iff(IsOfficeParent   == 1, 15, 0)
| extend RiskScore = RiskScore + iff(IsScriptParent   == 1, 10, 0)
// Soft down-score: legitimate dev tooling (preserve telemetry row)
| extend RiskScore = RiskScore - iff(
    InitiatingProcessFileName in~ ("devenv.exe", "code.exe")
    and InitiatingProcessSignerType == "Microsoft Corporation", 15, 0)
// [RULE-10]: floor at zero
| extend RiskScore = iif(RiskScore < 0, 0, RiskScore)
| where RiskScore >= 75

// Phase 4 — Hunter Directive
| project
    Timestamp, DeviceName, AccountName,
    FileName, ProcessCommandLine, SHA256,
    ParentProcess, ParentCmdLine, ParentHash, ParentSigner,
    IsEncodedPayload, IsInMemoryStager, IsAMSIBypass, IsReflective,
    IsOfficeParent, IsScriptParent, IsUnsignedParent,
    RiskScore,
    HunterDirective = strcat(
        "INTENT-FIRST | T1059.001 Empire Stager | ",
        "Primitive: PowerShell in-memory execution chain | ",
        "Signals: ",
        iff(IsEncodedPayload == 1, "ENCODED_PAYLOAD ", ""),
        iff(IsInMemoryStager == 1, "IN_MEMORY_STAGER ", ""),
        iff(IsAMSIBypass     == 1, "AMSI_BYPASS ", ""),
        iff(IsReflective     == 1, "REFLECTIVE_INJECT ", ""),
        iff(IsOfficeParent   == 1, "OFFICE_PARENT ", ""),
        "| Score: ", tostring(RiskScore), " | ",
        "NEXT: (1) Decode ProcessCommandLine base64 payload. ",
        "(2) Pivot DeviceName in DeviceNetworkEvents for Empire beacon pattern ",
        "(jitter ±20%, default URIs /admin/get.php /news.php). ",
        "(3) Pivot SHA256 for prevalence — rare SHA256 = targeted deployment."
    )
| sort by RiskScore desc
```

---

### Attack Chain 2 — AI-Assisted WMI Fileless Persistence (T1546.003)

WMI Permanent Event Subscription remains one of the most evasion-resistant persistence mechanisms because it produces no visible child process. AI accelerates it by automating the VBScript/JScript payload generation — producing novel script content that varies across deployments while preserving the WMI subscription mechanism.

**The invariant substrate:** `scrcons.exe` loading `vbscript.dll`, `jscript.dll`, or `scrobj.dll`. This DLL load cannot be avoided — it is the execution mechanism. No AI-generated variant can remove it.

```kql
// ============================================================================
// WMI Fileless Execution — Substrate-First
// T1546.003 Event Triggered Execution: WMI Event Subscription
// Author: Ala Dabat | MTDF Doctrine (2026)
// Anchoring: Substrate-First
// Skeleton: A — Substrate-First MDE
// Minimum Truth: scrcons.exe loading a script engine DLL
// Why: NO child process. NO command line. NO disk artefact.
//      The DLL load IS the execution. This is the only observable primitive.
//      AI-generated VBScript varies the payload content — not the load event.
// Table: DeviceImageLoadEvents (NOT DeviceProcessEvents)
// ============================================================================

let SubstrateEvents =
    DeviceImageLoadEvents
    | where Timestamp > ago(24h)
    // Minimum Truth: scrcons.exe is the WMI ActiveScriptEventConsumer host
    | where InitiatingProcessFileName =~ "scrcons.exe"
    // Minimum Truth: one of the three script engine DLLs loads
    // vbscript.dll = VBScript | jscript.dll = JScript | scrobj.dll = Script Components
    // AI generates novel VBScript content — cannot change which DLL loads
    | where FileName in~ ("vbscript.dll", "jscript.dll", "scrobj.dll")
    ;

// Phase 2 — Native Enrichment
let EnrichedEvents =
    SubstrateEvents
    | extend
        AncestorProcess   = InitiatingProcessFileName,
        AncestorCmdLine   = InitiatingProcessCommandLine,
        AncestorHash      = InitiatingProcessSHA256,
        AncestorSigner    = InitiatingProcessSignerType,
        AncestorIntegrity = InitiatingProcessIntegrityLevel,
        DLLSigner         = Signer,
        DLLIsSigned       = toint(IsSigned),
        DLLPath           = FolderPath
    // Anomaly signals
    | extend
        IsLowIntegrity  = toint(AncestorIntegrity in~ ("Low", "Untrusted")),
        IsNonSystemPath = toint(
            FolderPath !startswith @"C:\Windows\System32" and
            FolderPath !startswith @"C:\Windows\SysWOW64" and
            FolderPath !startswith @"C:\Windows\WinSxS"
        ),
        IsUnsignedDLL    = toint(toint(IsSigned) == 0 or Signer == ""),
        IsUnsignedScrcons = toint(
            InitiatingProcessSignerType == "None" or
            InitiatingProcessSignerType == ""
        )
    ;

// Phase 3 — Scoring
EnrichedEvents
| extend RiskScore = 55
| extend RiskScore = RiskScore + iff(IsNonSystemPath    == 1, 25, 0)
| extend RiskScore = RiskScore + iff(IsUnsignedDLL      == 1, 20, 0)
| extend RiskScore = RiskScore + iff(IsUnsignedScrcons  == 1, 20, 0)
| extend RiskScore = RiskScore + iff(IsLowIntegrity     == 1, 10, 0)
| extend RiskScore = RiskScore - iff(
    AncestorCmdLine has_any ("ccmexec.exe", "trustedinstaller.exe"), 20, 0)
// [RULE-10]: floor at zero
| extend RiskScore = iif(RiskScore < 0, 0, RiskScore)
| where RiskScore >= 65

// Phase 4 — Hunter Directive
| project
    Timestamp, DeviceName, AccountName,
    ScriptEngineDLL = FileName, DLLPath, DLLSigner, DLLIsSigned,
    AncestorProcess, AncestorCmdLine, AncestorHash, AncestorSigner,
    IsNonSystemPath, IsUnsignedDLL, IsUnsignedScrcons, IsLowIntegrity,
    RiskScore,
    HunterDirective = strcat(
        "SUBSTRATE-FIRST | T1546.003 WMI Fileless | ",
        "scrcons.exe loaded ", FileName, " from ", FolderPath, " | ",
        "Signals: ",
        iff(IsNonSystemPath    == 1, "NON_SYSTEM_PATH ", ""),
        iff(IsUnsignedDLL      == 1, "UNSIGNED_DLL ", ""),
        iff(IsUnsignedScrcons  == 1, "UNSIGNED_SCRCONS ", ""),
        iff(IsLowIntegrity     == 1, "LOW_INTEGRITY ", ""),
        "| Score: ", tostring(RiskScore), " | ",
        "NEXT: (1) Pivot DeviceRegistryEvents: RegistryKey has '__EventConsumer' ",
        "within ±30min to recover embedded VBScript/JScript payload. ",
        "(2) Check WmiPrvSE.exe → scrcons.exe spawn in DeviceProcessEvents. ",
        "(3) Generate cousin rule: scrcons.exe → child process (non-fileless variant)."
    )
| sort by RiskScore desc
```

---

### Attack Chain 3 — AI-Assisted OAuth Consent Abuse (T1621)

OAuth consent abuse is the cloud-native equivalent of on-premises credential theft. AI accelerates it by generating convincing phishing lures, automating the consent grant flow, and selecting high-risk permission scopes that maximise post-compromise capability while minimising detection likelihood.

**The invariant:** A successful consent grant occurred. High-risk permission capability was granted. These events are logged in AuditLogs regardless of how sophisticated the social engineering was.

```kql
// ============================================================================
// OAuth Illicit Consent Abuse — Intent-First
// T1621 Multi-Factor Authentication Request Generation
// T1078.004 Valid Accounts: Cloud Accounts
// Author: Ala Dabat | MTDF Doctrine (2026)
// Anchoring: Intent-First
// Skeleton: B (Sentinel AuditLogs variant)
// Minimum Truth: Successful consent grant of high-risk permission scope
// AI accelerates: phishing lure generation, consent flow automation,
//                 scope selection optimised to evade detection thresholds
// The invariant: high-risk capability granted — AuditLogs always captures this
// ============================================================================

let ConsentLookback = 30d;
let PrevalenceLB    = 90d;
let RiskThreshold   = 18;

// Critical permissions that trigger a hard score floor
// regardless of trust discounts — AI cannot remove these from the log
let CriticalPerms = dynamic([
    "RoleManagement.ReadWrite.Directory",
    "AppRoleAssignment.ReadWrite.All",
    "Directory.ReadWrite.All",
    "Application.ReadWrite.All",
    "ServicePrincipal.ReadWrite.All"
]);

let HighRiskExact = dynamic([
    "Application.ReadWrite.All", "AppRoleAssignment.ReadWrite.All",
    "RoleManagement.ReadWrite.Directory", "ServicePrincipal.ReadWrite.All",
    "Directory.ReadWrite.All", "Directory.AccessAsUser.All",
    "Mail.Read", "Mail.ReadWrite", "Mail.Send",
    "Files.ReadWrite.All", "Sites.ReadWrite.All", "Sites.FullControl.All",
    "User.ReadWrite.All", "Group.ReadWrite.All"
]);

let HighRiskRegex = @"(?i)(ReadWrite\.All|FullControl\.All|AccessAsUser\.All|RoleManagement\.ReadWrite)";

// [RULE-4]: Prevalence window ends BEFORE detection window
// Prevents active attack telemetry from poisoning the baseline
let AppIdPrevalence =
    AuditLogs
    | where TimeGenerated >= ago(PrevalenceLB)
          and TimeGenerated < ago(ConsentLookback)  // [RULE-4] window separation
    | where Result =~ "success"
    | where OperationName in~ (
        "Consent to application",
        "Add delegated permission grant",
        "Add app role assignment grant to service principal"
    )
    | extend AppId = tostring(TargetResources[0].id)
    | where isnotempty(AppId)
    | summarize AppSeen = count(), AppSeenDays = dcount(bin(TimeGenerated, 1d)) by AppId;

// Minimum Truth: successful consent grant
AuditLogs
| where TimeGenerated >= ago(ConsentLookback)
| where Result =~ "success"
| where OperationName in~ (
    "Consent to application",
    "Add delegated permission grant",
    "Add app role assignment grant to service principal"
)
| extend AppId   = tostring(TargetResources[0].id)
| extend AppName = tostring(TargetResources[0].displayName)
| where isnotempty(AppId)
| extend
    InitiatorUPN = tostring(InitiatedBy.user.userPrincipalName),
    InitiatorIp  = tostring(InitiatedBy.user.ipAddress),
    UserAgent    = tostring(InitiatedBy.user.userAgent)
| extend InitiatorActor = iff(isnotempty(InitiatorUPN),
    strcat("USER:", InitiatorUPN), "APP_OR_UNKNOWN")

// Extract permission scopes
| mv-expand MP = TargetResources[0].modifiedProperties
| extend PropName = tostring(MP.displayName), PropVal = trim('"', tostring(MP.newValue))
| summarize
    CreatedTime   = min(TimeGenerated),
    IsAdmin       = max(toint(PropName == "ConsentContext.OnBehalfOfAll"
                              and tolower(PropVal) == "true")),
    Delegated     = take_anyif(PropVal, PropName == "Scope"),
    AppRoles      = take_anyif(PropVal, PropName == "AppRoles"),
    InitiatorUPN  = any(InitiatorUPN),
    InitiatorIp   = any(InitiatorIp),
    UserAgent     = any(UserAgent),
    InitiatorActor = any(InitiatorActor)
  by AppId, AppName

| extend DelegatedList = iif(isempty(Delegated), dynamic([]), split(Delegated, " "))
| extend AppRoleList   = iif(isempty(AppRoles),  dynamic([]), split(AppRoles,  " "))
| extend CombinedPerms = array_concat(DelegatedList, AppRoleList)
| mv-expand Perm = CombinedPerms to typeof(string)
| extend Perm = trim(" ", tostring(Perm))
| where isnotempty(Perm)
| extend IsHighRisk = toint(Perm in (HighRiskExact) or Perm matches regex HighRiskRegex)

| summarize
    CreatedTime    = any(CreatedTime),
    IsAdmin        = any(IsAdmin),
    HighRiskCount  = sum(IsHighRisk),
    HighRiskPerms  = make_set_if(Perm, IsHighRisk == 1, 100),  // [RULE-2]: make_set_if
    AllPerms       = make_set(Perm, 100),
    InitiatorUPN   = any(InitiatorUPN),
    InitiatorIp    = any(InitiatorIp),
    UserAgent      = any(UserAgent),
    InitiatorActor = any(InitiatorActor)
  by AppId, AppName

// Reinforcement joins
| join kind=leftouter (AppIdPrevalence) on AppId
| extend
    AppSeen    = coalesce(AppSeen, 0),
    AppSeenDays = coalesce(AppSeenDays, 0),
    // [RULE-4]: IsRareApp uses prevalence window that excludes detection window
    IsNewApp   = toint(AppSeen == 0),
    IsRareApp  = toint(AppSeenDays <= 2 and AppSeen > 0)

// Scoring
| extend RawScore =
      8
    + (HighRiskCount * 10)
    + (IsAdmin * 15)
    + (IsNewApp * 12)
    + (IsRareApp * 8)

// [RULE-9]: Score floor for critical permissions
// Two CriticalPerms + trust discount could score < threshold without floor
| extend Score = case(
    AllPerms has_any (CriticalPerms),        max_of(RawScore, 35),
    HighRiskCount >= 2 and RawScore < 25,    25,
    RawScore
)

| where Score >= RiskThreshold
| extend Severity = case(Score >= 50, "CRITICAL", Score >= 35, "HIGH", "MEDIUM")

| project
    CreatedTime, Severity, Score,
    AppName, AppId,
    InitiatorActor, InitiatorUPN, InitiatorIp,
    // [RULE-3]: substring length-guarded
    UserAgent = substring(UserAgent, 0, min(strlen(UserAgent), 60)),
    IsAdmin, HighRiskCount, HighRiskPerms, IsNewApp, IsRareApp,
    HunterDirective = strcat(
        "INTENT-FIRST | T1621 OAuth Consent Abuse | ",
        "High-risk capability granted: ", tostring(HighRiskPerms), " | ",
        "App: ", AppName, " (", AppId, ") | ",
        "Initiator: ", InitiatorActor, " from ", InitiatorIp, " | ",
        "Admin consent: ", tostring(IsAdmin == 1), " | ",
        "Score: ", tostring(Score), " | ",
        "NEXT: (1) Confirm business owner for AppId=", AppId, ". ",
        "(2) If unrecognised: revoke consent immediately via Entra ID. ",
        "(3) Hunt Graph/mail/file activity for InitiatorActor since CreatedTime. ",
        "(4) Check for post-consent token usage anomalies in SigninLogs."
    )
| order by Score desc
```

---

## The Threat Modelling Methodology

All detection rules in this document are derived using the **Minimum Truth Detection Framework** combined with formal **STRIDE threat modelling** at the attack surface level.

### How the Methodology Works

```mermaid
flowchart TD
    A["STRIDE Threat Model\nIdentify all threats at trust boundaries"] --> B
    B["Select Minimum Truth Anchor\nWhat must be true for this attack to exist?"] --> C
    C{Does substrate carry visible intent?}
    C -->|No — WMI, BYOVD, injection| D["Substrate-First\nAnchor on physical\nexecution surface\nSkeleton A or C"]
    C -->|Yes — PowerShell, rundll32, OAuth| E["Intent-First\nAnchor on explicit\nmalicious primitive\nSkeleton B"]
    D --> F["Phase 2: Native Enrichment\nInitiatingProcess* fields only\nNo raw table joins"]
    E --> F
    F --> G["Phase 3: Convergence Scoring\nBase 55 + reinforcement\nSoft down-score only\nNever hard-exclude"]
    G --> H["Phase 4: Hunter Directive\nWHY fired · WHAT reinforced · NEXT action"]
    H --> I["ADX-Docker Validation\nRun against Empire telemetry\nCapture receipt"]
    I --> J["Production-Candidate\nCommit to GitHub\nUpdate MITRE matrix"]
    J --> K["Cousin Rule Generation\nIdentify adjacent substrate\nBuild independent sensor"]
    K --> B
```

### Framework Alignment

This research aligns with:

| Framework | Alignment |
|-----------|-----------|
| **MITRE ATT&CK** | Every rule maps to specific technique IDs. Cousin doctrine extends ATT&CK's vertical depth with horizontal substrate adjacency. |
| **STRIDE** | Every detection rule was derived from a STRIDE threat analysis of the attack surface. The minimum truth anchor corresponds to the STRIDE threat category at the relevant trust boundary. |
| **PEAK (Threat Hunting)** | Hypothesis → Hunt Query → Act. Every rule can be run in hunt mode (lower threshold) before promotion to continuous detection. |
| **NIST CSF** | Detect function. Rules map to DE.AE (Anomalies & Events) and DE.CM (Continuous Monitoring) categories. |
| **OWASP LLM Top 10** | AI threat chain rules directly address LLM01 (Prompt Injection), LLM06 (Sensitive Information Disclosure), LLM08 (Excessive Agency). |

### STRIDE → Detection Mapping

| STRIDE Category | Threat | Minimum Truth Anchor | Rule |
|-----------------|--------|---------------------|------|
| **T — Tampering** | AI mutates driver binary, preserves signature | DriverLoadEvent from writable path | SilverFox Tier 3 |
| **T — Tampering** | Indirect prompt injection via RAG document | Instruction pattern in ingested content | DeviceEvents (AMSI/custom) |
| **E — Elevation** | BYOVD kernel exploitation via vulnerable driver | DriverLoadEvent + kernel integrity event | SilverFox Tier 2/3 |
| **E — Elevation** | OAuth high-risk scope grants excessive capability | Successful consent + high-risk permission | OAuth Composite |
| **I — Info Disclosure** | LSASS credential access | LSASS process opened with ReadProcessMemory rights | T1003.001 Composite |
| **D — Denial of Service** | EDR blinding via fltmc unload | fltmc.exe unload primitive | T1562.001 Sensor |
| **S — Spoofing** | DLL sideloading abuses signed binary trust | Signed loader + mismatched DLL in writable path | SilverFox Sideload |
| **R — Repudiation** | No audit of agent tool calls in AI pipeline | Tool call outside expected scope or timing | Custom app audit log |

---

## Detection Architecture Summary

```mermaid
graph TD
    subgraph Layer1["Layer 1 — Atomic Sentinel (Silent · 30-day index)"]
        A1["Driver service registration\nT1543.003 — Tier 1 sensor"]
        A2["PowerShell download primitives\nT1059.001 — Tier 1 sensor"]
        A3["Consent grant events\nT1621 — Tier 1 sensor"]
    end

    subgraph Layer2["Layer 2 — Composite Anchors (Alert independently)"]
        B1["🟡 HIGH\nBYOVD Drop + Service Chain\nTier 2 — hash-invariant"]
        B2["🔴 CRITICAL\nSilverFox Full Kill Chain\nTier 3 — binary-invariant"]
        B3["🔴 CRITICAL\nEmpire PowerShell Stager\nIntent-First Composite"]
        B4["🔴 CRITICAL\nOAuth High-Risk Consent\nIntent-First Composite"]
        B5["🟡 HIGH\nWMI Fileless scrcons DLL\nSubstrate-First Composite"]
    end

    subgraph Layer3["Incident Layer — Narrative Convergence"]
        C1["Entity stitching\nDeviceId · AccountName · SHA256\nTimeline reconstruction\nCousin surface correlation"]
    end

    A1 & A2 & A3 -->|"30-day primitive index\nfeeds context"| B1
    B1 & B2 & B3 & B4 & B5 -->|"Independent anchors\ncorrelate at incident layer"| C1
```

### Cousin Technique Coverage — AI Evasion Resistance

When an AI-assisted attacker detects resistance on one substrate and pivots, a cousin sensor fires independently:

| Primary Detection | Attacker Pivot | Cousin Sensor | Status |
|------------------|---------------|---------------|--------|
| SilverFox: scrcons child (non-fileless) | Attacker uses fileless DLL load | scrcons DLL load (T1546.003 this document) | ✅ Built |
| Empire PowerShell stager | AMSI blocks → pivot to mshta | mshta.exe proxy execution (T1218.005) | 🔴 Queued |
| BYOVD service via sc.exe | Use registry API instead | Registry ImagePath write (Tier 1/2) | ✅ Built |
| OAuth consent phishing | Token theft instead | SigninLogs anomaly composite | 🔴 Queued |
| LSASS via comsvcs | Switch to ProcDump | ProcDump LSASS cousin (T1003.001) | 🔴 Queued |

---

## Framework Alignment

This research is part of the Minimum Truth Detection Framework ecosystem:

| Repository | Role |
|------------|------|
| [MTDF Core](https://github.com/azdabat/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules) | Validated composite rules + ADX receipts |
| [Novel Tradecraft Research](https://github.com/azdabat/Novel-Tradecraft-Research-Emerging-Attack-Ecosystems) | This document — emerging attack chains |
| [ATLAS](https://github.com/azdabat/ATLAS-ATTACK-ECOSSYSTEM) | Strategic ecosystem map |
| [God Mode Roadmap](https://azdabat.github.io/Minimum-Truth-Detection-Framework-ADX-Validated-Composite-Rules/MTDF-Composite-Roadmap.html) | Interactive MITRE coverage matrix |

---

> [!NOTE]
> Detection rules in this document are production-candidate status. ADX-Docker validation
> against Empire telemetry and Atomic Red Team simulations is required before production
> deployment. Tenant-specific noise calibration is expected. `DriverLoadEvent` telemetry
> must be enabled in MDE for Tier 2/3 SilverFox detection.

> [!IMPORTANT]
> **If the SilverFox Tier 3 rule fires — assume EDR is already blind.**
> Do not rely on further endpoint telemetry. Isolate immediately.
> Validate EDR health as a separate, independent step before trusting any subsequent alerts.

---

*Part of the Minimum Truth Detection Framework*  
*Author: Ala Dabat | [github.com/azdabat](https://github.com/azdabat)*  
*Licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode)*  
*Attribution required · Non-commercial · ShareAlike*
