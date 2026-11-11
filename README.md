# Risonia

**Risonia: Visual Cluster Conditioning and Reliability-Weighted Supervision for 3D Self-Supervised Learning**

**Bole Zhang, Ce Zhang, and Nantheera Anantrasirichai**

![Target](https://img.shields.io/badge/Target-CVPR_2027-555555)
![Status](https://img.shields.io/badge/Status-Submission_in_preparation-blue)
![Code](https://img.shields.io/badge/Code-Release_in_preparation-orange)

[Overview](#overview) · [Results](#results) · [Release status](#release-status) · [Citation](#citation)

> **Status:** Manuscript in preparation for **CVPR 2027**. Code and pretrained models are being prepared for release.

## Overview

**Risonia is a self-supervised framework for learning 3D point representations for outdoor scene perception.**

We evaluate Risonia on **nuScenes, PandaSet, and Waymo** for semantic segmentation, with an additional panoptic segmentation evaluation on nuScenes.

![Continuous Waymo feature visualization](assets/teaser.png)

*Waymo feature visualization. PCA colors use matched five-epoch visualization models; displayed scores are the main-table linear-probing results.*

## Results

The tables below report the **current manuscript's validation results**. mIoU values are percentages.

### Semantic segmentation

| Protocol | Method | nuScenes | PandaSet | Waymo |
| :-- | :-- | --: | --: | --: |
| Linear probing | Sonata | 66.10 | 47.98 | 60.50 |
| Linear probing | Concerto | 75.20 | 63.64 | 63.00 |
| Linear probing | Utonia | 75.50 | 70.60 | 63.80 |
| Linear probing | Risonia | **76.92** | **72.08** | **64.92** |
| Frozen-encoder decoder | Sonata | 77.30 | 65.93 | 69.10 |
| Frozen-encoder decoder | Concerto | 78.00 | 71.27 | 69.20 |
| Frozen-encoder decoder | Utonia | 78.20 | 71.82 | **69.40** |
| Frozen-encoder decoder | Risonia | **82.54** | **76.21** | 69.30 |
| Full fine-tuning | Sonata | 81.70 | 74.55 | **72.90** |
| Full fine-tuning | Concerto | 81.80 | 79.51 | 69.80 |
| Full fine-tuning | Utonia | 82.20 | 79.12 | 71.40 |
| Full fine-tuning | Risonia | **82.74** | **80.41** | **72.90** |

Best mIoU values within each protocol and dataset are bold, including ties. PTv3 is a separate supervised reference: **80.40 / 60.15 / 71.30 mIoU** on nuScenes / PandaSet / Waymo.

**Improvements over Concerto.** Absolute gains below are **mIoU percentage points**. Relative gains are computed from the mean mIoU across the three datasets.

| Protocol | nuScenes | PandaSet | Waymo | Mean gain | Relative gain in mean mIoU |
| :-- | --: | --: | --: | --: | --: |
| Linear probing | +1.72 | +8.44 | +1.92 | +4.03 | +5.98% |
| Frozen-encoder decoder | +4.54 | +4.94 | +0.10 | +3.19 | +4.39% |
| Full fine-tuning | +0.94 | +0.90 | +3.10 | +1.65 | +2.14% |

The machine-readable [semantic results](results/semantic_segmentation.csv) also include mAcc and overall accuracy.

### Panoptic segmentation on nuScenes

| Method | PQ | SQ | RQ | PQ things | PQ stuff |
| :-- | --: | --: | --: | --: | --: |
| Sonata | 41.72 | 77.62 | 51.42 | 35.43 | 52.20 |
| Concerto | 54.31 | 82.31 | 64.87 | 48.97 | 63.21 |
| **Risonia** | **62.33** | **83.51** | **73.74** | **58.37** | **68.93** |

Risonia improves PQ over Concerto by **8.02 percentage points**, or **14.77% relative**. Its semantic mIoU in this evaluation is **70.08**.

[Panoptic results CSV](results/panoptic_segmentation.csv)

### Qualitative results

<details>
<summary>Linear probing</summary>

![Linear semantic segmentation comparisons](assets/linear.png)

</details>

<details>
<summary>Decoder adaptation</summary>

![Frozen-encoder decoder comparisons](assets/decoder.png)

</details>

<details>
<summary>Full fine-tuning</summary>

![Full fine-tuning comparisons](assets/finetuning.png)

</details>

## Release status

- [x] Project introduction, results, and qualitative figures
- [ ] Public paper / preprint
- [ ] Code and pretrained models

The manuscript is being prepared for **CVPR 2027**. Code and pretrained models will be added in future updates.

## Citation

The manuscript is in preparation. The following provisional citation can be used for this project; it will be updated when the paper becomes publicly available.

```bibtex
@unpublished{zhang2026risonia,
  title  = {Risonia: Visual Cluster Conditioning and Reliability-Weighted Supervision for 3D Self-Supervised Learning},
  author = {Bole Zhang and Ce Zhang and Nantheera Anantrasirichai},
  year   = {2026},
  note   = {Manuscript in preparation for CVPR 2027},
  url    = {https://github.com/zbole/Risonia}
}
```

## Acknowledgments

This work builds on and compares with [Pointcept](https://github.com/Pointcept/Pointcept), [Sonata](https://github.com/facebookresearch/sonata), [Concerto](https://github.com/Pointcept/Concerto), and [Utonia](https://github.com/Pointcept/Utonia).

We thank the providers of [nuScenes](https://www.nuscenes.org/nuscenes), [PandaSet](https://github.com/scaleapi/pandaset-devkit), and the [Waymo Open Dataset](https://waymo.com/open/).

## Contact

Please use [GitHub Issues](https://github.com/zbole/Risonia/issues) for questions about the project and release progress.
