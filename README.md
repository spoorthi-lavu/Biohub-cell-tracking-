# Biohub Cell Tracking During Development

A deep learning and computer vision project focused on **3D cell detection and multi-object tracking in time-lapse microscopy data**.

## Overview

This project explores automated cell tracking during biological development using volumetric microscopy images. The objective is to detect cells across successive timepoints and establish temporal correspondences between detections to construct cell-tracking graphs.

The project was developed as part of a semester-long deep learning effort and a Kaggle cell-tracking competition.

## Objectives

- Explore and process 3D time-lapse microscopy data.
- Detect candidate cell locations in volumetric images.
- Investigate spatial calibration for accurate distance measurements.
- Associate detections across timepoints using assignment algorithms.
- Represent tracking results as nodes and edges.
- Generate and validate competition-compatible submission files.
- Compare experimental approaches and document their limitations.

## Dataset

The project works with volumetric microscopy data stored in Zarr format.

An example dataset has the following dimensions:

| Property | Value |
|---|---|
| Number of timepoints | 100 |
| Z-axis slices per timepoint | 64 |
| Image dimensions | 256 × 256 pixels |
| Data type | `uint16` |
| Storage format | Zarr |

Each timepoint contains a 3D volume, enabling cell detection in spatial coordinates across a temporal sequence.

## Methodology

### 1. Data Exploration and Preprocessing

- Inspected volumetric microscopy data and its dimensional structure.
- Examined Zarr arrays, image intensity values, and spatial calibration.
- Accounted for anisotropic voxel spacing when calculating physical distances.

### 2. Cell Detection

Implemented and evaluated a baseline approach for identifying candidate cell locations in 3D image volumes using image-processing-based detection.

Experiments explored Laplacian of Gaussian (LoG) detection and parameter tuning to identify candidate cell centers.

### 3. Spatial Calibration

Investigated the effect of anisotropic voxel spacing on spatial distances. Physical-space calibration is important because equal distances in voxel coordinates do not necessarily represent equal distances in the underlying specimen.

### 4. Temporal Association

Explored cell association across successive timepoints using spatial proximity and the Hungarian assignment algorithm.

The assignment approach provides a way to match detections between frames while enforcing one-to-one assignments under the selected cost formulation.

### 5. Tracking Graph Construction

Represented the output as a graph consisting of:

- **Nodes:** Detected cell instances at specific timepoints.
- **Edges:** Proposed temporal links between cell instances.

This representation supports the construction and validation of cell-tracking submissions.

### 6. Submission Generation and Validation

Developed a pipeline to generate a competition-compatible CSV containing cell nodes and temporal links.

The submission workflow included checks for:

- Required columns and valid row structure.
- Sequential row identifiers.
- Unique node identifiers.
- Valid source and target references for edges.
- Missing values and dataset coverage.

## Repository Structure

```text
Biohub-cell-tracking/
├── docs/                 # Project documentation
├── experiments/          # Experiments and evaluation
├── kaggle-notebooks/     # Kaggle notebooks and workflows
├── papers/               # Related research papers
├── src/                  # Source code
└── README.md
```

## Technology Stack

- **Language:** Python
- **Deep Learning:** PyTorch
- **Computer Vision:** 3D image processing and cell detection
- **Tracking:** Spatial association and Hungarian assignment
- **Data Handling:** NumPy and Zarr
- **Environment:** Kaggle Notebooks
- **Version Control:** Git and GitHub

## Experiments and Results

The project evaluated baseline cell detection and tracking approaches on volumetric microscopy data.

- Achieved approximately **0.807 detection recall** in a local evaluation experiment using a sampled time sequence and the specified matching procedure.
- Generated a structurally validated submission covering four test datasets.
- Produced a submission containing **5,996 nodes and 4,572 edges**, totaling 10,568 rows.
- Explored the difference between local detection performance and end-to-end competition performance.

The local detection recall and competition score measure different aspects of the pipeline and should not be interpreted as equivalent metrics.

## Limitations

- Reliable cell detection does not guarantee correct temporal associations.
- Simple spatial matching can struggle when cells move substantially between timepoints.
- Cell division, missed detections, false positives, and changes in cell appearance complicate tracking.
- End-to-end performance depends on both detection quality and the accuracy of temporal links.
- The baseline leaves room for more advanced motion modeling, division-aware tracking, and learned detection methods.

## Future Improvements

- Investigate adaptive motion models for temporal association.
- Compare nearest-neighbor linking with Hungarian assignment and motion-aware matching.
- Improve robustness to missed detections and false positives.
- Incorporate explicit cell-division handling.
- Evaluate detection and tracking separately using appropriate metrics.
- Explore learning-based approaches for improved 3D cell detection and temporal tracking.

## Learning Outcomes

This project provided practical experience with:

- 3D biomedical image analysis.
- Volumetric data processing.
- Object detection and multi-object tracking.
- Spatial calibration and assignment algorithms.
- Experiment design and evaluation.
- Building and validating structured machine learning submissions.

## Acknowledgements

This project was developed in the context of a Biohub cell-tracking challenge, using the associated microscopy data and evaluation framework.

