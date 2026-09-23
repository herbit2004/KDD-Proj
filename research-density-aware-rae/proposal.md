# Density-Stratified Neighbor Preservation for RAE

**COMP5331 project type:** Research  
**Group number:** [Group number]  
**Members (name; student ID; research/FYP supervisor and topic, if applicable):** [Member 1]; [Member 2]; [Member 3]; [Member 4]; [Member 5]; [Member 6, if applicable]  
**Relationship to members' research/FYP:** [For each member, state whether a research/FYP topic exists and, if so, explain how this course project differs.]  
**Course-project declaration:** This work is undertaken for COMP5331 and will be reported as a course project.

## Project description

### Background and research question

High-dimensional vector search depends on retaining meaningful nearest neighbors after data are compressed. A lower-dimensional representation can greatly reduce storage and query cost, but an attractive average neighbor recall does not guarantee uniform behavior across the space. A method may preserve neighborhoods in dense regions that dominate the sample and lose neighbors for points in sparse regions or near transitions between clusters. RAE, proposed by Zhang and Zhao, is a neural-network dimensionality reduction method designed for nearest-neighbor preservation in vector search. UMAP is a useful reference because its construction emphasizes local neighborhoods through a weighted graph. These methods motivate a more granular evaluation than one overall recall number.

We ask whether RAE's neighborhood preservation varies systematically with local density, and whether a modest density-aware adjustment to its training sampler or loss weighting reduces the worst-density-group gap. Our primary hypothesis is that the unmodified model has lower recall for at least one density stratum on data with strongly unequal cluster densities, even when aggregate recall is good. The secondary hypothesis is that a training-only density-aware sampling rule can improve the worst stratum without an excessive loss of average recall or training speed. The project is not premised on those hypotheses being true. Equal performance across strata, or a failed intervention, would still clarify when density-aware complexity is unnecessary or counterproductive.

### Prior work and scope of contribution

The main paper is Han Zhang and Dongfang Zhao, “RAE: A Neural Network Dimensionality Reduction Method for Nearest Neighbors Preservation in Vector Search.” The downloaded arXiv version is identified as an ICLR 2026 paper, while the course suggested-topic list and author repository refer to KDD 2026; we will use the course-provided version for the final bibliographic entry after checking the difference. Our related reading is McInnes, Healy, and Melville, “UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction.” We will compare RAE with PCA and UMAP to establish whether any observed density effect is distinctive to RAE or a general cost of reduction.

Density-based sampling and weighting are broad existing ideas, so our contribution is specifically a density-stratified diagnosis and a controlled integration into RAE's neighbor-preservation training setting. We will implement one simple rule rather than introducing a suite of loosely related heuristics. For every training vector, we will estimate local density from its distance to the kth neighbor in the original space, using a neighbor index built only from training data. Vectors will be divided into density quantiles. The proposed change will upsample underrepresented or low-recall density strata according to a prespecified formula, while keeping batch size and total optimizer steps unchanged. We will avoid using test recall to choose the sampling weights. A validation fold may be used to select among a small predefined set of strengths.

### Data and experimental design

We will first construct Gaussian-mixture vectors with controllable dense and sparse components, including a transition region. This allows us to know which differences arise from density rather than semantic class labels. We will then use one moderate-sized public vector dataset, such as image-feature vectors or a documented UCI dataset transformed into vectors, subject to access and computational feasibility. The intended upper bound is tens of thousands of vectors, not a billion-scale search corpus. For each dataset we will document original dimensionality, normalization, sample count, split, density distribution, and the original-space neighbor reference. We will fix one or two target dimensions that produce a meaningful compression, then use the same dimensions across methods.

The core comparison will include PCA, UMAP, unmodified RAE, and density-aware RAE. All learned methods will receive the same training partition. For RAE and its modification, we will fix the architecture, embedding dimension, optimizer, batch size, number of steps, and validation procedure; sampling weights will be the principal changing factor. We will run multiple seeds when runtime permits. Exact nearest neighbors in the original space will be computed on the held-out query/reference design, and reduced-space neighbors will be compared to them. The main metrics are recall@10 and recall@50 overall and by original-space density quantile. We will also report worst-quantile recall, mean gap between the best and worst quantile, training time, embedding time, and approximate query time with the same neighbor-search library. A method that helps a sparse group but badly harms overall recall will not be treated as a simple success.

We will test whether density quantiles are stable under reasonable changes to the density neighborhood size, since a result that depends on one arbitrary k would be fragile. We will also compare per-point recall with distance-to-neighbor scatter plots rather than relying entirely on group averages. An ablation will use uniform sampling with the same optimizer budget to distinguish an effect of density weighting from longer or differently batched training. If the algorithm already contains an implicit balancing mechanism, the density-aware change may yield little benefit; examining sampled-neighbor frequencies and loss contributions can help interpret that outcome.

### Deliverables and expected outcomes

The deliverables are a reproducible small-data RAE training setup, data and density-stratification scripts, a minimal sampling modification, baseline comparisons, and figures showing overall and group-level retrieval behavior. We will first establish one end-to-end run of the public implementation before changing the training code. The report will state any deviations from the paper's original dataset and runtime setting, especially if only a subset is feasible. We expect the synthetic mixtures to expose conditions where aggregate recall hides local failures; on real data the finding may be weaker, absent, or reversed. The value of the project lies in locating that boundary and quantifying the trade-off of a targeted intervention, rather than claiming a universal improvement in vector search.


As an additional diagnostic, we will measure how often a point's original-space neighbors belong to a different density stratum. Boundary points of this kind may be difficult for a reason other than low sample density alone. We will therefore show results both with all points and with boundary points marked separately. This analysis can tell us whether the proposed sampler addresses true sparse-region failures or merely shifts errors toward transitions between groups. It also limits the risk of presenting a density-group mean as a complete account of local geometry.

## Papers to read

1. Han Zhang and Dongfang Zhao. “RAE: A Neural Network Dimensionality Reduction Method for Nearest Neighbors Preservation in Vector Search.” 2026. [Paper](https://arxiv.org/abs/2509.25839) · [Code](https://github.com/explorerZH/RAE-KDD2026).
2. Leland McInnes, John Healy, and James Melville. “UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction.” arXiv:1802.03426. [Paper](https://arxiv.org/abs/1802.03426).
