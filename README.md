# Intelligent-Cigarette-Butt-Detection-and-Localization-Using-YOLOv8-Segmentation

YOLOv8-based dataset and code for detecting and localizing cigarette butts on university campuses. Includes 2,000 annotated images (OBB format), training scripts, and benchmark results for YOLOv8n/s/m/l segmentation models

A custom dataset of cigarette butt images collected across a university/college campus, created for the paper:

> **Intelligent Cigarette Butt Detection and Localization in University Campuses Using YOLOv8 Segmentation**
> *Author(s), IEEE Access, 2026 (under review).*
> DOI: *to be added upon publication*

**CigWasteNet** is designed for **tiny-object litter detection** research and supports training and benchmarking of YOLOv8 models for automated cigarette butt monitoring, targeted campus clean-up planning, and evaluation of smoke-free policies.

---

## 📊 Dataset Summary

| Property                             | Value                                     |
| ------------------------------------ | ----------------------------------------- |
| Dataset name                         | CigWasteNet                               |
| Original images collected            | 1,500                                     |
| Total images (after Roboflow export) | 2,000                                     |
| Annotated images                     | 2,000                                     |
| Classes                              | 2 *(see `data.yaml`)*                     |
| Annotation type                      | YOLOv8 Oriented Bounding Box (OBB)        |
| Annotation tool                      | Roboflow (manual annotation, 1 annotator) |
| Image format                         | PNG (RGB)                                 |
| Annotation format                    | .yaml                                     |
| Original image resolution            | 96 DPI *(1080 pixel)*                     |
| Training input size                  | 640 × 640                                 |
| Objects per image                    | min 1 · avg ≈ 5 · max 9                   |
| Duplicates removed                   | 19                                        |
| Distribution format                  | ZIP archive                               |

### Train / Validation / Test Split

| Subset     | Images    | Share |
| ---------- | --------- | ----- |
| Train      | 1,508     | 75.4% |
| Validation | 392       | 19.6% |
| Test       | 100       | 5.0%  |
| **Total**  | **2,000** | 100%  |

**Scene independence:** images from the same physical scene are kept within a single subset only, so no scene leaks across train/validation/test.

---

## 📷 Collection Conditions

- **Location:** multiple micro-environments across a university campus — entrances, corridors, staircases, courtyards, parking areas, sidewalks, roadsides, and informal gathering spots.
- **Devices:** multiple smartphones (Google Pixel, Samsung, OnePlus).
- **Collection period:** approximately 6–12 months.
- **Lighting:** daytime and evening.
- **Weather:** sunny and cloudy conditions.
- **Backgrounds:** concrete, grass, dirt/soil, asphalt, pavements, tiles, stairs, and leaf-covered ground.
- **Viewpoints:** vertical (top-down), horizontal, and diagonal/angled views.
- **Occlusion:** fully visible, partially occluded, and overlapping/layered cigarette butts.

### Known Challenges

Blurry or extremely small butts, unusual camera angles, visually similar background objects (rocks, leaves, paper fragments), dense overlapping clusters, and thin/elongated partially occluded instances.

---

## 📁 Repository / ZIP Structure

The dataset is distributed as a ZIP archive exported from Roboflow in **YOLOv8 Oriented Bounding Box** format:

```
CigWasteNet.zip
├── data.yaml            # class names, paths, and dataset configuration
├── README.md            # this file
├── train/
│   ├── images/          # 1,508 PNG images
│   └── labels/          # one .txt label file per image
├── valid/
│   ├── images/          # 392 PNG images
│   └── labels/
└── test/
    ├── images/          # 100 PNG images
    └── labels/
```

### Label Format (YOLOv8 OBB)

Each line in a label file describes one object using the four corner points of its oriented bounding box, normalized to `[0, 1]`:

```
class_index x1 y1 x2 y2 x3 y3 x4 y4
```

Class indices and names are defined in `data.yaml`.

---

## 🚀 Quick Start

### 1. Install Ultralytics

```bash
pip install ultralytics
```

### 2. Unzip the dataset

```bash
unzip CigWasteNet.zip -d CigWasteNet
```

### 3. Train a YOLOv8 model

```python
from ultralytics import YOLO

model = YOLO("yolov8l-obb.pt")   # or yolov8n/s/m/x variant

model.train(
    data="dataset/CigWasteNet.yolov8-obb/data.yaml",
    epochs=100,
    imgsz=640,
    batch=6,
)
```

### 4. Validate / Predict

```python
metrics = model.val(data="dataset/CigWasteNet.yolov8-obb/data.yaml")

results = model.predict("path/to/campus_image.png", conf=0.25)
results[0].show()
```

---

## 🏋️ Baseline Benchmark Results

All models were fine-tuned from COCO-pretrained weights for 100 epochs at 640 × 640 with default Ultralytics hyperparameters. Reported on the validation set (see the paper for full details):

| Model                  | Precision | Recall    | mAP@0.5   | mAP@0.5:0.95 (Box) | mAP@0.5:0.95 (Mask) |
| ---------------------- | --------- | --------- | --------- | ------------------ | ------------------- |
| YOLOv8n-seg            | 0.802     | 0.798     | 0.811     | 0.784              | 0.762               |
| YOLOv8s-seg            | 0.816     | 0.809     | 0.824     | 0.801              | 0.778               |
| YOLOv8m-seg            | 0.839     | 0.827     | 0.846     | 0.823              | 0.801               |
| **YOLOv8l-seg**        | **0.852** | **0.841** | **0.858** | **0.846**          | **0.829**           |

YOLOv8x-seg offered only marginal accuracy gains over YOLOv8l-seg at significantly higher computational cost.

---

## 📄 License

This dataset is released under the **MIT License**, copyright © 2026 **ICARL**. See the [`LICENSE`](LICENSE) file for full terms.

## 🙏 Citation

If you use this dataset in your research, please cite:

```bibtex
@article{cigwastenet2026,
  title   = {Intelligent Cigarette Butt Detection and Localization in
             University Campuses Using YOLOv8 Segmentation},
  author  = { Eng.Muhammad Kabir Ahmad , Prof.Dr.Usman Ghani Khan and Prof.Muhammad Waseem},
  journal = {IEEE Access},
  year    = {2026},
  note    = {Dataset: CigWasteNet}
}
```

## 📬 Contact

For questions about the dataset, please open an issue in this repository or contact the corresponding author.
