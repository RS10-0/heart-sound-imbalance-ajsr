# Evaluating Class-Imbalance Mitigation Strategies for Heart Sound Classification

Code and data-split records accompanying the manuscript submitted to the
*American Journal of Student Research (AJSR)*.

**Author:** [Your name]
**Contact:** [email]

## Overview

This repository contains the preprocessing and training code, dataset split
assignments, random seeds, model configurations, and per-recording test-set
predictions needed to reproduce the results in the manuscript. It compares
three ResNet-18 configurations for normal/abnormal phonocardiogram (PCG)
classification — standard cross-entropy (Baseline), inverse-frequency
class-weighted loss (Class-Weighted), and weighted random oversampling
(Oversampled) — evaluated with a source-held-out (leave-one-database-out)
protocol.

## Dataset

This study uses the **PhysioNet/Computing in Cardiology Challenge 2016**
dataset (version 1.0.0), publicly available at:
https://physionet.org/content/challenge-2016/1.0.0/

The dataset itself is **not** redistributed here. To reproduce, download it
from PhysioNet and place the training folders (`training-a` … `training-f`)
as described in the notebook. This study used the six labeled training
databases (3,240 recordings: 2,575 Normal, 665 Abnormal); the separately
distributed `validation` folder was not used. Only the single-channel
phonocardiogram (`.wav`) signal was used; ECG (`.dat`) signals were not loaded.

## Repository contents

- `heart_sound_pipeline.ipynb` — cleaned end-to-end notebook (preprocessing,
  splits, training, evaluation).
- `splits/split_assignments.csv` — every recording ID with its label, source
  database, and fold/split assignment.
- `predictions/` — per-recording test-set predictions for each model, fold,
  and seed.
- `requirements.txt` — Python package versions.
- `README.md` — this file.

## Preprocessing summary

- Resampled to 2,000 Hz; fixed 4-second window (zero-pad / truncate).
- Mel-spectrogram: n_fft = 256, win_length = 256 (Hann), hop_length = 64,
  center = True, 64 Mel bands, fmax = 1,000 Hz, power spectrogram.
- Converted to dB (ref = per-recording max); min–max normalized to [0, 255];
  3-channel replication; resized to 224 × 224; ImageNet normalization.

## Model and training

- ResNet-18, ImageNet-pretrained, binary head, fully fine-tuned.
- SGD (lr = 0.001, momentum = 0.9), batch size 32, [N] epochs.
- Checkpoint selected by best validation Abnormal-class F1.
- Random seeds: [LIST SEEDS, e.g. 0, 1, 2].
- Deterministic settings enabled (fixed seeds for Python/NumPy/PyTorch,
  deterministic cuDNN, seeded data-loader workers).

## Evaluation

- Metrics: accuracy, precision, sensitivity, specificity, F1 (Abnormal =
  positive class).
- 95% Wilson score confidence intervals; pairwise McNemar tests with
  Bonferroni adjustment.
- Source-held-out folds: training-a, training-b, training-e held out in turn;
  training-c/d/f retained in training in all folds.

## Environment

- Google Colab, Python 3, NVIDIA [GPU MODEL] GPU.
- See `requirements.txt` for exact package versions.

## Reproduction

1. Download the PhysioNet 2016 dataset (link above).
2. Open `heart_sound_pipeline.ipynb` and set the dataset path.
3. Run cells in order. Split assignments and seeds are fixed, so results
   should match those in `predictions/`.

## Citation

## License

MIT (see LICENSE).
