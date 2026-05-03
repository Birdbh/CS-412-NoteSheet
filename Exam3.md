# CS 412 Exam 3 Cheat Sheet: Classification, Evaluation & Advanced Models

## 1. Evaluation Metrics & Model Selection
* **Confusion Matrix**: `TP` (True Positives), `TN` (True Negatives), `FP` (False Positives/Type I Error), `FN` (False Negatives/Type II Error).
* **Accuracy**: `(TP + TN) / All` — Overall correctness of the model.
* **Error Rate**: `(FP + FN) / All` or `1 - Accuracy`.
* **Sensitivity (Recall / True Positive Rate)**: `TP / P = TP / (TP + FN)` — Percentage of actual positives correctly identified.
* **Specificity (True Negative Rate)**: `TN / N = TN / (TN + FP)` — Percentage of actual negatives correctly identified.
* **Precision**: `TP / (TP + FP)` — Percentage of positive predictions that are actually positive.
* **F1-Measure**: `(2 * Precision * Recall) / (Precision + Recall)` — Harmonic mean of Precision and Recall.
* **F_beta Measure**: `((beta^2 + 1) * P * R) / (beta^2 * P + R)` — Assigns beta times as much weight to recall as to precision.
* **ROC Curve**: Plots True Positive Rate vs. False Positive Rate. Closer to top-left = better model. Shows tradeoff between sensitivity and specificity. Different points are obtained by changing the decision threshold.
* **Cross-Validation**: Stratified k-fold ensures class distribution in each fold is roughly the same as the initial data.
* **Evaluation on Small Data**: Use Leave-one-out cross-validation or .632 Bootstrap.

## 2. Decision Trees
* **Entropy**: `H(Y) = - Sum(p_i * log2(p_i))` — Measure of uncertainty. Lower entropy = lower uncertainty.
* **Information Gain**: `InfoGain(A) = H(Y) - H(Y|A)` — Expected reduction in entropy caused by partitioning on attribute A. Split on max InfoGain.
* **Maximal Leaf Nodes**: For `N` data points and `K` binary features, max leaves is `min(N, 2^K)`.
* **Overfitting**: Overly complex tree fitting noise. Handled by **Pre-pruning** (halting early) or **Post-pruning** (removing branches based on validation accuracy).
* **Continuous Attributes**: Discretize, or find best split point (midpoints between adjacent sorted values).
* **Strengths**: Fast, interpretable, handles various data types. 
* **Weaknesses**: Prone to overfitting if not pruned.

## 3. Ensemble Methods
* **Concept**: Combine multiple base classifiers to improve overall accuracy and robustness. You can ensemble different types (SVM, Trees, Naive Bayes).
* **Bagging (Bootstrap Aggregating)**: Trains models independently on random subsamples with replacement. Reduces variance.
* **Random Forest**: Type of bagging model using decision trees. Trains on subset of data *and* uses a random subset of features for splitting at each node. Avoids overfitting.
* **Boosting (e.g., AdaBoost)**: Trains models iteratively, where each new model focuses on instances misclassified by the previous ones.
* **Boosting Weakness**: Highly susceptible to overfitting if the data is noisy (models overfit to the noise).

## 4. Bayes Classifier & Bayesian Networks
* **Bayes Theorem**: `P(C|X) = (P(X|C) * P(C)) / P(X)` — Computes posterior probability `P(C|X)`.
* **Naïve Bayes Assumption**: Assumes all attributes are conditionally independent given the class label: `P(X|C) = P(x_1|C) * P(x_2|C) * ... * P(x_n|C)`.
* **Strengths/Weaknesses**: Comparable to decision trees, fast, incremental learning. Weakness: Independence assumption is rarely strictly true.
* **Bayesian Networks**: Directed Acyclic Graphs (DAGs) representing joint probability distributions and conditional independences.
* **Plate Notation**: Simplifies Bayesian Network graphs by grouping variables that share the same conditional probability table.

## 5. Linear Classifiers & Support Vector Machines (SVM)
* **Linear Classifier Equation**: `W * X + b = 0`. Represents a separating hyperplane.
* **SVM Objective**: Searches for the Maximum Marginal Hyperplane (MMH) to separate classes with the largest margin.
* **Support Vectors**: The critical data points that define the margin boundaries.
* **Logistic Regression**: Predicts probability using the Sigmoid function: `P(Y=1|X) = Sigmoid(W*X + b)`. 
* **Gradient Descent**: Iterative algorithm to minimize a cost function (e.g., negative log-likelihood). Moves in direction of negative gradient.

## 6. Neural Networks & Deep Learning
* **Perceptron**: A single neuron calculating `y = f(W^T * X + b)`. `f` is a non-linear activation function.
* **Deep Learning Strengths**: End-to-end learning, requires little to no feature engineering. Solves non-linear problems (e.g., XOR requires multi-layer).
* **Deep Learning Weaknesses**: High computational cost, black-box model, requires huge data.
* **Backpropagation**: Computes the gradient for a neuron using error terms from its *next* layer. Can get stuck in local minima (doesn't always reach global minima).
* **CNN (Convolutional Neural Networks)**: Best for spatial data (images). Relies on **Parameter Sharing** and **Sparse Interactions**. Uses max pooling.
* **RNN (Recurrent Neural Networks)**: Best for sequential data. Uses hidden states to preserve long-term dependencies.

## 7. Pattern-Based Classification & KNN
* **K-Nearest Neighbors (KNN)**: A "lazy learner" (eager learning is false). Stores training data and delays processing until prediction time.
* **Distance-Weighted KNN**: Gives greater weight to closer neighbors, typically using `1/distance`.
* **KNN Uses**: Can be used for both categorical classification and numerical prediction (regression).
* **Pattern-Based Classification**: Uses frequent, discriminative patterns (e.g., k-itemsets) as features.
* **Benefits of Patterns**: K-itemsets (where k is small but >1) generally have higher discriminative power than single features. Captures complex, higher-order interactions. Handles graphs/sequences nicely.
* **CBA / CMAR Algorithms**: Uses high-confidence/support class association rules for prediction.

## 8. Classification with Weak Supervision
* **Goal**: Train classifiers with limited or noisy labeled data.
* **Distant Supervision**: Uses external knowledge bases (e.g., extracting Wikipedia titles as quality phrases) to automatically label data.
* **Transfer Learning**: Uses a model trained on a similar task/domain to help learn the new task.
* **Active Learning**: Involves a human annotator in the loop. Selects the most uncertain/informative unlabeled instances for human labeling. After each iteration, the labeled set grows and the unlabeled pool shrinks. Evaluation considers both accuracy and amount of labeled data used.
* **Zero-Shot Learning**: Model predicts labels for classes that were completely unseen during training.
