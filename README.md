# Real-Time Vehicle Detection & Overtaking Safety Assessment System

## Overview
This project implements **two end-to-end vehicle detection pipelines** for real-time traffic surveillance and driving analytics:
1. **YOLOv8-based System** – Optimized for high detection accuracy, real-time inference, and advanced overtaking safety classification.
2. **RetinaNet (ResNet-50 FPN) Pipeline** – A modular, customizable detection and analytics framework with distance and overtaking angle estimation.

Both approaches aim to detect vehicles from road camera footage, compute additional driving metrics, and assist in traffic monitoring, accident prevention, and driver assistance.

---

## Objectives
- Detect vehicles in real-time from traffic surveillance or dashcam footage.
- Estimate the **distance** of each detected vehicle from the camera.
- Calculate **overtaking angles** and assess overtaking safety.
- Evaluate performance with **precision, recall, mAP, distance accuracy, and angle accuracy**.
- Provide an annotated output video with bounding boxes, distances, angles, and safety labels.

---

## System 1 – YOLOv8-Based Pipeline

### Model
- **Architecture:** YOLOv8n (fused)
- **Parameters:** 3.15M  
- **GFLOPs:** 8.7  
- **Trained on:** Custom dataset of road vehicle classes (Car, Motorcycle, Bus, Truck).

### Performance Summary
| Metric                | Value    |
|-----------------------|----------|
| Precision             | **0.909**    |
| Recall                | **0.953**    |
| F1 Score              | **0.930**    |
| mAP@0.5               | **0.818**    |
| Mean Distance Error   | **2.81 m**   |
| Mean Angle Error      | **1.62°**    |
| Safety Classification ROC AUC | **0.594** |

**Vehicle Type Counts:**  
- Car: 698  
- Motorcycle: 485  
- Bus: 147  
- Truck: 461  

**Confidence Threshold Analysis:**  

| Threshold | Precision | Recall | F1   |
|-----------|-----------|--------|------|
| 0.3       | 0.909     | 0.953  | 0.930|
| 0.5       | 0.909     | 0.953  | 0.930|
| 0.7       | 0.910     | 0.953  | 0.931|
| 0.9       | 0.923     | 0.960  | 0.941|

---

## System 2 – RetinaNet (ResNet-50 FPN) Pipeline

### Model
- **Architecture:** RetinaNet (torchvision implementation)
- **Backbone:** ResNet-50 FPN
- **Pretrained On:** COCO Dataset
- **Target Classes:**
  - Car (COCO ID 3)
  - Motorcycle (COCO ID 4)
  - Bus (COCO ID 6)
  - Truck (COCO ID 8)

### Workflow
1. **Video Input**
   - Simulated dashcam footage (`sample_video.mp4`).
2. **Batch Processing**
   - Frames processed in batches of 8 for efficiency.
3. **Detection**
   - Outputs bounding boxes, class labels, and confidence scores.
4. **Distance Estimation**
   - Based on bounding box width + camera calibration.
5. **Overtaking Angle Calculation**
   - Difference between vehicle center and frame center.
6. **Visualization**
   - Bounding boxes in yellow with labels:
     ```
     [Class], D: [Distance m], A: [Angle°] | Acc: [Accuracy%]
     ```
