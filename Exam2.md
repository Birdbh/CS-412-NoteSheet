### Clustering Algorithms (Complexity | Type | Description | Non-Convexity)
*   **K-Means:** $O(tKn)$ | Partitioning | Iteratively updates centroids to minimize Sum of Squared Errors (SSE) | Cannot handle non-convex data.
*   **K-Means++:** $O(tKn)$ | Partitioning | Improves K-Means by selecting the first centroid randomly and subsequent centroids probabilistically based on their squared distance from existing centroids | Cannot handle non-convex data.
*   **K-Medoids (PAM):** $O(K(n-K)^2)$ | Partitioning | Uses actual data points (medoids) as cluster centers, iteratively swapping them with non-medoids to minimize cost; highly robust to noise/outliers | Cannot handle non-convex data.
*   **Kernel K-Means:** Higher than $O(N^2)$ | Partitioning | Maps data to high-dimensional space using kernels (Polynomial, Gaussian RBF, Sigmoid) to separate linearly inseparable data | CAN handle non-convex data.
*   **EM (Expectation-Maximization) / GMM (Gaussian Mixture):** Computationally expensive | Probabilistic Mixture | Soft clustering assuming data is a mixture of distributions; E-step calculates posterior probability $P(C_i|x_j)$ assigning points; M-step re-estimates mean, covariance, and priors to maximize expected likelihood | Cannot handle non-convex data (assumes spherical/elliptical).
*   **BIRCH:** $O(N)$ | Hierarchical (Agglomerative Micro/Macro) | Scans data once to build an in-memory CF-tree compressing data into sub-clusters incrementally | Cannot handle non-convex data (favors spherical clusters).
*   **DBSCAN:** $O(N \log N)$ with index, $O(N^2)$ without | Density-based | Discovers clusters by finding maximal sets of density-connected points | CAN handle non-convex data.
*   **OPTICS:** $O(N \log N)$ with index | Density-based | Extends DBSCAN by ordering points using core-distance and reachability-distance to find hierarchically nested structures of varying densities | CAN handle non-convex data.
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

