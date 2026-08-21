# Main Base-Board Requirements

> Defines the reusable controller and test-orchestration board. Protocol-specific connectors and high-speed signal paths belong on interface modules.

| Document field | Value |
|---|---|
| Document ID | PI-BASE-REQ-001 |
| Revision | Draft 0.2 |
| Date | 20 August 2026 |
| Applies to | Reusable base/controller board |

## 1. Scope

The base board coordinates experiments across USB, Ethernet and future PCIe modules. It supplies control, timing, triggers, logging, interlocks and shared power. It shall not directly route USB D+/D-, Ethernet pairs, USB SuperSpeed pairs or PCIe lanes.

## 2. Functional blocks

- Microcontroller or FPGA-based sequencer
- Module discovery and control interface
- External trigger input and output
- Timestamp and event log
- Host-control connection
- Hardware abort and safety interlocks
- Shared regulated power rails
- Nonvolatile configuration and revision identification

## 3. Module interface requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| BIF-001 | Must | The base board shall provide a documented, keyed module connector that prevents reversed insertion. | Mechanical inspection |
| BIF-002 | Must | The connector specification shall define every power, ground, control, identification, trigger and analog-measurement pin. | Interface review |
| BIF-003 | Must | High-speed protocol lanes shall not pass through a generic base-board connector unless that connector is explicitly designed and validated for them. | Layout review |
| BIF-004 | Must | The module interface shall provide enough ground pins to give control and measurement signals short return paths. | Pinout review |
| BIF-005 | Must | The base board shall identify a module before enabling module power or active controls. | Discovery test |
| BIF-006 | Must | Module power rails shall have documented voltage, current, sequencing and inrush limits. | Power test |
| BIF-007 | Must | Module control outputs shall assume inactive levels during reset and before firmware configuration. | Reset-state test |
| BIF-008 | Should | The interface should provide at least one synchronous trigger output, one event input and one general-purpose serial control channel. | Functional test |
| BIF-009 | Should | The interface should provide one or more analog monitor inputs for module current, voltage or detector outputs. | Calibration test |
| BIF-010 | Should | Module identity should be readable through a simple nonvolatile ID device or resistor-coded scheme. | Identification test |

## 4. Controller and sequencing requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| CTL-001 | Must | The controller shall execute deterministic switch, wait, trigger and abort actions. | Sequence test |
| CTL-002 | Should | The command set should support loops, conditional stops and repeated intermittent-fault patterns. | Script test |
| CTL-003 | Must | Sequence timing resolution, maximum delay, jitter and drift shall be characterized. | Oscilloscope timing test |
| CTL-004 | Must | A sequence shall validate every requested operation against the installed module's capability and limits before arming. | Invalid-script test |
| CTL-005 | Must | A hardware watchdog shall return controlled outputs to their safe states if the controller stops servicing it. | Watchdog test |
| CTL-006 | Must | The controller shall expose an immediately accessible abort function. | Abort test at each sequence step |
| CTL-007 | Should | Named test configurations and scripts should be storable and recallable. | Persistence test |
| CTL-008 | Must | Corrupt or incompatible stored configuration shall not be applied automatically. | Corruption test |

## 5. Trigger and timing requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| TRG-001 | Must | The base board shall provide at least one oscilloscope-compatible trigger output. | Voltage and timing test |
| TRG-002 | Must | Trigger voltage, polarity, edge rate, source impedance and protection limits shall be documented. | Electrical characterization |
| TRG-003 | Should | The base board should provide a protected external trigger input capable of initiating a configured event. | Functional test |
| TRG-004 | Must | Trigger-to-action delay and variation shall be measured for every automated switching path. | Timing characterization |
| TRG-005 | Should | Dedicated markers should identify sequence start, fault activation, recovery and abort. | Event-marker test |
| TRG-006 | Should | Multiple instruments should be able to share a trigger without exceeding the output drive specification. | Load test |

## 6. Logging and host-interface requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| LOG-001 | Must | Every log shall include base-board revision, firmware version, module identity and active configuration. | Log review |
| LOG-002 | Should | Logs should include relative timestamps for power, connection, fault, threshold, trigger and abort events reported by a module. | Event test |
| LOG-003 | Must | Timestamp resolution, rollover behaviour and accuracy shall be documented. | Clock test |
| LOG-004 | Should | The host interface should use USB-UART, USB CDC or another simple documented transport. | Host compatibility test |
| LOG-005 | Should | The command and log formats should be human-readable and suitable for scripting. | Interface review |
| LOG-006 | Must | Loss of the host connection shall not leave an armed fault indefinitely active. | Cable-removal test |

## 7. Base-board power requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| BPW-001 | Must | The base board shall have reverse-polarity, overcurrent and transient protection appropriate to its input. | Protection test |
| BPW-002 | Must | Shared module rails shall be current limited or switched so that a module fault cannot damage the host computer. | Controlled-overload test |
| BPW-003 | Must | Module power enable shall default off until identity and compatibility checks complete. | Startup test |
| BPW-004 | Should | The controller should measure base-board supply voltage and module-rail current for safety supervision. | Calibration test |
| BPW-005 | Must | Shared power shall remain within regulator temperature and power ratings under every allowed steady load. | Thermal test |
| BPW-006 | Must | Base-board power shall remain separate from a protocol module's inline target power unless the module-interface specification deliberately connects them. | Schematic review |

## 8. User-interface and mechanical requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| UI-001 | Must | Power, armed, active-fault, module-present and error states shall have independent visible indication. | Usability test |
| UI-002 | Must | The abort control shall be accessible without using the host application. | Inspection and functional test |
| UI-003 | Must | Potentially damaging actions shall require a deliberate arm-then-activate operation. | Misuse test |
| UI-004 | Should | The board should provide mounting holes, enclosure clearance and strain relief for host and trigger cables. | Mechanical fit test |
| UI-005 | Must | Connector functions, voltage domains and orientation shall be readable after assembly. | Assembly inspection |

## 9. Base-board acceptance tests

1. Inspect assembly, keying, labels and default control states.
2. Power from a current-limited supply and verify every shared rail.
3. Insert known, unknown and incompatible module-ID fixtures.
4. Characterize trigger levels and trigger-to-action timing.
5. Execute representative sequences and compare logs with oscilloscope captures.
6. Interrupt operation using abort, reset, watchdog, brownout and host-cable removal.
7. Overload each switched module rail within the defined test procedure.
8. Run a thermal test at maximum supported steady load.

## 10. Open design decisions

- Controller choice: microcontroller, FPGA or combined approach
- Module connector and pin count
- Module identification method
- Trigger voltage standard and connector type
- Number and bandwidth of analog monitor inputs
- External power-input voltage and maximum module power
- Whether the first revision includes an enclosure

