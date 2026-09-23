# I3｜TABFAIRGDT 公平表格数据生成

**类型：** Implementation · **课程方向：** generative data mining / algorithmic fairness · **主论文：** Panagiotou et al., ICDM 2025，见 [文献目录](references/README.md)。

## 研究内容与复现边界

TABFAIRGDT 使用自回归决策树生成表格记录，并在生成过程中处理指定敏感属性的公平性约束。论文强调与较重的生成器相比的速度、效用和公平性。本项目在一至两份公开数据上重现这三方面比较：原始数据训练的下游模型、普通树生成器、TABFAIRGDT，以及仓库中可轻量运行的一个生成基线。关注公平指标改善是否以任务效用或群体覆盖损失为代价。

配套阅读中，FairGAN 是生成公平数据的早期基线；Panagiotou 等人的比较研究分析类别与敏感群体不平衡；Abroshan 等人区分生成数据约束与下游预测公平性。这三篇用于确定指标、解释结果和定位基线；数值实验限定为仓库可运行的基线。

## 可用资源

| 维度 | 状态与具体入口 |
|---|---|
| 论文 | [arXiv v1](https://arxiv.org/abs/2509.19927)，ICDM 2025；CC BY 4.0，公开目录可保存署名 PDF。 |
| 代码 | [作者仓库](https://github.com/Panagiotou/TABFAIRGDT)，MIT；`example.py` 是最小入口，`main.py --dataset ... --methods ... --mode run` 可跑多方法，`--mode plot` 画图；另有 `run_experiment.py`、评估与竞品模块。**入口较齐，但尚未安装或跑通**。完整基线环境含 `synthcity`、`atom-ml[full]`，可选竞品和公平切分还要装额外包，不能把一次样例运行等同于整篇复现。 |
| 训练/评估 | 必须拟合生成器、采样合成数据、训练下游分类器并计算效用与群体公平指标；一般不需 GPU。 |
| 数据 | `tabular_datasets/dataset.py` 有数据集配置并使用 `ucimlrepo`、Folktables 等途径自动抓取支持的真实基准，首次运行需要网络并生成本地缓存。可先选 [UCI Adult](https://archive.ics.uci.edu/dataset/2/adult)；原始数据不直接捆绑在仓库。须固定敏感属性、预测标签、缺失值处理和 train/test 分离，防止生成数据泄漏到评估集。 |
| 算力 | 小中型表格数据在 CPU 可行。先用 10k–50k 行、少量随机种子估时，之后再决定是否扩大。 |

## 实验设计与产出

对每份数据固定训练/测试切分，仅训练集用于生成器拟合；用真实训练集和合成训练集分别训练相同下游分类器，在未触及的真实测试集上测准确率/AUROC、demographic parity difference、equal opportunity difference。再报告训练与采样耗时、连续/类别字段分布差异及各群体生成样本数。改变公平权重或约束强度，画效用—公平性曲线。用至少三个随机种子报告波动，尤其避免把单次公平指标波动解释成稳定收益。

## 实施依赖与核对点

仓库有 `example.py` 和多方法实验入口；主要实验链由数据加载、生成器拟合、合成数据采样、下游分类训练和真实测试集评估组成。需固定 Adult 数据版本、敏感属性编码、分类标签及同一训练/测试划分；竞品方法另需安装其依赖。当前已核对入口及依赖声明，尚无运行记录。
