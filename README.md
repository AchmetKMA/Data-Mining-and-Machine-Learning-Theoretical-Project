# Optimization of Knowledge Mining with Constraints, Efficiency, and Multiple Thresholds

An academic research project focusing on advancing Association Rules Mining (ARM) and High-Utility Itemset Mining (HUIM). This work evaluates algorithms that incorporate multiple user constraints, multiple minimum supports (MMS) to handle the rare item problem, high-efficiency itemset mining (HEIM), and dynamic multiple utility thresholds (MHUI-MUT).

Developed for the Data Mining and Learning Algorithms course at the Department of Computer Engineering & Informatics (CEID), University of Patras (2025).

## Table of Contents

- [Overview](#overview)
- [Research Pillars](#research-pillars)
- [Algorithms & Theoretical Models](#algorithms--theoretical-models)
- [Experimental Evaluation](#experimental-evaluation)
- [Future Work & Scalability](#future-work--scalability)
- [References](#references)

## Overview

Traditional Association Rules Mining (e.g., standard Apriori or FP-Growth) relies solely on item frequency, treating all items uniformly and ignoring qualitative characteristics like cost, profit, or strategic value. This project analyzes four core paradigms that transition knowledge discovery toward real-world application demands:

1. **Constrained ARM:** Utilizing simultaneous monotone and anti-monotone user constraints to prune search space and reduce uninteresting rules.
2. **Associative Classification with Multiple Minimum Supports:** Applying individual Minimum Item Support (MIS) thresholds to address the rare item problem in Classification Based on Association (CBA).
3. **High-Efficiency Itemset Mining (HEIM):** Shift from total profit to an efficiency ratio (Return on Investment: profit / investment cost).
4. **Multiple Utility Thresholds (MHUI-MUT):** Using differentiated per-item utility thresholds combined with multi-core parallel processing (PMHUI-MUT).

## Research Pillars

### 1. Dual-Constraint Optimization
- **Anti-Monotone Constraints:** If an itemset violates the constraint (e.g., `max(cost) <= min(price)`), none of its supersets will satisfy it.
- **Monotone Constraints:** If an itemset satisfies the constraint (e.g., `total(price) >= 100`), all its supersets will also satisfy it.
- **Pruning Strategy:** Utilizes four specialized structural lemmas within conditional FP-trees to skip invalid branches prior to candidate generation.

### 2. Multi-Support Associative Classifier (MMSCBA)
- **Problem Solved:** High global support thresholds discard rare but highly predictive class items (e.g., in medical diagnoses).
- **Strategy:** Each item $i_k$ is assigned a custom $MIS(i_k)$. A rule item $X \rightarrow c$ is generated if its support meets $\max_{x \in X}(MIS(x))$.
- **Evaluation Metrics:** Evaluated against rules using Maximum Likelihood, Laplace smoothing, custom Scoring functions, and Chi-Square ($\chi^2$) tests.

### 3. High-Efficiency Itemset Mining (MHEI)
- **Efficiency Metric:** Defined as $ef(X) = \frac{profit(X)}{investment(X)}$.
- **Search Space Reduction:** Introduces upper bounds—such as Subtree Efficiency (SE) and Local Efficiency (LE), alongside their Stricter variants (SSEF / SLEF)—to prune unpromising tree nodes without full database scans.

### 4. Dynamic Utility Thresholds & Parallelization (MHUI-MUT / PMHUI-MUT)
- **Dynamic Thresholding:** Uses $MU(i_k) = \max(\Delta \cdot p(i_k), GLMU)$ to tailor minimum utility requirements to individual item profitability profiles.
- **Extension Minimum Utility (EMU):** A tight upper bound designed to aggressively filter out unpromising candidate extensions.
- **Parallel Processing:** PMHUI-MUT partitions the prefix tree search space across multi-core architectures using task-decomposition strategies.

## Algorithms & Theoretical Models

| Paradigm | Target Challenge | Core Algorithm / Framework | Key Structural Mechanics |
| --- | --- | --- | --- |
| **Constrained ARM** | Rule explosion & low relevance | FP-Growth + Monotone/Anti-Monotone Pruning | Conditional FP-Trees, 4 Structural Lemmas |
| **Associative Classification** | Rare item exclusion in classification | MMSCBA | Per-item MIS, Rule Ranking (Laplace/$\chi^2$/Likelihood) |
| **High-Efficiency Mining** | Ignoring ROI/investment costs | MHEI | Set Enumeration Tree, Transaction Merging, SLEF/SSEF Upper Bounds |
| **Multiple Utility Thresholds** | Uniform utility threshold unrealism | MHUI-MUT / PMHUI-MUT | GLMU Dynamic Thresholds, EMU Bound, Thread-Pool Task Splitting |

## Experimental Evaluation

The studied algorithms were evaluated across multiple synthetic and real-world benchmark datasets (including UCI Repository datasets such as Breast Cancer Wisconsin, Balance Scale, Mushroom, Car, and SPMF datasets like Accidents, Chess, Connect, Kosarak, Retail):

- **Execution Speed:**
  - Double-constraint FP-Growth demonstrated up to $1.5\times-2\times$ speedups over standard FP-Growth+ on datasets up to 100K transactions.
  - MHUI-MUT achieved up to $3000\times$ faster runtime compared to single-threshold MHUI implementations on dense datasets due to transaction projection and merging.
  - PMHUI-MUT multi-threading yielded up to $3.3\times$ speedup on 4 cores and $5.5\times$ speedup on 8 cores.

- **Memory Overhead & Candidates:**
  - MHEI consistently maintained lower memory consumption compared to HEPMiner across varying efficiency thresholds ($\delta$).
  - Dynamic EMU upper bounds in MHUI-MUT reduced generated candidate itemsets by up to an order of magnitude.

- **Classification Accuracy:**
  - MMSCBA demonstrated superior classification accuracy on imbalanced datasets (e.g., Breast Cancer Wisconsin) compared to traditional single-support CBA, C4.5, RIPPER, and PART, while remaining fully interpretable compared to black-box models (SVM, ANN).

## Future Work & Scalability

Analysis of post-2020 literature highlights several strategic directions for expanding these techniques:

1. **Big Data & Distributed Systems:** Scaling associative classification algorithms via distributed frameworks like Apache Spark (e.g., CAP-growth and DAC architectures) to process multi-terabyte datasets.
2. **Top-K High-Efficiency Mining:** Eliminating user-specified efficiency thresholds ($\delta$) by adopting Top-K utility-list strategies (PE, NU, RIU, EUCS).
3. **Dynamic Profit Databases:** Extending utility mining algorithms (e.g., MCH-Miner) to handle real-time item price fluctuations, dynamic discounts, and streaming transactions.

## References

1. R. T. Ng et al., *"Exploratory mining and pruning optimizations of constrained association rules,"* SIGMOD, 1998.
2. B. Liu, W. Hsu, Y. Ma, *"Mining association rules with multiple minimum supports,"* KDD, 1999.
3. S. Zida et al., *"EFIM: a fast and memory efficient algorithm for high-utility itemset mining,"* Knowl. Inf. Syst., 2017.
4. J. Zhang et al., *"HEPMiner: Efficiently mining high-efficiency patterns in transactional databases,"* Inf. Sci., 2023.
5. S. Krishnamoorthy, *"Efficient mining of high utility itemsets with multiple minimum utility thresholds,"* Eng. Appl. Artif. Intell., 2018.
6. L. Venturini, E. Baralis, P. Garza, *"Scaling associative classification for very large datasets,"* ICDMW, 2016.
7. R. Kumar, K. Singh, *"Top-k high utility itemset mining: current status and future directions,"* Knowl. Eng. Rev., 2024.
8. B. Vo et al., *"A multi-core approach to efficiently mining high-utility itemsets in dynamic profit databases,"* IEEE Access, 2020.
