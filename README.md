# CANXLForge Pro (v1.0.0)

### ISO 11898-1:2024 & CiA 610-1 CAN XL Protocol Studio & 10-Point Normative Auditor

[![Normative Standard](https://img.shields.io/badge/Standard-ISO%2011898--1%3A2024-blue.svg)](https://www.iso.org) [![CiA Specification](https://img.shields.io/badge/CiA-610--1%20%7C%20611--1-green.svg)](https://www.can-cia.org) [![CRC Engine](https://img.shields.io/badge/Dual%20CRC-PCRC\(13\)%20%2B%20FCRC\(32\)-orange.svg)](#) [![Zero-Trust Compliance](https://img.shields.io/badge/Auditor-10--Point%20Normative%20Pass-brightgreen.svg)](#) [![License](https://img.shields.io/badge/License-Commercial%20B2B-purple.svg)](#)

---

## 1\. Executive Summary

**CANXLForge Pro** is an automotive-grade engineering studio and verification suite for the **3rd Generation CAN** standard: **CAN XL (ISO 11898-1:2024)** and **CiA 610-1 / CiA 611-1**.

As modern software-defined vehicles (SDV) transition from domain-oriented architectures to **Zonal E/E Architectures**, automotive OEMs and Tier-1 suppliers face bandwidth and MTU bottlenecks. While Classical CAN (8 bytes, 1 Mbit/s) and CAN FD (64 bytes, 5 Mbit/s) fall short, 10BASE-T1S Automotive Ethernet requires costly new wiring harness redesigns. **CAN XL bridges this critical divide** by delivering:

- Up to **2,048 octets (2 KiB)** payload capacity per frame (handling standard 1500-byte IEEE 802.3 Ethernet MTU without fragmenting).  
- Bitrates up to **10 Mbit/s to 20 Mbit/s** in the data phase with SIC (Signal Improvement Capability) transceivers.  
- Strict non-destructive bitwise bus arbitration via an **11-bit Priority ID** decoupled from the message addressing scheme.  
- Dedicated hardware-isolated fields: **SDU Type (8-bit)**, **VCID (Virtual CAN ID, 8-bit)**, and **Acceptance Field (32-bit)**.  
- **Dual CRC Protection**: 13-bit PCRC (Preamble CRC, poly `0x2B65`) protecting control and header fields prior to baud rate acceleration, and 32-bit FCRC (Frame CRC, poly `0x04C11DB7`, Hamming distance 6\) protecting payload.

CANXLForge Pro provides an interactive web workspace and a zero-dependency CLI runner that enables automotive firmware architects, ECU validation engineers, and cybersecurity auditors to synthesize, dissect, encapsulate Ethernet-over-CAN XL, and audit bitstreams against normative conformance rules.

---

## 2\. Key Capabilities & Features

1. **CAN XL Frame Synthesis (CiA 610-1)**  
     
   - Configurable 11-bit Priority ID (\$0x000 \\dots 0x7FF\$) with bus starvation alerts.  
   - SDU Type registry mapping (`0x00` Content-addressed, `0x01` Classical CAN/CAN FD, `0x02` Ethernet CiA 611-1, `0x03` IP, `0x04` AUTOSAR SecOC).  
   - 8-bit Virtual CAN ID (VCID) domain tagging for multi-tenant virtualization.  
   - 32-bit Acceptance Field filtering configuration.  
   - Up to 2,048 bytes payload generation with automated DLC mapping.

   

2. **CiA 611-1 Ethernet Encapsulation Engine**  
     
   - Direct packetization of IEEE 802.3 Ethernet frames (Destination MAC, Source MAC, EtherType, Payload).  
   - Automated padding and frame size computation without IP fragmentation.  
   - Real-time dissection back into structured L2 Ethernet packets.

   

3. **Dual CRC Validation & Calculation Engine**  
     
   - **13-bit PCRC**: Bit-exact generator using generator polynomial \$x^{13} \+ x^{11} \+ x^9 \+ x^8 \+ x^6 \+ x^5 \+ x^2 \+ 1\$ (`0x2B65`).  
   - **32-bit FCRC**: Bit-exact generator using ISO 11898-1 polynomial \$x^{32} \+ x^{26} \+ x^{23} \+ x^{22} \+ x^{16} \+ x^{12} \+ x^{11} \+ x^{10} \+ x^8 \+ x^7 \+ x^5 \+ x^4 \+ x^2 \+ x \+ 1\$ (`0x04C11DB7`).  
   - Bit error injection mode for negative ECU testing and fault injection validation.

   

4. **10-Point Normative Zero-Trust Compliance Auditor**  
     
   - Automatically runs bitstream through 10 automotive validation assertions:  
     - `RULE-CANXL-01`: XLF=1 & FDF=1 protocol discrimination flags.  
     - `RULE-CANXL-02`: Priority ID 11-bit boundary compliance.  
     - `RULE-CANXL-03`: Data length bounds (\$\\le 2,048\$ bytes).  
     - `RULE-CANXL-04`: SDU Type standard registration validation.  
     - `RULE-CANXL-05`: Virtual CAN ID (VCID) boundary enforcement.  
     - `RULE-CANXL-06`: Acceptance Field 32-bit bounds validation.  
     - `RULE-CANXL-07`: 13-bit PCRC mathematical integrity check.  
     - `RULE-CANXL-08`: 32-bit FCRC mathematical integrity check.  
     - `RULE-CANXL-09`: CiA 611-1 Ethernet header validation (when SDU=0x02).  
     - `RULE-CANXL-10`: Bus contention / starvation warning check.

   

5. **Universal Dual-Execution Model**  
     
   - **Interactive Web Studio**: Standalone single-page web app with zero external CDN dependencies, fully functional offline.  
   - **Python CLI Utility (`canxl-cli.py`)**: Zero-dependency standard library CLI tool supporting automated CI/CD pipelines, HIL testing, and batch trace analysis.

---

## 3\. Directory Layout

2026-10-11 — CANXLForge Pro/

├── README.md                                \# Root product specification

├── build/

│   ├── canxl-cli.py                        \# Standalone Python CLI runner

│   └── canxl-pro-v1.0.0.zip                \# Release distribution archive

├── documentation/

│   ├── ARCHITECTURE.md                     \# Protocol specification & CRC math

│   ├── USER\_GUIDE.md                       \# Operator manual & CLI reference

│   └── MONETIZATION\_AND\_MARKETING\_PLAYBOOK.md \# Commercialization & GTM strategy

├── screenshots/

│   └── canxl-studio-preview.svg            \# UI vector screenshot

└── source-code/

    ├── index.html                          \# Engineering web studio application

    ├── canxl.css                           \# Dark theme automotive UI styles

    ├── canxl-engine.js                     \# Core frame codec & CRC math engine

    ├── canxl-auditor.js                    \# 10-point normative compliance ruleset

    └── app.js                              \# UI interactive application controller

---

## 4\. Quickstart Guide

### Web Application

Simply open `source-code/index.html` in any modern web browser (Chrome, Edge, Firefox, Safari). No web server, Node.js, or external Internet connection is required.

### CLI Usage (Python 3.8+)

The CLI utility requires zero third-party packages:

\# Synthesize a standard CAN XL Frame

python3 build/canxl-cli.py frame \--prio 0x100 \--sdu 0x00 \--vcid 0x01 \--af 0x12345678 \--data "Hello CAN XL"

\# Encapsulate an IEEE 802.3 Ethernet packet (CiA 611-1)

python3 build/canxl-cli.py eth \--prio 0x07A \--vcid 0x01 \--dst 02:00:00:00:00:02 \--src 02:00:00:00:00:01 \--type 0x0800 \--payload "Automotive Diagnostics Payload"

\# Run 10-point normative compliance audit

python3 build/canxl-cli.py audit \--prio 0x050 \--sdu 0x02 \--vcid 0x01 \--af 0x11223344 \--data "Ping Test"

### Running Automated Test Suite

python3 test\_canxl.py

Expected output:

\============================================================

CANXLForge Pro \- ISO 11898-1:2024 Automated Verification Suite

\============================================================

\[PASS\] TEST 1: 13-bit PCRC Calculation

\[PASS\] TEST 2: 32-bit FCRC Calculation

\[PASS\] TEST 3: CAN XL Frame Synthesis

\[PASS\] TEST 4: CiA 611-1 Ethernet Encapsulation

\[PASS\] TEST 5: Raw Frame Dissection

\[PASS\] TEST 6: Normative Auditor \- Compliant Frame

\[PASS\] TEST 7: Normative Auditor \- Length Violation Flagged

\[PASS\] TEST 8: Normative Auditor \- SDU Type Violation Flagged

\[PASS\] TEST 9: Normative Auditor \- Priority Starvation Warning

\[PASS\] TEST 10: Fault Injection \- FCRC Corruption Detected

\============================================================

RESULTS: 10 Passed, 0 Failed (100.0% Pass Rate)

\============================================================

---

## 5\. Commercial Licensing & Target Market

- **Target Customers**: Automotive OEMs (BMW, Mercedes-Benz, Stellantis, Ford, GM), Tier-1 ECU Suppliers (Bosch, Continental, ZF, Denso), Automotive Tooling Vendors (Vector Informatik, Peak-System), Semiconductor Validation Teams (NXP, Infineon, TI, Renesas).  
- **Pricing & Packaging**:  
  - **Single Engineer Desktop License**: \$1,490 / seat / year  
  - **Tier-1 Team Site License (Floating 10 seats)**: \$8,900 / year  
  - **OEM Enterprise CI/CD & HIL Automation Royalty-Free Core**: \$24,500 one-time perpetual license  
- **Monetization Roadmap**: Detailed in `documentation/MONETIZATION_AND_MARKETING_PLAYBOOK.md`.

---

*CANXLForge Pro is developed and certified by the Autonomous Digital Product Factory.*