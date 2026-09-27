

# Slide 1 — The Auditor's Problem

### What to say

> “Let me start with a situation.
>
> Imagine that a bank uses an AI model to decide whether someone should get a loan.
>
> Now suppose Ann applies for a loan, and the model rejects her.
>
> As an auditor, I want to understand why this happened.
>
> In particular, I want to know whether the model used a protected attribute, such as age, while making this decision.
>
> So I use an explanation method like SHAP to inspect the model's decision.
>
> And this gives me a seemingly straightforward way to answer the question:
>
> **Did age actually influence the decision?**
>
> But the paper shows that this question is not as straightforward as it first appears.”

---

# Slide 2 — How Much Can We Trust That Explanation?

### What to say

> “So this leads us to the main question of the presentation:
>
> **How much can we actually trust that explanation?**
>
> If SHAP tells me that age is important, I would normally assume that this is evidence that the model is relying on age.
>
> But what if the explanation depends not only on the model, but also on how the input features were represented?
>
> That is the problem the authors investigate.
>
> And the interesting part is that the representation can be changed without necessarily changing what the underlying feature means.”

### Transition

> “So let's see what happens if we change only the representation of age.”

---

# Slide 3 — What If We Change Only How Age Is Represented?

### What to say

> “Here is the key example from the paper.
>
> Consider a particular individual from the ACS Income dataset.
>
> Initially, age is represented as a continuous value.
>
> For this individual, SHAP considers age to be the most important feature. Its SHAP weight is 0.99, and it has rank 1.
>
> Now the authors do something very simple.
>
> They bucketize age.
>
> Instead of representing the person's exact age, they represent it using an age interval. In the paper's example, age is divided into 12 equi-width intervals, and the interval is represented using its median.
>
> And suddenly, the SHAP value for age becomes 0.37 and its rank drops from **1 to 5**.
>
> And here's the important part:
>
> **The classifier model and the SHAP explainer remain fixed.**
>
> The only thing that changed for this individual was how the age feature was represented.
>
> So we have the same person, the same model, and the same prediction—but a different explanation.”

### Strong pause here.

> “And that is where the paper's problem begins.”

---

# Slide 4 — The Core Problem

### What to say

> “So let's formulate the problem clearly.
>
> An auditor wants to know whether an AI system is using a protected attribute.
>
> SHAP seems to give us an answer by telling us which features are important.
>
> But we have just seen that simply changing the representation of a feature can change its apparent importance.
>
> So now we have two questions.
>
> First:
>
> **Is SHAP actually sensitive to these feature-engineering choices?**
>
> And second:
>
> **If it is sensitive, can somebody deliberately exploit that sensitivity to make a protected feature look less important?**
>
> Before answering those questions, let's briefly understand what SHAP is actually doing.”

---

# Slide 5 — SHAP

### What to say

> “SHAP is based on Shapley values from cooperative game theory.
>
> The basic idea is that for a particular prediction, we want to distribute the model's output among the input features according to their contribution.
>
> So for one prediction, we might get something like:
>
> age contributes this much,
>
> income contributes this much,
>
> education contributes this much,
>
> and so on.
>
> These contribution values allow us to construct a local explanation for an individual prediction.
>
> In our auditor scenario, this is useful because we can look at the SHAP values and ask:
>
> **Was age one of the features that contributed strongly to this decision?**
>
> The paper's observation is that the answer we get can depend on how we represent age in the first place.”

---

# Slide 6 — The Hidden Variable: Feature Representation

### What to say

> “And this brings us to what the paper calls the feature representation.
>
> Consider age.
>
> We can represent someone's age directly as 30.
>
> Or we could represent it as an age interval, such as 25 to 35.
>
> Or we could use larger age groups.
>
> Semantically, we're still talking about the same thing: **age**.
>
> But from the model's perspective, these are different representations.
>
> The same thing happens with categorical features.
>
> For example, race could initially have categories such as White, Black, Asian and Other.
>
> We could then regroup these categories into different combinations.
>
> So the important distinction is:
>
> **The semantic attribute stays the same, but its representation changes.**
>
> And the paper asks whether SHAP is sensitive to that difference.”

---

# Slide 7 — Continuous Features: Bucketization

### What to say

> “For continuous features, the technique the authors focus on is called **bucketization**, or binning.
>
> There are two basic ways they discuss.
>
> The first is equi-width bucketization.
>
> Here, every interval has approximately the same numerical width.
>
> So if age ranges from 17 to 94, we divide that numerical range into intervals of equal size.
>
> The second is equi-depth bucketization.
>
> Here, instead of making the intervals equally wide, we try to put approximately the same number of observations into each bucket.
>
> So we're taking one continuous feature and replacing its precise value with a bucket.
>
> And the question is:
>
> **Does changing the number or boundaries of these buckets change what SHAP tells us about the feature?**”

---

# Slide 8 — Categorical Features: Encoding

### What to say

> “The same idea applies to categorical features.
>
> Initially, we could keep every category separate.
>
> For race, for example, we might have White, Black, Asian and Other as separate categories.
>
> But we could instead group categories together.
>
> For example, we could create a representation like White versus everyone else.
>
> Or Black versus everyone else.
>
> Or combine multiple categories into two or three groups.
>
> Again, we're not changing the underlying concept we're studying.
>
> We're changing its representation.
>
> And this gives us the central relationship of the paper:
>
> **Same semantic attribute → different representation → potentially different SHAP explanation.**”

---

# Slide 9 — Why Could Bucket Size Change SHAP?

### What to say

> “So why should bucketization affect SHAP at all?
>
> There are two intuitive reasons.
>
> First, bucket size changes the amount of information represented by the feature.
>
> With smaller buckets, we preserve more information about the original age.
>
> With larger buckets, more values are merged together, so the representation becomes coarser.
>
> Second, SHAP measures the contribution of the feature representation that it receives.
>
> So when we change that representation, we're changing the quantity whose contribution is being measured.
>
> Therefore, even though we still call the feature 'age', the actual representation that SHAP sees has changed.
>
> And that can change its contribution and its ranking.
>
> Now the authors want to determine whether this is just an isolated example, or whether it happens systematically.”

---

# Slide 10 — From Sensitivity to Exploitability

### What to say

> “This is where the paper's research questions become more precise.
>
> The first question is:
>
> **How sensitive are SHAP explanations to feature engineering?**
>
> In other words, if I change the representation, does the apparent importance of the feature change?
>
> The second question is more interesting:
>
> **Can this sensitivity be deliberately exploited?**
>
> If I know that certain representations make a protected feature look less important, could I deliberately choose such a representation?
>
> But there's one more thing we need to consider.
>
> Suppose the SHAP values change.
>
> Does that mean the explanation is now meaningless?
>
> The authors therefore also measure something called **fidelity**.
>
> Fidelity measures the proportion of observations for which the explanation still corresponds to the model's original prediction.
>
> So ideally, an attack would reduce the apparent importance of a protected feature **without simply destroying the explanation altogether.**”

---

# Slide 11 — Inference from the Paper

### What to say

> “So at this point, we have established the problem conceptually.
>
> Now let's move from the intuition to the experiments.
>
> The authors want to determine whether this sensitivity actually appears on real datasets, and whether it can then be exploited deliberately.
>
> I'll first go through their experimental setup, then we'll look at the results.”

This slide is basically a **chapter break**, so don't spend much time here.

---

# Slide 12 — The Experimental Setup

### What to say

> “The authors use two real-world datasets from the American Community Survey.
>
> The first is **ACS Income**, which contains 46,144 observations and 8 features. The task is to predict whether an individual's income is above 50 thousand dollars.
>
> The second is **ACS Public Coverage**, which contains 25,524 observations and 16 features. Here, the task is to predict whether an individual is covered by public health insurance.
>
> For both datasets, the authors treat **age and race as protected features**.
>
> The categorical features are one-hot encoded.
>
> For the classifier, they use XGBoost and perform hyperparameter tuning for overall accuracy.
>
> Then they use SHAP to generate local explanations.
>
> And they evaluate the explanations using three things:
>
> **SHAP value**, which tells us the magnitude of feature importance;
>
> **SHAP rank**, which tells us where the protected feature sits relative to the other features;
>
> and **fidelity**, which tells us whether the explanation remains faithful to the original prediction.”

---

# Slide 13 — Finding #1: Age

### What to say

> “Let's start with the continuous feature: age.
>
> The authors take the age feature and represent it using different numbers of buckets.
>
> They then retrain the model using each representation and evaluate the resulting SHAP explanations.
>
> And we can see a clear trend.
>
> As the number of buckets increases, the average SHAP importance of age increases.
>
> The average rank of age also moves closer to the top.
>
> And the percentage of observations where age is the most important feature increases.
>
> So the representation of age has a substantial effect on how important age appears to SHAP.”

### Then point at the graph

> “The important thing to notice here is not just that the value changes.
>
> **The ranking changes.**
>
> And ranking is particularly important in an audit because an auditor may look at the top few features and decide which features appear to drive the model.”

---

# Slide 14 — Finding #1: Age — Inference

### What to say

> “So what do we infer from this experiment?
>
> The more buckets we use, the more information about the original continuous age value we preserve.
>
> As a result, age becomes more prominent in the model's prediction and therefore in the SHAP explanation.
>
> But there's an important broader point here.
>
> The authors report that the relative importance of age can change by **as much as 20 rank positions** under different representations.
>
> And in cases where age was initially the most important feature, its importance frequently drops by around **3 to 5 positions** under some representations.
>
> So we're not talking about a tiny numerical difference.
>
> We're talking about potentially changing the conclusion an auditor could draw from the explanation.”

### Transition

> “But age is a continuous feature. What happens with categorical features?”

---

# Slide 15 — Finding #2: Race

### What to say

> “The authors perform a similar experiment with the categorical feature race.
>
> They start with four categories:
>
> White, Black, Asian and Other.
>
> Then they try different representations.
>
> One approach is called **one-vs-rest**, where one category is isolated and all the remaining categories are grouped together.
>
> For example:
>
> White versus everyone else,
>
> Black versus everyone else,
>
> Asian versus everyone else,
>
> and so on.
>
> They also consider different combinations where multiple race categories are merged.
>
> And again, the SHAP importance of race changes.
>
> Overall, the average SHAP value of race decreases compared with the original representation.
>
> More importantly, the fraction of observations for which race is the most important feature can decrease substantially.”

### Important nuance to say

> “There's also an interesting observation for the Asian-versus-rest case.
>
> Although race becomes the most important feature for fewer observations, the average SHAP value among those remaining observations can actually be higher.
>
> So simply looking at the overall average could hide what is happening to the subset of individuals for whom race still matters most.”

---

# Slide 16 — Inference

### What to say

> “So now we have seen the same basic phenomenon with two different kinds of features.
>
> For age, changing the bucketization changes the SHAP importance and ranking.
>
> For race, changing how categories are grouped can also reduce its apparent importance.
>
> So the first major conclusion is:
>
> **SHAP is sensitive to how features are represented.**
>
> And now we reach the more interesting question.
>
> If this representation affects what an auditor sees, could someone deliberately choose a representation that hides the importance of a protected feature?
>
> That's where the paper moves from sensitivity to an actual attack.”

---

# Slide 17 — Feature-Engineering Attack

### What to say

> “Now let's go back to our original auditor scenario.
>
> Imagine there are two parties.
>
> The first is the **vendor**.
>
> The vendor has access to the data, the model, and the complete data-engineering and modeling pipeline.
>
> The second is the **auditor**.
>
> The auditor receives the preprocessed data and the model, and can generate SHAP explanations, but does not control the data-engineering process.
>
> The vendor's adversarial goal is very specific:
>
> It wants the model to use a protected feature, but it doesn't want that protected feature to appear highly important when the auditor looks at SHAP.
>
> And the paper shows that feature representation can potentially be used to achieve exactly this.”

---

# Slide 18 — Bayesian Optimization Can Hide Protected Features

### What to say

> “For a continuous feature such as age, the authors formulate this as an optimization problem.
>
> Instead of manually choosing bucket boundaries, we want to search for bucket boundaries that make age appear less important according to SHAP.
>
> But we also want to maintain explanation fidelity.
>
> So there are essentially two objectives:
>
> **reduce the SHAP importance or rank of age,**
>
> while **maintaining sufficient fidelity.**
>
> The authors use Bayesian Optimization to search for these bucket boundaries.
>
> This is appropriate because evaluating SHAP for each possible representation can be computationally expensive, and Bayesian Optimization is designed for this kind of expensive black-box optimization problem.”

---

# Slide 19 — Bayesian Optimization Can Hide Protected Features

### What to say

> “This slide shows the actual attack process.
>
> The optimizer searches over different representations of age.
>
> For each representation, we calculate the SHAP rank of age and the fidelity of the resulting explanations.
>
> The optimizer then uses this information to search for better representations.
>
> In the experiments, the authors fix the minimum and maximum age values at 17 and 94 and optimize the bucket boundaries.
>
> They run 300 optimization iterations.
>
> Importantly, they impose a fidelity constraint: the attack must have fidelity at least as good as the equi-width bucketization baseline.
>
> So the goal isn't simply:
>
> *'Make age disappear from SHAP.'*
>
> It is:
>
> *'Make age less important according to SHAP while still producing a sufficiently faithful explanation.'*”

---

# Slide 20 — Final Takeaway

### What to say

> “So let's come back to the main idea.
>
> The paper shows that SHAP explanations depend on how features are represented.
>
> This is important because feature representation is usually treated as a data-engineering decision, something that happens before we even think about explainability.
>
> But the experiments show that these choices can directly affect the explanation.
>
> And more importantly, the authors show that this sensitivity can be deliberately exploited.
>
> An adversary can search for a representation that reduces the apparent importance of a protected feature while maintaining relatively high fidelity.
>
> So the main lesson is not that SHAP is useless.
>
> The more precise lesson is:
>
> **An explanation cannot be considered independently of the feature representation that produced it.**
>
> Therefore, when auditing an AI system, we shouldn't inspect only the model and the final SHAP explanation.
>
> We also need to consider the **data-engineering pipeline and the feature representations used by that pipeline.**”

---

# Slide 21 — Questions

Don't just say:

> “Thank you. Any questions?”

Instead, you can close the story first:

> “And if we go back to our auditor from the beginning, the original question was:
>
> **Did the model use Ann's age?**
>
> We might think SHAP gives us a direct answer.
>
> But after seeing these experiments, we know that the answer can depend on how age was represented.
>
> And that's really the central message of this paper.
>
> Thank you. I'm happy to take questions.”

---

## One thing I would change in your current delivery

There is a **very important distinction** you should emphasize when presenting Slides 13–19.

The paper has **two different experimental settings**:

### Sensitivity experiment

Here they change the representation in **both training and test/explainer data**:

```text
representation
      ↓
training data ──→ retrain model
      ↓
test data ──────→ SHAP
```

So this demonstrates:

> **Feature engineering changes SHAP explanations.**

### Attack experiment

Here they:

```text
Original data
     ↓
TRAIN MODEL
     ↓
FIX MODEL
     ↓
change only explainer input representation
     ↓
SHAP
```

So this demonstrates something stronger:

> **Different explanations can be produced for the same fixed model without retraining it.**

When you reach **Slide 17**, explicitly say:

> “Notice that the attack setting is different from the sensitivity experiment. Here, the model is already trained and fixed. We modify the representation supplied to the explainer, allowing different SHAP explanations to be produced for the same model.”

