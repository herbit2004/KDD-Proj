# R2｜局部密度变化下的近邻保持降维

**类型：** Research · **课程方向：** dimensionality reduction / vector search · **主论文：** Zhang and Zhao, RAE；完整引文见 [文献目录](references/README.md)。

## 研究问题与已有工作

RAE 以神经网络把高维向量压缩到低维，目标是尽可能保留供近邻检索使用的邻居关系。总体 recall@k 可以掩盖稀疏区域或不同密度簇中的困难样本。UMAP 提供基于近邻图的局部结构基线。本项目先问：RAE 相对 PCA/UMAP 的邻居保留率是否随局部密度系统性变化；随后测试一种按密度分层采样或加权的轻量训练改动能否缩小最差密度组的差距，同时维持总体检索质量。

Zelnik-Manor–Perona 的 self-tuning spectral clustering 解释为何局部尺度不能简单用全局阈值替代；QPAD 则是更接近检索目标的近期降维工作。二者使研究问题收窄为：在**保持 RAE 模型及损失不变**时，仅重新分配不同密度样本的训练曝光能否改善最差组 recall。QPAD 只在入口和数据协议兼容时加入数值基线，避免把方法差异误作公平比较。

**研究增量：** 密度感知采样、加权和局部邻居优化已有相关工作。本实验限定在 RAE 的近邻保留目标下，改变一项采样权重变量，并与相同训练预算的原 RAE 比较。分层与总体结果共同用于分析局部改进和整体检索质量之间的关系。

## 资源与实验

| 维度 | 状态与具体入口 |
|---|---|
| 论文版本 | 2026 年课程清单、[作者主页](https://faculty.washington.edu/dzhao/)和[作者代码仓库](https://github.com/explorerZH/RAE-KDD2026)均标 **KDD 2026**。当前可下载的 [arXiv v1 PDF](https://arxiv.org/abs/2509.25839) 与 Canvas PDF 首页仍写 ICLR 2026，属于版本标头冲突；对课程选题用 KDD 2026，引用 PDF 时注明其为早期版本。仓库 BibTeX 的 DOI 还是占位符，不能照抄。 |
| 代码 | MIT；有 `code/train.py`、`code/baselines.py`、`code/data_utils.py` 和依赖说明。训练/评估入口存在，但**仅接受作者规定的若干预计算嵌入格式**；数据未捆绑，未实际跑通。 |
| 数据 | 作者脚本指定 CelebA/ViT、IMDb/MPNet、Tiny ImageNet/DINOv2、Flickr30k/CLIP、SIFT1B 的特定路径与文件结构。本方案另用 [UCI Optical Recognition of Handwritten Digits](https://archive.ics.uci.edu/dataset/80/optical+recognition+of+handwritten+digits)：5,620 条、64 数值特征、有发布的训练/测试文件及 CC BY 4.0 数据许可。它不能直接进入作者加载器，需编写格式适配层。记录标准化、查询/库划分与原空间近邻定义。 |
| 训练/评估 | 需训练 RAE 与改动模型；FAISS/近邻索引用于评估。小数据 CPU 可以试通，小 GPU 有利于多种子比较。 |
| 算力 | UCI 实验使用 5,620 条、64 维向量；近邻索引、RAE 训练和三档采样强度均需计时。合成实验限制在数万条向量以内，固定目标维度、batch 大小和优化步数；可用小 GPU 加速训练。 |

以训练集第 20 近邻距离 `d_i` 定义局部密度；评价时按该距离的分位组汇总 recall。训练采样概率与 `clamp(d_i / median(d), 0.5, 2)^alpha` 成正比，`alpha` 在验证集从 0、0.5、1 中选择。比较 PCA、UMAP、原 RAE、密度采样 RAE 的 recall@10/50、最差密度组 recall、训练耗时和低维检索耗时；固定优化步数、batch 大小和采样总量。先做高斯混合可控实验，再使用 Optical Digits 的已发布划分。

## 实施依赖与核对点

作者的数据加载器需要特定预计算嵌入；本方案的合成向量和小型公开向量须转换为同样的张量、划分和索引格式。拟用小矩阵先核对训练与近邻评估入口，再固定原空间邻居定义、目标维度、优化步数和批量大小，进行密度分层比较。原论文的大规模嵌入不随仓库提供；当前已核对训练脚本和数据接口，尚无端到端运行记录。
