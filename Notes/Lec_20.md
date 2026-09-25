# Lec 20 : Grad CAM

## Introduction

Grad-CAM stands for Gradient-weighted Class Activation Mapping.
Grad-CAM generalizes CAM to work with any CNN architechrue.

## Notations

$$
    y^c = raw score
$$

$$
    A^k \in R^{u \times v}
$$

Here $A^k$ is an Activation Map.

## Steps in short

1. Compute the gradient
1. Calculate $\alpha$ by pooling the gradient
1. Weighted combination + ReLU
1. Hence, we get the resultant Heat Map.

> Detail equation of the steps in slide.

>[!todo]
> Look into slides for examples.

> [!Example] Reference
> Section in Book : [Interpretable ML Book](https://christophm.github.io/interpretable-ml-book/pixel-attribution.html)
> Detailed  overview : [IEEE Blog on Grad Cam](https://ieeexplore.ieee.org/document/8237336)
> Short summary and code : [Medium Blog](https://medium.com/@bmuskan007/grad-cam-a-beginners-guide-adf68e80f4bb)

---
