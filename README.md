# DermaSense: AI-Powered Skin Lesion Analysis & Bias Mitigation

**DermaSense** is a deep learning project designed to address the critical issue of racial bias in skin cancer diagnostics. Standard medical datasets (like ISIC) are heavily skewed towards light skin tones, causing AI models to fail when diagnosing darker skin (Fitzpatrick Type V & VI).

This project introduces a **Synthetic Bias Mitigation Pipeline** that uses Generative AI (Stable Diffusion XL) to create medically accurate synthetic data for underrepresented skin tones, proving that diverse training data significantly improves diagnostic safety and recall.

## 👥 Team

* **Afik Haviv**
* **Shir Molakandove**
* **Romi Yosef**

## 🚀 Project Pipeline

Our workflow consists of three main stages:

### 1. Synthetic Data Generation (The Fix)

* **Segmentation:** We use `rembg` (U²-Net) to isolate lesions from their original light-skin backgrounds.
* **Texture Generation:** We use **SDXL (Stable Diffusion XL)** with medical prompting to generate high-fidelity dark skin textures (Fitzpatrick VI).
* **Augmentation:** We use **Poisson Blending** and **Histogram Matching** to seamlessly fuse original lesions onto new dark skin backgrounds, preserving medical accuracy while changing the context.

### 2. Model Training (The Experiment)

* **Baseline Model (Biased):** A ResNet50 model trained *only* on the original ISIC 2016 dataset (Light Skin).
* **Diverse Model (Ours):** An identical ResNet50 model trained on a mixed dataset (ISIC + Our Synthetic Dark Skin Data).
* **Class Weighting:** We apply calculated class weights to the Loss Function to prevent "Model Collapse" and force the model to prioritize malignant cases.

### 3. Validation (The Proof)

* We evaluate both models head-to-head on a held-out test set of **Dark Skin Images**.
* **Result:** The Diverse Model demonstrates significantly higher **Recall** and **F1-Score**, proving it is safer and more reliable for diverse populations.

## 🛠️ Installation

**1. Clone the repository:**
```text
git clone https://github.com/AfikHaviv/DermaSense.git
cd DermaSense
```
**2. Create a Virtual Environment (Recommended):**
```text
python -m venv venv
```
# Windows:
```text
.\venv\Scripts\activate
```
# Mac/Linux:
```text
source venv/bin/activate
```

**3. Install Dependencies:**

*Note: This project requires a GPU (NVIDIA RTX recommended) for the Generative AI components.*
```text
pip install -r requirements.txt
```
## 📂 Folder Structure
```text
DermaSense/
│
├── data/
│   ├── raw/                  # Original ISIC 2016 Data (Download here)
│   ├── processed/            # Organized into benign/malignant folders
│   ├── masks/                # Generated binary masks
│   └── final_augmented_dataset/ # The output: Synthetic Dark Skin images
│
├── DermaSense.ipynb     # Main Project Notebook (Run this!)
|
├── requirements.txt     # Project dependencies
│
└── README.md
```
## 🏃‍♂️ How to Run

1.  Open `DermaSense.ipynb` in VS Code or Jupyter Lab.
2.  Run the cells in order. The notebook is structured to:
    * **Step 1-3:** Prepare data and generate masks.
    * **Step 4-5:** Generate synthetic backgrounds and blend images.
    * **Step 6:** Train the Baseline (Biased) Model.
    * **Step 7-8:** Train the Diverse (Weighted) Model.
    * **Step 9:** Run the final evaluation graph.

## 📊 Results Snapshot

| Metric | Biased Model (Standard) | Diverse Model (Ours) | Improvement |
| :--- | :--- | :--- | :--- |
| **Accuracy** | ~82% | **~95%** | **+13%** |
| **Recall** | ~54% | **~96%** | **+42%** |
| **F1-Score** | ~52% | **~92%** | **+40%** |

*Results based on evaluation on the held-out Dark Skin Test Set.*