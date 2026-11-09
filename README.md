# HNL - Human-readable NetList Format

HNL, Human-readable NetList, is a simple pin-oriented netlist format designed to be easy for humans to read and write, and easy for AI and software tools to interpret.

HNL is intended to describe electronic circuit connections in plain text before drawing a schematic, wiring a breadboard, designing a PCB, or converting the circuit into another CAD format.

Status: Draft v0.1

---

## Purpose

The purpose of HNL is to provide a simple text-based notation for describing electronic circuits in a way that is understandable by both humans and AI.

HNL is designed to:

- Describe components and their values
- Describe which component pins are connected to which nets
- Make the orientation of polarized components clear
- Provide a readable intermediate representation for circuit design
- Help with circuit checking before schematic capture or PCB layout
- Serve as an AI-friendly circuit representation
- Be simple enough to write by hand

HNL is not intended to replace SPICE, KiCad, or other EDA formats.  
Instead, it is intended to be a lightweight intermediate format.

---

## Basic Idea

HNL uses two basic types of lines:

    ComponentName = ComponentValue
    ComponentName.PinName = NetName

Example:

    R1 = 10k
    R1.1 = VCC
    R1.2 = OUT

This means:

- R1 is a 10k resistor
- Pin 1 of R1 is connected to VCC
- Pin 2 of R1 is connected to OUT

If multiple pins are connected to the same net name, they are considered electrically connected.

Example:

    R1.1 = VCC
    C1.1 = VCC
    IC1.8 = VCC

In this example, R1.1, C1.1, and IC1.8 are all connected to the same net, VCC.

---

## Minimal Example

    @format = HNL
    @version = 0.1
    @title = Minimal LED Example

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

## Features

- Human-readable
- Easy to write by hand
- Plain text format
- AI-friendly
- Pin-oriented notation
- Explicit pin-to-net mapping
- Clear polarity notation
- Simple syntax
- Easy to parse
- Suitable as an intermediate representation

---

## Intended Use Cases

HNL can be used for:

- Circuit design notes
- AI-assisted circuit design
- Pre-schematic circuit descriptions
- Breadboard wiring checks
- Perfboard or stripboard planning
- Simple netlist documentation
- Conversion to schematic or PCB CAD formats
- Educational circuit explanations
- Component connection reviews

---

## Not a SPICE Format

HNL is not a SPICE-compatible netlist format.

SPICE usually describes components using positional node order, such as:

    R1 VCC OUT 10k

HNL describes the same idea using explicit pin assignments:

    R1 = 10k
    R1.1 = VCC
    R1.2 = OUT

This makes the connection more verbose, but easier for humans and AI to interpret.

---

## File Extension

The recommended file extension is:

    .hnl

Example:

    example.hnl

---

## Metadata

An HNL file may begin with metadata lines.

Example:

    @format = HNL
    @version = 0.1
    @title = 555 Tremolo
    @author = Your Name
    @date = 2026-01-01

---

## Basic Rules

The minimal rules of HNL are:

    ComponentName = ComponentValue
    ComponentName.PinName = NetName
    The same net name means the same electrical connection.
    Polarized capacitors should use .+ and .- pins.
    Text after ; is a comment.

Example:

    C1 = 10uF Electrolytic Capacitor
    C1.+ = VCC
    C1.- = GND

---

## Comments

A semicolon starts a comment.

Example:

    R1 = 10k ; Input resistor
    R1.1 = IN
    R1.2 = N_BIAS

A line beginning with a semicolon is a full-line comment.

Example:

    ; Power section
    C1 = 100nF
    C1.1 = VCC
    C1.2 = GND

---

## Example: Audio Coupling Circuit

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

## Specification

See:

    SPEC.md

---

## Version

Current draft version:

    0.1

This specification is experimental and may change.

---

## License

Suggested license:

- Specification documents: CC BY 4.0
- Example files and tools: MIT License

You may choose a different license depending on your project goals.

---

## Project Status

HNL is currently a draft specification.

Feedback, examples, parser implementations, and conversion tools are welcome.
