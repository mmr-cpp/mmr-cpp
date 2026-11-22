# Key Takeaways

- MRI uses a fixed coordinate system centered at the imaging isocenter.
- The z-axis is aligned with the magnet bore and defines the longitudinal direction.
- The x-axis and y-axis define the transverse plane.
- B₀ is the static magnetic field and usually points along the +z direction.
- Some MRI systems are reverse-ramped, causing B₀ to point in the −z direction while the coordinate system remains unchanged.
- Certain nuclei, such as hydrogen (¹H), possess spin angular momentum and an associated magnetic moment.
- Hydrogen (¹H) is the primary nucleus used in MRI due to its abundance in the human body.
- When spins are placed in B₀, they tend to align either parallel or antiparallel to the field.
- A slight excess of spins occupies the lower-energy state, producing a net magnetization vector **M**.
- Spins in a static magnetic field experience a torque.
- This torque causes the magnetic moments and net magnetization to precess around B₀.
- The rotational motion is called **precession**.
- Precession occurs at the **Larmor frequency**:

  `ω₀ = γB₀`

- The gyromagnetic ratio (γ) depends on the nucleus being observed.
- For hydrogen (¹H), γ = 42.57638507 MHz/T.
- Precession is the fundamental physical phenomenon that gives rise to the MRI signal.
- The apparent direction of precession depends on the polarity of B₀:
  - Forward-ramped system → Clockwise (CW)
  - Reverse-ramped system → Counterclockwise (CCW)

- To generate a measurable MR signal, a second magnetic field called **B₁** is applied.
- B₁ is the radio-frequency (RF) magnetic field and is applied perpendicular to B₀.
- B₁ rotates at the same Larmor frequency as the spins.
- B₁ tips the magnetization vector away from B₀ and toward the transverse plane.
- The tipping angle is called the **flip angle (α)**.
- The tipping process is known as **excitation**, and B₁ is therefore often called the **excitation field**.
- The excitation rate is given by:

  `ω₁ = γB₁`

- The flip angle is determined by the RF pulse strength and duration:

  `α = γ ∫ B₁(t) dt`

- B₁ is much weaker than B₀ but is highly effective because it operates at the resonance (Larmor) frequency.
- The application of B₁ creates transverse magnetization.
- The transverse magnetization precesses and induces a voltage in the receiver coil.
- This induced voltage forms the MRI signal.
- MRI signals arise through the interaction of nuclear spins with the static magnetic field **B₀** and the radio-frequency field **B₁**.
