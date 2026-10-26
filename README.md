# Medical Image Risk Assessment

Multimodal type 2 diabetes risk assessment, early-detection analysis, and genetic analyses.

> **Development status:** This repository is being expanded and reorganized. The existing DenseNet121–XGBoost/TabPFN example is available. The complete analysis workflows, demonstration datasets, pretrained study models, and Docker environments are in preparation.

## 1. Overview

This project integrates medical imaging, clinical characteristics, and polygenic risk scores (PRS) for type 2 diabetes (T2D) risk assessment.

The repository is organized around three main analysis workflows:

1. **Multimodal prediction:** image preprocessing, DenseNet121 feature extraction, graph neural network (GNN) aggregation of abdominal images, feature integration, and prediction using XGBoost or TabPFN.
2. **Early-detection and survival analysis:** longitudinal data preparation, risk-group assignment, and Cox proportional hazards analysis.
3. **Genetic analyses:** genome-wide association studies (GWAS), PRS construction, and Mendelian randomization (MR), including sensitivity analyses.

The expanded release will include analysis scripts, documented parameters, demonstration datasets, pretrained models where appropriate, intermediate outputs, and example execution logs.

## 2. Analysis workflows

### 2.1 Multimodal prediction

The demonstration workflow will start from medical images and tabular inputs and apply the corresponding pretrained models:

1. Read abdominal (ABD) and bone mineral density (BMD) images.
2. Apply the preprocessing required by each image model.
3. Extract image features using modality-specific DenseNet121 models.
4. Aggregate multiple ABD images from the same participant using the trained GNN.
5. Retain DenseNet features for the left hip, right hip, and lumbar spine BMD images.
6. Combine image features with age, sex, family history, and the PRS required by the selected prediction model.
7. Generate prediction probabilities using XGBoost or TabPFN.

The full multimodal configuration contains the following predictors:

| Feature group | Number of features |
|---|---:|
| ABD GNN representation | 1,024 |
| Left hip BMD DenseNet representation | 1,024 |
| Right hip BMD DenseNet representation | 1,024 |
| Lumbar spine BMD DenseNet representation | 1,024 |
| Age, sex, and family history | 3 |
| One PRS for the selected model configuration | 1 |
| **Total** | **4,100** |

Participant identifiers and outcome labels are not predictors.

Feature names, ordering, preprocessing, fitted transformations, and PRS selection will be documented for each released model. Training and inference instructions will be provided separately.

### 2.2 Early-detection and survival analysis

This workflow will include:

- Preparation of the eligible longitudinal cohort.
- Definition of baseline, follow-up time, events, and censoring.
- Assignment of risk groups using documented prediction scores and thresholds.
- Cox proportional hazards analysis.
- Generation of hazard ratios, confidence intervals, tables, and survival figures.

A demonstration dataset with the required follow-up variables will be provided separately. Eligibility for survival analysis will follow the documented baseline and outcome definitions.

### 2.3 Genetic analyses

The genetic analysis workflow will include:

- Genotype quality control.
- Phenotype and covariate preparation.
- GWAS execution and result processing.
- Instrument selection and linkage disequilibrium (LD) clumping.
- Allele harmonization between exposure and outcome summary statistics.
- MR estimation and sensitivity analyses, including MR-PRESSO.

Demonstration data and execution instructions will be documented separately for GWAS and MR.

### 2.4 PRS construction and integration

Scripts will document the PRS-CSx and JointPRS workflows, including:

- External GWAS summary-statistic sources.
- LD reference panels and ancestry settings.
- Software versions and analysis parameters.
- SNP-weight estimation and individual-level scoring.
- Score combination, standardization, and other transformations.

The multimodal prediction demonstration will read prepared PRS values from a table. It will not require individual-level genotype data.

Each prediction model will use the PRS definition and transformations corresponding to its training configuration. Transformation parameters will not be re-estimated from the demonstration participants.

## 3. Planned repository structure

The following directories will be added as the corresponding workflows are prepared and verified.

| Location | Description |
|---|---|
| `pipelines/prediction/` | Image preprocessing, feature extraction, GNN aggregation, feature integration, training, and inference |
| `pipelines/survival/` | Longitudinal data preparation, survival analysis, and figures |
| `pipelines/genetics/gwas/` | Genotype QC, phenotype preparation, and GWAS |
| `pipelines/genetics/prs/` | PRS-CSx and JointPRS construction, scoring, and transformations |
| `pipelines/genetics/mr/` | Instrument preparation, MR, and sensitivity analyses |
| `data/demo/` | Demonstration inputs and data dictionaries |
| `configs/` | Analysis parameters and model configurations |
| `models/` | Model descriptions, download instructions, checksums, and feature definitions |
| `examples/expected_outputs/` | Reference demonstration outputs and execution logs |
| `docs/` | Detailed documentation and manuscript-result-to-script mappings |
| `docker/` | Container definitions and execution instructions |

The current root-level scripts implement the earlier image-classification example. They do not yet contain all workflows described above.

## 4. Data and demonstration datasets

### Imaging and multimodal demonstration

A 100-participant demonstration dataset is being prepared.

The release documentation will describe:

- Available image modalities and the number of images per participant.
- Clinical variables and PRS fields.
- Outcome labels, where applicable.
- Image modifications and any synthetic or modified tabular values.
- Input formats and participant-level data correspondence.

Modified or synthetic demonstration data are intended to demonstrate workflow execution. Their results should not be interpreted as estimates of performance in the original research cohort.

The existing `example_data/` directory belongs to the earlier image-classification example and is separate from the planned multimodal demonstration.

### Genetic and survival demonstrations

Original participant-level genotype data will not be distributed through this repository.

The planned GWAS demonstration will use synthetic genotype and phenotype data. The MR demonstration will use an example exposure–outcome summary-statistic pair with documented provenance and the required LD information.

The survival demonstration dataset will include the variables required by its documented analysis.

Access to the original research data remains subject to the relevant data-provider and institutional requirements.

## 5. Pretrained models

Pretrained study-model packages are in preparation.

Each released package will document:

- Model type and analysis version.
- Required input variables and image modalities.
- Feature names and ordering.
- Preprocessing and fitted transformations.
- Software requirements.
- Download location and file checksum.

The intended packages include modality-specific DenseNet models, the ABD GNN, and the corresponding downstream prediction models.

Details of any released survival-model package will be provided with its analysis instructions.

## 6. Installation and quick start

Installation instructions and executable commands for the expanded workflows are in preparation.

Each released workflow will provide:

1. Environment installation instructions.
2. Instructions for obtaining its demonstration data and models.
3. A command to run the demonstration.
4. Descriptions of intermediate and final outputs.
5. An example execution log and reference results.
6. Tested hardware and approximate runtime.

Docker instructions will be added after the corresponding workflows have been verified.

The existing `requirements_cpu.txt` and `requirements_gpu.txt` apply to the earlier example and are not yet verified environment specifications for the complete study workflows.

## 7. Reproducing the study analyses

Documentation will connect each reported analysis or manuscript result to:

- Its analysis script and execution command.
- The corresponding configuration and software versions.
- Required input data and model files.
- Intermediate and final outputs.
- Execution logs.

Historical analyses and revision analyses will be identified separately.

The public demonstrations will show how the released workflows execute on the supplied example data. Reproducing numerical manuscript results requires the original authorized research data and the matching analysis configuration.

## 8. Citation and contact

The manuscript citation, maintainer contact information, and licensing information will be added before the expanded release.

Relevant software resources:

- [PRS-CSx](https://github.com/getian107/PRScsx)
- [JointPRS](https://github.com/LeqiXu/JointPRS)

Additional method citations and software acknowledgments will accompany the corresponding analysis modules.
