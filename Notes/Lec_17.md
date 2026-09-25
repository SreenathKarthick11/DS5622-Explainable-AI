# Lecture 17 : Algorithmic Recourse

## Introduction

Its called a recourse if it satisfy the following condition
- valid
- Proximal
- Plausible

In algorithmic recourse , the goal is to empower the person by answering the question "what can you actually go and do to get the change in prediction" ?

> [!TODO] TODO
> Learn the difference between algorithmic recourse and counterfactual explaination based on the following.
> - Immutable feature
> - Feasible changes
> - Causal consistency
> - "Who it's for ?"
> - Success criterion


---

## GenRe - an algorithmic recourse approach

Instead of searching for good recourse instance every time (which prior methods do), GenRe trains a model once to directly output good recourse instances.

$$
    \phi (x) = argmax_{x^+} exp(-\lambda C(x,x^+))P(x^+|y^+)V(x^+)
$$

- $exp(-\lambda C(x,x^+))$ - cheap to reach
- $P(x^+|y^+)$ - looks like a real approval person (plausiblilty)
- $V(x^+)$ - actually flips the classifier's decision

The prior does not train for all goal at the same time.

> **Train , don't search**

> [!example] TODO
> Learn the detail about the algorithm from the slides.


### Manufacturing training pairs

$$
    Q(x^+|x) = \frac{exp(-\lambda C(x,x^+))}{\sum_{x^+ \in D_h^+} exp(-\lambda C(x,x^+))}
$$

$\forall x^+ \not \in D_h^+ $ , $Q=0 $

> more detail is slide

> [!warning] TODO
> Solve the problem in example.

---


## Model Architeture

- Encoder-Decoder transformer is used.
- Encoder reads the rejected instance x
- Decoder generates x' one feature at a time
- Each feature output is a soft histogram.

---

## Final Score

$$
Score = Validity + Plausiblity(LOF) - Cost/num\_features
$$

based on the score we can take the top k ( $x^+$ ) values.


---

Refererence : [Paper](https://proceedings.iclr.cc/paper_files/paper/2025/file/117308c91189385fd47cfc36d7e6e0e5-Paper-Conference.pdf)

---