# Manuscript result tables

These CSV files transcribe the current Risonia manuscript. All metric values use the percentage scale. Differences in these metrics are percentage points; relative percentage improvements are stated separately in the project README.

- `semantic_segmentation.csv`: 39 method/protocol/dataset records, including a separate supervised PTv3 reference; columns are mIoU, mAcc, and overall accuracy (`allacc`).
- `panoptic_segmentation.csv`: frozen-encoder nuScenes PQ, SQ, RQ, PQ-things, and PQ-stuff results.
- `pandaset_ablation.csv`: reliability, rendered-view, and original-camera cluster-condition ablations; 50 downstream epochs and four-view test-time augmentation.

Protocol names: `linear` freezes the point encoder and trains a linear classifier; `decoder` freezes the point encoder and trains the decoder; `full_finetuning` updates the encoder and task readout; `supervised_reference` identifies PTv3.
