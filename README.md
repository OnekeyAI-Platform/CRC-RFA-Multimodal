# Multimodal fusion of radiology and pathology models for predicting radiofrequency ablation response in colorectal cancer lung metastases: a multicenter study

### **Abstract**



**Background:** Accurately predicting treatment response and prognosis following radiofrequency ablation (RFA) in colorectal cancer (CRC) lung metastases remains challenging due to tumor heterogeneity and complex microenvironments. This study aimed to develop and validate an exploratory multimodal framework integrating radiology and pathology for complete response (CR) prediction and local tumor progression-free survival (LTPFS) stratification in CRC patients with lung metastases undergoing RFA, specifically aligning with data-driven clinical decision support system paradigm.

**Methods:** This retrospective study involved 249 eligible CRC lung metastases patients from two medical centers in China between 2016 and 2022. A multimodal approach was employed: Delta habitat analysis on paired pre- and post-ablation CT images characterized spatiotemporal tumor heterogeneity and dynamic treatment-induced changes. A 2.5D deep learning radiomics model leveraged multi-channel CT slices to capture crucial 3D contextual information. Weakly supervised pathomics signatures were derived from H&E-stained whole slide images (WSIs) using multi-instance learning (MIL), capturing intricate microscopic tissue characteristics. These multi-scale features were integrated with clinical variables via logistic regression to construct a unified predictive model. Model performance was evaluated in an independent test cohort using AUC, accuracy, sensitivity, specificity, and Kaplan-Meier analysis for survival outcomes, ensuring its potential for clinical translation.

**Results:** In the validation cohort, the Delta-Habitat signature (AUC = 0.818) outperformed conventional radiomics models, highlighting its ability to capture dynamic treatment effects. The combined multimodal model achieved promising response prediction (AUC = 0.872, 95% CI: 0.821 - 0.923), markedly surpassing single-modality signatures (AUC range: 0.672 - 0.818). This integrated model provided feasible LTPFS risk stratification (log-rank P < 0.001), effectively differentiating patient outcomes. Decision curve analysis confirmed the clinical net benefit offered by this multimodal approach, highlighting its potential for practical application and improved patient management.

**Conclusions:** This multimodal framework demonstrates significant potential for improving ablation outcomes prediction and guiding individualized treatment strategies. By harmonizing diverse radiological, pathological, and clinical digital information, this framework offers an exploratory multimodal predictive prototype that provides a methodological foundation for future clinical decision support systems development. 

 

**Keywords:** Colorectal cancer lung metastases, Radiofrequency ablation, Radiomics, Habitat, Deep learning, Pathomics, Complete response, Local tumor progression-free survival



## Workflow

Run the notebooks from the repository root, following their step numbers within each branch.

| Stage | Notebooks | Purpose |
| --- | --- | --- |
| Data preparation | [Step 0](Step0_Data_Preprocessing.ipynb) | Organize and rename images and masks; prepare training inputs. |
| Radiomics and habitats | Steps 1.1–1.4 | Extract delta radiomics, pre-treatment habitat and delta habitat signatures; visualize habitat clustering. |
| Radiology deep learning | Steps 2.1–2.6 | Prepare 2.5D inputs, train and evaluate CNNs, aggregate slice predictions into patient features, and visualize models. |
| Pathology deep learning | Steps 3.1–3.6 | Prepare pathology tiles, train and evaluate CNNs, and aggregate tile predictions into patient features. |
| Clinical analysis | [Baseline statistics](Step4.1_Clinical_Baseline_Statistics.ipynb), [clinical model](Step4.2_Clinical_Model.ipynb) | Summarize clinical variables and build the clinical signature. |
| Integrated evaluation | [Step 5.1](Step5.1_Summarize_Metrics.ipynb) | Combine signatures and evaluate discrimination, calibration, decision curves, and survival stratification where data are available. |

The `Step2.2.2` and `Step3.2.2` notebooks provide separate test-set prediction. Patient features include prediction histograms and TF-IDF representations. Model choices and signature names are configured in [config.yaml](config.yaml).

## Environment

Use a Python environment with Jupyter Notebook and the **Onekey toolkit (`onekey_algo`)** required by the imports. Onekey is an external dependency; its implementation and installation package are not included here. Start with the original study environment or obtain a compatible toolkit distribution from its provider.

Other dependencies used directly or through the toolkit include NumPy, pandas, SciPy, scikit-learn, PyRadiomics, PyTorch, MONAI, OpenCV, nibabel, Matplotlib, seaborn, joblib, XGBoost, LightGBM, and lifelines. Notebook metadata records Python 3.7.12, but the repository does not include a verified environment lockfile. CUDA-enabled training requires compatible PyTorch/CUDA versions; training notebooks currently select GPU 0.

## Data and configuration

The study datasets, cohort split files, trained weights, and generated results are not included. Supply the following inputs before running the corresponding stages:

- Paired pre-treatment and post-treatment CT images and matching ROI masks. Image and mask filenames must match, typically using `.nii` or `.nii.gz`.
- Cropped radiology slices and pathology tiles for the deep learning branches.
- A label CSV with `ID`, `group`, and `label` columns. The configured groups are `train`, `val`, and `test`; labels map `0` to CR and `1` to non-CR. Keep identifiers consistent across modalities.
- Clinical and, for survival analyses, survival CSV files.
- The `split_info/` sample lists and class-name files referenced by the training and prediction notebooks. Sample lists contain paths and labels separated by tabs; class-name files contain one class per line.

Update `radio_dir`, `path_dir`, `label_file`, `clinic_file`, and `survival_file` in `config.yaml`. Also inspect notebook-specific paths: some cells still use study-specific `D:` or `Z:` locations. Radiomics extraction parameters are in [custom_settings/](custom_settings/), with `exampleCT.yaml` selected by default. Create output directories such as `data/`, `features/`, `results/`, and `img/` when required by a notebook.

Start Jupyter from the repository root:

```bash
jupyter notebook
```

On Windows, [Launch_Notebooks.bat](Launch_Notebooks.bat) is a convenience launcher for the original Onekey/Conda installation; adjust its installation paths for another machine.

## Reproducibility notes

These are research notebooks with manually coordinated intermediate files, rather than a single executable pipeline. Review paths, cohort assignments, selected models, and required intermediate files before each stage. Some figures referenced there are generated outputs absent from this source tree. The [doc](doc) directory contains figures and local manuscript/review materials. Office manuscript and review files are excluded from Git by default in `.gitignore`, while remaining available locally.

No license or finalized publication citation has been supplied. Add the authors' chosen license and the final paper citation before public release.
