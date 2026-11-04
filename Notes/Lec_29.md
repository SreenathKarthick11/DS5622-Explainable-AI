# Lec 29: Logic Tensor Networks (LTNs)

## Big idea

**LTNs = neuro-symbolic framework** combining:

- **Neural learning** : learn from data via gradients
- **First-order logic (FOL)** : express domain knowledge explicitly
- **Fuzzy logic** : make logical formulas differentiable / numerical
- **Optimization** : train data modules and logical constraints together

> LTNs let models learn from **both data and logical reasoning**.

They support **Explainable AI** by making domain knowledge:

- explicit as logical formulas
- testable as truth degrees / satisfiability
- integrated into training rather than added after the fact

---

## What is an LTN?

A **Logic Tensor Network** is a neuro-symbolic framework that:

1. represents logical symbols as numerical tensors/functions
2. evaluates first-order logic formulas as fuzzy truth values in \([0,1]\)
3. trains neural networks and logical constraints jointly by gradient descent

So LTNs do **not** do classical theorem proving. They do **differentiable fuzzy satisfaction** of logical knowledge.

### Four building blocks

| Block | Role |
|---|---|
| **First-order logic** | Language for objects, relations, quantifiers, connectives |
| **Grounding** | Maps logical symbols to tensors / functions |
| **Fuzzy logic** | Gives numerical semantics to AND, OR, NOT, IMPLIES, ∀, ∃ |
| **Optimization** | Turns logical satisfaction into a differentiable loss |

---

## Expressing domain knowledge in predicate logic

In predicate logic, domain knowledge is written using:

- **Constants**: `alice`, `bob`, `img1`
- **Variables**: `x`, `y`
- **Predicates**: `Person(x)`, `Friends(x,y)`, `Smoker(x)`
- **Functions**: `fatherOf(x)`, `score(x)`
- **Connectives**: `¬`, `∧`, `∨`, `→`
- **Quantifiers**: `∀x`, `∃y`

### Example formulas

$$
∀x (Smoker(x) → Cancer(x)) \\
∀x∀y (Friends(x,y) → Friends(y,x)) \\
∀x (Person(x) → ∃y Parent(y,x))
$$

These formulas are the **knowledge base**. In an LTN, each formula is grounded into a differentiable truth value.

---

## How do we do grounding?

**Grounding** is the mapping from logical symbols to numerical objects.

Let the domain be \(D = \mathbb{R}^d\), i.e. objects are real-valued vectors/tensors.

A grounding \(G\) maps:

| Logical symbol | Grounded as |
|---|---|
| constant \(c\) | tensor \(c_G \in \mathbb{R}^d\) |
| variable \(x\) | tensor \(x_G \in \mathbb{R}^d\) or a batch of tensors |
| function \(f\) | function \(f_G : \mathbb{R}^{d \times \text{arity}} \to \mathbb{R}^d\) |
| predicate \(P\) | function \(P_G : \mathbb{R}^{d \times \text{arity}} \to [0,1]\) |
| formula \(\phi\) | truth value \(G(\phi) \in [0,1]\) |

### A grounded predicate

A **grounded predicate** is a predicate symbol \(P\) interpreted as a function

\[
P_G : D^n \to [0,1]
\]

that returns a **fuzzy truth degree** for each tuple of objects.

Example:

\[
P_G(x) = \sigma(\text{MLP}(x)) \in [0,1]
\]

or for a batch \(X \in \mathbb{R}^{N \times d}\):

\[
P_G(X) \in [0,1]^N
\]

So `Smoker(x)` does not return `True/False`; it returns a truth degree like `0.87`.

For a binary predicate:

\[
R_G(x,y) = \sigma(\text{MLP}([x;y])) \in [0,1]
\]

### Grounding a formula

Formulas are evaluated recursively:

- atom \(P(x)\) → \(P_G(x_G)\)
- \(\neg \phi\) → \(1 - G(\phi)\)
- \(\phi \land \psi\) → fuzzy t-norm, e.g. \(\min(G(\phi), G(\psi))\)
- \(\phi \lor \psi\) → fuzzy t-conorm, e.g. \(\max(G(\phi), G(\psi))\)
- \(\phi \to \psi\) → fuzzy implication
- \(\forall x\,\phi(x)\) → aggregate over all domain objects, e.g. minimum / product / mean
- \(\exists x\,\phi(x)\) → aggregate over all domain objects, e.g. maximum / sum / mean

Thus:

$$
G(∀x (Smoker(x) → Cancer(x))) ∈ [0,1]
$$

is a single differentiable truth degree for the whole rule.

>[!Caution] We cant always use Min & Max

---

## Optimization in LTNs

### What is Knowledge Base Satisfiability?

In classical logic, a knowledge base \(K = \{\phi_1,\dots,\phi_m\}\) is **satisfiable** if there exists an interpretation making every formula true — a binary yes/no.

In LTNs, satisfiability is **graded**: each formula is grounded to a fuzzy truth value \(G(\phi_i) \in [0,1]\), and KB satisfiability is an aggregate over all formulas:

\[
\mathrm{Sat}(K) = \underset{i}{\mathrm{Agg}} \; G(\phi_i) \in [0,1]
\]

Common aggregators:

| Aggregator | Formula | Behavior |
|---|---|---|
| **Min** | \(\min_i G(\phi_i)\) | strictest — one violated rule kills satisfaction |
| **Product** | \(\prod_i G(\phi_i)\) | penalizes multiple weak rules |
| **Mean** | \(\frac{1}{m}\sum_i G(\phi_i)\) | balanced, most common in practice |
| **Harmonic mean** | \(m / \sum_i 1/G(\phi_i)\) | penalizes the weakest rule more than the mean |

So **KB satisfiability** answers: *"How well does the current grounding (i.e. the neural networks) satisfy all logical constraints simultaneously?"*

- \(\mathrm{Sat}(K)=1\) → all formulas fully satisfied
- \(\mathrm{Sat}(K)=0\) → all formulas maximally violated

This is **soft satisfiability**, not classical SAT — LTNs never prove unsatisfiability; they only push groundings toward higher satisfaction.

### How it drives optimization

Training seeks groundings that **maximize** \(\mathrm{Sat}(K)\), i.e. minimize the logical loss. The overall objective is:

\[
L = L_{\text{data}} + \lambda L_{\text{logic}}
\]

where:

\[
L_{\text{logic}} = 1 - \mathrm{Sat}(K)
\]

or, equivalently, the per-formula form:

\[
L_{\text{logic}} = \sum_i \left(1 - G(\phi_i)\right)
\]

- \(L_{\text{data}}\): usual supervised/unsupervised loss
- \(L_{\text{logic}}\): penalizes low truth of logical constraints
- \(\lambda\): weight balancing data vs. logic

Because the fuzzy connectives, quantifier aggregators, and grounded predicates are all differentiable, gradients flow back into the predicates' neural networks, so the model learns to satisfy **both data and rules** jointly.

**Choice of aggregator matters:**

- **min** : enforces hard constraints (all-or-nothing)
- **mean** : allows soft trade-offs between rules
- **harmonic mean** : focuses learning on the worst-satisfied rule

>[!NOTE]
KB satisfiability in LTNs = the aggregated fuzzy truth of all formulas in \(K\); optimizing it means training neural groundings so that data and logical knowledge are satisfied together.

---


## Practical LTN pipeline

1. Define vocabulary: constants, variables, predicates, functions
2. Define grounding:
   - constants → tensors
   - predicates → neural nets or fixed functions returning \([0,1]\)
3. Encode domain knowledge as FOL formulas
4. Ground formulas to fuzzy truth values
5. Add logical loss to data loss
6. Optimize with gradient descent

---

## Key clarifications / corrections

- LTNs use **fuzzy first-order logic**, not classical Boolean FOL.
- “Grounding” does **not** just mean replacing variables by constants. In LTNs it means mapping all logical symbols to numerical/tensor functions.
- A predicate returns a **truth degree**, not a Boolean.
- Quantifiers are evaluated by **fuzzy aggregators** over the domain.
- Different fuzzy semantics — t-norms, implications, aggregators — change the meaning and behavior of the same formula.
- LTNs provide **differentiable logical constraints**, not formal proofs.

---

>[!Summary]
**LTN = first-order logic + tensor grounding + fuzzy semantics + gradient-based optimization**, letting a model learn from data while being constrained by explicit, testable domain knowledge.

---
