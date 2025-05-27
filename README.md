# OSR-Classifier

A PyTorch-based project for **Open-Set Recognition (OSR)** using a **Multi-task Autoencoder (MT-AE)** trained on the MNIST dataset to classify in-distribution samples and detect out-of-distribution (OOD) inputs.

## 🔍 Overview

This project explores methods for improving OSR performance by combining classification and reconstruction tasks into a unified autoencoder architecture. The model is trained to:

* **Classify** digits from the MNIST dataset.
* **Reconstruct** input images.
* **Detect OOD samples** by analyzing reconstruction differences.

## 🧠 Motivation

This project investigates a lightweight approach suitable for limited computational resources.

Previous experiments included content/style loss (from Neural Style Transfer theory) for threshold, but were ultimately replaced by a more stable reconstruction-based method.

## 🏗️ Architecture

### Multi-task Autoencoder (MT-AE)

* **Encoder**: CNN-based encoder extracting features.
* **Decoder**: Transpose convolutions reconstructing the image.
* **Classifier**: Fully connected layers for digit classification.

The Multi-task Autoencoder (AE) model combines a simple classifier for classifying MNIST digits with an Autoencoder (AE) to help distinguish between in-distribution (MNIST) and out-of-distribution (OOD) samples.
First, the AE encodes the input images. Then, the AE reconstructs the encoded vector to an image (with the decoder), and classifies it into one of the known classes.

### Loss Functions

* **Cross Entropy Loss**: For classification.
* **MSE Loss**: For image reconstruction.

### Thresholding

* **Norm Loss**: Euclidean distance between original and reconstructed images.
* **Diff Loss**: Binary image difference metric.
* Thresholds computed as `mean + 2*std` from training MNIST samples.

## ⚙️ Hyperparameters

| Parameter     | Value  |
| ------------- | ------ |
| Batch Size    | 1024   |
| Epochs        | 20     |
| Learning Rate | 0.0005 |
| Optimizer     | Adam   |
| Activation    | ReLU   |
| Classes       | 11     |

## 📊 Performance

| Model                  | MNIST Accuracy | OOD Accuracy | Total Accuracy |
| ---------------------- | -------------- | ------------ | -------------- |
| **MT-AE (Final)**      | 96.24%         | 95.42%       | 95.69%         |
| Style/Content Loss Ver | 92.33%         | 88.99%       | 90.66%         |
| Baseline Classifier    | 80.99%         | N/A          | N/A            |

## ✅ Strengths

* High accuracy on in-distribution and OOD samples.
* Efficient thresholding without relying on heavy metrics.
* Lightweight and easy to train on modest hardware.

## ⚠️ Limitations

* Performance may degrade with complex or natural images.
* Sensitive to OOD samples visually similar to MNIST digits.
* Designed for datasets with clear separation between known and unknown classes.
