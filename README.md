# Text-Based Electronic Diagram Markup

A lightweight, text-based markup system for representing electronic circuits using **Unicode symbols** and short **3–4 letter fallbacks**. Designed for quick documentation, educational purposes, and creating diagrams that are portable across plain-text platforms.

---

## Features

- Compact symbols for common components (resistors, capacitors, LEDs, logic gates, etc.)
- Clear line and node syntax for defining circuit paths
- Supports parallel and series connections with distribution (`DST`) and connection (`CON`) points
- Component value and label specification with parentheses syntax
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
```
[From_Node]: [Components] [To_Node]
```

- Example:
```
BAT: RES CON1
CON1: LED 
```

2. **Component Values and Labels**

Components can have values, labels, or other specifications in parentheses:
```
BAT(9V): RES(1K) CON1
CON1: LED(Red) GND
CAP(100uF): RES(470) DST1
IC555(Timer): RES(10K) CON2
```

**Value Format Examples:**
- Voltage: `BAT(9V)`, `BAT(3.3V)`
- Resistance: `RES(1K)`, `RES(470)`, `RES(2.2M)`
- Capacitance: `CAP(100uF)`, `CAP(10nF)`, `CAP(1pF)`
- Inductance: `LDR(10mH)`, `LDR(1uH)`
- Current: `FUS(500mA)`, `FUS(2A)`
- Colors: `LED(Red)`, `LED(Blue)`, `LED(Green)`
- IC Labels: `IC555(Timer)`, `IC741(OpAmp)`, `MCU(Arduino)`
- Custom Labels: `BTN(Reset)`, `MOT(Fan)`, `ANT(WiFi)`

3. **Components Exist Between Nodes**

- Components do not create new nodes.
- Components are placed on the path from one node to another.

4. **Distribution Points (`DST`)**

- Split a line into multiple branches:
```
DST1: RES(1K) CON1
DST1: CAP(100uF) CON2
```

5. **Connection Points (`CON`)**

- Merge multiple branches:
```
DST1: RES(1K) CON1
DST1: CAP(100uF) CON1
CON1: LED(Red)
```

6. **Flow Reading Order**

- Default: **top-to-bottom**
- Sequence: distribution → branches → connection → next node

7. **Chained Components**

- Multiple components can exist on a single line:
```
BAT(9V): RES(470) DIO LED(Red) CON1
```

---

## Example: Basic LED Circuit with Values

```
BAT(9V): RES(470) LED(Red) GND
```

**Explanation:** 9V battery connects through a 470Ω current-limiting resistor to a red LED, then to ground.

---

## Example: Complex Circuit with Values

A circuit with a battery, resistor network, capacitors, diodes, LED, lamp, and logic gates:

```
BAT(9V): RES(1K) DST1

DST1: CAP(100uF) CON1

DST1: DIO(1N4148) CON2

CON1: LED(Red) CON3

CON2: RES(2.2K) CON3

CON3: LMP(12V) GND

BAT(9V): CAPP(470uF) CON4

CON4: AND(74HC08) CON5

CON5: OR(74HC32) CON3

DST2: RES(10K) CON5

DST2: CAP(10nF) CON4
```

**Explanation:**

- `BAT(9V): RES(1K) DST1` → 9V battery feeds a distribution point through 1kΩ resistor
- Branches from `DST1`:
  - `CAP(100uF) CON1` → 100µF capacitor to connection point 1
  - `DIO(1N4148) CON2` → 1N4148 diode to connection point 2
- Connection points merge:
  - `CON1: LED(Red) CON3` → Red LED continues to lamp
  - `CON2: RES(2.2K) CON3` → 2.2kΩ resistor continues to same lamp node
- `CON3: LMP(12V) GND` → 12V lamp connected to ground
- Parallel logic gate branch:
  - `BAT(9V): CAPP(470uF) CON4` → 9V battery through 470µF polarized capacitor
  - `CON4: AND(74HC08) CON5` → 74HC08 AND gate output flows to OR gate
  - `CON5: OR(74HC32) CON3` → 74HC32 OR gate output merges at lamp
- `DST2` → additional 10kΩ resistor and 10nF capacitor feeding the logic path

---

## Unicode Symbol Syntax

The markup system uses **letter codes as the primary syntax**. Unicode symbols are provided as an alternative rendering that can be generated from the letter code base.

### Conversion from Letter Codes to Unicode

Any circuit written in letter codes can be automatically converted to Unicode symbols using the dictionary mapping:

**Primary Letter Code Syntax:**
```
BAT(9V): RES(1K) DST1
DST1: CAP(100uF) CON1
DST1: DIO(1N4148) CON2
CON1: LED(Red) CON3
CON2: RES(2.2K) CON3
CON3: LMP(12V) GND
```

**Generated Unicode Symbol Syntax:**
```
⟞±⊢(9V): ⟤(1K) ⌥1
⌥1: ⊣⊢(100uF) ⏧1
⌥1: ⏄(1N4148) ⏧2
⏧1: ⏄⭎⭎(Red) ⏧3
⏧2: ⟤(2.2K) ⏧3
⏧3: ⛒(12V) ⏚
```

### Usage Guidelines

- **Write circuits using letter codes** (BAT, RES, LED, etc.)
- **Unicode symbols are for display/rendering** when Unicode support is available
- **No mixed syntax** - use either all letter codes or all Unicode symbols for a circuit
- **Conversion tools** can transform letter code markup to Unicode symbols automatically

### When to Use Each

**Letter Codes (Primary):**
- Writing and editing circuits
- Plain text environments
- Maximum compatibility
- Documentation and sharing
- Version control systems

**Unicode Symbols (Generated):**
- Visual presentation
- Compact display
- Modern environments with Unicode support
- Final output rendering

---

## Value Units and Conventions

**Standard Units:**
- **Voltage:** V, mV (9V, 3.3V, 500mV)
- **Current:** A, mA, µA (2A, 500mA, 10µA)
- **Resistance:** Ω, K, M (470, 1K, 2.2M)
- **Capacitance:** F, mF, µF, nF, pF (1F, 100µF, 10nF, 1pF)
- **Inductance:** H, mH, µH (1H, 10mH, 100µH)
- **Frequency:** Hz, kHz, MHz, GHz (60Hz, 1kHz, 100MHz)

**Component Labels:**
- Use descriptive names for ICs: `IC555(Timer)`, `IC741(OpAmp)`
- Include part numbers when relevant: `DIO(1N4148)`, `AND(74HC08)`
- Add functional descriptions: `BTN(Reset)`, `LED(Status)`, `MOT(Cooling)`

---

## Comments and Documentation

Add comments using `#` for additional circuit documentation:

```
# Power supply section
BAT(9V): RES(1K) DST1    # Current limiting

# Signal processing
DST1: CAP(100uF) CON1    # Low-pass filter
DST1: DIO(1N4148) CON2   # Rectification

# Output stage  
CON1: LED(Status) GND    # Status indicator
CON2: LMP(Load) GND      # Main load
```
