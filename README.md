MRI-Metabolism-Mapper
A robust neuroimaging pipeline for physiological parameter extraction (CMRO2, OEF, CTH) featuring SyN registration and CVR-based masking.

Overview
This repository provides an automated, reproducible pipeline for processing multi-parametric MRI data. It is designed to handle raw DICOM inputs, perform non-linear spatial normalization, and extract quantitative physiological parameters (CMRO2, OEF, CTH). The methodology incorporates precise tissue segmentation, cerebrovascular reactivity (CVR)-based thresholding, and empirical exclusion of necrotic cores and venous artifacts to ensure high-fidelity statistical outputs.

Key Processing Steps
Automated Data Conversion: Bulk conversion of DICOM series to NIfTI format using dicom2nifti.

High-Fidelity Brain Extraction & Dual-Path Masking: Initial brain extraction is performed using HD-BET (Deep Learning-based robust skull stripping), followed by targeted manual refinement via ITK-SNAP to generate two distinct masks.

First, a Registration Mask—curated by strictly removing the cavernous sinus and residual skull base—is utilized for N4 bias field correction, K-means tissue segmentation, and SyN non-linear registration via ANTs.

Second, a Supratentorial ROI Mask—further refined by excluding the brainstem—is applied in the downstream pipeline to strictly confine the extraction of physiological parameters (CMRO2, OEF, CTH) to the target parenchyma, ensuring absolute anatomical accuracy.

Physiological Masking: Application of a supratentorial ROI with a 2-pixel erosion to minimize partial volume effects.

Vascular Territory Classification: Stratification of preserved and exhausted vascular territories based on MP-ASL derived CVR.

Data Trimming & QC: Exclusion of pixels below physiological thresholds (e.g., <10% for CMRO2 core exclusion) and automated generation of Quality Control (QC) histograms and spatial maps.
Vascular Territory Classification: Stratification of preserved and exhausted vascular territories based on MP-ASL derived CVR.

Data Trimming & QC: Exclusion of pixels below physiological thresholds (e.g., <10% for CMRO2 core exclusion) and automated generation of Quality Control (QC) histograms and spatial maps.

## Expected Directory Structure
For the automated batch-processing to work correctly, ensure your Google Drive (or local directory) is organized as follows before running the pipeline. All raw DICOMs and output NIfTI files should be grouped under each Patient ID.

base_dir/
│
└── [Patient_ID]/
    ├── T1/                 # Raw DICOM folder
    ├── MPASL_pre/          # Raw DICOM folder
    ├── MPASL_post/         # Raw DICOM folder
    ├── CMRO2/              # Raw DICOM folder
    ├── OEF/                # Raw DICOM folder
    ├── CTH/                # Raw DICOM folder
    │
    └── NIfTI_Output/       # NIfTI outputs and manual masks
        ├── hdbet_itksnap_refined_mask_[Patient_ID].nii.gz  (Manual Registration Mask)
        ├── roi_supratentorial_[Patient_ID].nii.gz          (Manual ROI Mask)
        └── ... (Pipeline will auto-generate converted .nii.gz files here)
