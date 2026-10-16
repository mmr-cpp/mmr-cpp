# Patient Considerations

MRI systems are designed around human physiological limitations and safety considerations.

Several important patient-related factors influence MRI system design, performance, and operating limits.

Listed roughly in order of significance:

1. Power deposition (heating)
2. Human body size
3. Effects of changing magnetic fields (dB/dt)
4. RF dielectric effects
5. Acoustic noise

These factors directly affect:

- B₀ field strength
- Bore size
- Gradient performance
- RF system design
- Safety limits

---

# 3.10.1 Power Deposition

MRI uses RF fields to excite nuclear spins.

These RF fields deposit energy into the patient's body and produce heat.

Because the body's ability to dissipate heat is limited, RF power deposition must be controlled.

---

## Specific Absorption Rate (SAR)

Power deposition is commonly expressed as:

```text
SAR (Specific Absorption Rate)

Units:
W/kg
```

SAR scales with body mass.

Typical regulatory limits:

```text
Controlled Operating Mode

≤ 4 W/kg
```

```text
Normal Operating Mode

≤ 2 W/kg
```

---

## Impact on MRI Design

Power deposition affects:

### B₀ Field Strength

Higher magnetic field strengths generally require:

```text
Higher RF Frequencies
```

which tend to increase:

```text
SAR
```

---

### RF Transmit System

SAR limits constrain:

- Maximum RF power
- Pulse sequence design
- Scan duration
- Flip angle selection

---

## Key Points

```text
More RF Power
      ↓
More Tissue Heating
      ↓
Higher SAR
      ↓
More Imaging Restrictions
```

---

# 3.10.2 Body Size

Whole-body MRI systems must accommodate the widest portions of the body.

Typically:

- Shoulders
- Torso
- Hips

determine minimum bore size.

---

## Historical Bore Sizes

```text
Early Systems       : 55 cm

1990s Systems       : 60 cm

Modern Systems      : 60–70 cm
```

---

## Advantages of Larger Bores

- Improved patient comfort
- Easier patient positioning
- Reduced claustrophobia
- Better patient access

---

## Engineering Challenges

Larger bore sizes increase:

- Magnet cost
- Gradient cost
- Power requirements

```text
Larger Bore
      ↓
Higher Cost
      ↓
More Power Needed
```

---

# 3.10.3 Changing Magnetic Fields (dB/dt)

MRI gradients create rapidly changing magnetic fields.

These changing fields are often described as:

```text
dB/dt
```

Because the human body is electrically conductive:

```text
Changing Magnetic Field
        ↓
Induced Voltage
        ↓
Induced Current
```

---

## Possible Biological Effects

### Peripheral Nerve Stimulation (PNS)

Patients may experience:

- Tingling
- Muscle twitching
- Discomfort

---

### Pain

At higher stimulation levels:

```text
Pain Threshold
```

may be reached.

---

### Cardiac Stimulation

Extremely large induced currents can affect cardiac tissue.

Modern clinical MRI systems operate below this threshold.

---

## Safety Regulations

MRI systems must comply with:

```text
IEC 60601-2-33
```

which limits gradient performance to minimize:

- PNS
- Pain
- Safety risks

---

## High-Field Systems

Very high field systems (7T and above) introduce additional concerns.

Patient motion through the strong static field may cause:

- Dizziness
- Nausea
- Vertigo

---

# 3.10.4 RF Effects

The human body interacts strongly with RF fields.

These interactions increase with:

- B₀ field strength
- Patient size
- RF frequency

---

## Why RF Effects Increase at Higher Fields

As B₀ increases:

```text
Larmor Frequency Increases
        ↓
RF Wavelength Decreases
        ↓
Field Uniformity Becomes Harder
```

---

## Dielectric Shading

Human tissue has a dielectric constant:

```text
ε > 1
```

often:

```text
ε >> 1
```

This shortens RF wavelength inside the body.

---

### Approximate RF Wavelength

At 3T:

```text
λ ≈ 25 cm
```

At 7T:

```text
λ ≈ 10 cm
```

---

## Dielectric Shading Effect

When body dimensions become comparable to wavelength:

```text
RF Interference
```

occurs.

This causes:

```text
Signal Bright Regions
+
Signal Dark Regions
```

within the image.

---

### Dielectric Shading Diagram

```text
Uniform RF Field

██████████████████


Inside Human Body

██████▒▒▒▒▒██████

Signal Shading
```

---

## Common Location

Most noticeable in:

```text
Torso Imaging
```

especially:

- Abdomen
- Pelvis
- Large body regions

---

## Induced Currents

RF fields induce:

### Displacement Currents

Produced by dielectric properties.

### Conductive Currents

Produced by tissue conductivity.

These currents generate secondary magnetic fields.

---

## Consequences

Secondary fields modify:

```text
Desired B₁ Field
```

leading to:

- Non-uniform flip angles
- Signal variation
- Bright centers
- Dark edges

---

### RF Uniformity

```text
Low Dielectric Material
         ↓
Uniform B₁
         ↓
Uniform Image
```

```text
High Dielectric Material
         ↓
Induced Currents
         ↓
B₁ Distortion
         ↓
Image Shading
```

---

# 3.10.5 Acoustic Limits

MRI gradients generate large mechanical forces.

---

## Origin of MRI Noise

Gradient coils carry:

```text
Rapidly Changing Current
```

while inside:

```text
Strong Static Field (B₀)
```

The interaction creates:

- Vibration
- Torque
- Mechanical motion

---

## Speaker Analogy

The gradient coil behaves similarly to:

```text
A Loudspeaker
```

where electrical currents generate physical motion.

---

## Acoustic Noise Generation

```text
Gradient Current
         ↓
Magnetic Force
         ↓
Coil Vibration
         ↓
Sound Waves
```

---

## Risks

High acoustic levels may cause:

- Temporary ringing in ears
- Temporary hearing loss
- Permanent hearing damage

---

## Protection

Patients use:

- Ear plugs
- Hearing protection
- Noise reduction systems

---

## Regulatory Control

MRI acoustic output is limited by:

```text
IEC 60601-2-33
```

to protect patients and healthcare workers.

---

# Summary Diagram

```text
PATIENT CONSIDERATIONS

┌─────────────────────────────┐
│  RF POWER                   │
│  → SAR / Heating            │
└─────────────┬───────────────┘
              │
              ▼

┌─────────────────────────────┐
│  BODY SIZE                  │
│  → Bore Diameter            │
└─────────────┬───────────────┘
              │
              ▼

┌─────────────────────────────┐
│  dB/dt                      │
│  → PNS / Pain               │
└─────────────┬───────────────┘
              │
              ▼

┌─────────────────────────────┐
│  RF EFFECTS                 │
│  → Shading / Nonuniformity  │
└─────────────┬───────────────┘
              │
              ▼

┌─────────────────────────────┐
│  ACOUSTIC NOISE             │
│  → Hearing Protection       │
└─────────────────────────────┘
```

---

# Key Takeaways

- Patient safety considerations strongly influence MRI system design.
- RF excitation deposits energy into tissue and is limited through SAR regulations.
- SAR is measured in W/kg and increases with RF power and field strength.
- Bore size is determined largely by patient anatomy and comfort requirements.
- Larger bore systems improve comfort but increase magnet and gradient costs.
- Rapidly changing gradient fields (dB/dt) induce currents in the body that can cause peripheral nerve stimulation.
- MRI systems are regulated to minimize PNS and prevent cardiac stimulation.
- RF wavelength decreases as field strength increases, leading to RF nonuniformity and dielectric shading.
- Dielectric effects are most pronounced at 3T and higher field strengths.
- Gradient coil vibration produces significant acoustic noise.
- Hearing protection is required because MRI systems can generate sound levels capable of causing hearing damage.
- System performance must always be balanced against patient safety, comfort, and regulatory limits.
