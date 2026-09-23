# Adaptive Fairness Weighting for Community Detection on Group-Imbalanced Graphs

**COMP5331 project type:** Research

**Group number:** [Group number]

**Member 1:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 2:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 3:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 4:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 5:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 6 (if applicable):** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Declaration:** This project is conducted solely within COMP5331. It is not work for another course, a research project, or an FYP.

## Project description

### Background and research question

Community detection groups nodes according to network structure. Modularity rewards partitions with more within-community connections than a degree-preserving reference model predicts. A high-modularity partition, however, can reproduce or intensify an uneven distribution of demographic or other sensitive groups across communities. The problem becomes especially difficult when group sizes are unequal: a fairness penalty calibrated for a balanced population can over-correct a small group or leave it concentrated in a few communities. Gkartzios, Pitoura, and Tsaparas combine modularity and fairness in deep community detection, while their group-modularity work offers another way to define the objective. More recent MOUFLON work explicitly studies multi-group modularity-based fairness. Together these papers show that the general quality–fairness trade-off is established, but they also motivate a focused question about how an operating point should change when the graph's group proportions and homophily change.

Our question is whether a single fixed fairness weight is reliable across graphs with different group imbalance and within-group connectivity, and whether a simple rule based on observable training-graph statistics can select a more suitable weight. We hypothesize that fixed weights can produce noticeably different quality–fairness trade-offs as minority-group proportion or homophily changes. We further hypothesize that a prespecified adaptive rule can reduce the variability of fairness outcomes without sacrificing a large amount of modularity. These hypotheses can fail: one weight may be robust across the tested range, or adaptive selection may add complexity without improving the Pareto frontier. We will report those outcomes rather than treating a favorable selected graph as sufficient evidence.

### Relationship to previous work

The main reference is “Modularity-Fair Deep Community Detection,” ICDM 2025. We will use its public implementation to establish fixed-weight baselines and to understand the deep and spectral variants of the method. “Fair Network Communities through Group Modularity,” WWW 2025, provides a related non-deep objective and comparison. “MOUFLON: Multi-group Modularity-based Fairness-aware Community Detection,” 2026, is a particularly close recent paper. We will read its objective, tuning procedure, and treatment of unequal groups before defining our final comparison. The proposed study examines how group imbalance and homophily jointly affect weight selection under controlled graph generation. It adds one transparent selection rule whose inputs and fitting procedure are specified before test-graph evaluation.

Our adaptive rule will use only statistics available from the graph and sensitive-group labels before evaluating a partition, such as group proportions and an estimate of within-group edge concentration. On validation graphs, we will label the grid weight that minimizes group-representation disparity while retaining at least 95% of the zero-fairness-weight modularity. A two-input regression on minority-group share and within-group edge concentration will predict `log(1 + weight)`; its inverse prediction will be clipped to the evaluated grid. Test graphs will use unseen generator seeds and held-out parameter combinations. This separation prevents the adaptive method from choosing the best weight after seeing test modularity or fairness. We will retain a fixed-weight grid as a full comparison curve and treat a graph-specific hindsight optimum only as an upper bound, not as a deployable baseline.

### Relationship among graph objectives

DMoN supplies the modularity-based deep graph clustering foundation underlying the main paper's model family. Gkartzios and colleagues' group-modularity paper gives a non-deep fairness-aware objective; their later ICDM paper integrates modularity and fairness into deep community detection. MOUFLON addresses multi-group modularity-based fairness; the present experiment isolates fixed-weight stability and graph-dependent weight selection for two groups. We will compare the papers' fairness definitions and optimization settings before fixing our own two-group metric. The resulting research question concerns the stability of one selected weight across graph regimes, measured against a full fixed-weight curve. This positions the adaptive rule as a testable operating-point procedure rather than a replacement for the established community-detection objectives.

### Method compatibility and data protocol

The main-paper implementation reads a binary sensitive-group field and its fairness calculations assume groups encoded as 0 and 1. Our primary study will vary the proportion of two groups. We will generate graphs with fixed planted-community proportions while independently changing the sensitive-group ratio and the correlation between group membership and communities. This separation is necessary to tell whether an apparent fairness gain reflects weight selection or simply an easier graph. For the real-data stage, the Deezer network has a binary label inferred from users' names; it is a proxy with uncertain individual accuracy, so we will interpret results as balance with respect to that published label, not as a measured social outcome. We will retain the original graph's node and edge counts, record any induced-subgraph sampling, and avoid choosing the adaptive rule from real-graph test outcomes.

### Data and experimental protocol

The primary data will be attributed stochastic-block-model graphs. We will vary the two-group ratio, the number of communities, within- versus across-community connection probabilities, and the correlation between sensitive groups and planted communities. These factors can be changed independently enough to reveal when a fairness objective conflicts sharply with structural quality. Graphs will initially contain roughly 500 to 2,000 nodes, which permits repeated runs on accessible hardware. We will also use one public real graph with group-related node attributes, such as a documented Deezer social-network subset, if its attribute semantics and use conditions fit the fairness question. We will describe the published Deezer label as a name-inferred group proxy and interpret the analysis in those terms.

For each graph and method, we will report modularity and a community-level representation disparity: the community-size-weighted mean of `|p_c - p|`, where `p_c` is the minority-group share in community `c` and `p` is its share in the full graph. We will also show each community's group counts. On synthetic graphs, where planted community labels are known, we will additionally report adjusted Rand index and normalized mutual information. A real network generally lacks a trusted community ground truth, so we will not use those supervised metrics there. The numerical comparison will include the main paper's fixed-weight method across a predefined weight grid, its zero-fairness-weight modularity setting, and the adaptive rule applied to the same implementation. MOUFLON provides a related multi-group objective for interpreting these two-group results; its published results will be discussed separately from the numerical comparisons.

We will show quality–fairness Pareto curves across the fixed-weight grid, then locate the adaptive rule's outcome on each curve. A scalar summary alone can hide a poor trade-off, so both metrics and group-specific distributions will remain visible. Results will be averaged across several graph seeds, with variability shown. We will inspect failure cases in which the proposed rule improves parity but collapses communities, or preserves modularity while leaving a minority group concentrated. To distinguish selection quality from model optimization noise, we will keep random seeds, training budgets, and initialization policy aligned where possible. Sensitivity to graph size and the minority-group share will be a secondary analysis after the two main factors have been studied.

### Deliverables and expected outcomes

The project will provide graph-generation code, configurations, fixed-weight and adaptive-weight experiments, evaluation scripts, and figures that make the trade-offs visible. The report will state exactly how fairness and quality were measured and how the validation graphs differed from test graphs. We will keep the group counts within every output community, so that an aggregate fairness score does not hide severe underrepresentation in one community. We expect a fixed weight to become less reliable under some imbalance/homophily combinations, but the experiment may instead find a wide stable range. In either case, the result will help explain when adaptive tuning is justified. The workload is bounded by small graphs and a compact parameter grid, while the literature comparison keeps the claim aligned with existing fair-community methods.


## Papers to read

1. **Main paper:** Christos Gkartzios, Evaggelia Pitoura, and Panayiotis Tsaparas. “Modularity-Fair Deep Community Detection.” *IEEE ICDM*, 2025. DOI: [10.1109/ICDM65498.2025.00036](https://doi.org/10.1109/ICDM65498.2025.00036).
2. **Deep modularity foundation:** Anton Tsitsulin, John Palowitch, Bryan Perozzi, and Emmanuel Müller. “Graph Clustering with Graph Neural Networks.” *Journal of Machine Learning Research* 24(127):1–21, 2023. [Paper](https://www.jmlr.org/papers/v24/20-998.html).
3. **Fair modularity objective:** Christos Gkartzios, Evaggelia Pitoura, and Panayiotis Tsaparas. “Fair Network Communities through Group Modularity.” *The Web Conference*, 2025. [Author PDF](https://www.cse.uoi.gr/~tsap/publications/Gkartzios-WWW2025.pdf).
4. **Closest multi-group work:** Georgios Panayiotou, Anand Mathew Muthukulam Simon, Matteo Magnani, and Ece Calikus. “MOUFLON: Multi-group Modularity-based Fairness-aware Community Detection.” *Data Mining and Knowledge Discovery* 40, article 92, 2026. [Publisher article](https://link.springer.com/article/10.1007/s10618-026-01260-5).
