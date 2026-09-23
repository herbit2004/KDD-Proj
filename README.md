# COMP5331 项目备选方案

本仓库汇集六个独立的 COMP5331 小组项目选题：三个 **Implementation**，三个 **Research**。每个目录均有英文 proposal、中文方案说明和逐篇列明来源的文献清单；六项是备选方案，最终选定其中一项。

| 编号 | 类型 · 课程主题 | 主要论文与相关阅读 | 拟完成的工作 | 代码、数据与实验条件 |
|---|---|---|---|---|
| [I1 · OVO/OVR](implementation-ovo-ovr/) | Implementation · Topic 2 Classification | Chen & Lin (ICDM 2025)；Hsu & Lin (2002)；Rifkin & Klautau (2004)。[文献](implementation-ovo-ovr/references/README.md) | 在 Segment 和 Vehicle 多分类数据上比较 OVO 与 OVR 的核 SVM；保持测试集不变，改变训练集类别比例，报告宏平均 F1、balanced accuracy 和分类别召回率。 | 作者 ZIP 提供数据获取、预处理、训练与结果脚本；LIBSVM 数据需由脚本下载；CPU 实验。作者代码未标明独立许可。 |
| [I2 · ExDBSCAN](implementation-exdbscan/) | Implementation · Topic 1 Clustering | Matthews et al. (KDD 2026)；Ester et al. (1996)；Romashov et al. (2022)；ExDBSCAN 补充材料。[文献](implementation-exdbscan/references/README.md) | 在二维合成数据和小型公开表格数据上，按固定 DBSCAN 聚类验证反事实有效性，并比较 ExDBSCAN 与 ExDBSCAN Random 的距离、多样性和耗时。 | 作者仓库提供核心算法；实验驱动引用了仓库未提供的输入文件，需补写小型驱动；数据可从 OpenML/UCI 获取；CPU 实验。代码未标明许可。 |
| [I3 · TABFAIRGDT](implementation-tabfairgdt/) | Implementation · Topic 7 Fairness, Equity and Justice | Panagiotou et al. (ICDM 2025)；FairGAN (2018)；Panagiotou et al. (2024)；Abroshan et al. (2024)。[文献](implementation-tabfairgdt/references/README.md) | 在 UCI Adult 上比较无公平调整与两档公平强度的生成器，测量合成数据的下游预测效用、群体公平性、样本分布和生成耗时。 | MIT 仓库有示例及实验入口；Adult 数据由加载器获取；该组设置可在 CPU 上实验。 |
| [R1 · 图异常](research-graph-anomalies/) | Research · Topic 1 Clustering | Latapy & Rajeh (ICDM 2025)；Poštuvan et al. (TMLR 2024)；MIDAS (AAAI 2020)。[文献](research-graph-anomalies/references/README.md) | 构造随机与结构化的链接异常，在相同时间切分和特征预算下评估简单图特征、经典分类器及特征选择方法。 | GPL-2.0 仓库有预处理、注入、特征和学习阶段及示例；可生成可控链接流；CPU 实验。 |
| [R2 · 密度分层 RAE](research-density-aware-rae/) | Research · Topic 4 Vector Search | Zhang & Zhao (课程列为 KDD 2026)；UMAP (2018)；Zelnik-Manor & Perona (2004)；QPAD (2025)。[文献](research-density-aware-rae/references/README.md) | 在高斯混合数据及 UCI Optical Digits 上按局部密度测量近邻保留率；固定架构和训练步数，比较原 RAE 与按近邻距离加权采样的 RAE。 | MIT 仓库有训练与基线脚本；原加载器依赖指定的预计算嵌入，Optical Digits 需编写适配层；小 GPU 可用于多次训练。 |
| [R3 · 公平社区发现](research-fair-community/) | Research · Topic 7 Fairness, Equity and Justice | Gkartzios et al. (ICDM 2025)；DMoN (JMLR 2023)；group modularity (WWW 2025)；MOUFLON (2026)。[文献](research-fair-community/references/README.md) | 在不同二元群体比例及同质连接强度的图上测量模块度与群体代表性，并比较固定公平权重和基于训练图统计量的选权规则。 | 作者仓库有模型和示例，现有公平统计按 0/1 群体编码；可生成带真值的合成图，真实图候选为 SNAP Deezer；代码未标明许可。 |

各目录中的 `proposal.md` 与 `proposal.pdf` 内容对应；`decision-brief.md` 列出复现范围、研究变量、数据入口、代码入口、评价指标及需要补充的实验组件。`references/README.md` 给出完整引文、版本、原始来源和 PDF 状态。未取得公开再分发依据的论文全文仅保存在本地 `references/local-pdf/`，不进入公开仓库；已确认许可的文件位于 `references/public-pdf/`。

方案格式依据本学期 [Project Proposal](https://canvas.ust.hk/courses/72132/pages/project-proposal) 和 [Suggested Topics](https://canvas.ust.hk/courses/72132/pages/project-suggestedtopics) 页面。课程要求具体题目、项目类型、小组与成员信息、约 1,000–2,000 词的项目描述及待读论文清单；自选推荐清单以外的论文作为选题须征得教师同意。ExDBSCAN 的 KDD 正文可通过 [Canvas 课程文件](https://canvas.ust.hk/courses/72132/files/12588624?wrap=1)阅读，本地文献目录目前保存的是独立的 arXiv 补充材料。资料核对日期：2026-09-24。
