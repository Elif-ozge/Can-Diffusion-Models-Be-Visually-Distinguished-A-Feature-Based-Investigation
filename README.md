# Can Diffusion Models Be Visually Distinguished? A Feature-Based Investigation

This repository contains the official implementation and research code for investigating unique visual "footprints" left by popular text-to-image diffusion models. 

By analyzing hand-crafted visual characteristics (color, contrast, sharpness, edge density, etc.), this project evaluates machine learning and deep learning approaches to trace an AI-generated image back to its originating model.

---

## Abstract

As synthetic content proliferates, identifying the source architecture of AI-generated media has become crucial for digital forensics and ethical AI transparency. Utilizing the open-source **Synthbuster** benchmark dataset, this project extracts multi-dimensional image features across three major models: **DALL-E 2**, **Midjourney v5**, and **Stable Diffusion 1.4**. 

Statistical significance was confirmed via ANOVA, followed by Recursive Feature Elimination (RFE) to isolate the top 10 most discriminative attributes. Classifiers including Support Vector Machines (SVM), Random Forests (RF), and a custom Deep Neural Network (DNN) were trained and evaluated.

---

## Key Findings & Results

* **Feasibility:** Diffusion architectures leave distinct, measurable statistical and structural signatures in their outputs.
* **Best Performer:** The **Deep Neural Network (DNN)** achieved the highest performance with an overall accuracy of **79.17%**, outperforming traditional ensemble and margin-based models.
* **Top Discriminative Features:** Feature importance analysis highlighted color distribution, contrast variance (luminance channel), sharpness (Laplacian variance), and edge density (Canny filtering) as major indicators.

### Model Comparison Summary

| Model / Classifier | Accuracy | Precision (Macro Avg) | Recall (Macro Avg) | F1-Score (Macro Avg) |
| :--- | :---: | :---: | :---: | :---: |
| **Deep Neural Network (DNN)** | **79.17%** | **0.79** | **0.79** | **0.79** |
| **Random Forest (RF)** | 79.00% | 0.79 | 0.79 | 0.79 |
| **Support Vector Machine (SVM - RBF)** | 75.00% | 0.76 | 0.75 | 0.75 |

---

## Feature Extraction 

The feature engineering workflow processes raw `.jpg` and `.png` images into tabular representations capturing:

1. **Color & Saturation:** Dominant/least common colors, HSV saturation range (avg/min/max).
2. **Luminance & Contrast:** Brightness range (avg/min/max) and contrast (standard deviation of the luminance channel).
3. **Structure & Symmetry:** Structural similarity-based horizontal/vertical symmetry scores.
4. **Detail & Spatial Distribution:** Sharpness (variance of Laplacian), Edge Density (Canny edge pixel ratio), and Whitespace Ratio.

---

## Methodology 
```mermaid
graph TD
    %% Dataset & Preprocessing
    subgraph STEP1["1. Data Acquisition & Preprocessing"]
        A["Synthbuster Dataset<br/><i>(DALL-E 2, Midjourney v5, Stable Diffusion 1.4)</i>"] --> B["Image Processing Pipeline<br/><i>(Feature Extraction)</i>"]
    end

    %% Feature Extraction Breakdown
    subgraph STEP2["2. Visual Feature Engineering"]
        B --> C1["<b>Color & Saturation</b><br/>HSV Range, Dominant Colors"]
        B --> C2["<b>Luminance & Contrast</b><br/>Brightness, Std Dev of Luminance"]
        B --> C3["<b>Structure & Symmetry</b><br/>Structural Similarity SSIM"]
        B --> C4["<b>Detail & Texture</b><br/>Laplacian Sharpness, Canny Edge Density"]
    end

    %% Feature Selection & Normalization
    subgraph STEP3["3. Feature Selection & Normalization"]
        C1 & C2 & C3 & C4 --> D["Data Cleaning & Filtering<br/><i>(Remove Zero-Variance Features)</i>"]
        D --> E["ANOVA Hypothesis Testing<br/><i>(p < 0.05 Verification)</i>"]
        E --> F["Recursive Feature Elimination (RFE)<br/><i>(Select Top 10 Features)</i>"]
        F --> G["Z-Score Standardization<br/><i>(StandardScaler)</i>"]
    end

    %% Modeling & Classification
    subgraph STEP4["4. Model Training & Evaluation"]
        G --> H1["SVM Classifier<br/><i>(RBF Kernel)</i>"]
        G --> H2["Random Forest<br/><i>(Ensemble Trees)</i>"]
        G --> H3["Deep Neural Network<br/><i>(Dense Keras Sequential)</i>"]

        H1 --> I1["Accuracy: 75.0%"]
        H2 --> I2["Accuracy: 79.0%"]
        H3 --> I3["<b>Accuracy: 79.17% (Best)</b>"]
    end

    %% Styling
    style STEP1 fill:#f8f9fa,stroke:#d1d5db,stroke-width:1px
    style STEP2 fill:#f0f7ff,stroke:#93c5fd,stroke-width:1px
    style STEP3 fill:#fefce8,stroke:#fde047,stroke-width:1px
    style STEP4 fill:#f0fdf4,stroke:#86efac,stroke-width:1px
    style H3 fill:#dcfce7,stroke:#16a34a,stroke-width:2px
    style I3 fill:#16a34a,stroke:#15803d,color:#ffffff,font-weight:bold
```

---

## Dataset & References. 

* **Dataset Source:** Synthbuster Benchmark Dataset ([Zenodo Link](https://zenodo.org/records/10066460))
* **Primary Reference:** Bammey, Q. (2024). *Synthbuster: Towards Detection of Diffusion Model Generated Images*. IEEE Open Journal of Signal Processing.
