---
title: Strain Mapping
short_title: Strain Mapping
label: strain_mapping_page
numbering:
  enumerator: 9.%s
---

:::{admonition} Learning goals
:class: tip
- Understand how a lattice distortion shows up in the positions of the Bragg disks.
- Run the strain workflow: lattice fitting from detected disks, then strain relative to a reference.
- Know the main precision limits, and the tricks (precession, patterned probes) that push past them.
:::

```{image} ./figures/cover-strain.jpg
:alt: Schematic of nanobeam strain mapping, in which a converged probe scanned over a strained crystal produces diffraction patterns whose Bragg disk positions encode the local lattice vectors
:width: 100%
```

Strain, the local deviation of the lattice from its relaxed spacing, controls the band structure of semiconductor devices, mobility in strained channels, ferroelastic domain patterns, and the mechanical response around defects and precipitates.
Nanobeam strain mapping {cite:p}`10.1063/1.4922994` measures it directly: the positions of the Bragg disks in each diffraction pattern encode the local reciprocal lattice vectors, so tracking how the disk positions shift as the probe scans gives the full 2D strain tensor ($\varepsilon_{xx}$, $\varepsilon_{yy}$, $\varepsilon_{xy}$, and the lattice rotation $\theta$) at every probe position, over fields of view of microns at nanometer resolution.

Among the many strain-measurement techniques available in the TEM {cite:p}`10.1016/j.ultramic.2013.03.014`, including geometric phase analysis of high-resolution images {cite:p}`10.1016/S0304-3991(98)00035-7`, nanobeam diffraction stands out for combining large fields of view, high precision, and modest dose.

## Strain in the Diffraction Pattern

The relationship between strain and the location of the nanobeam CBED disks is illustrated interactively in [](#fig_strain) for a uniaxially-strained Au sample.
As the lattice constant increases along the $x$-direction, the spacing of the diffracted disks along the $k_x$ direction decreases.
This is just the duality of real- and reciprocal-space that we have used throughout: when one expands, the other contracts.

:::{figure} #app:strained_gold
:name: fig_strain
:placeholder: ./figures/strain_mapping_placeholder.png
Strained Au nanobeam diffraction. Vary the applied strain and watch the diffracted disk spacing respond.
:::

Two things are worth noticing while you play with [](#fig_strain).
First, the effect is *small*: a 1% lattice change moves the disks by 1% of their spacing, which is a fraction of a pixel on a typical detector. This is why sub-pixel disk registration is the whole ball game.
Second, higher-order reflections move further in absolute terms than low-order ones, which is why the weak, high-$|k|$ disks are worth the effort to detect.

## The Workflow

1. **Probe template and disk detection.** As covered in [](#calibration_disk_detection_page). Use `'multicorr'` subpixel refinement here, not `'poly'`.
2. **Calibration.** Correct the origin (descan), the elliptical distortion, and the real-space to reciprocal-space rotation. Calibration errors map directly into artificial strain, and an uncorrected ellipticity in particular masquerades as a uniform biaxial strain field.
3. **Lattice fitting.** Choose basis vectors $\bm{g}_1$ and $\bm{g}_2$ from the Bragg vector map, ideally well-separated and close to perpendicular, and fit the full lattice at every probe position using all detected reflections rather than just those two.
4. **Strain from a reference.** Strain is always measured *relative to a reference lattice*: either the median lattice over a region of interest known to be unstrained, or manually specified reference vectors. The transformation between the local and reference lattice vectors, rotated into your chosen coordinate system, gives $\varepsilon_{xx}$, $\varepsilon_{yy}$, $\varepsilon_{xy}$ and $\theta$.

:::{figure} ./figures/strain-maps.jpg
:name: fig_strain_maps
:width: 100%
A complete result: the mean diffraction pattern with the fitted reciprocal lattice, and the four strain-tensor component maps ($\varepsilon_{xx}$, $\varepsilon_{yy}$, $\varepsilon_{xy}$, $\theta$) across a multilayer structure.
:::

The choice of reference region is a physics decision, not a software one. Strain maps are only as meaningful as the reference lattice they are measured against, and "the top-left corner looked flat" is not a physical argument.

## Precision and Pitfalls

Disk registration precision improves with sharp, uniform disk edges, which is where the convergence angle, the sample thickness, and the probe shape all enter.
Precision of $\sim 10^{-4}$ relative strain is achievable in favorable cases; a few $\times 10^{-3}$ is routine.

**Dynamical contrast inside the disks** is the main systematic. Thickness and mistilt vary across real samples and modulate the intensity *within* each disk, which biases center-fitting. Robust registration algorithms help, and so does precession {cite:p}`10.1107/S2052252514022283`, which we look at more closely in [](#orientation_mapping_page): rocking the beam through a cone integrates over the rocking curve and washes out the dynamical structure.

:::{figure} ./figures/maped-strain.jpg
:name: fig_precession_strain
:width: 100%
Conventional (top) versus precession/multi-beam-averaged (bottom) acquisition of the same region. Averaging through the rocking condition suppresses the dynamical intensity variations inside the disks, and the strain maps get visibly cleaner.
:::

**Patterned "bullseye" probes** attack the same problem from the optics side. Apertures with concentric rings or radial spokes milled into the condenser aperture imprint sharp internal structure onto every Bragg disk, improving registration precision severalfold at fixed dose {cite:p}`10.1016/j.ultramic.2019.112890`.

:::{figure} ./figures/bullseye-apertures.jpg
:name: fig_bullseye_apertures
:width: 60%
Bullseye and patterned condenser apertures fabricated with a focused ion beam. Installed in the condenser system, they shape every diffraction disk into a self-registering target.
:::

:::{figure} ./figures/bullseye-detection.jpg
:name: fig_bullseye_detection
:width: 90%
Disk detection with a bullseye probe: the patterned template cross-correlates sharply against each reflection, even where diffraction contrast varies across the disk.
:::

Two derived quantities are worth computing routinely: the **strain dilation** $\varepsilon_{xx} + \varepsilon_{yy}$, which isolates the volumetric part and is insensitive to the choice of in-plane axes, and **strain statistics over segmented regions**, for example comparing precipitates against the surrounding matrix in irradiated alloys {cite:p}`10.1016/j.actamat.2025.121095`.

```{attention} Try it yourself!
Run the full workflow from vacuum probe to strain maps, including the descan, ellipticity, and rotation calibrations:  
▶️ [Strain mapping of a partially cycled LiFePO₄ battery cathode](https://drive.google.com/file/d/1aQaRR-XZzCZayfYsSDVuz7prLeolsVKI/view?usp=drive_link)
```
