# 🏆 Geo-AI Hackathon | Theme 1

**Project:** Automated Feature Extraction for SVAMITVA Scheme using Advanced Computer Vision  
**Team ID:** Team_Nati-250448_Theme1  
**Team:** Adil Mahajan, Tavishi Amla, Divya Verma, Sarnish Kour  
**Status:** Complete and deployment-ready

## ⚡ Quick Results

| Metric | Value | Why It Matters |
|---|---:|---|
| Mean Validation IoU | ~91.9% | Strong overall segmentation quality for deployment workflows |
| Background Accuracy | ~97% | Stable separation of non-target regions in large orthophotos |
| Waterbody Accuracy | ~97% | Reliable extraction of high-impact natural assets |
| Rare Infrastructure Recall | Improved via specialist branch (about 77% in infra-centric evaluation) | Better detection of small, high-value assets often missed by baseline models |
| Output Type | Geo-referenced shapefiles (`.shp`) | Direct GIS integration for cadastral and planning pipelines |

---

## 🌍 Introduction

The SVAMITVA scheme is generating high-resolution rural drone imagery at scale, but manual GIS digitization remains slow, expensive, and inconsistent. This project builds an end-to-end Geo-AI pipeline that converts drone orthophotos into GIS-ready feature layers for practical land governance workflows.

---

## ❗ Problem Statement

India's rural drone imagery volume is growing rapidly, but converting raw imagery into usable cadastral intelligence is a bottleneck.

Key challenges:
- Manual feature extraction is time-consuming and difficult to scale nationally.
- Small infrastructure assets occupy less than 1% of pixels.
- Severe class imbalance biases models toward dominant classes.
- Legal and administrative workflows require high segmentation reliability.

---

## 🎯 Objectives

- Accurately segment rural features from SVAMITVA orthophotos.
- Improve learning for long-tail classes (especially infrastructure).
- Generate GIS-ready outputs that can be integrated directly.
- Build a pipeline that scales from village to district/state deployments.

---

## 🧭 End-to-End System Overview

1. Input geospatial orthophotos (`.tif`)
2. Automated preprocessing and tiling (`512 x 512`)
3. Deep learning semantic segmentation
4. Confidence-aware inference and post-processing
5. Geo-referenced vector output generation (`.shp`)

Primary extracted classes include:
- Buildings and roof types (RCC, tiled, tin)
- Roads (paved and unpaved)
- Water bodies (ponds, tanks, reservoirs)
- Infrastructure (wells, overhead tanks, distribution transformers)

---

## 🧠 Model and Training Strategy

### Core Architecture
- **DeepLabV3+** with **ResNet50** backbone for robust multi-scale segmentation.
- ASPP improves context capture for both large regions (water) and smaller objects.

### Imbalance Handling
- Weighted oversampling for rare-class-rich tiles (up to 20x sampling preference).
- Hybrid loss with Focal Loss (`gamma = 2.0`) and Dice Loss for hard-example focus and overlap optimization.

### Optimization Setup
- Optimizer: AdamW
- Learning rate: `1e-4`
- Batch size: `16`
- Training duration: up to `118` epochs with validation monitoring

### Specialist Extension
- A specialist branch is used for infra-centric recovery in highly imbalanced scenarios.
- This improves recall for rare infrastructure signals while maintaining strong general-class performance.

---

## 📈 Performance Snapshot

- Mean validation IoU: approximately **91.9%**
- Strong class-wise accuracy for dominant classes such as background and water (around **97%**)
- Improved rare-class sensitivity after focused sampling and specialist modeling
- Qualitative outputs show clean boundaries and strong road/building continuity

---

## 📸 The Visual Evidence 

We have generated a comprehensive visual portfolio to prove the model's robustness in the real world.

### 1. The "Complexity" Proof (Overlapping Classes)
*   **Evidence:** `comparison_complex_scene_1.png` to `_3.png` and `comparison_complex_scene_batch_1.png` to `_10.png`
*   **What it shows:** The model successfully disentangles complex interactions in a single tile. We found scenes containing up to **5 distinct classes** (Road + Building + Water + Infra + Background) co-existing perfectly.

### 2. The "Water" Proof
*   **Evidence:** `comparison_complex_scene_water.png`
*   **What it shows:** A rare, high-density example containing distinct water bodies alongside urban features. The model correctly segments Water (Blue) from Road (Gray) and Vegetation (Black).

### 3. The "Class Atlas"
*   **Evidence:** `class_comparisons/` folder.
*   **What it shows:** A dedicated "Best Case" example for every single class:
    *   `comparison_Building.png`
    *   `comparison_Road.png`
    *   `comparison_Infra.png` (The Specialist's victory!)
    *   `comparison_RCC.png` vs `comparison_Tiled.png` (Roof type distinction).

### 4. Rendered Output Gallery

Below are direct model output visualizations embedded from `outputs/`.

#### Core Comparison Views
![Complex Scene 1](outputs/comparison_complex_scene_1.png)
![Complex Scene 2](outputs/comparison_complex_scene_2.png)
![Complex Scene 3](outputs/comparison_complex_scene_3.png)

#### Waterbody + Mixed-Class Case
![Complex Scene Water](outputs/comparison_complex_scene_water.png)
![Complex Scene With Legend](outputs/comparison_complex_scene_with_legend.png)

#### Additional Batch Samples
![Batch Sample 1](outputs/comparison_complex_scene_batch_1.png)
![Batch Sample 5](outputs/comparison_complex_scene_batch_5.png)
![Batch Sample 10](outputs/comparison_complex_scene_batch_10.png)

---

## 📂 Submission Contents

This folder contains the complete deliverables:

### 1. The Code 💻
*   **`inference_merged.py`**: The core logic. Runs the Generalist + Specialist fusion.
*   **`train_full_scale.py`**: The main training loop (DeepLabV3+).
*   **`train_infra_specialist.py`**: The dedicated "Infra" training loop.

### 2. The Models 🧠
*   **`best_model_multiclass.pth`**: The Generalist weights (ResNet50).
*   **`infra_specialist.pth`**: The Specialist weights (ResNet18).

### 3. The Documentation 📄
*   **`final_project_success_report.md`**: High-level executive summary and metrics.
*   **`workflow_methodology_report.md`**: Deep dive into the "Layer Fusion" and algorithms.
*   **`comprehensive_metrics_report.md`**: Raw numbers, loss curves, and training logs.

---

## 🚀 How to Run

1.  **Setup:** Install requirements (`pip install -r requirements.txt`).
2.  **Inference:** Run `python inference_merged.py`.
    *   Input: `server 2/processed_data_multiclass/images/`
    *   Output: `final_predictions_moe/`
3.  **Visualization:** Check `final_predictions_highlights/` to see the filtered "Best of" results.

---

## 🗺️ Deployment and Impact

- Produces GIS-consumable outputs for downstream cadastral integration.
- Reduces manual digitization effort and turnaround time.
- Improves consistency of feature mapping across survey regions.
- Supports future scaling for district and state-level processing workloads.

---

## 🔭 Current Limitation and Future Work

Current limitation:
- Micro-infrastructure detection remains difficult due to tiny object footprints.

Planned improvements:
- Integrate LiDAR/height priors where available.
- Add active learning with human-in-the-loop correction.
- Expand asset taxonomy for broader rural planning applications.

---
