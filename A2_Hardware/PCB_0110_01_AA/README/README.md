# Isolated USB to RS-485 / RS-422 Converter

A compact, galvanically isolated **USB 2.0 to RS-485 / RS-422 communication interface** designed as a complete hardware engineering project, from system architecture and schematic design to PCB layout and manufacturing documentation.

The design provides dedicated RS-485 and RS-422 communication channels while maintaining galvanic isolation between the USB host and the field-side communication circuitry.

---

## Project Overview

The purpose of this project is to develop a robust USB interface for industrial serial communication networks.

The converter connects to a host computer through USB and provides:

- Half-duplex RS-485 communication
- RS-422 communication
- Galvanic isolation between USB and field side
- Isolated power for the communication interface
- Hardware transient protection
- Software-controlled termination
- Automatic receive-path selection
- USB-powered operation

The hardware was designed with industrial communication environments and EMC robustness in mind.

---

## System Architecture

```text
                         USB SIDE
                            │
                         USB-C
                            │
                   USB Protection
                            │
                       FT230XS
                      USB ↔ UART
                            │
              ┌─────────────┴─────────────┐
              │                           │
       Digital Isolation           Digital Isolation
              │                           │
              │                    Isolated Power
              │                           │
              └─────────────┬─────────────┘
                            │
                        FIELD SIDE
                            │
               ┌────────────┴────────────┐
               │                         │
          RS-485 Channel            RS-422 Channel
          ADM3065E                  ADM3065E
               │                         │
          Protection                 Protection
               │                         │
             A / B                   TX / RX Pairs
```

---

## Main Hardware

### USB Interface

The USB interface is based on the **FTDI FT230XS** USB-to-UART bridge.

The design uses a USB Type-C connector in USB 2.0 configuration.

The USB interface includes:

- USB Type-C connector
- USB ESD protection
- Common-mode filtering
- USB-to-UART conversion
- 3.3 V logic supply
- TX/RX status indication

The UART communication rate is designed around the capabilities of the FT230XS, supporting communication rates up to approximately **3 Mbaud**.

---

## Galvanic Isolation

The USB side and field communication side are galvanically isolated.

The isolation architecture transfers:

- UART TX
- UART RX
- Driver-enable control
- Termination control
- Auxiliary control signals

The isolated side operates from an isolated power domain, preventing direct electrical connection between the USB host ground and the RS-485 / RS-422 field-side ground.

This helps reduce problems caused by:

- Ground potential differences
- Ground loops
- Common-mode disturbances
- Industrial electrical noise

---

## RS-485 / RS-422 Interface

Two **Analog Devices ADM3065E** transceivers are used for the field communication interfaces.

Separate transceiver channels are provided for RS-485 and RS-422 operation.

The design includes:

- True fail-safe receiver behavior
- Differential communication
- Configurable termination
- Transient protection
- EMC-oriented component placement
- Dedicated field-side ground domain

The receive signals from the communication channels are combined before being returned to the USB-UART interface.

The architecture assumes that only one physical communication interface is actively connected at a time.

---

## Automatic Receive Path Selection

No mechanical RS-485 / RS-422 mode-selection switch is required.

The receive outputs are combined through logic before being connected to the UART RX input.

Because the unused receiver remains in its fail-safe HIGH state, the active communication channel can pull the combined RX signal LOW when receiving data.

Conceptually:

```text
RS485_RX ───┐
            ├── AND ──> UART_RX
RS422_RX ───┘
```

This allows the hardware to support the two receive paths without requiring a manual mode-selection jumper.

---

## Software-Controlled Termination

Bus termination is controlled electronically rather than with mechanical switches or jumpers.

The termination network uses **PhotoMOS solid-state relays** to connect or disconnect the termination resistor.

This allows termination to be controlled from the USB-UART interface.

Conceptually:

```text
A ───── 120 Ω ───── PhotoMOS ───── B
```

The termination system is designed so that the interface can be configured without opening the enclosure or changing PCB jumpers.

---

## Protection Architecture

The communication interface includes a multi-stage protection strategy intended to improve robustness against transient events on external communication lines.

The protection concept includes:

- TVS protection
- Series transient blocking
- Surge protection components
- Optional EMC tuning components
- Differential-line filtering

The design provides footprints and component options that allow the protection network to be optimized during EMC and validation testing.

---

## PCB Design

The PCB was designed in **Altium Designer**.

Important PCB design considerations include:

- Differential pair routing
- Short and symmetric RS-485 / RS-422 paths
- Continuous field-side reference plane
- Isolation-area separation
- Controlled USB differential routing
- Protection components placed close to external interfaces
- EMC-oriented return-current paths
- Separation of USB and isolated field-side domains

The RS-485 / RS-422 PCB traces are treated as differential signal pairs while the actual bus characteristic impedance is primarily defined by the external twisted-pair cable and termination network.

---

## Design Philosophy

The project follows a complete hardware development workflow:

```text
Requirements
     ↓
System Architecture
     ↓
Component Selection
     ↓
Schematic Design
     ↓
PCB Design
     ↓
Design Review
     ↓
Manufacturing Documentation
     ↓
Prototype / Bring-Up
     ↓
Validation & EMC Testing
```

The design includes several configurable footprints and DNP options so that component values can be optimized after prototype measurements and EMC testing.

---

## Repository Structure

```text
.
├── A0_Document/
├── A1_Document/
├── A2_Hardware/
│   └── PCB_0110_01_AA/
│       ├── pcb/
│       ├── sch/
│       └── user manuals/
├── A3_Mechanic/
├── A4_Cable/
└── A5_Software/
```

### A0_Document
General project documentation.

### A1_Document
Engineering and design-related documents.

### A2_Hardware
Hardware design files including Altium schematic and PCB sources.

### A3_Mechanic
Mechanical design files and enclosure-related documentation.

### A4_Cable
Cable and harness documentation.

### A5_Software
Software, configuration utilities, or supporting code related to the project.

---

## Key Components

| Function | Component |
|---|---|
| USB to UART | FTDI FT230XS |
| RS-485 / RS-422 Transceiver | Analog Devices ADM3065E |
| 3.3 V Regulation | LP5907 |
| Digital / Power Isolation | Isolated interface modules |
| Termination Switching | Vishay VO1400AEFTR PhotoMOS |
| USB Protection | ESD protection + common-mode filtering |
| Bus Protection | Multi-stage transient protection |

---

## Design Tools

- **Altium Designer** – Schematic capture and PCB design
- Component datasheets and manufacturer reference designs
- PCB design-rule and signal-integrity considerations
- EMC-oriented hardware design methodology

---

## Project Status

**Hardware design phase**

Current development activities include:

- Schematic review
- PCB layout optimization
- Protection-network evaluation
- Manufacturing documentation
- Design-for-EMC review

Prototype measurements and EMC validation will be used to finalize configurable component values.

---

## Disclaimer

This project is an engineering development and portfolio project.

Component values, protection networks, termination configurations, and EMC-related options may be modified following prototype testing and laboratory validation.

---

## Author

**Cemil Kendir**  
Electronics / Embedded Hardware Engineer

Hardware architecture • Schematic design • PCB design • Industrial communication • EMC-oriented design