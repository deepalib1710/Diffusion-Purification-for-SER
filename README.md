# EmoDiffPure
Prosody-conditioned DiffWave diffusion purification as a defence against adversarial perturbations for Speech Emotion Recognition (SER) on IEMOCAP.

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

## How it works

1. **Input:** a 4 s, 16 kHz speech clip. An attacker may have added a tiny perturbation
   (PGD-10, eps 0.008) so that the emotion classifier gets it wrong.
2. **Conditioning:** HuBERT-Base features (768-D) and pYIN pitch (F0) are extracted from the
   incoming audio and combined into a 769-D conditioning vector.
3. **Purification:** the clip is noised with the diffusion forward process in one jump to
   start index t* = 3 (noise std 0.0806), then DiffWave denoises it in 4 reverse steps,
   guided by the conditioning vector.
4. **Classification:** a CNN-BiLSTM with attention predicts one of four emotions
   (neutral, happy, sad, angry) from the purified audio. The classifier is fine-tuned on a
   50/50 mix of clean and purified audio, with the purifier frozen.
5. **Control:** the noise-only defence runs step 3 without the denoiser (same noise, no network).
   Comparing against it shows how much of the defence comes from the noise and how much
   from the denoiser.

Why the control matters: in our results the noise-only defence beats the full purifier, so
most of the robustness comes from the injected noise and the denoiser's reconstruction loses
emotion information.

## Repository contents
- Research is still in progress therefore complete implementation and code details will be updated in due time.

## Data
IEMOCAP is not included. Request access from the dataset authors and place it locally.
Paths in the notebook point to Google Drive and need editing for your setup.

## Status
Research code exported from Colab. A cleaned, scripted version is planned.

## Licence
To be decided.
