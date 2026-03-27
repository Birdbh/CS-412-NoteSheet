# Cluster Analysis Cheat Sheet: Formulas & Hand Calculations

## 1. Partitioning-Based Clustering

**K-Means Objective (Sum of Squared Errors - SSE)**
* **Formula:** `SSE = Σ(from k=1 to K) Σ(x_i in C_k) ||x_i - c_k||^2`
* **Terms:**
  * `K`: Total number of clusters.
  * `C_k`: The specific cluster you are currently evaluating.
  * `x_i`: A single data point inside cluster `C_k`.
  * `c_k`: The centroid (mean) of cluster `C_k`.
  * `||x_i - c_k||^2`: The squared Euclidean distance between point `x` and centroid `c`.
* **Hand Calc Tip:** To find the SSE for a cluster, calculate its mean. Then, find the distance from each point to that mean, square each distance, and add them all together. Repeat for all clusters and sum the results.

---

## 2. Hierarchical Clustering (Linkage & Distance)

**Ward's Criterion (Increase in SSE when merging Cluster A and Cluster B)**
* **Formula:** `ΔSSE = [ (N_A * N_B) / (N_A + N_B) ] * dist(c_A, c_B)^2`
* **Terms:**
  * `N_A` and `N_B`: The number of data points inside Cluster A and Cluster B.
  * `c_A` and `c_B`: The centroids (means) of Cluster A and Cluster B.
  * `dist(c_A, c_B)^2`: The squared distance between the two centroids.
* **Hand Calc Tip:** Use this when deciding which two clusters to merge. Calculate this value for all possible pairs of clusters. Merge the pair that gives the smallest `ΔSSE` value.

**General Linkage Distance (Between Cluster A and Cluster B)**
* **Single Link (Min):** Find the smallest distance between *any* point in A and *any* point in B.
* **Complete Link (Max):** Find the largest distance between *any* point in A and *any* point in B.

---

## 3. Clustering Validation: External (Ground Truth is Known)

*Setup Tip: Always draw a Contingency Table (Grid) first. Make the rows your Clusters (C) and columns your Ground Truth partitions (T). The number inside the cell is `|C_i ∩ T_j|` (points in Cluster i that belong to Class j).*

**Purity**
* **Formula:** `Purity = (1/N) * Σ(i=1 to k) max_j |C_i ∩ T_j|`
* **Terms:**
  * `N`: Total number of data points.
  * `max_j |C_i ∩ T_j|`: The highest value (majority class count) in row `i`.
* **Hand Calc Tip:** Look at your grid. Circle the largest number in each row. Add those circled numbers together, then divide by the total number of data points.

**Precision and Recall (For a specific cluster C_i)**
* **Precision Formula:** `|C_i ∩ T_j| / |C_i|`
  * **Hand Calc Tip:** (Max cell value in the row) / (Sum of the entire row).
* **Recall Formula:** `|C_i ∩ T_j| / |T_j|`
  * **Hand Calc Tip:** (Max cell value in the row) / (Sum of that specific column).
* **F-Measure Formula:** `(2 * Precision * Recall) / (Precision + Recall)`

**Pairwise Measures (Rand & Jaccard)**
* **Terms:** You are evaluating pairs of points. Total pairs = `N(N-1)/2`.
  * `TP` (True Positive): Pairs in the *same* cluster AND *same* true class. 
  * `FP` (False Positive): Pairs in the *same* cluster but *different* true class.
  * `FN` (False Negative): Pairs in *different* clusters but *same* true class.
  * `TN` (True Negative): Pairs in *different* clusters AND *different* true class.
* **Hand Calc Tip for TP:** Look at your contingency grid. For every cell with a value `n`, calculate `n(n-1)/2`. Add all these results together to get your total `TP`.
* **Jaccard:** `TP / (TP + FN + FP)` (Useful because it ignores the massive amount of True Negatives).
* **Rand Statistic:** `(TP + TN) / Total Pairs`

---

## 4. Clustering Validation: Internal (No Ground Truth)

**Silhouette Coefficient (For evaluating a single point `i`)**
* **Formula:** `s_i = (b_i - a_i) / max(a_i, b_i)`
* **Terms:**
  * `a_i`: The average distance from point `i` to all other points in its OWN cluster.
  * `b_i`: The average distance from point `i` to all points in the CLOSEST neighboring cluster.
* **Hand Calc Tip:** 1. Measure distance from your target point to all other points in its own group. Average them. This is `a`.
  2. Measure distance from your target point to all points in Group 2. Average them. 
  3. Do the same for Group 3, etc. 
  4. Find the smallest average from steps 2 & 3. This is `b`. 
  5. Plug into the formula. A result close to +1 means the point is perfectly clustered.

**Beta-CV**
* **Formula:** `Beta-CV = (Mean Intra-cluster Distance) / (Mean Inter-cluster Distance)`
* **Hand Calc Tip:** Find the average length of all edges connecting points *within* the same clusters. Divide that by the average length of all edges connecting points in *different* clusters. You want this ratio to be as small as possible.
