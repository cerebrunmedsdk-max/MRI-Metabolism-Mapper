# MRI-Metabolism-Mapper
A robust neuroimaging pipeline for physiological parameter extraction (CMRO2, OEF, CTH) featuring SyN registration and CVR-based masking.

Overview
This repository provides an automated, reproducible pipeline for processing multi-parametric MRI data. It is designed to handle raw DICOM inputs, perform non-linear spatial normalization, and extract quantitative physiological parameters (CMRO2, OEF, CTH). The methodology incorporates precise tissue segmentation, cerebrovascular reactivity (CVR)-based thresholding, and empirical exclusion of necrotic cores and venous artifacts to ensure high-fidelity statistical outputs.

Key Processing Steps:

Automated Data Conversion: Bulk conversion of DICOM series to NIfTI format using dicom2nifti.

Spatial Registration & Segmentation: N4 bias field correction and non-linear registration (SyN) using Advanced Normalization Tools (ANTs). Parenchyma is isolated via K-means clustering.

Physiological Masking: Application of a supratentorial ROI with a 2-pixel erosion to minimize partial volume effects.

Vascular Territory Classification: Stratification of preserved and exhausted vascular territories based on MP-ASL derived CVR.

Data Trimming & QC: Exclusion of pixels below physiological thresholds (e.g., <10% for CMRO2 core exclusion) and automated generation of Quality Control (QC) histograms and spatial maps.
