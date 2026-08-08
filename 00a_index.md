---
title: 'IUCr2026 | 4D-STEM Workshop'
short_title: 4D-STEM Workshop
numbering:
  heading_2: false
---

+++ {"part": "abstract"} 

When a converged electron beam is scanned across a thin sample, the resulting diffraction patterns encode rich structural information.
Modern 4D-STEM experiments record a full diffraction pattern at every scan position, generating datasets that combine real- and reciprocal-space information for structure determination, strain mapping, orientation analysis, and atomic-resolution imaging.

This hands-on workshop introduces open-source Python tools for simulating and analyzing 4D-STEM data.
Through interactive, cloud-based Jupyter notebook tutorials, participants will explore dynamical scattering simulations and analyze experimental datasets.
Topics include STEM imaging principles, multislice simulations, calibration and Bragg disk detection, strain and orientation mapping, and diffractive imaging approaches such as center-of-mass imaging and ptychography.

Designed for crystallographers and microscopists at all levels, the workshop requires no prior electron microscopy experience, only an interest in integrating computational and experimental methods for structure determination.

+++

## Practical Information

This is a **one-day workshop** on **Monday, 10 August 2026**, at the 2026 congress of the International Union of Crystallography.
See the [agenda](./00b_agenda.md) for the detailed schedule.


:::{admonition} Requirements for all participants
:class: warning
- **Bring a laptop.** Any operating system works; tablets are not recommended for the hands-on sessions.
- **Internet access is required.** Conference WiFi can be slow, so open the tutorial links before each session starts.
- **A free Google account** is needed to run the hands-on tutorials in [Google Colab](https://colab.research.google.com/). No software installation is required, Colab runs entirely in the browser. Before the workshop, sign in at [colab.research.google.com](https://colab.research.google.com/) once to check that your account works.

See [](#getting_started_page) for a short walkthrough of both the interactive figures on this site and the Colab notebooks.
:::

## How to Use This Site

The workshop uses two complementary media:

- **This website** is the lecture material. Most figures are *live*: they run real simulation and analysis code, so we can vary a convergence angle or a dose and watch the physics respond. We walk through these together during each session.
- **Google Colab notebooks**, linked from a "Try it yourself!" box at the end of each module, are the hands-on material. These are complete, self-contained analysis workflows on real and simulated datasets.

Use the sidebar or the [agenda](./00b_agenda.md) to jump to a module.
The [software ecosystem](./00d_software-ecosystem.md) page introduces the open-source packages used throughout, namely `py4DSTEM`, `abTEM`, and `quantEM`.
