# I3｜TABFAIRGDT 公平表格数据生成

**类型：** Implementation · **课程方向：** generative data mining / algorithmic fairness · **主论文：** Panagiotou et al., ICDM 2025，见 [文献目录](references/README.md)。

## 研究内容与复现边界

TABFAIRGDT 使用自回归决策树生成表格记录，并在生成过程中处理指定敏感属性的公平性约束。论文强调与较重的生成器相比的速度、效用和公平性。本项目在一至两份公开数据上重现这三方面比较：原始数据训练的下游模型、普通树生成器、TABFAIRGDT，以及仓库中可轻量运行的一个生成基线。关注公平指标改善是否以任务效用或群体覆盖损失为代价。

## 可用资源

| 维度 | 状态与具体入口 |
|---|---|
| 论文 | [arXiv v1](https://arxiv.org/abs/2509.19927)，ICDM 2025；CC BY 4.0，公开目录可保存署名 PDF。 |
| 代码 | [作者仓库](https://github.com/Panagiotou/TABFAIRGDT)，MIT；有 `example.py`、`main.py`、`requirements.txt`、`run_experiment.py` 和评估/基线模块。虽有实验入口，不能据此推断全部论文表格可一键生成。 |
| 训练/评估 | 必须拟合生成器、采样合成数据、训练下游分类器并计算效用与群体公平指标；一般不需 GPU。 |
| 数据 | 可从 [UCI Adult](https://archive.ics.uci.edu/dataset/2/adult) 或 OpenML 下载；仓库附合成样例，完整原始基准需另取。须固定敏感属性、预测标签、缺失值处理和 train/test 分离，防止生成数据泄漏到评估集。 |
| 算力 | 小中型表格数据在 CPU 可行。先用 10k–50k 行、少量随机种子估时，之后再决定是否扩大。 |

## 实验设计与产出

对每份数据固定训练/测试切分，仅训练集用于生成器拟合；用真实训练集和合成训练集分别训练相同下游分类器，在未触及的真实测试集上测准确率/AUROC、demographic parity difference、equal opportunity difference。再报告训练与采样耗时、连续/类别字段分布差异及各群体生成样本数。改变公平权重或约束强度，画效用—公平性曲线。用至少三个随机种子报告波动，尤其避免把单次公平指标波动解释成稳定收益。

## 难度与决策

**估计：中。** 代码及许可证最齐，CPU 成本可控；难点是公平定义、数据预处理和下游评估链条比单一算法更长。若选题，先运行仓库样例，再用一份数据打通整条流水线，最后添加第二份数据和基线。
