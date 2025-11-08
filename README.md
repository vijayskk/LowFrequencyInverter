# **Low-Frequency Inverter – Complete Project Documentation**

## 1. **Project Overview**

This project implements a **low-frequency DC–AC inverter** designed to convert low-voltage DC (typically 12–24 V) into a 50/60 Hz AC output suitable for powering appliances such as lamps, small fans, and resistive loads.
The design employs an **H-bridge MOSFET topology** with a dedicated high-side/low-side driver to achieve AC generation.
A microcontroller interface enables waveform generation via PWM, allowing flexible control and easy experimentation.

The PCB layout provides proper power handling, safety mechanisms, external load switching, and auxiliary connectors.

---

## 2. **Key Features**

* Low-frequency AC generation (50/60 Hz)
* H-bridge MOSFET output stage
* IR2104 high-/low-side gate driver
* Solar & battery power support
* Fast Schottky rectifiers for freewheeling
* Safety fuse protection
* Relay load control
* Indicator LEDs
* Fan output for cooling
* MCU interface for PWM control
* Expandable, modular architecture

---

## 3. **Applications**

* DC–AC inverter learning platform
* Backup/portable power supply
* Solar micro-inverter
* Motor/inductive load driving (low frequency)
* Lab experiments, academic projects

---

## 4. **Expanded Theory of Operation**

A low-frequency inverter converts DC input into alternating AC output. This design outputs **square-wave AC** using an H-bridge structure.

### 4.1 H-Bridge Power Stage

Four N-channel MOSFETs (IRF520) form the inverter H-bridge.
Only one diagonal pair conducts at a time to reverse polarity at the output:

```
ON → Q1 + Q4  → +V
ON → Q2 + Q3  → –V
```

This alternating conduction produces a square-wave AC signal.

### 4.2 Gate Driving — IR2104

High-side MOSFETs require gate voltages greater than the supply to turn ON. IR2104 serves as the high/low-side driver and provides:

* Level-shifting
* Proper MOSFET gate voltage
* Shoot-through protection
* Control logic from MCU

### 4.3 Control Signal

An external microcontroller generates complementary PWM with dead time to avoid simultaneous conduction.

### 4.4 Output Handling

Output is a **square wave**.
For 230 VAC output, an external transformer is needed:

* H-bridge → transformer → AC Load

For improved waveform quality, an LC filter may be added.

### 4.5 Protection Network

Schottky diodes (MBR1560) handle:

* Inductive kickback
* Freewheeling current
  Safety fuse and relay provide overload control and safe switching.

---

## 5. **System Block Diagram**

```
           +----------+
           |  Battery |
           |  12–24V  |
           +----+-----+
                |
           +----+-----+
           |  Fuse    |
           +----+-----+
                |
           +----+-----+
           |  H-Bridge |
           | MOSFETs   |
           +----+------+
                |
        +-------+------+
        |              |
+-------+-------+   Transformer (optional)
| IR2104 Driver |--------------> AC Out
+-------+-------+
        |
   +----+-----+
   |   MCU    |
   | PWM Ctrl |
   +----------+
```

---

## 6. **Hardware Architecture**

| Block             | Description                              |
| ----------------- | ---------------------------------------- |
| DC Input          | Provides primary supply (battery/solar)  |
| H-Bridge          | Switches DC to alternating AC            |
| Gate Driver       | IR2104 controls MOSFET gates             |
| Rectification     | MBR1560 diodes provide fast freewheeling |
| Control Interface | MCU pin header                           |
| Output Protection | Fuse + relay                             |
| Indicators        | Status LEDs                              |
| Cooling           | Fan output                               |
| Filter Network    | Optional LC smoothing                    |

---

## 7. **Component Specifications**

### ✅ **IRF520 – Power MOSFET**

| Parameter         | Value   |
| ----------------- | ------- |
| Vds               | 100 V   |
| Id                | 9.2 A   |
| Rds(on)           | ~0.27 Ω |
| Power dissipation | 60 W    |
| Package           | TO-220  |

Function: Power switching inside H-bridge.

---

### ✅ **IR2104 – Gate Driver**

| Parameter         | Value       |
| ----------------- | ----------- |
| Supply Vcc        | 10–20 V     |
| High-side voltage | up to 600 V |
| Output current    | 200 mA      |
| Logic             | 3.3/5 V     |

Function: Drives high-side and low-side MOSFETs safely.

---

### ✅ **MBR1560 – Schottky Rectifier**

| Parameter | Value   |
| --------- | ------- |
| VRRM      | 60 V    |
| Current   | 15 A    |
| Vf        | ~0.75 V |

Function: Freewheeling, rectification.

---

### ✅ **Inductor – 19 Turns**

Function: LC filtering / boost support.

---

### ✅ Connectors

* BAT — DC input
* LOAD — AC output
* FAN — Cooling fan
* MCU — PWM interface

---

## 8. **Electrical Specifications**

| Parameter        | Specification                |
| ---------------- | ---------------------------- |
| DC Input         | 12–24 V                      |
| Output Frequency | 50–60 Hz                     |
| Output Voltage   | Battery level square-wave    |
| High-voltage AC  | Via transformer              |
| Output Power     | ≤ ~120 W (cooling dependent) |
| Efficiency       | ~80–90%                      |
| Protection       | Fuse + MOSFET isolation      |

> Note: For 220–230 VAC, external transformer is required.

---

## 9. **Installation & Usage Guide**

### 9.1 Requirements

| Item        | Notes                  |
| ----------- | ---------------------- |
| DC Source   | 12–24 V battery/solar  |
| MCU         | ATmega / STM32 / ESP32 |
| Load        | ≤120 W                 |
| Heatsinks   | Mandatory              |
| Transformer | For 230 VAC output     |

---

### 9.2 Setup Procedure

1. Connect DC input at BAT terminal
2. Connect MCU PWM signals to interface header
3. Connect load at LOAD terminals
4. (Optional) Connect transformer for high voltage AC
5. Attach heatsinks & connect FAN
6. Power ON & enable PWM

---

### 9.3 PWM Guidelines

| Parameter | Value       |
| --------- | ----------- |
| Frequency | 50–60 Hz    |
| Waveform  | Square      |
| Dead Time | 300–1000 ns |

---

### 9.4 Troubleshooting

| Symptom        | Cause          | Fix             |
| -------------- | -------------- | --------------- |
| No AC output   | No PWM         | Check MCU       |
| MOSFET heating | No dead time   | Fix code        |
| Weak output    | No transformer | Add transformer |
| Loud hum       | Square wave    | Normal          |
| Fuse blows     | Overload/short | Inspect MOSFETs |

---

## 10. **PCB Features**

* IR2104 near MOSFETs for minimal gate trace
* Wide power traces
* Decoupling capacitors at supply inputs
* TO-220 MOSFET layout w/ heatsinks
* Clearly labeled silkscreen
* MCU pin header for easy control
* Fuse + relay protection
* Fan terminal

---

## 11. **Bill of Materials (BOM)**

| Item | Qty | Reference | Description                 |
| ---: | :-: | --------- | --------------------------- |
|    1 |  4  | Q1–Q4     | IRF520 MOSFET               |
|    2 |  1  | U26       | IR2104 Gate Driver          |
|    3 |  2  | D2,D3     | MBR1560CT Schottky          |
|    4 |  1  | U17       | 19-turn inductor            |
|    5 |  1  | Relay     | Load switching              |
|    6 |  1  | Fuse      | CQ rating                   |
|    7 |  1  | Header    | MCU interface               |
|    8 |  —  | Caps      | 0.1 µF, 1 µF, 10 µF, 100 µF |
|    9 |  —  | Res       | 1 kΩ, 2.2 kΩ, 10 kΩ         |
|   10 |  1  | Connector | FAN                         |
|   11 |  1  | Connector | BAT                         |
|   12 |  1  | Connector | LOAD                        |
|   13 |  4  | —         | TO-220 heatsinks            |

---

## 12. **Advantages**

* Simple, modular, reproducible
* Low cost
* High-current MOSFET stage
* Safe, protected design
* MCU friendly
* Expandable

---

## 13. **Suggested Improvements**

| Improvement             | Benefit           |
| ----------------------- | ----------------- |
| IRLZ44N/IRF3205 MOSFETs | Lower heat        |
| Temp sensing            | Automatic cooling |
| Current sensing         | Safety            |
| LC filter               | Sine-wave output  |
| Display interface       | Better UX         |
| MPPT front-end          | Solar efficiency  |

---

## 14. **Conclusion**

This Low-Frequency Inverter is a **compact, reliable, and educational** power conversion system.
It efficiently converts low-voltage DC into AC using an H-bridge MOSFET network controlled by the IR2104 gate driver.
The design supports an external MCU for flexible waveform control and provides core safety elements such as fuse protection, cooling support, and relay switching.

Its modular, well-laid-out PCB makes it ideal for:

* Students
* Experimenters
* Embedded developers
* Solar + power electronics research

---

## 15. **License**
This is an open-source project issued with standard MIT OpenSource License.See the LICENSE file for further more information.
