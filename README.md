# Region-Wise Crowd Counting Benchmark

This repository contains the image list and the region-wise ground truth of the
benchmark introduced in

> *Evaluating Spatial Reasoning of Large Vision-Language Models: A Benchmark for
> Region-Wise Crowd Counting* (IEEE Access, under review).

The benchmark has 250 images, 50 at each of five crowd-size levels. Each image
is divided into three vertical regions of equal width (left, center, right).
The ground truth gives the number of people in each region and the most-crowded
region.

**The images are not included.** Both source datasets are distributed under
their own terms, so this repository gives identifiers and annotations only.
Download the images from the official sources below and match them by
`source_dataset`, `source_split` and `source_file`.

## Files

| File | Content |
|---|---|
| `image_list.csv` | One row per image (250 rows) |
| `ground_truth.json` | The same information as one JSON record per image |

### Fields

| Field | Meaning |
|---|---|
| `image_id` | Benchmark identifier, e.g. `jhu_0055`, `sha_IMG_404` |
| `source_dataset` | `JHU-Crowd++` or `ShanghaiTech Part B` |
| `source_split` | Split of the official release: `train`, `val` or `test` |
| `source_file` | File name in that split, e.g. `0055.jpg`, `IMG_88.jpg` |
| `width`, `height` | Image size in pixels |
| `level` | Crowd-size level: 1 = 1–10, 2 = 11–20, 3 = 21–30, 4 = 31–40, 5 = 41–50 people |
| `total_count` | Number of annotated people |
| `left_count`, `center_count`, `right_count` | People in each region |
| `most_crowded_region` | Region(s) with the largest count, separated by `\|` when tied |
| `tied` | 1 if two or more regions share the largest count, otherwise 0 |
| `delta` | Headcount gap Δ between the most-crowded and the second-most-crowded region; 0 for tied images |

The paths of the source files are:

- JHU-Crowd++ v2.0: `<split>/images/<source_file>` (annotations in `<split>/gt/`).
- ShanghaiTech Part B: `part_B/<split>_data/images/<source_file>` (annotations in `part_B/<split>_data/ground-truth/GT_<source_file stem>.mat`).

The numbers in the `image_id` of ShanghaiTech Part B images are those of our
working copy, not of the official release. Use `source_split` and
`source_file`.

## How the ground truth is defined

Each annotated head point of the official annotations is assigned by its
horizontal coordinate x, with W the width of the original image:

- left: x < W/3
- center: W/3 ≤ x < 2W/3
- right: x ≥ 2W/3

The intervals are half-open, so a head exactly on a boundary belongs to the
region on its right. All points of the official annotations are used. This
includes 18 points that lie outside the image (17 in 15 ShanghaiTech Part B
images, 1 in 1 JHU-Crowd++ image); they are assigned by their x coordinate like
all other points.

When two or more regions share the largest count, every one of them is listed
in `most_crowded_region` and counts as a correct answer.

The JHU-Crowd++ images come from a pool of images with 1–50 people whose
image-level labels are `weather-condition = 0` and `distractor = 0`. Images
with umbrellas were removed. The images at each level were then chosen
manually from the two pools. This list, not the pool, defines the benchmark.

## Source datasets and terms of use

- **JHU-Crowd++** (V. A. Sindagi, R. Yasarla, and V. M. Patel, IEEE TPAMI,
  2022). The dataset is for academic and non-commercial use, and its license
  asks that the JHU-Crowd++ papers be cited.
- **ShanghaiTech Part B** (Y. Zhang, D. Zhou, S. Chen, S. Gao, and Y. Ma,
  "Single-image crowd counting via multi-column convolutional neural
  network," CVPR 2016).

## License

The image list and the region-wise ground truth in this repository are
released under the Creative Commons Attribution-NonCommercial 4.0
International license (CC BY-NC 4.0); see `LICENSE`. The source images and
their original annotations are not part of this repository and remain under
the terms of their datasets.

## Citation

If you use this benchmark, please cite our paper (reference to be added on
publication) and the two source datasets above.
