# DC Power Supply Design & Implementation

An academic electronics project involving the design, simulation, analysis, and hardware implementation of a DC power supply.

## Project Overview

The main objective of this project was to design and analyze a DC power supply that converts an AC input into a stable DC output.

The circuit was first designed and tested using **NI Multisim**, then implemented as a physical hardware prototype.

The power supply consists of four main stages:

1. Transformer
2. Bridge Rectifier
3. Filter Capacitor
4. Zener Diode + Transistor Voltage Regulator

---

## Circuit Stages

### 1. Transformer

The AC source provides **150V AC**.

A transformer with a **10:1 turns ratio** steps the voltage down:

**150V AC → 15V AC**

The output is still AC and follows a sinusoidal waveform.

### 2. Bridge Rectifier

The bridge rectifier consists of **four diodes**.

It converts the AC waveform into **pulsating DC** by flipping the negative half-cycles to the positive side.

### 3. Filter Capacitor

A large capacitor (**C2 = 4.7 mF**) is used to smooth the pulsating DC.

The capacitor charges when the rectified voltage is high and releases stored energy when the voltage decreases, reducing voltage ripple and producing a smoother DC voltage.

### 4. Voltage Regulator

The voltage regulation stage uses a **Zener diode and transistor**.

The Zener diode provides a reference voltage, while the transistor supplies the required load current.

The output is taken from the transistor emitter.

Because of the transistor's base-emitter voltage drop:

**Vout = VZener − VBE**

**Vout = 12V − 0.65V ≈ 11.35V**

---

## Component Selection

### C2 = 4.7 mF

C2 acts as the main charge reservoir after the bridge rectifier.

The capacitor must be large enough to reduce the voltage drop between rectifier pulses, while avoiding an unnecessarily long startup charging time.

The charging time constant is:

**τ = RC**

### R1 = 470 Ω

R1 limits the current flowing through the Zener diode and provides the required current to the transistor base.

The Zener diode has a maximum current of approximately **83 mA**.

With the selected resistor, the Zener current is approximately **16.6 mA**, keeping the Zener operating safely within its limits.

### C1 = 470 µF

C1 is placed across the output/load.

It helps reduce output voltage fluctuations, suppress voltage spikes, and improve transient response during sudden load changes.

The resulting time constant is approximately:

**τ = 0.047 s**

---

## Load Regulation Test

The circuit was tested using different load resistances.

Despite changing the load resistance from **20 Ω to 5000 Ω**, the output voltage remained relatively stable.

The measured output voltage variation was:

**ΔV = 11.435V − 11.28V = 0.155V**

This corresponds to approximately **1.37% variation**.

This demonstrates good voltage regulation across a wide range of load conditions.

---

## Zener + Transistor vs. Zener-Only Regulation

The project also investigated the difference between using the Zener diode alone and using the Zener diode with a transistor.

### Zener-Only Circuit

When the load resistance is high, the Zener can regulate the output effectively.

For example, with:

**Rload = 1000 Ω**

The output voltage was approximately:

**Vout = 12.042V**

However, the circuit fails under heavy loads.

For:

**Rload = 10 Ω**

The load requires approximately:

**I = V/R = 12/10 = 1.2A**

However, with **R1 = 65 Ω**, the maximum current that can be supplied is approximately:

**Imax = (19.8 − 12) / 65 ≈ 120 mA**

Therefore, the Zener cannot maintain its breakdown voltage and the output collapses to approximately:

**Vout = 2.547V**

---

## Zener Safety Analysis

The **1N4742A Zener diode** has a maximum power rating of approximately **1 W**.

Therefore, the maximum safe current is approximately:

**Imax = Pmax / Vz ≈ 1W / 12V ≈ 83 mA**

With a Zener-only circuit and **R1 = 65 Ω**, under very light load conditions most of the current flows through the Zener.

The current can reach approximately **120 mA**.

The resulting power is:

**P = V × I ≈ 12 × 0.12 = 1.44 W**

This exceeds the Zener's **1 W maximum power rating**.

Therefore, the Zener-only design could potentially damage the Zener diode under light-load conditions.

---

## Why the Transistor Is Important

The transistor solves the main limitations of the Zener-only design.

With the transistor:

- R1 provides only a relatively small bias current.
- The Zener provides the reference voltage.
- The transistor supplies the majority of the load current.
- The Zener is protected from excessive load current.
- The output remains more stable when the load changes.

The transistor therefore acts as a **current amplifier**, allowing a small base current to control a much larger collector/emitter current.

This makes the transistor-based regulator more suitable for handling heavy loads than the Zener-only configuration.

---

## Simulation

The complete circuit was designed and tested using **NI Multisim**.

The simulation was used to:

- Verify circuit operation.
- Analyze voltage levels at different stages.
- Test different load resistances.
- Compare the Zener-only and transistor-based circuits.
- Analyze voltage regulation.
- Select suitable component values.

---

## Hardware Implementation

After completing the simulation, the circuit was physically constructed and tested.

The hardware implementation allowed the simulation results to be compared with real circuit behavior.

Photos of the hardware implementation and simulation results are included in this repository.

---
