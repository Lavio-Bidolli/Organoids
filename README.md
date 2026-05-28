# Organoids — Phase Field Modeling of Cellular Interfaces

> A mean-field approach to simulate cell shape, adhesion, and division dynamics

This repository contains the code, results, and documentation for a **phase field model of cellular interfaces**, implemented as part of the *Physics of Complex Systems* course at Politecnico di Torino – Paris Cité.


---

## 📄 Repository Contents

| File / Folder | Description |
|---|---|
| `A phase field model for cellular interfaces.pdf` | **Full scientific report** — model derivation, numerical methods, results, conclusions |
| `organoids_simulations.ipynb` | Jupyter notebook with all numerical simulations and ANIMATIONS |
| `presentation_with_selected_frames/` | **Slide deck** — visual overview of the project and videos of selected simulations|
| `images/` | Figures and output plots from the simulations |

> **Quick overview?** → Open the **slide deck** in `presentation_with_selected_frames/`  
> **Full technical depth?** → Read the **PDF report**

---

## 🧬 Project Overview

Organoids are a thriving field at the intersection of nonlinear physics, biology, and applied medicine. This project presents a **phase field model** for the description of cellular membrane shape, implemented from scratch in Python and based on the work of Makiko Nonomura ([PLoS ONE, 2012](https://doi.org/10.1371/journal.pone.0033501)).

The model represents each cell's interface as a **continuous, sigmoid-like scalar field** `u(x, y, t)`:
- `u = 1` inside the cell
- `u = 0` outside
- a smooth, diffuse transition in between (the "membrane")

The time evolution of `u` is governed by a **PDE derived by minimizing a free-energy functional**, which encodes competing physical mechanisms: surface tension, volume conservation, and cell–cell adhesion.

---

## 🔬 Key Results (from the PDF)

### 1. Cell–Cell Interaction at Different Adhesion Strengths

Three adhesion regimes were explored by varying the parameter `η`:

- **η = 0** — no adhesion: cells remain perfectly circular, no overlap
- **η = 0.003** — weak adhesion: a slight elliptical deformation at the contact region, small "neck" visible in the interface profile
- **η = 0.065** — strong adhesion: clear membrane overlap, the adhesion density `eη` peaks sharply at the contact zone

The profile of `u₁`, `u₂`, and the adhesion density `eη` along the axis connecting the two cell centers clearly shows how the membrane thickness and the overlap region scale with adhesion strength.

### 2. Cell Division — Three Parameter Regimes

Cell growth was modeled through a slow, linear increase of the target volume. Division is triggered when volume exceeds a threshold, splitting the phase field with a sigmoid mask. Three qualitatively distinct regimes were identified:

| Regime | Parameters | Observed Behavior |
|---|---|---|
| Surface tension dominates | High `γ`, low `η` | Round, well-separated cells; grape-like cluster with visible gaps; cells never fully occupy available volume |
| Adhesion dominates | High `η`, low `γ` | Jagged, elongated interfaces at contact zones; cells do not separate; prolonged simulations can diverge |
| Enhanced diffusion | High `D`, same `γ`/`η` | Cells spread and occupy more volume; less compact final structure; growth proceeds before adhesion effects become dominant |

These results are in good agreement with Nonomura's original work and demonstrate that the **η/γ ratio** (adhesion-to-surface-tension) is the key control parameter governing emergent morphology.

### 3. Numerical Approach

An **explicit Euler scheme** was adopted for time integration, with a five-point stencil for the discrete Laplacian and periodic boundary conditions. An implicit Euler approach (Newton's method on the nonlinear system) was tested but discarded: it added complexity without improving accuracy or stability.

Volume is tracked at each step via a Riemann sum. The sigmoid mask used for division preserves total volume by construction.

---

## 🎬 Simulations

### Cell–Cell Adhesion

| Interaction between two cells | 
|:---:|:---:|
| ![Two-cells-interaction](presentation_with_selected_frames/GIF/two_cells_noses.gif) | ![Strong adhesion](presentation_with_selected_frames/GIF/two_cells_noses.gif) |

| Weak adhesion (η = 0.003) | Strong adhesion (η = 0.065) |
|:---:|:---:|
| ![Weak adhesion](presentation_with_selected_frames/GIF/low-adhesion.gif) | ![Strong adhesion](presentation_with_selected_frames/GIF/low-adhesion.gif) |

### Cell Division

| Surface tension dominates | Adhesion dominates | Enhanced diffusion |
|:---:|:---:|:---:|
| ![Surface tension](presentation_with_selected_frames/GIF/high-adhesion.gif) | ![Adhesion](presentation_with_selected_frames/GIF/high-adhesion.gif) | ![Diffusion](presentation_with_selected_frames/GIF/nome_file_5.gif) |

---

## ⚙️ The Model at a Glance

The total energy functional for two cells is:

$$E[u] = E_{\text{cell}}[u] + E_{\text{interactions}}[u]$$

where `E_cell` contains:
- a **Landau–Ginzburg** (double-well) potential enforcing binary cell identity
- a **gradient penalty** penalizing sharp interface variations (surface energy)
- a **volume constraint** (spring-like term, parameter `α`)

and `E_interactions` contains:
- a **repulsive** excluded-volume term (parameter `β`)
- an **attractive** adhesion term between membranes (parameter `η`)
- an additional **surface tension** term on the boundary (parameter `γ`)

The dynamics follow: `τ ∂_t u = −δE/δu`, i.e. gradient descent on the free energy.

---

## 🔭 Extensions and Open Questions

The conclusions of the paper identify several natural next steps:

- Introduction of **heterogeneous cell types** (different parameter sets per cell)
- Explicit **cell–substrate adhesion**
- **Chemotactic responses** under volume and stochastic constraints
- **Time-dependent volume constraints** or growth laws coupling phase-field dynamics to biological growth models
- Extension from 2D to **3D**, and scaling to many cells via auxiliary interface functions

---

## 📚 Reference

Makiko Nonomura, *"Study on Multicellular Systems Using a Phase Field Model"*, PLoS ONE 7.4 (2012).  
DOI: [10.1371/journal.pone.0033501](https://doi.org/10.1371/journal.pone.0033501)

---

## 👤 Author

**Flavio Bidolli** — Physics of Complex Systems, Politecnico di Torino – Paris Cité  
[GitHub](https://github.com/Lavio-Bidolli)
