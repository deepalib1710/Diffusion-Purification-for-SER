# EmoDiffPure2
Prosody-conditioned DiffWave diffusion purification as a defence against adversarial perturbations for Speech Emotion Recognition (SER) on IEMOCAP.
# EmoDiffPure

Prosody-conditioned diffusion purification as a defence for Speech Emotion Recognition (SER) on IEMOCAP.

## Overview
- Classifier: CNN-BiLSTM with attention, four emotion classes, 16 kHz audio, 4 s clips.
- Purifier: DiffWave conditioned on HuBERT-Base features and pYIN F0.
- Attacks: PGD-10 (eps 0.008) and adaptive EoT-PGD.
- Control: a Gaussian noise-only defence that adds the same noise but skips the denoiser.

## Main findings (IEMOCAP Session 1 held out, 1,085 utterances)
| Condition | Clean (%) | PGD-10 (%) |
|---|---|---|
| Undefended classifier | 54.0 | 1.9 |
| Gaussian noise only | 44.2 | 41.8 |
| EmoDiffPure purifier | 33.4 | 33.1 |

- The noise-only control outperforms the full purifier.
- About 73% of purified predictions are "sad"; the majority-class rate is 35.4%.
- Matched conditioning beats zero conditioning by 4.7 pp (95% CI 2.8 to 6.9).
- Adaptive attack runs (N = 30 and N = 100) are inconclusive.

## Repository contents
- `notebooks/` project notebook
- `demo/` live-demo cells

## Data
IEMOCAP is not included. Request access from the dataset authors and place it locally.
Paths in the notebook point to Google Drive and need editing for your setup.

## Status
Research code exported from Colab. A cleaned, scripted version is planned.

## Licence
To be decided.
