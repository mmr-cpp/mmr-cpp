# The Bloch Equation

Net magnetization is dynamic. During a typical MRI scan, the magnetization vector **M** is constantly changing in both direction and magnitude.

A major part of MRI physics involves controlling **M** and analyzing its behavior to determine tissue properties.

The dynamics of the magnetization vector are described by the **Bloch Equation**.

---

## Bloch Equation

```text
dM/dt = γ(M × B)
        - (Mx/T₂) î
        - (My/T₂) ĵ
        - ((Mz - M₀)/T₁) k̂
```

where:

- **M** = net magnetization vector
- **B** = applied magnetic field
- **γ** = gyromagnetic ratio
- **T₁** = spin-lattice (longitudinal) relaxation time constant
- **T₂** = spin-spin (transverse) relaxation time constant
- **M₀** = equilibrium magnetization produced by B₀
- **î, ĵ, k̂** = unit vectors in the x, y, and z directions
- **Mx, My, Mz** = x, y, and z components of magnetization

---

## Components of the Bloch Equation

### 1. Magnetic Field Interaction

```text
γ(M × B)
```

This term describes how magnetization interacts with external magnetic fields.

Effects:

- Causes magnetization to precess around the magnetic field.
- Changes the direction of M.
- Does not change the magnitude of M.

If M is aligned with B:

```text
M × B = 0
```

and no precession occurs.

---

### 2. Transverse Relaxation (T₂ Relaxation)

```text
-(Mx/T₂) î
-(My/T₂) ĵ
```

These terms describe the decay of transverse magnetization.

Effects:

- Causes magnetization in the x-y plane to decrease.
- Reduces the MR signal.
- Characterized by the time constant **T₂**.

---

### 3. Longitudinal Relaxation (T₁ Relaxation)

```text
-((Mz - M₀)/T₁) k̂
```

This term describes recovery of longitudinal magnetization.

Effects:

- Causes Mz to return toward its equilibrium value M₀.
- Characterized by the time constant **T₁**.
- Represents energy transfer from spins to the surrounding lattice.

---

# Relaxation Processes

After an RF pulse:

- Longitudinal magnetization (Mz) is reduced.
- Transverse magnetization (Mxy) is created.
- T₁ and T₂ relaxation begin simultaneously.

---

## T₁ Relaxation (Longitudinal Recovery)

Also called:

- Spin-Lattice Relaxation
- Longitudinal Relaxation

During T₁ relaxation, magnetization gradually recovers toward the +z direction.

### T₁ Recovery Diagram

```text
Mz
↑

M₀ |                           ●
   |                        ●
   |                     ●
   |                  ●
   |               ●
   |            ●
   |         ●
   |      ●
   |   ●
   |●
   +--------------------------------→ Time

Longitudinal Magnetization Recovery
Mz → M₀
```

### Physical View

```text
Immediately After RF Pulse

      +z
       ↑
       |
       |
O ─────→ M

Mz ≈ 0


During T₁ Recovery

      +z
       ↑
      /
     /
    /
   /
  M


At Equilibrium

      +z
       ↑
       |
       |
       M

Mz = M₀
```

---

## T₂ Relaxation (Transverse Decay)

Also called:

- Spin-Spin Relaxation
- Transverse Relaxation

During T₂ relaxation, spins gradually lose phase coherence.

As a result:

- Transverse magnetization decreases.
- MR signal decreases.

### T₂ Decay Diagram

```text
Mxy
↑

Mxy₀ |●
      |  ●
      |     ●
      |        ●
      |           ●
      |              ●
      |                  ●
      |                      ●
      |                          ●
      +--------------------------------→ Time

Transverse Magnetization Decay
Mxy → 0
```

### Physical View

```text
Immediately After Excitation

      ↗
   ↗
      ↗
   ↗

Spins in phase

Strong transverse magnetization
```

```text
Later

      ↗

            →

   ↘

               ↑

Spins losing phase coherence

Reduced transverse magnetization
```

```text
Long After Excitation

↑      →      ↓      ←

Completely dephased

Net transverse magnetization ≈ 0
```

---

# Combined T₁ and T₂ Relaxation

Both processes occur simultaneously.

```text
Magnetization

↑
|\
| \
|  \        T₂ Decay (Mxy)
|   \
|    \
|     \
|      \_____________________
|
|         /
|       /
|     /
|   /
| /
|/___________________________
           T₁ Recovery (Mz)

------------------------------→ Time
```

### Interpretation

- T₁ recovery increases longitudinal magnetization.
- T₂ decay decreases transverse magnetization.
- Together they determine MRI contrast and signal behavior.

---

## Equilibrium State

Relaxation continues until equilibrium is reached.

At equilibrium:

```text
Mx = 0
My = 0
Mz = M₀
```

Therefore:

- No transverse magnetization remains.
- Longitudinal magnetization has fully recovered.
- The relaxation terms become zero.

---

## Physical Meaning of the Bloch Equation

The Bloch Equation combines three key MRI phenomena:

1. Precession caused by magnetic fields.
2. T₂ relaxation causing transverse decay.
3. T₁ relaxation causing longitudinal recovery.

Together, these processes completely describe the time evolution of the magnetization vector.
