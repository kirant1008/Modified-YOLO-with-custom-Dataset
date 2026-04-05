# Modified YOLOv3 with Custom Dataset

A PyTorch implementation of YOLOv3 object detection and multi-object tracking, trained on a custom **Person + Dog** dataset built from Open Images and COCO.

The network architecture is a lighter variant of YOLOv3 (starting at 16 filters instead of 32) with 3 detection scales and 9 anchors, configured for 416x416 input.

## Features

- **Training** (`train_oid.py`) — Fine-tune from pretrained YOLOv3 weights on your custom dataset
- **Image detection** (`object_tracker_image.py`) — Run inference on a single image with bounding box visualization
- **Video tracking** (`object_tracker_video.py`) — Real-time detection + SORT multi-object tracking on video
- **SORT tracker** (`sort.py`) — Kalman filter-based tracking with Hungarian assignment

## Project Structure

```
.
├── config/
│   ├── edit.cfg              # Modified YOLOv3 network architecture (2 classes)
│   ├── classes.names         # Class labels
│   ├── custom_data.data      # Dataset config
│   └── train.txt / test.txt  # Image path lists
├── utils/
│   ├── datasets.py           # PyTorch datasets with padding/resizing
│   ├── utils.py              # NMS, IoU, target building, AP computation
│   └── parse_config.py       # .cfg and .data file parsers
├── models.py                 # Darknet/YOLOv3 model definition
├── train_oid.py              # Training script
├── object_tracker_image.py   # Single-image inference
├── object_tracker_video.py   # Video inference + tracking
├── sort.py                   # SORT multi-object tracker
└── pyproject.toml            # Dependencies (managed with uv)
```

## Setup

This project uses [uv](https://docs.astral.sh/uv/) for dependency management.

```bash
# Install uv (if not already installed)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Install all dependencies
uv sync
```

### Dependencies

torch, torchvision, numpy, Pillow, scikit-image, scipy, filterpy, numba, opencv-python, matplotlib

## Building a Custom Dataset

### From COCO

1. Download COCO annotations and images (2017 or 2014)
2. Set the annotation and image folder paths in `extract.py`
3. Run `extract.py` to get a custom dataset in YOLO format

### From Open Images Dataset

1. Install [OIDv4 ToolKit](https://github.com/EscVM/OIDv4_ToolKit.git)
2. Extract specific classes and their labels using OID
3. Convert annotations to YOLO format: run `extracting_and_converting_annotations.py`
4. Generate train/test splits: set image paths in `creating-train-test-txt-files.py`
5. Generate `.names` and `.data` files: set image paths in `creating-files-data-and-names.py`
6. Copy the `.data` and `.names` files into the `config/` folder

## Training

1. Update the paths in `train_oid.py` arguments to point to your dataset, config, and weight files
2. Run training:

```bash
uv run python train_oid.py \
  --image_folder /path/to/images \
  --model_config_path config/edit.cfg \
  --data_config_path config/custom_data.data \
  --weights_path config/yolov3.weights \
  --class_path config/classes.names \
  --epochs 30
```

Checkpoints are saved to `checkpoints_md_custom/` after each epoch.

## Inference

Update the config, weights, and class paths at the top of the inference scripts, then run:

```bash
# Single image
uv run python object_tracker_image.py

# Video with tracking
uv run python object_tracker_video.py
```

## References

- [OIDv4 ToolKit](https://github.com/EscVM/OIDv4_ToolKit.git)
- [COCO class extraction](https://github.com/AlphaArslan/ML_COCO_extract_specific_classes)
- [PyTorch custom YOLO training](https://github.com/cfotache/pytorch_custom_yolo_training)
- [Training YOLO for Object Detection in PyTorch](https://medium.com/data-science/training-yolo-for-object-detection-in-pytorch-with-your-custom-dataset-the-simple-way-1aa6f56cf7d9)
- [Object Detection and Tracking in PyTorch](https://medium.com/data-science/object-detection-and-tracking-in-pytorch-b3cf1a696a98)
