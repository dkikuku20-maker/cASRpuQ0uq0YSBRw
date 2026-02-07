

# 📄 MonReader — Page Flip Detection from a Single Image

## 🔍 Project Overview

**MonReader** is a computer vision project that detects whether a book page is being flipped using **a single image frame**.
The goal is to support **automatic document scanning apps**, where detecting the exact moment of a page flip allows the app to capture clean scans without manual input.

This project focuses on **real-world usability**, not just model accuracy.

---

## 💼 Business Problem

Manual document scanning is slow and error-prone, especially for:

* Blind or visually impaired users
* Researchers scanning large documents
* Mobile users flipping pages quickly

**Problem:**
How can a system automatically detect *when* a page is being flipped, using only one image at a time?

**Why this matters:**

* Reduces user effort
* Enables hands-free scanning
* Improves scan quality and speed
* Supports accessibility-focused applications

---

## 📊 Dataset Description

* Data collected from **smartphone videos** of page flipping
* Videos were split into **individual image frames**
* Each frame labeled as:

  * `flip` → page is actively flipping
  * `notflip` → page is static
* File naming format:
  `VideoID_FrameNumber.jpg`

### Key design choice

All frames from the **same video stay in the same data split** to avoid data leakage.

---

## 🎯 Project Goal

> Predict whether a page is being flipped using **a single image frame**.

---

## 📈 Success Metric

**F1 Score** (higher is better)

Why F1?

* Balances **precision** (are flip predictions correct?)
* Balances **recall** (are most flips detected?)
* Appropriate for real-world decision systems

---

## 🧠 Approach & Methodology

### 1️⃣ Data Preparation

* Extracted file paths and labels into a metadata table
* Grouped frames by video ID
* Split data into:

  * **Training set** (learning)
  * **Validation set** (evaluation on unseen videos)

---

### 2️⃣ Model

* **ResNet-18 (pretrained on ImageNet)**
* Final classification layer replaced with a **binary output**
* Transfer learning used to:

  * Reduce training time
  * Improve performance with limited data

---

### 3️⃣ Training Strategy

* Trained in **mini-batches**
* One **epoch** = one full pass through the training data
* During training:

  * Model learns from labeled images
* During validation:

  * Model is evaluated on unseen data
  * No learning happens

---

### 4️⃣ Evaluation

At each epoch we track:

* Training Loss
* Training F1 Score
* Validation Loss
* Validation F1 Score

This helps detect:

* Learning progress
* Overfitting
* Generalization quality

---

## 📊 Model Performance (Validation)

```
Epoch 1/5 | Train Loss: 0.6251 | Train F1: 0.7870 | Val F1: 0.8839
Epoch 2/5 | Train Loss: 0.4940 | Train F1: 0.8871 | Val F1: 0.8839
Epoch 3/5 | Train Loss: 0.4720 | Train F1: 0.8863 | Val F1: 0.8839
Epoch 4/5 | Train Loss: 0.4478 | Train F1: 0.8866 | Val F1: 0.8761
```

### Interpretation (Non-Technical)

* The model **learns quickly**
* Validation performance stays strong → **good generalization**
* Slight fluctuation is normal in real-world data
* No major overfitting observed

---

## 🧪 Why No Separate Test Set?

For this assignment:

* Validation set already contains **unseen videos**
* Follows common industry practice when test labels are not required
* Model performance is measured fairly and safely

---

## 🛠️ Technologies Used

* Python
* PyTorch
* TorchVision
* TorchMetrics
* Pretrained CNNs (ResNet-18)
* Jupyter Notebook

---

## ▶️ How to Run the Project

```bash
git clone https://github.com/your-username/MonReader.git
cd MonReader
pip install -r requirements.txt
jupyter notebook MonReader.ipynb
```

---

## 🔮 Future Work

* Extend from single image → **video-level predictions**
* Aggregate frame predictions across time
* Export predictions to structured formats (e.g., JSON)
* Deploy as a mobile or API-based service

---

## 👤 Author

**Dessailly Kikuku**
Machine Learning & Data Science
Focused on **human-centered AI and accessibility**

🔗 LinkedIn: *(add link)*
🔗 Portfolio: *(add link)*


* Help you answer **“Do you have customers?”** professionally
* Create a **non-technical demo script for recruiters**

You’re not just done — you’ve built something **defensible, explainable, and real** 💪
