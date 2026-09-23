# Revisiting OVO and OVR under Multiclass Imbalance

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

Multiclass classification is often implemented by combining binary decisions. One-versus-one (OVO) trains a classifier for each pair of classes, while one-versus-rest (OVR) trains one classifier per class against all other classes. Both are standard choices, and a comparison based only on overall accuracy can suggest that they behave similarly. Yet when class frequencies differ, the errors that matter for a rare class contribute little to accuracy. Chen and Lin's 2025 study revisits this comparison and asks whether familiar conclusions still hold when performance is evaluated with measures sensitive to minority classes. Our project will implement a focused reproduction of that empirical question. We will compare OVO and OVR under controlled changes in class imbalance while holding the data split, classifier family, and tuning budget constant.

The central research question for this implementation is whether the difference between OVO and OVR becomes larger or changes direction as the training class distribution becomes less balanced. We will examine macro-averaged F1, balanced accuracy, geometric-mean recall, and class-wise recall alongside ordinary accuracy. A second question is whether apparent differences are stable across datasets and random seeds or depend on a particular split. These questions make the implementation more informative than simply obtaining one table of final scores. Our goal is to reproduce the paper's qualitative comparison on a limited, explicitly documented set of public datasets, and to identify which experimental choices are necessary to make the comparison fair.

### Relation to prior comparisons

Hsu and Lin's earlier multiclass SVM comparison treats OVO and OVR as competing decompositions and reports strong practical performance for OVO. Rifkin and Klautau argue that a carefully tuned OVR classifier can be as accurate as more elaborate schemes. These studies establish why the comparison cannot be settled by the number of binary classifiers alone; neither centers the class-imbalance and minority-class metrics emphasized by Chen and Lin. We will read their experimental protocols to identify how kernel choice, validation, and score aggregation could alter the apparent ranking. Our reproduction will therefore report both accuracy and minority-sensitive metrics under an aligned tuning budget, making the relationship between the older conclusions and the 2025 findings explicit.

### Paper and implementation scope

The primary paper is Kuan-Ting Chen and Chih-Jen Lin, “Revisiting One-Versus-One and One-Versus-Rest: Insights into Imbalanced Multi-class Classification,” ICDM 2025. The authors provide a paper and code archive on their publication page. We will inspect that implementation for its data representation, kernel configuration, decision aggregation, and parameter search. The core reproduction will use the kernel support vector machine setting, where the OVO/OVR decomposition can be isolated cleanly. If the code archive contains working scripts for the paper's other model families, we may add one small neural-classifier comparison, but the SVM comparison is the committed result. This boundary keeps the project feasible without changing its central question.

We will use Segment (2,310 instances, seven classes) and Vehicle (846 instances, four classes) from the author archive's LIBSVM download list. Both are small enough for repeated kernel SVM fitting and provide different numbers of classes. We will document the dataset version, source URL, class counts, missing-value handling, categorical encoding, feature scaling, and any sample-size restriction. The downloaded LIBSVM files are already scaled; we will record that upstream transformation. Any additional normalization, imputation, and model selection will be fit within the training fold only. We will retain one fixed test partition for each replicate, while constructing several training partitions with different class ratios. This design isolates the effect of training imbalance from a change in the evaluation population.

### Reproduction checkpoints

The author archive contains separate environment, dataset retrieval, preprocessing, SVM, neural-network, and result-generation scripts. We will use its SVM path as the primary reproducibility target and record the commit or archive checksum, software versions, and exact dataset identifiers. Before comparing algorithms, we will confirm that OVO voting and OVR score selection agree with the definitions in the paper on a tiny hand-checked multiclass example. We will then run one unchanged author configuration and preserve its output as a reference. Our controlled imbalance study will reuse the same test partition across methods and imbalance levels, while fitting preprocessing and model-selection steps inside each training fold. This order prevents a change in the test population from being mistaken for an OVO–OVR effect. We will present both the paper-aligned result and the new imbalance curves, with the exact differences in data preparation stated beside each comparison.

### Experimental protocol

For each dataset, we will create an approximately balanced condition and at least two increasingly imbalanced conditions by downsampling one or more classes in the training set. The precise ratios will be chosen so that the rarest class still has enough examples to support stratified validation. We will train OVO and OVR with identical kernel types and a comparable hyperparameter-search budget. Because OVO and OVR involve different binary problems, equality of a single numerical regularization value is not sufficient to establish fairness; instead, we will apply the same predefined search grid and validation procedure to both. We will include a simple majority-class or unweighted multiclass baseline to make the effect of imbalance visible. Results will be repeated with at least five random seeds, with seed lists and splits saved for reuse.

Our primary outputs will be the macro-F1 and balanced-accuracy curves against training imbalance. We will also report overall accuracy, geometric-mean recall, per-class precision and recall, confusion matrices, and training time. A curve can show whether a method's aggregate advantage comes from a rare class or from the largest class; the class-wise metrics and confusion matrices will make that interpretation checkable. Where possible, we will reproduce a corresponding figure or table from Chen and Lin under a closely matched configuration. If the public dataset version or author code differs from the paper's exact configuration, we will state the deviation and report both the original target and our local protocol. We will avoid labeling a separately implemented experiment as an exact numerical replication.

To make the results robust, we will compare variation across seeds rather than selecting the best run. We will report the mean and standard deviation of each metric and inspect whether a visually large difference exceeds normal split-to-split variability. We will also run a small ablation with and without class weighting or a changed decision threshold if those are supported in the chosen implementation, since imbalance remedies can interact with the decomposition strategy. This ablation will be clearly separated from the central OVO/OVR comparison. No test-set result will be used to select hyperparameters. If a dataset has too few observations in a minority class for a stable comparison, we will replace it before interpreting the outcome.

### Deliverables and expected insight

Before the final comparison, we will run two checks on the experimental pipeline. First, every generated training split must contain the intended number of examples from each class, and its paired test partition must remain unchanged across imbalance conditions. Second, predictions from a small hand-checkable example must be consistent with the selected OVO or OVR aggregation rule. These checks matter because an accidental change in split composition or decision aggregation could appear as an algorithmic effect. We will archive the resulting split summaries and one example prediction trace with the code.

The implementation will produce a reproducible pipeline for downloading or preparing selected datasets, creating saved imbalance splits, fitting both decompositions, computing metrics, and generating tables and plots. The accompanying report will describe the paper's claim, the reproduced configuration, any departures caused by data or code availability, and the extent to which the results support the claim. The central result will be a controlled comparison of accuracy and minority-sensitive metrics across documented class ratios. The report will relate any differences from the published results to data version, class distribution, model tuning, and decision aggregation. CPU training times will be reported for each dataset and condition.

## Papers to read

1. **Main paper:** Kuan-Ting Chen and Chih-Jen Lin. “Revisiting One-Versus-One and One-Versus-Rest: Insights into Imbalanced Multi-class Classification.” *IEEE ICDM*, 2025. [Paper and code](https://www.csie.ntu.edu.tw/~cjlin/papers/ovo-ovr/).
2. **Method comparison:** Chih-Wei Hsu and Chih-Jen Lin. “A Comparison of Methods for Multi-class Support Vector Machines.” *IEEE Transactions on Neural Networks* 13(2):415–425, 2002. [Author PDF](https://www.csie.ntu.edu.tw/~cjlin/papers/multisvm.pdf).
3. **Contrasting OVR evidence:** Ryan Rifkin and Aldebaro Klautau. “In Defense of One-Vs-All Classification.” *Journal of Machine Learning Research* 5:101–141, 2004. [Paper](https://www.jmlr.org/papers/v5/rifkin04a.html).
