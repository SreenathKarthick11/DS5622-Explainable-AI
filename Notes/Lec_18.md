# Lec 18 : Interpreting Neural Networks

## Introduction

Now we will look into **Concept based** approches to overcome the limitations we had in the Local & Global Model agnoistic methods.

Major limitiations are the follows :
- Features aren't always human friendly
- Expressiveness is limited by feature space

## TCAV

TCAV stands for **Testing with Concept Activation Vectors**

**Summary**
For any user-chose concept, TCAV measures how much the concept influcence the model predictions.

**Steps**
- Building a CAV
- The decision boundary seperates the concept class activations from the random class activations.
> [!Note]
CAV = direction representing a concept in a particular layer's latent space.
- Conceptual sensitivity for a single input
$$
    S_{c,k,l}(x) = \triangledown h_{l,k}(\hat f_l(x))\cdot v_l^C
$$
- Interpret the sign.
- Compute TCAV score (aggregating across a whole class)
$$
    TCAV_{Q,C,K,L}= \frac { |\{ x \in X_k : S_{C,k,l}(x) > 0\}|}{|X_k|}
$$

- Statistical significance testing (making the TCAV trustworthy)

> For details steps refer the slides.

> [!Example] Example in slides depicts how TCAV is used for interpretation for InceptionV3 model for identification of Zebra.
> Notice that features such as stripe passed the significance test, compared to doted.

---

Refererence : [Chapter 27](https://christophm.github.io/interpretable-ml-book/cnn-features.html)

---