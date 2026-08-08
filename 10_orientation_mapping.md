---
title: Orientation and Phase Mapping
short_title: Orientation Mapping
label: orientation_mapping_page
numbering:
  enumerator: 10.%s
---

:::{admonition} Learning goals
:class: tip
- Relate a crystal's zone-axis orientation to the diffraction pattern it produces.
- Build an orientation plan from a reference structure and match it against measured Bragg peaks.
- Read orientation, correlation-score, and phase maps, and know when to distrust them.
:::

```{image} ./figures/cover-orientation.jpg
:alt: Schematic of automated crystal orientation mapping, in which diffraction patterns from a polycrystalline film are matched against a library of simulated patterns over all orientations
:width: 100%
```

Most functional and structural materials are polycrystalline, and their properties depend on grain size, texture, grain boundary character, and phase distribution.
Automated crystal orientation mapping (ACOM) in 4D-STEM {cite:p}`10.1016/j.matchar.2014.08.010` measures all of these: at every probe position the recorded diffraction pattern is matched against a library of patterns simulated over all plausible crystal orientations, and the best match assigns a local orientation.
It is the transmission analogue of electron backscatter diffraction in the SEM, at nanometer resolution, and on the very same dataset used for strain mapping.

## Orientation in the Diffraction Pattern

The link between the zone-axis orientation and the CBED pattern is less immediate than the strain case.
[](#fig_orientations) shows a simplified polycrystalline sample with three distinct zone-axis orientations of Au.

:::{figure} #app:orientations_gold
:name: fig_orientations
:placeholder: ./figures/orientation_mapping_placeholder.png
Orientation mapping nanobeam diffraction across a three-grain Au sample.
:::

Try positioning the probe in each of the three grains, and then on the boundaries between them.
Note that a grain boundary need not be aligned with the beam direction, so as the probe traverses the sample we observe a *diffuse* boundary in projection, with two overlapping patterns superimposed rather than a sharp switch from one to the other.
Handling that overlap gracefully is one of the things a good matching algorithm has to do.

To match crystalline orientations, then, we need diffraction patterns computed along many zone-axis orientations, and a way to score a measurement against that database.

## The Workflow

1. **Reference structures.** Load the candidate crystal structures, typically from CIF files, and compute their structure factors up to the maximum scattering vector your detector records.
2. **Orientation plan.** Simulate diffraction patterns over a grid of orientations covering the symmetry-reduced zone-axis range: a lookup table of expected Bragg peak positions and intensities. Grid spacings of a few degrees, followed by local refinement of the best match, balance accuracy against speed. Crystal symmetry is what makes this tractable, since only the asymmetric unit of orientation space needs to be sampled.
3. **Bragg peak detection.** Exactly as in [](#calibration_disk_detection_page). The same calibrated Bragg vectors feed both the strain and the orientation analysis, which is one of the quiet strengths of 4D-STEM: one acquisition, several complete analyses.
4. **Correlation matching.** For each probe position, score the measured peaks against the orientation library {cite:p}`10.1017/S1431927622000101` and keep the best match or matches. Returning multiple matches with a minimum angular separation between them is how overlapping grains along the beam direction are handled.
5. **Orientation and phase maps.** The result is an orientation map, conventionally displayed with inverse-pole-figure coloring for the in-plane and out-of-plane directions, plus a per-position correlation score. Running plans for several candidate phases and comparing their correlation scores produces a **phase map** and quantitative phase-fraction estimates.

:::{note} Where the simulations come back
Steps 1 and 2 are dynamical diffraction simulation, the subject of [](#bloch_wave_algorithm_page). In practice ACOM libraries are usually computed kinematically, because a full dynamical calculation at every orientation is expensive and, as the next section explains, precession makes the experiment more kinematical anyway.
:::

## Precession and Pattern Quality

Zone-axis nanobeam patterns are strongly dynamical: intensities oscillate with thickness and with small mistilts, which degrades matching against kinematical templates.
**Precession electron diffraction** {cite:p}`10.1107/S2052252514022283` rocks the beam on a cone, typically 0.3 to 1 degree, while descanning below the sample.
This integrates through the rocking curve and produces more complete, more kinematical-looking patterns.

:::{figure} ./figures/precession-patterns.png
:name: fig_precession_patterns
:width: 100%
The effect of precession: as the rocking radius increases, more reflections are excited and their intensities become more uniform, much closer to the kinematical patterns the orientation library assumes.
:::

The same effect can be simulated, which is a useful way to build intuition for how much precession you actually need.
[](#fig_precession_sim) shows precessed patterns computed for a Si$_3$N$_4$ sample.

:::{figure} #app:blochwave_precession_py4d
:name: fig_precession_sim
:placeholder: ./figures/Si3N4_precession_py4DSTEM.png
Simulated precession electron diffraction patterns as a function of precession angle, computed with the Bloch-wave machinery of [](#bloch_wave_algorithm_page).
:::

Precession substantially improves both orientation reliability and phase discrimination, and it narrows the strain error distribution too, as we saw in [](#fig_precession_strain).

## Phase Mapping in Hard Cases

Phase discrimination gets genuinely difficult when the candidate structures are closely related: polymorphs that share a parent lattice and differ only subtly in symmetry and spacing.
Ferroelectric hafnium zirconium oxide is the canonical example.

:::{figure} ./figures/hzo-polymorphs.jpg
:name: fig_hzo_polymorphs
:width: 100%
The HZO polymorph problem: monoclinic, tetragonal, and two orthorhombic phases (one of them the ferroelectric structure) differ only slightly in lattice parameters and symmetry, a stringent test for any diffraction-based phase mapping.
:::

:::{figure} ./figures/phase-map-comparison.jpg
:name: fig_phase_map_comparison
:width: 100%
Benchmarking phase mapping on simulated HZO data with known ground truth: recovered phase maps and reliability scores can be validated quantitatively before the method is trusted on an experiment.
:::

This is a general and rather crystallographic point. When the phases you are trying to distinguish are close, the honest way to establish that your method can distinguish them is to simulate the data with known ground truth and check that the analysis recovers it, before pointing the analysis at an unknown.

## Practical Notes

- **Matching is only as good as the calibration.** The reciprocal pixel size can be refined by fitting the measured Bragg peak radial distribution against the structure factors of a phase you know is present.
- **Inspect the correlation-score map on its own.** Low scores flag overlapping grains, unindexed phases, or regions where the library simply does not contain the right structure. A pretty orientation map with a uniformly poor score map is telling you something.
- **Two matching philosophies.** Template matching of the *full pattern* (as in `pyxem` {cite:p}`10.1016/j.ultramic.2022.113517`) and sparse matching of *detected peaks* (as in `py4DSTEM`'s ACOM) are complementary. Both are open source, so you can try each on your data.
- **ACOM gives you strain for free.** The distortion between each measured pattern and its best-matched simulation yields a per-phase strain map, with no manual choice of basis vectors, and it works in polycrystals where a single reference lattice would be meaningless.

```{attention} Try it yourself!
ACOM on a two-phase Ti alloy, with phase quantification and strain extracted directly from the matched patterns:  
▶️ [Phase, orientation & strain of a two-phase Ti alloy](https://drive.google.com/file/d/1_DaUuEqq5vx_1ZM5R7zChEo7iA_ccprC/view?usp=drive_link)

Or run the repository notebooks on Colab:  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://githubtocolab.com/curiousbeams/workshop-20260810-iucr-4dstem/blob/main/notebooks/try-it-yourself/09.strain_orientation_mapping_01.ipynb) and [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://githubtocolab.com/curiousbeams/workshop-20260810-iucr-4dstem/blob/main/notebooks/try-it-yourself/10.strain_orientation_mapping_02.ipynb) 
```
