# HNL Format Specification

Human-readable NetList Format

Version: Draft v0.1

---

## 1. Introduction

This document defines the HNL format.

HNL, Human-readable NetList, is a simple pin-oriented netlist format designed to be easy for humans to read and write, and easy for AI and software tools to interpret.

HNL describes electronic circuit connections using plain text.

HNL is intended for:

- Circuit design notes
- AI-assisted circuit interpretation
- Pre-schematic circuit descriptions
- Breadboard wiring checks
- Perfboard or PCB planning
- CAD conversion preprocessing
- Simple human-readable netlist documentation

HNL is not a SPICE-compatible format.  
HNL is not tied to any specific EDA or CAD software.

---

## 2. Design Goals

The design goals of HNL are:

1. Human readability
2. Easy hand writing
3. Explicit pin-to-net mapping
4. AI-friendly structure
5. Simple parsing
6. Clear polarity notation
7. CAD-independent representation
8. Minimal syntax

The core concept is:

    Component.Pin = Net

Example:

    R1.1 = VCC
    R1.2 = OUT

---

## 3. File Format

An HNL file is a plain text file.

The recommended text encoding is UTF-8.

The recommended file extension is:

    .hnl

Example:

    led-example.hnl

---

## 4. Basic Syntax

HNL has two main line types:

    ComponentName = ComponentValue
    ComponentName.PinName = NetName

The first form defines a component.

The second form defines a connection between a component pin and a net.

Example:

    R1 = 10k
    R1.1 = VCC
    R1.2 = OUT

This means:

- R1 is a 10k resistor
- Pin 1 of R1 is connected to VCC
- Pin 2 of R1 is connected to OUT

---

## 5. Component Definition

A component definition associates a component name with a value or description.

Syntax:

    ComponentName = ComponentValue

Examples:

    R1 = 10k
    C1 = 100nF
    IC1 = NE555
    Q1 = 2N7000
    VR1 = 100k Audio Taper
    SW1 = Momentary Footswitch

The value may include spaces.

Example:

    SW_BYPASS = 3PDT Footswitch

---

## 6. Pin Connection Definition

A pin connection definition associates a component pin with a net.

Syntax:

    ComponentName.PinName = NetName

Examples:

    R1.1 = VCC
    R1.2 = OUT

    Q1.G = N_GATE
    Q1.D = N_AUDIO
    Q1.S = VBIAS

The part before the dot is the component name.  
The part after the dot is the pin name.

---

## 7. Components

### 7.1 Recommended Component Prefixes

The following prefixes are recommended.

| Prefix | Meaning |
|---|---|
| R | Resistor |
| C | Capacitor |
| L | Inductor |
| D | Diode |
| LED | LED |
| Q | Transistor / FET |
| IC | Integrated Circuit |
| U | IC / Module |
| VR | Variable Resistor / Potentiometer |
| SW | Switch |
| J | Jack / Connector |
| BAT | Battery |
| TP | Test Point |
| X | Crystal / Resonator |
| SPK | Speaker |
| MIC | Microphone |

Examples:

    R_GAIN = 1M
    C_IN = 100nF
    IC1 = TL072
    JOUT = Output Jack

---

## 8. Pin Names

Pin names may be numeric or symbolic.

Numeric pin examples:

    R1.1 = IN
    R1.2 = OUT

    IC1.1 = GND
    IC1.8 = VCC

Symbolic pin examples:

    Q1.G = N_GATE
    Q1.D = OUT
    Q1.S = GND

    JIN.TIP = IN
    JIN.SLEEVE = GND

---

## 9. Nets

A net represents an electrical connection.

All pins assigned to the same net name are considered connected.

Example:

    R1.1 = VCC
    C1.1 = VCC
    IC1.8 = VCC

In this example, R1.1, C1.1, and IC1.8 are connected to the same net.

---

## 10. Recommended Net Names

The following net names are recommended.

| Net Name | Meaning |
|---|---|
| VCC | Positive power supply |
| VDD | Logic positive power supply |
| V+ | Positive power supply |
| VEE | Negative power supply |
| VSS | Negative supply or ground |
| GND | Ground |
| AGND | Analog ground |
| DGND | Digital ground |
| IN | Input |
| OUT | Output |
| VBIAS | Bias voltage |
| N_xxx | Internal node |
| NC | Not connected |

Internal nodes should preferably use the prefix:

    N_

Examples:

    N_AUDIO
    N_GATE
    N_TIMING
    N_DISCH

---

## 11. Not Connected Pins

A not connected pin should be assigned to:

    NC

Example:

    IC1.5 = NC

If the pin is connected to an actual component, it should not be marked as NC.

Example:

    IC1.5 = N_CTRL
    C_CTRL.1 = N_CTRL
    C_CTRL.2 = GND

---

## 12. Comments

A semicolon starts a comment.

Everything after the semicolon on the same line is ignored by parsers.

Example:

    R1 = 10k ; Input resistor
    R1.1 = IN
    R1.2 = N_BIAS

A line beginning with a semicolon is a comment line.

Example:

    ; Power supply section
    C1 = 100nF
    C1.1 = VCC
    C1.2 = GND

---

## 13. Empty Lines

Empty lines are ignored.

They may be used to separate component blocks.

Example:

    R1 = 10k
    R1.1 = VCC
    R1.2 = OUT

    C1 = 100nF
    C1.1 = OUT
    C1.2 = GND

---

## 14. Metadata

An HNL file may include metadata lines.

Metadata lines begin with:

    @

Recommended metadata keys:

| Key | Meaning |
|---|---|
| @format | Format name |
| @version | HNL specification version |
| @title | Circuit title |
| @author | Author |
| @date | Date |
| @description | Short description |

Example:

    @format = HNL
    @version = 0.1
    @title = 555 Tremolo
    @author = Your Name
    @date = 2026-01-01

Metadata lines should usually appear at the beginning of the file.

---

## 15. Polarized Capacitors

Polarized capacitors should use the pin names:

    +
    -

Example:

    C1 = 10uF Electrolytic Capacitor
    C1.+ = VCC
    C1.- = GND

For electrolytic capacitors, the component definition should include a description such as:

    Electrolytic Capacitor

or:

    Electrolytic

Example:

    C_OUT = 1uF Electrolytic Capacitor
    C_OUT.+ = N_AUDIO
    C_OUT.- = OUT

---

## 16. Non-Polarized Capacitors

Non-polarized capacitors should use numeric pins.

Example:

    C1 = 100nF Ceramic Capacitor
    C1.1 = VCC
    C1.2 = GND

Example:

    C_IN = 100nF Film Capacitor
    C_IN.1 = IN
    C_IN.2 = N_AUDIO

---

## 17. Diodes and LEDs

Diodes should use the following pin names.

| Pin | Meaning |
|---|---|
| A | Anode |
| K | Cathode |

Example:

    D1 = 1N4148
    D1.A = IN
    D1.K = OUT

LEDs should use the same pin names.

Example:

    LED1 = Red LED
    LED1.A = VCC
    LED1.K = N_LED

---

## 18. BJTs

Bipolar junction transistors should use the following pin names.

| Pin | Meaning |
|---|---|
| B | Base |
| C | Collector |
| E | Emitter |

Example:

    Q1 = 2SC1815
    Q1.B = N_BASE
    Q1.C = OUT
    Q1.E = GND

---

## 19. FETs

MOSFETs and JFETs should use the following pin names.

| Pin | Meaning |
|---|---|
| G | Gate |
| D | Drain |
| S | Source |

Example:

    Q1 = 2N7000
    Q1.G = N_GATE
    Q1.D = N_AUDIO
    Q1.S = VBIAS

---

## 20. Potentiometers

Potentiometers should generally use numeric pins.

Recommended convention:

| Pin | Meaning |
|---|---|
| 1 | Left terminal |
| 2 | Wiper |
| 3 | Right terminal |

Example:

    VR1 = 100k Audio Taper
    VR1.1 = IN
    VR1.2 = OUT
    VR1.3 = GND

Physical pin order may differ depending on the actual part.  
Comments may be used when necessary.

Example:

    VR_VOL = 100k Audio Taper
    VR_VOL.1 = N_VOLIN ; CCW terminal
    VR_VOL.2 = OUT     ; Wiper
    VR_VOL.3 = GND     ; CW terminal

---

## 21. Switches

Switches may use numeric pin names or functional pin names.

Example:

    SW1 = Momentary Switch
    SW1.1 = VCC
    SW1.2 = N_RESET

For complex switches, use the physical pin numbering whenever possible.

Example:

    SW_BYPASS = 3PDT Footswitch
    SW_BYPASS.1 = IN
    SW_BYPASS.2 = OUT
    SW_BYPASS.3 = GND

---

## 22. Jacks and Connectors

### 22.1 Mono Jack

Example:

    JIN = Input Jack
    JIN.TIP = IN
    JIN.SLEEVE = GND

### 22.2 Stereo Jack

Example:

    JIN = Stereo Input Jack
    JIN.TIP = IN_L
    JIN.RING = IN_R
    JIN.SLEEVE = GND

### 22.3 DC Jack

Example:

    JDC = DC Jack
    JDC.CENTER = VCC
    JDC.SLEEVE = GND

Alternative notation:

    JDC.+ = VCC
    JDC.- = GND

---

## 23. Units

### 23.1 Resistors

Recommended resistor value notation:

    100R
    1k
    4.7k
    10k
    100k
    1M

### 23.2 Capacitors

Recommended capacitor value notation:

    10pF
    100nF
    1uF
    2.2uF
    47uF

The notation uF is recommended for easier text input.  
The notation µF may also be accepted.

### 23.3 Voltages

Recommended voltage notation:

    9V
    5V
    3.3V

---

## 24. Series and Parallel Connections

HNL does not use special syntax for series or parallel connections.

Series and parallel relationships are represented by net names.

### 24.1 Parallel Example

    C1 = 100nF
    C1.1 = VCC
    C1.2 = GND

    C2 = 10uF Electrolytic Capacitor
    C2.+ = VCC
    C2.- = GND

C1 and C2 are connected in parallel between VCC and GND.

### 24.2 Series Example

    R1 = 10k
    R1.1 = IN
    R1.2 = N_MID

    R2 = 10k
    R2.1 = N_MID
    R2.2 = OUT

R1 and R2 are connected in series from IN to OUT.

---

## 25. Recommended Description Order

The following order is recommended, but not required.

1. Metadata
2. Power supply
3. Input and output connectors
4. Bias circuits
5. ICs and active components
6. Timing circuits
7. Signal path
8. Controls
9. Output section
10. Supporting components

---

## 26. Error and Warning Rules

A checker or parser may report errors and warnings.

### 26.1 Errors

The following should be treated as errors:

- A pin connection references an undefined component
- The same component is defined more than once with different values
- A pin name is empty
- A net name is empty
- The same component pin is assigned to multiple different nets
- A required pin is missing in a strict validation mode

Example error:

    R1 = 10k
    R1.1 = VCC
    R1.1 = GND

R1.1 is assigned to both VCC and GND.

### 26.2 Warnings

The following may be treated as warnings:

- No GND net exists
- No power net exists
- A net has only one connected pin, except NC
- A resistor has more than two pins
- A polarized capacitor has only + or only - defined
- A capacitor uses + and - but is not described as electrolytic or polarized
- The + pin of an electrolytic capacitor is connected to GND
- An IC appears to have unconnected power pins

Example warning:

    C1 = 10uF Electrolytic Capacitor
    C1.+ = VCC

C1.- is missing.

---

## 27. Minimum Valid Example

    @format = HNL
    @version = 0.1
    @title = Minimal Example

    BAT1 = 9V Battery
    BAT1.+ = VCC
    BAT1.- = GND

    R1 = 1k
    R1.1 = VCC
    R1.2 = N_LED

    LED1 = Red LED
    LED1.A = N_LED
    LED1.K = GND

---

## 28. Complete Example

    @format = HNL
    @version = 0.1
    @title = Audio Coupling Example

    JIN = Input Jack
    JIN.TIP = IN
    JIN.SLEEVE = GND

    C_IN = 100nF Film Capacitor
    C_IN.1 = IN
    C_IN.2 = N_AUDIO

    R_BIAS = 1M
    R_BIAS.1 = N_AUDIO
    R_BIAS.2 = VBIAS

    C_OUT = 1uF Electrolytic Capacitor
    C_OUT.+ = N_AUDIO
    C_OUT.- = OUT

    JOUT = Output Jack
    JOUT.TIP = OUT
    JOUT.SLEEVE = GND

---

## 29. Reserved Characters

The following characters have special meaning in HNL:

| Character | Meaning |
|---|---|
| = | Assignment |
| . | Component and pin separator |
| ; | Comment start |
| @ | Metadata line prefix |

The following pin names are allowed and have special meaning for polarized parts:

    +
    -

---

## 30. Parsing Notes

A basic parser may follow these steps:

1. Read the file line by line
2. Remove comments beginning with ;
3. Trim whitespace
4. Ignore empty lines
5. If the line begins with @, parse it as metadata
6. If the left side contains a dot, parse it as a pin connection
7. Otherwise, parse it as a component definition

Example:

    R1 = 10k

is a component definition.

Example:

    R1.1 = VCC

is a pin connection.

---

## 31. Relationship to Other Formats

HNL is conceptually related to:

- SPICE netlists
- EDA netlists
- KiCad netlists
- INI-style key-value files
- Verilog named port connections
- Pin mapping formats

However, HNL is intended to be simpler and more readable.

HNL is pin-oriented rather than simulation-oriented.

---

## 32. Future Extensions

Future versions may define additional optional fields, such as:

- Footprint
- Manufacturer part number
- Tolerance
- Voltage rating
- Power rating
- Package type
- SPICE model
- KiCad symbol name
- KiCad footprint name
- Subcircuits
- Hierarchical sheets
- Differential pairs
- Buses
- Mechanical constraints
- JSON representation
- CAD export rules

These features are not part of the core v0.1 specification.

---

## 33. Core Rule Summary

The core rules of HNL are:

    ComponentName = ComponentValue
    ComponentName.PinName = NetName
    The same net name represents the same electrical connection.
    Polarized capacitors should use .+ and .- pins.
    Diodes and LEDs should use .A and .K pins.
    Text after ; is a comment.

This small set of rules is sufficient to describe many simple circuits in a human-readable and AI-friendly way.
