# Results: BraTS2020, preset=single, seeds=[0] (n=1)

Values are mean ± sample std over seeds (single value when n=1). Slice-level metrics over tumour-bearing test slices.

Slices train/val/test (seed 0): 15586/2782/4446


## Segmentation (Tables 12/13)

| Method | DSC | IoU | Jaccard | Hausdorff (px) | % empty predictions |
|---|---|---|---|---|---|
| Proposed (Swin-ResSegNet + SC-GWO) | 0.7803 | 0.6791 | 0.6791 | 17.55 | 0.3 |
| Swin-ResSegNet without SC-GWO refinement | 0.7803 | 0.6791 | 0.6791 | 17.54 | 0.3 |
| Swin-ResSegNet + pure GWO refinement | 0.7803 | 0.6791 | 0.6791 | 17.54 | 0.3 |
| Faster R-CNN (box as mask) | 0.6384 | 0.4983 | 0.4983 | 18.86 | 0.2 |

## Classification (Tables 14/15), positive class = malignant (HGG)

| Method | Accuracy | Precision | Recall | Specificity | F1 | MSE | RMSE | AUC (trapezoid) | AUC (exact) | Balanced acc. | Patient-level AUC |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Proposed (WFA-ResNet) | 0.7310 | 0.8181 | 0.8606 | 0.1663 | 0.8388 | 0.2115 | 0.4599 | 0.6246 | 0.6246 | 0.5134 | 0.6621 |
| ResDenseNet | 0.7175 | 0.8007 | 0.8689 | 0.0578 | 0.8334 | 0.2295 | 0.4791 | 0.3739 | 0.3739 | 0.4634 | 0.2814 |
| DenseNet-121 | 0.6255 | 0.8438 | 0.6621 | 0.4663 | 0.7420 | 0.2323 | 0.4819 | 0.5919 | 0.5919 | 0.5642 | 0.6667 |
| ResNet-18 | 0.7238 | 0.8518 | 0.7995 | 0.3940 | 0.8248 | 0.1751 | 0.4185 | 0.6694 | 0.6697 | 0.5967 | 0.7989 |
| DNN | 0.5416 | 0.8567 | 0.5241 | 0.6181 | 0.6503 | 0.2541 | 0.5041 | 0.5883 | 0.5880 | 0.5711 | 0.6847 |
| CNN | 0.5792 | 0.7852 | 0.6643 | 0.2084 | 0.7197 | 0.2470 | 0.4970 | 0.4272 | 0.4259 | 0.4364 | 0.3831 |

Majority-class (always 'malignant') accuracy ≈ test malignant fraction = 0.8133. A classifier is only informative if AUC/balanced accuracy are clearly above 0.5 / the majority rate.

## Ablation (Table 16), seeds=[0]

| Configuration | Accuracy % | AUC (exact) | Specificity | Balanced acc. | Seg. DSC | # features |
|---|---|---|---|---|---|---|
| Full model | 73.10 | 0.6246 | 0.1663 | 0.5134 | 0.7803 | 2044 |
| w/o SC-GWO weight refinement | 70.76 | 0.6339 | 0.3759 | 0.5798 | 0.7803 | 2007 |
| SC-GWO replaced by pure GWO | 68.65 | 0.6340 | 0.4157 | 0.5821 | 0.7803 | 2028 |
| w/o wavelet branch | 70.02 | 0.6081 | 0.3120 | 0.5507 | 0.7803 | 2044 |
| w/o spatial attention branch | 64.75 | 0.5728 | 0.3747 | 0.5424 | 0.7803 | 2044 |

## Computation time (Table 17)

| Item | Value |
|---|---|
| infer_ms_per_slice_cls__CNN | 0.34 |
| infer_ms_per_slice_cls__DNN | 0.01 |
| infer_ms_per_slice_cls__DenseNet-121 | 2.88 |
| infer_ms_per_slice_cls__ResDenseNet | 0.30 |
| infer_ms_per_slice_cls__ResNet-18 | 0.81 |
| infer_ms_per_slice_cls_proposed | 0.88 |
| infer_ms_per_slice_seg__Faster R-CNN | 18.68 |
| infer_ms_per_slice_seg_proposed | 2.28 |
| pipeline_total_s (minutes) | 57.1 |
| scgwo_feature_selection_s (minutes) | 15.0 |
| scgwo_refine_s (minutes) | 2.5 |
| seg_train_s (minutes) | 39.1 |
| train_s__CNN (minutes) | 0.2 |
| train_s__DNN (minutes) | 0.0 |
| train_s__DenseNet-121 (minutes) | 2.1 |
| train_s__Faster R-CNN (minutes) | 10.1 |
| train_s__ResDenseNet (minutes) | 0.2 |
| train_s__ResNet-18 (minutes) | 0.6 |
| wfa_train_s (minutes) | 0.6 |

SC-GWO selected features (of 4096): 2044

(Paired significance tests need at least 3 seeds; run PRESET='full'.)

## 95% patient-level bootstrap CIs (seed 0, 500 resamples over test patients)

| Quantity | Estimate | 95% CI |
|---|---|---|
| DSC, Proposed | 0.7803 | [0.7496, 0.8042] |
| DSC, NoRefine | 0.7803 | [0.7496, 0.8042] |
| DSC, Faster R-CNN | 0.6384 | [0.6163, 0.6581] |
| AUC (exact), Proposed | 0.6246 | [0.5205, 0.7322] |
| AUC (exact), CNN | 0.4259 | [0.3347, 0.5255] |
| AUC (exact), DNN | 0.5880 | [0.5124, 0.6684] |
| AUC (exact), ResNet-18 | 0.6697 | [0.5859, 0.7461] |
| AUC (exact), DenseNet-121 | 0.5919 | [0.5129, 0.6682] |
| AUC (exact), ResDenseNet | 0.3739 | [0.2871, 0.4698] |
| AUC (exact), abl w/o wavelet branch | 0.6081 | [0.5235, 0.7018] |
| AUC (exact), abl w/o spatial attention branch | 0.5728 | [0.4772, 0.6633] |
| AUC (exact), abl w/o SC-GWO weight refinement | 0.6339 | [0.5264, 0.7399] |
| AUC (exact), abl pure GWO | 0.6340 | [0.5340, 0.7316] |