# Lecture 16 : Counterfactual Explanations

Counterfactual explanations describe how to change an input minimally to achive a different predicated outcome from a model.

>[!Question]
> What is the smallest change to the input that would lead to different prediction ?

> [!Note]
> We are interested in alternative scenario that can change the outcome of prediction.


**Example** : Let's consider a bank loan application.
We could get a reason that would help us understand what needs to be changed for us to change the prediction from  `reject` to `approval`.

> [!Danger] Warning
> Counterfactuals suffers from **Rashomon effects**
> Each counterfactual will give different interpretation for a prediction.

Lets look at the requirements for counterfactual explanations.
- Minimal features (should change the few features)
- the instance generated needs to be similar as possible to the instance in question.
- It's desirable to generate multiple diverse counterfactual explanations
- the feature values of generating the instance should be similar to given instane

---

## Wachter Method

$$
arg min_{cf} d(x_{orig},x_{cf}) + \lambda (f(x_{cf}) - y_{target})^2
$$


d : is the distance function
$\lambda$ : is the trade-off parameter that balances btw small changes and acheiving the desired outcome/target.

The d is calcualted by manhattan distance scaled by Median Absolute Deviation.
$$
    d(x,x')=\sum_{i=1}^{n}  \frac{|x_i' - x_i|}{MAD(X_i)}
$$

$$
    MAD(X_i)=Median(|X_i - Median(X)|)
$$

> Understand the reason why they chose manhattan distance over euclidean
> **HINT** : sparcity

**Disadvantages**
- Only considers two criteria (ignores sparsity and realism)
- Currents its not spare and have unrealistic feature contribution.
- Struggles with categorical features with many level.

---


### Grower distance

The grower distance is a normalised distance metric designed to handle all the data types
- numerical
- catergorical
- binary

$$
    D_{Grower}(x_1,x_2) = \frac{1}{p} \sum_{j=1}^{p} d_j(x_1,x_2)
$$

where

$$
    d_j(x_1,x_2)= \frac{|x_{1} - x_{2}|}{max(X_k) - min(X_k)}
$$


### NSGA-II (Nondominated Search Genetic Algorithm)

It's a genetic algorithm.

> More detials in slides


**Domination** : A solution X dominates Y if
- X is no worse than Y in all objective functions
- X is strictly better than Y in at least one objective function

**Pareto optimality**
- A solution is Pareto optimal if no other solution is strictly better in all objectives
- A pareto-optimal solution is one that is not dominated by any other.

Nondomination sorting, NSGA-II ranks solutions into Pareto fronts.
In case of ties we use Crowding distance.

**Crowding Distance** :
It calculates how close the point is with its neighbour hood.

> [!TODO]
> Go through the example problems in the slides

**Selection** :
- NSGA-II uses binary tournaments selection which is based on Pareto rank and crowding distance to select individual for the next generation
- If one solution has a better rank, it is selected

**Recombinaition (Crossover)**:
- Creates new solution (offspring) by combining feature from two parents

**Mutation**
- Introduces small changes to solutions to maintain diversity and avoid premature convergence

**Children (Offspring)**
- Then next generation of solution formed from crossover and mutations.

> Look into the advantages from slides

## Dandl's method for generating counterfactuals

$$
    L(x,x',y',X^{obs})=(o_1(\hat{f}(x'),y'),o_2(x,x'),o_3(x,x'),o_4,(x',X^{obs}))
$$


$o_1$ - prediction
$o_2$ - distance
$o_3$ - sparsity
$o_4$ - realism

The overall objective of Dandl's method is minimize the above.


---

Refererence : [Chapter 15](https://christophm.github.io/interpretable-ml-book/counterfactual.html)

---