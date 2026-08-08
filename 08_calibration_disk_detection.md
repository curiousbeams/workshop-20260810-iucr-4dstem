---
title: Calibration and Bragg Disk Detection
short_title: Calibration & Disk Detection
label: calibration_disk_detection_page
numbering:
  enumerator: 8.%s
---

:::{admonition} Learning goals
:class: tip
- Recognize the nanobeam diffraction geometry and how it differs from the atomic-resolution geometry used so far.
- Detect Bragg disks with a probe template, and understand what each detection hyperparameter does.
- Apply the calibration chain that turns detector pixels into physical units.
:::

Everything up to this point has been about *simulating* what a converged probe does to an electron wave.
We now switch to the inverse problem on experimental data, and to a different corner of the experimental parameter space.

## The Nanobeam Geometry

The phase-retrieval techniques we explore later in the day are aimed at atomic-resolution imaging, which requires an aberration-corrected probe with a large convergence angle.
But a great deal of the physics we care about, the sample strain and the local crystal orientation, does not require atomic resolution.
These structural descriptors are accessible at nanometer resolution by using a *small* convergence angle, a geometry we call **nanobeam electron diffraction** (NBED).

The trade-off is the one we have already seen in [](#electron_wavefunctions_page): a small convergence semi-angle $\alpha$ means a large, weakly-focused real-space probe (typically 1 to 5 nm) but *sharp, non-overlapping diffraction disks*.
Large $\alpha$ gives the opposite: an atomic-sized probe and broad, overlapping disks that interfere with each other.
Overlapping disks carry the phase information that ptychography exploits, but they make it very hard to say precisely *where* a reflection is.
Nanobeam diffraction gives up spatial resolution to buy precision in reciprocal space, which is exactly the right trade when what you want to measure is a lattice spacing or a zone-axis orientation.

The rest of this session is about the measurement that both strain mapping and orientation mapping are built on: finding where the disks are, in physical units.

## Detecting Bragg Disks

Because the beam is converged, each reflection appears as a *disk*, not a spot, and its shape is (to first order) the shape of the aperture.
This is what makes detection tractable: we already know the shape we are looking for, so we can look for it by cross-correlation.

1. **Record a vacuum probe.** Move to a hole in the sample and record the unscattered probe. This gives you the true disk shape, and measures the convergence angle at the same time.
2. **Build a template.** The raw vacuum probe is turned into a cross-correlation kernel, typically by shaping its edge with a sigmoid. The soft edge is what gives sub-pixel precision rather than a plateau of nearly-equal correlation scores.
3. **Cross-correlate and find maxima.** Correlate the template against every diffraction pattern and locate the correlation maxima with subpixel precision {cite:p}`10.1016/j.ultramic.2016.12.021`.

[](#fig_disk_detection) lets you vary the detection hyperparameters on a nanobeam dataset and see which peaks survive.

:::{figure} #app:nanobeam_disk_detection
:name: fig_disk_detection
:placeholder: ./figures/bragg_disk_detection_placeholder.png
Bragg disk detection on a simulated nanobeam dataset, a gold nanoparticle illuminated at a 5 mrad convergence semi-angle. Move the cursor over the left panel to place the probe; the middle panel shows the resulting diffraction pattern (or the probe template), and the right panel the cross-correlogram with detected peaks circled. Vary the hyperparameters and watch peaks appear and disappear.
:::

The three hyperparameters that matter most:

- **Correlation power** interpolates between cross-correlation (power 1, robust to noise, broad peaks) and phase correlation (power 0, sharp peaks, noise-sensitive). Intermediate values are usually best.
- **Minimum peak intensity** is the correlation threshold below which a maximum is discarded. Too low and you index noise; too high and you lose the weak high-order reflections that constrain the lattice fit most tightly.
- **Minimum peak spacing** prevents a single disk from being detected several times, which matters when the correlogram peaks are broad.

There is also a choice of **subpixel refinement** algorithm: parabolic fitting (`'poly'`) is fast and fine for exploratory work, while upsampled cross-correlation (`'multicorr'`) is what you should use for high-precision strain mapping.

:::{important} Tune on a handful of patterns first
Detection over a full scan is millions of cross-correlations. Always tune the hyperparameters on a few representative patterns, including one from a thick region and one from near the sample edge, before launching the full run.
:::

## Loading and Browsing Data

In `py4DSTEM`, a 4D-STEM dataset is a `DataCube` whose four dimensions are conventionally ordered `(Rx, Ry, Qx, Qy)`: two real-space scan coordinates, then two reciprocal-space detector coordinates.
Most vendor and community formats are read directly (Gatan `.dm4`, Thermo Fisher Velox `.emd`, DECTRIS, Merlin/Medipix `.mib`, EMPAD raw, EMD 1.0), and any array can be wrapped:

```python
import py4DSTEM
datacube = py4DSTEM.import_file("experiment.dm4")

# or, from a raw array:
import numpy as np
data = np.load("scan.npz")["arr_0"]     # shape (Rx, Ry, Qx, Qy)
datacube = py4DSTEM.DataCube(data=data)
```

Analysis products (mean patterns, virtual images, Bragg peaks, calibrations) are stored alongside the data in a tree that can be saved to and reloaded from HDF5.
This matters in practice: disk detection over millions of patterns is expensive, and being able to checkpoint and share the resulting Bragg vectors saves hours.

The first thing to do with any new dataset is *look at it*.
Scrubbing through diffraction patterns as a function of probe position builds intuition about what is actually in the data, where the vacuum is, which regions are crystalline, how much the pattern changes between neighboring positions, and immediately reveals problems like detector saturation or beam damage.
The mean and maximum diffraction patterns over all probe positions give a compact overview of everything the detector saw.

:::{figure} ./figures/dataset-overview.jpg
:name: fig_dataset_overview
:width: 100%
Standard first-look products for a 4D-STEM dataset: the vacuum probe, the maximum diffraction pattern over all positions, the Bragg vector map, the simultaneously recorded HAADF image, and the radial histogram of detected Bragg peaks.
:::

## The Calibration Chain

A useful mental model: every measurement in this session is a *position* in the diffraction pattern, so every distortion of the diffraction pattern propagates directly into the physics.
The standard calibration chain is:

1. **Pixel sizes.** The real-space step size comes from the scan settings; the reciprocal-space pixel size comes from the camera length, or better, is measured from a known reference. Always sanity-check the scale bars on your virtual images afterwards.
2. **Origin and descan correction.** The center of the diffraction pattern shifts as the beam scans, due to imperfect descan alignment. A classic signature is a central disk that looks like a rounded rectangle in the *maximum* diffraction pattern: the circular center disk convolved with the rectangle traced out by the descan across the scan. We measure the origin at every probe position and fit a smooth low-order surface to it, with robust fitting to suppress outliers.
3. **Elliptical distortion.** Projector lens distortions and detector tilt stretch the diffraction pattern into a slight ellipse. This can be measured from the amorphous halo of a carbon support film or from a standard sample, then either corrected or handled directly by working in polar-elliptical coordinates.

:::{figure} ./figures/polar-elliptical.jpg
:name: fig_polar_elliptical
:width: 90%
Elliptical distortion in practice: resampling a ring pattern in plain polar coordinates leaves the rings wavy along the angular direction. Including the fitted ellipticity straightens them, which sharpens every radial measurement downstream.
:::

4. **Real-space to reciprocal-space rotation.** The scan direction and the detector axes are rotated relative to each other, since scan coils and camera each have their own orientation, and data can additionally be transposed on read-in. This rotation must be measured, for example with a center-of-mass analysis of the center beam (which we return to in [](#dpc_page)), where the correct rotation produces clean dipole contrast along $x$ in $\mathrm{CoM}_x$ and along $y$ in $\mathrm{CoM}_y$. Getting this wrong rotates your strain tensor and your orientation maps.
5. **Reciprocal pixel size against a known structure.** For quantitative work, the reciprocal pixel size can be refined by comparing measured Bragg peak positions against structure factors calculated from a known reference crystal, for example from a CIF file. This is the point where the simulation machinery from this morning re-enters the analysis workflow.

:::{tip} Record a vacuum probe in every session
It provides the template for disk detection, measures the convergence angle, and captures the true probe shape. If you forget, a synthetic probe or a template extracted from a thin sample region can substitute, but it is never as good.
:::

```{attention} Try it yourself!
Load, browse, and virtually image a simulated polycrystalline gold dataset, then run your first disk detection:  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://githubtocolab.com/curiousbeams/workshop-20260810-iucr-4dstem/blob/main/notebooks/try-it-yourself/py4dstem_01_basics_disk_detection.ipynb) **py4DSTEM basics: 4D-STEM data and Bragg disk detection**
```
