Perfect. Below is a **clean, professional GitHub README** written **exactly at the level a professor + recruiter expects**, but still **easy to understand**.
You can paste this directly into `README.md`.

---

# 📄 MonReader — Page Flip Detection from a Single Image

## Project Overview

MonReader is a computer vision project focused on **detecting whether a document page is being flipped using a single image**.

This problem comes from real-world mobile document scanning systems, where detecting the *exact moment* a page is flipped is critical to capturing high-quality scans automatically — especially for accessibility use cases such as assisting visually impaired users.

Unlike traditional motion detection that relies on video, this project intentionally works with **single frames only**, making the task significantly more challenging and more practical for mobile devices.

---

## Problem Statement

Given an image extracted from a smartphone camera preview, predict whether the page is:

* **Flipping** (`1`)
* **Not flipping** (`0`)

The model must make this decision **without seeing past or future frames**.

---

## Why This Is Hard (In Simple Terms)

Humans use motion to detect page flips.
This model must learn **visual cues that suggest motion**, such as:

* Page curvature or bending
* Blurred edges
* Partial page exposure
* Hand interaction patterns

All from **one still image**.

---

## Dataset Description

* Data consists of labeled images extracted from page-flipping videos
* Folder structure:

  ```
  images/
    ├── training/
    │   ├── flip/
    │   └── notflip/
    └── testing/
        ├── flip/
        └── notflip/
  ```

Each image filename follows the format:

```
VideoID_FrameNumber.jpg
```

This allows frames from the same video to be grouped together.

---

## ⚠️ Data Leakage Prevention (Important)

To avoid training on frames from the same video that appear in validation:

* Frames are grouped by `video_id`
* Entire videos are assigned to **either training or validation**
* No video appears in both splits

This ensures the model is evaluated on **truly unseen data**.

---

## Approach Summary

### 1. Metadata Construction

* Each image is represented as a row containing:

  * File path
  * Label (flip / notflip)
  * Video ID
  * Validation flag

### 2. Dataset & DataLoader

* Custom PyTorch `Dataset` loads images **on demand**
* Images are converted to tensors and normalized
* Training data uses light augmentation
* Validation data remains deterministic

### 3. Model

* Pretrained **ResNet-18**
* Final layer replaced with **one output neuron**
* Uses **sigmoid logic** for binary classification

### 4. Loss Function

* `BCEWithLogitsLoss`
* Combines sigmoid + binary cross-entropy
* Numerically stable and standard for binary tasks

### 5. Metric

* **F1 Score**
* Chosen to balance:

  * Missing page flips (false negatives)
  * Triggering scans at the wrong time (false positives)

---

## Training Results (Example)

```
Epoch 1/5 | Train F1: 0.79 | Val F1: 0.88
Epoch 2/5 | Train F1: 0.89 | Val F1: 0.88
Epoch 3/5 | Train F1: 0.89 | Val F1: 0.88
Epoch 4/5 | Train F1: 0.89 | Val F1: 0.87
```

### Interpretation (Plain English)

* The model quickly learns meaningful visual patterns
* Validation performance remains stable
* No major overfitting observed
* The model generalizes well to unseen videos

---

## Why F1 Score?

In this application:

* **Missing a page flip** = bad user experience
* **False triggers** = wasted scans

F1 score balances both concerns better than accuracy.

---

## Key Design Decisions

* Single-image prediction (not video-based)
* Video-aware data splitting
* Lightweight augmentation
* Simple, explainable baseline model

---

## Technologies Used

* Python
* PyTorch
* Torchvision
* TorchMetrics
* Pandas
* PIL

---

## Future Improvements

* Sequence-based modeling (frame aggregation)
* Temporal smoothing at inference
* Confidence-based triggering
* Lightweight mobile deployment

---

## Final Notes

This project focuses on **clarity, correctness, and real-world constraints** rather than over-engineering.
Every design choice is intentional and defensible in both technical and non-technical discussions.

---

If you want next:

* 🔹 a **shorter README**
* 🔹 a **“For Recruiters” section**
* 🔹 or a **demo script explanation**

Just say the word.
