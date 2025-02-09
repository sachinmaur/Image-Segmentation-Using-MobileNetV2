# Image Segmentation Using MobileNetV2
## Introduction
Image segmentation is a fundamental task in computer vision, where each pixel in an image is classified as either part of the foreground (region of interest) or the background. Applications of segmentation include:

- **Medical imaging** (e.g., skin lesion detection)
- **Autonomous driving** (e.g., lane detection, obstacle segmentation)
- **Object detection**

This project leverages **MobileNetV2**, a lightweight pre-trained encoder, for feature extraction, and a **custom decoder** to generate pixel-wise segmentation masks.

---

## Dataset
The dataset used in this project is the **ISIC (International Skin Imaging Collaboration) dataset**, which is widely used for skin lesion segmentation tasks. It contains dermoscopic images labeled with binary segmentation masks.


### Dataset Details
- **Training Set:** 900 images (128×128 resolution)
- **Testing Set:** 379 images (128×128 resolution)
- **Train/Test Masks:** Corresponding binary segmentation masks

### Data Preprocessing
- **Image Preprocessing:**
  - Resized all images to **128×128** resolution.
  - Normalized using mean `[0.485, 0.456, 0.406]` and standard deviation `[0.229, 0.224, 0.225]`.
  - Applied data augmentation techniques, including **random horizontal flipping** and **rotations (±60°)**.
- **Mask Preprocessing:**
  - Resized to match the image dimensions.
  - Applied the same augmentations as images to maintain correspondence.

### Dataset Visualization
Below is an example of a dataset sample:
- **Left:** Original image
- **Right:** Corresponding ground truth mask
![1](https://github.com/user-attachments/assets/2bcdea16-2a8a-440c-b233-9e56f8e443e1)
---

## Model Architecture
### Encoder
- MobileNetV2 (pre-trained on ImageNet) extracts feature maps of size **4×4×1280**.
- Two configurations:
  - **Model 1:** Frozen encoder (weights not updated during training)
  - **Model 2:** Fine-tuned encoder (weights updated during training)

### Decoder
- **Five convolutional layers** (1280 → 512 → 256 → 64 → 32 → 16)
- **Bilinear upsampling layers** for spatial resolution recovery
- **Batch normalization, ReLU activation, and dropout layers** for regularization
- **Final 1×1 convolution** with sigmoid activation for binary segmentation

**Observations:**
- Model 2 achieves better segmentation accuracy and generalization.
- Fine-tuning the encoder significantly improves IoU and Dice Score.
- Model 2 has lower training and validation loss, indicating better convergence.

---

## Conclusion
- **Model 2 consistently outperforms Model 1** across all metrics.
- Fine-tuning the MobileNetV2 encoder improves segmentation performance.
- Future work will explore:
  - **Advanced loss functions** to enhance segmentation accuracy.
  - **Multi-scale feature aggregation** for better feature representation.

