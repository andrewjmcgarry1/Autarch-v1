# Autarch-v1: melanoma vs. other skin lesions (from-scratch CNN)

> **Research only. Not for clinical use.** This model is a research prototype. It must not be used to diagnose, screen, or make decisions about any person's health.

Autarch-v1 is a 3.10-million-parameter convolutional neural network, designed and trained from scratch (no pretrained weights), that estimates the probability that a dermoscopy image shows melanoma, compared with the other six lesion categories in the HAM10000 dataset.

## Results (held-out test set, scored once)

Test set: 1,529 images from 1,119 lesions, of which 176 are melanoma. Split was done by lesion, so no lesion appears in more than one split.

| Metric | Mean of 3 seeds | Release model (seed 2) |
|---|---|---|
| AUC | 0.872 ± 0.003 | 0.869 (95% CI 0.837–0.900) |
| Sensitivity at 90% specificity | 0.612 ± 0.012 | 0.602 (95% CI 0.513–0.691) |

The release model's operating point, chosen on the validation set, gives:

- Specificity 0.893 (95% CI 0.872–0.913)
- PPV 0.422, NPV 0.945
- Confusion matrix: TP 106, FN 70, FP 145, TN 1208

**Read these numbers carefully.**
- About 40% of melanomas are missed at this operating point.
- The false-alarm rate is much higher for benign keratosis (about 22% flagged) and vascular lesions (about 21%) than for benign nevi (about 10%).
- Subgroup results (sex, age) rest on small counts and are descriptive only.
- Overall accuracy is 86%, which is below the 88.5% you would get by always predicting "not melanoma." Accuracy is therefore not used as a headline metric.
- The test set was scored once. Validation was used for every decision, including the choice of light augmentation and test-time augmentation. Test-time augmentation added about 0.01 AUC on the release model (0.859 without it, 0.869 with it).

## Model weights

The trained model is hosted on Kaggle: [https://www.kaggle.com/models/aurevenllc/autarch-v1/]

The weights are not stored in this repository.

## Model details

- **Type:** convolutional neural network, custom architecture, from scratch
- **Size:** 3.10 million parameters, about 21 weight layers
- **Input:** dermoscopy photo resized to 128 × 128 pixels
- **Output:** probability of melanoma (versus the other six categories)
- **Framework:** PyTorch
- **Training:** AdamW, one-cycle learning rate (peak 2e-3), 60 epochs, batch size 64, mixed precision, light augmentation, EMA weights, last epoch used (no epoch selection)
- **Hardware:** one NVIDIA T4 GPU, about 7 minutes per run
- **Decision threshold:** set on validation for 90% specificity

## Data

[HAM10000](https://doi.org/10.7910/DVN/DBW86T) (Tschandl, Rosendahl & Kittler, 2018): 10,015 dermoscopy images, 7,470 lesions, 1,113 melanoma.

**The dataset is not included in this repository.** Download it from the original source and check its license terms before reuse.

Splits are in `results/splits.csv`, which lists each image's split. Running `code/step1_data_prep.py` regenerates the same split.

## Known limitations

- Trained and tested on one public dataset, which is mostly lighter skin and dermoscopy images. Performance on other skin tones, phone photos, or other clinics is untested and is expected to be worse.
- No external validation.
- Labels are a mix of histopathology and expert consensus and contain some noise.
- Not compared with pretrained models or with dermatologists.
- Confidence intervals are wide because the test set has only 176 melanomas.

## Reproducing the results

1. Install the dependencies: `pip install -r requirements.txt`
2. Download HAM10000 and place it where `code/step1_data_prep.py` expects it (the script was run on Kaggle with the dataset attached).
3. Run the steps in order: `step1_data_prep.py`, `step2_data_loading.py`, `step3_architecture.py`, `step4_training.py`, `step4b_experiments.py`, `step5_final_test.py`.

The scripts were developed on Kaggle with a T4 GPU. Each run takes about 7 minutes.

## Repository contents

```
code/      the six pipeline scripts, run in order
results/   results tables, the test report, per-image test predictions, plots, splits.csv
model/     autarch_v1_final.pt (release weights) and the model card
```

## How this was made

The architecture, experiments, and analysis were designed and run by the author. Claude (Anthropic) assisted with writing the code and checking the analysis. Every number reported here was produced by the code in this repository.

## License

All rights reserved. You may view the code and results, and cite this work. Please contact the author before copying, modifying, or using the code or model weights for any other purpose.

## Citation

If you use this work, please cite the repository and HAM10000:

```
Tschandl P, Rosendahl C, Kittler H. The HAM10000 dataset, a large collection of multi-source
dermatoscopic images of common pigmented skin lesions. Scientific Data. 2018;5:180161.
```
