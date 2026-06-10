\---



\##  Repository \& Code Structure



The technical implementation is split into modular Jupyter Notebooks, tracking the pipeline dependencies stage by stage:



\###  1. Library Preparation \& Filtering (`1\_library\_screening.ipynb`)

\* \*\*Objective:\*\* Ingest the initial raw compound library (\~50,000 substances) and apply strict physicochemical filters.

\* \*\*Methodology:\*\* Used \*\*RDKit\*\* to evaluate \*\*Lipinski's Rule of Five\*\* constraints (Molecular Weight 300–500 Da, LogP < 5, etc.) and applied a \*\*PAINS\*\* (Pan-Assay Interference Compounds) substructure filter.

\* \*\*Output:\*\* Curated a clean, lead-like screening dataset of 46,353 interference-free compounds.



\###  2. QSAR Modeling \& Active Selection (`2\_qsar\_modeling.ipynb`)

\* \*\*Objective:\*\* Build a predictive Machine Learning model to identify high-probability active hits based on ChEMBL bioactivity data.

\* \*\*Methodology:\*\* Featurized molecular structures using 2D topological \*\*Morgan Fingerprints\*\* (Radius = 2, 2048 bits). Trained and cross-validated a \*\*Random Forest Classifier\*\* via Scikit-Learn to compute class probabilities (`Prob\_QSAR`).

\* \*\*Output:\*\* Ranked and exported top candidate compounds showing the highest predicted likelihood of activity.



\###  3. Pharmacophore \& Ligand Alignment (`3\_pharmacophore\_search.ipynb`)

\* \*\*Objective:\*\* Assess 3D structural compatibility and spatial feature overlaps against a known reference.

\* \*\*Methodology:\*\* Generated multi-conformer ensembles using distance geometry algorithms. Performed 3D alignments using Open3DAlign (`O3A`) against \*\*Febuxostat\*\* (a powerful non-purine XO inhibitor) to analyze vital pharmacophoric features (Donors, Acceptors, Aromatics, and Hydrophobes).



\### 📝 4. Final Scientific Report (`docs/final\_report.pdf`)

\* A full comprehensive academic paper describing the theoretical background of gout pathophysiology, machine learning metric evaluations, and final structure-based validation utilizing \*\*AutoDock Vina\*\* (Target PDB entry: \*\*1N5X\*\*).



\---



\##  Tech Stack \& Prerequisites



Make sure you have the following frameworks installed in your environment:



\* \*\*Python 3.12+\*\*

\* \*\*RDKit\*\* (In-silico molecular manipulation)

\* \*\*Scikit-Learn\*\* (Statistical Machine Learning models)

\* \*\*Py3Dmol / NGLview\*\* (Optional - for conformer rendering)

\* \*\*Pandas \& NumPy\*\* (Data wrangling matrix structures)



```bash

pip install rdkit scikit-learn pandas numpy

