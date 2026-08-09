# IUCr2026 | 4D-STEM workshop

Converged probe electron diffraction workshop — nanobeam 4D-STEM analysis and diffractive imaging phase retrieval — as taught by Georgios Varnavides and Stephanie Ribet at the 2026 congress of the International Union of Crystallography, 10 August 2026.

Published at <https://cbl.curve.space/articles/curious-beams-workshop-20260810-iucr-4dstem>.

## Building the site

```bash
npm install -g mystmd   # or: pip install mystmd
myst start              # local dev server with live reload
myst build --html       # static site in _build/html
```

Pushes are deployed to Curvenote by the workflows in `.github/workflows/`.

## Repository layout

- `*.md` — the lecture pages, ordered to follow the [agenda](./00b_agenda.md).
- `notebooks/*.ipynb` — the notebooks behind the site's interactive figures. Each cell tagged `#| label: app:<name>` is embedded into a page via `:::{figure} #app:<name>`.
- `notebooks/try-it-yourself/*.ipynb` — the hands-on Google Colab tutorials.
- `notebooks/data/` — datasets shipped to the Binder image for the interactive figures.
- `figures/` — static placeholder images shown before a live figure is computed.

## Hands-on notebooks

Each notebook downloads its own dataset from Google Drive with `gdown`, so participants need only a Google account.

| Notebook | Session |
| --- | --- |
| `py4dstem_01_basics_disk_detection` | Calibration and Bragg disk detection |
| `py4dstem_02_strain_LFP` | Strain mapping |
| `py4dstem_03_orientation_AuAgPd` | Orientation mapping |
| `py4dstem_04_phase_orientation_strain_Ti` | Orientation mapping (capstone) |
| `quantem_01_direct_ptychography_kernels` | Direct phase retrieval |
| `quantem_02_hyperparameter_optimization` | Direct phase retrieval |
| `quantem_03_ptycho_experimental_workflow` | Iterative phase retrieval (needs a T4 GPU runtime) |
