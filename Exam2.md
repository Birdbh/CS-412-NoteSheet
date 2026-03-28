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
*   **Entropy:** For each row, calculate $e_i = - \sum (p \log p)$ where $p$ is (cell value / row sum), then calculate total conditional entropy by taking the weighted average of all $e_i$ values based on row sizes ($\sum (\frac{\text{row sum}}{N} \times e_i)$).
