# I2｜ExDBSCAN 反事实聚类解释

**类型：** Implementation · **课程方向：** clustering / explainable data mining · **主论文：** Matthews et al., KDD 2026，见 [文献目录](references/README.md)。

## 研究内容与复现边界

DBSCAN 会把点归到密度簇或噪声，但“怎样最小改变一个点使其被分到另一簇”不能只看欧氏距离。ExDBSCAN 保持原聚类固定，以目标簇核心点的 `eps` 邻域判定新点归属；用密度连接图选择兼顾接近性和多样性的核心点，再构造反事实。本项目先重现二维案例，再在小型公开表格数据上比较 ExDBSCAN 与论文的 **ExDBSCAN Random** 基线。实验限定论文实现支持的单点数值特征和固定聚类，不覆盖全部 30 个基准。

除主论文外，DBSCAN 原论文提供核心点与密度可达性的定义，BayCon 提供通用反事实搜索参照，ExDBSCAN 的 Additional Material 提供伪代码与扩展实验。四份材料的角色不同：主论文与补充材料确定复现目标，DBSCAN 用于独立有效性检查，BayCon 用于解释基线差异；缩小后的数值对照以可构造的 ExDBSCAN Random 为主。

## 可用资源

| 维度 | 状态与具体入口 |
|---|---|
| 论文 | 2026 年 KDD 正文 12 页，DOI [10.1145/3770855.3817692](https://doi.org/10.1145/3770855.3817692)；正文首页声明 CC BY 4.0，Canvas 可阅读。另有 [arXiv Additional Material](https://arxiv.org/abs/2605.30225)，24 页，本地已下载。两者是**不同文档**，目前本地目录缺 KDD 正文 PDF。 |
| 代码 | [作者仓库](https://github.com/tommasoamico/ExDBSCAN)有 `modules/dbscanCounterfactuals.py` 等核心实现；未见 LICENSE、`requirements.txt` 或锁定环境。`runExDbscan.py` 启动时即读取不存在的 `data/iclrDbscan/resultsDiversityNonActionableDPP.csv`，还依赖缺失的 `datasetsParameters.json`；**原样克隆后不能直接运行主实验**。需要自建小型驱动脚本调用核心类，并记录兼容性修改。未实际运行。 |
| 训练/评估 | 无神经模型训练；需要拟合 DBSCAN，按固定簇结构生成解释，独立核对目标簇核心点邻域的有效性，并计算论文定义的 proximity、DPP diversity 和每次查询耗时。 |
| 数据 | 论文用多份 OpenML 表格数据，可经 [OpenML](https://www.openml.org/)下载。先用可控二维合成数据和 UCI Iris/Wine 等小数据；记录数值化、标准化、`eps`、`min_samples` 与噪声比例。 |
| 算力 | CPU 足够做小型数据；反事实候选搜索可能随维度和候选数增长。限制数据规模，并记录每个 query 的运行时间。 |

## 实验设计与产出

在合成双月形/多密度数据上做边界和噪声点案例图，之后在公开表格数据上固定 DBSCAN 参数和查询点。主比较是 ExDBSCAN 对 ExDBSCAN Random：两者使用相同的核心点式构造，仅选点策略不同，能检验论文的主要贡献。逐查询报告有效解释数、标准化特征空间距离、密度图上的 DPP 多样性、耗时及失败率。核验时**不重新聚类**，而用原始核心点的邻域判定新点归属。交付小型驱动、固定输入、指标代码和图表。

## 难度与决策

**估计：中高；不属于省事复现。** 算力低但工程风险高：仓库核心模块可读，主实验驱动所需文件却缺失，代码也未声明再利用许可。若小型驱动无法在短时间内调用核心类，这项应退出候选。它适合愿意处理算法实现细节和实验基础设施的小组，不能与 I1 一样按“下载即跑”预期安排。
