# Vehicle Detection, Classification, and License Plate Recognition Pipeline

This repository presents a modular computer vision system for **vehicle detection, make classification, license plate detection, and optical character recognition (OCR)**.  
The pipeline integrates multiple deep learning models, image preprocessing, and structured annotation to create an end-to-end framework for automated vehicle analysis from video input.

---

## 1. Overview

The system processes videos through a sequence of independent modules, each handling a specific task:

1. **Frame Loader** – extracts video frames and timestamps, optionally skipping frames for faster processing.  
2. **Vehicle Detection** – detects vehicles using **YOLO11x (Ultralytics)** and outputs bounding boxes.  
3. **Make Classification** – classifies each detected vehicle using a custom-trained **YOLO11x-cls** model.  
4. **License Plate Detection** – detects plates within cropped vehicle images using **YOLOS-Small**.  
5. **OCR (Plate Recognition)** – extracts alphanumeric text from detected plates using **fast-plate-ocr** and regex filtering for Pakistani plate formats.  
6. **Night-Time Preprocessing (optional)** – enhances low-light frames using Gamma correction and CLAHE for better OCR performance.  
7. **Annotation and Output** – overlays detection and recognition results on the original frames to produce an annotated output video.

Each stage operates modularly, allowing for easy testing, replacement, and future scalability.

---

## 2. Pipeline Flow

Input Video → Frame Loader → Vehicle Detection (YOLO11x) → Vehicle Make Classification (YOLO11x-cls) → License Plate Detection (YOLOS-Small) → Night-Time Preprocessing (optional) → License Plate OCR (fast-plate-ocr + Regex Filter) → Annotation & Output Video

---

## 3. Module Performance Summary

| **Module** | **Model Used** | **Reported / Observed Accuracy** | **Evaluation Notes** |
|-------------|----------------|----------------------------------|----------------------|
| **Vehicle Detection** | YOLO11x (Ultralytics) | ~79.5 mAP<sub>50–95</sub> (COCO benchmark) | Strong general detection; consistent across multiple vehicle types. |
| **Vehicle Make Classification** | Custom YOLO11x-cls | 98.58% Top-1, 99.74% Top-5 | Trained for 48 epochs (batch 16) on a custom dataset; performs best on clear, large crops. |
| **License Plate Detection** | YOLOS-Small (nickmuchi) | ~49% AP (reported) | Successfully detects plates in most mid-range views; lower accuracy on distant or small crops. |
| **OCR (Plate Reading)** | fast-plate-ocr (ankandrew) | No fixed benchmark – strong qualitative performance | Performs well on clear and sharp plates; affected by motion blur or extreme lighting. |
| **Night-time Preprocessing (Optional)** | Gamma + CLAHE | Qualitative improvement | Enhances dimly lit scenes; glare and blackout remain challenging extremes. |

---

## 4. Datasets

| Dataset | Source | Access |
|----------|---------|--------|
| **Custom Vehicle Dataset** | Self-annotated via Roboflow | [Roboflow Project 🔗](https://universe.roboflow.com/aimlcv/car_make_classification-2) |
| **Hugging Face Dataset Copy** | Public hosted version | [Hugging Face Link 🔗](https://huggingface.co/datasets/em4bm4lik/car-make-classification-pk) |

---

## 5. Future Work

- Add object tracking (e.g., DeepSORT / ByteTrack) for consistent car IDs across frames.  
- Improve OCR robustness under glare and motion blur.  
- Develop adaptive preprocessing that automatically adjusts for brightness and reflection.  
- Build a web dashboard (Streamlit / FastAPI) for live monitoring and control.  

---

## 6. Acknowledgements

This project builds upon several open-source resources:

- [Ultralytics YOLO11](https://github.com/ultralytics/ultralytics) – vehicle detection and classification  
- [Nickmuchi YOLOS-Small](https://github.com/nickmuchi/yolos-license-plate) – license plate detection  
- [Ankandrew fast-plate-ocr](https://github.com/ankandrew/fast-plate-ocr) – OCR for plate reading  
- [Roboflow](https://roboflow.com) – dataset creation and annotation  
- [Hugging Face](https://huggingface.co) – dataset and model hosting  
