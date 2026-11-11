# Method and reported configuration

[Back to the project overview](../README.md)

This document summarizes the method in the current research manuscript. Portable implementation files and exact launch commands are being prepared for release.

## Visual conditioning and prediction

Each input point has coordinates, camera-projected RGB, and a surface normal. A point-attribute stem maps these nine channels to 64 channels. The encoder entrance concatenates a 32-channel visual condition, giving a 96-channel input.

A frozen DINOv3 visual encoder supplies 1,024-dimensional patch descriptors. The original-camera descriptors are complete visual prediction targets. Their cluster IDs supply the main input conditions. Descriptors from three auxiliary rendered views are clustered independently to provide complementary grouping cues.

Cluster assignment is many-to-one: different descriptors can receive the same ID and therefore the same main condition. A predictor using only that condition cannot distinguish all within-cluster descriptor differences. Risonia limits direct visual-feature copying while letting the point encoder use point attributes and neighborhood context.

Cluster centers and assignment frequencies use online exponential moving averages over unique valid patches. Updates occur after successful optimizer steps; validation and downstream adaptation freeze the cluster statistics. The point-learning gradients do not update the frozen visual encoder or clustering statistics.

## Sparse rendered views

Bird's-eye, front-left, and front-right views use 512 x 512 RGB rasters and depth-buffered 3 x 3 point splats. Uncovered pixels retain a light-gray background (245, 245, 245). Points without observed camera RGB use a neutral color (150, 150, 150). Missing surfaces are not inpainted.

The visual encoder receives a fully defined image tensor despite sparse scene coverage. Only patches with visibility-valid projected-point support participate in clustering. A point without a valid association receives no condition from that view. Rendered descriptors provide bounded auxiliary grouping cues; only original-camera descriptors serve as complete visual prediction targets.

## Joint point encoding

The encoder jointly processes the conditioned point set. Stored pooling correspondences map the E1-E4 multiscale features back to input points; their channel-wise concatenation forms the per-point vector used for visual prediction. Features of eligible points are averaged within each original-camera patch, then a prediction head estimates its complete descriptor.

## Reliable visual supervision

Frozen image-predicted depth is aligned per camera to measured point depth. Absolute and relative depth disagreement determine a soft reliability target. A small calibration head refines the weight within a depth-agreement-dependent bound. Detached reliability weights form a normalized weighted visual-alignment loss; the calibration head has a separate binary cross-entropy objective against the soft depth-agreement target.

Risonia also retains the masked, rolled-mask, and local-to-global 3D self-distillation objectives. Synchronized masking hides corresponding camera and rendered conditions for selected prediction targets, and view dropout varies auxiliary context. The moving-average point teacher receives no cluster conditions.

## Configuration reported in the manuscript

| Component | Setting |
| :-- | :-- |
| Frozen visual encoder | DINOv3 ViT-L/16, LVD-1689M pretraining |
| Visual descriptor dimension | 1,024 |
| Point attributes | XYZ + camera-projected RGB + surface normal |
| Attribute stem | 9 -> 64 |
| Visual-condition width | 32 |
| Encoder widths | 96, 128, 256, 512, 768 |
| Visual-prediction scales | E1-E4, mapped back to input points |
| Original-camera clusters | 256 |
| Frequency penalty | 0.03 |
| Original-camera ID projection | 8 -> 32 |
| Auxiliary views | Bird's-eye, front-left, front-right |
| Auxiliary render / patch grid | 512 x 512 / 32 x 32 |
| Clusters per auxiliary view | 32 |
| Combined auxiliary projection | Bias-free 15 -> 32 |
| Auxiliary strength | 0.1 |
| Training keep probability per auxiliary view | 0.25 |
| Downstream auxiliary views | All available views |
| Point splat / visibility tolerance | 3 x 3 / 0.15 m |
| Bird's-eye span / oblique field of view | 100 m / 80 degrees |
| Frozen depth predictor | DA3NESTED-GIANT-LARGE-1.1 |
| Absolute / relative depth tolerances | 3 m / 0.2 |
| Calibration lower-bound factor | 0.5 |
| Calibration head | 6 -> 32 -> 1, GELU |
| 3D loss weights: mask / roll / local | 0.125 / 0.125 / 0.25 |
| Visual-alignment / calibration loss weights | 5 / 0.1 |
| Optimizer / schedule | AdamW / cosine OneCycle |
| Mixed precision | BF16 |

Visual cluster conditions are retained during downstream evaluation. Depth prediction and reliability calibration are pretraining-only components. Risonia is therefore a visually conditioned model at downstream use as well as during pretraining.
