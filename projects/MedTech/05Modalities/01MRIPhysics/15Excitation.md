# Excitation

To create a detectable MR signal, the net magnetization vector **M** must be tipped away from the z-axis.

This process is called:

```text
Excitation
```

Excitation uses a radio-frequency magnetic field called **B₁** to rotate the magnetization vector away from the longitudinal direction.

The RF field is:

- Applied perpendicular to B₀
- Produced by an RF transmit coil
- Typically milliseconds in duration
- Modulated in amplitude and phase

---

# Why Excitation Is Needed

Before excitation:

```text
      +z
       ↑
       |
       |
       M

No detectable MR signal
```

The magnetization is aligned with B₀.

Because it is not rotating in the transverse plane, no signal is induced in the receive coil.

---

## After Excitation

```text
      +z
       ↑

       |
       |

O ─────► M

Transverse Magnetization
```

The transverse component precesses and induces a measurable MR signal.

---

# The B₁ Field

The excitation field:

```text
B₁
```

is:

- Perpendicular to B₀
- Applied in the transverse plane
- Rotates at the Larmor frequency

Most MRI transmit coils generate a:

```text
Quadrature RF Field
```

which follows the natural precession of the spins.

---

## B₀ and B₁ Together

```text
                 +z
                  ↑
                  │
                  │  B₀
                  │
                  │
                  O────────► B₁
```

- B₀ produces spin alignment and precession.
- B₁ produces rotation (excitation) of M.

---

# Bloch Equation During Excitation

The complete Bloch equation is:

```text
dM/dt = γ(M × B)
        - (Mx/T₂) î
        - (My/T₂) ĵ
        - ((Mz - M₀)/T₁) k̂
```

---

## Simplification During RF Excitation

RF pulses are very short:

```text
Milliseconds
```

while relaxation times are typically:

```text
Hundreds of milliseconds
to
Several seconds
```

Therefore:

```text
T₁ and T₂ effects
can be ignored
during excitation.
```

The Bloch equation becomes:

```text
dM/dt = γ(M × B)
```

---

## Magnetic Field During Excitation

The total magnetic field is:

```text
B = B₀ + B₁
```

where:

- B₀ = static magnetic field
- B₁ = rotating RF field

Thus:

```text
dM/dt = γ[M × (B₀ + B₁)]
```

---

# Rotating Frame Transformation

Analyzing magnetization in the laboratory frame is difficult because both:

- M
- B₁

are rotating at the Larmor frequency.

---

## Laboratory Frame

```text
Observer

      M ↺

      B₁ ↺

Complicated Motion
```

---

## Rotating Frame

A rotating coordinate system is introduced that rotates at:

```text
ω₀ = γB₀
```

the Larmor frequency.

In this frame:

```text
B₀ disappears
```

and

```text
B₁ appears stationary
```

---

### Rotating Frame Concept

```text
Laboratory Frame

      M ↺
      B₁ ↺


Rotating Frame

      M
      ↓
      B₁

Simple Rotation
```

---

## Why Use the Rotating Frame?

The rotating frame makes excitation much easier to understand.

In the rotating frame:

```text
B₁ directly rotates M.
```

The problem becomes pure geometric rotation.

---

# Flip Angle

The RF pulse tips magnetization by a specific angle.

This angle is called the:

```text
Flip Angle (α)
```

---

## Common Flip Angles

### 90° Pulse

```text
Before

      ↑ M

After

──────► M
```

Rotates magnetization completely into the transverse plane.

---

### 180° Pulse

```text
Before

      ↑ M

After

      ↓ M
```

Inverts magnetization.

---

# Flip Angle Equation

The flip angle depends on:

- RF amplitude
- RF pulse duration

```text
α = γ ∫ B₁(t) dt
```

For a rectangular RF pulse:

```text
α = γB₁τ
```

where:

- α = flip angle
- γ = gyromagnetic ratio
- B₁ = RF amplitude
- τ = pulse width

---

# Selective Excitation

Often only one slice of tissue should be excited.

This is called:

```text
Selective Excitation
```

---

## How Selective Excitation Works

A gradient is applied during the RF pulse.

Typically:

```text
Gz
```

is applied.

---

### Frequency Variation

The gradient causes resonance frequency to vary with position:

```text
f(z) = γ(B₀ + Gz·z)
```

Only spins matching the RF frequency are excited.

---

## Slice Selection

```text
RF Pulse

      █████

          +
          
Slice Gradient

      ┌─────┐
──────┘     └──────

                ↓

Only One Slice
Excited
```

---

# RF Bandwidth and Slice Thickness

The excited slice thickness depends on:

```text
RF Bandwidth
```

and

```text
Gradient Strength
```

---

### Thick Slice

```text
Wide Frequency Range

████████████████
```

---

### Thin Slice

```text
Narrow Frequency Range

████
```

---

## Slice Thickness Relationship

```text
Slice Thickness

      ∝

RF Bandwidth

      /

Gradient Strength
```

---

# Small Tip-Angle Excitation

When flip angles are small:

```text
α < 20°
```

the response becomes approximately linear.

---

## Small Tip Approximation

```text
sin(α) ≈ α
```

This greatly simplifies analysis.

---

## Advantages

Used in:

- Fast imaging
- Gradient Echo imaging
- Parallel transmit methods

because:

```text
Linear Mathematics
```

can replace the full Bloch solution.

---

# Refocusing

Excitation creates transverse magnetization.

As spins evolve, phase differences appear.

Refocusing is used to bring spins back into phase.

---

## Spin Echo Refocusing

Uses:

```text
180° RF Pulse
```

---

### Spin Echo

```text
90°
   │
   ▼

████

      180°
          │
          ▼

          ████

             Echo
```

Refocusing removes many phase errors produced by:

- B₀ inhomogeneity
- Susceptibility effects

---

# Area vs Energy

RF pulses are often characterized by pulse area.

---

## Pulse Area

```text
Area = ∫ B₁(t)dt
```

Flip angle depends on pulse area.

---

## Important Principle

Two pulses can produce the same flip angle:

```text
High Amplitude
Short Duration
```

or

```text
Low Amplitude
Long Duration
```

provided the pulse area is identical.

---

### Example

```text
Pulse A

████████


Pulse B

██
██
██
██
```

Both may have equal area and therefore equal flip angle.

---

# Linearity

For large flip angles:

```text
Bloch Equation
must be solved.
```

Behavior is nonlinear.

---

For small flip angles:

```text
Linear Approximation
```

is valid.

This significantly simplifies RF pulse design.

---

# Fourier Transform vs Bloch Equation

Two major approaches are used to analyze excitation.

---

## Bloch Equation Approach

Solves:

```text
dM/dt = γ(M × B)
```

Advantages:

- Exact
- Works for all flip angles
- Describes full magnetization dynamics

Disadvantages:

- Computationally intensive

---

## Fourier Transform Approach

Used for:

```text
Small Tip-Angle Excitation
```

Advantages:

- Simpler mathematics
- Faster analysis
- RF pulse design becomes easier

---

## Key Relationship

Under the small-tip-angle approximation:

```text
Excitation Profile

≈

Fourier Transform
of RF Pulse
```

This is a fundamental concept in MRI pulse design.

---

# Excitation Process Summary

```text
B₀ Aligns Spins
         ↓
Apply RF Pulse (B₁)
         ↓
Magnetization Rotates
         ↓
Flip Angle Created
         ↓
Transverse Magnetization Forms
         ↓
Precession
         ↓
MR Signal Generated
```

---

# Key Takeaways

- Excitation rotates magnetization away from the z-axis.
- Excitation is produced by the RF magnetic field B₁.
- B₁ is applied perpendicular to B₀ and rotates at the Larmor frequency.
- During RF excitation, relaxation effects are usually negligible.
- The Bloch equation simplifies to `dM/dt = γ(M × B)`.
- The rotating frame transforms a complex rotating-field problem into a simpler stationary-field problem.
- The amount of rotation is called the flip angle (α).
- Flip angle is determined by RF pulse area: `α = γ∫B₁(t)dt`.
- Slice-selective excitation uses RF pulses together with gradients.
- Slice thickness depends on RF bandwidth and gradient strength.
- Small-tip-angle excitation allows linear approximations and Fourier-transform methods.
- Large flip-angle excitation generally requires full Bloch equation analysis.
- Refocusing pulses restore spin coherence and generate echoes.
- Excitation creates transverse magnetization, which is ultimately responsible for the detectable MR signal.
