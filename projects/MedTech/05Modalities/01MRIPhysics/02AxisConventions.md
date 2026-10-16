# Theory of Operations
---

# Axis Conventions

## z-axis (Longitudinal Axis)
- Aligned with the magnet bore (tunnel) axis.
- **+z** direction points into the bore from the patient entry end.
- The direction of **+z** never changes, regardless of magnet ramp direction.

## y-axis (Vertical Axis)
- **+y** points vertically upward.

## x-axis (Horizontal Axis)
- Determined using the right-hand rule.
- When standing at the patient entry end and looking into the bore:
  - **+x** points to the left.

## Origin of the Coordinate System
- Located at the imaging isocenter.
- Isocenter is the point where all three gradient fields (**Gx, Gy, Gz**) are zero.

## Static Magnetic Field (B₀)
- Assumed to point in the **+z** direction unless otherwise stated.
- Some MRI systems can be forward-ramped or reverse-ramped, which changes the polarity of **B₀** relative to **+z**.
- Even if **B₀** polarity changes, the **+z** axis direction remains fixed (into the bore from the patient end).

## Terminology
- **Longitudinal direction** = direction along the z-axis (B₀-aligned axis).
- **Transverse direction** = directions perpendicular to the z-axis, namely the x-axis and y-axis.

---

## Coordinate System Diagram

```text
              +y
               ↑
               |
               |
(+x) ←---------O---------→ (-x)
               |
               |
               ↓
              -y

O = Isocenter

+z : Into the bore
-z : Out of the bore
```

---

# Unit Vectors
The unit vectors in the positive coordinate directions are:

- **î** (x-direction)
- **ĵ** (y-direction)
- **k̂** (z-direction)
  
---

# What are Polar Coordinates?
Instead of describing a point using Cartesian coordinates (**x, y**), a point can be described using:
- **r** = distance from the origin
- **θ (theta)** = angle from a reference direction
Expressed as:
(r, θ)
or
r∠θ

---

# MRI Polar Coordinate Convention
MRI systems use the standard mathematical convention for polar coordinates.
- **0°** (or 0 radians) points along the **+x** direction.
- Angles increase counterclockwise (**CCW**).
Therefore:
- 0° → +x
- 90° → +y
- 180° → -x
- 270° → -y

---

## Polar Coordinate Reference Diagram

```text
             +y (90°)
                 |
                 |
                 |
180° (-x) -------O------- +x (0°)
                 |
                 |
                 |
             -y (270°)
```

