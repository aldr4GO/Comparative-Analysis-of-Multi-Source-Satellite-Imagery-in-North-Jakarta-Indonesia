<!--META
{
  "title": "Efficient Net-Based LULC Classification",
  "desc_portfolio": "Trained EfficientNet models, achieving up to 98% validation accuracy in land cover classification."
}
META-->
![thumbnail.png](https://github.com/aldr4GO/Comparative-Analysis-of-Multi-Source-Satellite-Imagery-in-North-Jakarta-Indonesia/blob/main/thumbnail.png)

# 🛰️ Comparative Analysis of Multi-Source Satellite Imagery in North Jakarta, Indonesia

A comprehensive study comparing PlanetScope (optical) and Sentinel-1 (radar) satellite imagery over a 5x5 km area in North Jakarta. This project evaluates spectral behavior, edge detection performance, and applies deep learning via EfficientNet for object classification.

---

## 📌 Objectives

- Compare spatial and spectral capabilities of multi-source satellite data.
- Perform statistical and edge-based image analysis.
- Use EfficientNet CNN models for object detection in satellite imagery.

---

## 🗂️ Data Sources

| Source       | Type     | Description                                                                 |
|--------------|----------|-----------------------------------------------------------------------------|
| PlanetScope  | Optical  | 4-band (RGB + NIR), 3m resolution image (Dec 20, 2024), 5–10% cloud cover.  |
| Sentinel-1   | Radar    | Dual-pol SAR GRD data (Dec 18, 2024), processed with SNAP toolbox.          |

---

## ⚙️ Preprocessing Pipeline

### 🛰 Satellite Image Preparation

- **PlanetScope (Optical)**:
  - DN to surface reflectance conversion.
  - Visual band combination and normalization using NumPy and Matplotlib.

- **Sentinel-1 (Radar)**:
  - Preprocessing via SNAP Toolbox:
    - Orbit correction
    - Radiometric calibration
    - Speckle filtering
    - Terrain correction
  - Post-processed in Python:
    - Converted radar labels: foreground class `1`, background as `255`.
    - Normalized and saved as 8-bit TIFFs using `rasterio`.


### 🧩 Patch Creation for Model Training

- To train the EfficientNet model, raster images and corresponding labels are divided into fixed-size patches.
  
| Source       | No. of patches| Dimensions | Bands                              |
|--------------|---------------|------------|------------------------------------|
| PlanetScope  | 193		       | 4×224×224  | Red, Green, Blue, NIR              |
| Sentinel-1   | 256           | 3×60×60    | Sigma0_VH, Sigma0_VV, Sigma0_VV_db |

- **Steps**:
  1. Input satellite images and label masks are sliced into 50% overlapping patches (`224×224` or `60×60`).
  2. Patches with meaningful class presence (foreground pixels) are retained.
  3. Patches are saved as `.npz` files with associated class labels.

- PS: These patches are used as input to the EfficientNet models for training and evaluation.
---

## 📊 Statistical Analysis

### 🌈 Reflectance Distribution (PlanetScope)
<img width="975" height="625" alt="image" src="https://github.com/user-attachments/assets/1f08e017-8d62-4b45-9adc-ba1a862519f7" />


- NIR and Red bands highlight vegetation and urban structures distinctly.
- Reflectance values normalized to 0–1 range.

### 📶 Backscatter Analysis (Sentinel-1)
<img width="975" height="628" alt="image" src="https://github.com/user-attachments/assets/c9ee1c8f-7aa2-4965-8f19-4e755214cfb5" />


- VV polarization shows higher variation, capturing structural complexity.
- VH polarization is less intense and useful for differentiating vegetation.

---

## 🧱 Edge Detection

### 🌇 PlanetScope – Canny Edge Detection

<img width="975" height="520" alt="image" src="https://github.com/user-attachments/assets/ecd04293-0ba4-48b5-a5f2-81ddde1b1d17" />

- Sharp, continuous edges around roads, buildings, and coastlines.
- Benefited from high spatial and spectral resolution.

### 🌃 Sentinel-1 – Canny Edge Detection
<img width="975" height="485" alt="image" src="https://github.com/user-attachments/assets/1d1db01a-f123-4dfa-883f-6b672f1c0b46" />



- Strong radar reflections from urban blocks and coastline changes.
- Edge clarity impacted by speckle and texture noise.

---

## 🧠 Deep Learning with EfficientNet

### ✅ EfficientNet Model Architecture
<img width="1342" height="409" alt="image" src="https://github.com/user-attachments/assets/40c605e4-bfc3-4ba4-b1a9-3d2766635e4a" />


- Used EfficientNet-B0 to B5 variants.
- Compound scaling balances depth, width, and resolution.

### 📈 Evaluation Metrics

| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| 0     | 1.00      | 0.96   | 0.98     | 28      |
| 1     | 0.92      | 1.00   | 0.96     | 12      |
| **Accuracy** | **–** | **–** | **0.97** | **40** |

---

## 📌 Key Observations

| Feature       | PlanetScope (Optical) | Sentinel-1 (Radar)     |
|---------------|------------------------|-------------------------|
| Urban Areas   | High visual clarity     | Strong radar return     |
| Vegetation    | NIR differentiation     | Moderate texture edges  |
| Water Bodies  | Low reflectance, clear  | Very dark (low return)  |
| Weather Impact| Cloud-sensitive         | All-weather capable     |

---

## 🧠 Future Directions

- Fusion of optical and radar imagery for hybrid modeling.
- Temporal monitoring using time-series analysis.
- Advanced unsupervised segmentation and change detection.

---

## 🛠 Tech Stack

- Python, PyTorch, Matplotlib, NumPy, Pandas
- SNAP Toolbox (ESA)
- Docker (for PlanetScope preprocessing)
- EfficientNet (via `efficientnet_pytorch`)

---

## 👥 Contributors

- **Tanish Verma** – [tanish_v@mfs.iitr.ac.in](mailto:tanish_v@mfs.iitr.ac.in)
- **Sargam Goyal** – [sargam_g@mfs.iitr.ac.in](mailto:sargam_g@mfs.iitr.ac.in)
- **Priyani Nagle** – [priyani_n@mfs.iitr.ac.in](mailto:priyani_n@mfs.iitr.ac.in)
- **Shradhdha Agrawal** – [agrawal_ss@mfs.iitr.ac.in](mailto:agrawal_ss@mfs.iitr.ac.in)

---

## 📎 Resources

📄 [Project Report (PDF)](./Comparative-Analysis-of-Multi-Source-Satellite-Imagery-in-North-Jakarta-Indonesia.pdf)<br>
📄 [EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks](https://arxiv.org/abs/1905.11946)
<!--📁 Datasets: PlanetScope and Sentinel-1 (publicly accessible via Planet Labs and ASF DAAC) -->

---

<!-- 
## 🖼️ Notes

📌 **Add the following images to the `/images/` directory:**

- `preprocessing_pipeline.png`
- `boxplot_planetscope.png`
- `boxplot_sentinel1.png`
- `canny_planetscope.png`
- `canny_sentinel1.png`
- `efficientnet_scaling.png`

---

> *This study demonstrates the value of integrating diverse satellite datasets for robust urban analysis and deep learning-based feature classification.* -->

