---
title: Interactive Python Computing
short_title: Getting Started
label: getting_started_page
numbering:
  heading_2: false
---

:::{admonition} Learning goals
:class: tip
- Enable live compute on this site and interact with a figure.
- Open a hands-on notebook in Google Colab, including switching on a GPU runtime.
- Know how to reproduce everything locally after the workshop.
:::

This workshop leans on two ways of running Python without installing anything: **live figures on this website**, and **hands-on notebooks on Google Colab**.
This short session gets both working on your laptop before we start the science.

## Live Figures on This Site

Most figures in these pages are not images.
They are the *outputs of Jupyter notebook cells*, and they can be brought to life on demand.
The notebooks that generate them are in the [Interactive Widgets](./notebooks/01.stem-measurements.ipynb) section of the sidebar, so you can always read the code behind a figure.

When you first load a page, each live figure shows a static placeholder image so that the page is useful even offline.
To make it interactive:

1. Click the **compute button** on each figure.
2. This spins up a cloud kernel via [Binder](https://mybinder.org), built from the `Dockerfile` and `requirements.in` in this repository, so the environment matches ours exactly.
3. Wait for the connection indicator to turn green, then press the run button on a figure. The placeholder is replaced by the live widget.

Once a figure is live, the sliders, toggles and click-to-position controls actually re-run the underlying simulation or reconstruction.
This is the whole point: rather than showing you a figure of what happens when the convergence angle doubles, we change it and look.

## Hands-On Notebooks on Google Colab

Interactive figures are deliberately small and fast so they stay responsive.
The complete workflows, the kind you would run on your own data, live in Google Colab notebooks.
At the end of each module you will find a box like this:

```{attention} Try it yourself!
Click the following badge to try a complete notebook on Colab:  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://githubtocolab.com/curiousbeams/workshop-20260810-iucr-4dstem/blob/main/notebooks/try-it-yourself)
```

Clicking the badge opens the notebook in Colab in a new tab.
Before the workshop starts, please:

1. Sign in at [colab.research.google.com](https://colab.research.google.com/) with a free Google account and confirm it works.
2. Open any one of the notebooks and run its first cell. This installs the required packages into the Colab session, which takes a minute or two and is the most common thing to get stuck on.

:::{tip} Turning on a GPU
The iterative ptychography notebooks are much faster on a GPU. In Colab, go to **Runtime → Change runtime type** and select a **T4 GPU** accelerator, then re-run the notebook from the top. The free tier is sufficient for everything we do today.
:::

Colab notebooks are yours: **File → Save a copy in Drive** keeps an editable version in your own Google Drive, with any changes and outputs you produce today.

## Running Everything Locally

Nothing here depends on the cloud.
To reproduce the site and its notebooks on your own machine:

```bash
git clone https://github.com/curiousbeams/workshop-20260810-iucr-4dstem.git
cd workshop-20260810-iucr-4dstem
pip install -r requirements.in     # or: uv pip install -r requirements.in
jupyter lab                        # to run the notebooks
```

To build and serve the website itself, you additionally need [MyST Markdown](https://mystmd.org):

```bash
npm install -g mystmd
myst start
```

The hands-on Colab notebooks download their datasets at runtime, so they work locally too, provided you have an internet connection the first time you run them.
