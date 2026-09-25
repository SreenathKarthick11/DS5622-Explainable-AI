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
    font-weight: 700;
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

  .image-container {
    display:flex;
    justify-content:center;
    align-items:center;
    margin:0 auto;
    margin-top:0px;
    border: 2px solid black;
    border-radius: 8px;
    padding: 10px;
  }

  /* HEADER / FOOTER */

  header {
    color: #64748B;
  }

  footer {
    color: #64748B;
  }
---

<!-- SECTION -->
<!-- _class: section -->

# Paper Presentation

## SHAP-based Explanations are Sensitive to Feature Representation

<br>

<span class="muted">
M Sreenath Karthick (112301042)
</span>
<span class="muted">
M Murali Karthick (112301019)
</span>

---


<!-- _class: lead -->

# The Auditor's Problem

## Can we trust what an AI Explanation tells us?

<br>

> **A bank uses an AI model to decide loan applications.**
> 
> Ann is rejected.
> 
> The auditor asks:
> 
> **“Did the model use Ann's age to make this decision?”**

---


<div class="two-col">

<div class="image-container">

![h:250px](../Images/shap_ann.png)
</div>

<div class="image-container">

![h:300px](../Images/shap_ann2.png)
</div>

</div>

<br>

<div class="box-warning">
<div class="box-title">How much can we trust that explanation?</div>
</div>

---

<!-- _class: lead -->

## What If We Change Only How Age Is Represented?


<div class="two-col">

<div class="box">

<div class="box-title">Continuous Value Represenation</div>

*Age : 30*
```text
Rank #1         
SHAP = 0.99
```

</div>

<div class="box">

<div class="box-title">Bucket Value Representation</div>

*Age bucket : 25 - 35*
```text
Rank #5
SHAP = 0.37
```
</div>

</div>

<div class="box-note">

<div class="box-title">Note</div>

<b> No changes in Model or Explanation </b>

These numbers are directly from the paper's illustrative ACS Income example: continuous age vs 12-bucket equi-width encoding .

</div>

---

<!-- _class: lead -->

# The Core Problem

<div class="box-danger">

<div class="box-title">Concern</div>

*“An auditor wants to know whether an AI is using a protected attribute. SHAP seems to provide an answer. But if simply changing how that feature is represented can change its apparent importance, how reliable is that answer?”*
</div>

<div class="box-note">

<div class="box-title">The question</div>

But isn't SHAP supposed to explain the model?

What happens when we change how a feature is represented? Can this be gamed?

</div>

---

<!-- _class: lead -->

# SHAP

### Shapley values quantify each feature's contribution to a model prediction.
<br>

$$
\phi_i(f,x)
=
\sum_{S \subseteq N \setminus \{i\}}
\frac{|S|!(|N|-|S|-1)!}{|N|!}
\left[
f_x(S \cup \{i\}) - f_x(S)
\right]
$$

<div class="box">

<div class="box-title">Inference</div>

Feature contribution values for a particular prediction.

</div>

---

<!-- _class: section -->

# The Hidden Variable

## Feature Representation

The Feature is the same. The Representation is not.

---

<!-- _class: lead -->

## Continuous Features: Bucketization

<div class="two-col">

<div class="box">

<div class="box-title">Equi-Width</div>

Each interval has the same numerical width.

</div> 

<div class="box"> 

<div class="box-title">Equi-Depth</div>

Each interval contains roughly the same number of observations.

</div>
</div> 

<div class="image-container">

![h:260px](../Images/buckets.png)
</div>

---

<!-- _class: lead -->

## Categorical Features: Encoding

<div class="two-col">

<div>

<div class="box">

<div class="box-title">Original Categories</div>

*One category → one representation*

We can encode categories individually, or change how categories are grouped or encoded.


</div>


<div class="box-warning">

<div class="box-title">Why does this matter?</div>

Same semantic attribute → different feature representation → potentially different SHAP explanation

</div>

</div>

<div class="image-container">

![h:390px](../Images/bucket_cat.png)

</div>

</div>

---

<!-- _class: lead -->

# Why Could Bucket Size Change SHAP?

<div class="box">

<div class="box-title">1. Bucket size changes the information available</div>

Smaller buckets preserve more of the original feature information.

Larger buckets <b>merge more values together</b>, making the representation coarser.

</div>

<div class="box">

<div class="box-title">2. SHAP works with that representation</div>

SHAP measures the feature's <b>marginal contribution</b> to the prediction.

When the representation changes, the contribution being measured can also change.

</div>

---

<!-- _class: lead -->

## From Sensitivity to Exploitability

<div class="two-col">

<div class="box">

**1. How sensitive are SHAP explanations to feature engineering?**

Does changing the feature representation  
change its apparent importance?

</div>

<div class="box">

**2.Can this sensitivity be deliberately exploited?**

Can a representation be chosen to reduce the apparent importance of a protected feature?

</div>

</div>

<div class="box">

**3. If the SHAP values change, are they still faithful to the original prediction?**
</div>

<div class="box-note">

<div class="box-title">Definition</div>
<b>Fidelity</b> : proportion of observations for which the explanation corresponds to the originally predicted outcome.

</div>

---


