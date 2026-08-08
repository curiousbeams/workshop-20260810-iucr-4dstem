# IUCr2026 | 4D-STEM workshop

Converged probe electron diffraction workshop — nanobeam 4D-STEM analysis and diffractive imaging phase retrieval — as taught by Georgios Varnavides and Stephanie Ribet at the 2026 congress of the International Union of Crystallography, 10 August 2026.

## Building the site

```bash
npm install -g mystmd   # or: pip install mystmd
myst start              # local dev server with live reload
myst build --html       # static site in _build/html
```

## Repository layout

- `*.md` — the lecture pages, ordered to follow the [agenda](./00b_agenda.md).
- `notebooks/*.ipynb` — the notebooks behind the site's interactive figures. Each cell tagged `#| label: app:<name>` is embedded into a page via `:::{figure} #app:<name>`.
- `notebooks/try-it-yourself/*.ipynb` — the hands-on Google Colab tutorials.
- `notebooks/data/` — datasets shipped to the Binder image for the interactive figures.
- `figures/` — static placeholder images shown before a live figure is computed.

## Outstanding work

- `notebooks/08.calibration-disk-detection.ipynb` and `notebooks/12.center-of-mass-imaging.ipynb` are new and have not been executed. Run them and commit the outputs, then render `figures/nanobeam_disk_detection_placeholder.png` and add `:placeholder:` back to the corresponding figure directives.
- The hands-on notebooks for the quantEM phase-retrieval sessions still need to be adapted from [`electronmicroscopy/quantem-tutorials`](https://github.com/electronmicroscopy/quantem-tutorials), including hosting Colab-sized copies of their datasets.
- There is no abTEM hands-on notebook for the multislice session yet.
- `myst.yml` has TODOs for Stephanie Ribet's ORCID and the IUCr funding statement.
