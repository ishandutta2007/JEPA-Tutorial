# Chapter 18: V-JEPA Architecture

The Video Joint-Embedding Predictive Architecture (V-JEPA) adapts the core principles of JEPA to the complexities of the video domain. In this chapter, we will delve into the specific architectural components and design choices that make V-JEPA a powerful model for video representation learning.

## The Core Architecture

Like its image-based counterpart, V-JEPA is composed of three main components:

1.  **Context Encoder:** Processes the unmasked portion of a video clip (the context).
2.  **Target Encoder:** Processes the masked portion of the video clip (the target). This is a momentum encoder.
3.  **Predictor:** Predicts the representation of the target based on the representation of the context.

All three components are typically based on the Vision Transformer (ViT) architecture, which is well-suited for handling the spatiotemporal data in videos.

## Handling the Time Dimension

The key innovation in V-JEPA is how it handles the time dimension. Unlike I-JEPA, which operates on 2D patches, V-JEPA works with 3D "tubelets" that span both space and time.

### Spatiotemporal Tubelets

-   A video clip is divided into a grid of 3D patches, or **tubelets**.
-   Each tubelet has a spatial extent (e.g., 16x16 pixels) and a temporal extent (e.g., 2 frames).
-   These tubelets serve as the input tokens to the Vision Transformer encoders.

### 3D Positional Embeddings

To provide the model with information about the position of each tubelet in space and time, V-JEPA uses **3D positional embeddings**. These embeddings encode the x, y, and t coordinates of each tubelet, allowing the model to reason about spatiotemporal relationships.

## The Masking Strategy in V-JEPA

The masking strategy in V-JEPA is also adapted for the video domain.

-   **Spatiotemporal Masks:** The masks are applied to entire tubelets, effectively hiding a region of the video for a certain duration.
-   **Predicting the Future (and Past):** The masking strategy can be designed to predict future frames, past frames, or both. This flexibility allows the model to learn a rich understanding of temporal dynamics.

## The V-JEPA Workflow

Let's walk through the workflow of V-JEPA with a video clip as input:

1.  **Input:** A video clip is divided into spatiotemporal tubelets.

2.  **Masking:** A set of tubelets is masked out to create a context and a target.

3.  **Encoding:**
    -   The context tubelets (with their positional embeddings) are fed to the **context encoder**.
    -   The target tubelets are fed to the **target encoder**.

4.  **Prediction:**
    -   The **predictor** takes the representation of the context and predicts the representations of the target tubelets.

5.  **Loss and Update:**
    -   The loss is calculated between the predicted and actual target representations.
    -   The weights of the context encoder and predictor are updated.
    -   The weights of the target encoder are updated via momentum.

## The Power of a Video World Model

By learning to predict in this self-supervised manner, V-JEPA builds an internal "world model" that captures the underlying dynamics of the visual world. This model can then be used for a wide range of downstream tasks, from action recognition to video generation.

In the next chapter, we will discuss the process of training and evaluating V-JEPA models.