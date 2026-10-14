# SC-GWO Brain Tumour Framework

Reference implementation (PyTorch) of a brain-tumour segmentation and classification pipeline evaluated on **BraTS 2019** and **BraTS 2020**:

* **Swin-ResSegNet** – hybrid Swin-Transformer / residual-CNN whole-tumour segmentation network (FLAIR slices).
* **SC-GWO** – Sine-Cosine Grey Wolf Optimizer, used (i) to refine the 25 parameters of the final 1×1 segmentation head and (ii) for wrapper feature selection over 4096-d VGG-16 features.
* **WFA-ResNet** – Wavelet-Frequency Attention residual classifier (Haar DWT branch + spatial multi-head attention + dense head) for HGG (malignant) vs LGG (benign) classification.
* Baselines: Faster R-CNN, YOLO v9 / v8 (segmentation); CNN, DNN, ResNet-18, DenseNet-121, ResDenseNet (classification).

> **Please read before citing results.** This is a single-seed study. The proposed segmentation backbone reaches a whole-tumour Dice of about 0.78 and outperforms the Faster R-CNN and YOLO baselines trained with the same small budget, but **the data do not support a benefit of SC-GWO** (segmentation unchanged by the refinement; classification ablations within the bootstrap uncertainty) **or a clearly better classifier**: WFA-ResNet's accuracy is below the majority-class rate on both datasets. Two executions of the same BraTS 2020 configuration with the same seed differed by up to 0.04 AUC. See the paper for the full discussion.

## Results (seed 0, patient-level split, test slices)

| Dataset | Method | Dice | IoU | Hausdorff (px) |
|---|---|---|---|---|
| BraTS 2019 | Proposed (Swin-ResSegNet + SC-GWO) | 0.7773 | 0.6768 | 15.37 |
| BraTS 2019 | YOLO v9 / YOLO v8 | 0.6858 / 0.6555 | 0.5925 / 0.5736 | 13.77 / 12.41* |
| BraTS 2019 | Faster R-CNN (box as mask) | 0.6119 | 0.4730 | 21.67 |
| BraTS 2020 | Proposed (Swin-ResSegNet + SC-GWO) | 0.7803 | 0.6791 | 17.55 |
| BraTS 2020 | YOLO v9 / YOLO v8 | 0.6436 / 0.6855 | 0.5515 / 0.5915 | 18.58 / 14.17* |
| BraTS 2020 | Faster R-CNN (box as mask) | 0.6384 | 0.4983 | 18.86 |

\*YOLO models leave 13–20 % of test slices empty; their Hausdorff distance excludes those slices and is not comparable.

Classification (malignant = positive; AUC with 95 % patient-level bootstrap interval; majority-class accuracy 0.793 / 0.813):

| Dataset | WFA-ResNet accuracy | WFA-ResNet AUC | Best baseline AUC |
|---|---|---|---|
| BraTS 2019 | 0.7273 | 0.593 [0.470, 0.717] | DenseNet-121 0.654 |
| BraTS 2020 | 0.7310 | 0.625 [0.521, 0.732] | ResNet-18 0.670 |

All numbers, ablations, timings and figures are in [`results/`](results) (`tables.md`, `seed0.json`, `figures/`). YOLO baselines: [`results/YOLO`](results/YOLO).

## Repository layout

```
notebooks/01_main_pipeline.ipynb                  full pipeline: segmentation, SC-GWO, classification, ablation, baselines, tables/figures
notebooks/02_yolo_baselines_both_datasets.ipynb   YOLO v9 / v8 segmentation baselines on both datasets
results/                                          result tables, JSON, figures of the reported runs
```

## Data (not included)

| Dataset | Kaggle source | Grade labels |
|---|---|---|
| BraTS 2019 | https://www.kaggle.com/datasets/aryashah2k/brain-tumor-segmentation-brats-2019 | `HGG/` and `LGG/` folders |
| BraTS 2020 | https://www.kaggle.com/datasets/awsaf49/brats20-dataset-training-validation | `name_mapping.csv` (Grade column) |

The per-slice `.h5` mirror of BraTS 2020 (`awsaf49/brats2020-training-data`) carries **no** grade labels and cannot be used for classification. Please respect the BraTS data-use terms; the data are not redistributed here.

## Reproduce on Kaggle

1. Create a notebook, **Add Input** for the dataset(s), switch **GPU** and **Internet** on.
2. Upload `notebooks/01_main_pipeline.ipynb`, set `DATASET_TO_RUN = "BraTS2019"` (or `"BraTS2020"`) in the *15.0 Experiment settings* cell.
3. **Save Version → Save & Run All (Commit)**. Download the zip from the version's *Output* tab (a Draft Session loses its files when it ends). Expect about 45–60 min per dataset.
4. For the YOLO baselines run `notebooks/02_yolo_baselines_both_datasets.ipynb` the same way with both datasets attached (about 45–60 min).

Settings used for the reported results (`single` preset): 30 / 40 epochs (segmentation / classification), early-stopping patience 6, SC-GWO N=20, T=50 (weights) and N=30, T=100 (feature selection), baselines 5 epochs on a random subset of 3,000 slices, seed 0. Presets `quick` (smoke test) and `full` (five seeds) are available in the same cell.

Local run: `pip install -r requirements.txt`, point `DATA_ROOT_OVERRIDE` to the data folder, and run the notebook with a CUDA GPU.

## Known limitations

* One seed and one split per dataset; BraTS 2020 largely overlaps BraTS 2019, so it is not an independent validation.
* Baselines use their own, smaller training budgets (not budget-matched); YOLO v7 / v6 were not run (no segmentation weights in `ultralytics`).
* 2-D, FLAIR-only, whole-tumour-only; HGG/LGG is used as a proxy for malignant/benign.
* Results are not bit-for-bit reproducible (GPU non-determinism, stochastic feature selection).

## Citation

If you use this code, please cite the accompanying paper (details to be added) and the BraTS challenge papers (Menze et al., 2015; Bakas et al., 2017, 2018).

## License

MIT (see [LICENSE](LICENSE)). The BraTS data are subject to their own terms.
