# VDiff-5329 (VDiff-SR) : Diffusion with Hierarchical Visual Autoregressive Priors for Image Super-Resolution

This project proposes a novel VAR-Diffusion hybrid framework, named VDiff-SR, to address the problem of real-world image super-resolution (Real-ISR). The core idea of VDiff-SR is to inject hierarchical multi-scale features from a pretrained Visual AutoRegressive (VAR) generative model into the denoising process, aiming to overcome the limitations of traditional diffusion models in leveraging multi-scale structural priors.

## Introduction

Real-world image super-resolution is fundamentally ill-posed due to unknown and complex degradations in low-resolution inputs. Although recent diffusion-based ISR methods have demonstrated powerful generative modeling capabilities, they often underutilize multi-scale structural priors. VDiff-SR addresses these issues through the following designs:

*   **Condition-Gated Unit (CoGU)**: Adaptively modulates the fusion of pretrained VAR features into the diffusion backbone.
*   **Cross-Scale Prior-Aligned Attention (CSPA)**: Captures both local (same-scale) and cross-scale context evolution, ensuring that the denoiser attends to multi-scale priors at every step.
*   **Inverse Noise Prediction**: Enhances the alignment between the latent distribution induced by VAR and the model's output.

By leveraging the rich semantic context that VAR encodes in a coarse-to-fine latent hierarchy, VDiff-SR effectively guides the diffusion trajectory, overcoming informational ambiguity from unknown degradations. This synergy yields significant perceptual improvements on standard ISR benchmarks.

## Installation and Environment Setup

```
pip install -r requirements.txt
```

## Training

You can use the following command to train the VDiff-SR model:

```
python main_distill.py --cfg_path configs/VDiff.yaml --save_dir logs/VDiff
```

Please ensure that your configuration file (`configs/VDiff.yaml`) and dataset paths are set up correctly.

## Inference

After training, you can use the following command for inference to generate super-resolved images:

```
python inference.py -i ../../data/DIV2K_valid_LR_bicubic/X4 -o results/VDiff/DIV2K_val_var --scale 4 --ckpt1 logs/VDiff/2025-04-23-17-41/ckpts/model_4340.pth --ckpt2 logs/VDiff/2025-04-23-17-41/ckpts/model_4340_pvar.pth -r ../../data/DIV2K_valid_HR --one_step
```

Parameter description:
*   `-i`: Input path for low-resolution images.
*   `-o`: Output path for super-resolved images.
*   `--scale`: Super-resolution magnification factor.
*   `--ckpt1`: Path to the main model weights.
*   `--ckpt2`: Path to the pretrained VAR model weights (if required by the inference script).
*   `-r`: Path to high-resolution reference images (for evaluation).
*   `--one_step`: (May represent one-step diffusion or another specific inference mode, depending on your implementation).

## License
