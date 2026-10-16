# Gradient Fields

## Why Are Gradient Fields Needed?

The static magnetic field **B₀** is designed to be highly homogeneous throughout the imaging volume.

If B₀ were exactly the same everywhere:

- All spins would experience the same magnetic field.
- All spins would precess at the same Larmor frequency.
- Signals from different locations would be indistinguishable.
- No spatial information could be obtained.

In other words, MRI would be unable to determine where a signal originated.

Gradient fields solve this problem by making the magnetic field vary with position.

As a result:

- Spins at different locations experience different magnetic fields.
- Spins precess at different frequencies.
- Signals can be spatially localized.

This spatial encoding is the foundation of MRI.

---

## Historical Importance

The introduction of magnetic field gradients transformed Nuclear Magnetic Resonance (NMR) into Magnetic Resonance Imaging (MRI).

Gradient-based spatial encoding enabled:

- Localization of signals
- Two-dimensional imaging
- Three-dimensional imaging
- Fast image acquisition

A simplified view of MRI history:

```text
NMR
 ↓
Addition of Gradient Fields
 ↓
Spatial Encoding
 ↓
MRI
```

---

# What Is a Gradient Field?

A gradient field is a magnetic field whose strength changes with position.

Unlike the static field B₀, which is approximately constant, a gradient field increases or decreases linearly across space.

MRI uses three gradient fields:

- Gx (x-gradient)
- Gy (y-gradient)
- Gz (z-gradient)

Each gradient provides spatial encoding along one axis.

---

## MRI Gradient System

```text
                +z

                 ↑
                 │
                 │
                 │
                 O────→ +x
                /
               /
             +y

Encoding Axes

Gx → Left/Right encoding
Gy → Up/Down encoding
Gz → Head/Foot encoding
```

---

# Ideal Gradient Fields

## X-Gradient Field

An ideal x-gradient field varies with x-position.

```text
B(x,y,z) = Gx · x · k̂
```

where:

- Gx = x-gradient strength
- x = position along x-axis
- k̂ = unit vector in the z-direction

Notice that the field still points along z.

Only its strength changes with x.

---

### X-Gradient Diagram

```text
Field Strength

      +
      |
      |
      |
------O------→ x
      |
      |
      |
      -

x < 0  → Negative field contribution
x = 0  → Zero gradient contribution
x > 0  → Positive field contribution
```

---

## Y-Gradient Field

Similarly,

```text
B(x,y,z) = Gy · y · k̂
```

where:

- Gy = y-gradient strength
- y = position along y-axis

The field remains z-directed but changes strength with y position.

---

### Y-Gradient Diagram

```text
          +y

           +
           |
           |
           |
-----------O-----------
           |
           |
           |
           -

Field strength varies with y
```

---

## Z-Gradient Field

Likewise,

```text
B(x,y,z) = Gz · z · k̂
```

where:

- Gz = z-gradient strength
- z = position along z-axis

---

# Characteristics of Gradient Fields

Ideal gradients have the following properties:

- Linear variation with position.
- Infinite spatial extent (theoretical).
- Only z-directed magnetic field components.
- Constant rate of change.

Real gradient systems:

- Have limited regions of linearity.
- Produce small x and y field components.
- Deviate from ideal behavior at large distances from isocenter.

---

# Gradient Strength

The rate at which the magnetic field changes with position is called the gradient strength.

Typical whole-body MRI systems:

```text
2 – 10 G/cm

or

20 – 100 mT/m
```

Stronger gradients generally allow:

- Faster imaging
- Higher spatial resolution
- More efficient spatial encoding

---

## Relative Size of B₀ and Gradient Fields

Gradient fields are much weaker than B₀.

Typical values:

```text
B₀ = 1.5 T
    = 15,000 G

Strong Gradient:
5 G/cm
```

At an offset of 24 cm:

```text
Gradient Contribution

= (5 G/cm)(24 cm)

= 120 G
```

Comparison:

```text
Static Field    : 15,000 G
Gradient Field  :    120 G
```

Thus B₀ is approximately two orders of magnitude larger.

---

# Frequency Mapping

When a gradient field is added, the total magnetic field becomes position dependent.

For an x-gradient:

```text
B(x) = B₀ + Gx x
```

The Larmor frequency becomes:

```text
f(x) = γB(x)
```

Substituting:

```text
f(x) = γ(B₀ + Gx x)

     = γB₀ + γGx x
```

or

```text
f(x) = f₀ + γGx x
```

where:

- f₀ = center frequency at isocenter
- γGx x = frequency shift caused by the gradient

---

## Frequency-Encoding Concept

Without a gradient:

```text
Location A → 63.8646 MHz
Location B → 63.8646 MHz
Location C → 63.8646 MHz

Impossible to distinguish locations
```

With a gradient:

```text
Location A → 63.5 MHz

Location B → 63.9 MHz

Location C → 64.3 MHz
```

Now different frequencies correspond to different locations.

---

## Frequency Mapping Diagram

```text
Frequency

64.3 MHz  ●
           \
            \
             \
63.9 MHz  ●---\----------
               \
                \
63.5 MHz    ●----\--------

--------------------------------→ Position
```

Different positions produce different frequencies.

MRI uses this relationship to determine spatial location.

---

# Why Gradients Enable Imaging

The complete process is:

1. B₀ creates spin alignment and precession.
2. Gradient fields change magnetic field strength with position.
3. Different positions acquire different frequencies.
4. Receiver coils detect a mixture of frequencies.
5. MRI reconstruction algorithms convert those frequencies into image locations.
6. An anatomical image is produced.

Without gradients:

```text
No Spatial Encoding
→ No Image
```

With gradients:

```text
Spatial Encoding
→ Frequency Differences
→ Position Information
→ MRI Image
```

---

