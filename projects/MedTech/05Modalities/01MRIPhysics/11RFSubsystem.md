# The RF Subsystem

The RF (Radio Frequency) subsystem performs two essential MRI functions:

1. Creates the **B₁ transmit field** used to excite nuclear spins.
2. Detects the resulting MR signal emitted by those spins.

The RF subsystem is divided into three major parts:

- Transmit Chain (TXC)
- Receive Chain (RXC)
- RF Coils

---

# RF Subsystem Overview

```text
                    RF SUBSYSTEM

          ┌─────────────────────────┐
          │      TRANSMIT CHAIN     │
          │          (TXC)          │
          └───────────┬─────────────┘
                      │
                      ▼
                  RF Coil
                      │
                Nuclear Spins
                      │
                      ▼
          ┌─────────────────────────┐
          │      RECEIVE CHAIN      │
          │          (RXC)          │
          └─────────────────────────┘
```

---

# RF Transmit Chain (TXC)

The transmit chain creates the RF pulses used to excite the MR resonance.

Main components:

1. RF waveform generation
2. RF amplifier
3. Power monitor
4. RF coils
5. T/R switches
6. Switch and bias controls

---

# Transmit / Receive Modes

MRI alternates between:

```text
TRANSMIT
```

and

```text
RECEIVE
```

modes.

During TRANSMIT:

- RF amplifier generates high-power RF pulses.
- B₁ field excites spins.

During RECEIVE:

- RF amplifier is disabled.
- MR signal is detected.

---

## Power Comparison

Transmit power:

```text
1 – 40 kW
```

Receive signal:

```text
nW range
```

Difference:

```text
≈ 10¹²
```

This huge dynamic range strongly influences RF system design.

---

# Fig. 3-21 : RF Transmit Chain

```text
                     TRANSMIT CHAIN (TXC)

 ┌─────────────────┐
 │ Status &        │
 │ Control         │
 └──────┬──────────┘
        │
        ▼
 ┌─────────────────┐
 │ RF Waveform     │
 │ Generation      │
 └──────┬──────────┘
        │ RF Pulse
        ▼
 ┌─────────────────┐
 │ RF Amplifier    │
 └──────┬──────────┘
        │
        ▼
 ┌─────────────────┐
 │ Directional     │
 │ Couplers        │
 └──────┬──────────┘
        │
        ├────────► Power Monitor
        │
        ▼
 ┌─────────────────┐
 │ T/R Switches    │
 └──────┬──────────┘
        │
        ├────────► Body Coil
        │
        └────────► Local Coil

                MRI Patient

                     ↓

             MR Signal Generated
```

---

# Status & Control

Controls and synchronizes:

- RF pulse timing
- Amplifier enable/disable
- Coil biasing
- T/R switching
- Power monitoring

---

# RF Waveform Generation

Functions:

- Creates RF pulse waveforms
- Maintains scanner RF frequency
- Controls:
  - Amplitude
  - Phase
  - Frequency

Acts as the RF synthesizer.

The generated frequency must match

```text
Larmor Frequency
```

---

# RF Amplifier

Amplifies low-power RF pulses to high-power transmit signals.

Typical power levels:

### 1.5 T

Body Coil:

```text
10 – 20 kW
```

Local Coil:

```text
1 – 2 kW
```

### 3 T

Body Coil:

```text
30 – 40 kW
```

Local Coil:

```text
3 – 5 kW
```

---

## Blanking and Unblanking

During TRANSMIT:

```text
Amplifier = Unblanked
```

During RECEIVE:

```text
Amplifier = Blanked
```

Reasons:

- Reduces heat production
- Reduces receive noise

---

# Power Monitor

Power monitoring prevents excessive patient heating.

Directional couplers measure:

```text
Forward Power
```

and

```text
Reflected Power
```

Difference:

```text
Patient Absorbed Power
+
Coil Losses
```

---

# T/R Switches

Transmit/Receive switches route signals appropriately.

---

## During Transmit

```text
RF Amplifier
      │
      ▼
    Coil

Preamplifier
Protected
```

Functions:

- Deliver transmit power
- Protect receive electronics

---

## During Receive

```text
MR Signal
    │
    ▼
Preamplifier

Amplifier Isolated
```

Functions:

- Block amplifier noise
- Allow low-loss receive path

---

## Typical Performance

Transmit loss:

```text
0.3 – 0.5 dB
```

Isolation:

```text
60 – 75 dB
```

---

# Switch & Bias Controls

Switch states follow:

```text
TRANSMIT ↔ RECEIVE
```

Typical switching times:

```text
10 – 50 μs
```

May require:

```text
100 – 500 V
```

bias voltages.

---

# RF Receive Chain (RXC)

The receive chain converts the detected MR signal into digital data.

Major functions:

1. Coil Interface
2. Preamplification
3. Digitization
4. Demodulation
5. Filtering
6. Decimation

Output:

```text
Raw Data
```

used by image reconstruction.

---

# Fig. 3-22 : Receive Chain

```text
Receive Coil
     │
     ▼
┌────────────┐
│ Coil Conn. │
└─────┬──────┘
      │
      ▼
┌────────────┐
│ Preamplifier│
└─────┬──────┘
      │
      ▼
┌────────────┐
│ Digitizer  │
└─────┬──────┘
      │
      ▼
┌────────────────┐
│ Complex        │
│ Demodulation   │
└─────┬─────┬────┘
      │     │
      │I    │Q
      ▼     ▼

┌─────────┐ ┌─────────┐
│Filtering│ │Filtering│
└────┬────┘ └────┬────┘
     │           │
     ▼           ▼

┌─────────┐ ┌─────────┐
│Decimate │ │Decimate │
└────┬────┘ └────┬────┘
     │           │
     └─────┬─────┘
           ▼

      To Recon
```

---

# Coil Connection

Provides interface between:

```text
Receive Coil
```

and

```text
Preamplifier
```

Can support:

- Single-channel coils
- Multi-channel arrays

---

# Preamplifier

The MR signal is extremely weak.

Typical signal level:

```text
nW
```

Functions:

- Low-noise amplification
- SNR preservation
- Signal conditioning

Preamps are usually mounted close to the coil.

---

# Digitization

Converts analog RF signals to digital data.

Benefits:

- Better noise immunity
- Easier data transport
- Supports multi-channel imaging

Modern scanners often digitize very close to the coil.

---

# Complex Demodulation

Converts:

```text
RF Signal
```

to

```text
Baseband Signal
```

Output:

```text
I (In-phase)
Q (Quadrature)
```

components.

---

## IQ Diagram

```text
          Q
          ↑
          │
          │
          │
----------O----------→ I
          │
          │

Magnitude + Phase
```

---

# Filtering

Removes unwanted noise.

Benefits:

- Improved SNR
- Reduced bandwidth

---

# Decimation

Reduces sample rate.

Purpose:

- Lower storage requirements
- Lower processing requirements

while preserving signal information.

---

# RF Coils

RF coils act as magnetic antennas.

They:

- Generate B₁ during transmit
- Detect MR signal during receive

---

# Body Coil

Present in almost all MRI systems.

Characteristics:

- Built into scanner
- Covers full imaging volume
- Primarily used for transmit

---

## Body Coil Concept

```text
      __________________

     /                  \
    /                    \
   |      Patient         |
   |                      |
    \                    /
     \__________________/

        Body Coil
```

---

# Local Coils

Designed for specific anatomies.

Examples:

- Head
- Knee
- Wrist
- Spine
- Shoulder

Advantages:

- Better signal reception
- Higher SNR
- Lower transmit power

---

# Transmit vs Receive Coils

## Transmit Coils

Purpose:

```text
Create B₁ Field
```

Characteristics:

- High power handling
- Uniform field coverage

---

## Receive Coils

Purpose:

```text
Detect MR Signal
```

Characteristics:

- High sensitivity
- Small coverage area
- Often receive-only

---

## T/R Coils

Perform both functions:

```text
Transmit
+
Receive
```

Examples:

- Body Coil
- Head Coil

---

# Volume Coils

Enclose anatomy.

Provide:

```text
Uniform RF Coverage
```

Examples:

- Body coil
- Head coil
- Knee coil

---

# Local Coils

Do not enclose anatomy.

Provide:

```text
High Sensitivity
```

Examples:

- Shoulder coil
- Surface coil
- Spine array

---

# Quadrature Operation

Most MRI systems use:

```text
Quadrature Transmit
```

where B₁ rotates with nuclear precession.

Benefits:

- Better efficiency
- Reduced RF power deposition

---

# Quadrature Receive

Uses:

```text
I
+
Q
```

signals.

Benefit:

```text
√2 SNR Improvement
```

for volume coils.

---

# Coil Decoupling

Most imaging uses:

```text
Transmit = Body Coil

Receive = Local Coil
```

The two coils must be decoupled.

---

## During Transmit

Local coil is disabled.

Purpose:

- Prevent induced currents
- Prevent heating
- Prevent transmit distortion

---

## During Receive

Body coil is disabled.

Purpose:

- Prevent receive noise
- Improve SNR

---

# Key Takeaways

- The RF subsystem creates the B₁ field and detects MR signals.
- The RF subsystem consists of:
  - Transmit Chain (TXC)
  - Receive Chain (RXC)
  - RF Coils
- The transmit chain generates and amplifies RF pulses.
- The receive chain converts weak MR signals into digital data.
- MRI alternates between TRANSMIT and RECEIVE modes.
- RF amplifiers can deliver 1–40 kW of transmit power.
- MR receive signals are typically in the nanowatt range.
- T/R switches protect receive electronics during transmit.
- Receive chains use preamplification, digitization, IQ demodulation, filtering, and decimation.
- RF coils act as magnetic antennas.
- Body coils provide large-area transmit coverage.
- Local coils provide higher receive sensitivity.
- Volume coils provide uniform RF coverage.
- Quadrature operation improves transmit efficiency and receive SNR.
- Proper coil decoupling is essential for safe and efficient MRI operation.
- Together, B₀, B₁, gradients, and RF coils enable MRI signal generation and image formation.
