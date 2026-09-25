# Lec 21 : Learned Features and Network Dissection

## Learned Features

### Core Idea

Find the input that maximizes the activation of a chosen unit.

**Unit** : an indvidual neuron , a channel (feature map), a entire layer, or the final class probability. (pre-softmax recommended)

Generally we prefer the channels , wrather than neurons.

---

### Feature visualization through optimization

- The Optimization formula (for one neuron)
$$
    x^* = arg max_x h_{n,u,v,z}(x)
$$

- The Optimization formula for one channel
$$
    x^* =  arg max_x \sum_{u,v} h_{n,u,v,z}(x)
$$

> The steps are explained in the slides
---

### Optimization Process

- Option 1 : search training data for images that maximize the activation.
- Option 2 : genereate new images from random noise, iteratively optimize.

> [!Caution]
> The Problem with learned features is that it is subjective.

---

## Network Dissection

**Network Dissection** quantifies the interpretability of a unit of a convolutional neural network. It links highly activated areas of CNN channels with human concepts.

### The Big Hypothesis

The idea of disentangled features.

**Entangled Features** : no single unit corresponds to a concept - many units jointly contribute.

Disentangled network : individual units cleanly corresponds to specific real world concepts (e.g. skyscrapper)

We assume disentanglment for the methord.

> [!Note]
> CNN are not perfectly disentangled.

---

### Steps

- Get images with human-labeled visual concepts, from stripes to skyscrapers.
- Measure the CNN channel activations for these images.

$$
    M_k(x) = S_k(x) > T_k
$$

- Quantify the alignment of activations and labeled concepts.

$$
    IoU_{k,c} = \frac {M_k(x) \cap L_c(x)}{M_k(x) \cup L_c(x)}
$$

> detail steps in slides.

---

> [!Example] Reference
> Section in Interpertable ML Book : [Chapter 27](https://christophm.github.io/interpretable-ml-book/cnn-features.html)


---