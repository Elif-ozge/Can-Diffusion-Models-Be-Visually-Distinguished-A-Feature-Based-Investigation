# Can Diffusion Models Be Visually Distinguished? A Feature-Based Investigation

## Abstract

As synthetic content proliferates, identifying the source architecture of AI-generated media has become crucial for digital forensics and ethical AI transparency. This repository investigates unique visual "footprints" left by popular text-to-image diffusion models (**DALL-E 2**, **Midjourney v5**, and **Stable Diffusion 1.4**) by analyzing hand-crafted statistical and structural characteristics (color distribution, contrast, sharpness, edge density, etc.).

Utilizing the open-source **Synthbuster** benchmark dataset, multi-dimensional visual features were extracted and validated via ANOVA hypothesis testing. Recursive Feature Elimination (RFE) was subsequently applied to isolate the top 10 most discriminative attributes, which were then evaluated across Support Vector Machines (SVM), Random Forests (RF), and a custom Deep Neural Network (DNN) to trace generated images back to their originating models.

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
        A[" Synthbuster Dataset<br/><i>(DALL-E 2, Midjourney v5, Stable Diffusion 1.4)</i>"] --> B[" Feature Extraction Pipeline"]
    end

    %% Feature Extraction Breakdown
    subgraph STEP2["2. Visual Feature Engineering"]
        B --> C1[" <b>Color & Saturation</b><br/>HSV Range, Dominant Colors"]
        B --> C2[" <b>Luminance & Contrast</b><br/>Brightness, Std Dev of Luminance"]
        B --> C3[" <b>Structure & Symmetry</b><br/>Structural Similarity SSIM"]
        B --> C4[" <b>Detail & Texture</b><br/>Laplacian Sharpness, Edge Density"]
    end

    %% Feature Selection & Normalization
    subgraph STEP3["3. Feature Selection & Normalization"]
        C1 & C2 & C3 & C4 --> D[" Data Cleaning & Filtering<br/><i>(Remove Zero-Variance Features)</i>"]
        D --> E[" ANOVA Hypothesis Testing<br/><i>(p < 0.05 Verification)</i>"]
        E --> F[" Recursive Feature Elimination (RFE)<br/><i>(Select Top 10 Features)</i>"]
        F --> G[" Z-Score Standardization<br/><i>(StandardScaler)</i>"]
    end

    %% Modeling & Classification
    subgraph STEP4["4. Model Training & Evaluation"]
        G --> H1[" SVM Classifier<br/><i>(RBF Kernel)</i>"]
        G --> H2[" Random Forest<br/><i>(Ensemble Trees)</i>"]
        G --> H3[" Deep Neural Network<br/><i>(Dense Keras Sequential)</i>"]

        H1 --> I1["Accuracy: 75.0%"]
        H2 --> I2["Accuracy: 79.0%"]
        H3 --> I3[" <b>Accuracy: 79.17% (Best)</b>"]
    end

    %% Transparent background for subgraphs
    style STEP1 fill:none,stroke:#6366f1,stroke-width:2px,stroke-dasharray: 5 5,color:#6366f1
    style STEP2 fill:none,stroke:#06b6d4,stroke-width:2px,stroke-dasharray: 5 5,color:#06b6d4
    style STEP3 fill:none,stroke:#f59e0b,stroke-width:2px,stroke-dasharray: 5 5,color:#f59e0b
    style STEP4 fill:none,stroke:#10b981,stroke-width:2px,stroke-dasharray: 5 5,color:#10b981

    %% Colorful Node Styles
    style A fill:#e0e7ff,stroke:#4338ca,stroke-width:2px,color:#312e81
    style B fill:#cfffe5,stroke:#059669,stroke-width:2px,color:#064e3b

    style C1 fill:#ffe4e6,stroke:#e11d48,stroke-width:2px,color:#881337
    style C2 fill:#fef3c7,stroke:#d97706,stroke-width:2px,color:#78350f
    style C3 fill:#e0f2fe,stroke:#0284c7,stroke-width:2px,color:#0c4a6e
    style C4 fill:#f3e8ff,stroke:#9333ea,stroke-width:2px,color:#581c87

    style D fill:#fef3c7,stroke:#b45309,stroke-width:1.5px,color:#78350f
    style E fill:#fed7aa,stroke:#ea580c,stroke-width:1.5px,color:#7c2d12
    style F fill:#fde68a,stroke:#d97706,stroke-width:1.5px,color:#78350f
    style G fill:#fef08a,stroke:#ca8a04,stroke-width:1.5px,color:#713f12

    style H1 fill:#e0f2fe,stroke:#0284c7,stroke-width:2px,color:#0c4a6e
    style H2 fill:#dcfce7,stroke:#16a34a,stroke-width:2px,color:#14532d
    style H3 fill:#fae8ff,stroke:#c026d3,stroke-width:2.5px,color:#701a75

    style I1 fill:#f1f5f9,stroke:#64748b,color:#334155
    style I2 fill:#f1f5f9,stroke:#64748b,color:#334155
    style I3 fill:#22c55e,stroke:#15803d,stroke-width:2px,color:#ffffff,font-weight:bold
```

---

### Model Comparison Summary

| Model / Classifier | Accuracy | Precision (Macro Avg) | Recall (Macro Avg) | F1-Score (Macro Avg) |
| :--- | :---: | :---: | :---: | :---: |
| **Deep Neural Network (DNN)** | **79.17%** | **0.79** | **0.79** | **0.79** |
| **Random Forest (RF)** | 79.00% | 0.79 | 0.79 | 0.79 |
| **Support Vector Machine (SVM - RBF)** | 75.00% | 0.76 | 0.75 | 0.75 |

---

## Key Findings & Results

* **Feasibility:** Diffusion architectures leave distinct, measurable statistical and structural signatures in their outputs.
* **Best Performer:** The **Deep Neural Network (DNN)** achieved the highest performance with an overall accuracy of **79.17%**, outperforming traditional ensemble and margin-based models.
* **Top Discriminative Features:** Feature importance analysis highlighted color distribution, contrast variance (luminance channel), sharpness (Laplacian variance), and edge density (Canny filtering) as major indicators.

---

## Dataset & References. 

* **Dataset:** Synthbuster Benchmark Dataset ([Zenodo Link](https://zenodo.org/records/10066460))
* **Primary Reference:**  Q. Bammey, "Synthbuster: Towards Detection of Diffusion Model Generated Images," in *IEEE Open Journal of Signal Processing*, vol. 5, pp. 1-9, 2024, doi: [10.1109/OJSP.2023.3337714](https://doi.org/10.1109/OJSP.2023.3337714).
