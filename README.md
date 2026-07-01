# Predicting Drug-Target Interactions with 1D-CNN & Cross-Attention Fusion

A machine-learning framework for predicting Drug-Target Interaction (DTI) probabilities from 1D protein sequences and drug molecular fingerprints, utilizing dynamic thresholding for reliable high-throughput virtual screening.

## Overview

The discovery of new drugs is often limited by the high cost and time constraints of wet-lab experiments and complex 3D molecular docking simulations. This project provides a fast, interpretable, and highly efficient deep-learning alternative for early-stage virtual screening of drug-target pairs.

The framework predicts **interaction probabilities** directly from simplified chemical descriptors (SMILES) and protein sequences. By utilizing an integrated training approach across multiple datasets and applying dynamic F1-based thresholding, the model adapts to varying data distributions to maximize predictive accuracy.

## Core Idea

The pipeline combines three levels of architectural innovation:

1. **Feature Extraction (Encoders)**
   A Multi-Layer Perceptron (MLP) processes 2048-bit Morgan Fingerprints into drug tokens, while a 1D-Convolutional Neural Network (1D-CNN) processes protein sequences to identify pocket-like receptor tokens without needing 3D spatial data.

2. **Model-Level Fusion**
   A Transformer-based Cross-Attention mechanism acts as the fusion layer. It uses the drug tokens as *queries* and the protein pocket tokens as *keys/values* to identify interaction hot-spots before passing the embeddings to a linear classifier.

3. **Dynamic Threshold Optimization**
   To handle varying dataset imbalances and scoring scales, the model automatically scans and calculates an optimal decision boundary (threshold) based on the validation F1 score before evaluating the test sets.

## Methodology

The complete workflow consists of:

```text
Drug SMILES string & Protein 1D Sequence
        ↓
Morgan Fingerprint & Sequence Tokenization
        ↓
Integrated Data Merging (DAVIS, BIOSNAP, BindingDB, KIBA)
        ↓
Drug Encoder (MLP) & Protein Encoder (1D-CNN)
        ↓
Transformer-based Cross-Attention Fusion
        ↓
Binary Cross Entropy Loss (with pos_weight balancing)
        ↓
Dynamic F1-based Decision Thresholding
        ↓
Predicted DTI Probability and Binary Classification
```

## Dataset

The model is evaluated on four benchmark drug-target interaction datasets (**BIOSNAP and BindingDB**)

Each sample includes:

* Drug SMILES string
* Target protein amino acid sequence
* Binary interaction label / Affinity score

## Model Performance

| Dataset       | Optimized Threshold | AUC    | AUPRC  | F1 Score | SENSITIVITY | SPECIFICITY |
| ------------- | ------------------: | -----: | -----: | -------: | ----------: | ----------: |
| **BIOSNAP** |                0.45 | 0.8843 | 0.8924 |   0.8106 |      0.8051 |      0.8166 |
| **BindingDB** |                0.90 | 0.9091 | 0.6193 |   0.6145 |      0.7213 |      0.8952 |

## Key Findings

* The proposed 1D-CNN + Transformer fusion achieves strong predictive accuracy for DTI prediction while remaining computationally lightweight.
* Integrated training (merging train/val splits across the four datasets) creates a highly generalized model capable of strong performance on independent test sets.
* Dynamic threshold optimization is crucial for performance; the optimal threshold varied drastically due to underlying class imbalances and distinct dataset characteristics.
* The method does not require PDB/mmCIF files (3D crystal structures), making it suitable for rapid screening of novel targets where spatial structures are unavailable.

## Intended Use

This framework can support:

* Early-stage virtual screening for drug discovery
* Rapid protein-ligand interaction prediction
* Benchmarking of sequence-based machine learning models
* Drug repurposing studies
* High-throughput filtering prior to complex 3D molecular docking

## Results

## AUROC curve
<img width="882" height="767" alt="image" src="https://github.com/user-attachments/assets/32cbf9f8-e6dc-4be9-b782-d51113b59aff" />

## AUPRC curve
<img width="878" height="765" alt="image" src="https://github.com/user-attachments/assets/c2746ca1-3c56-40fb-8887-49198252685b" />

## BiosNAP Residue Profile
<img width="1757" height="707" alt="image" src="https://github.com/user-attachments/assets/d8d87571-9115-401f-ad5f-56de2a479dab" />

## BindingDB Residue Profile
<img width="1760" height="699" alt="image" src="https://github.com/user-attachments/assets/8f90b615-d261-417b-82fe-a017f49b1985" />

## BindingDB RAtom Importance map
<img width="851" height="683" alt="image" src="https://github.com/user-attachments/assets/38a94cce-b7ef-4273-993e-16ec97272dda" />

## BiosNAP RAtom Importance map
<img width="1735" height="808" alt="image" src="https://github.com/user-attachments/assets/362b9180-e53f-4079-a341-88c889ac1a4d" />





