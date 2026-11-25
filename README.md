# Gastruloid-digital-twin-showcase

# In Silico Gastruloid Morphogenesis: Reaction–Diffusion & Automated Bioimage Inference

[![Open Project 1 In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/gastruloid-digital-twin-showcase/blob/main/01_wnt_gradient_pde_simulation.ipynb)
[![Open Project 2 In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/gastruloid-digital-twin-showcase/blob/main/02_morphometry_and_parameter_inference.ipynb)

A mechanistic computational biology showcase extending experimental microfluidic gastruloid research (originally performed at EPFL LSCB). Everything runs reproducibly in **Google Colab** using standard scientific Python (`NumPy`, `SciPy`, `scikit-image`, `Matplotlib`).

---

## Biological Motivation

Mouse embryonic stem cell (mESC) gastruloids break rotational symmetry and establish an anteroposterior (AP) axis driven by localized posterior Wnt signaling. In microfluidic gradient experiments, two key physical-biological questions emerged:
1. **Microfluidic Gradient Washout:** Why does a spatial CHIR99021 gradient fail to lock sustained directional AP elongation when the gastruloid floats freely inside a microfluidic trapping chamber?
2. **Biphasic Wnt Dosing:** Why do moderate, transient CHIR99021 pulses (18–24 h, 0.5x–1x) promote robust axial elongation, whereas higher concentrations (3x–5x) or prolonged pulses (48 h) arrest or shorten the aggregate?

---

## Repository Structure

```text
gastruloid-digital-twin-showcase/
├── 01_wnt_gradient_pde_simulation.ipynb        # PDE transport + stochastic rotational diffusion
├── 02_morphometry_and_parameter_inference.ipynb # Medial-axis morphometry + ODE calibration
├── figures/
│   ├── project1_results.png
│   ├── project2_morphometry.png
│   └── project2_phase_diagram.png
└── README.md
