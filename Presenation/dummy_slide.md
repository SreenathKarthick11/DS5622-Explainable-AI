---
marp: true
theme: default
paginate: true
size: 16:9
html: true
style: |
  /* GLOBAL */

  section {
    font-family: "Aptos", "Inter", "Arial", sans-serif;
    background: #FFFFFF;
    color: #1F2937;
    padding: 55px 70px;
    font-size: 25px;
  }

  /* HEADINGS */

  h1 {
    color: #14532D;
    font-size: 42px;
    font-weight: 700;
    margin-bottom: 20px;
  }

  h2 {
    color: #14532D;
    font-size: 34px;
    font-weight: 650;
    border-bottom: 3px solid #DCFCE7;
    padding-bottom: 10px;
  }

  h3 {
    color: #166534;
    font-size: 27px;
  }

  strong {
    color: #166534;
  }

  /* TEXT */

  p {
    line-height: 1.45;
  }

  ul {
    line-height: 1.45;
  }

  li::marker {
    color: #16A34A;
  }

  em {
    color: #64748B;
  }

  /* CODE  */

  code {
    background: #F1F5F9;
    color: #14532D;
    padding: 3px 7px;
    border-radius: 4px;
  }

  /*  TABLES */

  table {
    font-size: 21px;
  }

  th {
    background: #14532D;
    color: #FFFFFF;
  }

  td {
    border-color: #E2E8F0;
  }

  /* BLOCKQUOTE  */

  blockquote {
    border-left: 5px solid #16A34A;
    background: #F0FDF4;
    padding: 15px 20px;
    color: #166534;
  }

  /* BOXES */

  .box {
    background: #F0FDF4;
    border-left: 5px solid #16A34A;
    border-radius: 8px;
    padding: 18px 24px;
    margin: 15px 0;
  }

  .box-title {
    color: #14532D;
    font-weight: 700;
    font-size: 25px;
    margin-bottom: 8px;
  }

  .box-note {
    background: #EFF6FF;
    border-left: 5px solid #2563EB;
    border-radius: 8px;
    padding: 18px 24px;
    margin: 15px 0;
  }

  .box-note .box-title {
    color: #1D4ED8;
    font-weight: 700;
    font-size: 25px;
    margin-bottom: 8px;
  }


  /* Yellow - Warning */
  .box-warning {
    background: #FFFBEB;
    border-left: 5px solid #F59E0B;
    border-radius: 8px;
    padding: 18px 24px;
    margin: 15px 0;
  }

  .box-warning .box-title {
    color: #B45309;
    font-weight: 700;
    font-size: 25px;
    margin-bottom: 8px;
  }


  /* Red - Danger / Important Issue */
  .box-danger {
    background: #FEF2F2;
    border-left: 5px solid #DC2626;
    border-radius: 8px;
    padding: 18px 24px;
    margin: 15px 0;
  }

  .box-danger .box-title {
    color: #B91C1C;
    font-weight: 700;
    font-size: 25px;
    margin-bottom: 8px;
  }

  /* HIGHLIGHTS */

  .highlight {
    background: #DCFCE7;
    color: #14532D;
    padding: 3px 8px;
    border-radius: 5px;
    font-weight: 600;
  }

  .muted {
    color: #64748B;
  }

  .small {
    font-size: 19px;
  }

  /* TWO COLUMN LAYOUT */

  .two-col {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 35px;
  }

  /* TITLE SLIDE */

  section.lead {
    border-left: 12px solid #14532D;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  section.lead h1 {
    font-size: 48px;
    line-height: 1.15;
  }

  /* SECTION DIVIDER */

  section.section {
    background: #14532D;
    color: #FFFFFF;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  section.section h1 {
    color: #86EFAC;
    font-size: 70px;
    margin-bottom: 5px;
  }

  section.section h2 {
    color: #FFFFFF;
    border: none;
    font-size: 42px;
  }

  section.section .muted {
    color: #DCFCE7;
  }

  /* HEADER / FOOTER */

  header {
    color: #64748B;
  }

  footer {
    color: #64748B;
  }
---

<!--  NORMAL SLIDE -->

# The Auditor's Problem

A model can make an accurate prediction, but an auditor still needs to understand **why**.

- What factors influenced the decision?
- How important was each factor?
- Can we trust the explanation?

---

<!--  BOX -->

# The Auditor's Problem

<div class="box">

<div class="box-title">A simple question</div>

Why did the model make this particular prediction?

</div>

A prediction may be accurate, but an auditor still needs to understand the reasoning behind it.

---

<!-- BOX + HIGHLIGHT -->

# The Core Problem

<div class="box">

<div class="box-title">The question</div>

Does an explanation remain consistent when we change how the **same feature is represented?**

</div>

<br>

<span class="highlight">Same data → Different representation → Different explanation?</span>

---

<!-- BLOCKQUOTE -->

# Why Explainability?

> A prediction tells us **what** the model decided.
> An explanation tries to tell us **why**.

<br>

This distinction becomes important when decisions need to be **audited, interpreted, or trusted**.

---

<!-- TWO COLUMN -->

# From Prediction to Explanation

<div class="two-col">

<div>

### Prediction

The model outputs:

**Loan → Reject**

<br>

But the prediction alone does not tell us why.

</div>

<div>

### Explanation

An explanation might tell us:

- Income ↓
- Debt ↑
- Credit history ↓

These factors help us understand the decision.

</div>

</div>

---

<!-- TWO COLUMN WITH BOXEs -->

# What SHAP Provides

<div class="two-col">

<div class="box">

<div class="box-title">Prediction</div>

Model output for a particular instance.

</div>

<div class="box">

<div class="box-title">Feature Attribution</div>

Contribution of each feature toward the prediction.

</div>

</div>

<br>

<div class="two-col">

<div class="box">

<div class="box-title">Positive contribution</div>

Pushes the prediction toward the target class.

</div>

<div class="box">

<div class="box-title">Negative contribution</div>

Pushes the prediction away from the target class.

</div>

</div>

---

<!-- 7. HIGHLIGHT  -->

# The Paper's Question

SHAP explanations are commonly interpreted as feature contributions.

But there is another factor to consider:

<br>

<span class="highlight">How the feature itself is represented.</span>


For example:

```text
Age = 25

25
↓
25 years
↓
0.25 × 100
↓
"25 years old"
```

The underlying information is the same.

---

<!-- TABLE -->
Same Information, Different Representation
| Representation | Example |
| -------------- | ------: |
| Raw value      |    `25` |
| Normalized     |  `0.25` |
| Standardized   | `-0.42` |
| Categorical    | `Young` |

<br>

The model may receive mathematically related representations of the same underlying information.

---


<!-- TABLE WITH EMPHASIS  -->
What We Would Expect
| Feature representation | Underlying information | Explanation |
| ---------------------- | ---------------------- | ----------- |
| `25`                   | Same                   | Similar     |
| `0.25`                 | Same                   | Similar     |
| `-0.42`                | Same                   | Similar     |

<br>

Ideally, changing the representation should not arbitrarily change the explanation.

---

<!-- CODE -->
Feature Transformation

Suppose the original feature is:

```
x = 25
```

We transform it using:

```
x_scaled = x / 100
```
Now:

```
x       = 25
x_scaled = 0.25
```

The numerical representation changed, but the underlying feature did not.

---

<!-- MUTED TEXT -->
## A Subtle Concern

The model sees a numerical representation.

The human interprets the explanation in terms of the underlying concept.

<br>
<span class="muted"> This creates a potential gap between model representation and human interpretation. </span>

---
<!-- SECTION -->
<!-- _class: section -->

# 01

## Why should we care about explanations?

<span class="muted">
From prediction → explanation → trust
</span>


---
<!-- SECTION LEAD -->
<!-- _class: lead -->

# SHAP-based Explanations are Sensitive to Feature Representation

<br>

### What happens when we change how a feature is represented?

<br>

**M. Sreenath Karthick**

<span class="muted">Paper Presentation · Explainable AI</span>

---
<!-- Combined Box -->
# The Auditor's Dilemma

<div class="box">

<div class="box-title">The model is correct.</div>

The prediction is accurate.

</div>

<div class="box">

<div class="box-title">But is the explanation reliable?</div>

The auditor wants to know which features actually influenced the decision.

</div>

<br>

<span class="highlight">Accuracy ≠ Explanation Reliability</span>

---

<!-- Two-Column + Box Combination -->

# What Does an Explanation Tell Us?

<div class="two-col">

<div class="box">

<div class="box-title">Model Prediction</div>

What did the model decide?

</div>

<div class="box">

<div class="box-title">Model Explanation</div>

Why did the model decide it?

</div>

</div>

---

<div class="box-note">

<div class="box-title">Note</div>

SHAP explanations depend on the representation given to the model.

</div>

<div class="box-warning">

<div class="box-title">Warning</div>

Different feature representations may produce different SHAP values.

</div>

<div class="box-danger">

<div class="box-title">Problem</div>

An explanation may appear convincing while being sensitive to feature representation.

</div>

