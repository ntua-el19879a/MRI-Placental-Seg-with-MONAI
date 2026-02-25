---
title: "Placenta Segmentation in MRI with Deep Learning"
subtitle: "Diploma Thesis Presentation"
author: "Grigorios Tsenos"
date: "February 2026"
---

# Topic and Context

- National Technical University of Athens (ECE School)
- Thesis focus: 3D placenta segmentation from MRI
- Goal: fair and reproducible model comparison

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_36-36_trim.png){width=67%}

---

# Why This Problem Matters

- Placenta health is critical for pregnancy outcome
- Quantification needs reliable segmentation masks
- Manual delineation is slow and observer-dependent
- Automation enables scalable analysis

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_36-36_trim.png){width=63%}

---

# Core Challenges

- Strong shape and location variability
- Ambiguous organ boundaries in MRI
- Motion artifacts and intensity inhomogeneity
- Limited amount of annotated 3D data

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_36-36_trim.png){width=58%}

---

# Study Objectives

- Build one unified experimental protocol
- Compare CNN, Transformer, and SSM families
- Keep training conditions identical across models
- Combine quantitative and qualitative evaluation

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/diagram_model_families.png){width=72%}

---

# Architecture Families Compared

- CNN: U-Net, Attention U-Net, DynUNet, SegResNet
- Transformer: UNETR, SwinUNETR
- State-space: SegMamba
- Same data pipeline for all runs

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/diagram_model_families.png){width=76%}

---

# Unified Experimental Pipeline

- Input standardization and geometric alignment
- Patch sampling and augmentation strategy
- Common optimization and validation flow
- Fair comparison by design

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/diagram_pipeline.png){width=92%}

---

# Dataset and Split

- Total MRI volumes: 137
- Training set: 109 volumes
- Validation set: 28 volumes
- 3D volume-level evaluation

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/chart_data_split.png){width=62%}

---

# Preprocessing Choices

- Orientation normalization to RAS
- Resampling to voxel spacing (2.0, 2.0, 2.0) mm
- Intensity normalization with percentiles
- Foreground ROI cropping and fixed patch shape

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_57-57_trim.png){width=45%}

---

# Training Protocol

- Patch size: (96, 96, 64)
- Loss: DiceCE (with sigmoid, no background)
- Optimizer: AdamW + CyclicLR scheduler
- 120 epochs, EMA, sliding-window inference

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_60-60_trim.png){width=45%}

---

# U-Net Foundation

- Encoder-decoder with skip connections
- Strong baseline for medical segmentation
- 3D variants capture volumetric context

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_48-48_trim.png){width=70%}

---

# Residual and Attention Ideas

- Residual blocks support deeper optimization
- Attention gates refine skip features
- Better focus on the target organ boundaries

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_49-49_trim.png){width=70%}

---

# Transformer Design Choices

- UNETR: ViT-style encoder with U-shaped decoder
- SwinUNETR: hierarchical windows and patch merging
- Locality priors improved stability versus pure ViT

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_50-50_trim.png){width=76%}

---

# Quantitative Results (Validation Set)

- Best Dice: SegMamba Heavier = 0.8606
- Very close: SegResNet Heavier = 0.8601
- Strong Transformer baseline: SwinUNETR ~0.849
- Lowest performer: UNETR = 0.772

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_64-64_trim.png){width=72%}

---

# Ranking by Dice

- Top cluster around Dice ~0.86
- Small differences among top 4 models
- Classic U-Net still competitive baseline

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/chart_dice_ranking.png){width=88%}

---

# Training Curves: Group A

- Attention U-Net, DynUNet, and SwinUNETR converge fast
- Early gains in first epochs, then plateau behavior
- Best epoch selected by validation Dice

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_65-65_trim.png){width=72%}

---

# Training Curves: Group B

- SegResNet and SegMamba show strong stability
- UNETR behaves as an outlier in convergence quality
- Curves support the final ranking trend

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_66-66_trim.png){width=74%}

---

# Qualitative Results (Set 1)

- Clear masks for DynUNet and SwinUNETR variants
- Good shape consistency across examples
- Boundary quality aligns with Dice/IoU trends

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_69-69_trim.png){width=78%}

---

# Qualitative Results (Set 2)

- SegResNet variants yield clean, continuous masks
- U-Net is acceptable but less consistent at boundaries
- Visual behavior matches quantitative scores

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_70-70_trim.png){width=80%}

---

# Qualitative Results (Set 3)

- SegMamba predictions are close to ground truth
- UNETR shows more false positives and instability
- Qualitative inspection reinforces outlier behavior

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_71-71_trim.png){width=80%}

---

# Runtime and Practical Cost

- Fastest run: U-Net (~0.77 h)
- SwinUNETR and SegResNet Heavier are costly
- SegMamba includes additional loading overhead
- Accuracy gains must justify compute cost

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_74-74_trim.png){width=70%}

---

# Accuracy vs Compute Trade-off

- Top accuracy does not always mean best efficiency
- Lighter variants are often near-identical in quality
- Practical deployment should optimize both axes

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/chart_dice_vs_runtime.png){width=88%}

---

# Key Conclusions

- Unified protocol enabled fair cross-family comparison
- Best overall performers: SegMamba and SegResNet
- SwinUNETR remained competitive with higher cost
- UNETR underperformed in this dataset/split

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/chart_runtime.png){width=82%}

---

# Future Work and Next Steps

- Add independent test set and cross-validation
- Validate on external, multi-center datasets
- Integrate uncertainty estimation for trust
- Explore fine-tuning from best checkpoints
- Extend to multimodal and multi-task learning

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_36-36_trim.png){width=55%}

---

# Thank You

- Questions and discussion

![](/Users/gregtsen/Desktop/notes/MRI-Placental-Seg-with-MONAI/presentation_assets/page_36-36_trim.png){width=55%}
