# Discrete Cardiac Proarrhythmia Model
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

## Overview

This repository contains the simulation parameters and monolayer geometries used in the study:

> **Sinitsyna A.P., Slotvitsky M.M., Tsvelaya V.A.**
> *"Discrete Cellular Modeling Reveals APD-Dependent Thresholds and Fibrosis-Anchored Reentry in the Proarrhythmic Risk Assessment of Multi-Channel Blocking Anti-Cancer Drugs"*

The simulations were performed using the [openCARP](https://opencarp.org/) cardiac electrophysiology simulation environment. The discrete cellular model represents a 1×1 cm virtual monolayer of human induced pluripotent stem cell-derived cardiomyocytes (hiPSC-CMs), with each computational element corresponding to an individual cell (50×50 µm).

## Repository Structure

```
discrete-cardiac-proarrhythmia-model/
├── README.md
├── LICENSE
├── meshes/
│   ├── acute_obstacle/
│   │   ├── monolayer_acute_obstacle.elem
│   │   ├── monolayer_acute_obstacle.pts
│   │   └── monolayer_acute_obstacle.lon
│   └── diffuse_fibrosis/
│       ├── monolayer_diffuse_fibrosis.elem
│       ├── monolayer_diffuse_fibrosis.pts
│       └── monolayer_diffuse_fibrosis.lon
├── parameters/
│   ├── control/
│   │   └── parameter_acute_obstacle.par
│   │   └── parameter_fibrosis_ramp.par
│   ├── mitoxantrone/
│   │   └── parameter_acute_obstacle.par
│   │   └── parameter_fibrosis_ramp.par
│   ├── dasatinib/
│   │   └── parameter_acute_obstacle.par
│   │   └── parameter_fibrosis_ramp.par
│   ├── sunitinib/
│   │   └── parameter_acute_obstacle.par
│   │   └── parameter_fibrosis_ramp.par
│   ├── nilotinib/
│   │   └── parameter_acute_obstacle.par
│   │   └── parameter_fibrosis_ramp.par
│   └── erythromycin/
│   │   └── parameter_acute_obstacle.par
│   │   └── parameter_fibrosis_ramp.par

```

## Description of Contents

### Meshes (`/meshes/`)

Two monolayer architectures are provided in openCARP-compatible mesh format (`.elem`, `.pts`, `.lon`):

- **`acute_obstacle/`** — A 200×200 px discrete cellular monolayer (1×1 cm) containing a thin non-conducting stripe (100 µm width, conductance 0.0001 mS/cm) extending from the midpoint of one edge to the center. Used for the arrhythmogenicity corridor test.

- **`diffuse_fibrosis/`** — A 200×200 px monolayer with a circular zone of diffuse fibrosis (radius 0.5 mm) at the center. The fibrotic zone contains randomly distributed fibroblast elements (30% fraction, conductance 0.0001 mS/cm) and cardiomyocytes with 30% reduced intercellular conductance.

Both meshes include 10% randomly selected elements with 10-fold reduced intracellular and intercellular conductance to reproduce baseline tissue heterogeneity.

### Simulation Parameters (`/parameters/`)

Each subfolder contains the openCARP parameter file (`.par`) for one of the six experimental conditions:

| Condition | Concentration (µM) | G_Na block | G_Kr block | G_CaL block |
|-----------|------|---------------------|------------|------------|-------------|
| Control | 0 | 0 | 0 | 0 |
| Mitoxantrone | 7.87 | 0.014 | 0.069 | 0.34 |
| Dasatinib | 5.12 | 0.053 | 0.27 | 0.11 |
| Sunitinib | 0.47 | 0.013 | 0.28 | 0.0095 |
| Nilotinib | 0.40 | 0.00054 | 0.29 | 0.022 |
| Erythromycin | 10.0 | 0 | 0.33 | 0 |

Ion channel blockade was implemented using the pore-block model based on IC₅₀ and Hill coefficient data from [Passini et al., 2017](https://doi.org/10.3389/fphys.2017.00668). Maximum conductances (G_Na, G_Kr, G_CaL) in the Kernik hiPSC-CM model were scaled by the factor (1 − f_block).

### Scripts

- Run the pacing frequency sweep on the acute obstacle monolayer (step: 5 ms, duration: 10 s per frequency) using
```
openCARP +F parameter_acute.par 
```
- Runs the ramp pacing protocol on the diffuse fibrosis monolayer (2-5 Hz over 20 s).
```
openCARP +F parameter_fibrosis_ramp.par 
```

## Requirements

- [openCARP](https://opencarp.org/) (version ≥ 15.0 recommended)
- Python 3.8+ (for post-processing with [Optimap](https://github.com/optimap/optimap))
- [Meshalyzer](https://carpentry.medunigraz.at/meshalyzer/) (for visualization)

## Quick Start

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/discrete-cardiac-proarrhythmia-model.git
cd discrete-cardiac-proarrhythmia-model

# Run the arrhythmogenicity test for dasatinib
cd parameters/dasatinib
openCARP +F parameter_acute.par 

# Run the fibrosis ramp protocol for erythromycin
cd parameters/dasatinib
openCARP +F parameter_fibrosis_ramp.par 
```

## Electrophysiological Model

The single-cell electrophysiology is described by the **Kernik model** adapted for hiPSC-CMs:

> Kernik D.C., Morotti S., Wu H., et al. (2019). A computational model of induced pluripotent stem-cell derived cardiomyocytes incorporating experimental variability from multiple data sources. *The Journal of Physiology*, 597(17), 4533–4564. DOI: [10.1113/JP278078](https://doi.org/10.1113/JP278078)

Key model parameters:
- Time step: 0.01 ms
- Integration scheme: Rush-Larsen
- Diffusion solver: Implicit Crank-Nicolson
- Intracellular conductance: 2.0 mS/cm (standard), 0.2 mS/cm (heterogeneous elements)
- Extracellular conductance: 0.8 mS/cm (standard), 0.08 mS/cm (heterogeneous elements)

## License

This repository is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

## Contact

- **Corresponding author:** A.P. Sinitsyna — sinitsyna.ap@phystech.edu
## Acknowledgments

This work was conducted at the Moscow Center for Advanced Studies and the Moscow Regional Research and Clinical Institute named after M.F. Vladimirsky.
