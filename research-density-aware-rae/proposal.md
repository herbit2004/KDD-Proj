# Density-Stratified Neighbor Preservation for RAE

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

High-dimensional vector search depends on retaining meaningful nearest neighbors after data are compressed. A lower-dimensional representation can greatly reduce storage and query cost, but an attractive average neighbor recall does not guarantee uniform behavior across the space. A method may preserve neighborhoods in dense regions that dominate the sample and lose neighbors for points in sparse regions or near transitions between clusters. RAE, proposed by Zhang and Zhao, is a neural-network dimensionality reduction method designed for nearest-neighbor preservation in vector search. UMAP is a useful reference because its construction emphasizes local neighborhoods through a weighted graph. These methods motivate a more granular evaluation than one overall recall number.

We ask whether RAE's neighborhood preservation varies systematically with local density, and whether a density-aware adjustment to its training sampler reduces the worst-density-group gap. Our primary hypothesis is that the unmodified model has lower recall for at least one density stratum on data with strongly unequal cluster densities, even when aggregate recall is good. The secondary hypothesis is that a training-only density-aware sampling rule can improve the worst stratum without an excessive loss of average recall or training speed. We will report both aggregate and stratum-level results, including cases where the adjustment leaves recall unchanged or reduces it.

### Prior work and scope of contribution

The main paper is Han Zhang and Dongfang Zhao, “RAE: A Neural Network Dimensionality Reduction Method for Nearest Neighbors Preservation in Vector Search,” KDD 2026. Our related reading is McInnes, Healy, and Melville, “UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction.” We will examine how the papers construct, optimize, and evaluate neighborhoods before selecting the density estimator for our intervention. This will keep the stratified analysis aligned with the retrieval task and clarify whether the observed differences come from a model objective, a sampling decision, or the underlying neighbor graph. We will compare RAE with PCA and UMAP to establish whether any observed density effect is distinctive to RAE or a general cost of reduction.

The study combines density-stratified retrieval evaluation with one sampling change to RAE's neighbor-preservation training. The encoder architecture and loss remain fixed. We will construct a neighbor index from training vectors only and form density quantiles using each vector's original-space 20th-neighbor distance. Let `d_i` be its distance to the 20th original-space training neighbor. We will sample with probability proportional to `clamp(d_i / median(d), 0.5, 2)^alpha`, where `alpha` is chosen from {0, 0.5, 1} on a validation set. This increases exposure to locally sparse points. We will select the value with the highest worst-stratum validation recall among those retaining at least 95% of the uniform-sampling model's overall validation recall; `alpha=0` is always an eligible fallback. Test recall remains held out. Batch size and optimizer steps remain unchanged.

### Closest methodological context

UMAP builds a neighborhood graph using local scaling and provides a widely used dimensionality-reduction reference, but its visual-embedding objective is different from retrieval recall. Zelnik-Manor and Perona demonstrate why a single global scale can be unsuitable when clusters have different local densities; their self-tuning spectral method motivates our density stratification without itself solving the RAE training problem. Fu and Zhao's QPAD is a recent retrieval-oriented reduction method that explicitly targets nearest-neighbor structure. We will read QPAD to delimit what is already known about retrieval-aware objectives and to define a retrieval-oriented comparison point in the literature. The proposed intervention is narrower: it tests whether a fixed RAE model's training exposure can be redistributed across density strata while its architecture and objective remain unchanged.

### Implementation and data interface

The author implementation includes separate training and baseline entry points, but its data loader expects specific precomputed embedding files for CelebA, IMDb, Tiny ImageNet, Flickr30k, or SIFT1B. Our synthetic mixtures and small public vectors will therefore require a documented loader adapter that produces the same tensor and split format before any training claim can be made. We will first verify the published training and retrieval logic on a tiny synthetic matrix, then use a manageable subset of one documented public vector source. Density thresholds will be computed on the training vectors and frozen before fitting either RAE variant; held-out query density will be measured against the same training reference set. We will save query/reference IDs so that original-space and reduced-space recall are evaluated against exactly the same neighbor task. The density-aware intervention will change sampling weights only; its optimizer, encoder, target dimension, and number of update steps will match the unchanged model.

### Data and experimental design

We will first construct Gaussian-mixture vectors with controllable dense and sparse components, including a transition region. This allows us to know which differences arise from density rather than semantic class labels. We will then use the UCI Optical Recognition of Handwritten Digits dataset (5,620 observations, 64 numerical features) as a public real-vector case. Its published train/test files define the initial reference and held-out query sets. For each dataset we will document original dimensionality, normalization, sample count, split, density distribution, and the original-space neighbor reference. We will fix one or two target dimensions that produce a meaningful compression, then use the same dimensions across methods.

The core comparison will include PCA, UMAP, unmodified RAE, and density-aware RAE. All learned methods will receive the same training partition. For RAE and its modification, we will fix the architecture, embedding dimension, optimizer, batch size, number of steps, and validation procedure; sampling weights will be the principal changing factor. We will use three random seeds for each learned method. Exact nearest neighbors in the original space will be computed on the held-out query/reference design, and reduced-space neighbors will be compared to them. The main metrics are recall@10 and recall@50 overall and by original-space density quantile. We will also report worst-quantile recall, mean gap between the best and worst quantile, training time, embedding time, and approximate query time with the same neighbor-search library. A method that helps a sparse group but badly harms overall recall will not be treated as a simple success.

We will test whether density quantiles are stable under reasonable changes to the density neighborhood size, since a result that depends on one arbitrary k would be fragile. We will also compare per-point recall with distance-to-neighbor scatter plots rather than relying entirely on group averages. An ablation will use uniform sampling with the same optimizer budget to distinguish an effect of density weighting from longer or differently batched training. If the algorithm already contains an implicit balancing mechanism, the density-aware change may yield little benefit; examining sampled-neighbor frequencies and loss contributions can help interpret that outcome.

We will also measure how often a point's original-space neighbors belong to a different density stratum. Boundary points of this kind may be difficult for a reason other than low sample density alone. We will therefore show results both with all points and with boundary points marked separately. This analysis can tell us whether the proposed sampler addresses true sparse-region failures or merely shifts errors toward transitions between groups. It also limits the risk of presenting a density-group mean as a complete account of local geometry.

### Deliverables and expected outcomes

The deliverables are a reproducible small-data RAE training setup, data and density-stratification scripts, a minimal sampling modification, baseline comparisons, and figures showing overall and group-level retrieval behavior. We will first establish one end-to-end run of the public implementation before changing the training code. The report will state any deviations from the paper's original dataset and runtime setting, especially if only a subset is feasible. We expect the synthetic mixtures to expose conditions where aggregate recall hides local failures; on real data the finding may be weaker, absent, or reversed. The main result will identify the density strata where any gain occurs and quantify its cost in aggregate recall and runtime.

## Papers to read

1. **Main paper:** Han Zhang and Dongfang Zhao. “RAE: A Neural Network Dimensionality Reduction Method for Nearest Neighbors Preservation in Vector Search.” *KDD*, 2026; available manuscript carries an earlier ICLR header. [arXiv paper](https://arxiv.org/abs/2509.25839).
2. **Neighborhood baseline:** Leland McInnes, John Healy, and James Melville. “UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction.” arXiv:1802.03426, 2018. [Preprint](https://arxiv.org/abs/1802.03426).
3. **Local-scale foundation:** Lihi Zelnik-Manor and Pietro Perona. “Self-Tuning Spectral Clustering.” *Advances in Neural Information Processing Systems* 17, 2004. [Proceedings PDF](https://proceedings.neurips.cc/paper/2004/file/40173ea48d9567f1f393b20c855bb40b-Paper.pdf).
4. **Retrieval-oriented neighboring work:** Jiuzhou Fu and Dongfang Zhao. “QPAD: Quantile-Preserving Approximate Dimension Reduction for Nearest Neighbors Preservation in High-Dimensional Vector Search.” arXiv:2504.16335, 2025. [Preprint](https://arxiv.org/abs/2504.16335).
