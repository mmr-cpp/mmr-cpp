# Imaging Sequences

Imaging sequences consist of coordinated:

- RF waveforms
- Gradient waveforms
- Control signals

that are played out over time to generate MRI images.

Different imaging sequences are designed to highlight different tissue characteristics, including:

- Proton Density (PD)
- T₁ Relaxation
- T₂ Relaxation
- Diffusion
- Perfusion
- Blood Flow
- Chemical Shift

Differences in these tissue properties appear as differences in signal intensity, producing image contrast.

The wide variety of imaging sequences is one of MRI's greatest strengths and a major reason for its clinical versatility.

---

# Components of an Imaging Sequence

A typical MRI sequence coordinates:

```text
RF Pulse
Gradient Fields
ADC (Receiver)
MR Signal
```

to acquire image data.

A sequence controls:

- When spins are excited
- How spatial encoding is performed
- When signals are acquired
- How contrast is generated

---

# Example: Spin Echo Sequence

Spin Echo (SE) is one of the most widely used MRI sequences.

It uses:

- A 90° excitation pulse
- A 180° refocusing pulse

to generate an echo.

---

## Spin Echo Timing Diagram

```text
Time ─────────────────────────────────────────►

RF

      90°                180°
       │                   │
       ▼                   ▼

       █                   █


Slice Gradient

     ┌───┐             ┌───┐
─────┘   └─────────────┘   └────────────


Phase Gradient

          ┌─────┐
──────────┘     └───────────────────────

(Repeated with different amplitudes)


Readout Gradient

                         ┌─────────┐
─────────────────────────┘         └────


ADC

                           ┌─────┐
───────────────────────────┘     └──────


MR Signal

              FID

               \
                \
                 \
                  \_____/\
                         ↑
                       Echo
```

---

## Why Spin Echo?

Advantages:

- Excellent image quality
- Reduced sensitivity to B₀ inhomogeneity
- Robust against susceptibility effects

Disadvantages:

- Longer scan times

---

# Why Different Sequences Are Used

Different imaging sequences are designed to:

- Increase sensitivity to specific tissue properties.
- Enhance T₁, T₂, diffusion, perfusion, or flow contrast.
- Suppress specific tissues such as fat or CSF.
- Reduce scan times.
- Improve image quality.
- Reduce artifacts.
- Improve motion robustness.
- Increase temporal resolution.

---

# MRI Sequence Reference Table

| Sequence | Description / Primary Use |
|-----------|--------------------------|
| **SE (Spin Echo)** | Uses a 180° RF pulse to refocus the echo. Produces PD-, T₁-, or T₂-weighted images. |
| **STIR (Short Tau Inversion Recovery)** | Inversion Recovery sequence with TI selected to suppress fat signal. Commonly used for edema and lesion detection. |
| **FLAIR (Fluid Attenuated Inversion Recovery)** | Inversion Recovery sequence with TI selected to suppress CSF signal. Widely used in neuroimaging. |
| **FSE (Fast Spin Echo)** | Uses multiple 180° RF pulses to rapidly fill k-space and reduce scan time while maintaining SE contrast. |
| **SSFSE (Single Shot Fast Spin Echo)** | Acquires an entire image following a single excitation using a long echo train. Extremely fast and useful for motion-prone patients. |
| **FR-FSE (Driven Equilibrium FSE)** | Adds 180° and 90° pulses to restore longitudinal magnetization, increasing T₂ weighting. |
| **FIR (Fast Inversion Recovery)** | Inversion Recovery combined with FSE. Often used to create FIR-STIR or FIR-FLAIR images. |
| **GRE (Gradient Echo)** | Uses gradient reversal instead of a 180° pulse for echo formation. Produces PD-, T₁-, and T₂*-weighted images depending on TR, TE, and flip angle. |
| **FGRE (GRASS)** | Steady-state gradient echo sequence with short TR for faster acquisition and T₂* weighting. |
| **SPGR (Spoiled Gradient Recalled Echo)** | Uses RF spoiling to eliminate residual transverse magnetization, producing strong T₁-weighted images. |
| **SSFP (Steady-State Free Precession)** | Simultaneously samples spin echoes and stimulated echoes, providing T₂-weighted contrast. |
| **FIESTA** | Balanced SSFP sequence with high SNR and bright fluid signal. Produces mixed T₂/T₁ contrast. |
| **FIESTA-C** | Phase-balanced FIESTA with alternating RF phase to reduce fat-related artifacts and improve image quality. |
| **EPI (Echo Planar Imaging)** | Uses rapid gradient reversals to collect data extremely quickly. Basis for DWI and fMRI. |
| **DWI (Diffusion Weighted Imaging)** | Measures water molecule diffusion. Essential for acute stroke and tissue characterization. |
| **PROPELLER** | Radial k-space acquisition technique providing motion correction and improved image quality. Can be combined with FSE, FIR, DWI, and SE. |
| **TRICKS** | Time-resolved MR angiography technique used to visualize contrast kinetics and blood flow dynamics. |
| **IDEAL** | Fat-water separation technique using phase-sensitive acquisition. Produces fat-only and water-only images. |
| **ASSET** | Parallel imaging technique that reduces k-space sampling requirements and scan time. Improves temporal resolution. *(Technique rather than a pulse sequence.)* |
| **LAVA / VIBRANT** | 3D T₁-weighted Gradient Echo sequence with RF fat saturation, commonly used for dynamic breast and abdominal imaging. |

---

# Key Takeaways

- Imaging sequences coordinate RF pulses, gradients, ADC acquisition, and timing to generate MRI images.
- Different sequences create different image contrast by emphasizing tissue properties such as T₁, T₂, diffusion, flow, and proton density.
- Spin Echo is one of the most fundamental MRI sequences and provides robust image quality.
- Specialized sequences can suppress fat (STIR), suppress CSF (FLAIR), improve speed (FSE, EPI), improve motion robustness (PROPELLER), or provide functional information (DWI, TRICKS).
- Gradient Echo sequences generally provide faster imaging than Spin Echo sequences.
- Parallel imaging techniques such as ASSET reduce scan times by reducing k-space sampling requirements.
- The diversity of imaging sequences is one of MRI's greatest strengths and enables the visualization of anatomy, pathology, flow, diffusion, perfusion, and vascular structures.
