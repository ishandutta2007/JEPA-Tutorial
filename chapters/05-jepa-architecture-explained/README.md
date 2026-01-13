# Chapter 5: JEPA Architecture Explained

Now that we have covered the core principles of predictive architectures, let's dive into the specific architecture of the Joint-Embedding Predictive Architecture (JEPA). This chapter will break down the main components of the JEPA model and explain how they work together.

## The Overall Architecture

The JEPA model is composed of three main components:

1.  **Context Encoder:** This network takes a "context" block of data (e.g., a patch of an image) and computes a high-level representation.
2.  **Target Encoder:** This network takes a "target" block of data (e.g., another patch of the same image) and computes its high-level representation.
3.  **Predictor:** This network takes the output of the context encoder and tries to predict the output of the target encoder.

The key is that the entire process of prediction occurs in the embedding space, not in the pixel space.

## A Step-by-Step Walkthrough

Here’s how the JEPA architecture processes data during a single training step:

1.  **Data Preparation:** An input sample (e.g., an image) is used to create one context block and one or more target blocks. These blocks are essentially patches or sub-samples of the original data.

2.  **Context Encoding:** The context block is passed through the **context encoder**, which produces a representation `s_context`.

3.  **Target Encoding:** The target block(s) are passed through the **target encoder**, which produces representation(s) `s_target`.

4.  **Prediction:** The representation from the context encoder, `s_context`, is fed into the **predictor** network. The predictor then outputs a predicted representation, `s_predicted`.

5.  **Loss Calculation:** The model calculates the difference (e.g., using Mean Squared Error) between the predicted representation, `s_predicted`, and the actual target representation, `s_target`.

6.  **Parameter Update:** The loss is used to update the parameters of the **predictor** and the **context encoder** through backpropagation.

## The Asymmetric Design: Momentum Encoder

A crucial detail of the JEPA architecture is that the **target encoder** is not updated via backpropagation. Instead, its weights are an exponential moving average (EMA) of the context encoder's weights. This is often referred to as a **momentum encoder**.

-   **Why use a momentum encoder?** This technique prevents the model from "collapsing" into a trivial solution where the context and target encoders always produce the same output. By making the target encoder's updates slower and more stable, it provides a consistent target for the predictor to learn from.

## Diagram of the JEPA Architecture

```
+-----------------+      +-----------------+
|  Context Block  |      |  Target Block   |
+-----------------+      +-----------------+
        |                        |
        v                        v
+-----------------+      +-----------------+
| Context Encoder |      | Target Encoder  | (Momentum)
+-----------------+      +-----------------+
        |                        |
        v                        v
   s_context                 s_target
        |
        v
+-----------------+
|    Predictor    |
+-----------------+
        |
        v
   s_predicted
        |
        v
+-----------------+
|      Loss       |<-+
+-----------------+  |
        |          |
        +----------+
```

In the following chapters, we will explore each of these components—the encoders, the predictor, and the loss function—in greater detail.