Depth Anything — Results
This folder contains the main experimental outputs of the Depth Anything project, including model comparison results, per-image evaluation metrics, video depth estimation, and the proximity warning system.
1. Results Overview
⚖️ 1.1. Model Trade-off Analysis
Folder: DepthAnything_models_Trade-off_results/
This folder contains the quantitative evaluation results for Depth Anything V2 Small and Base on eight RGB-D image pairs from the Depth8 dataset.
File	Description
depth_anything_model_comparison.csv	Summary of model performance, including depth estimation errors, processing time, FPS, and GPU memory usage.
depth_anything_per_image_results.csv	Per-image evaluation results for a more detailed comparison between the Small and Base models.
The comparison evaluates the trade-off between depth estimation accuracy and computational efficiency. The Base model achieves slightly lower depth estimation errors, while the Small model offers faster processing and lower GPU memory consumption.
Note: The Depth8 dataset contains only eight image pairs in this experiment. Therefore, the results are useful for testing and comparing the implemented pipeline, but should not be interpreted as a comprehensive benchmark of the models.
🎞️ 1.2. Video Depth Estimation
The following GIF presents a short visualization of depth estimation on video frames using Depth Anything V2.
File: DepthAnything_videoOutput_short.gif

The experiment uses the TUM RGB-D dataset, specifically the freiburg1_xyz sequence. RGB frames are processed to estimate depth maps, allowing the model's output to be examined across a video sequence.
The GIF is a short project-generated visualization of the experiment, not an original depth video distributed by the dataset provider.
🚗 1.3. Proximity Warning System
Folder: DepthAnything_warning_sys/
This folder contains the outputs of the video-based proximity warning experiment using the Depth Anything V2 Metric Small model and Roadscapes driving footage.
File	Description
DepthAnything_ProximityWarning_results.csv	Quantitative results from the proximity warning experiment, including frame-level warning information and estimated proximity values.
Sequence_Day_11_metric_warning_36_to_42.gif	GIF visualization of the proximity warning system operating on the Roadscapes video sequence.

The system estimates metric depth, analyzes a selected region of interest (ROI), and uses the 10th percentile of valid depth values as a proximity indicator. A warning is activated when this value falls below the selected threshold for the required number of consecutive frames.
The threshold and warning logic were selected experimentally for this project. The system is intended as a computer vision demonstration and has not been validated as a safety-critical driving system.
________________________________________
2. Datasets
Three datasets were used in the project. Each supports a different stage of the experiments.
📚 2.1. Depth8 Dataset
Description
The Ultralytics Depth8 dataset is a compact collection of eight indoor RGB-D image pairs sampled from the SUN RGB-D dataset. It contains four training images and four validation images from Kinect v1 and Kinect v2 captures, paired with dense ground-truth depth maps.
Use in This Project
Depth8 was used for:
•	Phase 1: Single-image depth estimation.
•	Phase 2: Numerical analysis of predicted depth maps.
•	Phase 3: Comparison of Depth Anything V2 Small and Base.
•	Phase 6: Near, medium, and far object analysis using selected regions of interest.
Limitation
Depth8 is intended for pipeline testing and debugging, not meaningful benchmarking. Its eight images are insufficient for comprehensive model performance evaluation. Results obtained using this dataset should therefore be interpreted as small-scale experimental results.
Attribution and License
Source: Ultralytics Depth8 Dataset Documentation
The Ultralytics documentation does not specify a license for the Depth8 dataset, and the underlying SUN RGB-D dataset's license is not specified there either. Do not assume that the dataset is unrestricted for redistribution or reuse. Consult the original dataset sources for applicable permissions before redistributing or reusing the data.
________________________________________
🎞️ 2.2.  TUM RGB-D Dataset
Description
The TUM RGB-D dataset, provided by the Technical University of Munich, contains RGB-D sequences and ground-truth information for research in visual odometry and simultaneous localization and mapping (SLAM).
Use in This Project
The freiburg1_xyz sequence was used in:
•	Phase 4: Frame-by-frame depth estimation using Depth Anything V2 Small.
•	Phase 5: Visualization of colorized depth maps and RGB/depth overlays.
The video experiment processed 798 frames at a resolution of 640 × 480. The generated depth visualization is a project output, not the original TUM RGB-D depth video.
Example Result
The following GIF shows a short visualization of the predicted depth maps generated for this project.

Attribution and License
Dataset: TUM RGB-D Dataset, Technical University of Munich.
Official source: RGB-D SLAM Dataset and Benchmark
The official TUM RGB-D page states that, unless otherwise indicated, dataset data is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0). The accompanying source code has a separate BSD-2-Clause license.
When using or redistributing the original data, or materials derived from it, consult the official license information and provide appropriate attribution.
Dataset Paper
J. Sturm, N. Engelhard, F. Endres, W. Burgard, and D. Cremers.
"A Benchmark for the Evaluation of RGB-D SLAM Systems."
IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), 2012.

🚗 2.3. Roadscapes Video Dataset — Day 11
Description
The Roadscapes Video Dataset contains dashcam video clips from diverse real-world driving conditions.
For Phase 7, driving footage from Day 11 was processed using Depth Anything V2 Metric Small (ViT-S) to generate depth maps and evaluate an ROI-based proximity warning system.
The original footage was trimmed to the interval from 6 to 50 seconds, producing a 44-second clip with the following properties:
Property	Value
Duration	44 seconds
Number of frames	1,320
Resolution	1280 × 720
Input frame rate	30 FPS
Use in This Project
The experiment used the 10th percentile (P10) of valid depth values within a selected region of interest as a proximity indicator. Warnings were activated or cleared using an 8-meter threshold and a three-consecutive-frame rule.
The generated results include the proximity-warning video, depth visualization video, and a CSV file containing frame-by-frame results.
These are outputs of the project experiment, not original Roadscapes dataset files.
Attribution and License
Dataset: Roadscapes Video Dataset, specifically Day 11.
Source: Roadscapes Video Dataset on Hugging Face
The dataset card specifies the Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0).
This license requires appropriate attribution and restricts use to non-commercial purposes under its terms. Before redistributing or reusing the original footage or materials derived from it, review the full license and the original dataset card.
Dataset Citation
@dataset{roadscapes_video,
  author    = {vijpandaturtle},
  title     = {Roadscapes Video Dataset},
  year      = {2025},
  publisher = {Hugging Face},
  url       = {https://huggingface.co/datasets/vijpandaturtle/roadscapes-video}
}
________________________________________
3. Important Notes
•	Relative versus metric depth: The standard Depth Anything V2 model produces relative depth estimates, which should not automatically be interpreted as physical distances in meters. Metric depth estimation is used for the proximity warning experiment.
•	Evaluation scope: The quantitative model comparison uses only eight Depth8 image pairs and does not establish general performance across all scenes or datasets.
•	Computational performance: Processing speed depends on the model, hardware, input resolution, and processing pipeline. The reported FPS values should be interpreted in the context of the experimental setup.
•	Proximity threshold: The 8.0 m threshold is an experimental parameter, not a validated international safety standard or a universally applicable safe-distance threshold.
•	Dataset terms: Please consult the original dataset documentation and license information before redistributing dataset files or derived content. This repository's result files and visualizations should not be assumed to change the original datasets' licensing conditions.
4. Reproducibility
The CSV files provide quantitative results, while the GIFs offer visual examples of the model outputs. The accompanying notebooks and code in the main project directory document the experimental procedures and implementation.
For the complete workflow, refer to the main project README and the associated notebooks.


