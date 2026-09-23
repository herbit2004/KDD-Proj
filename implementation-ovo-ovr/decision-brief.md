# I1｜不平衡多分类中的 OVO 与 OVR

**类型：** Implementation · **课程方向：** classification / imbalanced learning · **主论文：** Chen and Lin, ICDM 2025，见 [文献目录](references/README.md)。

## 研究内容与复现边界

论文重新比较 one-versus-one (OVO) 与 one-versus-rest (OVR) 的多分类分解方式，尤其关注类别不平衡时整体准确率可能掩盖少数类表现的问题。核心不是发明新模型，而是在可控实验中分清分解策略、类别比例、超参数与评估指标各自的影响。项目以核 SVM 为主要复现对象，在两至三份可公开取得的多分类数据上重复训练；神经网络相关比较可作为时间允许时的扩展。直接复现所有论文数据和模型并非必要。

文献链包括 Chen–Lin 主论文、Hsu–Lin 2002 年的多分类 SVM 比较，以及 Rifkin–Klautau 2004 年对 OVR 的反向证据。后两篇帮助判断差异究竟来自分解策略，还是核、调参和统计指标；数值复现以主论文为准，不要求重跑两篇早期论文的完整实验。

## 可用资源

| 维度 | 状态与具体入口 |
|---|---|
| 论文 | 作者 [PDF](https://www.csie.ntu.edu.tw/~cjlin/papers/ovo-ovr/OVO-OVR.pdf)，ICDM 2025；本地已核对首页标题、作者、9 页。 |
| 代码 | [作者页面](https://www.csie.ntu.edu.tw/~cjlin/papers/ovo-ovr/)的 `ovo-ovr-code.zip` **包含完整实验阶段**：`build_env.sh`、`get_data.sh`、`process_data.sh`、`run_svm.py`、`run_nn.py`、`generate_results.py`、`additional_results.py`，并附 README 和 LIBSVM。已检查文件清单与说明，**尚未安装和实际跑通**。作者部分未见单独许可声明；附带 LIBSVM 有自己的版权文件，不能推断整包采用同一许可证。 |
| 训练/评估 | `run_svm.py` 是主入口，`run_nn.py` 为可选扩展；`generate_results.py` 汇总 Accuracy/MacroF1，`additional_results.py` 生成不平衡分组及混淆矩阵。论文级脚本齐全，但还需检查版本与下载地址。 |
| 数据 | `get_data.sh` 从 KEEL/LIBSVM/UCI 抓取原始数据，`process_data.sh` 预处理并建立分层五折；ZIP **不含完整原始数据**。优先选脚本中能直接抓取的两至三份小型数据，记录来源、样本数和类别比例。数据集各自许可仍须核对。 |
| 算力 | CPU 即可；核 SVM 的开销随样本量上升明显。建议先用每份数千样本的子集，并记录训练时间。 |

## 实验设计与产出

固定相同的训练/测试划分，对每份数据创建原始分布与多个训练集重采样比例；测试集保留同一分布。比较 OVO、OVR 的宏平均 F1、balanced accuracy、G-mean、各类 recall 与总体 accuracy，至少重复五个随机种子，并报告均值、方差和分类别混淆矩阵。主要图是“不平衡比例—宏平均指标”曲线。若论文使用不同的采样或调参协议，需在报告中明确区分严格复现与小规模复现实验。代码交付包括数据准备、训练、评估和重画图表脚本。

## 难度与决策

**估计：低至中；三项 Implementation 中最稳。** 作者提供了从环境、下载到汇总的脚本，缺口主要是旧依赖、源站可用性与实验口径对齐。首个里程碑应是用作者脚本在一份小数据上生成 SVM 结果；随后再做受控的训练集重采样。若必须独立重写关键算法，应在报告中区分作者代码复现与独立实现。尚无实际跑通证据。
