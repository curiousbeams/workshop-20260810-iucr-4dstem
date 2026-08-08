---
title: The Open-Source 4D-STEM Software Ecosystem
short_title: Software Ecosystem
label: software_ecosystem_page
numbering:
  heading_2: false
---

:::{admonition} Learning goals
:class: tip
- Know what `abTEM`, `py4DSTEM`, and `quantEM` each do best.
- Recognize the wider ecosystem and pick a sensible starting tool for a given task.
:::

A healthy ecosystem of open-source Python packages has grown up around 4D-STEM simulation and analysis.
They overlap in places, and that is a feature: you can move data between them, cross-check results, and pick the tool whose workflow fits your problem.
This workshop uses three of them, and this page is a quick tour of those plus their neighbors.

## abTEM

[abTEM](https://abtem.readthedocs.io/) [@doi:10.12688/openreseurope.13015.1] simulates TEM and STEM experiments from first principles: multislice and PRISM image simulation directly from atomic models, entirely in Python.
Simulation lets you generate test data with known ground truth, design experiments before you book microscope time (convergence angle, thickness, tilt sensitivity), and build the diffraction template libraries that orientation mapping matches against.
`abTEM` builds on the [atomic simulation environment](https://wiki.fysik.dtu.dk/ase/) (ASE) for structure handling, so any structure format ASE can read is a valid starting point.

We use `abTEM` in [](#multislice_algorithm_page) through [](#detectors_phonons_page).

## py4DSTEM

[py4DSTEM](https://github.com/py4dstem/py4DSTEM) [@doi:10.1017/S1431927621000477] is an open-source Python package for 4D-STEM analysis, developed at Lawrence Berkeley National Laboratory and by a broad community of contributors.
It covers the full experimental pipeline: file I/O across many vendor formats, calibration, virtual imaging, Bragg disk detection, strain mapping, automated crystal orientation mapping, and fluctuation microscopy.
If you have an experimental 4D-STEM dataset and want structural descriptors out of it, this is the usual starting point.

We use `py4DSTEM` in [](#calibration_disk_detection_page) through [](#orientation_mapping_page).

## quantEM

[quantEM](https://electronmicroscopy.github.io/quantem-docs/) [@10.5281/zenodo.18642593] is a newer open-source toolkit for quantitative electron microscopy built on PyTorch, so the same analysis code runs on CPUs and GPUs and integrates naturally with automatic differentiation and deep learning workflows.
It spans imaging, diffraction, ptychography, tomography, and spectroscopy, and is under active development ([code on GitHub](https://github.com/electronmicroscopy/quantem)).
Its phase-retrieval module is what we use in the afternoon: a single interface covers center-of-mass imaging, tilt-corrected bright field, single side-band, and iterative ptychography.

We use `quantEM` in [](#phase_problem_page) through [](#iterative_ptycho_page).

## The Wider Ecosystem

Several other packages solve overlapping problems, and are worth knowing about even though we do not use them today:

- [pyxem](https://pyxem.readthedocs.io/) is a 4D-STEM analysis library built on the [HyperSpy](https://hyperspy.org/) multi-dimensional data framework, particularly strong for scanning precession electron diffraction workflows and template-matching orientation mapping [@doi:10.1016/j.ultramic.2022.113517], with lazy out-of-core processing via Dask for datasets larger than memory.
- [Kelvin_STEM](https://github.com/maclariz/Kelvin_STEM) is a set of fast 4D-STEM tools for virtual imaging, digital dark field, and clustering on large datasets.
- [PtyRAD](https://ptyrad.readthedocs.io/) and [phaser](https://hexane360.github.io/phaser/dev/) are dedicated, GPU-accelerated ptychographic reconstruction packages.
- [prismatic](https://prism-em.com/) is a fast C++/CUDA implementation of multislice and PRISM simulation.

## Which Tool Should I Use?

| Task | Good starting points |
| --- | --- |
| Build an atomic model | ASE, pymatgen |
| Simulate STEM images and diffraction | abTEM, prismatic |
| Load / browse / calibrate experimental 4D data | py4DSTEM, pyxem, quantEM |
| Virtual imaging (BF/ADF/custom masks) | any of the above; Kelvin_STEM for speed on large data |
| Bragg disk detection | py4DSTEM, pyxem |
| Strain mapping | py4DSTEM, pyxem |
| Orientation and phase mapping | py4DSTEM, pyxem |
| Amorphous / PDF analysis | py4DSTEM, quantEM |
| Direct phase retrieval (CoM, tcBF, SSB) | quantEM, py4DSTEM |
| Iterative ptychography | quantEM, PtyRAD, phaser |

:::{note} All of these are community projects
Every package above is developed openly, and most are maintained by small teams of researchers. Issues and pull requests are genuinely welcome, and "this tutorial did not work for me" is a useful bug report.
:::
