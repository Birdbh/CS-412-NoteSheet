Here is your highly condensed, one-line-per-entry bulleted cheat sheet covering all your requested constraints:

### Clustering Algorithms (Complexity | Type | Description | Non-Convexity)
*   **K-Means:** $O(tKn)$ | Partitioning | Iteratively updates centroids to minimize Sum of Squared Errors (SSE) | Cannot handle non-convex data.
*   **Kernel K-Means:** Higher than $O(N^2)$ | Partitioning | Maps data to high-dimensional space using kernels (Polynomial, Gaussian RBF, Sigmoid) to separate linearly inseparable data | CAN handle non-convex data.
*   **EM (Expectation-Maximization) / GMM (Gaussian Mixture):** Computationally expensive | Probabilistic Mixture | Soft clustering assuming data is a mixture of distributions; E-step calculates posterior probability $P(C_i|x_j)$ assigning points; M-step re-estimates mean, covariance, and priors to maximize expected likelihood | Cannot handle non-convex data (assumes spherical/elliptical).
*   **BIRCH:** $O(N)$ | Hierarchical (Agglomerative Micro/Macro) | Scans data once to build an in-memory CF-tree compressing data into sub-clusters incrementally | Cannot handle non-convex data (favors spherical clusters).
*   **DBSCAN:** $O(N \log N)$ with index, $O(N^2)$ without | Density-based | Discovers clusters by finding maximal sets of density-connected points | CAN handle non-convex data.
*   **STING:** $O(K)$ where $K$ is # of lowest-level cells | Grid-based | Divides spatial area into hierarchical rectangular cells storing pre-calculated stats (count, mean, min, max, std dev) | CAN handle non-convex data.
*   **CLIQUE:** Scales linearly | Grid-based & Subspace (Density) | Discretizes space into grids, finds dense units in 1D, and uses Apriori principle (if not dense in 1D, cannot be dense in 2D) to build higher-dimensional subspace clusters | CAN handle non-convex data in subspaces.

### DBSCAN Point Definitions & Relations
*   **Core Point:** A point with at least $MinPts$ objects within its $Eps$ ($\epsilon$) neighborhood radius.
*   **Border Point:** A point with fewer than $MinPts$ inside its radius, but falls within the $Eps$ neighborhood of a core point.
*   **Directly Density-Reachable:** Point $p$ is directly reachable from $q$ if $p$ is inside the $Eps$-neighborhood of $q$, and $q$ is a core point.
*   **Density-Reachable:** Point $p$ is reachable from $q$ if there is a chain of directly density-reachable points linking $q$ to $p$.
*   **Density-Connected:** Point $p$ and point $q$ are connected if there is a third point $o$ such that both $p$ and $q$ are density-reachable from $o$.

### BIRCH CF Vector
*   **Clustering Feature (CF):** A triplet $CF = (N, LS, SS)$ representing Count ($N$), Linear Sum of points ($\sum x_i$), and Square Sum of points ($\sum x_i^2$); used to incrementally and efficiently calculate cluster centroid, radius, and diameter by simply adding vectors together ($CF_1 + CF_2$).

### Clustering Validation Methods
*   **Extrinsic (External - requires ground truth):** Purity, Maximum Matching, F-Measure (Precision/Recall), Conditional Entropy, Normalized Mutual Information (NMI), Jaccard Coefficient, Rand Statistic, Fowlkes-Mallow Measure.
*   **Intrinsic (Internal - data only):** BetaCV Measure, Normalized Cut, Modularity, Silhouette Coefficient (also acts as a relative measure).

### Table Calculations ($C_n$ = cluster rows, $T_n$ = ground truth columns, $N$ = total sum)
*   **Purity:** Find the highest value in each cluster row, sum those maximums together, and divide by the total number of objects $N$.
*   **TP (True Positive):** For every individual cell value $v$ in the table, calculate combinations $\frac{v(v-1)}{2}$, and sum them all together.
*   **FP (False Positive):** Calculate total pairs for each cluster row using row sum $R$, $\frac{R(R-1)}{2}$, sum them all together, and subtract the $TP$.
*   **FN (False Negative):** Calculate total pairs for each ground truth column using col sum $C$, $\frac{C(C-1)}{2}$, sum them all together, and subtract the $TP$.
*   **TN (True Negative):** Calculate total possible pairs in the entire dataset $\frac{N(N-1)}{2}$, and subtract $(TP + FP + FN)$.
*   **Clustering Entropy $H(C)$:** Represents the entropy of the output clusters; calculated as $- \sum (P_{C_i} \ln P_{C_i})$ where $P_{C_i}$ is the probability of a point being in cluster $i$ (Cluster Row Sum / Total $N$).
*   **Ground Truth Entropy $H(T)$:** Represents the entropy of the actual classes; calculated as $- \sum (P_{T_j} \ln P_{T_j})$ where $P_{T_j}$ is the probability of a point being in class $j$ (Ground Truth Column Sum / Total $N$).
*   **Conditional Entropy $H(T|C)$:** Represents the entropy of the ground truth given the clustering; first calculate each row's individual entropy $H(T|C_i) = - \sum (p \ln p)$ where $p$ is (Cell Value / Cluster Row Sum), then calculate the final value by taking the weighted average of all rows $\sum (\frac{\text{Cluster Row Sum}}{N} \times H(T|C_i))$.

The most likely reason you are getting the wrong answer is the **logarithmic base** you are using on your calculator. 

In computer science, entropy is traditionally calculated using log base 2 ($\log_2$), but many data mining courses and platforms use the **natural log ($\ln$ or $\log_e$)** by default when they just write "$\log$". 

Let's break down the exact math step-by-step using the **natural log ($\ln$)** to show you how to get the numbers in the options:

### 1. Calculate the Output Clustering Entropy $H(C)$
First, get the total number of points in each cluster:
*   **Cluster 1 ($C_1$):** $4 + 5 + 7 = 16 \text{ points}$
*   **Cluster 2 ($C_2$):** $1 + 3 + 10 = 14 \text{ points}$
*   **Total ($N$):** $30 \text{ points}$

Now, apply the entropy formula $H(C) = - \sum (P_{C_i} \times \ln P_{C_i})$:
*   $P(C_1) = 16 / 30 = 0.5333$
*   $P(C_2) = 14 / 30 = 0.4667$

$H(C) = - [ (0.5333 \times \ln(0.5333)) + (0.4667 \times \ln(0.4667)) ]$
$H(C) = - [ (0.5333 \times -0.6286) + (0.4667 \times -0.7621) ]$
$H(C) = - [ -0.3352 - 0.3557 ] = \mathbf{0.6909}$ 

### 2. Calculate the Ground Truth Entropy $H(T)$
First, get the total number of points in each *class* (color) by adding them across both clusters:
*   **Yellow ($T_1$):** $4 \text{ (from C1)} + 1 \text{ (from C2)} = 5 \text{ points}$
*   **Red ($T_2$):** $5 \text{ (from C1)} + 3 \text{ (from C2)} = 8 \text{ points}$
*   **Blue ($T_3$):** $7 \text{ (from C1)} + 10 \text{ (from C2)} = 17 \text{ points}$
*   **Total ($N$):** $30 \text{ points}$

Now, apply the entropy formula $H(T) = - \sum (P_{T_j} \times \ln P_{T_j})$:
*   $P(T_{yellow}) = 5 / 30 = 0.1667$
*   $P(T_{red}) = 8 / 30 = 0.2667$
*   $P(T_{blue}) = 17 / 30 = 0.5667$

$H(T) = - [ (0.1667 \times \ln(0.1667)) + (0.2667 \times \ln(0.2667)) + (0.5667 \times \ln(0.5667)) ]$
$H(T) = - [ (0.1667 \times -1.7915) + (0.2667 \times -1.3216) + (0.5667 \times -0.5679) ]$
$H(T) = - [ -0.2986 - 0.3525 - 0.3218 ] = \mathbf{0.9730}$

### Conclusion
Depending on if this is a multiple-select question, both **"The entropy of the output clustering is 0.6909"** and **"The entropy of the ground truth is 0.9730"** are correct statements! 

When you sit for your exam, if your entropy calculations aren't matching the multiple-choice options, switch your calculator from $\log_{10}$ or $\log_2$ to the **natural log ($\ln$)**.
