# Text-Based Electronic Diagram Markup

A lightweight, text-based markup system for representing electronic circuits using **Unicode symbols** and short **3–4 letter fallbacks**. Designed for quick documentation, educational purposes, and creating diagrams that are portable across plain-text platforms.

---

## Features

- Compact symbols for common components (resistors, capacitors, LEDs, logic gates, etc.)
- Clear line and node syntax for defining circuit paths
- Supports parallel and series connections with distribution (`DST`) and connection (`CON`) points
- Fully readable even if Unicode symbols do not render (fallbacks like `LED`, `RES`)

---

## Symbol Dictionary (Short Fallbacks)

| Symbol       | Component                | Fallback |
|--------------|-------------------------|----------|
| ⟤           | Resistor                | RES      |
| ⏚           | Ground                  | GND      |
| ∿            | Sine Wave               | SIN      |
| ⎍            | Square Wave             | SQW      |
| Ω            | Ohm                     | OHM      |
| μ            | Micro                   | MIC      |
| ⌥            | Distribution Point      | DST      |
| ⏧            | Connection Point / Jct  | CON      |
| ⤚            | Socket                  | SOC      |
| ⟞±⊢        | Battery                 | BAT      |
| ⊣⊢           | Capacitor               | CAP      |
| ⊣⊢+          | Polarized Capacitor     | CAPP     |
| ⅏            | Inductor                | LDR      |
| ⏛            | Fuse                    | FUS      |
| ⏄            | Diode                   | DIO      |
| ⏄⭎⭎        | LED                     | LED      |
| ⭗            | Button                  | BTN      |
| ⧎            | Diac                    | DIA      |
| ⊼            | NAND Gate               | NND      |
| ⋀            | AND Gate                | AND      |
| ⋁            | OR Gate                 | OR       |
| ⊽            | NOR Gate                | NOR      |
| ¬            | NOT Gate                | NOT      |
| -⊻           | XOR Gate                | XOR      |
| ⨳            | Bridge Rectifier        | BRG      |
| ⛒            | Lamp                    | LMP      |
| ⍾            | Bell                    | BEL      |
| ᛉ            | Antenna                 | ANT      |
| Ⓜ            | Motor                   | MOT      |
| Ⓥ            | Voltmeter               | VOLT     |
| Ⓐ            | Ammeter                 | AMM      |

---

## Syntax Rules

1. **Line Format**

Every connection is defined as:

[From_Node]: [Components] [To_Node]


- Example:

BAT: RES CON1
CON1: LED 


2. **Components Exist Between Nodes**

- Components do not create new nodes.
- Components are placed on the path from one node to another.

3. **Distribution Points (`DST`)**

- Split a line into multiple branches:

DST1: RES CON1
DST1: CAP CON2


4. **Connection Points (`CON`)**

- Merge multiple branches:

DST1: RES CON1
DST1: CAP CON1
CON1: LED


5. **Flow Reading Order**

- Default: **top-to-bottom**
- Sequence: distribution → branches → connection → next node

6. **Chained Components**

- Multiple components can exist on a single line:

BAT: RES DIO CON1

---

## Example: Complex Circuit

A circuit with a battery, resistor network, capacitors, diodes, LED, lamp, and logic gates:

BAT: RES DST1

DST1: CAP CON1

DST1: DIO CON2

CON1: LED CON3

CON2: RES CON3

CON3: LMP

BAT: CON4

CON4: AND CON5

CON5: OR CON3

DST2: RES CON5

DST2: CAP CON4

**Explanation:**

- `BAT: RES CON1` → battery feeds a distribution point with a resistor.
- Branches from `CON1`:
  - `CAP CON1` → capacitor to connection point 1
  - `DIO CON2` → diode to connection point 2
- Connection points merge:
  - `CON1: LED CON3` → LED continues to lamp
  - `CON2: RES CON3` → resistor continues to same lamp node
- Parallel logic gate branch:
  - `BAT: ⊣⊢+ CON4` → polarized capacitor feeds AND gate
  - `CON4: AND CON5` → AND output flows to OR gate
  - `CON5: OR CON3` → OR output merges at lamp
- `DST2` → additional resistor and capacitor feeding the logic path

---
