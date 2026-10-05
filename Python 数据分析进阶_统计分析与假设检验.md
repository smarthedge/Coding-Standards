# [深入解析：Python 数据分析进阶：统计分析与假设检验](https://www.cnblogs.com/gccbuaa/p/19689402)

## ***\**\*引言\*\**\***

​    在数据分析项目里,我们经常需要基于样本数据做出推断和决策。统计分析与假设检验是支撑"从数据到结论"的关键方法。掌握它们,可以让你更严谨地回答"效果是否显著""差异是否真实"等问题,避免被偶然现象误导。

​    本文将系统讲解:

​        • 描述统计与概率分布的基础概念

​        • 假设检验的完整流程与常用方法

​        • 使用Python实现完整分析,包含数据处理、统计检验与可视化

​        • 结合真实案例,从数据加载到结论解读

### ***\**\*核心知识点\*\**\***

#### ***\**\*1. 描述统计\*\**\***

**描述统计** 是用数学方法对数据进行概括和描述的分析方法。

##### ***\**\*\*\*\*集中趋势指标\*\*\*\*\**\***

​    • **均值(mean)**: 所有数值的总和除以数量,受极端值影响大

​    • **中位数(median)**: 排序后位于中间的值,不受极端值影响

​    • **众数(mode)**: 出现频率最高的数值

##### ***\**\*\*\*\*离散程度指标\*\*\*\*\**\***

​    • **方差(variance)**: 各数据点与均值偏差的平方和的平均值

​    • **标准差(standard deviation)**: 方差的平方根,衡量数据分散程度

​    • **极差(range)**: 最大值与最小值的差

​    • **四分位距(IQR)**: 上四分位数与下四分位数的差

##### ***\**\*\*\*\*分布形态\*\*\*\*\**\***

​    • **偏度(skewness)**: 衡量分布的不对称性

​    ◦ 正偏(右偏): 长尾在右侧

​    ◦ 负偏(左偏): 长尾在左侧

​    • **峰度(kurtosis)**: 衡量分布的尖锐程度

#### ***\**\*2. 概率分布\*\**\***

##### ***\**\*\*\*\*正态分布\*\*\*\*\**\***

最重要的连续概率分布,又称高斯分布,其特点:

​    • 钟形曲线,关于均值对称

​    • 约68%的数据在±1个标准差内,95%在±2个标准差内

​    • 许多自然现象都近似服从正态分布

##### ***\**\*\*\*\*t分布\*\*\*\*\**\***

​    适用于小样本(n<30)且总体标准差未知的情况,类似于正态分布但尾部更厚。

##### ***\**\*\*\*\*卡方分布\*\*\*\*\**\***

​    主要用于:

​    • 检验分类变量的独立性

​    • 检验方差是否等于某个值

##### ***\**\*\*\*\*F分布\*\*\*\*\**\***

​    主要用于:

​        • 方差分析(ANOVA)

​        • 比较两个方差

#### ***\**\*3. 假设检验的完整流程\*\**\***

​    假设检验是统计推断的核心方法,用于根据样本数据判断关于总体参数的假设是否成立。

##### ***\**\*\*\*\*Step 1: 建立假设\*\*\*\*\**\***

​    • **零假设(H0)**: 默认状态,通常表示"没有差异""没有效果"

​    • **备择假设(H1)**: 我们想证明的假设,表示"有差异""有效果"

##### ***\**\*\*\*\*Step 2: 选择显著性水平α\*\*\*\*\**\***

​    • 常用值: 0.05 (5%) 或 0.01 (1%)

​    • α是拒绝H0时犯第一类错误(弃真)的概率

##### ***\**\*\*\*\*Step 3: 选择检验方法并计算统计量\*\*\*\*\**\***

​    根据数据类型和研究问题选择合适的检验方法(见后文方法对比)

##### ***\**\*\*\*\*Step 4: 做出决策\*\*\*\*\**\***

​    • **p值法**: 比较p值与α

​    ◦ p < α: 拒绝H0,结果具有统计显著性

​    ◦ p ≥ α: 不拒绝H0

​    • **临界值法**: 比较统计量与临界值

##### ***\**\*\*\*\*Step 5: 解释结论\*\*\*\*\**\***

​    • **统计意义**: 结果是否具有统计学意义

​    • **实际意义**: 结果在现实应用中的价值和影响

#### ***\**\*4. 常用假设检验方法对比\*\**\***

| **检验方法**    | **数据类型**  | **适用场景**                     | **Python函数**                 |
| --------------- | ------------- | -------------------------------- | ------------------------------ |
| 单样本t检验     | 连续          | 检验样本均值与已知均值差异       | scipy.stats.ttest_1samp()      |
| 双样本独立t检验 | 连续,两组独立 | 比较两组独立样本的均值差异       | scipy.stats.ttest_ind()        |
| 配对t检验       | 连续,成对数据 | 比较同一组对象的两次测量差异     | scipy.stats.ttest_rel()        |
| 卡方独立性检验  | 分类          | 检验两个分类变量是否独立         | scipy.stats.chi2_contingency() |
| 单因素ANOVA     | 连续,多组     | 比较三组及以上独立样本的均值差异 | scipy.stats.f_oneway()         |

### ***\**\*代码实践\*\**\***

#### ***\**\*环境准备\*\**\***

首先导入必要的Python库:

```python
import numpy as np
import pandas as pd
from scipy import stats
import statsmodels.api as sm
from statsmodels.formula.api import ols
import matplotlib.pyplot as plt
import seaborn as sns
# 设置绘图风格
sns.set(style="whitegrid", font_scale=1.2)
plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号
```

#### ***\**\*1. 描述统计实践\*\**\***

生成示例数据并进行描述统计分析:

```python
# 设置随机种子保证结果可复现
np.random.seed(42)
# 生成服从正态分布的数据
data = np.random.normal(loc=50, scale=10, size=200)
# 转换为Series便于分析
data_series = pd.Series(data, name='示例数据')
# 计算描述统计量
print("=== 描述统计结果 ===")
print(f"样本数量: {len(data_series)}")
print(f"均值: {data_series.mean():.2f}")
print(f"中位数: {data_series.median():.2f}")
print(f"标准差: {data_series.std():.2f}")
print(f"最小值: {data_series.min():.2f}")
print(f"最大值: {data_series.max():.2f}")
print(f"25%分位数: {data_series.quantile(0.25):.2f}")
print(f"75%分位数: {data_series.quantile(0.75):.2f}")
print(f"偏度: {data_series.skew():.2f}")
print(f"峰度: {data_series.kurt():.2f}")
```

输出示例:

```python
=== 描述统计结果 ===
样本数量: 200
均值: 50.03
中位数: 50.14
标准差: 9.71
最小值: 20.53
最大值: 76.98
25%分位数: 43.20
75%分位数: 56.78
偏度: 0.05
峰度: -0.34
```

#### ***\**\*2. 数据可视化\*\**\***

可视化是理解数据分布的重要手段:

```python
# 创建画布
fig, axes = plt.subplots(2, 2, figsize=(14, 10))
# 直方图+核密度估计
sns.histplot(data_series, kde=True, ax=axes[0, 0], color='steelblue', bins=20)
axes[0, 0].set_title('数据分布与核密度估计', fontsize=14)
axes[0, 0].set_xlabel('值')
axes[0, 0].set_ylabel('频数')
# 箱线图
sns.boxplot(y=data_series, ax=axes[0, 1], color='lightgreen')
axes[0, 1].set_title('箱线图', fontsize=14)
axes[0, 1].set_ylabel('值')
# Q-Q图(检验正态性)
sm.qqplot(data_series, line='45', ax=axes[1, 0])
axes[1, 0].set_title('Q-Q图(正态性检验)', fontsize=14)
# 小提琴图
sns.violinplot(y=data_series, ax=axes[1, 1], color='orange')
axes[1, 1].set_title('小提琴图', fontsize=14)
axes[1, 1].set_ylabel('值')
plt.tight_layout()
plt.show()
# 正态性检验
statistic, p_value = stats.shapiro(data_series)
print(f"\n=== Shapiro-Wilk正态性检验 ===")
print(f"统计量: {statistic:.4f}, p值: {p_value:.4f}")
print(f"结论: {'不拒绝正态性假设' if p_value > 0.05 else '拒绝正态性假设'}")
```

#### ***\**\*3. 单样本t检验\*\**\***

检验样本均值是否显著不同于某个已知值:

```python
# 场景: 检验产品平均重量是否为50g
null_mean = 50  # 零假设: 总体均值 = 50
# 执行单样本t检验
t_stat, p_value = stats.ttest_1samp(data_series, null_mean)
print("=== 单样本t检验结果 ===")
print(f"零假设: 总体均值 = {null_mean}")
print(f"备择假设: 总体均值 ≠ {null_mean}")
print(f"检验统计量 t = {t_stat:.4f}")
print(f"p值 = {p_value:.4f}")
print(f"显著性水平 α = 0.05")
# 做出决策
alpha = 0.05
if p_value < alpha:
    print(f"\n决策: 拒绝零假设 (p < {alpha})")
    print("结论: 样本均值与{null_mean}存在显著差异")
else:
    print(f"\n决策: 不拒绝零假设 (p ≥ {alpha})")
    print(f"结论: 没有证据表明样本均值与{null_mean}存在差异")
# 计算置信区间
n = len(data_series)
se = data_series.std() / np.sqrt(n)
ci_lower = data_series.mean() - stats.t.ppf(1 - alpha/2, n-1) * se
ci_upper = data_series.mean() + stats.t.ppf(1 - alpha/2, n-1) * se
print(f"\n95%置信区间: [{ci_lower:.2f}, {ci_upper:.2f}]")
```

#### ***\**\*4. 双样本独立t检验\*\**\***

比较两组独立样本的均值差异:

```python
# 场景: 比较两个班级学生的考试成绩差异
np.random.seed(123)
class_A = np.random.normal(loc=75, scale=10, size=30)
class_B = np.random.normal(loc=80, scale=12, size=30)
# 描述统计对比
print("=== 班级A成绩描述统计 ===")
print(f"均值: {class_A.mean():.2f}, 标准差: {class_A.std():.2f}")
print("\n=== 班级B成绩描述统计 ===")
print(f"均值: {class_B.mean():.2f}, 标准差: {class_B.std():.2f}")
# 方差齐性检验(Levene检验)
levene_stat, levene_p = stats.levene(class_A, class_B)
print(f"\n=== Levene方差齐性检验 ===")
print(f"统计量: {levene_stat:.4f}, p值: {levene_p:.4f}")
# 根据方差齐性结果选择t检验方法
if levene_p > 0.05:
    # 方差齐性: 使用equal_var=True
    t_stat, p_value = stats.ttest_ind(class_A, class_B, equal_var=True)
    test_type = "方差齐性t检验"
else:
    # 方差不齐: 使用Welch's t检验
    t_stat, p_value = stats.ttest_ind(class_A, class_B, equal_var=False)
    test_type = "Welch's t检验(方差不齐)"
print(f"\n=== {test_type}结果 ===")
print(f"检验统计量 t = {t_stat:.4f}")
print(f"p值 = {p_value:.4f}")
# 可视化对比
plt.figure(figsize=(10, 6))
sns.boxplot(data=[class_A, class_B], palette=['lightblue', 'lightcoral'])
plt.xticks([0, 1], ['班级A', '班级B'])
plt.title('两个班级成绩分布对比', fontsize=14)
plt.ylabel('成绩')
plt.show()
```



#### ***\**\*5. 配对t检验\*\**\***

比较同一组对象在两个时间点或条件下的差异:

```python
# 场景: 某减肥计划前后体重对比
np.random.seed(456)
before = np.random.normal(loc=75, scale=8, size=20)
after = before - np.random.normal(loc=3, scale=2, size=20)  # 平均减重3kg
# 执行配对t检验
t_stat, p_value = stats.ttest_rel(before, after)
print("=== 配对t检验结果 ===")
print(f"干预前平均体重: {before.mean():.2f} kg")
print(f"干预后平均体重: {after.mean():.2f} kg")
print(f"平均变化: {after.mean() - before.mean():.2f} kg")
print(f"检验统计量 t = {t_stat:.4f}")
print(f"p值 = {p_value:.4f}")
# 可视化配对差异
plt.figure(figsize=(12, 5))
# 左图: 配对数据连线
plt.subplot(1, 2, 1)
for i in range(len(before)):
    plt.plot([1, 2], [before[i], after[i]], 'k-', alpha=0.3)
plt.scatter([1]*len(before), before, s=80, label='干预前', color='blue')
plt.scatter([2]*len(after), after, s=80, label='干预后', color='red')
plt.xticks([1, 2], ['干预前', '干预后'])
plt.ylabel('体重(kg)')
plt.title('配对数据变化')
plt.legend()
# 右图: 差异分布
differences = after - before
plt.subplot(1, 2, 2)
sns.histplot(differences, kde=True, color='purple', bins=10)
plt.axvline(x=0, color='red', linestyle='--', label='无差异线')
plt.axvline(x=differences.mean(), color='green', linestyle='--',
            label=f'平均变化: {differences.mean():.2f}')
plt.xlabel('体重变化(kg)')
plt.title('差异分布')
plt.legend()
plt.tight_layout()
plt.show()
```

#### ***\**\*6. 卡方独立性检验\*\**\***

检验两个分类变量之间是否相关:

```python
# 场景: 研究性别与购买偏好是否相关
# 创建列联表
observed = pd.DataFrame({
    '购买': [30, 45],
    '不购买': [20, 25]
}, index=['男性', '女性'])
print("=== 观测频数列联表 ===")
print(observed)
# 执行卡方独立性检验
chi2, p_value, dof, expected = stats.chi2_contingency(observed)
print(f"\n=== 卡方独立性检验结果 ===")
print(f"卡方统计量: {chi2:.4f}")
print(f"自由度: {dof}")
print(f"p值: {p_value:.4f}")
# 显示期望频数
print("\n=== 期望频数表 ===")
expected_df = pd.DataFrame(expected,
                           index=observed.index,
                           columns=observed.columns)
print(expected_df)
# 可视化列联表
plt.figure(figsize=(10, 6))
# 热力图
plt.subplot(1, 2, 1)
sns.heatmap(observed, annot=True, fmt='d', cmap='Blues', cbar=False)
plt.title('观测频数')
plt.subplot(1, 2, 2)
sns.heatmap(expected_df, annot=True, fmt='.1f', cmap='Greens', cbar=False)
plt.title('期望频数(假设独立)')
plt.tight_layout()
plt.show()
```

#### ***\**\*7. 单因素方差分析(ANOVA)\*\**\***

比较三个或以上独立组的均值差异:

```python
# 场景: 比较三种不同教学方法的学生成绩
np.random.seed(789)
method_A = np.random.normal(loc=72, scale=8, size=25)
method_B = np.random.normal(loc=78, scale=9, size=25)
method_C = np.random.normal(loc=81, scale=7, size=25)
# 执行单因素ANOVA
f_stat, p_value = stats.f_oneway(method_A, method_B, method_C)
print("=== 单因素ANOVA结果 ===")
print(f"方法A均值: {method_A.mean():.2f}")
print(f"方法B均值: {method_B.mean():.2f}")
print(f"方法C均值: {method_C.mean():.2f}")
print(f"F统计量: {f_stat:.4f}")
print(f"p值: {p_value:.4f}")
# 准备数据用于详细ANOVA分析
data_anova = pd.DataFrame({
    'score': np.concatenate([method_A, method_B, method_C]),
    'method': ['A']*25 + ['B']*25 + ['C']*25
})
# 使用statsmodels进行详细ANOVA分析
model = ols('score ~ C(method)', data=data_anova).fit()
anova_table = sm.stats.anova_lm(model, typ=2)
print("\n=== ANOVA详细表 ===")
print(anova_table)
# 事后检验(Tukey HSD)
from statsmodels.stats.multicomp import pairwise_tukeyhsd
tukey = pairwise_tukeyhsd(endog=data_anova['score'],
                          groups=data_anova['method'],
                          alpha=0.05)
print("\n=== Tukey HSD事后检验结果 ===")
print(tukey)
# 可视化
plt.figure(figsize=(12, 5))
# 箱线图
plt.subplot(1, 2, 1)
sns.boxplot(x='method', y='score', data=data_anova,
            palette=['lightblue', 'lightgreen', 'lightcoral'])
plt.title('三种方法成绩分布对比')
plt.xlabel('教学方法')
plt.ylabel('成绩')
# 均值条形图+误差棒
plt.subplot(1, 2, 2)
means = data_anova.groupby('method')['score'].mean()
stds = data_anova.groupby('method')['score'].std()
bars = plt.bar(means.index, means.values,
               yerr=stds.values,
               capsize=5,
               color=['lightblue', 'lightgreen', 'lightcoral'])
plt.title('各方法均值对比')
plt.xlabel('教学方法')
plt.ylabel('平均成绩')
# 添加数值标签
for bar, mean in zip(bars, means.values):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.5,
             f'{mean:.1f}', ha='center', va='bottom')
plt.tight_layout()
plt.show()
```

### ***\**\*完整案例分析\*\**\***

#### ***\**\*案例: 鸢尾花数据集分析\*\**\***

##### ***\**\*\*\*\*1. 问题定义\*\*\*\*\**\***

​    我们想知道: 鸢尾花的三个物种(setosa, versicolor, virginica)在花萼长度(sepal_length)上是否存在显著差异?

##### ***\**\*\*\*\*2. 数据加载与探索\*\*\*\*\**\***

```python
# 加载鸢尾花数据集
iris = sns.load_dataset('iris')
print("=== 数据集基本信息 ===")
print(f"数据形状: {iris.shape}")
print(f"\n前5行数据:")
print(iris.head())
print(f"\n物种分布:")
print(iris['species'].value_counts())
print(f"\n描述统计(按物种分组):")
print(iris.groupby('species')['sepal_length'].describe())
```

##### ***\**\*\*\*\*3. 可视化探索\*\*\*\*\**\***

```python
# 设置画布
fig, axes = plt.subplots(2, 2, figsize=(14, 10))
# 箱线图
sns.boxplot(x='species', y='sepal_length', data=iris, ax=axes[0, 0],
            palette='Set2')
axes[0, 0].set_title('不同物种花萼长度箱线图', fontsize=14)
axes[0, 0].set_xlabel('物种')
axes[0, 0].set_ylabel('花萼长度(cm)')
# 小提琴图
sns.violinplot(x='species', y='sepal_length', data=iris, ax=axes[0, 1],
               palette='Set2')
axes[0, 1].set_title('不同物种花萼长度小提琴图', fontsize=14)
axes[0, 1].set_xlabel('物种')
axes[0, 1].set_ylabel('花萼长度(cm)')
# 直方图
for i, species in enumerate(['setosa', 'versicolor', 'virginica']):
    axes[1, 0].hist(iris[iris['species']==species]['sepal_length'],
                    alpha=0.7, label=species, bins=15)
axes[1, 0].set_title('花萼长度分布直方图', fontsize=14)
axes[1, 0].set_xlabel('花萼长度(cm)')
axes[1, 0].set_ylabel('频数')
axes[1, 0].legend()
# Q-Q图(检验setosa的正态性)
setosa_data = iris[iris['species']=='setosa']['sepal_length']
sm.qqplot(setosa_data, line='45', ax=axes[1, 1])
axes[1, 1].set_title('Setosa花萼长度Q-Q图', fontsize=14)
plt.tight_layout()
plt.show()
```

##### ***\**\*\*\*\*4. 正态性检验\*\*\*\*\**\***

```python
print("=== 各物种花萼长度正态性检验(Shapiro-Wilk) ===")
for species in iris['species'].unique():
    data = iris[iris['species']==species]['sepal_length']
    stat, p = stats.shapiro(data)
    print(f"{species}: 统计量={stat:.4f}, p值={p:.4f}, "
          f"{'满足正态性' if p > 0.05 else '不满足正态性'}")
```

##### ***\**\*\*\*\*5. 方差齐性检验\*\*\*\*\**\***

```python
from scipy.stats import levene
setosa = iris[iris['species']=='setosa']['sepal_length']
versicolor = iris[iris['species']=='versicolor']['sepal_length']
virginica = iris[iris['species']=='virginica']['sepal_length']
stat, p = levene(setosa, versicolor, virginica)
print(f"\n=== Levene方差齐性检验 ===")
print(f"统计量: {stat:.4f}, p值: {p:.4f}")
print(f"结论: {'满足方差齐性' if p > 0.05 else '方差不齐'}")
```

##### ***\**\*\*\*\*6. 执行单因素ANOVA\*\*\*\*\**\***

```python
# 方法1: 使用scipy
f_stat, p_value = stats.f_oneway(setosa, versicolor, virginica)
print("=== 单因素ANOVA结果(scipy) ===")
print(f"F统计量: {f_stat:.4f}")
print(f"p值: {p_value:.4f}")
# 方法2: 使用statsmodels(更详细)
model = ols('sepal_length ~ C(species)', data=iris).fit()
anova_table = sm.stats.anova_lm(model, typ=2)
print("\n=== ANOVA详细表(statsmodels) ===")
print(anova_table)
# 计算效应量(eta平方)
ss_total = anova_table['sum_sq'].sum()
ss_between = anova_table.loc['C(species)', 'sum_sq']
eta_squared = ss_between / ss_total
print(f"\n=== 效应量 ===")
print(f"eta平方 = {eta_squared:.4f}")
print(f"解释: 物种可以解释花萼长度变异的{eta_squared*100:.2f}%")
```

##### ***\**\*\*\*\*7. 事后多重比较\*\*\*\*\**\***

```python
# Tukey HSD事后检验
tukey = pairwise_tukeyhsd(endog=iris['sepal_length'],
                          groups=iris['species'],
                          alpha=0.05)
print("=== Tukey HSD事后检验结果 ===")
print(tukey)
# 可视化Tukey结果
from statsmodels.stats.multicomp import MultiComparison
mc = MultiComparison(iris['sepal_length'], iris['species'])
result = mc.tukeyhsd()
result.plot_simultaneous(comparison_name='setosa',
                        xlabel='花萼长度均值差异',
                        title='95%置信区间差异图')
plt.show()
```

##### ***\**\*\*\*\*8. 结论解读\*\*\*\*\**\***

**统计意义:**

​    • ANOVA的p值 < 0.001, 远小于显著性水平0.05

​    • 拒绝零假设,说明至少有两个物种的花萼长度均值存在显著差异

**实际意义:**

​    • eta平方 = 0.618, 物种可以解释花萼长度61.8%的变异

​    • 这是一个非常大的效应量,说明物种是影响花萼长度的关键因素

**具体差异:**

​    • Tukey HSD检验显示,三组两两之间的差异都具有统计显著性

​    • virginica的花萼最长(均值约6.59cm), setosa最短(均值约5.01cm)

​    • 这与生物学认知一致,可以用于物种识别

### ***\**\*总结与经验分享\*\**\***

#### ***\**\*1. 统计分析完整流程\*\**\***

```python
数据收集 → 数据清洗 → 探索性分析(可视化+描述统计) →
正态性检验 → 方差齐性检验 → 选择合适检验方法 →
执行检验 → 结果解释 → 业务决策
```

#### ***\**\*2. 常见误区与避坑指南\*\**\***

##### ***\**\*\*\*\*误区1: 盲目使用t检验,不检查前提条件\*\*\*\*\**\***

​    • **问题**: 直接对偏态分布或异常值多的数据做t检验

​    • **建议**: 先做正态性检验和可视化,不满足时考虑非参数检验(Mann-Whitney U等)

##### ***\**\*\*\*\*误区2: 混淆统计显著性和实际意义\*\*\*\*\**\***

​    • **问题**: 大样本下微小的差异也会统计显著(p<0.05),但实际价值不大

​    • **建议**: 关注效应量(如Cohen's d, eta平方),不只看p值

##### ***\**\*\*\*\*误区3: 多重比较未做校正\*\*\*\*\**\***

​    • **问题**: 同时做多组比较会增加第一类错误概率

​    • **建议**: 使用Bonferroni校正或Tukey HSD等方法

##### ***\**\*\*\*\*误区4: p<0.05就是"显著"吗?\*\*\*\*\**\***

​    • **问题**: 将0.05作为绝对门槛,忽视p值本身的大小

​    • **建议**: 报告精确的p值,结合研究背景判断

##### ***\**\*\*\*\*误区5: 忽视置信区间\*\*\*\*\**\***

​    • **问题**: 只关注点估计(如均值),忽视估计的不确定性

​    • **建议**: 报告95%置信区间,给出更全面的信息

#### ***\**\*3. 假设检验方法选择决策树\*\**\***

```python
开始
│
├─ 数据类型?
│   ├─ 分类变量 → 卡方独立性检验 / Fisher精确检验
│   └─ 连续变量 → 继续下一步
│
├─ 比较几组?
│   ├─ 1组 → 单样本t检验 / Wilcoxon符号秩检验
│   ├─ 2组 → 继续下一步
│   └─ ≥3组 → ANOVA / Kruskal-Wallis检验
│
├─ 两组是否独立?
│   ├─ 独立 → 独立双样本t检验 / Mann-Whitney U检验
│   └─ 配对 → 配对t检验 / Wilcoxon符号秩检验
│
└─ 正态性检验
    ├─ 满足正态性 → 参数检验(t检验/ANOVA)
    └─ 不满足正态性 → 非参数检验
```

#### ***\**\*4. 实际项目经验技巧\*\**\***

##### ***\**\*\*\*\*技巧1: 始终设置随机种子\*\*\*\*\**\***

```python
np.random.seed(42)  # 确保结果可复现
```

##### ***\**\*\*\*\*技巧2: 可视化先行,后做检验\*\*\*\*\**\***

​    • 先用箱线图、直方图、Q-Q图等了解数据分布

​    • 异常值和非正态在可视化中一目了然

##### ***\**\*\*\*\*技巧3: 报告完整结果\*\*\*\*\**\***

​    不要只说"有显著性差异",应包含:

​    • 检验方法名称

​    • 检验统计量值

​    • 精确的p值

​    • 效应量

​    • 95%置信区间

##### ***\**\*\*\*\*技巧4: 注意样本量\*\*\*\*\**\***

​    • 样本量太小(n<30): 慎用参数检验,考虑非参数检验

​    • 样本量太大(n>1000): 关注效应量,不要被微小差异误导

##### ***\**\*\*\*\*技巧5: 代码复用与封装\*\*\*\*\**\***

将常用检验封装成函数:

```python
def compare_two_groups(data1, data2, group1_name, group2_name):
    """比较两组数据,输出完整分析报告"""
    # 描述统计
    print(f"=== {group1_name} vs {group2_name} ===")
    print(f"{group1_name}: n={len(data1)}, 均值={data1.mean():.2f}, 标准差={data1.std():.2f}")
    print(f"{group2_name}: n={len(data2)}, 均值={data2.mean():.2f}, 标准差={data2.std():.2f}")
    # 正态性检验
    _, p1 = stats.shapiro(data1)
    _, p2 = stats.shapiro(data2)
    normal = (p1 > 0.05) and (p2 > 0.05)
    print(f"正态性: {group1_name}(p={p1:.3f}), {group2_name}(p={p2:.3f})")
    # 方差齐性检验
    _, levene_p = stats.levene(data1, data2)
    equal_var = levene_p > 0.05
    print(f"方差齐性: p={levene_p:.3f}")
    # 选择检验方法
    if normal:
        if equal_var:
            t_stat, p_value = stats.ttest_ind(data1, data2, equal_var=True)
            method = "独立双样本t检验"
        else:
            t_stat, p_value = stats.ttest_ind(data1, data2, equal_var=False)
            method = "Welch's t检验"
    else:
        t_stat, p_value = stats.mannwhitneyu(data1, data2)
        method = "Mann-Whitney U检验"
    print(f"\n检验方法: {method}")
    print(f"统计量: {t_stat:.4f}")
    print(f"p值: {p_value:.4f}")
    # 效应量
    if normal:
        pooled_std = np.sqrt(((len(data1)-1)*data1.std()**2 +
                            (len(data2)-1)*data2.std()**2) /
                            (len(data1)+len(data2)-2))
        cohens_d = (data1.mean() - data2.mean()) / pooled_std
        print(f"Cohen's d: {cohens_d:.4f}")
    return p_value
# 使用示例
compare_two_groups(method_A, method_B, "方法A", "方法B")
```

**结语**

​    统计分析与假设检验是数据科学工作者的必备技能。希望通过本文的学习,你不仅掌握了各种检验方法的实现代码,更重要的是理解了它们的适用场景和背后的统计原理。记住:工具是手段,解决实际问题才是目的。在真实项目中,始终结合业务背景来解释统计结果,才能让数据分析真正产生价值。

**欢迎在评论区交流学习心得,也欢迎分享你在项目中遇到的统计分析问题!**

​    *本文代码在Python 3.8+环境下测试通过,主要依赖库版本:*

*NumPy 1.21+, Pandas 1.3+, SciPy 1.7+, Statsmodels 0.13+, Matplotlib 3.4+, Seaborn 0.11+*