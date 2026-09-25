

**14 slides**. The story is:

> **“An auditor wants to know whether an AI is using a protected attribute. SHAP seems to give the answer. But then we discover that simply changing how the feature is represented can change the answer.”**


---

# Slide 1 - The Auditor's Problem

### Title

**Can We Trust What an AI Explanation Tells Us?**

### Put on the slide

Use a large scenario rather than technical content:

> **A bank uses an AI model to decide loan applications.**
> 
> Ann is rejected.
> 
> The auditor asks:
> 
> **“Did the model use Ann's age to make this decision?”**

Then put a small SHAP-style visualization on the right:

```text
Why was Ann rejected?

Age             ██████████  +0.99
Income          █████       +0.52
Debt            ███         +0.31
Employment      ██          +0.18
...
```

### Image to use

A simple illustration:

**Auditor → AI model → Loan decision**

with Ann's profile somewhere in the diagram.

You could have:

```text
             ┌─────────────┐
Ann ────────→│  AI Model   │──────→ REJECTED
             └─────────────┘
                    ↑
                    │
                 SHAP
                    │
             "Age was important"
                    ↑
                 Auditor
```

Don't use a real bank logo. A generic illustration is better.

### What you say

> “Let's start with a situation. Imagine you're an auditor investigating an AI system used by a bank. Ann applies for a loan and the model rejects her.
> 
> As an auditor, one of the things you want to know is whether the model relied on a protected attribute such as age.
> 
> You don't have access to the entire development pipeline. But you do have the model and you can generate SHAP explanations.
> 
> And SHAP tells you that age was one of the most important features.
> 
> So naturally, you might think: _Okay, the model is using age._
> 
> But this paper asks a much more subtle question:
> 
> **How much can we trust that explanation?**”

---

# Slide 2 - Something Strange Happens

### Title

**What If We Change Only How Age Is Represented?**

This is your **first big reveal**.

### Put on slide

Show:

```text
                    SAME MODEL
                        │
              ┌─────────┴─────────┐
              │                   │
           Age = 30            Age < 50
              │                   │
             SHAP                SHAP
              │                   │
          Rank #1              Rank #5
          SHAP = 0.99          SHAP = 0.37
              │                   │
              └─────────┬─────────┘
                        ↓
               SAME PREDICTION
               DIFFERENT STORY
```

These numbers are directly from the paper's illustrative ACS Income example: continuous age had SHAP weight **0.99 and rank 1**, while after 12-bucket equi-width encoding it became **0.37 and rank 5**, with the model and explainer fixed.

### Image

**Do NOT use a generic SHAP image here.**

Make this slide yourself with two side-by-side representations:

```text
Age
30
```

versus

```text
Age bucket
25 ─── 35
     ↑
    30
```

Then put the two SHAP bars underneath.

### What you say

> “Now here's where things get interesting.
> 
> We haven't changed the model.
> 
> We haven't changed the person.
> 
> We've only changed how age is represented.
> 
> In the original representation, SHAP gives age a weight of 0.99 and ranks it first.
> 
> After bucketizing age, the importance falls to 0.37 and age drops to fifth place.
> 
> So the prediction can stay the same, while the explanation changes dramatically.
> 
> **That's the problem this paper starts investigating.**”

Pause here.

---

# Slide 3 - But Isn't SHAP Supposed to Explain the Model?

### Title

**What Exactly Is SHAP Explaining?**

### Put on slide

Keep it extremely simple.

```text
              MODEL PREDICTION
                     │
                     ↓
                   SHAP
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Age          Income       Education
      +0.42         +0.31         -0.18
```

Then a small statement:

> **SHAP assigns contribution values to features for a prediction.**

### Optional visual

A simple SHAP waterfall plot.

If you use an actual SHAP waterfall screenshot, make sure it is clearly labeled as an illustrative/example SHAP plot rather than claiming it is from the paper.

### What you say

> “Before going further, let's quickly establish what SHAP is doing.
> 
> SHAP gives us feature contribution values for a particular prediction.
> 
> In a local explanation, we can look at the features and ask: which ones contributed most strongly to this prediction?
> 
> So if age has a large SHAP value, we might interpret that as age being important for this decision.
> 
> And that is exactly why SHAP is useful for auditing.
> 
> But the paper asks whether this explanation is stable when we change the representation of the underlying features.”

---

# Slide 4 - The Hidden Variable: Feature Representation

### Title

**The Feature Is the Same. The Representation Is Not.**

### Put on slide

Use a visual progression:

```text
               AGE
                │
      ┌─────────┼──────────┐
      ↓         ↓          ↓
     30       30–40       < 50
```

Then categorical example:

```text
RACE

White
Black
Asian
Other

        ↓ regroup

White + Black
Asian + Other
```

### Main message

Put this in a box:

> **Same semantic attribute ≠ same representation**

### Image

This slide should be almost entirely your own diagram.

Use **bucket graphics**:

```text
17 ───── 30 ───── 50 ───── 70 ───── 94
          │
        Age = 30
```

then:

```text
17 ─────────────── 50 ─────────────── 94
         Age < 50
```

### What you say

> “Feature engineering gives us many ways of representing the same underlying concept.
> 
> Age can be kept as an exact continuous value.
> 
> Or we can turn it into age groups.
> 
> Race can have several categories, or categories can be merged.
> 
> From a human perspective, we're still talking about the same underlying attribute.
> 
> But SHAP doesn't operate on some abstract concept of 'age.'
> 
> It operates on the representation given to it.
> 
> And that is the hidden variable the paper investigates.”

---

# Slide 5 - Why Could This Change SHAP?

### Title

**A Small Preprocessing Choice Can Change the Explanation**

### Put on slide

Show:

```text
Continuous

17 18 19 20 21 22 23 24 ...
│  │  │  │  │  │  │  │
Precise information
```

versus

```text
Bucketized

17 ───────── 30
31 ───────── 50
51 ───────── 70
71 ───────── 94

Coarser information
```

Then:

```text
Representation
      ↓
Model / SHAP input
      ↓
Feature contribution
      ↓
Explanation
```

### What you say

> “Why should this matter?
> 
> When we bucketize a feature, we're changing the information available in that representation.
> 
> Instead of saying someone is exactly 31, we might say they're in the 30-to-40 bucket.
> 
> The model and the explanation mechanism now see a different representation.
> 
> And that can change how much contribution SHAP attributes to that feature.
> 
> The important point is that bucketization isn't necessarily suspicious. It's a completely normal data-engineering operation.
> 
> That's what makes the problem interesting.”

---

# Slide 6 - The Paper's Research Questions

### Title

**So the Authors Ask Two Questions**

Don't overload this slide.

### Put on slide

Large numbered questions:

### **1**

> **How sensitive are SHAP explanations to feature engineering?**

### **2**

> **Can this sensitivity be deliberately exploited?**

Then at the bottom:

```text
Sensitivity  →  Exploitability
```

### What you say

> “At this point, the paper essentially splits into two parts.
> 
> First, the authors ask: is this just one strange example, or is SHAP systematically sensitive to representation?
> 
> That's the first contribution.
> 
> Then comes the more concerning question.
> 
> If we know that representation changes SHAP, can someone deliberately choose a representation that makes a protected feature look less important?
> 
> That's the second contribution: the feature-engineering attack.”

This is a very important transition.

---

# Slide 7 - How Did They Test It?

### Title

**The Experimental Setup**

### Put on slide

Use four boxes:

```text
DATASETS
──────────────
ACS Income
46,144 observations
8 features

ACS Public Coverage
25,524 observations
16 features
```

```text
MODEL
──────────────
XGBoost

Hyperparameter
tuning for accuracy
```

```text
PROTECTED
FEATURES
──────────────
Age
Race
```

```text
EXPLANATION
──────────────
SHAP

Compare:
• SHAP value
• SHAP rank
• Fidelity
```

The dataset sizes and feature counts are from the paper.

### Image

Use a pipeline:

```text
ACS Dataset
     ↓
Feature Engineering
     ↓
XGBoost
     ↓
SHAP
     ↓
Compare explanations
```

### What you say

> “The authors test this on two real-world datasets from the American Community Survey.
> 
> One predicts whether income is above 50 thousand dollars, and the other predicts public health insurance coverage.
> 
> They treat age and race as protected features.
> 
> They train XGBoost models and then use SHAP to evaluate the explanations.
> 
> And they look at three things: the SHAP value of the protected feature, its rank compared with other features, and something called fidelity.”

---

# Slide 8 - What Does Fidelity Mean?

### Title

**But Are These Explanations Still Faithful?**

This slide is important because otherwise the audience may think:

> “Of course SHAP changes — maybe the new explanation is simply wrong.”

### Put on slide

```text
Prediction
    │
    ↓
SHAP contributions
    │
    ├── Age       +0.37
    ├── Income    +0.42
    ├── Education +0.11
    └── ...
    │
    ↓
Can the contributions
reconstruct the original prediction?
```

Then:

> **Fidelity = fraction of explanations that remain faithful to the original prediction**

The paper defines fidelity as the proportion of observations for which the explanation reconstructs/corresponds to the originally predicted outcome.

### What you say

> “This is an important detail.
> 
> The authors don't just say, 'the SHAP values changed, therefore something is wrong.'
> 
> They also check whether the explanation remains faithful to the original prediction.
> 
> In other words, can the SHAP contributions still account for the model's prediction?
> 
> This becomes especially important later, because the attack manages to reduce the apparent importance of protected features while maintaining high fidelity.”

---

# Slide 9 — Finding #1: Age Is Sensitive

### Title

**Finding 1: Age Can Move Dramatically in the SHAP Ranking**

### Put on slide

I would use the paper's **Figure 4** here if you can extract it from the PDF.

The paper reports that as the number of age buckets increases:

- average SHAP importance changes
    
- average rank changes
    
- the percentage of observations where age is the most important feature changes
    

The paper also reports that age can shift by **as much as 20 rank positions** in some cases.

### Visual

Preferably:

**Paper Figure 4**

or recreate a simplified conceptual graph:

```text
Age importance
     ↑
     │                 ●
     │             ●
     │         ●
     │      ●
     │   ●
     └────────────────────→
       2  3  4  5  6  7
             # buckets
```

And beside it:

```text
More buckets
      ↓
Age becomes more important
```

### What you say

> “Now we get the first experimental result.
> 
> The authors vary how many buckets are used for age.
> 
> And SHAP changes substantially.
> 
> As the representation becomes more granular, age can become more important in the explanations.
> 
> The paper reports rank changes of up to around 20 positions in some cases.
> 
> So this isn't just a tiny numerical fluctuation.
> 
> **The explanation can materially change depending on how the feature was engineered.**”

---

# Slide 10 - Finding #2: Race Can Be Hidden

### Title

**And It Happens With Categorical Features Too**

### Put on slide

Start with:

```text
Original race representation

White
Black
Asian
Other
```

Then:

```text
Merge categories

White + Black
Asian + Other
```

Then arrow:

```text
                ↓

       Race becomes less important
             according to SHAP
```

The paper specifically reports that merging categories can dramatically increase the rank of race, meaning lower apparent importance; in some cases, merging White with Black or White with Asian can reduce race's importance to nearly zero.

### Image

Use **paper Figure 7 or Figure 9** here.

Figure 9 is particularly good because it visually shows the race ranking changing under different bucketizations.

### What you say

> “And this isn't limited to continuous features like age.
> 
> The authors do the same experiment with race.
> 
> Instead of treating White, Black, Asian and Other as separate categories, they try different combinations.
> 
> And again, the SHAP importance changes.
> 
> In some representations, race becomes much less important according to SHAP.
> 
> So now we have seen the same phenomenon for both continuous and categorical features.”

---

# Slide 11 - Now the Story Changes

### Title

**But What If Someone Does This Deliberately?**

This should be a very visual transition slide.

### Put on slide

Large text:

> **Until now, the representation was just a preprocessing choice.**
> 
> **What if it becomes a strategy?**

Then:

```text
Normal feature engineering
             ↓
     Changes SHAP
             ↓
       We discover:
             ↓
    This can be exploited
```

### Image

Return to your auditor.

Show:

```text
                    VENDOR
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
      DATA          MODEL       PIPELINE
   ENGINEERING
                      │
                      ↓
                  AUDITOR
                      │
                     SHAP
```

### What you say

> “This is where the paper moves from observation to attack.
> 
> So far, we've seen that ordinary representation choices can change SHAP.
> 
> But remember our auditor from the beginning.
> 
> The auditor sees the preprocessed data and the model.
> 
> The vendor controls the data-engineering pipeline.
> 
> So what happens if the vendor deliberately chooses a representation that makes a protected feature appear less important?”

Pause.

> “The authors call this a **feature-engineering attack on SHAP**.”

---

# Slide 12 - The Feature-Engineering Attack

### Title

**How Do You Hide a Feature From SHAP?**

### Put on slide

Use a 4-step attack loop:

```text
        ① Choose representation
                  ↓
        ② Generate SHAP
                  ↓
        ③ Measure protected
           feature importance
                  ↓
        ④ Check fidelity
                  │
                  └──────→ Optimize again
```

For age:

```text
Bucket boundaries
       ↓
Bayesian Optimization
       ↓
Find representation
that reduces SHAP rank
```

The paper uses Bayesian Optimization for continuous age bucket boundaries, with a fidelity constraint. It tunes bucket boundaries and evaluates SHAP rank.

### What you say

> “For a continuous feature like age, the authors formulate this as an optimization problem.
> 
> Instead of choosing ordinary bucket boundaries, they search for boundaries that reduce the SHAP importance of age.
> 
> They use Bayesian Optimization because evaluating SHAP is expensive and the search space is essentially a black-box optimization problem.
> 
> But there's a constraint:
> 
> **We don't want to destroy the explanation's fidelity.**
> 
> So the attack tries to reduce the apparent importance of age while keeping the explanations faithful.”

---

# Slide 13 - Does the Attack Actually Work?

### Title

**The Concerning Part: The Prediction Doesn't Have to Change Much**

This should be one of your strongest slides.

### Put on slide

For age:

```text
                 AGE

Original       ────────────────
representation

Optimized      ────
representation

       ↓ SHAP importance/rank

Protected feature
appears LESS important

       BUT

Fidelity ≥ 88%
```

For race:

```text
                 RACE

Category merging
       ↓
SHAP rank ↓ importance
       ↓
Fidelity ≥ 98%
```

The paper reports age attack fidelity of at least **88%**, while the race bucketization attacks had at least **98%** perfect-fidelity explanations.

### Better image

Use **paper Figure 8** for age.

It directly shows:

- Base
    
- Equi-width
    
- Bayesian Optimization
    
- number of buckets
    
- average rank
    

The paper states that the attack can substantially increase the rank of age while maintaining fidelity at least as high as equi-width bucketization.

### What you say

> “And this is the key result of the paper.
> 
> The attack works.
> 
> With age, the authors can find bucketizations that push age down in the SHAP ranking, while maintaining fidelity of at least 88 percent.
> 
> With race, the bucketization attacks have at least 98 percent perfect fidelity.
> 
> So the problem isn't simply that we're producing obviously broken explanations.
> 
> **We can change what SHAP tells the auditor while retaining high explanation fidelity.**”

---

# Slide 14 - Back to the Auditor

### Title

**What Does the Auditor See?**

This brings the story back to Slide 1.

### Put on slide

Split screen:

### Before

```text
SHAP

Age        ██████████
Income     █████
Education  ███

"Age looks important"
```

### After representation change

```text
SHAP

Income     ███████
Education  █████
Age        ██

"Age looks less important"
```

Then underneath:

```text
           SAME UNDERLYING MODEL
                    │
                    ↓
          DIFFERENT REPRESENTATION
                    │
                    ↓
             DIFFERENT STORY
```

Be careful with “same model”: this specifically applies to the **attack setting**, where the paper trains on original data, keeps the model fixed, and changes only the explainer input representation.

### What you say

> “Let's return to our auditor.
> 
> The auditor isn't necessarily looking at the raw data or the complete engineering pipeline.
> 
> They are looking at the model and the explanation.
> 
> If the representation supplied to the explainer changes, the apparent importance of the protected feature can change.
> 
> And in the attack setting, the authors show that this can happen **without retraining the model**.
> 
> So the auditor could receive a perfectly valid-looking explanation that tells a very different story.”

---

# Slide 15 - What Should We Take Away?

### Title

**So… Can We Trust SHAP?**

I would **not** put:

> ❌ SHAP cannot be trusted.

That is stronger than what the paper establishes.

Instead put:

> **SHAP may be faithful to the representation it is given.**
> 
> **But the representation itself can influence the explanation.**

Then:

```text
              MODEL
                │
                ↓
        DATA REPRESENTATION
                │
                ↓
              SHAP
                │
                ↓
          EXPLANATION
```

Highlight the middle:

> **The data-engineering pipeline matters.**

### What you say

> “So does this mean we should stop using SHAP?
> 
> No. That's not really the conclusion of the paper.
> 
> The more precise lesson is that SHAP explanations are not independent of feature representation.
> 
> SHAP can be faithful to the representation it receives.
> 
> But if that representation itself is changed, the explanation can change.
> 
> Therefore, if we're using explanations for auditing or fairness analysis, we need to pay attention not just to the model, but also to the data-engineering pipeline that produces the representation.”

---

# Slide 16 - The Bigger Lesson

### Title

**Don't Audit Only the Model. Audit the Pipeline.**

### Put on slide

This should be your final visual:

```text
                 ┌──────────────┐
                 │     DATA     │
                 └──────┬───────┘
                        ↓
              ┌──────────────────┐
              │ DATA ENGINEERING │
              │                  │
              │ encoding         │
              │ bucketization     │
              │ grouping          │
              └────────┬─────────┘
                       ↓
                ┌────────────┐
                │   MODEL    │
                └─────┬──────┘
                      ↓
                  ┌───────┐
                  │ SHAP  │
                  └───┬───┘
                      ↓
                EXPLANATION
```

At the bottom:

> **The explanation depends on more than the model.**

### What you say

> “And I think this is the biggest takeaway from the paper.
> 
> When we audit an AI system, we often think about the model:
> 
> Is the model accurate?
> 
> Is the model fair?
> 
> Can we explain its predictions?
> 
> But this paper shows that there is another layer we need to think about:
> 
> **How was the data represented before it reached the model and the explainer?**
> 
> Because seemingly harmless engineering choices can change the explanation.
> 
> And if those choices can be manipulated, then they become part of the audit surface.
> 
> So the final message is:
> 
> **Don't audit only the model. Audit the pipeline that produces the explanation.**”

---

# The overall visual strategy

I would **not** put lots of screenshots from the paper on every slide. Instead, mix three types of visuals.

### 1. Story visuals — Slides 1, 2, 11, 14

Create your own clean diagrams.

For example:

```text
AUDITOR
   ↓
MODEL
   ↓
PREDICTION
   ↓
SHAP
   ↓
"Did age matter?"
```

These make the talk feel like a story.

---

### 2. Concept visuals — Slides 3–6

Use simple diagrams:

```text
Age = 37
    ↓
Age = 30–40
    ↓
Different representation
```

and:

```text
White
Black
Asian
Other

      ↓

White + Black
Asian + Other
```

Don't use complicated paper figures here.

---

### 3. Actual paper results — Slides 9, 10, 12, 13

Here you **should use the paper's figures** because you're presenting the actual experimental evidence.

I'd prioritize:

|Slide|Paper figure|
|---|---|
|9 — Age sensitivity|**Figure 4**|
|10 — Race sensitivity|**Figure 7 / Figure 9**|
|12 — Attack|**Figure 8 / attack formulation**|
|13 — Attack works|**Figure 8 + Table 2/3 values**|

This gives you a nice progression:

**Story → Concept → Evidence → Attack → Evidence → Lesson**

---

# One important improvement to the original 14-slide flow

I would actually make it **16 slides**, because **fidelity deserves its own slide**.

The narrative then becomes:

```text
ACT 1 — THE QUESTION

1. Auditor's Problem
2. Something Strange Happens
3. What Exactly Is SHAP?

        ↓

ACT 2 — THE HIDDEN PROBLEM

4. Feature Representation
5. Why Representation Matters
6. Research Questions
7. Experimental Setup
8. What Is Fidelity?

        ↓

ACT 3 — THE EVIDENCE

9. Age Is Sensitive
10. Race Is Sensitive

        ↓

ACT 4 — THE ATTACK

11. What If This Is Deliberate?
12. How the Attack Works
13. The Attack Works

        ↓

ACT 5 — THE CONSEQUENCE

14. Back to the Auditor
15. Can We Trust SHAP?
16. Don't Audit Only the Model
```

That gives the presentation a much more natural **“wait → discover → investigate → escalate → conclude”** structure rather than:

> Introduction → Related Work → Methodology → Results → Conclusion.

And for this paper, I think that storytelling approach will make the **central idea — “same model, different representation, different explanation” — stick much better**.

---

