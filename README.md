# Master’s Thesis: Multimodal Feature Extraction and Fusion 
### Title: Learning Feature Extraction Techniques To Encode Complex NuScenes Dataset

This repository contains materials related to my Master’s thesis completed as part of the **MSc in Information Technology (Data Analytics)** at **Deakin University**.

## Overview
This thesis explores multimodal feature extraction and fusion techniques using the **NuScenes dataset**, with a focus on how information from heterogeneous sensors such as camera, LiDAR, and radar can be encoded and combined for perception tasks in autonomous driving systems.

The work examines representation learning pipelines and feature-level fusion strategies, and studies how design choices influence downstream perception performance. The emphasis is on understanding system behaviour and methodological trade-offs rather than achieving state-of-the-art results.

## Repository Structure
- `thesis/` contains the final submitted Master’s thesis in PDF format  
- `notebooks/` contains experimental and exploratory Jupyter notebooks used during the research  
- `data/` contains data for  data preprocessing, feature extraction, and model experimentation  
- `data/montage-image-example` examples of montage image created during preprocessing

## Notebooks and purpose

### 01 - NuScenes Dataset Transformation (Scene → Sample Level)
NuScenes Sample-wise Data Transformation
The original NuScenes dataset is organized at the scene level, which makes direct multimodal learning and batching challenging. To address this, the dataset was transformed into a sample-wise format where each row corresponds to a single timestamped sample. For every sample, file paths to associated sensor data—including camera images, LiDAR point clouds, radar point clouds, and pre-generated montage images—are stored as structured columns. This transformation enables efficient data loading, synchronized multimodal access, and consistent training across all models.

### 02 - Point Cloud–based Model (LiDAR + Radar)
Point Cloud Model
This model uses engineered features extracted from raw LiDAR and radar point clouds rather than operating directly on full point sets. For each sample, statistical summaries such as coordinate-wise sums and aggregated surface normal information are computed from LiDAR and multi-view radar sensors. These features are concatenated into a fixed-length vector and passed through a deep fully connected network with batch normalization and dropout. The model serves as a lightweight baseline to evaluate how much discriminative power can be obtained from compressed geometric representations of point cloud data.

### 03 - Image-based Model
Image Model
The image-based model processes RGB camera inputs using a convolutional neural network composed of standard and depthwise separable convolutions with residual connections. The architecture progressively extracts spatial features from 128×128 images and aggregates them using global average pooling before classification. This model captures visual scene context and acts as a unimodal reference for evaluating the contribution of camera data in isolation.

### 04 - Continuous Model
Continuous Feature Model
This model operates on low-dimensional continuous inputs derived from scene-level or sensor metadata. The features are processed using a stack of fully connected layers with batch normalization and dropout to reduce overfitting. Although simple in structure, this model provides an important baseline to quantify how much predictive signal is contained in non-visual, non-geometric data alone.

### 05 - Multimodal Fusion Model (Images + Point Clouds + Continuous Data)
Multimodal Fusion Model
The multimodal model combines camera images, engineered point cloud features, and continuous inputs into a unified architecture. Each modality is first processed by a dedicated sub-network tailored to its data characteristics. The intermediate predictions from each branch are then concatenated and refined using additional fully connected layers to produce the final output. This design enables late-stage feature fusion and allows the model to learn complementary information across heterogeneous sensor modalities.



## Notes
The code included in this repository reflects the exploratory nature of applied academic research. Due to dataset licensing constraints, environment differences, and hardware limitations, full reproducibility is not guaranteed. This repository is intended for transparency and reference purposes rather than production use.