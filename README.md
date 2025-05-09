# PaDiM-Anomaly-Detection
This is an implementation of the paper [PaDiM: a Patch Distribution Modeling Framework for Anomaly Detection and Localization](https://arxiv.org/pdf/2011.08785).   

Some parts of the code are borrowed from both SPADE-pytorch(https://github.com/byungjae89/SPADE-pytorch) and MahalanobisAD-pytorch(https://github.com/byungjae89/MahalanobisAD-pytorch) projects
<p align="center">
    <img src="imgs/pic1.png" width="1000"\>
</p>

## Requirement
* python == 3.13.3
* pytorch == 2.6
* cuda == 12.6

## Environment Setup
You can set up the required environment using the provided `env.yaml` file:

```bash
# Create conda environment from yaml file
conda env create -f env.yaml

# Activate the environment
conda activate padim
```

## Usage

### Command Line Arguments

The script accepts the following command-line arguments:

| Argument | Description | Default |
|----------|-------------|---------|
| `--data_path` | Path to the dataset directory | `data` |
| `--save_path` | Path to save outputs | `./output` |
| `--arch` | Model architecture (`resnet18` or `wide_resnet50_2`) | `wide_resnet50_2` |
| `--batch_size` | Batch size for data loading | `16` |
| `--seed` | Random seed for reproducibility | `1024` |
| `--img_resize` | Image resize dimensions | `512` |
| `--crop_size` | Center crop dimensions | `384` |
| `--total_dimensions_resnet18` | Total feature dimensions for ResNet18 | `448` |
| `--num_dimensions_resnet18` | Number of selected dimensions for ResNet18 | `100` |
| `--total_dimensions_wide_resnet50_2` | Total feature dimensions for WideResNet50 | `1792` |
| `--num_dimensions_wide_resnet50_2` | Number of selected dimensions for WideResNet50 | `700` |
| `--class_names` | Classes to process (space separated) | All MVTec classes |

### Example Commands

Run on all available MVTec classes with default settings:
```bash
python main.py
```

Run on specific classes with custom settings:
```bash
python main.py --arch resnet18 --class_names bottle cable --batch_size 8 --img_size 256 --crop_size 224
```

Use a different data path:
```bash
python main.py --data_path /path/to/mvtec_dataset --class_names carpet grid
```

## Datasets

**MVTec AD datasets**  
Download from [MVTec website](https://www.mvtec.com/company/research/datasets/mvtec-ad/)

**NOTE**  
Although the code comes under the MIT License, the MVTec AD dataset comes under the **CC BY-NC 4.0 (Creative Commons Non-Commercial)** License.

## Dataset Structure

The dataset should follow this directory structure:

```
data/
└── dataset_name/
    ├── <object_class_1>/
    │   ├── ground_truth/
    │   │   └── <defect_type>/
    │   │       └── <image_mask>.png
    │   ├── test/
    │   │   ├── <defect_type>/
    │   │   │   └── <test_image>.png
    │   │   └── good/
    │   │       └── <test_image>.png
    │   └── train/
    │       └── good/
    │           └── <train_image>.png
    ├── <object_class_2>/
    │   └── ...
    └── <object_class_n>/
        └── ...

```

Place the dataset in the `data` directory or update the `--data_path` argument when running the code.

**Note:** The ground truth folder is only required if you want the Pixel-level anomaly detection accuracy (ROCAUC) score.

## Results
### Implementation results on MVTec
* Image-level anomaly detection accuracy (ROCAUC)

|MvTec|R18-Rd100|WR50-Rd550|
|:---:|:---:|:---:|
|Carpet| 0.984| 0.999|
|Grid|0.898 | 0.957|
|Leather|0.988 | 1.0|
|Tile| 0.959| 0.974|
|Wood|0.990 | 0.988|
|All texture classes| 0.964| 0.984|
|Bottle|0.996 | 0.998|
|Cable| 0.855| 0.922|
|Capsule|0.870 | 0.915|
|Hazelnut|0.841 |0.933 |
|Metal nut| 0.974| 0.992|
|Pill|0.869 | 0.944|
|Screw| 0.745| 0.844|
|Toothbrush|0.947 |0.972 |
|Transistor| 0.925| 0.978|
|Zipper| 0.741| 0.909|
|All object classes|0.876|0.941 |
|All classes| 0.905|0.955 |

* Pixel-level anomaly detection accuracy (ROCAUC)

|MvTec|R18-Rd100|WR50-Rd550|
|:---:|:---:|:---:|
|Carpet| 0.988| 0.990|
|Grid| 0.936| 0.965|
|Leather|0.990 |0.989 |
|Tile|0.917 | 0.939|
|Wood| 0.940| 0.941|
|All texture classes| 0.953|0.965 |
|Bottle|0.981 | 0.982|
|Cable|0.949| 0.968|
|Capsule| 0.982| 0.986|
|Hazelnut|0.979 | 0.979|
|Metal nut| 0.967|0.971 |
|Pill|0.946 |0.961 |
|Screw| 0.972| 0.983|
|Toothbrush|0.986 |0.987 |
|Transistor| 0.968|0.975 |
|Zipper|0.976| 0.984|
|All object classes|0.971|0.978 |
|All classes| 0.965| 0.973|

 ### ROC Curve

* ResNet18

<p align="center">
    <img src="imgs/roc_curve_r18.png" width="1000"\>
</p>

* Wide_ResNet50_2

<p align="center">
    <img src="imgs/roc_curve_wr50.png" width="1000"\>
</p>

### Localization examples

<p align="center">
    <img src="imgs/bottle.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/cable.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/capsule.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/carpet.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/grid.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/hazelnut.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/leather.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/metal_nut.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/pill.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/screw.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/tile.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/toothbrush.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/transistor.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/wood.png" width="600"\>
</p>
<p align="center">
    <img src="imgs/zipper.png" width="600"\>
</p>

## Reference
[1] Thomas Defard, Aleksandr Setkov, Angelique Loesch, Romaric Audigier. *PaDiM: a Patch Distribution Modeling Framework for Anomaly Detection and Localization*. https://arxiv.org/pdf/2011.08785

[2] https://github.com/byungjae89/SPADE-pytorch

[3] https://github.com/byungjae89/MahalanobisAD-pytorch
