# Results: BraTS2019, preset=single, seeds=[0] (n=1)

Values are mean ± sample std over seeds (single value when n=1). Slice-level metrics over tumour-bearing test slices.

Slices train/val/test (seed 0): 13926/2462/4250


## Segmentation (Tables 12/13)

| Method | DSC | IoU | Jaccard | Hausdorff (px) | % empty predictions |
|---|---|---|---|---|---|
| Proposed (Swin-ResSegNet + SC-GWO) | 0.7773 | 0.6768 | 0.6768 | 15.37 | 0.9 |
| Swin-ResSegNet without SC-GWO refinement | 0.7773 | 0.6768 | 0.6768 | 15.36 | 0.9 |
| Swin-ResSegNet + pure GWO refinement | 0.7773 | 0.6768 | 0.6768 | 15.37 | 0.9 |
| Faster R-CNN (box as mask) | 0.6119 | 0.4730 | 0.4730 | 21.67 | 0.1 |

## Classification (Tables 14/15), positive class = malignant (HGG)

| Method | Accuracy | Precision | Recall | Specificity | F1 | MSE | RMSE | AUC (trapezoid) | AUC (exact) | Balanced acc. | Patient-level AUC |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Proposed (WFA-ResNet) | 0.7273 | 0.8207 | 0.8394 | 0.2985 | 0.8299 | 0.2167 | 0.4655 | 0.5931 | 0.5927 | 0.5690 | 0.6795 |
| ResDenseNet | 0.6125 | 0.7990 | 0.6830 | 0.3428 | 0.7364 | 0.2402 | 0.4901 | 0.5108 | 0.5107 | 0.5129 | 0.4808 |
| DenseNet-121 | 0.7678 | 0.8057 | 0.9317 | 0.1407 | 0.8641 | 0.1653 | 0.4065 | 0.6533 | 0.6538 | 0.5362 | 0.6718 |
| ResNet-18 | 0.6767 | 0.7923 | 0.8026 | 0.1952 | 0.7974 | 0.2162 | 0.4650 | 0.5762 | 0.5761 | 0.4989 | 0.5974 |
| DNN | 0.6395 | 0.8444 | 0.6684 | 0.5289 | 0.7462 | 0.2248 | 0.4741 | 0.6402 | 0.6403 | 0.5987 | 0.7103 |
| CNN | 0.7927 | 0.7927 | 1.0000 | 0.0000 | 0.8844 | 0.1947 | 0.4413 | 0.4532 | 0.4478 | 0.5000 | 0.3808 |

Majority-class (always 'malignant') accuracy ≈ test malignant fraction = 0.7927. A classifier is only informative if AUC/balanced accuracy are clearly above 0.5 / the majority rate.

## Ablation (Table 16), seeds=[0]

| Configuration | Accuracy % | AUC (exact) | Specificity | Balanced acc. | Seg. DSC | # features |
|---|---|---|---|---|---|---|
| Full model | 72.73 | 0.5927 | 0.2985 | 0.5690 | 0.7773 | 1983 |
| w/o SC-GWO weight refinement | 73.93 | 0.6323 | 0.2860 | 0.5719 | 0.7773 | 2050 |
| SC-GWO replaced by pure GWO | 71.48 | 0.6124 | 0.2611 | 0.5473 | 0.7773 | 1991 |
| w/o wavelet branch | 72.66 | 0.6358 | 0.3893 | 0.6021 | 0.7773 | 1983 |
| w/o spatial attention branch | 60.14 | 0.5749 | 0.4188 | 0.5340 | 0.7773 | 1983 |

## Computation time (Table 17)

| Item | Value |
|---|---|
| infer_ms_per_slice_cls__CNN | 0.34 |
| infer_ms_per_slice_cls__DNN | 0.01 |
| infer_ms_per_slice_cls__DenseNet-121 | 2.83 |
| infer_ms_per_slice_cls__ResDenseNet | 0.31 |
| infer_ms_per_slice_cls__ResNet-18 | 0.79 |
| infer_ms_per_slice_cls_proposed | 1.11 |
| infer_ms_per_slice_seg__Faster R-CNN | 18.00 |
| infer_ms_per_slice_seg_proposed | 2.20 |
| pipeline_total_s (minutes) | 51.8 |
| scgwo_feature_selection_s (minutes) | 15.8 |
| scgwo_refine_s (minutes) | 2.4 |
| seg_train_s (minutes) | 33.0 |
| train_s__CNN (minutes) | 0.2 |
| train_s__DNN (minutes) | 0.0 |
| train_s__DenseNet-121 (minutes) | 2.1 |
| train_s__Faster R-CNN (minutes) | 10.0 |
| train_s__ResDenseNet (minutes) | 0.3 |
| train_s__ResNet-18 (minutes) | 0.6 |
| wfa_train_s (minutes) | 0.6 |

SC-GWO selected features (of 4096): 1983

(Paired significance tests need at least 3 seeds; run PRESET='full'.)

## 95% patient-level bootstrap CIs (seed 0, 500 resamples over test patients)

| Quantity | Estimate | 95% CI |
|---|---|---|
| DSC, Proposed | 0.7773 | [0.7454, 0.8036] |
| DSC, NoRefine | 0.7773 | [0.7454, 0.8036] |
| DSC, Faster R-CNN | 0.6119 | [0.5811, 0.6338] |
| AUC (exact), Proposed | 0.5927 | [0.4704, 0.7170] |
| AUC (exact), CNN | 0.4478 | [0.3207, 0.5613] |
| AUC (exact), DNN | 0.6403 | [0.5396, 0.7358] |
| AUC (exact), ResNet-18 | 0.5761 | [0.4642, 0.6867] |
| AUC (exact), DenseNet-121 | 0.6538 | [0.5429, 0.7508] |
| AUC (exact), ResDenseNet | 0.5107 | [0.3958, 0.6008] |
| AUC (exact), abl w/o wavelet branch | 0.6358 | [0.5171, 0.7454] |
| AUC (exact), abl w/o spatial attention branch | 0.5749 | [0.4775, 0.6856] |
| AUC (exact), abl w/o SC-GWO weight refinement | 0.6323 | [0.5166, 0.7482] |
| AUC (exact), abl pure GWO | 0.6124 | [0.5152, 0.7127] |