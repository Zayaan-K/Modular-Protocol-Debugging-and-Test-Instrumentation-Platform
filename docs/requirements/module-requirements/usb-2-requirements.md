# USB 2.0 Interface-Module Requirements

> Defines the protocol-specific interposer module placed inline between a USB host and device. This module owns USB connectors, D+/D-, VBUS, CC pins, protection, switching, probing and USB-specific fault injection.

| Document field | Value |
|---|---|
| Document ID | PI-USB2-REQ-001 |
| Revision | Draft 0.2 |
| Date | 20 August 2026 |
| Applies to | USB 2.0 and USB-C operating in USB 2.0 mode |

## 1. Scope and limits

- Supported: USB low speed (1.5 Mb/s), full speed (12 Mb/s), and a carefully routed high-speed-compatible pass-through target (480 Mb/s).
- Supported connectors: USB Type-A and/or USB-C configurations selected for Revision A.
- Not supported: USB 3.x SuperSpeed probing, USB Power Delivery, or USB-IF compliance certification.
- SuperSpeed pins shall not be connected to breakout headers.

## 2. Pass-through and signal-integrity requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| SIG-001 | Must | The module shall provide one clearly identified upstream host connection and one downstream device connection. | Inspection |
| SIG-002 | Must | D+ and D- shall be routed as a controlled 90-ohm differential pair using the selected PCB stack-up. | Impedance calculation and fabrication review |
| SIG-003 | Must | D+/D- mismatch, vias, branches and reference-plane discontinuities shall be minimized and documented. | Layout review |
| SIG-004 | Must | The USB data pair shall have a continuous high-frequency return reference beneath it. | Layout review |
| SIG-005 | Must | No ordinary 2.54 mm header shall form a long unterminated branch from D+ or D-. | Layout review |
| SIG-006 | Must | In normal mode, the module shall pass low-speed and full-speed enumeration and transfers. | Compatibility test |
| SIG-007 | Should | The passive direct path should pass representative USB high-speed enumeration and sustained transfers. | Host/device/cable matrix |
| SIG-008 | Must | High-speed-compatible operation shall not be described as formal USB compliance certification. | Documentation review |

## 3. Probe-access requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| PRB-001 | Must | The module shall expose D+, D-, upstream VBUS, downstream VBUS, ground and shield at labelled test points. | Inspection and continuity |
| PRB-002 | Must | Every signal test location shall have an adjacent ground suitable for a short probe connection. | Probe-fit inspection |
| PRB-003 | Should | D+/D- should have compact paired pads suitable for differential probing. | Usability test |
| PRB-004 | Should | Probe attachment and test-point stubs should be characterized by repeating USB transfer tests with the intended probes attached. | Probe-impact test |
| PRB-005 | Should | A logic-analyzer output shall use a buffered or otherwise validated path and shall not directly overload D+/D-. | Electrical review and transfer test |

## 4. USB power measurement and routing

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| PWR-001 | Must | Independent upstream and downstream VBUS measurement points shall be provided. | DMM inspection |
| PWR-002 | Must | The VBUS path shall include a documented Kelvin-routed current shunt with rated resistance, tolerance and power. | Calibration and layout review |
| PWR-003 | Should | An amplified current-monitor output should declare gain, bandwidth, offset, noise and output range. | Oscilloscope calibration |
| PWR-004 | Must | Downstream VBUS shall support controlled disconnection and shall prevent uncontrolled reverse current. | Fault test |
| PWR-005 | Must | Host-powered and externally powered target modes shall be interlocked against source contention. | Misuse test |
| PWR-006 | Must | The external target-power input shall state allowable voltage, polarity, current and ground relationship. | Inspection and protection test |
| PWR-007 | Must | The module shall include appropriate overcurrent and reverse-current protection. | Controlled-overload test |
| PWR-008 | Must | A visible indicator shall show when downstream VBUS is energized. | Functional test |
| PWR-009 | Must | Added VBUS drop at the maximum supported current shall be measured and published. | Four-wire measurement |
| PWR-010 | Should | The measurement path should support inrush, steady-state current, peak current, droop and recovery captures. | Measurement demonstration |

## 5. USB fault-injection requirements

> **Safety principle:** Reset, controller loss and unpowered operation shall return faults to a documented normal or disconnected-safe state. Destructive combinations must be prevented or explicitly guarded.

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| FLT-001 | Must | VBUS, D+ and D- connection shall be independently controllable. | Functional test |
| FLT-002 | Must | Data switching shall preserve acceptable normal-mode signal integrity. | Transfer and waveform test |
| FLT-003 | Must | Overlapping paths shall use break-before-make operation where contention could occur. | Timing measurement |
| FLT-004 | Must | D+ and D- shall not be intentionally connectable to an unsafe voltage. | Schematic and misuse review |
| FLT-005 | Should | Current-limited pull-up and pull-down networks should be selectable on D+ and D-. | Resistance and functional test |
| FLT-006 | Should | Characterized series-resistance options should be selectable independently in D+ and D-. | Transfer test |
| FLT-007 | Should | Characterized shunt-capacitance or load options should be selectable through short paths. | Capacitance and waveform test |
| FLT-008 | Should | The module should support bounded VBUS brownout or droop using an external supply or current-limited control element. | Oscilloscope test |
| FLT-009 | Must | Ground opening shall not be a casual front-panel function. Any research implementation shall be separately guarded and document alternate return paths. | Design review |
| FLT-010 | Should | Automated faults should support configurable delay and duration with a simultaneous trigger marker. | Timing test |
| FLT-011 | Could | Intermittent-fault patterns may include pulse trains, randomized dropouts and contact-bounce emulation within safe limits. | Sequence test |
| FLT-012 | Must | Allowed stresses, prohibited combinations and recovery procedures shall be documented before target connection. | Documentation review |

## 6. USB-C USB 2.0 requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| USBC-001 | Must | USB-C support shall be limited to USB 2.0 D+/D-, VBUS, ground, shield and required CC behaviour. | Schematic review |
| USBC-002 | Must | Duplicated receptacle D+ pins and duplicated D- pins shall be connected for reversible orientation as required. | Continuity test |
| USBC-003 | Must | CC resistors shall match the selected source/sink and plug/receptacle roles. | Resistance and connection test |
| USBC-004 | Must | The module shall not advertise unsupported current. | Connection test |
| USBC-005 | Must | SuperSpeed pins shall be unconnected or reserved only for a separately validated future design. | Layout review |
| USBC-006 | Must | Revision A shall not claim USB Power Delivery capability. | Documentation review |

## 7. Protection, grounding and isolation

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| ISO-001 | Must | ESD protection shall have capacitance and clamping behaviour compatible with the supported USB speed. | Component and layout review |
| ISO-002 | Should | Shield-to-circuit-ground treatment should support documented direct, RC or disconnected configurations where safe. | Continuity and impedance test |
| ISO-003 | Could | A selectable USB isolator path may be provided for low/full speed only unless explicitly rated and validated at high speed. | Part review and speed test |
| ISO-004 | Must | The non-isolated nature of ordinary oscilloscope ground connections shall be documented. | Documentation review |

## 8. Cable and known-target diagnostics

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| DIA-001 | Should | In an unpowered diagnostic mode, the module should test VBUS, ground, D+, D- and shield continuity and common shorts. | Known-good/bad cable set |
| DIA-002 | Should | The diagnostic mode should detect D+/D- reversal where the cable type permits it. | Reversed fixture |
| DIA-003 | Should | The module should estimate cable VBUS resistance or loaded voltage drop over a documented range. | Calibrated comparison |
| DIA-004 | Could | Intermittent continuity may be logged while a cable is mechanically disturbed. | Flex test |
| DIA-005 | Should | A known USB endpoint or loopback workload should help distinguish host, fixture, cable and target failures. | End-to-end test |

## 9. USB module acceptance tests

1. Inspect connectors, ESD devices, switch defaults, labels and D+/D- routing.
2. Verify unpowered continuity and absence of shorts.
3. Power through a current-limited supply and calibrate VBUS/current outputs.
4. Enumerate and transfer with low-speed and full-speed devices.
5. Test representative high-speed hosts, devices and cables without probes, then with intended probes.
6. Measure VBUS drop, inrush, load steps, current-monitor noise and thermals.
7. Exercise every allowed fault and allowed fault combination.
8. Measure switch interval, fault duration, skew and trigger relationship.
9. Interrupt automated operation with abort, reset, brownout and base-board disconnection.
10. Record limitations and update the compatibility matrix.

## 10. Open design decisions

- Exact Type-A and Type-C connector arrangement
- Maximum supported target current
- Whether Battery Charging modes are excluded
- Data-line switch technology and package
- Current-shunt resistance and amplifier bandwidth
- Brownout-generation method
- Whether cable diagnostics require a separate adapter
- PCB layer count and fabrication stack-up

