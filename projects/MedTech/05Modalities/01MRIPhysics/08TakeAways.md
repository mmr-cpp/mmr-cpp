# Key Takeaways

- MRI uses a fixed coordinate system centered at the imaging isocenter.
- The z-axis is aligned with the magnet bore and defines the longitudinal direction.
- The x-axis and y-axis define the transverse plane.
- B₀ is the static magnetic field and is designed to be highly homogeneous throughout the imaging volume.
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

- B₁ is much weaker than B₀ but is highly effective because it operates at the resonance frequency.
- The application of B₁ creates transverse magnetization.
- The transverse magnetization precesses and induces a voltage in the receiver coil.
- This induced voltage forms the MRI signal.

- The Bloch Equation describes how magnetization changes with time.
- Magnetization interacts with magnetic fields through the cross-product term:

  `γ(M × B)`

- The cross-product term causes precession but does not change the magnitude of **M**.
- T₂ relaxation causes transverse magnetization (**Mx** and **My**) to decay.
- T₁ relaxation causes longitudinal magnetization (**Mz**) to recover toward **M₀**.
- T₂ relaxation is also called **spin-spin relaxation**.
- T₁ relaxation is also called **spin-lattice relaxation**.
- T₁ recovery and T₂ decay occur simultaneously after RF excitation.
- T₁ controls recovery along the z-axis.
- T₂ controls loss of magnetization in the transverse plane.
- MRI contrast and signal behavior are strongly influenced by T₁ and T₂ relaxation.
- At equilibrium:

  `Mx = 0`

  `My = 0`

  `Mz = M₀`

- A perfectly homogeneous B₀ field provides no spatial information because all spins would precess at the same frequency.
- Gradient fields introduce controlled spatial variations in magnetic field strength.
- MRI uses three gradient fields:
  - **Gx** for x-axis encoding
  - **Gy** for y-axis encoding
  - **Gz** for z-axis encoding
- Gradient fields vary approximately linearly with position.
- Although the gradient field changes with position, its magnetic field remains directed along the z-axis.
- Gradient strength is measured in **G/cm** or **mT/m**.
- Typical whole-body MRI gradient strengths are **20–100 mT/m** (2–10 G/cm).
- Gradient fields are much weaker than B₀.
- Adding a gradient makes the total magnetic field position dependent.
- Because Larmor frequency depends on magnetic field strength, frequency becomes position dependent.
- Different spatial locations therefore produce different resonance frequencies.
- Frequency mapping allows MRI to determine the spatial origin of a signal.
- Gradients provide the spatial encoding required for image formation.
- Spatial encoding using gradients was the key breakthrough that transformed NMR into modern MRI.
- MRI images are ultimately produced through the combined action of:
  - Static field (**B₀**)
  - RF excitation field (**B₁**)
  - Gradient fields (**Gx**, **Gy**, **Gz**)
  - Relaxation processes (**T₁** and **T₂**)
  - Signal detection and reconstruction.
