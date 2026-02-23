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
*   **Support:** $P(A \cup B)$ (Percentage of transactions containing both).
*   **Confidence ($A \Rightarrow B$):** $P(B|A) = \frac{\text{Support}(A \cup B)}{\text{Support}(A)}$
*   **Lift:** $\frac{P(A \cup B)}{P(A)P(B)}$ ( >1 positive correlation, <1 negative correlation, =1 independent). *Sensitive to null transactions.*
*   **Chi-Square ($\chi^2$):** $\sum \frac{(\text{Observed} - \text{Expected})^2}{\text{Expected}}$. *Sensitive to null transactions.*
*   **Kulczynski Measure:** $\frac{1}{2} (P(A|B) + P(B|A))$. *Null-invariant. Range [0,1].*

## 3. Algorithms & Concepts Quick Reference
*   **KDD Process:** Pre-processing (Cleaning, Integration, Reduction, Transformation) $\rightarrow$ Pattern Discovery (Mining) $\rightarrow$ Post-Processing (Evaluation, Visualization).
*   **PCA (Principal Component Analysis):** Unsupervised, linear dimensionality reduction. Finds orthogonal vectors (components) that capture maximum variance.
*   **Frequent Patterns:** 
    *   *Closed Pattern:* No superset has the *exact same* support.
    *   *Max Pattern:* No superset is frequent at all.
    *   *Size Relationship:* {All} $\ge$ {Closed} $\ge$ {Max}.
*   **Apriori:** Breadth-first. Uses **downward closure** (if an itemset is frequent, all subsets are frequent) to prune candidates.
*   **FP-Growth:** Depth-first. No candidate generation. Uses an FP-tree and recursive conditional databases.
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

## 5. Hand-Problem Reference & User Notes
*   **Apriori Candidate Generation:** How do we determine candidate $k$-itemsets given frequent $(k-1)$-itemsets? By combining frequent all pairs of 2-itemsets with the same prefix, e.g., (A,C) and (A,E), and then pruning those which contain a subset of 2 items which are not frequent we can arrive at the final 3-itemset candidate pool. For example, (A,C) and (A,E) combine to make the candidate (A,C,E), however if (C,E) was not given as a frequent 2-itemset then we know (A,C,E) cannot be frequent and it is pruned.
*   **PrefixSpan Projection:** How would the sequence `<abe(f)(bcde)(ab)cf>` appear in the projected database for `c`? In the c-projected database the postfix of the sequence would be `<(_de)(ab)cf>` because we only consider the subsequence prefixed with the first occurrence of c. Note that it does not become `<(b_de)(ab)cf>` because PrefixSpan only considers the items alphabetically after the given prefix in order to avoid duplicate counting.
*   **Sequential vs. Itemsets:** What is the difference between `ab` and `(ab)`? `ab` means `a` is bought, then `b` is bought (order matters). `(ab)` means `a` and `b` are bought at the same time (unordered internally). Gaps are allowed in subsequences: `(ab)dac` is a valid subsequence of `c(abd)edfa(cd)`.
*   **Graph Counting:** Can a subgraph appear more than once in a single graph in the database? Yes. Does each occurrence increase the support? No, no matter how many times the subgraph appears in a particular graph it will only increase the support of that subgraph by one.

***
