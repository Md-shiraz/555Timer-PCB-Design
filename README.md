# 555 Timer DC Motor Controller

A KiCad hardware project for an adjustable 555-timer-driven DC motor switch. An LM555 generates a control signal, and an N-channel MOSFET switches the motor on the low side. A potentiometer and diode/resistor network set the timing behavior. The design is intended for learning and prototyping; it is not a production-ready motor driver.

## Project contents

| File | Description |
| --- | --- |
| `555Timer.kicad_pro` | KiCad project settings |
| `555Timer.kicad_sch` | Circuit schematic |
| `555Timer.kicad_pcb` | PCB layout |
| `docs/control-circuit.png` | Schematic overview |
| `docs/pcb-top-view.png` | 3D view of the PCB |

Open `555Timer.kicad_pro` in KiCad to access the schematic and board together.

## How the circuit works

The LM555 (`U2`) is connected as an oscillator: its trigger and threshold inputs share the timing node, with a 100 kΩ potentiometer (`RV1`), two 1N4007 diodes, and resistors forming the timing network. The diodes provide separate paths for timing-capacitor charge and discharge. Adjusting the potentiometer changes the generated signal's timing and duty cycle.

The timer output drives the gate of `Q1`, an N-channel MOSFET. The MOSFET switches the motor (`M1`) between the positive supply and ground, so the 555 controls the motor without carrying its load current. `R3` is connected as a gate pull-down. `C1` and `C2` are 0.1 µF capacitors in the timer circuit.

This is an open-loop controller: it does not measure motor speed or regulate it against load changes. The resulting motor behavior depends on the supply, motor, potentiometer setting, and load.

## Schematic

![Control circuit overview](docs/control-circuit.png)

## PCB

![PCB top view](docs/pcb-top-view.png)

The board layout includes the timer and timing components, a potentiometer footprint, a MOSFET footprint, and a two-position screw terminal for the motor connection. Confirm the board's connector assignments and polarity against the schematic and PCB before wiring.

## Main components

| Reference | Part/value | Role |
| --- | --- | --- |
| `U2` | LM555xM | Timer/oscillator |
| `Q1` | N-channel MOSFET (`Q_NMOS_GDS` symbol) | Low-side motor switch |
| `M1` | DC motor (`Motor_DC` schematic symbol) | Switched load |
| `RV1` | 100 kΩ potentiometer | Timing adjustment |
| `R1`, `R2`, `R3` | 10 kΩ each | Timing and gate-bias network |
| `D1`, `D2` | 1N4007 | Timing-network steering diodes |
| `C1`, `C2` | 0.1 µF each | Timer capacitors |

The MOSFET symbol is generic; select a specific part only after checking its pinout, gate-drive requirements, voltage rating, current rating, and thermal limits for the intended motor and supply.

## Getting started

1. Install KiCad 8 or newer.
2. Open `555Timer.kicad_pro`.
3. Review the schematic and PCB together. Check the MOSFET pin mapping, motor-terminal polarity, component values, and footprints against the parts you intend to use.
4. Run the Electrical Rules Checker (ERC) on the schematic and the Design Rules Checker (DRC) on the PCB. Resolve errors and review warnings; do not assume a passing check establishes that the design is electrically or thermally suitable.
5. For fabrication, plot the required Gerber copper, solder-mask, silkscreen, and board-outline layers, and generate drill files. Inspect the generated files and confirm the manufacturer's capabilities before ordering.

## Before powering or manufacturing

- The project does not specify a validated operating-voltage or motor-current range. Establish these limits from the selected components and board layout before use.
- Verify that the MOSFET is suitable for the timer's gate voltage and the motor's startup/stall current; check dissipation and heatsinking as applicable.
- Check whether the motor needs additional transient suppression. The timing diodes are not a substitute for a flyback clamp across an inductive load.
- Check copper widths, connector ratings, clearances, soldering, and mechanical fit for the actual application.
- Start testing with a current-limited supply and an appropriate low-risk load, and monitor the MOSFET and motor for excessive heating.

This is a prototype design and should be reviewed and bench-tested before use. The included images are visual references; the KiCad schematic and PCB files are the editable design sources.
