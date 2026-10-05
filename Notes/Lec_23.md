# Lec 23 : Intepreting LLMs

We will be looking at **Logit Lens**.

## Introduction

- What is a language model "thinking" at each layer ?
- Using Logit Lens, we can study the outcome of each layer in the transformer.

## Breif Intro to LLMs

- All language models try to predict the next word.
- The model produces the probability distribution

## Logit Lens

- First we observe the tokens at the final layer.
- **Prediction Trajectory** : how the models prediction changes over each layer.
- We need to understand that these predictions are not random.
- Intermediate predictions are often supresingly meaningful.

### Possible Interpretation

The model may progressivily refine the representation.

### Rank Lens

- How highly ranked is the eventual answers at each layer ?
- At every layer , find the rank of the model's evnetual find the predicition.

### KL Divergence

- This measure the how different are two probability distributions.

- The probabilty distribution of the intermediate layer is compared with the final layer.

> [!Note]
If the KL divergence decreases with depth the intermediate prediction is becoming increasingly similar to the final prediction.

---

## Limitation

- Through this approach we still can't understand the how the model predicts.

> [!Example] **Reference**
> Link to the Blog Post: [Click Here](https://www.lesswrong.com/posts/AcKRB8wDpdaN6v6ru/interpreting-gpt-the-logit-lens)