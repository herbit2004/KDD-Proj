# 参考文献与资源

1. **主论文：** Emmanouil Panagiotou、Benoît Ronval、Arjun Roy、Ludwig Bothmann、Bernd Bischl、Siegfried Nijssen 与 Eirini Ntoutsi. “TABFAIRGDT: A Fast Fair Tabular Data Generator using Autoregressive Decision Trees.” *IEEE International Conference on Data Mining (ICDM)*，2025。[arXiv 页面](https://arxiv.org/abs/2509.19927) · [论文 PDF](https://arxiv.org/pdf/2509.19927)。本地文件：`local-pdf/2509.19927v1.pdf`（10 页）。arXiv 页面标明 [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)；公开目录 `public-pdf/` 保存了署名副本。
2. **生成基线与研究动机：** Depeng Xu、Shuhan Yuan、Lu Zhang 与 Xintao Wu. “FairGAN: Fairness-aware Generative Adversarial Networks.” *IEEE International Conference on Big Data*，570–575 页，2018。[arXiv 页面](https://arxiv.org/abs/1805.11202) · [PDF](https://arxiv.org/pdf/1805.11202)。本地文件：`local-pdf/1805.11202.pdf`（11 页，arXiv 第 1 版）。该文研究公平合成数据与下游分类器的关系；其 arXiv 许可未明确授予公开再分发权利。
3. **不平衡数据比较：** Emmanouil Panagiotou、Arjun Roy 与 Eirini Ntoutsi. “Synthetic Tabular Data Generation for Class Imbalance and Fairness: A Comparative Study.” arXiv:2409.05215，第 1 版，2024。[arXiv 页面](https://arxiv.org/abs/2409.05215) · [PDF](https://arxiv.org/pdf/2409.05215)。本地文件：`local-pdf/2409.05215.pdf`（15 页）。该文同时讨论类别与敏感群体不平衡，供生成器评价方案参考；未确认公开再分发许可。
4. **公平性定义：** Mahed Abroshan、Andrew Elliott 与 Mohammad Mahdi Khalili. “Imposing Fairness Constraints in Synthetic Data Generation.” *Proceedings of AISTATS*，PMLR 238:2269–2277，2024。[会议页面](https://proceedings.mlr.press/v238/abroshan24a.html) · [PDF](https://proceedings.mlr.press/v238/abroshan24a/abroshan24a.pdf)。本地文件：`local-pdf/abroshan24a.pdf`（22 页，含附录）。该文区分生成阶段的公平约束与下游预测的公平评价；未确认这一 PDF 文件的公开再分发条款。

**数据与代码：** [作者 GitHub 仓库](https://github.com/Panagiotou/TABFAIRGDT)采用 MIT 许可，包含示例、实验和评价脚本。主要基准为 [UCI Adult](https://archive.ics.uci.edu/dataset/2/adult)；仓库的数据加载器和 UCI 页面提供获取入口。示例代码设 `lamda=0` 表示不作公平调整，拟合时可设置更高的数值。

**版本差异：** Canvas 推荐清单仅列出四位作者；课程 PDF 与所链接的 arXiv 第 1 版均列出上述七位作者。
