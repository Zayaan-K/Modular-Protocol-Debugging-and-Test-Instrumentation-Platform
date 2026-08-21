# System and Future-Platform Requirements

> Defines the complete protocol-interposer platform, the boundary between the reusable base board and protocol modules, and the rules that future USB, Ethernet, and PCIe modules must follow.

| Document field | Value |
|---|---|
| Document ID | PI-SYS-REQ-001 |
| Revision | Draft 0.2 |
| Date | 20 August 2026 |
| Applies to | Entire platform and all protocol modules |

## 1. Product definition

The project is a modular protocol interposer and test orchestrator. It assists an oscilloscope and logic analyzer by exposing signals, controlling experiments, injecting bounded faults, synchronizing instruments, and logging events. IT DOES NOT REPLACE THOSE INSTRUMENTS ONLY SUPPORTS THEIR OPERATION.

## 2. Architecture and ownership

```text
Host/target connection
        |
Protocol interface module
        |
Defined module interface
        |
Reusable base board
        |
PC, oscilloscope and logic analyzer
```

| Element | Owns |
|---|---|
| System/platform | Common behaviour, module rules, safety model, test records, release policy and future expansion |
| Base board | Controller, timing, triggers, logging, host control, interlocks and shared low-voltage power |
| Protocol module | Connectors, signal path, protocol-specific protection, line switching, measurement front ends and electrical limits |

## 3. Platform requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| SYS-001 | Must | The platform shall accept replaceable protocol-specific interface modules through a documented electrical and mechanical interface. | Architecture review |
| SYS-002 | Must | A protocol module shall own every circuit that directly loads, terminates, protects or switches its high-speed signal path. | Schematic review |
| SYS-003 | Must | The base board shall not assume a protocol voltage, impedance or connector pinout unless it is declared by the installed module. | Interface review |
| SYS-004 | Must | Every module shall declare its identity, hardware revision, supported modes, voltage limits and safe default state. | Module-identification test |
| SYS-005 | Must | Unsupported or unidentified modules shall remain unpowered or in a defined safe state. | Unknown-module test |
| SYS-006 | Must | Normal/pass-through mode shall not require controller firmware to preserve a protocol path when the module design permits a passive path. | Power-off continuity test |
| SYS-007 | Must | Fault injection shall be bounded by module-specific safe limits and shall require explicit enablement. | Fault review and misuse test |
| SYS-008 | Must | Reset, brownout, watchdog timeout, host disconnection and script abort shall return controlled outputs to documented safe states. | Recovery test matrix |
| SYS-009 | Must | The platform shall distinguish host-side and target-side connections physically and in documentation. | Inspection |
| SYS-010 | Must | Hardware revisions, firmware versions, module identity and active configuration shall be recorded with each automated test. | Log review |
| SYS-011 | Should | A common command model should support module discovery, configuration, switching, waits, triggers, measurements, loops and aborts. | Command-interface test |
| SYS-012 | Should | Future modules should reuse the base-board trigger, timing, logging and host-control facilities without redesigning them. | Future-module design review |
| SYS-013 | Must | USB 3.x, Ethernet and PCIe capability shall not be claimed until a separate module and validation plan exist for each protocol. | Release review |
| SYS-014 | Must | High-speed-compatible operation shall not be represented as formal standards compliance or certification. | Documentation review |
| SYS-015 | Should | The architecture should allow a module to declare unavailable functions so software cannot expose controls that the hardware does not implement. | Capability-discovery test |

## 4. Common experiment model

Every automated experiment should be representable as:

1. Identify the base board and installed module.
2. Validate the requested configuration against module limits.
3. Apply a known normal state.
4. Arm instruments or emit a synchronization marker.
5. Run timed switching, measurement or fault actions.
6. Record events and results.
7. Return to the safe state.

## 5. Common safety requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| SAF-001 | Must | Every module shall document absolute maximum voltage, current, temperature and connection conditions. | Documentation review |
| SAF-002 | Must | No single normal user action shall short two power sources or directly short a power rail to ground. | Misuse test |
| SAF-003 | Must | Potentially damaging configurations shall require an arm-then-activate action and visible indication. | Usability test |
| SAF-004 | Must | A hardware-level path shall force safety-critical controls to a safe state when the controller is unpowered or held in reset. | Unpowered-state test |
| SAF-005 | Must | The emergency abort function shall not depend solely on the host application remaining responsive. | Host-crash test |
| SAF-006 | Must | Allowed fault combinations and prohibited combinations shall be machine-checkable or physically prevented. | Configuration-matrix test |
| SAF-007 | Must | Instrument ground assumptions, isolation limits and external-supply relationships shall be documented for every module. | Documentation review |

## 6. Verification and release policy

| Test | Acceptance activity |
|---|---|
| Platform inspection | Verify labels, module keying, safe defaults and revision identification. |
| Unknown-module test | Confirm an unidentified or incompatible module cannot enter an active state. |
| Recovery matrix | Interrupt operation with reset, brownout, watchdog, abort and control-cable removal. |
| Timing correlation | Compare logged timestamps with oscilloscope trigger markers. |
| Fault matrix | Test every permitted fault individually and every permitted combination. |
| Regression | Repeat affected acceptance tests after PCB, BOM, firmware or interface changes. |

## 7. Future protocol modules

| Module | Status | Separate work required |
|---|---|---|
| USB 2.0 / USB-C USB 2.0 | Revision A target | USB-specific signal, power, CC and fault requirements |
| Ethernet | Future | Magnetics, isolation, link-speed, PoE and pair-routing requirements |
| USB 3.x | Future | Multi-gigabit switching, fixtures, compliance strategy and suitable test equipment |
| PCIe | Future | Lane topology, reference clock, reset, power, sideband and high-speed validation requirements |

## 8. Required release documentation

- System architecture and module-interface specification
- Base-board and module schematics, PCB sources and BOMs
- PCB stack-ups and controlled-impedance calculations
- Firmware source, command protocol and versioning method
- Assembly, bring-up and calibration procedures
- Safety limits and prohibited configurations
- Acceptance-test reports and known limitations

