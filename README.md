# 🚀 Planetary Image Enhancement using Residual CNN (ResNet-Based)

This project presents a residual CNN-based image enhancement pipeline for planetary surfaces, particularly focused on Mars and crater regions. The model enhances satellite image quality using patch-based training, residual learning, and reflection padding to preserve texture and structural continuity while eliminating patch artifacts.

---

## 📂 Dataset Overview

The dataset is structured into raw planetary patches and their corresponding enhanced (ground truth) versions. Patches are of size **512×512**.

### 🔹 Raw Input Patches (To be Enhanced)
- `Patches_mars/`  
- `Patches_crater/`  

### 🔹 Ground Truth (Enhanced) Patches
- `Patches_enhanced_mars/`  
- `Patches_enhanced_crater/`  
- `Patches_enhanced_mars2/`  

These datasets are used to train the model to learn the transformation from raw to enhanced planetary imagery.

---

## 🗂️ Folder Structure

- Patches_mars/ # Raw Mars patches
- Patches_crater/ # Raw Crater patches
- Patches_enhanced_mars/ # Ground Truth enhanced Mars
- Patches_enhanced_crater/ # Ground Truth enhanced Crater
- Patches_enhanced_mars2/ # Additional enhanced Mars data
- Enhanced_mars_resnet/ # Output of ResNet-enhanced Mars
- Enhanced_crater_resnet/ # Output of ResNet-enhanced Crater
- Patches_mars13/ # Test image patches (mars13.jpg)
- Enhanced_mars13_resnet/ # Enhanced patches from test image
- mars13.jpg # Raw large image
- resnet_mars13.png # Final stitched enhanced output
- mars_enhancement_resnet.pth # Trained ResNet model
- Chart.png # Metrics visualization chart


---

## 🧠 Model Architecture – Residual CNN (ResNet)

This CNN model uses a deep residual learning approach to enhance planetary images patch-by-patch. Key components:

- **Input**: 512×512 RGB patches  
- **Reflection Padding**: Prevents border artifacts during patch convolution  
- **Residual Blocks**: 8 blocks with skip connections to retain features and ease optimization  
- **Batch Normalization & ReLU**: Used after each convolution  
- **Final Output**: 3-channel enhanced patch with sigmoid activation

The model is optimized using **Mean Squared Error loss** and the **Adam optimizer** with a learning rate of `1e-4`. Trained for **50 epochs** with batch size 6.

---

## 🔁 Enhancement Pipeline

1. **Image Splitting**  
   - Large raw images (e.g., `mars13.jpg`) are split into non-overlapping 512×512 patches using `split_image_smooth`.

2. **Patch Enhancement**  
   - Each patch is passed through the trained ResNet model. Output patches are normalized and saved.

3. **Stitching with Gaussian Feathering**  
   - Enhanced patches are stitched into the full image using `final_smooth_stitch` with **Gaussian weight masks** to blend overlapping regions and avoid visible seams.

---

## 📊 No-Reference Image Quality Metrics

| Image      | BRISQUE ↓ | NIQE ↓ | PIQE ↓ | Entropy ↑ | SNR ↑   | HVS Sharpness ↑ |
|------------|-----------|--------|--------|-----------|---------|------------------|
| original   | 1.56      | 1.10   | 99.17  | 5.70      | 53.78   | 1.32             |
| GT         | 0.60      | 0.56   | 17.55  | 6.79      | 39.55   | 173.82           |
| unet       | 0.38      | 0.38   | 40.33  | 6.64      | 48.98   | 11.09            |
| resnet     | 0.48      | 0.49   | 51.18  | 7.29      | 40.56   | 81.94            |
| gan        | 0.61      | 0.62   | 12.77  | 6.78      | 29.66   | 282.74           |

> 🔺 **↑ Higher is better**, 🔻 **↓ Lower is better**  
> ✅ ResNet achieves **highest entropy**, **balanced SNR**, and **strong sharpness** while maintaining low BRISQUE and NIQE values.  
> ✅ GAN shows extremely high HVS sharpness but lower SNR.  
> ✅ U-Net gives lowest BRISQUE but slightly lower entropy than ResNet.

---

## 📊 Chart Visualization

The comparison of metrics across models is illustrated below:

![Metrics Chart](Chart.png)

- **Top Row (↑ Higher is Better)**:
  - **Entropy**: ResNet achieves the highest, indicating rich texture.
  - **SNR**: ResNet is competitive, balancing sharpness with noise control.
  - **HVS Sharpness**: GAN is highest; ResNet balances detail with naturalness.

- **Bottom Row (↓ Lower is Better)**:
  - **BRISQUE & NIQE**: U-Net performs best, followed by ResNet.
  - **PIQE**: GAN achieves lowest distortion; ResNet performs moderately well.

These metrics confirm that ResNet maintains a strong trade-off between sharpness, texture richness, and natural appearance.

---

## 🧠 Interpretation & Highlights

- **Entropy (7.29)**: Indicates that ResNet enhances surface texture more than all models.
- **BRISQUE (0.48)**: Competitive natural image quality, close to U-Net.
- **SNR (40.56)**: Shows ResNet balances sharpness and denoising well.
- **HVS (81.94)**: Outperforms U-Net significantly in perceptual sharpness.
- **Visual Quality**: ResNet images show no tile lines, enhanced clarity, and improved contrast.

---

## 🧾 Conclusion

The **ResNet-based CNN enhancement model** effectively improves planetary imagery by:

- Enhancing **surface textures** and **shadows** on Martian landscapes
- Avoiding **patch boundary artifacts** using **reflection padding** and **Gaussian blending**
- Producing **sharp, clear, and natural-looking** full images
- Achieving **high entropy and perceptual sharpness** with low distortions

When compared to U-Net and GAN:
- **ResNet provides the best texture enhancement (entropy)**
- **Balances naturalness, sharpness, and smooth transitions**
- **Excels in scenarios requiring both structure and realism**

This makes it highly suitable for planetary science, cartography, and research visualization of extraterrestrial terrains.

---

