### Methods (As We Did)

To solve the challenges of mitosis detection, we developed a robust pipeline called **FMDet**, which consists of three main components: **Fourier-based data augmentation**, **segmentation mask generation**, and a **segmentation-based network architecture**.

#### 1. **Fourier-Based Data Augmentation**
We started by addressing the domain shift problem using Fourier-based data augmentation. The motivation was that variations in the low-frequency amplitude spectrum do not affect high-level semantic features. 

We performed the following steps:
- Applied a **Fast Fourier Transform (FFT)** to images from both source and reference domains.
- Replaced the low-frequency spectrum of the source domain images with that from the reference domain, while keeping the phase spectrum unchanged.
- Used an inverse FFT to reconstruct the augmented images, which retained the label of the original source image but reflected the style of the reference domain.

This augmentation created a diverse set of training images without additional manual labeling and ensured that the model learned to generalize across unseen domains.

#### 2. **Segmentation Mask Generation**
Since the provided annotations were bounding boxes, we transformed these into **pixel-level segmentation masks**:
- We utilized a pre-trained **HoVer-Net** model to generate precise nuclear boundaries from the histopathological images.
- By intersecting these nuclear boundaries with the bounding box annotations, we obtained refined pixel-level segmentation masks, focusing only on the mitotic nuclei.

This step allowed us to improve detection precision by focusing on the exact mitotic regions rather than coarse bounding boxes.

#### 3. **Segmentation-Based Network Architecture**
We designed a custom segmentation model based on the **U-Net architecture** and enhanced it with attention mechanisms to capture robust features:
- For the **encoder**, we used **SE-ResNeXt50**, which integrates Squeeze-and-Excitation (SE) modules to dynamically emphasize important feature channels.
- For the **decoder**, we added **Selective Kernel (SK) modules**, which adaptively adjust receptive field sizes by aggregating features from multiple convolutional kernel sizes.

These components allowed us to extract and refine multi-scale features, ensuring that the model accurately identified mitoses, even with variations in cell morphology, size, and texture.

#### 4. **Loss Functions**
We trained the model using a combination of **focal loss** and **Dice loss**:
- **Focal Loss**: Addressed the imbalance between mitotic and non-mitotic samples by penalizing easy predictions more heavily.
- **Dice Loss**: Ensured high overlap between predicted and ground truth segmentation masks, improving boundary accuracy.

The final loss function was a weighted sum of these two components, optimizing the network to balance detection accuracy and segmentation quality.

Would you like me to proceed with the **Experimental Setup** or another section?
