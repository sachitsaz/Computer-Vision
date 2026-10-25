# 🔍 Depth Anything
### Monocular Depth Estimation & Video-Based Proximity Warning

---

## 📌 Overview

This project explores monocular depth estimation using **Depth Anything V2**, from single-image analysis to video processing and a depth-based proximity warning system. It investigates depth-map generation, numerical analysis, model accuracy, computational efficiency, visualization, near/far object comparison, and video-based warning generation.

The project consists of seven phases, progressing from fundamental depth estimation to a practical application for identifying potentially nearby objects in driving footage.

---
## 📂 Repository Structure

The `Depth_Anything/` folder contains the code, documentation, and experimental outputs for all seven phases.

| Folder / Content | Description |
|---|---|
| **Code and notebooks** | Implementations of the depth-estimation experiments and warning system. |
| 📑 `Depth_Anything_Reports/` | Detailed methodology, explanations, analysis, and conclusions for each phase. |
| 🖼️ `Depth_Anything_Results/` | Generated depth maps, visualizations, comparison results, processed videos, and other experimental outputs. |

Please refer to the corresponding report for the detailed explanation of each phase and to the results folder to inspect its outputs.

---

## 🧭 Project Roadmap

| Phase | Topic | Main Focus |
|:---:|---|---|
| 1 | 🖼️ Single-Image Depth Estimation | Generating depth maps |
| 2 | 📊 Numerical Analysis | Understanding depth values |
| 3 | ⚖️ Small vs. Base Comparison | Accuracy and efficiency |
| 4 | 🎞️ Video Depth Estimation | Frame-by-frame processing |
| 5 | 🎨 Depth Visualization | RGB and depth overlays |
| 6 | 📏 Near/Far Object Analysis | Comparing selected regions |
| **7** | **🚗 Proximity Warning System** | **Video-based warning generation** |

---

## 🖼️ Phase 1 — Single-Image Depth Estimation

Applied Depth Anything V2 Small to an RGB image from the Depth8 dataset to generate and visualize a depth map.

- Loaded and processed an input image.
- Generated and displayed the predicted depth map alongside the original image.
- Saved the resulting depth visualization.
- Examined the spatial information represented by the prediction.

---

## 📊 Phase 2 — Numerical Analysis of a Depth Map

Analyzed the predicted depth map using NumPy and visualized the distribution of its values.

- Calculated the map's shape, data type, minimum, maximum, mean, and standard deviation.
- Identified the pixel coordinates of the minimum and maximum values and examined the center pixel.
- Plotted a histogram of predicted depth values.
- Examined the distinction between relative depth and metric depth.

> **💡 Key Finding**
>
> Depth Anything V2's standard model output represents relative, inverse-depth-like information rather than directly calibrated physical distances. Predicted values cannot automatically be interpreted as distances in meters.

---

## ⚖️ Phase 3 — Small vs. Base Model Comparison

Compared Depth Anything V2 Small and Base using all eight available RGB-D image pairs from the Depth8 dataset. The predicted maps were aligned with the metric ground-truth maps using a scale-and-shift transformation before calculating the error metrics.

### 📈 Accuracy and Performance

| Metric | Small | Base |
|:---|---:|---:|
| MAE (m) | 0.1042 | 0.0985 |
| RMSE (m) | 0.1311 | 0.1247 |
| AbsRel | 0.0672 | 0.0634 |
| Total processing time (ms) | 141.08 | 185.70 |
| FPS | 7.51 | 5.91 |
| Peak GPU memory (MB) | 257.40 | 628.69 |

> **💡 Key Finding**
>
> Base achieved slightly better accuracy but required more processing time and GPU memory. Small offered a better balance between accuracy and computational efficiency. Neither model achieved the target of 20 FPS on the tested hardware.

*Note: The evaluation used eight images, so the results should be interpreted as a small-scale experiment rather than a comprehensive benchmark.*

---

## 🎞️ Phase 4 — Video Depth Estimation

Applied Depth Anything V2 Small frame by frame to the `freiburg1_xyz` sequence from the TUM RGB-D dataset.

- Generated a depth map for each of the 798 frames.
- Converted the predicted maps into colorized visualizations.
- Saved the frame-by-frame depth output as a video.
- Measured inference and end-to-end processing performance.

### ⚡ Processing Performance

| Metric | Result |
|:---|---:|
| Video resolution | 640 × 480 |
| Number of frames | 798 |
| Input frame rate | 30 FPS |
| Average inference time | 60.01 ms/frame |
| Inference speed | 16.66 FPS |
| End-to-end processing speed | 13.23 FPS |

> **💡 Key Finding**
>
> The model generated depth maps throughout the video, but the processing pipeline did not achieve real-time performance at the input rate of 30 FPS.

---

## 🎨 Phase 5 — Depth Visualization and RGB Overlay

Explored different ways of combining RGB images with colorized depth maps using the PLASMA color map.

- Tested alpha values of 0.2, 0.4, 0.5, 0.7, and 0.8.
- Examined the trade-off between original image details and depth visibility.
- Added an interactive alpha slider to adjust the overlay.
- Selected an alpha value of 0.5 as a balance between RGB appearance and depth information.

> **💡 Key Finding**
>
> An overlay is useful for combining scene appearance with depth information. However, displaying the original RGB image and colorized depth map side by side is preferable when comparing near and far objects because it preserves the details of the original scene.

---

## 📏 Phase 6 — Near and Far Object Analysis

Investigated whether Depth Anything V2 could distinguish objects at different distances using manually selected regions of interest (ROIs) in `image_08.png` from the Depth8 dataset.

Three regions were selected to represent a near object, an object at medium distance, and a far object. Predicted depth statistics were calculated and compared with the dataset's metric ground truth.

### 🔎 Comparison of Selected Regions

| Selected region | Mean predicted relative depth | Mean GT depth (m) |
|:---|---:|---:|
| A — Near | 6.636 | 0.751 |
| B — Medium | 2.328 | 1.473 |
| C — Far | 1.340 | 1.842 |

> **💡 Key Finding**
>
> The predictions preserved the near-to-far ordering, but their numerical trend was opposite to the metric ground-truth distances. This illustrates why relative-depth values must not be treated as physical distances without appropriate calibration.

---

# 🚗 Phase 7 — Depth-Based Proximity Warning System

> **⭐ Main Application of the Project**

## 🎯 Objective

Developed a video-based proximity warning system using **Depth Anything V2 Metric Small (ViT-S)** to estimate depth and warn when potentially nearby scene content is detected within a selected region of interest.

Unlike the earlier phases, which focus primarily on depth estimation, visualization, and analysis, this phase integrates depth estimation into a warning pipeline that operates across consecutive video frames.

### 🎥 Dataset and Input Video

The experiment used driving footage from **Day 11 of the Roadscapes dataset**. The original video was trimmed to the interval from 6 to 50 seconds.

| Property | Value |
|:---|:---|
| Video duration | 44 seconds |
| Number of frames | 1,320 |
| Resolution | 1280 × 720 |
| Input frame rate | 30 FPS |
| Model | Depth Anything V2 Metric Small (ViT-S) |

### 🔄 Methodology

The processing pipeline is:

**Input video → Depth estimation → Depth map → ROI analysis → Proximity decision → Warning output**

For each frame, the system generates a depth map and evaluates a selected region of interest.

- **ROI selection:** The ROI covers 80% of the image width (10%–90%) and 70% of its height (25%–95%).
- **Proximity indicator:** The 10th percentile (P10) of valid depth values within the ROI is calculated to represent the nearer end of the estimated depth distribution.
- **Warning threshold:** A threshold of 8 meters was selected based on preliminary tests.
- **Temporal stabilization:** The warning is activated after three consecutive frames below the threshold and cleared after three consecutive frames above it.
- **Warning display:** When active, the video displays `WARNING! OBJECT TOO CLOSE`.
- **Result recording:** Depth statistics and warning status are recorded for every frame in a CSV file.

The three-frame rule helps prevent warning transitions caused by a single fluctuating prediction.

### 📈 Results

The system generated a proximity-warning video, a depth visualization video, and a CSV file containing results for all 1,320 frames.

| Metric | Result |
|:---|---:|
| Average processing speed | 4.90 FPS |
| Warning activations | 8 |
| Warning clearances | 7 |
| Frames with warning active | 759 (57.5%) |
| Total warning-active time | Approximately 25.3 s |
| P10 range | 3.08–9.33 m |
| Mean P10 | Approximately 7.41 m |

The first warning was activated at approximately 0.07 seconds. Additional activations occurred at approximately 1.53, 3.30, 3.77, 9.70, 13.80, 33.73, and 38.90 seconds. The warning remained active at the end of the video.

### 🧠 Interpretation

The results demonstrate that depth estimates can be incorporated into a video-processing pipeline to generate proximity warnings. The ROI-based P10 statistic provides a simple indicator of nearby scene content, while the consecutive-frame rule stabilizes warning transitions.

However, the warning was active during 57.5% of the processed frames. This indicates that the selected 8-meter threshold produces frequent warnings in this particular video and requires further calibration before practical use.

### ⚠️ Limitations

- **No explicit object detection or tracking:** The system analyzes depth statistics within an ROI rather than estimating the distance of a specifically identified object.
- **ROI sensitivity:** Different objects and scene regions within the ROI can influence the P10 value.
- **Threshold calibration:** The 8-meter threshold is experimental and is not a validated vehicle-safety distance.
- **Processing speed:** The average speed of 4.90 FPS is below the input rate of 30 FPS, so the current implementation does not operate in real time.
- **Depth estimation reliability:** Predictions may be affected by differences between the model's training domain and the driving footage.

### 🔭 Future Work

Potential improvements include:

- Integrating object detection and tracking for object-specific proximity estimation.
- Validating predicted distances against ground-truth measurements.
- Calibrating the warning threshold using systematic experiments.
- Optimizing inference and video processing to improve FPS.
- Evaluating the system across additional driving scenes and conditions.

> **🏁 Phase 7 Conclusion**
>
> The experiment demonstrates a complete prototype pipeline from video-based depth estimation to temporally stabilized proximity warnings. Although it is not yet suitable for real-time vehicle-safety use, it provides a foundation for further investigation of depth-based warning systems.

---

## 🏁 Overall Conclusions

The project progresses from fundamental monocular depth estimation to a video-based proximity warning application. The experiments demonstrate the usefulness of depth maps for scene interpretation, model comparison, and near/far analysis, while also highlighting the differences between relative and metric depth.

The final phase demonstrates how depth estimates can support a warning mechanism over consecutive video frames. Further work is needed to improve processing speed, validate distance estimates, calibrate warning thresholds, and increase reliability before such a system could be considered for safety-critical applications.
