---
title: STEM Measurements
label: stem_measurements_page
numbering:
  enumerator: 1.%s
---

:::{admonition} Learning goals
:class: tip
- Distinguish the imaging and diffraction acquisition modes of a STEM instrument.
- Explain why STEM is inherently a diffraction-mode technique, and what "diffractive imaging" then means.
- Describe what a 4D-STEM dataset is: a full diffraction pattern recorded at every scan position.
- State the common objective of BF-STEM, DF-STEM, DPC and ptychography: estimating the sample potential from intensities alone.
:::

## Imaging and Diffraction Modes

Scanning transmission electron microscopy (STEM) enables the characterization of specimens from the micron scale down to the atomic scale, making it an indispensable characterization tool for any materials scientist [@https://doi.org/10.1007/978-0-387-76501-3].
STEM instruments operate in two complementary acquisition modalities: **imaging mode**, which produces a magnified real-space image of the specimen, and **diffraction mode**, which records the angular distribution of scattered electrons in reciprocal space [@https://doi.org/10.1557/s43577-026-01100-3].


:::{figure} ./figures/schematic_light.png
:class: dark:hidden
:name: fig_stem_modes_light
Three ways of illuminating a specimen and collecting the scattered electrons.
**Left:** conventional TEM imaging mode, where parallel illumination passes through the specimen and post-specimen optics form a magnified real-space image on the detector. 
**Middle:** diffraction mode with a near-parallel beam, where discrete diffracted beams land as sharp, well-separated spots.
**Right:** a converged STEM probe, which contains a range of incident wave vectors and therefore spreads each reflection into a disk on the detector.
:::


:::{figure} ./figures/schematic_dark.png
:class: hidden dark:block
:name: fig_stem_modes_dark
:enumerated: false
Three ways of illuminating a specimen and collecting the scattered electrons.
**Left:** conventional TEM imaging mode, where parallel illumination passes through the specimen and post-specimen optics form a magnified real-space image on the detector. 
**Middle:** diffraction mode with a near-parallel beam, where discrete diffracted beams land as sharp, well-separated spots.
**Right:** a converged STEM probe, which contains a range of incident wave vectors and therefore spreads each reflection into a disk on the detector.
:::


Traditional parallel-illumination TEM imaging, the left panel of [](#fig_stem_modes_light), is widely used across disciplines, from high-resolution studies of frozen-hydrated biomolecules to lattice-resolved and defect imaging of materials.

STEM, in contrast, employs a converged electron probe containing a broad range of incident wave vectors, the middle and right panels of [](#fig_stem_modes_light).
When this probe interacts with the specimen, it produces a diffraction pattern that encodes the local scattering.
In this sense **STEM is inherently a diffraction-mode technique**, although real-space images can be obtained by processing the resulting position-resolved diffraction patterns, a process we will refer to as **diffractive imaging**.
This approach routinely provides interpretable images of materials down to atomic resolution and forms the basis of modern crystallographic characterization.

Notice that the middle and right panels of [](#fig_stem_modes_light) differ only in the convergence angle of the illumination.
That single knob turns out to organize the whole day.
A small convergence angle keeps the diffracted disks sharp and separated and underpins the strain and orientation mapping of [](#calibration_disk_detection_page) through [](#orientation_mapping_page).
A large convergence angle overlaps the disks which makes the overlap regions *interfere*, encoding the phase information that the diffractive imaging methods of [](#phase_problem_page) through [](#iterative_ptycho_page) recover.

## A Typical 4D-STEM Measurement

When an accelerated beam of electrons passes through a thin sample, the electron wavefunction is scattered due to interactions with the sample potential. Before describing the mathematics we will use the interactive widget below to obtain an intuition for the physics.

:::{figure} #app:stem_measurements
:name: fig_stem_measurements
:placeholder: ./figures/stem_measurements_placeholder.png
Typical dataset collected during a STEM experiment using a probe-corrected microscope on a crystalline sample.
:::

In the left panel of [](#fig_stem_measurements), we are plotting the projected potential of a crystalline sample, specifically a thin film made up of 7 layers of gold viewed along the [111] zone-axis.
In the middle panel of [](#fig_stem_measurements) we are plotting the converged electron probe in real-space incident on the sample.
Note the probe is complex-valued and here we are encoding the phase of the electron wavefunction using the hue channel.
Finally, in the right panel of [](#fig_stem_measurements) we are plotting the diffraction-space intensity of the probe after it has interacted with the sample.
Note that the probe wavefunction is complex-valued, but we only plot its amplitude to highlight the limitation of current detectors in collecting the phase of the electron wavefunction.

The STEM techniques we will explore today such as bright-field (BF) STEM, dark-field (DF) STEM, differential phase contrast (DPC), or ptychography all share the same objective: namely to obtain robust estimates of the sample potential (left panel of [](#fig_stem_measurements)) using a set of diffraction intensities (right panel of [](#fig_stem_measurements)).
