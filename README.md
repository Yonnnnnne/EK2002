# EK2002

复现 Eaton & Kortum (2002), *Technology, Geography, and Trade*（Econometrica）的论文结果。

复现所需的原始数据与代码放在 [`Technology, Geography and Trade/`](Technology,%20Geography%20and%20Trade/) 目录下，主体代码为 Jupyter Notebook `code.ipynb`，按论文的实证与反事实部分依次排列。

## 目录与文件说明

| 文件 | 功能 |
| --- | --- |
| `code.ipynb` | 复现代码，覆盖数据读取、描述性统计、回归估计与反事实模拟，按章节顺序执行 |
| `trade1.fmt` | 原始二进制数据：双边贸易额、距离分档、是否接壤，以及研发、人力资本、人口密度、劳动力与工资等国家特征 |
| `trade2.fmt` | 原始二进制数据：教育年限、国内贸易额、进口额、制造业支出等 |
| `trade3.fmt` | 原始二进制数据：各国总进口、总出口、名义工资、研发存量、劳动力、人口密度 |
| `datafe.fmt` | 原始二进制数据：汇率、制造业劳动力、名义工资、19×19 贸易矩阵、GDP，以及由回归系数合成的地理障碍矩阵 |
| `getdist.dat` | 双边距离数据（单位：千英里） |
| `pppdat1.prn` | 18 国 50 种制造业商品的价格数据（美国为基准，代码中补零） |
| `country_code.csv` | 19 个样本国的编号、缩写与国名对照表 |
| `regression_data.csv` | 整理好的回归样本：在原始数据基础上补充 `common_language`、`both_in_EU`、`both_in_EFTA` 等虚拟变量与距离变量，并含论文已计算好的因变量 `trade_prime`，供回归部分直接读取 |

其中 `.fmt` 文件为不带格式说明的 Fortran 二进制文件，由 notebook 中的 `read_ek_data` 按固定字节偏移与 `float64` 类型读入。

## 复现流程

notebook 自上而下分为四块，依次对应论文的各个部分：

1. **数据读取与整理**：读取上述原始文件，拼接为各国信息表、双边贸易表与价格表，并完成工资、劳动力的人力资本调整及少数缺失/异常值的修正。
2. **描述性统计**：
   - Table I / Figure I：样本国的进口占制造业支出比重、工资、劳动力、制造业占 GDP 比重，以及标准化进口份额与距离的关系。
   - Table II / Figure II：用价格数据构造的价格度量，及其与标准化进口份额的关系。
3. **回归与参数估计**：
   - Table III：贸易方程的基准 OLS。
   - Table IV：考虑误差项相关结构后的 GLS。
   - Table V：用工资数据估计 `θ`；随后用价格数据再估计 `θ`。
   - Table VI / Table VII：由回归结果计算各国技术水平与地理障碍，并把地理参数换算为成本影响。
4. **反事实模拟**：先求解基准均衡（工资、价格、贸易份额），再依次模拟自给自足、零地理障碍、贸易翻倍、提高美国/德国技术水平、移除关税、移除 EC 内关税等情形，对应 Table IX–Table XII 与 Figure III。每个情形均区分劳动力可流动与不可流动两种设定。

## 如何使用

1. 安装依赖：`pandas`、`numpy`、`matplotlib`、`scipy`、`statsmodels`。
2. 在 `Technology, Geography and Trade/` 目录下启动 Jupyter，打开 `code.ipynb`。
   notebook 使用相对路径读取数据，工作目录必须为该目录。
3. 从头依次运行全部单元格。表格与图形在对应单元格输出，反事实部分会先求解基准均衡，再逐个场景求解，运行时间主要花在这一部分。

各段落的 markdown 单元格写有论文公式、变量口径与实现上的取舍说明，可与代码对照阅读。
