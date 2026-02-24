***
## 1. Formulas & Statistical Measures
*   **Mean:** $\mu = \frac{1}{N} \sum x_i$
*   **Median:** Middle value. Positively skewed: Mode < Median < Mean. Negatively skewed: Mean < Median < Mode.
*   **Variance ($\sigma^2$) / Standard Deviation ($\sigma$):** Measures data dispersion. $\sigma = \sqrt{\frac{1}{N}\sum (x_i - \mu)^2}$
*   **Z-Score Normalization:** $v' = \frac{v - \mu}{\sigma}$
*   **Decimal Scaling:** $v' = \frac{v}{10^j}$ (where $j$ is the smallest integer such that $\max(|v'|) < 1$)
*   **Jaccard Coefficient (Asymmetric Binary):** $J = \frac{M_{11}}{M_{01} + M_{10} + M_{11}}$ (Ignore 0-0 matches)
*   **Cosine Similarity:** $\cos(\theta) = \frac{A \cdot B}{||A|| ||B||}$
*   **Minkowski Distance ($L_p$ norm):** $d(i, j) = (|x_{i1} - x_{j1}|^p + ... + |x_{il} - x_{jl}|^p)^{1/p}$
    *   **Manhattan ($L_1$):** Sum of absolute differences.
    *   **Euclidean ($L_2$):** Straight line distance.
    *   **Supremum ($L_\infty$):** Maximum single-attribute difference.

## 2. Pattern Evaluation Formulas
*   **Support $(A \cup B)$**: The proportion of transactions containing $A$ and $B$.
*   **Support $(A)$**: The proportion of transactions containing $A$.
*   **Support $(B)$**: The proportion of transactions containing $B$.
*   **Confidence ($A \Rightarrow B$):** $P(B|A) = \frac{\text{sup}(A \cup B)}{\text{sup}(A)}$
*   **Lift:** $\frac{P(A \cup B)}{P(A)P(B)}$ (Range: $[0, \infty]$). *Sensitive to null transactions.*
*   **Chi-Square ($\chi^2$):** $\sum \frac{(\text{Observed} - \text{Expected})^2}{\text{Expected}}$ (Range: $[0, \infty]$). *Sensitive to null transactions.*
    *   **Expected Frequency ($E_{ij}$):** $E_{ij} = \frac{R_i \times C_j}{N}$
*   **AllConf:** $\frac{\text{sup}(A \cup B)}{\max\{\text{sup}(A), \text{sup}(B)\}}$ (Range: $[0, 1]$). *Null-invariant.*
*   **Coherence:** $\frac{\text{sup}(A \cup B)}{\text{sup}(A) + \text{sup}(B) - \text{sup}(A \cup B)}$ (Range: $[0, 1]$). *Null-invariant.*
*   **Cosine:** $\frac{\text{sup}(A \cup B)}{\sqrt{\text{sup}(A)\text{sup}(B)}}$ (Range: $[0, 1]$). *Null-invariant.*
*   **Kulczynski (Kulc):** $\frac{\text{sup}(A \cup B)}{2} (\frac{1}{\text{sup}(A)} + \frac{1}{\text{sup}(B)})$ (Range: $[0, 1]$). *Null-invariant.*
*   **MaxConf:** $\max \{ \frac{\text{sup}(A \cup B)}{\text{sup}(A)}, \frac{\text{sup}(A \cup B)}{\text{sup}(B)} \}$ (Range: $[0, 1]$). *Null-invariant.*

## 3. Algorithms & Concepts Quick Reference
*   **KDD Process:** Pre-processing (Cleaning, Integration, Reduction, Transformation) $\rightarrow$ Pattern Discovery (Mining) $\rightarrow$ Post-Processing (Evaluation, Visualization).
*   **PCA (Principal Component Analysis):** Unsupervised, linear dimensionality reduction. Finds orthogonal vectors (components) that capture maximum variance.
*   **Frequent Patterns:** 
    *   *Closed Pattern:* No superset has the *exact same* support.
    *   *Max Pattern:* No superset is frequent at all.
    *   *Size Relationship:* {All} $\ge$ {Closed} $\ge$ {Max}.
*   **Apriori:** Breadth-first. Uses **downward closure** (if an itemset is frequent, all subsets are frequent) to prune candidates.
*   **FP-Growth:** Depth-first. No candidate generation. Uses an FP-tree and recursive conditional databases.
* **FP-Tree Root Children:** 1) Build F-list (frequent items sorted descending by count, ties alphabetical), 2) Sort items *within each transaction* by the F-list, 3) The root's children are simply the **very first item** of each sorted transaction and their occurrence counts.
*   **SPADE:** Sequential mining using vertical data format (TID lists) and set intersections.
*   **PrefixSpan:** Sequential mining using pattern-growth. Uses **pseudo-projection** (pointers/offsets) to save memory.
*   **Graph Mining (Apriori-based - AGM, FSG):** Vertex/edge growing. If a size-$k$ graph is frequent, all subgraphs must be frequent.
*   **Graph Mining (Pattern-growth - gSpan):** DFS right-most path extension to prevent duplicate subgraph generation. CloseGraph directly mines closed graphs.
*   **Phrase Mining:** ToPMine (0 training data, agglomerative), SegPhrase (tiny training data), AutoPhrase (distant supervision, positive-only).

## 4. Constraint-Based Mining Rules
*   **Anti-Monotone:** If $S$ violates $c$, supersets violate $c$ (e.g., `sum(price) < 10`). Prunes pattern space.
*   **Monotone:** If $S$ satisfies $c$, supersets satisfy $c$ (e.g., `min(price) < 10`). 
*   **Data Anti-Monotone:** If transaction $T$ cannot satisfy $c$ for pattern $P$, it cannot satisfy $c$ for $P$'s supersets. Prunes data space.
*   **Succinct:** Can enforce by directly manipulating data (e.g., dropping all items $> \$50$).
*   **Convertible:** Becomes (anti-)monotone if items are sorted properly.
  
| Constraint | Anti-monotone | Monotone | Succinct | Convertible |
| :--- | :---: | :---: | :---: | :--- |
| $\min(S.A) \ge v$ | Yes | No | Yes | Strongly Yes |
| $\min(S.A) \le v$ | No | Yes | Yes | Strongly Yes |
| $\max(S.A) \ge v$ | No | Yes | Yes | Strongly Yes |
| $\max(S.A) \le v$ | Yes | No | Yes | Strongly Yes |
| $\text{count}(S) \le v$ | Yes | No | Weakly | Yes |
| $\text{count}(S) \ge v$ | No | Yes | Weakly | Yes |
| $\text{sum}(S.A) \le v \ (\forall i \in S, i.A \ge 0)$ | Yes | No | No | Yes |
| $\text{sum}(S.A) \ge v \ (\forall i \in S, i.A \ge 0)$ | No | Yes | No | Yes |
| $\text{sum}(S.A) \le v \ (v \ge 0, \forall i \in S, i.A \, \theta \, 0)$ | No | No | No | Yes (Anti-monotone) |
| $\text{sum}(S.A) \ge v \ (v \ge 0, \forall i \in S, i.A \, \theta \, 0)$ | No | No | No | Yes (Monotone) |
| $\text{sum}(S.A) \le v \ (v \le 0, \forall i \in S, i.A \, \theta \, 0)$ | No | No | No | Yes (Monotone) |
| $\text{sum}(S.A) \ge v \ (v \le 0, \forall i \in S, i.A \, \theta \, 0)$ | No | No | No | Yes (Anti-monotone) |
| $\text{range}(S.A) \le v$ | Yes | No | No | Strongly Yes |
| $\text{range}(S.A) \ge v$ | No | Yes | No | Strongly Yes |
| $\text{avg}(S.A) \, \theta \, v$ | No | No | No | Strongly Yes |
| $\text{median}(S.A) \, \theta \, v$ | No | No | No | Strongly Yes |
| $\text{var}(S.A) \ge v$ | No | No | No | Yes |
| $\text{var}(S.A) \le v$ | No | No | No | Yes |
| $\text{std}(S.A) \ge v$ | No | No | No | Yes |
| $\text{std}(S.A) \le v$ | No | No | No | Yes |
| $\text{var}^{N-1}(S.A) \, \theta \, v$ | No | No | No | Yes |
| $\text{md}(S.A) \ge v$ | No | No | No | Yes |
| $\text{md}(S.A) \le v$ | No | No | No | Yes |

## 5. Hand-Problem Reference & User Notes
*   **Apriori Candidate Generation:** How do we determine candidate $k$-itemsets given frequent $(k-1)$-itemsets? By combining frequent all pairs of 2-itemsets with the same prefix, e.g., (A,C) and (A,E), and then pruning those which contain a subset of 2 items which are not frequent we can arrive at the final 3-itemset candidate pool. For example, (A,C) and (A,E) combine to make the candidate (A,C,E), however if (C,E) was not given as a frequent 2-itemset then we know (A,C,E) cannot be frequent and it is pruned.
*   **PrefixSpan Projection:** How would the sequence `<abe(f)(bcde)(ab)cf>` appear in the projected database for `c`? In the c-projected database the postfix of the sequence would be `<(_de)(ab)cf>` because we only consider the subsequence prefixed with the first occurrence of c. Note that it does not become `<(b_de)(ab)cf>` because PrefixSpan only considers the items alphabetically after the given prefix in order to avoid duplicate counting.
*   **Sequential vs. Itemsets:** What is the difference between `ab` and `(ab)`? `ab` means `a` is bought, then `b` is bought (order matters). `(ab)` means `a` and `b` are bought at the same time (unordered internally). Gaps are allowed in subsequences: `(ab)dac` is a valid subsequence of `c(abd)edfa(cd)`.
*   **Graph Counting:** Can a subgraph appear more than once in a single graph in the database? Yes. Does each occurrence increase the support? No, no matter how many times the subgraph appears in a particular graph it will only increase the support of that subgraph by one.

***
