# Chapter 4: Core Principles of Predictive Architectures

Having established the limitations of previous self-supervised methods, we now turn our attention to the core principles that underpin predictive architectures like JEPA. These principles represent a shift in how models learn representations from unlabeled data.

## The Predictive Learning Paradigm

At its heart, a predictive architecture learns by forming an internal model of the world and then using that model to make predictions about what it will "see" next. The key idea is that in order to make accurate predictions, the model must learn high-level, abstract representations that capture the underlying structure of the data.

## Key Principles

There are three fundamental principles that define predictive architectures in the context of self-supervised learning:

### 1. Prediction in an Abstract Representation Space

This is the most crucial principle. Instead of predicting raw sensory inputs (like pixels), the model predicts representations of that data in a highly abstract, embedding space.

-   **Why it matters:** Predicting in an abstract space relieves the model from the burden of capturing every low-level detail. For example, it doesn't need to predict the exact texture of a leaf on a tree, but rather the concept of "leafiness." This encourages the model to learn semantic features.

### 2. The Use of a Predictor Module

Predictive architectures employ a dedicated "predictor" module. This module takes the representation of a "context" block of data and uses it to predict the representation of a "target" block.

-   **Context and Target:** The input data is divided into at least two parts: a context (what the model "sees") and a target (what the model "predicts").
-   **The Predictor's Role:** The predictor's job is to forecast the target's representation based on the context's representation. The model's parameters are updated to improve the accuracy of these predictions.

### 3. Asymmetric Architecture (Context and Target Encoders)

Many predictive architectures, including JEPA, use an asymmetric design with two separate encoders:

-   **Context Encoder:** This encoder processes the context block and produces its representation.
-   **Target Encoder:** This encoder processes the target block and produces the "ground truth" representation that the predictor aims to match.

This asymmetry, often combined with techniques like a momentum encoder for the target, helps to prevent "collapsing" representations and stabilizes the training process.

## Summary of Principles

| Principle | Description | Advantage |
| :--- | :--- | :--- |
| **Prediction in Abstract Space** | Predict representations, not pixels. | Focuses on semantic features, computationally efficient. |
| **Predictor Module** | A dedicated module forecasts target representations from context. | Enables the core predictive learning task. |
| **Asymmetric Architecture** | Separate encoders for context and target. | Stabilizes training and prevents model collapse. |

These principles collectively enable predictive architectures to learn powerful and efficient representations from unlabeled data. In the next chapter, we will see how these principles are instantiated in the specific architecture of the Joint-Embedding Predictive Architecture (JEPA).