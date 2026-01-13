# Chapter 6: Target and Context Encoders

In this chapter, we will take a closer look at two of the most critical components of the Joint-Embedding Predictive Architecture (JEPA): the **context encoder** and the **target encoder**. These two networks work in tandem to create the representations that form the basis of JEPA's predictive learning process.

## The Role of the Encoders

At a high level, both encoders perform a similar function: they take a block of input data (e.g., an image patch) and map it to a high-dimensional representation in the embedding space. However, they have distinct roles and are updated differently during training.

### The Context Encoder

The context encoder is the "student" in the learning process. Its job is to process the context block—the part of the data that the model is given—and produce a representation that contains enough information to predict the representations of the target blocks.

-   **Input:** Receives the context block(s) from the input data.
-   **Output:** Produces a representation, `s_context`.
-   **Training:** The context encoder's weights are updated via backpropagation, driven by the loss calculated from the predictor's performance. It is actively learning to extract the most useful information from the context.

### The Target Encoder

The target encoder can be thought of as the "teacher." It provides the "ground truth" representations that the predictor aims to match. Its role is to produce a stable and consistent representation of the target blocks.

-   **Input:** Receives the target block(s) from the input data.
-   **Output:** Produces a representation, `s_target`.
-   **Training:** This is a key feature of JEPA. The target encoder's weights are **not** updated directly through backpropagation. Instead, they are an exponential moving average (EMA) of the context encoder's weights. This is known as a **momentum encoder**.

## The Momentum Encoder Explained

The use of a momentum encoder for the target network is a crucial architectural choice that helps to stabilize training and prevent the model from "collapsing."

-   **What is a momentum encoder?** The weights of the target encoder are a slowly evolving average of the context encoder's weights. This means that the target encoder's representation changes more gradually and smoothly over time.

-   **Why is it important?** By providing a stable and consistent target, the momentum encoder prevents the model from learning a trivial solution (e.g., where both encoders output the same constant value). It forces the predictor to learn meaningful features from the context in order to match the slowly evolving target representations.

The formula for updating the target encoder's weights (`θ_t`) based on the context encoder's weights (`θ_c`) and a momentum coefficient (`m`) is:

`θ_t ← m * θ_t + (1 - m) * θ_c`

Typically, the momentum coefficient `m` is a value close to 1 (e.g., 0.999), which ensures that the target encoder's updates are very slow.

## The Asymmetric Design

The combination of a trainable context encoder and a non-trainable, momentum-updated target encoder creates an **asymmetric architecture**. This asymmetry is fundamental to the success of JEPA and other similar self-supervised learning models.

In the next chapter, we will focus on the third major component of the JEPA architecture: the predictor module.