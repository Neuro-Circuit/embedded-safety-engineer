# 🛡️ Embedded Safety Engineer

### A Smart Claude Code Skill for Professional Embedded Firmware, Real-Time Systems & Hardware-Aware Security

**Current version:** `0.1.0`  
**Status:** Foundation / Early Release  
**License:** MIT

> **Embedded Safety Engineer** is a modular engineering Skill designed to help Claude Code write, review, refactor, and audit embedded firmware and RTL with explicit attention to hardware constraints, real-time behavior, concurrency, memory safety, reliability, and defensive security.

---

## 🚀 Why this Skill?

Embedded software is fundamentally different from ordinary application software.

A program can compile successfully and still fail because:

- an ISR takes too long;
- a shared variable is accessed without proper synchronization;
- `volatile` is incorrectly treated as thread safety;
- CPU and DMA access the same buffer concurrently;
- cache coherency is ignored;
- a protocol parser trusts an attacker-controlled length;
- an integer overflow changes a buffer boundary;
- a watchdog is refreshed by a partially dead system;
- a firmware update accepts an unauthorized image;
- a production debug interface exposes privileged functionality;
- an FPGA design crosses clock domains incorrectly;
- timing constraints do not match the actual hardware architecture.

**Embedded Safety Engineer** adds a hardware-aware engineering layer to Claude Code so that code generation is accompanied by structured safety, reliability, real-time, and security reasoning.

It is designed around:

```text
Discover
   ↓
Detect Hardware / Software Context
   ↓
Classify Risk
   ↓
Load Relevant Rules
   ↓
Threat + Failure Modeling
   ↓
Design
   ↓
Implement
   ↓
Review
   ↓
Verify
   ↓
Report Remaining Risks
```

---

# ✨ Key Capabilities

## 🧠 Smart Context Detection

The Skill first attempts to understand the project instead of blindly applying generic rules.

It looks for evidence such as:

- MCU / SoC / FPGA
- CPU core
- compiler and toolchain
- SDK / HAL / LL / framework
- RTOS
- linker script
- memory layout
- bootloader
- OTA/update mechanism
- interrupts
- DMA
- cache
- MPU / TrustZone / security extensions
- communication interfaces
- external inputs
- timing requirements
- build and test commands

### Anti-hallucination principle

The Skill explicitly instructs Claude Code **not to invent hardware-specific facts**.

It should not fabricate:

- register addresses
- interrupt vectors
- DMA mappings
- memory addresses
- clock limits
- peripheral behavior
- undocumented security capabilities

If the exact device behavior matters and the available evidence is insufficient, the assumption must be made explicit and authoritative documentation should be requested or consulted.

---

# 🎯 Risk-Aware Engineering

Not every embedded task has the same risk.

The Skill classifies work into:

| Level | Examples |
|---|---|
| 🟢 LOW | Documentation, formatting, simple GPIO changes |
| 🟡 MEDIUM | Peripheral drivers, UART, SPI, I2C |
| 🟠 HIGH | ISR, DMA, RTOS scheduling, bootloaders, protocol parsers |
| 🔴 CRITICAL | Secure boot/update, key handling, safety-critical control, production security |

For **HIGH** and **CRITICAL** tasks, the review becomes more demanding and explicitly considers:

- memory safety
- concurrency
- timing
- failure modes
- attack surface
- privilege boundaries
- recovery
- verification

---

# ⚡ Interrupt & ISR Safety

The Skill reviews interrupt handlers for:

- bounded execution
- correct interrupt acknowledgement
- interrupt priority
- nesting
- starvation
- latency
- shared-state access
- reentrancy
- safe handoff to task/main context

It strongly discourages inappropriate ISR behavior such as:

- blocking
- sleeping
- dynamic allocation
- heavy parsing
- network/filesystem operations
- lengthy logging
- unbounded loops

The goal is not simply:

> "Does this ISR compile?"

but:

> "Can this ISR execute safely under worst-case system load?"

---

# 🔄 Concurrency & Synchronization

For shared data, the Skill asks:

```text
Who writes?
Who reads?
From which context?
Who owns the object?
How long does it live?
Is the access atomic?
What ordering is required?
What synchronization mechanism is used?
Can it be re-entered?
```

It checks for:

- race conditions
- deadlocks
- lock-order violations
- priority inversion
- starvation
- reentrancy problems
- interrupt/task interactions

### Important rule

```text
volatile != atomicity
volatile != synchronization
volatile != thread safety
```

---

# 🚚 DMA & Cache Safety

DMA is treated as an independent execution actor.

The Skill reviews:

- CPU/DMA ownership
- buffer lifetime
- buffer bounds
- alignment
- DMA-accessible memory
- cache coherency
- memory barriers where required
- circular buffers
- double buffering
- completion/error handling
- CPU/DMA race windows

This is especially important on cache-enabled Cortex-M systems and SoCs.

---

# ⏱️ Real-Time Engineering

Where timing matters, the Skill considers:

- periods
- deadlines
- interrupt latency
- jitter
- blocking time
- WCET assumptions
- CPU utilization
- resource contention
- priority interactions

It deliberately avoids claiming a hard real-time guarantee without evidence, measurement, or appropriate analysis.

---

# 🧵 RTOS Awareness

The Skill includes RTOS-specific review concepts for:

- tasks/threads
- priorities
- stack sizes
- mutexes
- semaphores
- queues
- timers
- ISR-safe APIs
- blocking
- priority inversion
- deadlocks
- starvation
- watchdog interaction

It can be used as a foundation for future dedicated FreeRTOS, Zephyr, and CMSIS-RTOS profiles.

---

# 🧠 Memory Safety

The Skill reviews common embedded memory failures:

- buffer overflow
- buffer underflow
- out-of-bounds access
- use-after-free
- dangling pointers/references
- double free
- null/invalid pointers
- uninitialized memory
- integer overflow
- integer truncation
- signed/unsigned conversion
- alignment errors
- aliasing problems
- stack overflow
- heap fragmentation
- DMA memory corruption

It encourages bounded operations and validation before memory access.

---

# 🔐 Defensive Firmware Security

Security is treated as part of the architecture rather than an afterthought.

The Skill considers:

### Attack Surface

- UART
- USB
- CAN
- Ethernet
- BLE
- Wi-Fi
- external configuration
- update images
- IPC/shared memory
- debug interfaces
- recovery interfaces

### Input Validation

Before:

```text
indexing
copying
parsing
allocation
arithmetic
state transitions
privileged operations
```

input should be validated.

---

# 🔑 Secure Boot & Firmware Updates

For bootloaders and update mechanisms, the Skill reviews:

- authenticity
- integrity
- secure boot chain
- firmware verification
- anti-rollback
- version validation
- key provisioning
- key storage
- interrupted update recovery
- downgrade resistance
- production/debug separation
- failure behavior

It does **not** encourage custom cryptography.

Where possible, it prefers vetted cryptographic primitives and platform security mechanisms.

---

# 🕵️ Debug & Physical Attack Surface

When relevant, the Skill considers:

- JTAG
- SWD
- boot/recovery modes
- debug unlock
- flash extraction
- fault injection
- voltage glitching
- clock glitching
- reset abuse
- side-channel leakage

It does not promise that a device is impossible to compromise.

Instead, it helps identify attack paths and document residual risk.

---

# 🛡️ Reliability & Fail-Safe Engineering

The Skill considers failure as an expected engineering condition.

A common model is:

```text
Detect
  ↓
Contain
  ↓
Record safely
  ↓
Enter defined safe state
  ↓
Recover / Restart when justified
```

Relevant areas include:

- watchdogs
- brownout
- reset behavior
- power loss
- interrupted nonvolatile writes
- peripheral lockups
- communication timeouts
- retry limits
- sensor plausibility
- reset loops
- persistent-state corruption

A watchdog is treated as a recovery mechanism—not a replacement for correct architecture.

---

# 🔌 Supported Languages

The foundation release includes rule packs for:

### Firmware

- C
- C++
- Rust
- Assembly

### RTL / FPGA

- Verilog
- SystemVerilog
- VHDL

---

# 🧩 Supported Platforms

The foundation release includes guidance for:

### Microcontrollers

- AVR
- ARM Cortex-M
- STM32
- ESP32
- RP2040

### FPGA

- FPGA / RTL development

> Platform-specific behavior should always be verified against the exact device and authoritative documentation.

---

# 🏗️ Project Architecture

The Skill is intentionally modular.

```text
embedded-safety-engineer/
│
├── SKILL.md
├── README.md
├── CHANGELOG.md
├── LICENSE
├── MANIFEST.txt
│
├── rules/
│   ├── universal.md
│   ├── c.md
│   ├── cpp.md
│   ├── rust.md
│   ├── assembly.md
│   ├── verilog.md
│   ├── systemverilog.md
│   └── vhdl.md
│
├── platforms/
│   ├── avr.md
│   ├── arm-cortex-m.md
│   ├── stm32.md
│   ├── esp32.md
│   ├── rp2040.md
│   └── fpga.md
│
├── realtime/
│   ├── interrupts.md
│   ├── concurrency.md
│   ├── dma.md
│   ├── rtos.md
│   └── timing.md
│
├── security/
│   ├── memory.md
│   ├── firmware.md
│   ├── secure-boot.md
│   ├── communications.md
│   ├── debug-ports.md
│   ├── fault-injection.md
│   └── side-channel.md
│
├── safety/
│   ├── watchdog.md
│   ├── fail-safe.md
│   └── power-loss.md
│
├── quality/
│   ├── architecture.md
│   ├── testing.md
│   └── static-analysis.md
│
└── checklists/
    ├── pre-code.md
    ├── post-code.md
    ├── security-review.md
    └── release.md
```

This architecture makes future releases easier to extend without turning the Skill into an unmaintainable monolithic prompt.

---

# 🛠️ Installation

## Option 1 — Project-local installation

Clone or copy this repository into the Skills directory supported by your installed Claude Code version.

Example:

```bash
git clone <YOUR_REPOSITORY_URL>
```

Then place the Skill directory in the appropriate Claude Code Skills location.

The exact Skills discovery path can change between Claude Code releases, so use the current Claude Code documentation for the installation path supported by your version.

After installation, verify that the Skill is discoverable as:

```text
embedded-safety-engineer
```

---

## Option 2 — ZIP installation

Download:

```text
embedded-safety-engineer-v0.1.0.zip
```

Extract it:

```bash
unzip embedded-safety-engineer-v0.1.0.zip
```

Then place:

```text
embedded-safety-engineer-v0.1.0/
```

in your Claude Code Skills directory.

---

# 💻 Using It

Once installed, use Claude Code normally inside an embedded project.

Examples:

```text
Review this STM32 DMA driver for:
- ISR safety
- race conditions
- buffer ownership
- cache coherency
- timing
- error recovery
- security issues
```

Or:

```text
Implement a secure UART packet parser.
Treat all packet data as untrusted.
Check integer overflow, buffer boundaries, state transitions,
timeouts, and denial-of-service risks.
```

Or:

```text
Audit this FreeRTOS application for:
- deadlocks
- priority inversion
- race conditions
- ISR misuse
- stack risks
- watchdog behavior
- timing problems
```

Or:

```text
Review this bootloader for secure boot, firmware authenticity,
anti-rollback, key handling, interrupted update recovery,
debug-port exposure, and failure behavior.
```

Or:

```text
Review this SystemVerilog design for:
- CDC
- metastability
- reset synchronization
- latch inference
- multiple drivers
- timing hazards
- synthesis/simulation mismatches
```

---

# 🔍 Recommended Project Context

The Skill works best when the repository contains or exposes:

```text
MCU / SoC / FPGA part number
Datasheet / Reference Manual
Board information
Compiler version
SDK / HAL version
RTOS configuration
Linker script
Memory map
Clock configuration
Build commands
Test commands
Security requirements
Timing requirements
```

For hardware-specific work, the exact part number is especially valuable.

For example:

```text
STM32
```

is not sufficient to safely assume every peripheral/register behavior.

Prefer:

```text
STM32H743ZI
```

or the exact target used by the project.

---

# 🧪 Verification Philosophy

The Skill encourages using the tools already configured by the project.

### C / C++

Possible tools include:

- compiler warnings
- clang-tidy
- Clang Static Analyzer
- cppcheck
- MISRA tooling
- CERT-oriented analysis

### Rust

Possible tools include:

- cargo fmt
- cargo clippy
- cargo audit
- cargo deny

### FPGA / HDL

Possible checks include:

- lint
- simulation
- CDC analysis
- synthesis
- timing analysis
- assertions/properties

The Skill follows one important rule:

> **It must never claim that a test, build, static analysis, or hardware verification was performed unless it was actually performed.**

---

# 📋 Review Output

For substantial changes, the Skill encourages a structured engineering report:

```text
Embedded Engineering Review
---------------------------

Target:
Language:
Framework/RTOS:
Risk:

Changes:
- ...

Safety / reliability:
- ...

Concurrency / timing:
- ...

Security:
- ...

Verification:
- ...

Remaining assumptions:
- ...
```

Findings are reported as:

```text
Severity | Finding | Impact | Recommendation | Verification
```

Severity levels:

```text
INFO
LOW
MEDIUM
HIGH
CRITICAL
```

Unresolved HIGH or CRITICAL findings should never be silently hidden.

---

# 📦 Release Checklist

Before a production-oriented release, the Skill encourages checking:

- [ ] production debug policy
- [ ] secure boot/update policy
- [ ] compiler warnings
- [ ] static analysis
- [ ] unit/integration tests
- [ ] boundary/error cases
- [ ] reset behavior
- [ ] watchdog recovery
- [ ] power-cycle behavior
- [ ] persistent-state integrity
- [ ] rollback/version policy
- [ ] known security risks
- [ ] known hardware assumptions

---

# 🔒 Security Disclaimer

This project is a **defensive engineering aid**.

It does not guarantee:

- absolute security
- immunity from exploitation
- absence of vulnerabilities
- functional safety certification
- compliance certification
- hardware correctness
- production readiness

It should complement, not replace:

- datasheet/reference-manual review
- hardware validation
- static analysis
- code review
- penetration testing
- HIL testing
- fault injection testing where appropriate
- formal verification where required
- functional-safety processes
- product-security review
- applicable certification processes

---

# 🗺️ Roadmap

## v0.1.0 — Foundation

Included:

- smart context detection
- rule routing
- risk classification
- C/C++/Rust/Assembly
- Verilog/SystemVerilog/VHDL
- AVR
- ARM Cortex-M
- STM32
- ESP32
- RP2040
- FPGA
- interrupts
- concurrency
- DMA
- RTOS
- timing
- memory safety
- firmware security
- secure boot
- secure update/OTA concepts
- communications security
- debug-port security
- fault injection
- side-channel considerations
- watchdog
- fail-safe
- power/reset resilience
- testing
- static analysis
- GitHub documentation

## v0.2.x — RTOS & MCU Expansion

Planned:

- deeper FreeRTOS profile
- Zephyr profile
- CMSIS-RTOS profile
- expanded STM32 family guidance
- expanded ESP32-family guidance
- more MCU-specific checks
- project configuration profiles

## v0.3.x — Security Engineering

Planned:

- stronger threat-model templates
- CWE-oriented review mapping
- secure OTA workflows
- key-management guidance
- production debug policy templates
- security review reports
- security regression checklists

## v0.4.x — Verification

Planned:

- automated build/test discovery
- CI-oriented workflows
- static-analysis orchestration
- HIL-oriented checklists
- fault-injection workflows
- machine-readable review reports

## v0.5.x — FPGA / Hardware Verification

Planned:

- deeper CDC/RDC analysis
- timing-oriented review
- reset-domain analysis
- HDL verification profiles
- assertion-oriented workflows

## v1.0.0 — Production Foundation

Target:

A mature, extensible embedded engineering Skill with comprehensive platform,
security, real-time, safety, verification, and CI profiles.

---

# 📐 Versioning

This project follows Semantic Versioning:

```text
MAJOR.MINOR.PATCH
```

### MAJOR

Breaking changes to Skill architecture or behavior.

### MINOR

Backward-compatible capabilities, rule packs, and features.

### PATCH

Bug fixes, corrections, documentation improvements, and non-breaking refinements.

Current release:

```text
0.1.0
```

---

# 🤝 Contributing

Contributions are welcome.

When adding a rule:

1. Keep it technically defensible.
2. Separate platform-specific behavior from universal rules.
3. Never invent hardware facts.
4. Avoid absolute security claims.
5. Prefer actionable engineering checks.
6. Explain the reasoning behind important rules.
7. Add or update the appropriate checklist.
8. Update `CHANGELOG.md`.
9. Follow Semantic Versioning.

---

# 📄 License

MIT License.

See [`LICENSE`](LICENSE).

---

# ⭐ Project Vision

The long-term goal is not to create another code-generation prompt.

The goal is to build an **Embedded Engineering Intelligence Layer** for Claude Code:

```text
             Claude Code
                  │
                  ▼
      ┌──────────────────────┐
      │ Embedded Safety      │
      │ Engineer Skill       │
      └──────────┬───────────┘
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
   Hardware   Software   Threat
   Context    Context    Context
       │         │         │
       └─────────┼─────────┘
                 ▼
        Engineering Reasoning
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
     Safety   Security   Quality
       │         │         │
       └─────────┼─────────┘
                 ▼
       Implementation + Review
                 │
                 ▼
          Verification
                 │
                 ▼
       Professional Firmware
```

The objective is to make AI-assisted embedded development **more predictable,
auditable, maintainable, secure, and hardware-aware**.

---
