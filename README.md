# UniGaze Ablation Study: Professional Domain Priors vs. General Feature Fitting

This repository contains an ablation study comparing **UniGaze** (a domain-specific gaze estimation model) with state-of-the-art general-purpose vision models (**DINOv2** and **ViT**). The study explores the robustness of these models under constrained data environments and their ability to generalize physical geometric logic.

##  Project Overview
The notebook `UniGaze_Ablation_Study.ipynb` performs a **Linear Probing** experiment on the Olivetti Faces dataset. By freezing the backbones and training only a regression head, we evaluate how much "gaze-related knowledge" is inherently stored in each model's feature space.

### Key Features:
* **Automated Labeling**: Implementation of the **PnP (Perspective-n-Point)** algorithm to derive 3D gaze/head-pose labels from 2D facial landmarks.
* **Comparative Analysis**: Head-to-head comparison between UniGaze, DINOv2 (ViT-L/14), and standard ViT.
* **Robustness Testing**: Evaluation of model performance under noise injection to distinguish between "true understanding" and "dataset overfitting."
# UniGaze Ablation Study: Professional Domain Priors vs. General Feature Fitting

This repository contains an ablation study comparing **UniGaze** (a domain-specific gaze estimation model) with state-of-the-art general-purpose vision models (**DINOv2** and **ViT**). The study explores the robustness of these models under constrained data environments and their ability to generalize physical geometric logic.

---

##  How to Run

You can run this study directly in your browser using Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tcplasma/UniGaze_Ablation_Study-and-Thoughts/blob/main/UniGaze_Ablation_Study_.ipynb)

### Instructions:
1. Click the **"Open In Colab"** button above.
2. Ensure the Runtime Type is set to **GPU (T4 or higher)**.
3. Run the cells sequentially. The environment setup (Section 0) will automatically install necessary dependencies:
   ```bash
   pip install unigaze face-alignment timm
---

##  Critical Experimental Insights

### 1. Qualitative Superiority
While quantitative metrics (Angular Error) might show similar results across models in simple environments, **UniGaze** demonstrates superior **Qualitative Accuracy**. Its predicted gaze vectors align more naturally with human physiological intent, proving that its domain-specific pre-training captures the "physics of sight" rather than just "pixel patterns."

### 2. The PnP Labeling Paradox
The Ground Truth (GT) used in this notebook (PnP) primarily reflects **Head Pose**. 
* **Observation**: UniGaze is trained on high-precision **Gaze Trackers** in controlled laboratory settings (e.g., ETH-XGaze). 
* **Analysis**: When a subject's eyes move independently of their head, UniGaze's accurate tracking may be penalized by the simplified PnP labels. This highlights the limitation of using "Head Pose" as a proxy for "Gaze."

### 3. Feature Fitting vs. Physical Priors
By introducing noise into the dataset, the experiment reveals:
* **General Models (DINOv2/ViT)**: Act as "Image Statisticians." Their performance is high on clean data due to their ability to fit the specific distribution of the Olivetti dataset, but it degrades rapidly under noise.
* **UniGaze**: Acts as a "Gaze Physicist." It relies on stable, pre-learned **Physiological Priors**. It exhibits much higher **Robustness**, proving that it understands the invariant geometric structures of the human eye.

---

##  Tech Stack
* **Core Model**: UniGaze (Transformer-based)
* **Backbones**: DINOv2 (Meta AI), ViT (Timm)
* **Geometry**: OpenCV (solvePnP), Face-Alignment (FAN)
* **Framework**: PyTorch
