### Evaluation Metrics & Model Selection
* **Accuracy:** $(TP + TN) / (TP + TN + FP + FN)$ | Overall correctness of the model.
* **Error Rate:** $(FP + FN) / (TP + TN + FP + FN)$ | Equivalent to $1 - \text{Accuracy}$.
* **Precision:** $TP / (TP + FP)$ | Percentage of positive predictions that are actually positive.
* **Sensitivity (Recall / True Positive Rate):** $TP / (TP + FN)$ | Percentage of actual positives correctly identified.
* **Specificity (True Negative Rate):** $TN / (TN + FP)$ | Percentage of actual negatives correctly identified.
* **F1-Measure:** $\frac{2 \times (\text{Precision} \times \text{Recall})}{\text{Precision} + \text{Recall}}$ | Harmonic mean of Precision and Recall.
* **$F_\beta$ Measure:** $\frac{(1 + \beta^2) \times \text{Precision} \times \text{Recall}}{(\beta^2 \times \text{Precision}) + \text{Recall}}$ | Assigns $\beta$ times as much weight to recall as to precision.
* **ROC Curve:** Plots True Positive Rate (Sensitivity) vs. False Positive Rate ($1 - \text{Specificity}$). Closer to top-left is better; different points are obtained by changing the decision threshold.
* **Small Dataset Evaluation:** Use Leave-one-out cross-validation or $.632$ Bootstrap instead of standard holdout methods.
* **.632 Bootstrap:** A robust method for small datasets. You create a training set by sampling $N$ instances uniformly with replacement from the original data. The probability of an instance *not* being chosen is $(1 - 1/N)^N \approx 0.368$, meaning the training set contains roughly $63.2\%$ of the original unique data points, and the remaining $36.8\%$ form the test set. The overall accuracy is calculated as: $Acc = 0.632 \times Acc_{test} + 0.368 \times Acc_{train}$.
* **Multi-Class Classification:** **One-vs-All** builds $m$ classifiers (each class vs. the rest); **One-vs-One** builds $m(m-1)/2$ binary classifiers (every possible pair) and uses voting to predict, generally performing better.

### Decision Trees & Information Theory
* **Entropy:** $H(Y) = -\sum (p_i \log_2(p_i))$ | Measure of uncertainty; lower entropy = lower uncertainty.
* **Conditional Entropy $H(T|C)$:** Represents the entropy of the ground truth given the clustering; first calculate each row's individual entropy $H(T|C_i) = - \sum (p \ln p)$ where $p$ is (Cell Value / Cluster Row Sum), then calculate the final value by taking the weighted average of all rows $\sum (\frac{\text{Cluster Row Sum}}{N} \times H(T|C_i))$.
* **Information Gain:** $InfoGain(A) = H(Y) - H(Y|A)$ | Expected reduction in entropy caused by partitioning on attribute $A$; split on the attribute with the max InfoGain.
* **Maximal Leaf Nodes:** $\min(N, 2^K)$ | For $N$ data points and $K$ binary features, representing the absolute maximum complexity of the tree.
* **Overfitting Management:** Controlled via **Pre-pruning** (halting growth early based on constraints) or **Post-pruning** (removing branches after growth if validation accuracy drops).
* **RainForest Algorithm:** A framework for fast decision tree construction on large datasets that cannot fit in main memory. It separates the scalability of the tree construction from the split evaluation by maintaining an **AVC-set** (Attribute-Value, Class label) structure at each node. This AVC-set holds the aggregated frequency statistics needed to calculate Information Gain/Entropy, allowing the tree to be built without keeping the raw data in memory.
* **Gain Ratio:** $Gain(A) / SplitInfo(A)$ | Overcomes Information Gain's bias toward attributes with a large number of distinct values (used in C4.5). $SplitInfo(A) = -\sum \frac{|D_j|}{|D|} \log_2 \frac{|D_j|}{|D|}$ acts as a penalty term.
* **Gini Index:** $1 - \sum p_i^2$ | A measure of dataset impurity used in CART algorithms. Lower Gini = lower impurity (more homogeneous). The best split maximizes the reduction in impurity: $\Delta Gini(A) = Gini(D) - Gini_A(D)$.
* **Chi-Squared ($\chi^2$) Statistic:** $\sum \frac{(O - E)^2}{E}$ | Used by CHAID to test for statistical independence between a candidate attribute and the class label. Helps prevent overfitting by stopping splits if the association is not statistically significant.
* **Minimum Description Length (MDL):** $\text{Cost(Tree)} + \text{Cost(Data|Tree)}$ | An information-theoretic selection measure based on Occam's Razor. Evaluates alternative splits by favoring the model that requires the fewest bits to encode both the tree structure itself and any misclassification errors.

### Ensemble Methods
* **Bagging (Bootstrap Aggregating):** Trains base models independently on random subsamples (with replacement) to reduce variance.
* **Random Forest:** A bagging model using decision trees; trains on subsets of data *and* uses a random subset of features for splitting at each node to avoid overfitting.
* **Boosting (e.g., AdaBoost):** Trains models iteratively where each new model focuses on instances misclassified by the previous ones. Highly susceptible to overfitting if data is noisy.

### Bayesian Classification
* **Bayes Theorem:** $P(C|X) = \frac{P(X|C) P(C)}{P(X)}$ | Computes the posterior probability of class $C$ given observation $X$.
* **Naïve Bayes Assumption:** $P(X|C) = \prod_{i=1}^{n} P(x_i|C)$ | Assumes all attributes are conditionally independent given the class label. Faster but rarely strictly true in reality.
* **Bayesian Networks:** Directed Acyclic Graphs (DAGs) representing joint probability distributions and conditional independences. Can be simplified visually using Plate Notation.
* **Cascade (Chain Structure):** $A \rightarrow C \rightarrow B$ | If $C$ is known, the path is blocked, meaning $A$ and $B$ are conditionally **independent** ($A \perp B \mid C$). If $C$ is unknown, $A$ and $B$ are dependent.
* **Common Parent (Fork Structure):** $A \leftarrow C \rightarrow B$ | If the common cause $C$ is known, the path is blocked, meaning $A$ and $B$ are conditionally **independent** ($A \perp B \mid C$). If $C$ is unknown, they are dependent. $P(B|A) \cdot P(C|A) = P(B,C \mid A)$
* **V-Structure (Common Child / Collider):** $A \rightarrow C \leftarrow B$ | If $C$ (or any of its descendants) is known, the path *opens*, meaning $A$ and $B$ become conditionally **dependent** (a phenomenon known as "explaining away"). If $C$ is unknown, $A$ and $B$ are strictly **independent** ($A \perp B$).
* **Zero-Probability Problem:** In Naïve Bayes, if a specific attribute value never occurs for a given class in the training data, its conditional probability becomes zero. Because probabilities are multiplied, this single zero ruins the entire prediction.
* **Laplacian Correction (Laplace Smoothing):** Solves the zero-probability problem by adding 1 to every count so that no probability is ever strictly zero. 
* **Laplacian Formula:** $P(x_i|C) = \frac{\text{count}(x_i, C) + 1}{\text{count}(C) + |V|}$ | Where $\text{count}(x_i, C)$ is the number of times attribute value $x_i$ appears in class $C$, $\text{count}(C)$ is the total number of items in class $C$, and $|V|$ is the total number of distinct values that attribute $x$ can take.

### Linear Classifiers & SVM
* **Linear Classifier:** $W^T X + b = 0$ | Represents a separating hyperplane used for binary classification.
* **SVM Objective:** Searches for the Maximum Marginal Hyperplane (MMH) to separate classes with the largest possible margin. Defended by **Support Vectors** (critical data points at the boundary).
* **Logistic Regression:** $P(Y=1|X) = \frac{1}{1 + e^{-(W^T X + b)}}$ | Predicts probability using the Sigmoid function.
* **Gradient Descent:** Iterative algorithm moving in the direction of the negative gradient to minimize a cost function (e.g., negative log-likelihood).
* **Selective Declustering (CB-SVM):** Handles class imbalance by clustering the majority class to reduce its size, but intentionally "declusters" (expands) macro-clusters near the decision boundary to maintain high precision for finding support vectors.

### Neural Networks & Deep Learning
* **Perceptron:** $y = f(W^T X + b)$ | A single neuron combining weights, bias, and a non-linear activation function $f$. Multiple layers required for non-linear problems like XOR.
* **Backpropagation:** Computes gradients using error terms from the *next* layer to update weights. Can get stuck in local minima (does not guarantee global minima).
* **Convolutional Neural Networks (CNN):** Ideal for spatial data (images); relies on Parameter Sharing, Sparse Interactions, and Max Pooling.
* **Recurrent Neural Networks (RNN):** Ideal for sequential data; uses hidden states to preserve long-term dependencies.

### Pattern-Based Classification & KNN
* **K-Nearest Neighbors (KNN):** "Lazy learner" that stores training data and delays processing until prediction time. Can be used for classification or regression.
* **Distance-Weighted KNN:** Assigns greater weight to closer neighbors, usually calculated as $1/d$ where $d$ is the distance to the query point.
* **Pattern-Based Classification:** Uses frequent, discriminative patterns (e.g., $k$-itemsets) as features. Captures complex, higher-order interactions and handles graphs/sequences better than standard features.
* **CBA / CMAR Algorithms:** Classification methods that use high-confidence/high-support class association rules to form predictions.

### Classification with Weak Supervision
* **Distant Supervision:** Automatically labels data using external knowledge bases (e.g., extracting Wikipedia titles as quality phrases).
* **Transfer Learning:** Leverages a model trained on a similar task/domain to help learn a new, related task.
* **Active Learning:** Involves a human annotator; algorithm queries the most uncertain/informative unlabeled instances for human labeling. Evaluated on accuracy relative to the amount of manual labeling used.
* **Zero-Shot Learning:** Model predicts labels for classes that were completely unseen during the training phase.
