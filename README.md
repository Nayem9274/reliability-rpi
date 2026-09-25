# Reliability Profiling for Protein-Cold RNA–Protein Interaction Prediction

This repository contains notebook-based code for reproducing the computational
experiments in **When Should RNA–Protein Interaction Predictions Be Trusted?
Reliability Profiling and Failure Detection under Unseen-Protein Shift**.


## Repository layout

```text
.
├── 01_primary_benchmark.ipynb
├── 02_posthoc_analysis.ipynb
├── 03_threshold_aligned_reliability.ipynb
├── requirements.txt                                      
└── results/                     
```

### 01_primary_benchmark.ipynb
Runs the three-dataset, four-model protein-cold benchmark, performs split checks,
saves validation/test predictions and metrics, and creates the compact artifact
used by the downstream notebooks.

### 02_posthoc_analysis.ipynb
Uses the saved prediction artifact without retraining the RPI models. It contains
protein-balanced evaluation and additional post-hoc reliability analyses.

### 03_threshold_aligned_reliability.ipynb
Recomputes the final confidence/certainty-based reliability analyses using the
validation-selected operating threshold. Use these outputs for the final HC90,
failure-risk, secondary-leakage, LODO, and selective-prediction results when they
overlap with older post-hoc outputs.

## Data

The benchmark datasets and pretrained RNA-FM / ProtT5 embeddings are the same
resources distributed with ZHMolGraph:

- Zenodo: https://zenodo.org/records/14747845

Set the data path in Notebook 01 before running.

## Recommended run order

1. Open `01_primary_benchmark.ipynb`.
2. Set the dataset/input path and the desired output path.
3. Run all setup and split-preflight cells.
4. Run the required model/dataset/seed shards. The notebook can reuse completed
   folds from its output directory.
5. Export `dependable_rpi_journal_v1_ARTIFACTS.zip`.
6. Add that artifact as an input to the environment used for Notebook 02 and run
   `02_posthoc_analysis.ipynb`.
7. Run `03_threshold_aligned_reliability.ipynb` on the same primary artifact.
8. Use Notebook 03 outputs as the final threshold-aligned reliability results.

The internal artifact name `dependable_rpi_journal_v1_ARTIFACTS.zip` is retained
for compatibility with the current notebooks.

## Environment

The notebooks were designed for a Python/PyTorch environment with GPU support
for primary model training. Post-hoc analyses can run on CPU.

## Reproducing figures and tables without retraining

The primary notebook stores compact predictions and figure-source tables, while
the downstream notebooks operate on the saved prediction artifact. Therefore,
users interested only in the reliability analyses can start from the released
artifact/CSV outputs instead of retraining all primary models.

## Notes

- Run notebook cells sequentially unless a section explicitly says it is optional.
- Do not mix outputs generated under different split definitions.
- The failure detector must be evaluated with protein grouping for the
  deployment-matched unseen-protein setting.
- Full benchmark protein degree is retrospective diagnostic information and is
  not an input to the deployable failure detector.
