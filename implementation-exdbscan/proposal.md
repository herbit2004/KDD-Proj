# Reproducing ExDBSCAN Counterfactual Clustering Explanations

**COMP5331 project type:** Implementation

**Group number:** [Group number]

**Member 1:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 2:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 3:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 4:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 5:** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Member 6 (if applicable):** Student ID [ ]; name [ ]; research/FYP supervisor [name or none]; own research/FYP topic [topic or none]; difference from this project [explanation or not applicable].

**Declaration:** This project is conducted solely within COMP5331. It is not work for another course, a research project, or an FYP.

## Project description

### Motivation and objective

Density-based clustering is useful when clusters have irregular shapes and observations may be labeled as noise. DBSCAN makes these decisions through local density and reachability, but an assigned cluster label alone does not explain what would have to change for a particular point to receive a different assignment. Counterfactual explanations turn this into a concrete question: find a small feasible change to an observation that produces a specified alternative outcome. For DBSCAN, this is unusually subtle because the target assignment depends on density reachability rather than distance to a centroid. The paper explains the original clustering through a fixed core-point membership rule. Matthews and colleagues propose ExDBSCAN to reason about this setting. Our project will reproduce its central explanation-quality experiment on a manageable selection of data and analyze cases where validity, proximity, and diversity pull in different directions.

The main objective is to assess whether the released implementation produces valid and useful counterfactuals under a repeatable DBSCAN setup. We will first reproduce two-dimensional examples, where the original point, candidate counterfactual, cluster borders, and noise region can be inspected visually. We will then evaluate the method on the public Iris and Wine datasets using their numerical features. Rather than attempting the entire paper's benchmark collection, we will use a protocol with a fixed query set and explicit parameters, so that each reported explanation can be audited. The outcome will include successful explanations, unsuccessful attempts, and runtime, not only the best examples.

### Relation to prior methods

Ester and colleagues define DBSCAN's core points, border points, density reachability, and noise; those definitions determine the exact fixed-clustering membership test needed to score a counterfactual. BayCon provides a model-agnostic optimization-based counterfactual approach and appears among the comparative approaches discussed in the ExDBSCAN paper. Reading BayCon will help us distinguish a generic search procedure from ExDBSCAN's graph-aware core-point selection, although the main numerical baseline in our limited experiment will be ExDBSCAN Random. The KDD paper is the primary method source, while its separately released additional material provides pseudocode, non-actionable-feature cases, and detailed per-dataset tables. These readings jointly specify the algorithm, a valid evaluation rule, and the interpretation of proximity versus diversity.

### Paper and algorithm scope

The primary paper is Pernille Matthews and coauthors, “ExDBSCAN: Explaining DBSCAN with Counterfactual Reasoning,” KDD 2026; the authors published a separate arXiv document containing additional material. We will use the public author repository as the starting implementation, map its main script and helper modules to the procedure in the paper, and pin the repository revision used for our experiments. We will distinguish the original method from any small compatibility fixes required by current dependencies. The core experiment concerns a single query point and numerical feature changes within the ranges supported by the code. Handling many simultaneous point modifications or domain-specific feasibility constraints is outside the initial scope. This focuses the reproduction on the paper's principal counterfactual objective.

We will define a counterfactual's validity using the same evaluation convention as the paper: after the proposed change, the clustering outcome must meet the designated target or status condition. Before running comparative experiments, we will verify this definition against small synthetic examples. This step is essential because a candidate may look close to another cluster geometrically without satisfying DBSCAN's density-reachability condition. We will apply the paper's fixed-clustering assignment rule and will not rerun DBSCAN after generating a candidate; rerunning would answer a different question. If the paper and implementation support several target conditions, the report will specify which condition each result addresses.

### Reproduction checkpoints

The KDD paper defines membership of a proposed counterfactual against the original, fixed DBSCAN clustering: a candidate belongs to a target cluster when it lies within the epsilon-neighborhood of a core point in that cluster. We will implement this membership check as an independent evaluator and test it on a small configuration with known core, border, and noise points. Our principal baseline will be the paper's ExDBSCAN Random variant, which uses the same core-point construction but samples target-cluster core points uniformly. That comparison isolates the structured proximity-and-diversity selection. We will measure both Euclidean proximity and the paper's graph-distance-based diversity, and report the number of valid explanations per query. The public code provides the core method but its benchmark driver expects result and parameter files absent from the repository. The experiment therefore includes constructing a small, explicit driver around the released method before the paper-style comparison can run.

### Data and evaluation design

Our synthetic data will include separated blobs, a curved shape with noise, and a mixture of high- and low-density groups. These examples expose different counterfactual situations: a point near a boundary, a point deep inside a cluster, and a point initially labeled as noise. For the public-data stage, we will use UCI Iris and Wine, apply training-set-derived scaling, and document the selected features and any excluded variables. We will select DBSCAN `eps` and `min_samples` through a transparent preliminary grid based on obtaining a nontrivial clustering. Once parameters are fixed, all compared methods will receive the same clustering, query points, target conditions, and search budget.

We will measure validity rate, counterfactual distance from the original observation, diversity among multiple explanations for one query, and elapsed time per query. Distances will be computed in scaled feature space so that a high-range variable does not dominate by unit choice. We will compare ExDBSCAN with the paper's ExDBSCAN Random baseline under the same query set and explanation count. The random-core baseline specifically tests whether structured selection improves over the same valid core-based construction. We will report distances only for valid counterfactuals while separately reporting failure rates, preventing a method that fails frequently from appearing unusually close. For each dataset, we will use a fixed set of query points spanning clusters and noise, repeat stochastic components with several seeds, and retain all configuration files.

The principal plots will show example explanations on synthetic data and metric distributions across queries on public data. We will inspect difficult cases individually: points for which no valid alternative is found, explanations that cross a substantial low-density gap, or multiple returned counterfactuals that are nearly identical despite a diversity objective. A sensitivity analysis will change `eps` and `min_samples` within a small neighborhood of the chosen values, since DBSCAN results are parameter-dependent. This is not intended to search for the most favorable score; it tests whether the method's relative behavior is stable when the density definition changes moderately. We will state any dataset or parameter choices that make a target cluster unreachable under the allowed perturbation constraints.

### Deliverables and expected insight

We will validate the metric implementation independently on hand-designed point configurations. A candidate within the epsilon-neighborhood of a target-cluster core point, a candidate close to a border point but outside all target-cluster core neighborhoods, and a candidate remaining in noise will form unit-scale examples for the evaluator. We will save the fixed original cluster assignments and each candidate's target-membership result. The resulting diagnostic cases will guide interpretation of aggregate scores.

The project will deliver a runnable experiment script, pinned environment information, prepared data and query configurations, evaluation code, and regenerated figures and tables. The report will explain the algorithmic definition of a valid DBSCAN counterfactual and compare the released implementation with its stated objective. A successful reproduction should show where ExDBSCAN improves validity or proximity and where its additional reasoning costs runtime. If some public-data cases do not reproduce the paper's advantages, we will investigate cluster geometry, noise proportion, feature scaling, and target feasibility as possible explanations. The scope is deliberately small enough for CPU experimentation while preserving the distinctive scientific question of counterfactual explanation for density-based clustering.

## Papers to read

1. **Main paper:** Pernille Matthews, Lena Krieger, Tommaso Amico, Arthur Zimek, Thomas Seidl, and Ira Assent. “ExDBSCAN: Explaining DBSCAN with Counterfactual Reasoning.” *KDD*, 2026. DOI: [10.1145/3770855.3817692](https://doi.org/10.1145/3770855.3817692).
2. **Algorithm foundation:** Martin Ester, Hans-Peter Kriegel, Jörg Sander, and Xiaowei Xu. “A Density-Based Algorithm for Discovering Clusters in Large Spatial Databases with Noise.” *KDD*, pp. 226–231, 1996. [AAAI PDF](https://cdn.aaai.org/KDD/1996/KDD96-037.pdf).
3. **Counterfactual comparator:** Piotr Romashov, Martin Gjoreski, Kacper Sokol, Maria Vanina Martinez, and Marc Langheinrich. “BayCon: Model-agnostic Bayesian Counterfactual Generator.” *IJCAI*, pp. 740–746, 2022. [Proceedings paper](https://www.ijcai.org/proceedings/2022/104).
4. **Supporting document:** Matthews et al. “ExDBSCAN: Explaining DBSCAN with Counterfactual Reasoning — Additional Material.” arXiv:2605.30225, 2026. [PDF](https://arxiv.org/pdf/2605.30225).
