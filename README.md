# Medical Image Risk Assessment

Multimodal type 2 diabetes (T2D) risk assessment, early-detection analysis, and genetic analyses.

## Overview

This repository provides executable research workflows integrating medical images, clinical characteristics, and polygenic risk scores (PRS) for T2D risk assessment.

Three major research components are included:

1. **Multimodal prediction:** DenseNet121 feature extraction, abdominal graph neural network (GNN) aggregation, feature integration, and prediction using XGBoost or TabPFN.
2. **Early detection:** Longitudinal analysis using Cox proportional hazards models.
3. **Genetic analysis:** PRS construction, GWAS, Mendelian randomization (MR), and effective independent test estimation.

The repository includes demonstration data, Jupyter notebooks, trained model artifacts, and example outputs.

Demonstration results must not be interpreted as estimates obtained from the original research cohort.

## Repository structure

```text
GitHub/
├── Demo_data/
│   ├── 1-1.Sample_information/
│   ├── 1-2.Sample_image/
│   └── 1-3.Sample_genotype/
├── External_data/
├── Outputs/
├── model_weights/
│   ├── Demo-trained_models/
│   └── Study-trained_models/
├── docs/
├── environments/
├── local_data/
├── 0.Read_demo_data.ipynb
├── 1.Step1_PRS_Construction.ipynb
├── 2-1.Step2_DenseNet_ABD.ipynb
├── 2-2.Step2_DenseNet_BMD_left_hip.ipynb
├── 2-3.Step2_DenseNet_BMD_right_hip.ipynb
├── 2-4.Step2_DenseNet_BMD_spine.ipynb
├── 3.Step3_GNN.ipynb
├── 4.Step4_PRS_Integration_and_Feature_Assembly.ipynb
├── 5-1.Step5_XGBoost.ipynb
├── 5-2.Step5_TabPFN.ipynb
├── 6.Step6_Early_Detection.ipynb
├── 7.Step7_Genetic_Analysis.ipynb
├── 8.Step8_MR.ipynb
├── 9.Step9_Independent_Test_Counts.ipynb
└── README.md
```

The `External_data/` and `local_data/` directories are excluded from Git. External software, reference panels, and other required inputs must be obtained separately where applicable.

## Demonstration workflow

The multimodal demonstration uses 100 participants with modified demonstration inputs and executes the complete model-training workflow.

The participants are divided into four subsets:

| Dataset | N | Purpose |
|---|---:|---|
| Training1 | 32 | Train four modality-specific DenseNet121 models |
| Training2 | 32 | Train the abdominal GNN and XGBoost; provide TabPFN in-context training data |
| Validation | 16 | Model selection and validation |
| Testing | 20 | Final demonstration evaluation |

The workflow includes:

- Abdominal ultrasound (ABD) images
- Bone mineral density (BMD) images: left hip, right hip, and spine
- Demographic and clinical characteristics
- Polygenic risk scores
- T2D outcome labels

The image and tabular demonstrations are distinct from the original research dataset.

The 100-participant demonstration trains new models. It does not merely apply the study-trained models to the demonstration participants.

## Notebook execution order

| Step | Notebook | Analysis |
|---|---|---|
| 0 | `0.Read_demo_data.ipynb` | Read and validate demonstration data |
| 1 | `1.Step1_PRS_Construction.ipynb` | PRS construction and scoring |
| 2 | `2-1` through `2-4.Step2_DenseNet_*.ipynb` | Train modality-specific DenseNet121 models |
| 3 | `3.Step3_GNN.ipynb` | Train the abdominal GNN |
| 4 | `4.Step4_PRS_Integration_and_Feature_Assembly.ipynb` | Assemble multimodal prediction features |
| 5 | `5-1.Step5_XGBoost.ipynb` | XGBoost training and evaluation |
| 5 | `5-2.Step5_TabPFN.ipynb` | TabPFN analysis and evaluation |
| 6 | `6.Step6_Early_Detection.ipynb` | Early-detection and survival analyses |
| 7 | `7.Step7_Genetic_Analysis.ipynb` | GWAS demonstration |
| 8 | `8.Step8_MR.ipynb` | Mendelian randomization |
| 9 | `9.Step9_Independent_Test_Counts.ipynb` | Effective independent SNP and phenotype tests |

Each notebook documents its corresponding analysis. Outputs are organized under `Outputs/`.

Some analyses require separately acquired external software or reference data.

## Multimodal feature structure

The full multimodal predictor set contains 4,100 features:

| Feature group | Number |
|---|---:|
| ABD GNN representation | 1,024 |
| BMD left hip representation | 1,024 |
| BMD right hip representation | 1,024 |
| BMD spine representation | 1,024 |
| Age, sex, and family history | 3 |
| PRS | 1 |
| **Total** | **4,100** |

Feature order and preprocessing must remain consistent between model training and inference.

## Trained model artifacts

Two groups of model artifacts are provided under `model_weights/`.

### Demo-trained_models

These artifacts were generated from the demonstration workflow.

They include trained DenseNet, GNN, XGBoost, and early-detection model files, together with applicable configuration files. TabPFN configuration information is also included.

These demonstration-trained models are separate from the original study models.

### Study-trained_models

The study-model bundle contains:

- Four historical modality-specific DenseNet121 checkpoints
- One historical abdominal GNN/GCN checkpoint
- A reconstructed M12 XGBoost model using 4,100 features
- The corresponding `feature_order_4100.json`

The M12 XGBoost artifact is a verified reconstruction, not the original serialized checkpoint.

A study-specific TabPFN neural-network checkpoint cannot be provided because standard TabPFN relies on pretrained foundation-model weights and in-context learning rather than generating a separately trained neural-network checkpoint for this dataset.

Reproducing historical TabPFN predictions additionally requires the appropriate support data and configuration, which are not included as an unrestricted public study dataset.

Study-trained artifacts are provided separately from the 100-participant demonstration training workflow.

### Study-trained Cox model (PRS rescaled to 0–1)

The study-trained Cox proportional hazards model was refitted
using the original research data with a 0–1 transformation
of the PRS variable `SCORESUM`.

The original serialized model,
`Cox_SCORESUM01_Model.joblib`, is retained in the private
research workspace and is **not included in the public
GitHub release**, because the serialized lifelines object
contains individual-level training information.

The accompanying `Cox_SCORESUM01_Scaler.json` is retained
in `model_weights/Study-trained_models/Cox/` to document
the PRS scaling configuration. It is not a standalone
prediction model.

The 100-participant demonstration Cox workflow is provided
separately in Step 6.

## Effective independent tests (Step 9)

Step 9 illustrates the estimation of effective independent test counts.

For genetic variants, the demonstration uses a subset of the provided chromosome 22 genotype data to construct a SNP correlation matrix and perform eigenvalue-based calculations.

For phenotypes, fully synthetic, correlated imaging and glycemic traits are generated. Pearson correlation matrices and the Li–Ji method are used to estimate effective test counts.

The original study reported:

| Analysis | Original effective number |
|---|---:|
| Genetic variants, KGGSEE/GEC | 768,377.22 |
| 37 imaging phenotypes | 23 |
| 40 phenotypes, including T2D, fasting glucose, and HbA1c | 25 |

The study-wide statistical significance threshold was approximately 2.6 × 10^-9.

The demonstration uses a much smaller set of variants and synthetic phenotypes. Its estimates are not expected to match the original study.

The SNP demonstration does not reproduce the complete genome-wide KGGSEE/GEC procedure.

## Environment and execution

Environment specifications are available in `environments/`:

- `main.yml`
- `gpu.yml`
- `prscsx.yml`
- `jointprs.yml`
- `mr.yml`

Additional software and hardware requirements are documented in `docs/software_requirements.md`.

Start Jupyter from the repository root and open the relevant notebooks.

For example:

```bash
cd /path/to/GitHub
jupyter lab
```

Select the environment appropriate for each analysis.

External GWAS summary statistics, LD reference panels, and PRS software may need to be downloaded and configured separately. Files or symbolic links in the excluded `External_data/` directory are not distributed through Git.

The presence of recorded demonstration outputs does not guarantee that all notebooks have been validated in a fresh, independent computing environment.

## Data availability and interpretation

The original participant-level research data are not distributed.

The repository contains demonstration data prepared for illustrating the analysis workflows. Some example inputs are modified from research inputs, whereas other data are simulated. Their provenance and permitted redistribution must be reviewed before public release.

Demonstration outputs, fitted models, and genetic association results should not be treated as independent validation of the corresponding manuscript findings.

Reproduction of original study estimates may require access to restricted source data and matching historical software configurations.

## Software references

- [PRS-CSx](https://github.com/getian107/PRScsx)
- [JointPRS](https://github.com/LeqiXu/JointPRS)

Additional method-specific references are provided in the analysis notebooks.

## Citation and license

Manuscript citation and repository license information will be finalized before public release.
