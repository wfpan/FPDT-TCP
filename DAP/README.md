## Data Aggregation Process for Plotting Boxplots and Statistical Analysis
We use *APFD* to illustrate the data aggregation process. The other metrics (i.e., *PTL* and $T_{eff}$) follow the same approach.

As mentioned in our work, for each version pair, we generate 100 mutated versions to simulate potential faults, resulting in 100 *APFD* values. To perform statistical hypothesis testing (*average ranking
of the Friedman test*, *Wilcoxon signed-rank test*, and *Cliff’s Delta effect size test*) across the entire dataset, it is necessary to determine the appropriate level of data aggregation.

We originally considered two strategies for aggregating these *APFD* values and finally selected **Strategy 1**. Specifically:

#### Strategy 1: Aggregation at the Version-Pair Level (Selected Approach)

**Methodology:**
For each version pair, we calculate the median of the 100 *APFD* values derived from the *100* mutated versions. This median value serves as the representative performance metric for that specific version pair. Subsequently, all such median values from all version pairs **in a specific system** are pooled together to form the final dataset for the system (for plotting boxplots), and all such median values from all version pairs **across all systems** are pooled together to form the final dataset for statistical analysis (*average ranking of the Friedman test*, *Wilcoxon signed-rank test*, and *Cliff’s Delta effect size test*). 

**Justification and Rationale:**

- **Robustness against Stochasticity:** Mutation testing inherently introduces randomness due to the stochastic nature of mutant generation and distribution. A single *APFD* value derived from one mutant set may be an outlier. By generating 100 mutated versions and taking the **median**, we obtain a robust estimate of the TCP technique's performance for that version pair. The **median** is preferred over the **mean** as it is less sensitive to extreme outliers and skewed distributions, which are common in *APFD* data.
- **Preservation of Statistical Power:** By retaining the median value for each version pair, we maintain a sufficiently large sample size for the statistical tests. The sample size corresponds to the total number of version pairs across all systems. This ensures that the *average ranking of the Friedman test*, *Wilcoxon signed-rank test*, and *Cliff’s Delta effect size test* have adequate statistical power to detect significant differences between the TCP techniques.
- **Representation of Variance:** This strategy preserves the variance across different version pairs. Since different version pairs involve different code structures and change histories, their inherent difficulty for fault detection varies. Treating each version pair as an independent observation allows the statistical tests to account for this variability, leading to more generalizable conclusions.

#### Strategy 2: Aggregation at the System Level (Rejected Approach)

**Methodology:**
This strategy involves a two-step aggregation. First, the median *APFD* is calculated for each version pair (as in **Strategy 1**). Second, these median values are further aggregated by calculating the median of all version pairs within a specific system. This results in a single representative *APFD* value per system. All such representative *APFD* values **across all systems** are pooled together to form the final dataset for statistical analysis (*average ranking of the Friedman test*, *Wilcoxon signed-rank test*, and *Cliff’s Delta effect size test*). 

**Critique and Limitations:**

- **Insufficient Sample Size:** The primary drawback of this strategy is the drastic reduction in sample size. Since our study involves only five systems, the sample size for the statistical tests would be $N=5$. In non-parametric testing, such a small sample size results in extremely low statistical power, making it nearly impossible to reject the null hypothesis even if significant differences exist (high probability of Type II error).
- **Loss of Information:** Aggregating data to the system level masks the variations between different version pairs within the same system. It assumes that a system's performance can be represented by a single scalar, ignoring the diverse characteristics of different versions. This **double aggregation** (median of medians) obscures the true distribution of the data and fails to reflect the technique's performance across the diverse spectrum of version pairs.

#### Our Final Choice (**Strategy 1**)

Based on the analysis above, **Strategy 1** is the scientifically rigorous choice. It effectively balances the need to eliminate the randomness of mutant generation (by using the median of 100 runs) with the need to maintain a sufficient sample size for valid statistical inference. Therefore, the *average ranking of the Friedman test*, *Wilcoxon signed-rank test*, and *Cliff’s Delta effect size test* are conducted on the dataset composed of the median *APFD* values of all individual version pairs.

#### A running example to illustrate our data aggregation process ( **Strategy 1**)
Here is a concrete example illustrating our data aggregation process. 

For simplicity, let's assume an experiment with 2 Systems, each containing 2 Version Pairs. For every version pair, we generate 100 Mutants, resulting in 100 *APFD* values per version pair.

**Aggregation Process** 

- Calculate the Mutant Median: For each version pair, calculate the median of the 100 *APFD* values.
- Direct Pooling: Pool all the median values from all version pairs across all systems directly into a single dataset for statistical testing.

**Data Flow Example**
System 1:
    Version Pair A: 100 APFD values -> Median = 0.75
    Version Pair B: 100 APFD values -> Median = 0.82
System 2:
    Version Pair C: 100 APFD values -> Median = 0.68
    Version Pair D: 100 APFD values -> Median = 0.79

**Final Dataset for Plotting Boxplots** 
System 1:
| version pairs | median |
| ------- | ------- |
| A | 0.75 |
| B | 0.82 |

System 2:
| version pairs | median |
| ------- | ------- |
| C | 0.68 |
| D | 0.79 |

**Final Dataset for Statistical Testing** 
(*average ranking of the Friedman test*, *Wilcoxon signed-rank test*, and *Cliff’s Delta effect size test*):

| version pairs | median |
| ------- | ------- |
| A | 0.75 |
| B | 0.82 |
| C | 0.68 |
| D | 0.79 |

Sample Size (*N*) = 4. These data points fully represent the independent performance of different version pairs. The sample size is sufficient for valid statistical testing.

