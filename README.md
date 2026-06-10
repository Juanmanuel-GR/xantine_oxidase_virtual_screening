#  Integrated Virtual Screening & Computational Drug Discovery Workflow for Xanthine Oxidase Inhibitors

![Python](https://img.shields.io/badge/python-3.12-blue.svg)
![RDKit](https://img.shields.io/badge/RDKit-2025.09-orange.svg)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-latest-green.svg)
![AutoDock Vina](https://img.shields.io/badge/AutoDock%20Vina-1.2.5-red.svg)

**Author:** Juan Manuel Gallego Rabadán
---
## Scientific Background & Project Rationale

Gout is an intensely painful form of inflammatory arthritis caused by hyperuricemia—the chronic supersaturation of uric acid leading to the precipitation of monosodium urate (MSU) crystals within joints and soft tissues. 

The enzyme **Xanthine Oxidase (XO)** plays a critical gatekeeper role in human purine catabolism, catalyzing the sequential oxidation of hypoxanthine to xanthine, and xanthine to uric acid. Inhibiting XO represents the benchmark clinical strategy for managing gout (as seen with drugs like Allopurinol and Febuxostat).

This repository implements a **multi-tiered In-Silico Virtual Screening (VS) pipeline** to discover novel, drug-like, non-purine XO inhibitors from a vast initial chemical space. The framework transitions systematically from computational data reduction toward high-confidence molecular interactions.

---

##  Overall Workflow Architecture

The screening cascade applies sequential filters to balance computational cost and predictive accuracy, narrowing down an initial library of **~50,000 ChEMBL compounds** into a handful of elite lead candidates.

---

##  Detailed Technical Phase Breakdown

###  Phase 1: Library Curation & Substructure Filtering (`1_library_screening.ipynb`)
The objective of this stage is to eliminate undesirable compounds before committing heavy computing resources to 3D simulation.
* **Lipinski’s Rule of Five (Ro5) Optimization:** Compounds were evaluated via **RDKit** to enforce optimal pharmacokinetic profiles. Filters applied: Molecular Weight ($300 \le \text{MW} \le 500 \text{ Da}$), $\text{LogP} < 5$, Hydrogen Bond Donors $\le 5$, and Acceptors $\le 10$.
* **PAINS (Pan-Assay Interference Compounds) Exclusion:** Implemented a substructure alert system using Smarts-based structural keys to filter out reactive, promiscuous, or fluorescent artifacts that cause false-positive assays.
* **Data Engineering Outcome:** Successfully pruned the initial noisy library down to **46,353 highly viable, lead-like structures**.

###  Phase 2: Predictive QSAR Modeling & Hit Selection (`2_qsar_modeling.ipynb`)
Using bioactivity data extracted from ChEMBL for known Xanthine Oxidase modulators, a Quantitative Structure-Activity Relationship (QSAR) model was built.
* **Molecular Featurization:** 2D chemical topologies were mapped into numerical matrices utilizing **Morgan Fingerprints** (Circular fingerprints with a radius of 2, vectorized into fixed 2048-bit bitvectors).
* **Machine Learning Inferences:** Trained a robust **Random Forest Classifier** via Scikit-Learn. The model evaluates non-linear compound relationships to compute an active probability score (`Prob_QSAR`).
* **Output:** Candidates were prioritized and ranked based on their probability of belonging to the biological active class.

###  Phase 3: 3D Conformer Ensembles & Pharmacophore Alignment (`3_pharmacophore_search.ipynb`)
Molecules do not interact as flat 2D lines; their 3D spatial orientation is paramount to binding pocket compatibility.
* **Stochastic Conformer Generation:** For the top QSAR candidates, RDKit's distance geometry algorithms generated multi-conformer ensembles to sample accessible low-energy spatial shapes.
* **Open3DAlign (O3A) Core Integration:** Extracted structural alignments by maximizing steric and electronic overlaps against **Febuxostat** as a rigid 3D reference.
* **Pharmacophore Mapping:** Evaluated specific geometric distances between vital chemical features: Hydrogen Bond Donors, Acceptors, Aromatic Rings, and Hydrophobic Centers to pinpoint critical structural overlaps.

###  Phase 4: Target Docking & Bio-affinity Validation (`docs/final_report.pdf`)
The ultimate validation shifts from ligand-based features to receptor-based atomic physics.
* **Target Preparation:** Handled the crystal structure of **Xanthine Oxidase (PDB: 1N5X)**, isolating the protein chain, removing solvent molecules, and parameterizing the catalytic site coordinates.
* **Molecular Docking Simulation:** Utilized **AutoDock Vina** to compute binding free energies ($\Delta G$ in kcal/mol), simulating flexible ligand configurations inside the static enzymatic binding pocket.
* **Analysis:** Correlated low docking scores (highest binding affinity) with previous QSAR probabilities to establish a multi-tiered consensus ranking.

---

## Software Environment & Dependency Setup

The pipeline requires a standard Chemoinformatics setup. It is recommended to run this inside a Conda environment:

```bash
# Core installations via pip
pip install rdkit scikit-learn pandas numpy matplotlib

