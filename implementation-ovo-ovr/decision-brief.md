# I1｜不平衡多分类中的 OVO 与 OVR

**项目类型：** 复现类 · **课程方向：** 分类与类别不平衡学习 · **主论文：** Chen 与 Lin，ICDM 2025，见[文献目录](references/README.md)。

## 研究内容与复现边界

论文重新比较一对一（OVO）与一对其余（OVR）的多分类分解方式，尤其关注类别不平衡时整体准确率可能掩盖少数类表现的问题。项目以核支持向量机（SVM）为主要复现对象，使用作者下载清单中的 Segment（2,310 条、7 类）和 Vehicle（846 条、4 类）数据，控制类别比例、超参数与评估指标；神经网络比较列为扩展实验。

文献链包括 Chen–Lin 主论文、Hsu–Lin 2002 年的多分类 SVM 比较，以及 Rifkin–Klautau 2004 年的 OVR 实验。后两篇用于确定核、调参和聚合方式对比较结论的影响；数值复现目标取自主论文。

## 可用资源

| 维度 | 状态与具体入口 |
|---|---|
| 论文 | 作者 [PDF](https://www.csie.ntu.edu.tw/~cjlin/papers/ovo-ovr/OVO-OVR.pdf)，ICDM 2025；本地已核对首页标题、作者、9 页。 |
| 代码 | [作者页面](https://www.csie.ntu.edu.tw/~cjlin/papers/ovo-ovr/)的 `ovo-ovr-code.zip` **包含完整实验阶段**：`build_env.sh`、`get_data.sh`、`process_data.sh`、`run_svm.py`、`run_nn.py`、`generate_results.py`、`additional_results.py`，并附 README 和 LIBSVM。已检查文件清单与说明，**尚未安装和实际跑通**。作者部分未见单独许可声明；附带 LIBSVM 有自己的版权文件，不能推断整包采用同一许可证。 |
| 训练与评估 | `run_svm.py` 是主入口，`run_nn.py` 为可选扩展；`generate_results.py` 汇总准确率与宏平均 F1，`additional_results.py` 生成不平衡分组及混淆矩阵。论文级脚本齐全，但还需检查版本与下载地址。 |
| 数据 | `get_data.sh` 从 KEEL/LIBSVM/UCI 获取数据；[LIBSVM Segment 与 Vehicle 条目](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/multiclass.html)提供类别数、样本数和特征数。`process_data.sh` 建立分层五折。作者清单使用预缩放的 `segment.scale`、`vehicle.scale`；记录上游缩放来源，项目新增预处理只在训练折拟合。ZIP 不含完整数据。 |
| 算力 | CPU 实验；两份数据均低于 2,500 条，记录每折训练与调参耗时。 |

## 实验设计与产出

固定相同的训练集与测试集划分，对每份数据创建原始分布与多个训练集重采样比例；测试集保留同一分布。比较 OVO、OVR 的宏平均 F1、平衡准确率、几何平均数（G-mean）、各类别召回率与总体准确率，至少重复五个随机种子，并报告均值、方差和各类别混淆矩阵。主要图是“不平衡比例—宏平均指标”曲线。若论文使用不同的采样或调参方案，需在报告中明确区分严格复现与小规模复现实验。代码交付包括数据准备、训练、评估和重画图表脚本。

## 实施依赖与核对点

作者 ZIP 覆盖环境建立、数据下载、预处理、SVM 训练和结果汇总。实施时需先记录依赖版本、数据下载地址和一份小数据的原始脚本输出，再加入训练集重采样实验。最终结果分别标明沿用作者代码的部分、修改的数据处理和新增指标；当前只有文件清单与脚本入口检查，尚无端到端运行记录。
