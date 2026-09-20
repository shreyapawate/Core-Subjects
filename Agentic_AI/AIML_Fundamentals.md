Absolutely — for the BMC Helix interview, focus on **intuition, when to use each algorithm, and common interview distinctions**, not mathematical derivations.

# 5. AIML FUNDAMENTALS

## 1. Machine Learning Types

### Supervised Learning

Training data contains **input features X + known target Y**.

Used when we want to predict an outcome.

#### Classification

Predicts a **discrete category/class**.

Examples:

* Spam / Not Spam
* High / Medium / Low priority ticket
* Fraud / Not Fraud

Common algorithms:

* Logistic Regression
* Decision Tree
* Random Forest
* XGBoost
* KNN
* SVM
* Naive Bayes

#### Regression

Predicts a **continuous numerical value**.

Examples:

* House price
* Ticket resolution time
* Sales prediction

Common algorithms:

* Linear Regression
* Decision Tree / Random Forest
* XGBoost

---

### Unsupervised Learning

Training data has **no target labels**.

#### Clustering

Groups similar data points.

Example:

* Group customers by behavior
* Group IT tickets by similarity

**K-Means** → most common basic clustering algorithm.

#### Dimensionality Reduction

Reduces number of features while trying to preserve useful information.

Used for:

* Visualization
* Removing redundant features
* Faster ML training
* Noise reduction

Common technique:

* **PCA (Principal Component Analysis)**

---

# 2. Important Algorithms

## Linear Regression

**Purpose:** Predict continuous values.

Idea:

> Find the best-fit relationship between input features and a numerical target.

Example:
`Years of Experience → Salary`

Key points:

* Output is continuous.
* Uses a linear relationship.
* Sensitive to outliers.
* Common baseline regression model.

**Interview:**

> Linear regression is used when the target variable is continuous and we want to model its relationship with input features using a linear function.

---

## Logistic Regression

Despite its name, primarily used for **classification**.

Idea:

> Calculates probability of belonging to a class and applies a threshold to make the prediction.

Example:
`Ticket features → High Priority / Not High Priority`

Key points:

* Uses **sigmoid function** for binary classification.
* Output probability is between `0 and 1`.
* Threshold commonly starts around `0.5`, but can be changed.
* Can be extended to multiclass classification.

**Important distinction:**

* Linear Regression → continuous output
* Logistic Regression → classification probability

---

## Decision Tree

A tree of **if-else decisions**.

Example:

```text
Ticket severity > 8?
      |
   YES → Priority = HIGH
      |
     NO
      ↓
Customer affected?
      |
   YES → MEDIUM
```

Key points:

* Works for classification and regression.
* Easy to interpret.
* Handles nonlinear relationships.
* Can work with numerical and categorical features.
* Can overfit easily if tree becomes too deep.

Important hyperparameters:

* `max_depth`
* `min_samples_split`
* `min_samples_leaf`

---

## Random Forest

**Ensemble of many Decision Trees.**

Idea:

> Train multiple trees on different samples/features and combine their predictions.

For classification → majority voting
For regression → averaging

Key points:

* Reduces overfitting compared with a single deep tree.
* More robust than one decision tree.
* Good general-purpose algorithm.
* Less interpretable than a single tree.

**Interview distinction:**

```text
Decision Tree
      ↓
One tree → higher variance

Random Forest
      ↓
Many trees → combine predictions → more stable
```

---

## XGBoost

**Gradient boosting algorithm based on decision trees.**

Idea:

> Build trees sequentially, where each new tree focuses on correcting errors made by previous trees.

```text
Tree 1
  ↓
Errors
  ↓
Tree 2 learns errors
  ↓
Errors
  ↓
Tree 3
  ↓
Final prediction
```

Key points:

* Very powerful for **tabular/structured data**.
* Uses boosting rather than bagging.
* Trees are built **sequentially**.
* Usually requires careful hyperparameter tuning.
* Can overfit without proper regularization.

### Random Forest vs XGBoost

| Random Forest                     | XGBoost                                      |
| --------------------------------- | -------------------------------------------- |
| Bagging                           | Boosting                                     |
| Trees built largely independently | Trees built sequentially                     |
| Focuses on reducing variance      | Focuses on correcting previous errors        |
| Usually easier to tune            | Usually more tuning-sensitive                |
| Strong general baseline           | Often very strong on structured/tabular data |

---

## KNN — K-Nearest Neighbors

Idea:

> Predict based on the labels/values of the closest K data points.

Example:

```text
New ticket
   ↓
Find K most similar tickets
   ↓
Look at their classes
   ↓
Predict class
```

Key points:

* Can be used for classification and regression.
* **Instance-based / lazy learning**.
* Little training computation; prediction can be expensive.
* Sensitive to feature scaling.
* Choice of `K` matters.

**Important:** Distance-based algorithms require appropriate feature scaling.

---

## SVM — Support Vector Machine

Idea:

> Find a decision boundary that separates classes with the maximum possible margin.

```text
Class A   |       Class B
 ● ● ●    |    ○ ○ ○
 ● ● ●    |    ○ ○ ○
          ↑
     decision boundary
```

Key points:

* Mainly used for classification.
* **Support vectors** are the critical data points closest to the boundary.
* Maximizes margin between classes.
* Kernel functions allow nonlinear boundaries.
* Can work well with high-dimensional data.
* Scaling is generally important.

Common kernels:

* Linear
* Polynomial
* RBF

---

## K-Means

Unsupervised clustering algorithm.

Idea:

> Divide data into K clusters by assigning points to the nearest centroid and repeatedly updating centroids.

```text
Data
 ↓
Choose K
 ↓
Initialize centroids
 ↓
Assign points to nearest centroid
 ↓
Recalculate centroids
 ↓
Repeat until convergence
```

Key points:

* `K` = number of clusters.
* Uses distance, commonly Euclidean distance.
* Sensitive to feature scaling.
* Sensitive to initial centroid placement.
* Works best when clusters are reasonably compact/separable.

---

## Naive Bayes

Probabilistic classification algorithm based on **Bayes' theorem**.

genui{"learning_viz":{"type_id":"BAYES_THEOREM","initial_values":{"pA":0.2,"pBGivenA":0.85,"pBGivenNotA":0.1}}}

Key assumption:

> Features are conditionally independent given the class.

Example:
`Email words → Spam / Not Spam`

Key points:

* Very fast.
* Works particularly well for text classification.
* Requires relatively little training data.
* "Naive" assumption = features are treated as conditionally independent.
* Common variants: Gaussian, Multinomial, Bernoulli.

---

# 3. Train / Validation / Test

### Training Set

Used to **learn model parameters**.

### Validation Set

Used during development to:

* Tune hyperparameters
* Compare models
* Select model configuration

### Test Set

Used **only for final evaluation** on unseen data.

Typical split:

```text
Dataset
   |
   ├── Training → learn
   |
   ├── Validation → tune/select
   |
   └── Test → final evaluation
```

### Important interview point

**Never tune your model using the test set.**

Otherwise, test performance becomes overly optimistic.

---

# 4. Overfitting vs Underfitting

## Overfitting

Model learns:

> Data + noise

Result:

* Very good training performance
* Poor unseen-data performance

Example:
A very deep decision tree memorizes training examples.

Solutions:

* Regularization
* Simpler model
* More training data
* Cross-validation
* Feature selection
* Early stopping
* Pruning for trees

---

## Underfitting

Model is **too simple** to capture the underlying pattern.

Result:

* Poor training performance
* Poor validation/test performance

Solutions:

* More expressive model
* Better features
* Reduce excessive regularization
* Train longer where applicable

---

# 5. Bias vs Variance

### High Bias

Model makes overly strong simplifying assumptions.

Usually associated with:
**Underfitting**

### High Variance

Model is highly sensitive to training data.

Usually associated with:
**Overfitting**

```text
High Bias     → Underfitting
High Variance → Overfitting
```

### Bias-Variance Tradeoff

Goal:

> Find a model complexity that generalizes well to unseen data.

---

# 6. Regularization

Technique used to **reduce overfitting** by penalizing overly complex models.

Main types:

### L1 — Lasso

Adds penalty based on absolute coefficient values.

Important property:

* Can drive some coefficients to **zero**
* Useful for feature selection

### L2 — Ridge

Adds penalty based on squared coefficient values.

Important property:

* Shrinks coefficients toward zero
* Usually does not make them exactly zero

```text
L1 → can eliminate features
L2 → shrinks features
```

For tree/boosting models, regularization can also involve constraints such as tree depth, minimum samples, learning rate, etc.

---

# 7. Feature Engineering

Creating or transforming input features to make them more useful for the model.

Examples:

Raw:

```text
Date = 21/09/2026
```

Engineered:

```text
Day = 21
Month = 9
DayOfWeek = Monday
```

Other examples:

* Extracting text features
* Log transformation
* Interaction features
* Aggregations
* Encoding categorical variables
* Creating domain-specific features

**Interview answer:**

> Feature engineering means transforming raw data into meaningful features that help a model learn useful patterns.

---

# 8. Feature Scaling

Putting numerical features on comparable scales.

Common methods:

### Standardization

Transforms values so they have approximately:

* Mean = 0
* Standard deviation = 1

### Min-Max Scaling

Maps values to a specified range, commonly `[0,1]`.

### Algorithms especially affected by scaling:

* KNN
* K-Means
* SVM
* Logistic Regression
* Neural networks

### Usually less important for:

* Decision Trees
* Random Forest
* XGBoost

**Why?**

Distance/gradient-based models can be affected by feature magnitude, while tree-based models split using feature thresholds.

---

# 9. Cross-Validation

Technique for estimating how well a model generalizes.

### K-Fold Cross-Validation

```text
Dataset
 ↓
Split into K folds

Fold 1 → Validation
Folds 2-5 → Training

Fold 2 → Validation
Folds 1,3-5 → Training

...
```

Final performance = average performance across folds.

Benefits:

* More reliable evaluation than one random split.
* Useful for model/hyperparameter selection.
* Helps detect whether performance is consistent.

### Important

For classification, **Stratified K-Fold** is commonly preferred because it preserves class proportions.

---

# 10. Data Leakage

One of the most important interview concepts.

**Data leakage = information from outside the training data improperly influences model training.**

Example:

You want to predict whether a ticket will be resolved.

If you create a feature using:

`final_resolution_time`

before making the prediction, the model has access to information that would not actually be available at prediction time.

→ Leakage.

### Another common example

Scaling the **entire dataset before train/test splitting**:

```text
Wrong:
Entire dataset → calculate scaling parameters → split
```

Instead:

```text
Split data
   ↓
Training → fit scaler
   ↓
Validation/Test → transform using training scaler
```

### Why leakage is dangerous

It produces:

> Artificially high validation/test performance that does not represent real-world performance.

---

# 11. High-Value Interview Comparisons

### Classification vs Regression

```text
Classification → category
Regression     → continuous number
```

### Supervised vs Unsupervised

```text
Supervised   → labeled target
Unsupervised → no target labels
```

### Bagging vs Boosting

```text
Bagging
→ models trained independently
→ combine predictions
→ Random Forest

Boosting
→ models trained sequentially
→ each model improves previous errors
→ XGBoost
```

### Parametric vs Non-Parametric

**Parametric:**
Assumes a fixed form/limited set of parameters.

Examples:

* Linear Regression
* Logistic Regression
* Naive Bayes

**Non-parametric:**
Model complexity can adapt to data.

Examples:

* KNN
* Decision Tree

### Generative vs Discriminative

**Generative:**
Models how data/classes are generated.

Example:

* Naive Bayes

**Discriminative:**
Focuses on decision boundary / predicting target.

Examples:

* Logistic Regression
* SVM
* Decision Tree

---

# 12. Algorithm Selection — Quick Interview Map

| Problem                              | Good candidates                         |
| ------------------------------------ | --------------------------------------- |
| Continuous prediction                | Linear Regression, XGBoost              |
| Binary classification                | Logistic Regression, Tree, XGBoost, SVM |
| Text classification                  | Naive Bayes, Logistic Regression        |
| Nonlinear tabular data               | Random Forest, XGBoost                  |
| Similarity/distance-based prediction | KNN                                     |
| Finding natural groups               | K-Means                                 |
| High-dimensional classification      | SVM                                     |
| Interpretable decision rules         | Decision Tree                           |

## BMC Helix-style examples

```text
Ticket priority prediction
→ Classification
→ Logistic Regression / Random Forest / XGBoost

Ticket resolution time prediction
→ Regression
→ Linear Regression / XGBoost

Group similar incidents
→ Clustering
→ K-Means

Spam/support email classification
→ Classification
→ Naive Bayes / Logistic Regression
```

# MUST REMEMBER

1. **Classification → categorical output**
2. **Regression → continuous output**
3. **Random Forest → bagging**
4. **XGBoost → boosting**
5. **KNN/K-Means/SVM → scaling is important**
6. **Trees → scaling usually unnecessary**
7. **Overfitting → high variance**
8. **Underfitting → high bias**
9. **L1 → feature selection / coefficients can become zero**
10. **L2 → coefficient shrinkage**
11. **Validation → tuning; Test → final evaluation**
12. **Cross-validation → more reliable generalization estimate**
13. **Data leakage → unrealistically good evaluation**
14. **Feature engineering → create useful representations from raw data**
15. **K-Means → unsupervised clustering**
16. **Naive Bayes → conditional independence assumption**
17. **SVM → maximum-margin decision boundary**
18. **XGBoost → sequential error correction using boosting**
19. **Random Forest → many independent trees + aggregation**
20. **Scaling should be fitted on training data only**

These are the **AIML fundamentals worth prioritizing for an Agentic-AI interview**; you don't need to memorize mathematical derivations of every algorithm unless the interviewer specifically asks.
