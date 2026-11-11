# Release status

[Back to the project overview](../README.md)

**Current stage: public project preview for a manuscript being prepared for CVPR 2027.**

## Included now

- Project description, motivation, and method overview.
- Main semantic-segmentation results with mIoU, mAcc, and overall accuracy.
- nuScenes panoptic results and PandaSet component ablations.
- Paper figures and a provisional citation.

## In preparation

- Portable self-supervised pretraining and downstream evaluation code.
- Dataset preparation, calibrated image-point association, frozen-feature extraction, and auxiliary rendering tools.
- Pinned software environments and experiment configurations.
- Pretrained checkpoints, model cards, and download links.
- Installation, training, inference, and reproduction instructions.
- Public manuscript or preprint link.

No runnable implementation or model checkpoint is included in this initial preview. The CSV tables record the current manuscript results; they are not newly produced benchmark runs from this repository.

## Evaluation scope

Pretraining and adaptation are performed separately on nuScenes, PandaSet, and Waymo training splits. The semantic readouts are linear probing, frozen-encoder decoder adaptation, and full fine-tuning. Shared input attributes are XYZ, camera-projected RGB, and normals; Risonia also uses visual cluster conditions during downstream evaluation.

Concerto is reproduced with the same frozen DINOv3 encoder as Risonia. Utonia's official pretrained checkpoint is adapted with newly trained downstream readouts. PTv3 is listed separately as a supervised reference.

The nuScenes panoptic comparison uses the same semantic/center-offset head, grouping procedure, and 50-epoch downstream budget for all frozen encoders. It covers all 6,019 validation frames using ten-view test-time augmentation.

## Figures and numerical scores

The teaser retains the original matched five-epoch PCA visualization controls. Its feature colors and displayed main-table linear-probing scores have different scopes, as stated in the README and manuscript caption. PCA bases are fitted separately for the two encoders; PCA hues do not denote shared semantic classes. Ground truth uses semantic colors.

The other qualitative figures use matched readouts for Concerto and Risonia within each comparison. White regions denote areas without observed point support.

## Data and licensing

The datasets, third-party models, and their weights remain subject to their providers' terms. This repository does not redistribute raw datasets or third-party model weights. Licensing information for the implementation will be supplied with the code release.

## Updates

2026-11-11: initial project preview with documentation, manuscript result tables, and figures.
