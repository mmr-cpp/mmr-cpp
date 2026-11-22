# Eddy Currents

Gradient-field-induced eddy currents play a major role in MRI image quality.

Eddy currents are unwanted electrical currents induced in conductive materials exposed to changing magnetic fields.

In MRI systems, the gradients generate rapidly changing magnetic fields that induce eddy currents in:

- Magnet structures
- Gradient coils
- RF body coils
- RF shields
- Conductive support structures

These eddy currents create secondary magnetic fields that distort the intended gradient fields.

---

# Why Eddy Currents Matter

Eddy currents affect MRI in two ways:

## 1. Image Quality Degradation

Eddy-current-generated magnetic fields can cause:

- Spatial distortion
- Frequency shifts
- Phase errors
- Geometric inaccuracies
- Image artifacts

---

## 2. Heating

Eddy currents dissipate energy through resistance.

```text
Gradient Energy
       ↓
Eddy Currents
       ↓
I²R Heating
```

### Consequences

#### Magnet

Heat generated in the cold mass increases cryogenic load.

```text
More Eddy Current Heating
          ↓
More Helium Boil-Off
          ↓
Higher Operating Cost
```

#### RF Coil

Heating can raise bore temperature and increase patient thermal load.

---

# 3.11.1 A Circuit View

Eddy currents can be understood using two inductively coupled loops.

---

## Fig. 3-27 : Inductive Coupling

```text
Driving Circuit

      Is
      ↑

 ┌─────────┐
 │ Current │
 │ Source  │
 └────┬────┘
      │

     L0
      │

     Mutual
   Inductance M

      │

 ┌─────────┐
 │   L1    │
 │         │
 │   R1    │
 └─────────┘

      I2
```

---

### Induced Voltage

A changing gradient current produces:

```text
Vs = M · dIs/dt
```

where:

- M = mutual inductance
- Is = gradient current

---

### Eddy Current Response

The induced current follows a first-order exponential response:

```text
τ = L1/R1
```

where:

- τ = time constant
- L1 = inductance
- R1 = resistance

---

## Key Observation

Eddy currents are driven by:

```text
dIs/dt
```

not by current itself.

Therefore:

```text
Fast Gradient Changes
          ↓
Large Eddy Currents
```

---

# MRI Interpretation

In MRI:

```text
Gradient Amplifier
        ↓
Gradient Coil
        ↓
Changing Magnetic Field
        ↓
Eddy Currents
```

The resulting eddy current field is an exponential response to the gradient waveform.

---

# 3.11.2 Eddy Fields

Eddy currents generate their own magnetic fields.

According to Lenz's Law:

```text
Induced Field
Opposes Change
```

---

## Eddy Current Example

A gradient winding is surrounded by an aluminum cylinder.

When current changes:

1. Magnetic flux changes.
2. Eddy currents are induced.
3. Eddy currents generate their own magnetic field.
4. The eddy field opposes the original field change.

---

## Fig. 3-28 : Eddy Current Example

```text
Gradient Field

      ┌────────────┐
      │            │
      │            │
──────┘            └──────


Eddy Field

      \______
             \______
                    \___


Total Field

      ┌─────────────┐
      │             │
      │             │
──────┘             └──────
          \
           \__
              \__
```

---

## Lenz's Law

```text
Increasing Gradient Field
          ↓
Negative Eddy Field

Decreasing Gradient Field
          ↓
Positive Eddy Field
```

The eddy field always opposes the change.

---

# Multiple Eddy Current Paths

Real systems contain many conductive structures.

Each structure contributes:

```text
Amplitude α
Time Constant τ
```

Therefore:

```text
Total Eddy Field

=

Field₁
+
Field₂
+
Field₃
+
...
```

---

# 3.11.3 Characterization

Eddy currents are characterized using:

1. Amplitude (α)
2. Time Constant (τ)

---

## Time Constant (τ)

Describes decay rate.

```text
Field(t)

 ^
 |
 |\
 | \
 |  \
 |   \
 |    \
 +----------------→ Time
```

Every time constant:

```text
Amplitude decreases by

1/e

≈ 37%
```

of the previous value.

---

## Amplitude (α)

Amplitude describes how large the eddy field is relative to the desired gradient field.

Examples:

```text
100% Eddy Field

Completely cancels
the gradient step
initially
```

```text
20% Eddy Field

Offsets only 20%
of the gradient field
```

---

## Fig. 3-29 : Eddy Current Characterization

```text
Gradient Field

        ┌─────────────
       /
      /
     /
────┘

Eddy Response

╲
 ╲
  ╲
   ╲───────────────

Time Constant = τ
```

---

## Practical Measurement

Ideal step functions are impossible because:

```text
Infinite Voltage Required
```

Instead MRI systems characterize eddy currents using gradient ramps.

Curve fitting is then used to estimate:

```text
α
and
τ
```

for each eddy-current path.

---

# 3.11.4 Spatial Dependence

Eddy fields vary with position.

Just as gradient fields vary in space, eddy-current fields also vary in space.

---

## Problem

Compensation at one location:

```text
Perfect at Point A
```

does not guarantee:

```text
Perfect at Point B
```

---

## Solution

Measure eddy fields at multiple locations.

Common measurement locations:

```text
+x
-x

+y
-y

+z
-z
```

---

# Eddy Field Components

Measured eddy fields are decomposed into:

## Zeroth Order

Looks like:

```text
B₀ Field
```

Uniform across space.

---

## First Order

Looks like:

```text
Gx
Gy
Gz
```

gradient fields.

---

## Higher Order

More complicated spatial distortions.

Examples:

```text
Curvature
Asymmetry
Local Distortions
```

---

## Eddy Field Decomposition

```text
Total Eddy Field

      =
      
B₀ Component

      +

Gx Component

      +

Gy Component

      +

Gz Component

      +

Higher Order Components
```

---

# Zeroth and First Order Fields

Two measurement points:

```text
+A

-B
```

allow separation into:

### First Order Component

```text
GEC
```

Looks like a gradient field.

---

### Zeroth Order Component

```text
B0EC
```

Looks like a B₀ shift.

---

## Conceptual Diagram

```text
Field

 ^

 |      /
 |     /
 |    /
 |   /
 |  /
 +----------------→ z

First Order
```

```text
Field

 ^
 |
 |──────────────
 |
 +----------------→ z

Zeroth Order
```

---

# 3.11.5 Eddy Current Compensation (ECC)

MRI systems use Eddy Current Compensation (ECC) to minimize these effects.

ECC operates in real time.

---

## ECC Process

```text
Requested Gradient
         ↓
Characterization Model
         ↓
Digital Filters
         ↓
Compensation Waveform
         ↓
Gradient Amplifier
```

---

## Advantage

Compensation is based on:

- Known system behavior
- Gradient waveform

Therefore:

```text
Automatic
```

and independent of the pulse sequence.

---

# 3.11.5.1 First Order Compensation

First-order eddy currents resemble gradient fields.

They can be compensated using:

```text
Gradient Pre-Emphasis
```

---

## Pre-Emphasis Concept

```text
Original Gradient

      /|
     / |
    /  |
___/   |


Compensation Added

    /\
   /  \
__/    \__


Result

Desired Gradient
```

---

## How It Works

The gradient waveform is modified before reaching the amplifier.

Digital filters add corrections that cancel the predicted eddy-current field.

---

# 3.11.5.2 Zeroth Order Compensation

Gradient coils cannot create a pure B₀ field.

Therefore a different compensation method is required.

---

## Method 1

Dedicated:

```text
B₀ Compensation Coil
```

creates the opposing field.

---

## Method 2

Adjust MR frequency.

Using:

```text
Δω = γΔB₀
```

If a zeroth-order eddy field shifts B₀:

```text
Frequency Shift
```

can compensate for it.

---

# 3.11.5.3 Higher Order Compensation

Higher-order eddy currents remain after:

- Zeroth-order correction
- First-order correction

---

## Challenge

Higher-order spatial patterns are complex.

Examples:

```text
Curvature

Nonlinear Distortion

Localized Errors
```

---

## Current Approach

Most systems:

```text
Characterize
+
Minimize
+
Correct
```

rather than attempting exact compensation.

Application-specific corrections may be used for imaging sequences highly sensitive to eddy currents.

---

# Summary Diagram

```text
Gradient Switching
         ↓
    dB/dt
         ↓
   Eddy Currents
         ↓
 Secondary Fields
         ↓
 Image Artifacts
         ↓
     ECC
         ↓
 Improved Image Quality
```

---

# Key Takeaways

- Eddy currents are induced whenever changing gradient fields interact with conductive materials.
- Eddy currents create unwanted secondary magnetic fields.
- These fields cause image distortion, frequency errors, phase errors, and artifacts.
- Eddy currents also generate heat through resistive losses.
- Eddy current response is characterized by:
  - Amplitude (α)
  - Time Constant (τ)
- Eddy currents are proportional to the rate of change of gradient current (dI/dt).
- Real MRI systems contain multiple eddy-current paths with different amplitudes and decay constants.
- Eddy fields are spatially decomposed into:
  - Zeroth-order (B₀-like)
  - First-order (Gx, Gy, Gz-like)
  - Higher-order components
- First-order eddy currents are compensated using gradient pre-emphasis.
- Zeroth-order eddy currents are compensated using B₀ coils or frequency adjustments.
- Higher-order eddy currents are more difficult to correct and often require specialized solutions.
- Eddy Current Compensation (ECC) is essential for maintaining MRI geometric accuracy and image quality.
