# Hearing Image Textures Through the Laplacian Spectrum

[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-orange.svg)](https://pytorch.org/)

This repository contains the implementation and research for the final project in Applied Mathematics (BSc) at ORT Braude College of Engineering[cite: 1, 2]. The project explores the intersection of Spectral Graph Theory and Computer Vision, specifically investigating whether the Laplacian spectrum of an image graph can effectively characterize and classify visual textures[cite: 1, 2].

## 📖 About the Project

Inspired by Mark Kac's famous 1966 mathematical question, "Can One Hear the Shape of a Drum?", this project translates the continuous eigenvalue problem into the discrete domain of digital images[cite: 1, 2]. While continuous isospectral shapes exist, the discrete Laplacian spectrum serves as a powerful geometric and topological signature for texture analysis[cite: 1, 2]. 

By modeling images as weighted graphs, we simulate discrete heat diffusion to analyze textures[cite: 1, 2]:
*   **Smooth Textures:** Allow isotropic, rapid heat diffusion, producing a spectrum dominated by low-frequency eigenvalues[cite: 1, 2].
*   **Rough/Stochastic Textures:** Act as diffusion barriers due to sharp intensity gradients, shifting spectral energy toward high frequencies[cite: 1, 2].

## 🧠 Methodology

The analytical pipeline translates spatial intensity variations into structural spectral signatures[cite: 1, 2]:
1.  **Pre-processing:** Images are converted to grayscale and downsampled (e.g., 50–64 px) to maintain computational efficiency while preserving fundamental texture roughness[cite: 1, 2].
2.  **Graph Construction:** Each pixel is mapped as a vertex in an 8-connected grid graph[cite: 1, 2]. Edges are weighted using a Gaussian kernel based on pixel intensity differences, stored efficiently using SciPy sparse CSR matrices[cite: 1, 2].
3.  **Laplacian Normalization:** The Symmetric Normalized Laplacian ($L_{sym} = I - D^{-1/2} W D^{-1/2}$) is calculated to ensure stable, scale-invariant eigenvalues bounded between [0, 2][cite: 1, 2].
4.  **Spectral Extraction:** The `eigsh` solver (ARPACK) extracts the first $k=50$ non-trivial eigenvalues, forming the baseline "spectral signature" of the image[cite: 1, 2].

## 🏗️ The Spectral-Classical Hybrid Architecture (TextureSpectralNet)

Relying solely on global topology compresses localized high-frequency micro-textures, leading to confusion between topologically similar materials (e.g., sponge vs. brown bread)[cite: 1, 2]. To solve this, a hybrid deep learning architecture was developed[cite: 1, 2]:

*   **Spectral Features (168 dimensions):** Multi-scale eigenvalues of $L_{sym}$ ($\sigma = 0.05, 0.1, 0.3$) and their statistical moments capture the macroscopic global topology[cite: 1, 2].
*   **Classical Features (68 dimensions):** Local Binary Patterns (LBP, 26 bins), Gray-Level Co-occurrence Matrix (GLCM, 10 metrics), and Gabor filters (32 responses) isolate high-frequency micro-textures[cite: 1, 2].
*   **TextureSpectralNet (MLP):** A custom Deep Multi-Layer Perceptron compresses the fused 236-dimensional feature vector through sequential layers (512 -> 256 -> 128 -> Output) utilizing BatchNorm, ReLU, and Dropout for robust classification[cite: 1, 2].

## 📊 Datasets & Results

The architecture was rigorously evaluated across two fundamentally different datasets[cite: 1, 2]:

### 1. KTH-TIPS (Material-Centric, Controlled)
*   **Description:** 10 material classes (e.g., corduroy, sponge, linen) with systematic variations in illumination, scale, and pose[cite: 1, 2].
*   **Results:** The purely spectral baseline achieved 61.11% accuracy[cite: 1, 2]. The Hybrid TextureSpectralNet (MLP) achieved a state-of-the-art **98.15% accuracy**, perfectly classifying 5 out of 10 categories[cite: 1, 2].

### 2. Describable Textures Dataset - DTD (Descriptive, "In-the-Wild")
*   **Description:** 47 perceptual categories (e.g., banded, smeared, bubbly) featuring extreme intra-class diversity, background clutter, and amorphous, non-periodic graphs[cite: 1, 2].
*   **Results:** The spectral baseline achieved 18.62% (compared to a 2.13% random baseline)[cite: 1, 2]. The Hybrid model doubled this performance to **37.61%**[cite: 1, 2]. The model excelled on geometric textures (e.g., 'banded' at 80%) but highlighted the theoretical limits of standard heat diffusion on purely chaotic textures (e.g., 'spiralled' at 2.5%)[cite: 1, 2].

## 🛠️ Technologies & Libraries

*   **Language:** Python[cite: 1, 2]
*   **Data Processing & Graph Math:** NumPy, SciPy (Sparse, ARPACK)[cite: 1, 2]
*   **Computer Vision:** OpenCV, Pillow[cite: 1, 2]
*   **Machine Learning / Deep Learning:** PyTorch (TextureSpectralNet), XGBoost (Tree-based baselines)[cite: 1, 2]
*   **Visualization:** Matplotlib[cite: 1, 2]

## 👨‍🎓 Author

*   **Shadi Alkeesh** - Final Project for BSc in Applied Mathematics[cite: 1, 2].
*   **Supervisor:** Assoc. Prof. Emil Saucan[cite: 1, 2].
