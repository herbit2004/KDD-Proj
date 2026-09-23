# I1｜不平衡多分类中的 OVO 与 OVR

**类型：** Implementation · **课程方向：** classification / imbalanced learning · **主论文：** Chen and Lin, ICDM 2025，见 [文献目录](references/README.md)。

## 研究内容与复现边界

论文重新比较 one-versus-one (OVO) 与 one-versus-rest (OVR) 的多分类分解方式，尤其关注类别不平衡时整体准确率可能掩盖少数类表现的问题。核心不是发明新模型，而是在可控实验中分清分解策略、类别比例、超参数与评估指标各自的影响。项目以核 SVM 为主要复现对象，在两至三份可公开取得的多分类数据上重复训练；神经网络相关比较可作为时间允许时的扩展。直接复现所有论文数据和模型并非必要。

## 可用资源

| 维度 | 状态与具体入口 |
|---|---|
| 论文 | 作者 [PDF](https://www.csie.ntu.edu.tw/~cjlin/papers/ovo-ovr/OVO-OVR.pdf)，ICDM 2025；本地已核对首页标题、作者、9 页。 |
| 代码 | [作者页面](https://www.csie.ntu.edu.tw/~cjlin/papers/ovo-ovr/)提供代码 ZIP；不是一个带持续维护测试、环境锁定和一键复现流程的完整仓库。许可需查看 ZIP 内说明后确定。 |
| 训练/评估 | 需要训练核 SVM；评估可独立用 scikit-learn 指标实现。若沿用作者代码，先确认数据格式、参数搜索范围和随机种子。 |
| 数据 | [LIBSVM 数据集](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/)、UCI 和 KEEL 有公开下载入口。优先选择小至中等规模、至少三类、有原始或可控制类别比例的数据；下载前记录数据版本和使用条款。 |
| 算力 | CPU 即可；核 SVM 的开销随样本量上升明显。建议先用每份数千样本的子集，并记录训练时间。 |

## 实验设计与产出

固定相同的训练/测试划分，对每份数据创建原始分布与多个训练集重采样比例；测试集保留同一分布。比较 OVO、OVR 的宏平均 F1、balanced accuracy、G-mean、各类 recall 与总体 accuracy，至少重复五个随机种子，并报告均值、方差和分类别混淆矩阵。主要图是“不平衡比例—宏平均指标”曲线。若论文使用不同的采样或调参协议，需在报告中明确区分严格复现与小规模复现实验。代码交付包括数据准备、训练、评估和重画图表脚本。

## 难度与决策

**估计：低至中。** 优点是问题清楚、计算轻、指标标准化，易形成可审查的对照实验。主要风险是作者代码 ZIP 的可运行性和原论文多个实验设置的对齐成本；即使原代码无法运行，基于 LIBSVM/scikit-learn 的独立实现仍可检验核心结论，但应如实命名为 independent reproduction。适合优先选择稳妥完成 Implementation 的小组。
