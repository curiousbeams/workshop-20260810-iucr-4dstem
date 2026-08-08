---
title: Differential Phase Contrast
label: dpc_page
numbering:
  enumerator: 12.%s
---

:::{admonition} Learning goals
:class: tip
- Relate the center-of-mass shift of the diffraction pattern to the local gradient of the sample potential.
- Build a center-of-mass signal from segmented and from pixelated detectors.
- Integrate the center of mass to recover a phase image, and recognize the failure modes.
:::

Having established the importance and utility of PCI in HRTEM, we turn our attention to 4D-STEM methods which retrieve the lost phase information algorithmically.

The first such technique follows from the observation that when an electron beam interacts with a sample the center of mass (CoM) of the momentum distribution of the beam shifts.
This can be observed in the first interactive figure we introduced, [](#fig_stem_measurements) reproduced below:

:::{figure} #app:stem_measurements
:name: fig_stem_measurements_dpc
:placeholder: ./figures/stem_measurements_placeholder.png
:::

The nature of the CoM shift is related to the probe and sample features size and can thus be used to reconstruct the projected electrostatic potential {cite:p}`10.1016/j.ultramic.2015.10.011`.
Atomic-scale features lead to a redistribution of signal within the BF disk while long-range fields, such as electric and magnetic fields, cause near-rigid shifts of the entire bright-field disk.

## Pixelated Detector CoM

While early developments used segmented detectors, more recently with multiple annular and radial rings {cite:p}`10.1093/jmicro/dfq014`, to estimate the center of mass shifts, here we will demonstrate the technique with pixelated detector measurements.

Specifically, we form two virtual images using the first moment detectors:
```{math}
:label: com_detector
\begin{align}
    CoM_x(\bm{R}) &= \int k_x\, I(\bm{R},\bm{k})\, d\bm{k} \\
    CoM_y(\bm{R}) &= \int k_y\, I(\bm{R},\bm{k})\, d\bm{k},
\end{align}
```
where $I(\bm{R},\bm{k})$ is the 4D-STEM datacube we introduced in [](#detectors_phonons_page).

The measured CoM vector $\vec{CoM}(\bm{R}) = \left(CoM_x(\bm{R}), CoM_y(\bm{R})\right)$ is proportional to the in-plane gradient of the projected electrostratic potential $\nabla_{\bm{r}}V(\bm{R})$, which can be reconstructed using Fourier integration:
```{math}
:label: fourier_integration
V(\bm{R}) = \mathbb{R}\left\{\mathcal{F}_{\bm{k}\rightarrow\bm{R}}^{-1} \left[ \frac{i \bm{k}}{k^2} \cdot \mathcal{F}_{\bm{R}\rightarrow \bm{k}} \left[\vec{CoM}(\bm{R}) - \nabla_{\bm{r}}V(\bm{R}) \right] \right] \right\}, 
```

where $\mathbb{R}\left\{\cdot \right\}$ denotes taking the real-part of the complex-valued expression.

[](#fig_dpc) illustrates the above steps interactively on a gold nanoparticle.
Move the cursor over the left panel to place the probe and watch the CoM crosshairs in the diffraction pattern track the local potential gradient.
Toggling "show reconstruction" reveals the reconstruction gradient $\vec{CoM}(\bm{R})$ accumulated over the full scan, and "integrate gradient" applies [](#fourier_integration) to recover the projected potential itself.

```{figure} #app:center_of_mass_imaging
:name: fig_dpc
```

Two controls are worth exploring in [](#fig_dpc).
The **convergence semi-angle** sets the probe size, and therefore the spatial resolution of the reconstruction: too small and the probe cannot resolve the atomic columns, too large and the CoM shift within the disk becomes hard to interpret.
The **dose** slider adds Poisson shot noise. CoM imaging is remarkably dose-efficient, and the reconstructed potential survives to surprisingly low doses, which is exactly why the technique matters for beam-sensitive samples.

```{attention} Try it yourself!
Click the following badge to try a complete notebook on Colab:  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://githubtocolab.com/curiousbeams/workshop-20260810-iucr-4dstem/blob/main/notebooks/try-it-yourself/01.dpc_01.ipynb)
```
