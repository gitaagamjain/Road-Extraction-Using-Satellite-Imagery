🛰️ Satellite Image Road Extraction

This repository contains a localized proof-of-concept for extracting road networks from satellite imagery using semantic segmentation. The project was developed as an educational capstone to explore the transition from traditional machine learning to deep learning for spatial data.

📊 Dataset

The data used is from the DeepGlobe Road Extraction Challenge.

Inputs: RGB satellite image patches.

Ground Truth: Binary masks (white for roads, black for background).

Note: Due to file size limits, the dataset is not included in this repository. It must be downloaded separately from Kaggle.

🧠 Methodology

Phase 1: The Baseline (Random Forest)

The project began by flattening the image pixels and training a Random Forest classifier based strictly on RGB color values. While it could identify asphalt color profiles, it lacked the spatial awareness to connect continuous lines or handle shadows, resulting in noisy, "salt-and-pepper" masks.

Phase 2: The U-Net Implementation (PyTorch)

To capture spatial geometry, the workflow transitioned to a Fully Convolutional Neural Network (U-Net).

Architecture: A standard U-Net modified to start at 32 channels instead of 64 to fit within standard hardware constraints.

Hardware: Trained on a single NVIDIA T4 GPU (via Google Colab).

Memory Management: Utilized PyTorch's DataLoader with micro-batching (batch size of 4) and Automatic Mixed Precision (AMP) to prevent VRAM overflow.

Loss & Metrics: Optimized using BCEWithLogitsLoss for numerical stability and evaluated using Intersection over Union (IoU).

Phase 3: Mitigation of Overfitting

An initial training run on 400 images resulted in heavy overfitting. The final model pipeline implements three standard regularization techniques to force generalization:

Dataset Expansion: Increased the training subset from 400 to 2,000 images.

Data Augmentation: Implemented runtime horizontal/vertical flips and color jittering to prevent exact pixel memorization.

L2 Regularization: Applied weight decay (1e-4) to the Adam optimizer.

⚠️ Project Limitations

Scale: This model was trained on a restricted subset of the total dataset due to compute limits. It is a Minimum Viable Product (MVP), not a production-ready model.

Edge Cases: The model outputs probability heatmaps that may still struggle with complex intersections, dirt roads, or roads heavily obscured by dense tree canopies.

🚀 How to Run

Clone this repository.

Open the .ipynb notebook in Google Colab.

Set the runtime hardware accelerator to T4 GPU (Runtime > Change runtime type).

Download the DeepGlobe dataset and update the path variables in the first cell to point to your local or Drive folder.

Execute the cells sequentially to initialize the pipeline, train the model, and generate visual heatmaps.
