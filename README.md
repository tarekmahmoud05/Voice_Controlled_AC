# 🚗 Voice-Controlled Automotive Cabin Air Management System

An embedded systems project that automates rear-cabin climate control in vehicles, using dual temperature sensing, PWM-driven fan/hood logic, a custom motor driver shield PCB, and a Python voice-recognition interface for hands-free operation.

Built as part of the **Embedded Systems** and **Manufacturing of Electronic Components** modules — Electrical and Electronics Engineering, Coventry University.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Problem Statement](#-problem-statement)
- [System Architecture](#-system-architecture)
- [Hardware](#-hardware)
- [Control Logic](#-control-logic)
- [Voice Recognition Interface](#-voice-recognition-interface)
- [Repository Structure](#-repository-structure)
- [Getting Started](#-getting-started)
- [Testing](#-testing)
- [Results](#-results)
- [Future Work](#-future-work)
- [Team](#-team)
- [References](#-references)

---

## 🔍 Overview

Solar radiation through the rear windshield creates a "greenhouse effect" that conventional front-vent HVAC systems can't reach, with rear-cabin temperatures documented to exceed ambient by up to 47°C under direct sun. This forces manual driver adjustments mid-drive — a known distraction risk — and increases compressor load, raising fuel consumption by roughly 8%.

This project tackles that with a **roof-mounted ventilation system** built around an **AVR ATmega328P**, which autonomously switches between **intake (Fan Mode)** and **exhaust (Hood Mode)** based on real-time temperature deltas between the cabin and the outside air. A voice-controlled desktop app lets the driver set a target temperature hands-free, and a custom-fabricated PCB drives the motor.

## ❗ Problem Statement

- Rear cabin glazing causes thermal stratification untouched by standard HVAC vents
- Manual rear-climate adjustment while driving is a documented distraction hazard
- Compensating compressor load increases fuel consumption

## 🏗️ System Architecture

![System Architecture Diagram](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/tarekmahmoud05/Voice_Controlled_AC/main/diagrams/architecture.puml)

The firmware is organized into modular driver layers (ADC, DIO, PWM, UART, LCD, Button, String utilities), each with its own `.ino`/`.h` pair, integrated in a top-level main file. Full module breakdown, pin mapping, and control-flow diagrams are in [`Project Design/CW3_project_design.md`](Project Design/CW3_project_design.md).

## 🔧 Hardware

| Component | Function |
|---|---|
| ATmega328P (AVR) | Processes control logic, ADC inputs, and PWM output for the fan |
| Dual LM35 Temperature Sensors | Monitor cabin (`inTemp`) and ambient (`outTemp`) temperature |
| DC Fan | Roof-mounted intake/exhaust airflow |
| 16x2 LCD Display | Real-time temperature and system status |
| Push Button | Manual Start/Stop toggle and target-temperature trigger |
| L293D IC | Dual H-Bridge for bidirectional motor control |
| L7805 CV | 5V voltage regulator for stable logic power |
| Custom Motor Driver Shield (PCB) | In-house designed, milled, and soldered 2-layer shield carrying the L293D |
| Resistors/Capacitors | UART pull-ups and decoupling |

### Custom Motor Driver Shield (PCB)

Designed and fabricated as part of the Manufacturing of Electronic Components module:

1. **Circuit Design** — Schematic design of the motor driver shield
2. **Layout Design** — 2-layer PCB layout in Altium
3. **File Conversion** — G-code generation from Altium Gerber data
4. **Surface Mapping** — Auto-leveling the FR4 board for uniform milling depth
5. **Isolation Routing** — CNC milling of copper boundaries with a V-bit
6. **Drilling** — CNC drilling for through-hole component leads
7. **Post-Processing** — Deburring, cleaning, solder mask application
8. **Soldering** — Manual soldering with multi-point continuity validation

PCB schematics, layout files, and Gerber/G-code outputs are in [`pcb/`](pcb).

## 🧠 Control Logic

**Pin Mapping**

| Component | Pin | Type | Function |
|---|---|---|---|
| Internal Temp Sensor | PC5 | Analog In | Cabin temperature (`inTemp`) |
| External Temp Sensor | PC0 | Analog In | Ambient temperature (`outTemp`) |
| Control Button | PB4 | Digital In | Start/Stop toggle, triggers UART command |
| Motor Direction 1 | PD2 | Digital Out | Direction control (w/ PD3) |
| Motor Direction 2 | PD3 | Digital Out | Direction control (w/ PD2) |
| Motor Speed (PWM) | PB2 (OC1B) | PWM Out | Duty cycle 0–255 |
| LCD Data (D4–D7) | PD4–PD7 | Digital Out | 4-bit LCD data bus |
| LCD Enable | PB1 | Digital Out | LCD enable signal |
| LCD Register Select | PB0 | Digital Out | LCD command/data select |

**Fan/Hood decision logic:**
![Fan Hood Decision Logic](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/tarekmahmoud05/Voice_Controlled_AC/main/diagrams/logic.puml)

Speed is mapped from the temperature difference onto an 8-bit PWM duty cycle (165–255 range, set by Timer 1 in Fast PWM Mode 14), generated via `OCR1B`. Direction is set through PD2/PD3, and motor commands are wrapped in `fan()`, `hood()`, and `stop()` helper functions on top of low-level `DC_Start`/`DC_Stop` calls.

Full firmware control flow, initialization routines, and the UART status-string framing are documented in [`docs/CW3_project_design.md`](docs/CW3_project_design.md).

## 🎙️ Voice Recognition Interface

A Python desktop application provides hands-free target-temperature control:

1. **Audio Capture** — PyAudio with ambient noise filtering
2. **Speech Processing** — Speech-to-text via the Google Speech Recognition API
3. **Command Parsing** — Keyword filtering to extract the numeric setpoint
4. **UART Bridge** — Sends the validated value to the ATmega328P at 9600 bps
5. **GUI Feedback** — Real-time system status and target temperature via TTKBootstrap

**Workflow:** press the button to start recording → speak the desired temperature → press the button again to stop recording and send it to the MCU.

## 📁 Repository Structure

![Repository Structure Tree](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/tarekmahmoud05/Voice_Controlled_AC/main/diagrams/structure.puml)



## 🚀 Getting Started

### Hardware Requirements
- Arduino UNO / ATmega328P
- Dual LM35 temperature sensors
- L293D motor driver IC (on the custom shield, or a breakout)
- DC fan
- 16x2 LCD (4-bit mode)
- Push button
- L7805 voltage regulator
- USB cable for UART/serial connection

### Firmware Setup
1. Open `firmware/voice_Controlled_AC.ino` in the Arduino IDE
2. Connect components according to the [pin mapping](#-control-logic) above
3. Select board: **Arduino UNO**
4. Upload the sketch



## ✅ Testing

The `tests/` directory contains automated and semi-automated Python test scripts used to validate:
- ADC temperature readings against known reference values
- Serial communication / UART protocol framing
- Fan/Hood decision logic across the cabin/ambient temperature matrix
- Voice command parsing accuracy

Demonstration videos of the full system in operation are available in same directory.

## 📊 Results

Experimental testing showed the system reliably reduces thermal stagnation in the rear cabin and switches correctly between intake and exhaust modes according to the temperature differential logic, while the voice interface reduced the need for manual driver interaction with the climate controls.

## 🔮 Future Work

- **Sensor Fusion** — Add humidity and CO₂ sensors for air quality management and glass defogging, beyond just temperature
- **Wireless Connectivity** — Replace wired UART with Bluetooth/Wi-Fi for a fully wireless smartphone/PC-to-vehicle link
- **ML-Based Optimization** — Lightweight model to predict user temperature preferences from historical data and outdoor weather conditions

## Mady by

**Eng** Tarek Mahmoud Younes,
Student at **Coventry University**,
Specialized in **Electrical and Electronics Engineering**

**Supervised by:** Dr. Mostafa Rabie, Eng. Medhat Toubar
**School of Engineering — Electrical and Electronics Engineering, Coventry University**

## 📚 References

1. Alkhalaf, A., et al. (2021). *Investigation of the Effect of Solar Ventilation on the Cabin Temperature of Vehicles Parked under the Sun.* [mdpi.com](https://www.mdpi.com/2071-1050/13/24/13963)
2. Farrington, R. & Rugh, J. (2000). *Impact of vehicle air-conditioning on fuel economy.* [nrel.gov](https://www.nrel.gov/docs/fy00osti/28960.pdf)
3. NHTSA (2021). *Driver distraction and in-vehicle climate controls.* [crashstats.nhtsa.dot.gov](https://crashstats.nhtsa.dot.gov/Api/Public/ViewPublication/813064)

---

*If you use or build on this project, a star ⭐ on the repo is always appreciated.*
