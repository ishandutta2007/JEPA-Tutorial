# Chapter 14: Downstream Task: Image Classification

The ultimate goal of self-supervised learning is to produce representations that are useful for a wide range of downstream tasks. In this chapter, we will focus on one of the most common and important downstream tasks: **image classification**.

## From Pre-training to Fine-tuning

After pre-training I-JEPA on a large, unlabeled dataset like ImageNet, we are left with a powerful feature extractor—the context encoder. This encoder has learned to produce high-quality, semantic representations of images. Now, we can leverage these learned representations for image classification.

There are two main approaches for using a pre-trained I-JEPA model for image classification:

### 1. Linear Probing

Linear probing is a common method for evaluating the quality of self-supervised representations. It involves freezing the weights of the pre-trained encoder and training a simple linear classifier on top of its output.

-   **How it works:**
    1.  Freeze the weights of the pre-trained I-JEPA context encoder.
    2.  Add a new, randomly initialized linear classification head to the model.
    3.  Train *only* the classification head on a labeled dataset (e.g., ImageNet with labels).
-   **What it tells us:** The performance of the linear probe is a direct measure of the "linearity" of the learned representations. If the representations are well-separated in the embedding space, even a simple linear classifier should be able to achieve high accuracy.

### 2. Fine-tuning

Fine-tuning involves unfreezing the entire pre-trained model and training it end-to-end on the downstream task with a small learning rate.

-   **How it works:**
    1.  Replace the pre-training head with a new classification head.
    2.  Unfreeze the weights of the entire model (or a portion of it).
    3.  Train the entire model on the labeled dataset with a small learning rate.
-   **What it tells us:** Fine-tuning allows the model to adapt its learned features to the specific nuances of the downstream task. This typically results in higher performance than linear probing, as the model has more flexibility to adjust its representations.

## The I-JEPA Advantage in Image Classification

I-JEPA has demonstrated several key advantages for image classification tasks:

-   **High Accuracy:** I-JEPA has achieved state-of-the-art or competitive results on a variety of image classification benchmarks, including ImageNet-1K.
-   **Label Efficiency:** Because I-JEPA learns from unlabeled data, it can achieve high accuracy with a smaller amount of labeled data for fine-tuning. This is particularly valuable in domains where labeled data is scarce or expensive to obtain.
-   **Robustness:** The semantic representations learned by I-JEPA are often more robust to variations in input data, leading to better generalization on unseen examples.
-   **Reduced Bias:** By avoiding heavy data augmentations during pre-training, I-JEPA can produce representations with less inherent bias, which can be beneficial for fairness and robustness in downstream applications.

## Example: Fine-tuning on ImageNet

To fine-tune a pre-trained I-JEPA model on ImageNet for classification, you would:

1.  Load the weights of the pre-trained I-JEPA context encoder.
2.  Append a linear layer with 1,000 output units (for the 1,000 ImageNet classes) and a softmax activation function.
3.  Train the model on the labeled ImageNet training set using a standard cross-entropy loss function.
4.  Evaluate the model's performance on the ImageNet validation set.

In the next chapter, we will explore another common downstream task: object detection.