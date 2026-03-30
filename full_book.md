# 因果推断：献给求真敢为者

> **原书名**：Causal Inference for the Brave and True  
> **原作者**：Matheus Facure  
> **中文翻译**：许文立 & 黄文喆（澳门城市大学金融学院）  
> **来源**：<https://github.com/matheusfacure/python-causality-handbook>  
> **版本日期**：2026-03-30  
> **版权**：© 2020 Matheus Facure（原著）；中文翻译版权归译者所有

---

## 目录

- [因果推断：献给求真敢为者](#因果推断献给求真敢为者)

**第一部分 - 阳**

- [01 - 因果性导论](#01---因果性导论)
- [02 - 随机实验](#02---随机实验)
- [03 - 统计回顾：最危险的方程](#03---统计回顾最危险的方程)
- [04 - 图形因果模型](#04---图形因果模型)
- [05 - 线性回归的惊人有效性](#05---线性回归的惊人有效性)
- [06 - 分组与虚拟变量回归](#06---分组与虚拟变量回归)
- [07 - 超越混杂因素](#07---超越混杂因素)
- [08 - 工具变量](#08---工具变量)
- [09 - 不依从性与局部平均处理效应](#09---不依从性与局部平均处理效应)
- [10 - 匹配法](#10---匹配法)
- [11 - 倾向得分](#11---倾向得分)
- [12 - 双重稳健估计](#12---双重稳健估计)
- [13 - 双重差分法](#13---双重差分法)
- [14 - 面板数据与固定效应](#14---面板数据与固定效应)
- [15 - 合成控制法](#15---合成控制法)
- [16 - 断点回归设计](#16---断点回归设计)

**第二部分 - 阴**

- [17 - 预测模型入门](#17---预测模型入门)
- [18 - 异质性处理效应与个性化](#18---异质性处理效应与个性化)
- [19 - 评估因果模型](#19---评估因果模型)
- [20 - 即插即用估计器](#20---即插即用估计器)
- [21 - 元学习器](#21---元学习器)
- [22 - 去偏/正交机器学习](#22---去偏正交机器学习)
- [23 - 处理效应异质性和非线性的挑战](#23---处理效应异质性和非线性的挑战)
- [24 - 双重差分传奇](#24---双重差分传奇)
- [25 - 合成双重差分](#25---合成双重差分)

**附录**

- [Debiasing with Orthogonalization](#debiasing-with-orthogonalization)
- [Debiasing with Propensity Score](#debiasing-with-propensity-score)
- [When Prediction Fails](#when-prediction-fails)
- [Why Prediction Metrics are Dangerous For Causal Models](#why-prediction-metrics-are-dangerous-for-causal-models)
- [Conformal Inference for Synthetic Controls](#conformal-inference-for-synthetic-controls)

---
# 因果推断：献给求真敢为者

![brave-and-true](images/brave-and-true.png)

## 中文版说明
> 原书名：[**Causal Inference for the Brave and True**](https://github.com/matheusfacure/python-causality-handbook)  
> 作者：Matheus Facure  
> 版权所有 © 2020 Matheus Facure
> 
> 本中文版由澳门城市大学金融学院的  
> **许文立**与**黄文喆**合作完成，作为课程学习的一部分。
> 
> 本仓库包含本书中文版的全部译文、示例数据与配套源码，供教学与研究使用。  
> 在版权及开源协议允许范围内，我们进行了翻译、整理与本地化扩展。  
> 原文版权及MIT协议约束内容仍归原作者所有，中文版翻译与Stata代码版权归译者所有。  
> 
> | 仓库 | 作者 | 资源链接 |
> |------|------|----------|
> | 示例数据与译文 | 黄文喆、许文立 | [GitHub 仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh) |
> | Stata代码与Slides | 许文立、黄文喆 | [GitHub 仓库](https://github.com/wenddymacro/stata-causality-handbook-zh) |
> | 内地镜像 | 许文立、黄文喆 | [文件夹](https://ccn1gnd5qat1.feishu.cn/drive/folder/RZD8fSq1Tlha8rdYs33cDuCln9g) ｜ [压缩包](https://ccn1gnd5qat1.feishu.cn/drive/folder/Ou1VflSC1l2ExmdB3GgcNE7nned) |
> 
> **联系方式：**  
> 黄文喆（澳门城市大学金融学院）｜[carlzhe@outlook.com](mailto:carlzhe@outlook.com)  
> 许文立（澳门城市大学金融学院）｜[wlxu@cityu.edu.mo](mailto:wlxu@cityu.edu.mo) 

## 阅读指南

本书内容分为两大部分，取《易经》“阴阳”之意：

- **第一部分 - 阳**  
  代表阳面，象征因果推断中已被广泛接受、逻辑清晰、理论成熟的计量方法。  
  如果你希望系统入门，理解回归、实验设计、工具变量等核心概念，这是最好的起点。

- **第二部分 - 阴**  
  代表阴面，象征尚在发展中的实践方法，强调预测、个性化估计与机器学习的融合应用。  
  内容更具实验性，适合对新方法、新方向有探索兴趣的读者。

---

以下为原作者 Matheus Facure 所撰内容，由译者翻译整理。

## 序言

这是一本用轻松诙谐但又不失严谨的方式学习效应估计与敏感性分析的书。全程使用 Python，并尽可能加入了我找到的各种梗图。

本书第一部分涵盖了因果推断的核心概念与模型。您将学习如何用潜在结果框架表示因果问题，了解因果图、偏误及其应对方法。这部分内容大多已得到广泛认可，是我从书籍、大学课程及在线教育资源中提炼整合而成。可将第一部分视作您探索因果问题的坚实安全基础。

第二部分（进行中）聚焦因果推断在（主要是科技）行业中的现代发展与应用。第一部分主要关注平均处理效应的识别，而第二部分则转向个性化及使用 CATE 模型进行异质性效应估计。这部分内容多源于个人经验，远非成熟科学理论，更具实验性且可能随时调整——毕竟，我也仍在学习过程中。

## 致谢
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 第一部分 - 阳


---

# 01 - 因果性导论


## 为什么要学？

首先，你可能在想：这对我有什么好处？答案如下：

## 数据科学今非昔比（或终成其是）

数据科学家被《哈佛商业评论》誉为[21 世纪最性感的职业](https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century)。这绝非虚言。十年来，数据科学家一直处于聚光灯下。人工智能专家的[薪资堪比体育巨星](https://www.economist.com/business/2016/04/02/million-dollar-babies)。在追逐名利的过程中，数百名年轻专业人士疯狂投身于这场淘金热，只为尽快获得数据科学家的头衔。围绕这股热潮，全新的产业如雨后春笋般涌现。神奇的教学方法声称无需接触数学公式就能让你成为数据科学家。咨询专家们承诺，只要企业能挖掘数据潜力，就能收获百万财富。人工智能或机器学习被称为新时代的电力，而数据则是新石油。

在这一切发生的同时，经济学家们试图解答教育对个人收入的真实影响，生物统计学家们努力探究饱和脂肪是否会增加心脏病发作的概率，心理学家则致力于验证肯定的言语是否真能带来更美满的婚姻。而我们似乎忽略了一点：其实早就有人一直在用“老派”的方式进行数据科学研究。说实话，数据科学并不是一个新领域。我们之所以现在觉得它“新”，只是因为媒体的大肆宣传才让我们如此关注它。

借用 Jim Collins 的一个比喻：想象你正往杯中倒一杯你最喜欢的冰啤酒。若操作得当，杯中绝大部分是酒液，顶部仅浮着一指厚的泡沫层。这杯啤酒恰似数据科学。

1. 酒液象征着数据科学的本质——统计学基础、科学探索精神以及对复杂问题的热忱。数百年来，这些核心要素已被证明具有不可估量的价值。
2. 而泡沫则代表那些建立在虚幻预期之上的浮华之物，终将消散无踪。

而这层泡沫破灭的速度可能超乎你的想象。正如《The Economist》所言

> 那些预测人工智能将改变世界的咨询公司，同时也报告称，现实中的企业管理者发现 AI 很难真正落地，市场的热情也正在逐渐降温。研究公司高德纳（Gartner）的斯维特兰娜·西库拉（Svetlana Sicular）表示，2020 年可能是 AI 掉进该公司著名的“炒作周期”下坡阶段的一年。投资者也开始对“随波逐流”的现象保持警惕：风险投资基金 MMC 对欧洲 AI 创业公司的调查发现，有 40% 的公司似乎根本没有在使用任何 AI 技术。

作为数据科学家——或者更准确地说，作为"纯粹"的科学家——我们在这场狂热中应当如何自处？明智之举是首先学会忽略这些泡沫。我们追求的是实质内容。数学与统计学的价值亘古长青，现在也不会例外。其次，要认清真正赋予你工作价值的是核心能力，而非那些尚未被掌握的最新炫目工具。

最后但同样重要的是，请记住学习没有捷径可走。数学与统计学的知识之所以珍贵，恰恰在于其习得之艰难。倘若人人皆能轻易掌握，供过于求必将使其价值贬损。因此，**振作起来**！竭尽所能学好它们。更何况，何乐而不为呢？让我们在这场只属于 **“求真敢为者”** 的旅程中，尽情享受沿途的乐趣吧。

![img](./images/01/tougher-up-cupcake1.jpg)

## 回答另一类的问题

当前的机器学习在预测类问题上表现非常出色。正如 Ajay Agrawal、Joshua Gans 和 Avi Goldfarb 在《Prediction Machines》一书中所说的那样：“新一代人工智能并未真正带来‘智能’，而是赋予了我们智能的关键组成部分——预测能力。”我们可以用机器学习做出各种惊艳的应用，但前提是——我们必须将问题表述成一个预测问题：

 - 想实现英语到中文的翻译吗？那就构建一个机器学习模型，在输入英语句子时预测对应的中文句子。
 - 想要识别人脸？那就创建一个机器学习模型，预测图片某部分是否存在人脸。
 - 想打造一辆自动驾驶汽车？那就用一个机器学习模型，根据车辆周围图像和传感器数据，预测方向盘转向角度及刹车与油门的压力。

然而，机器学习并非万能良药。在严格界定的范围内，它能创造奇迹；但若数据稍有偏离模型所适应的范围，它也可能惨败。再举《Prediction Machines》中的一个例子：“在许多行业中，低价常与低销量相关联。以酒店业为例，旅游淡季时价格低廉，而需求高峰、酒店爆满时价格则居高不下。基于此类数据，一个天真的预测可能会认为提高价格将带来更多客房销售。”

机器学习在处理这类逆向因果关系的问题上表现尤为糟糕。它们要求我们回答“假设”性问题，经济学家称之为反事实推理。例如：

 - 如果我现在为商品标价时采用另一个价格，结果会怎样？  
 - 如果我选择低糖饮食而非目前的低脂饮食，又会发生什么？
 - 如果我在银行工作，放贷时改变客户筛选标准，会对收益有何影响？
 - 如果我是地方政府官员，怎样才能改进教育体系？是要每个孩子配备一台平板？还是应该建设一座传统图书馆？

这些问题背后，都隐藏着我们真正想要解答的因果问题。因果问题不仅存在于商业决策中（如如何提高销量），也贯穿于我们每个人关心的生活命题：

- 上名校是否真的有助于我获得更高收入？（教育是否导致更高的收入？）
- 移民是否会影响我找工作的机会？（移民是否导致失业率上升？）
- 给穷人发放现金是否能降低犯罪率？

无论身处何种领域，你很可能已经或终将需要回答某种类型的因果问题。遗憾的是，对机器学习而言，我们无法依赖相关性预测来解决这类问题。回答这类问题比多数人想象的更具挑战性。你的父母可能反复告诫你"相关不等于因果"，"相关不等于因果"。但事实上，解释其中缘由需要更深入的探讨。

而这，正是本书开篇要探讨的主题。至于本书接下来的章节，将致力于探索如何使相关转化为因果。

## 当“相关性”就是“因果性”

从直觉上讲，我们其实大致知道为什么相关性不能等同于因果性。

如果有人告诉你：“那些给学生配发平板的学校，表现普遍比没配平板的学校更好”，你可能很快就会反驳：有可能这些发得起平板的学校本身就更富裕。

因此，即使不发平板，他们的学生可能也比平均水平表现更好。

正因如此，我们不能断定“在课堂上给学生发平板会提高学习成绩”；我们只能说“发平板”这件事和“更高的成绩”在数据上存在相关性。比如在巴西，可以通过 ENEM 成绩来衡量：

```{dropdown} 查看 Stata 代码
```stata
clear
set obs 100
set seed 123

* 1. Generate tuition data (normal distribution)
gen tuition = round(rnormal(1000, 300))

* 2. Standardize tuition and calculate Logistic probability
sum tuition
gen z = (tuition - r(mean)) / r(sd)
gen p = invlogit(z)  // p = expit(z)

* 3. Generate tablet variable (binomial distribution)
gen tablet = runiform() < p  // Method 1
* Alternative: gen tablet = rbinomial(1, p)  // Method 2 (Stata 14+)

* 1. Define value labels
label define tablet_label 0 "False" 1 "True"

* 2. Apply labels to variable
label values tablet tablet_label

* 3. Check results (verify labels are applied)
tab tablet

* 4. Generate ENEM scores (with tablet and tuition effects)
gen enem_score = rnormal(200 - 50 * tablet + 0.7 * tuition, 200)

* 5. Standardize scores to 0-1000 range
sum enem_score
replace enem_score = (enem_score - r(min)) / (r(max) - r(min)) * 1000
* Drop observations where enem_score equals 0
drop if enem_score == 0

* Check results
tab tablet
sum enem_score tuition, detail

```python
import pandas as pd
import numpy as np
from scipy.special import expit
import seaborn as sns
from matplotlib import pyplot as plt
from matplotlib import style

style.use("fivethirtyeight")

np.random.seed(123)
n = 100
tuition = np.random.normal(1000, 300, n).round()
tablet = np.random.binomial(1, expit((tuition - tuition.mean()) / tuition.std())).astype(bool)
enem_score = np.random.normal(200 - 50 * tablet + 0.7 * tuition, 200)
enem_score = (enem_score - enem_score.min()) / enem_score.max()
enem_score *= 1000

data = pd.DataFrame(dict(enem_score=enem_score, Tuition=tuition, Tablet=tablet))
```

```{dropdown} 查看 Stata 代码
```stata
graph box enem_score, over(tablet) ///
    title("ENEM score by Tablet in Class") ///
    ytitle("ENEM Score") ///
    box(1, color(blue)) ///
    box(2, color(orange)) ///
    graphregion(color(white)) ///
    plotregion(color(white))


* Create scatterplot with different colors for Tablet groups
twoway (scatter enem_score tuition if tablet == 1, mcolor(blue) msymbol(Oh) msize(medlarge)) ///
       (scatter enem_score tuition if tablet == 0, mcolor(red) msymbol(X) msize(medlarge)), ///
       title("ENEM score by Tuition Cost") ///
       ytitle("ENEM Score") ///
       xtitle("Tuition Cost") ///
       legend(order(1 "Tablet: True" 2 "Tablet: False") ///
              position(11) ring(0) cols(1)) ///  
       graphregion(color(white)) ///
       plotregion(color(white)) ///
       xsize(10) ysize(6)

```python
plt.figure(figsize=(6,8))
sns.boxplot(y="enem_score", x="Tablet", data=data).set_title('ENEM score by Tablet in Class')
plt.show()
```

为了超越简单的直觉，我们首先建立一些符号体系。这将是我们讨论因果关系的日常语言——把它当作我们这些“求真敢为者”的通用语，在未来一次次关于因果推断的战斗中，我们会用这套语言彼此辨识、并高声呐喊。

我们把 $T_i$ 定义为为单位 i 是否接受处理（treatment）的变量。

$
T_i=\begin{cases}
1 \ \text{如果单位i接受了处理}\\
0 \ \text{否则}\\
\end{cases}
$

此处所说的“处理”指的是我们希望探究其效果的某种干预措施的术语。在本例中，该处理即为向学生发放平板电脑。（注：有时你也可能会看到用$D$来表示处理，而不是$T$。）

接下来，我们用 $Y_i$ 表示单元 i 的观测结果变量。

这个结果变量就是我们关注的核心。我们希望了解的是：处理是否对结果有影响。以“平板教学”为例，这个结果就是学生的学业表现。

这时，问题开始变得有趣起来。因果推断的根本问题在于，**我们永远无法同时观察同一单位接受处理与未接受处理的两种状态下的结果**。这就像面前有两条分岔的道路，而我们只能知晓所选之路的前方风景。正如罗伯特·弗罗斯特（Robert Frost）在他的诗中所写：

> 两条小路分岔在金黄的树林中，  
可惜我不能同时踏上两条，  
我站了很久，  
沿着其中一条望去，直到它在林中转弯，消失不见。  


为了真正理解这个问题，我们将频繁使用 **潜在结果**这一概念进行讨论。

它们之所以称为潜在，是因为它们实际上并未发生，而是代表了在采取某种干预措施后 **可能发生的情况**。我们有时将实际发生的潜在结果称为事实性结果，而未发生的则称为反事实结果。

关于符号，我们引入一个额外的下标：

$Y_{0i}$ 表示未接受处理时单元 i 的潜在结果。

$Y_{1i}$ 表示 **同一单元 i**在接受处理后的潜在结果。

有时潜在结果可能以函数形式表示为 $Y_i(t)$，需注意区分。 $Y_{0i}$ 可能是 $Y_i(0)$ ，而 $Y_{1i}$ 可能是 $Y_i(1)$。本文多数情况下将采用下标表示法。

![img](./images/01/potential_outcomes.png)

回到我们的例子，

如果学生 i 所在的教室使用了平板电脑，那么$Y_{1i}$就表示他的学业表现。无论实际情况如何，这对$Y_{1i}$并无影响，结果始终相同。如果学生 i 确实拿到了平板，我们就能观测到$Y_{1i}$；如果没有拿到，我们观测到的是$Y_{0i}$。

请注意：即便我们没有观测到$Y_{1i}$，它依然是“存在”的——只是我们无法看到。这种我们看不到但理论上存在的潜在结果，就被称为“反事实潜在结果”（counterfactual potential outcome）。

在定义了潜在结果之后，我们就可以写出个体处理效应：

 $Y_{1i} - Y_{0i}$
 
当然，由于“因果推断的基本难题”（只能观测其中一个潜在结果），我们永远无法直接知道某个个体的处理效应。所以，与其去估计每个个体的因果效应，不如转而去估计一个更容易的量：**平均处理效应（Average Treatment Effect, ATE）**：

$ATE = E[Y_1 - Y_0]$

其中， `E[...]` 表示期望值。另一个更易估计的量是**对已接受处理者的平均处理效应（Average Treatment Effect on the Treated, ATT）**：

$ATT = E[Y_1 - Y_0 | T=1]$

虽然我们无法同时看到两种潜在结果，但为了讨论方便，假设我们能够做到。假设因果推断之神被我们在统计战役中英勇奋斗的精神所感动，赐予我们如神般洞察潜在替代结果的能力。凭借此力，假设我们收集了 4 所学校的数据，知晓它们是否向学生提供平板电脑及其在年度学术测试中的成绩。此处，平板电脑即处理变量，故 $T=1$ 表示学校向学生提供平板电脑，$Y$ 则为测试成绩。

```{dropdown} 查看 Stata 代码
```stata
clear

* Create the dataset
input i Y0 Y1 T Y TE
1 500 450 0 500 -50
2 600 600 0 600 0
3 800 600 1 600 -200
4 700 750 1 750 50
end

* Label the variables
label variable i "Observation ID"
label variable Y0 "Outcome without treatment"
label variable Y1 "Outcome with treatment"
label variable T "Treatment status"
label variable Y "Observed outcome"
label variable TE "Treatment effect"

* Display the data
list

```python
pd.DataFrame(dict(
    i= [1,2,3,4],
    Y0=[500,600,800,700],
    Y1=[450,600,600,750],
    T= [0,0,1,1],
    Y= [500,600,600,750],
    TE=[-50,0,-200,50],
))
```

这里的 $ATE$ 就是最后一列的平均值，即处理效应的平均值：

$ATE=(-50 + 0 - 200 + 50)/4 = -50$

这意味着平板电脑平均使学生的学业成绩降低了50分。此处的 $ATT$ 将是当 $T=1$ 时最后一列的平均值:

$ATT=(- 200 + 50)/2 = -75$

这表明，对于接受处理的学校，平板电脑平均使学生的学业成绩降低了 75 分。当然，我们永远无法确知这一点。实际上，上表将呈现如下情况：

```{dropdown} 查看 Stata 代码
```stata
clear

* Create the dataset with missing values (.)
input i Y0 Y1 T Y TE
1 500 .  0 500 .
2 600 .  0 600 .
3 .  600 1 600 .
4 .  750 1 750 .
end

* Label the variables
label variable i "Observation ID"
label variable Y0 "Outcome without treatment (missing if treated)"
label variable Y1 "Outcome with treatment (missing if control)" 
label variable T "Treatment status"
label variable Y "Observed outcome"
label variable TE "Treatment effect (missing for all)"

* Format missing value display
format Y0 Y1 TE %8.0g  // Shows . for missing values

* Display the data
list

```python
pd.DataFrame(dict(
    i= [1,2,3,4],
    Y0=[500,600,np.nan,np.nan],
    Y1=[np.nan,np.nan,600,750],
    T= [0,0,1,1],
    Y= [500,600,600,750],
    TE=[np.nan,np.nan,np.nan,np.nan],
))
```

你或许会说，这显然不够理想，但我难道不能计算处理组的均值并与未处理组的均值进行比较吗？换言之，难道不能直接进行 $ATE=(600+750)/2  - (500 + 600)/2  = 125$ 操作吗？答案是不行！请注意结果差异之大。你已犯下将相关误认为因果的最严重错误。要理解其中缘由，让我们深入探究因果推断的主要敌人。

## 偏误

偏误正是使“相关性 ≠ 因果性”的关键原因。

幸运的是，凭借直觉我们就能轻松理解它。让我们回顾课堂平板电脑的案例。当面对"提供平板电脑的学校学生成绩更优异"这一论断时，我们可以反驳说，这些学校即便没有平板电脑，成绩可能依然更高。因为它们很可能比其他学校资金更充裕，从而能聘请更优秀的教师、提供更好的教室设施等。换言之，问题在于接受处理（配备平板电脑）的学校与未接受处理的学校根本不具备可比性。

如果我们用“潜在结果”的符号来表达这一点：**接受处理的学校的 $Y_0$**（即如果它们没发平板时的成绩）与 **未接受处理学校的$Y_0$**（真实的无平板成绩）是不同的。要记住，处理组的 $Y_0$ 是反事实（counterfactual），我们无法观察到它，只能推理它。在这个特定案例中，我们可以依靠现实经验进一步推理。我们有理由相信，处理组的$Y_0$本来就比未处理组的$Y_0$更高。因为能给学生配平板的学校，也往往有其他资源，这些都会促成更高的成绩。让我们停下来消化一下这点，理解“潜在结果”概念需要时间。请重读本段并确保理解其含义。

有了这个基础，我们可以用一个基本的数学公式来展示为何相关不等于因果。相关性的计算方式是 $E[Y|T=1] - E[Y|T=0]$，在我们的例子中，这表示拥有平板电脑的学校平均测试分数减去没有平板电脑的学校平均测试分数。而因果关系的正确计算方式是 $E[Y_1 - Y_0]$。

我们现在来分析“相关性指标”，并将其中的观测结果替换成潜在结果，以探究它们之间是如何联系的。对于接受处理的个体，观察到的结果是 $Y_1$；而对于未接受处理的个体，观察到的结果则是 $Y_0$。

$
E[Y|T=1] - E[Y|T=0] = E[Y_1|T=1] - E[Y_0|T=0]
$

现在，让我们对 $E[Y_0|T=1]$进行加减运算。这是一个反事实结果，它揭示了如果接受处理的个体未接受处理，其原本可能的结果会是什么。

$
E[Y|T=1] - E[Y|T=0] = E[Y_1|T=1] - E[Y_0|T=0] + E[Y_0|T=1] - E[Y_0|T=1]
$

最后，我们重新排列各项，合并部分期望值，于是乎结果显而易见：

$
E[Y|T=1] - E[Y|T=0] = \underbrace{E[Y_1 - Y_0|T=1]}_{ATT} + \underbrace{\{ E[Y_0|T=1] - E[Y_0|T=0] \}}_{BIAS}
$

这段简单的数学涵盖了我们在因果问题中遇到的所有难题。我无法强调你理解它的每一个方面有多么重要。如果你被迫要在手臂上纹点什么，这个方程应该是个不错的选择。它值得你珍视并理解其传达的信息，就像可以百般解读的圣典。事实上，让我们更深入地探讨一下。我们将其分解，看看其中的一些含义。首先，这个方程说明了为什么相关不等于因果。如我们所见，相关性等于处理组效应加上一个偏误项。偏误来源于处理组和对照组在处理前就存在的差异，假设两者均未接受处理。现在，当有人告诉我们课堂上的平板电脑能提高学业成绩时，我们可以明确指出为何持怀疑态度。我们认为，在这个例子中，$E[Y_0|T=0] < E[Y_0|T=1]$，即那些有能力为孩子提供平板电脑的学校本身就比无力提供的学校更好，与是否使用平板电脑无关。

为何会出现这种情况？待我们深入讨论混杂因素时将进一步阐述，但此刻你可以理解为偏误的产生源于许多我们无法控制的因素与处理同时发生变化。因此，接受与未接受平板电脑教学的学校不仅在设备配置上存在差异，还在学费成本、地理位置、师资力量等方面有所不同。要断言课堂平板电脑能提升学业表现，我们需要确保这两类学校在其他条件上平均而言是相似的。

```python
plt.figure(figsize=(10,6))
sns.scatterplot(x="Tuition", y="enem_score", hue="Tablet", data=data, s=70).set_title('ENEM score by Tuition Cost')
plt.show()
```

既然我们已经理解了问题所在，现在让我们来看看解决方案。我们还可以探讨要使相关等同于因果所需满足的条件。**若 $E[Y_0|T=0] = E[Y_0|T=1]$，那么相关即等同于因果！** 理解这一点不仅仅是记住这个等式，其背后还有着强有力的直观论证。称 $E[Y_0|T=0] = E[Y_0|T=1]$ 意味着处理组与对照组在处理前具有可比性。或者说，当被处理者未接受处理时，若能观测到其 $Y_0$，其结果将与未处理者相同。从数学上看，偏误项将因此消失：

$
E[Y|T=1] - E[Y|T=0] = E[Y_1 - Y_0|T=1] = ATT
$

此外，若处理组与未处理组仅在处理本身存在差异，则 $E[Y_0|T=0] = E[Y_0|T=1]$，此时处理组所受的因果影响与未处理组相同（因为两者极为相似）。

$
\begin{align}
E[Y_1 - Y_0|T=1] &= E[Y_1|T=1] - E[Y_0|T=1] \\
&= E[Y_1|T=1] - E[Y_0|T=0] \\
&= E[Y|T=1] - E[Y|T=0]
\end{align}
$

在此情况下，**均值差异即成为因果效应**：

$
E[Y|T=1] - E[Y|T=0] = ATT
$

此外，若处理组与未处理组仅在干预措施本身存在差异，我们同样满足 $E[Y_1|T=0] = E[Y_1|T=1]$条件，即确保两组对干预的反应相似。此时，处理组与未处理组不仅在干预前可互换，干预后亦保持可互换性。这种情况下， $E[Y_1 - Y_0|T=1]=E[Y_1 - Y_0|T=0]$ 且
 
$
E[Y|T=1] - E[Y|T=0] = ATT = ATE
$

鉴于其重要性，我认为有必要通过直观图示再次阐释。若对处理组与未处理组进行简单均值比较（蓝点代表未接受干预组，即未使用平板电脑），结果如下：

![img](./images/01/anatomy1.png)

需注意两组结果差异可能源于两个因素：

1. 处理效果：考试成绩的提高源于给孩子们提供了平板电脑。
2. 测试成绩的部分差异可归因于优质教育的高昂学费。在此情境下，接受处理与未接受处理的群体之所以不同，是因为前者承担了显著更高的学费成本。而处理组与对照组之间的其他差异并非由处理本身引起。

个体处理效应是指某单元的实际结果与其在假设接受另一种处理时可能产生的理论结果之间的差异。唯有具备如神明般观测潜在结果的能力（如下方左图所示），我们才能获知真实的处理效应。这些反事实结果以浅色标识。

![img](./images/01/anatomy2.png)

右图中展示了先前讨论的偏误现象。当强制所有个体不接受处理时，我们仅能观察到 $T_0$ 潜在结果，此时偏误便显现出来。通过比较处理组与对照组的差异可以发现：若二者存在系统性差异，则说明存在非处理因素导致两组分化。这种干扰因素即为偏误，它会掩盖真实的处理效应。

现在，将此与一个无偏误的假设情境进行对比。假设平板电脑是随机分配给学校的。在这种情况下，富裕学校和贫困学校获得干预的机会均等。干预措施将在学费分布范围内得到良好分配。

![img](./images/01/anatomy3.png)

在此情形下，接受干预与未接受干预群体间的结果差异即为平均因果效应。这是因为除了干预本身外，干预组与对照组之间不存在其他差异来源。我们观察到的所有差异都必须归因于此。换言之，这种情况下不存在偏误。

![img](./images/01/anatomy4.png)

若设定所有人均不接收干预，仅观察 $Y_0$状态，我们将发现干预组与对照组之间不存在差异。

这正是因果推断所致力于解决的艰巨任务。其核心在于寻找巧妙方法消除偏误，使处理组与未处理组具有可比性，从而确保我们所观察到的差异仅反映平均处理效应。归根结底，因果推断旨在拨开所有错觉与误读的迷雾，揭示世界运行的真相。既然我们已经理解这一点，现在就可以继续掌握那些消除偏误的最有力方法——这些"求真敢为者"的武器，来识别因果效应。

## 核心要点

到目前为止，我们已经知道了：相关性 ≠ 因果性。

更重要的是，我们也准确理解了为什么不等同，以及如何让相关性接近因果性。

我们引入了“潜在结果（potential outcomes）”的符号，作为理解因果推理的核心工具。借助它，我们学会了把统计学问题看作两个可能的现实：一个是接受处理的现实，另一个则是不接受处理的现实。但遗憾的是，我们每次只能观测其中一个现实，这正是因果推断的根本难题所在。

接下来，我们将学习几种基本的估计因果效应的方法，
从最经典、也最可靠的手段——**随机实验（Randomized Trial）** 开始。

在此过程中，我也会穿插回顾一些统计学基础概念。

最后，我想用一句常出现在因果推断课堂上的台词作为收尾，
它出自一部功夫剧：

> “一个人的命运，早已写好。人只能顺着命运走完这一生。” —— 凯恩（Caine）  
“是的，但每个人又的确可以选择如何生活。
虽然这两句话看似矛盾，却都是真理。”
—— Old Man  




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 02 - 随机实验


## 黄金标准

在上一节中，我们探讨了相关性与因果性之间的差异及其原因，并明确了将相关转化为因果所需的条件。

$
E[Y|T=1] - E[Y|T=0] = \underbrace{E[Y_1 - Y_0|T=1]}_{ATT} + \underbrace{\{ E[Y_0|T=1] - E[Y_0|T=0] \}}_{BIAS}
$


回顾一下，若不存在偏误（bias），相关即可转化为因果。当 $E[Y_0|T=0]=E[Y_0|T=1]$ 时，偏误便不会产生。换言之，若处理组与对照组除处理因素外其他条件均等或可比，则相关即成为因果。或用更专业的术语来说，当未处理组的结果等同于处理组的反事实结果时。需记住，此反事实结果指的是处理组若未接受处理时将会出现的结果。

我认为我们在用数学术语解释如何将相关等同于因果关系方面做得还算不错。但那仅是理论层面的探讨。现在，我们介绍消除偏误的第一个工具：**随机实验（Randomised Experiments）**。随机实验将群体中的个体随机分配到处理组或控制组。接受处理的比例不必是50%，实验中可能仅有10%的样本接受处理。

随机化通过使潜在结果独立于处理方式，彻底消除了偏误。

$
(Y_0, Y_1) \perp\!\!\!\perp T
$

这一点起初可能令人困惑（对我而言确实如此）。但别担心，我会进一步解释的。如果结果和处理方式无关，这是否也意味着处理毫无效果？确实如此！但请注意，我这里说的不是实际结果（outcomes）而是**潜在结果（potential outcomes）**。潜在结果指的是在接受处理（$Y_1$）或处于对照组（$Y_0$）时可能出现的结果。在随机实验中，我们不希望结果独立于处理方式，因为我们相信处理会导致结果变化。但我们期望潜在结果能独立于处理方式。

![img](./images/02/indep.png)

所谓潜在结果独立于处理，即意味着在期望值上，处理组与对照组的结果是相同的。简而言之，这表明处理组和对照组具有可比性。或者说，了解处理分配情况并不能提供任何关于处理前结果的信息。因此，$(Y_0, Y_1)\perp T$ 意味着：处理是导致处理组与对照组结果差异的唯一因素。要理解这一点，需注意到独立性恰好意味着

 
$
E[Y_0|T=0]=E[Y_0|T=1]=E[Y_0]
$

正如我们所见，这使得

$
E[Y|T=1] - E[Y|T=0] = E[Y_1 - Y_0]=ATE
$

因此，随机化为我们提供了一种方法，即通过处理组与对照组均值的简单差异来定义处理效应。


## 一个遥远学校的例子

2020 年，新冠疫情迫使企业适应社交距离措施。送货服务变得普及，大型企业转向远程工作策略。学校也不例外，许多机构开始建立自己的在线课程资源库。

危机爆发四个月后，许多人思考这些引入的变革能否持续。毫无疑问，在线学习有其优势：它能节省房地产和交通成本，因而更为经济；同时，通过整合全球顶尖教学内容而非局限于固定教师群体，它还能实现更高程度的数字化。尽管如此，我们仍需解答在线学习对学生学业表现究竟产生消极还是积极影响。

一种解法方法是选取主要提供在线课程的学校学生，与采用面对面授课的学校学生进行比较。但如今我们已明白，这并非最佳途径。可能在线学校仅吸引了自律性强、即使面对面授课也能表现优异的学生群体。若如此，我们将面临正向偏误——接受在线教育的学生学术表现优于未接受者： $E[Y_0|T=1] > E[Y_0|T=0]$.

反之，也可能是因为在线课程更便宜，主要吸引经济条件较差、需半工半读的学生群体。在这种情况下，即便他们参与面对面授课，其表现仍可能逊于传统学校学生。若该假设成立，则会出现反向偏误——接受在线教育的学生学术表现反而更差： $E[Y_0|T=1] < E[Y_0|T=0]$. 

因此，尽管可进行简单对比，其结论却缺乏说服力。无论如何，我们永远无法确定是否存在潜在偏误掩盖了真实的因果效应。

![img](./images/02/lurking_bias.png)

为解决这一问题，我们需要使处理组与未处理组具备可比性 $E[Y_0|T=1] = E[Y_0|T=0]$。实现这一目标的方法是随机将在线课程和面对面课程分配给学生。若能成功实施，除所接受的处理方式不同外，处理组和未处理组在其他方面平均而言将保持一致。

幸运的是，已有经济学家为我们完成了这项工作。他们通过随机分配课程，使部分学生接受面对面授课，另一部分仅参与在线课程，第三组则采用线上线下混合教学模式。学期末，研究者收集了标准化考试的成绩数据。

数据呈现如下：

```{dropdown} 查看 Stata 代码
```stata
* Direct import from GitHub URL (Recommended for Stata 16+)
* import delimited "https://raw.githubusercontent.com/matheusfacure/python-causality-handbook/master/causal-inference-for-the-brave-and-true/data/online_classroom.csv", clear

* Clear any existing data from memory
clear

* Import the CSV file from local path
import delimited "./data/online_classroom.csv", encoding(UTF-8) 

* Alternative import command (for Stata versions before 14)
* insheet using "./data/online_classroom.csv", clear

* Check if data loaded correctly
describe  // Display variable names and types
list in 1/5  // Show first 5 observations

```python
import pandas as pd
import numpy as np

data = pd.read_csv("./data/online_classroom.csv")
print(data.shape)
data.head()
```

可以看到我们拥有 323 个样本。虽然称不上大数据，但足以开展工作。为了估计因果效应，我们可以简单地计算各处理组的平均得分。

```{dropdown} 查看 Stata 代码
```stata
* Step 1: Create class_format variable
gen class_format = "face_to_face"  // Set default value
replace class_format = "online" if format_ol == 1
replace class_format = "blended" if format_blended == 1

* Step 2: Calculate means by group
collapse (mean) asian black falsexam format_blended format_ol gender, by(class_format)

* Step 3: Display results
list, noobs clean

```python
(data
 .assign(class_format = np.select(
     [data["format_ol"].astype(bool), data["format_blended"].astype(bool)],
     ["online", "blended"],
     default="face_to_face"
 ))
 .groupby(["class_format"])
 .mean())
```

是的，就这么简单。我们可以看到，面对面课程的平均得分为 78.54 分，而在线课程的平均得分为 73.63 分。这对在线学习的支持者来说不是什么好消息。因此，在线课程的 $ATE$ 为 -4.91。这意味着，平均而言，**在线课程会导致学生的成绩下降约 5 分**。就是这样。你无需担心在线课程可能有负担不起面对面课程的较差学生，或者就此而言，你不必担心不同处理组的学生在接受处理之外有任何其他差异。通过设计，随机实验旨在消除这些差异。

因此，验证随机化过程是否正确（或确认所查看的数据无误）的一个有效方法是检查处理组与对照组在预处理变量上是否均衡。我们的数据包含性别和种族信息，可用于观察各群体间这些特征是否相似。可以看出，在 `gender` 、 `asian` 、 `hispanic` 和 `white` 变量上，两组表现相当接近。然而， `black` 变量似乎存在些许差异。这提醒我们注意小样本数据集可能出现的情况：即使在随机化条件下，也可能偶然出现组间差异。而在大样本中，此类差异往往会消失。

## 理想实验

随机实验或随机对照试验（RCT），是获得因果效应最可靠的方法。这种方法非常直接，效果也极具说服力，强大到很多国家都要求用它来验证新药是否有效。

打个通俗的比方：你可以把 RCT 想象成高铁，而其他因果推断方法更像是大巴车。大巴也能把人从一个地方送到另一个地方，但路上堵车、颠簸不定、速度慢。而高铁速度快、路线清晰、几乎不受干扰。

所以说，如果条件允许，我们当然希望所有的因果研究都能坐“高铁”。一个设计精良的随机对照试验，是科学家心目中的理想工具。

![img](./images/02/science_dream.png)

遗憾的是，这些方法往往要么成本极高，要么完全违背伦理。有时，我们根本无法控制分配机制。设想你是一名医生，试图评估孕期吸烟对新生儿体重的影响，你不可能随机强制一部分孕妇在孕期吸烟。又或者，假设你供职于一家大型银行，需要评估信用额度对客户流失率的影响，向客户随机分配信用额度的成本将过于高昂。再比如，你想了解提高最低工资对失业率的影响，但显然语法随意指定各国采用不同的最低工资标准。道理不言自明。

后文我们将探讨如何通过条件随机化降低实验成本，但对于违背伦理或不可行的实验仍束手无策。尽管如此，在处理因果问题时，始终值得思考理想实验的形态。不妨自问：倘若可能，为揭示这一因果效应，你会设计怎样的完美实验？这种思考往往能为我们指明方向，即便在缺乏理想实验条件时，也能找到揭示因果效应的途径。


## 分配机制

在随机实验中，将个体分配到处理组或控制组的机制是随机的。正如我们之后会看到的，所有的因果推断方法，其实都在试图识别“处理是如何被分配的”这一机制。如果我们能够明确知道这个机制是如何运作的，即使它不是随机的，我们的因果推断也会更加可信。

但遗憾的是，这种“分配机制”不能仅靠观察数据就识别出来。举个例子：假设你有一个数据集，显示“受教育程度”和“财富”之间存在相关性。你无法仅凭这点判断到底是教育导致了财富，还是财富影响了教育。你必须依赖自己对现实世界的理解，来推断一个合理的分配机制。

比如你可以认为：学校通过教育提升了人的生产力，从而让人获得更高收入；或者如果你对教育持悲观态度，也可以说学校对生产力没什么帮助，这种相关性只是因为富裕家庭更有条件让孩子接受高等教育，所以它是一个“伪相关”。

在因果问题中，我们常常可以提出两种相反的解释：一种是“X 导致了 Y”，另一种是“有个第三变量 Z 同时导致了 X 和 Y，从而造成 X 与 Y 之间的表面相关”。

正因为如此，了解和掌握“处理是如何被分配的”这一机制，会让你的因果结论更有说服力。这也是因果推断最吸引人的地方之一：应用机器学习往往只是“按对顺序点按钮”，而应用因果推断则要求你深入思考数据背后真正的生成机制。


## 核心要点

我们探讨了随机实验是揭示因果影响最简单且最有效的方法。它之所以有效，是因为它让处理组和控制组在其他方面具有可比性。

当然，我们并不能在所有情况下都进行随机实验，但思考一下“如果可以做，我们会怎么设计理想实验”依然是非常有帮助的。

这时候，熟悉统计学的人可能会跳出来质疑：“你没有考虑因果效应估计值的方差啊！怎么知道这 4.91 分的下降不是偶然的呢？”换句话说，怎么判断这个差异是否具有统计显著性呢？

他们的质疑是完全正确的。别担心，我接下来就会回顾一些相关的统计概念。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 03 - 统计回顾：最危险的方程

霍华德·韦纳（Howard Wainer）在其 2007 年的著名文章中论述了一些极其危险的方程：

“有些方程的危险在于你知晓它们，而另一些的危险则在于你对其一无所知。前者之所以构成威胁，是因为其内涵的秘密会打开通往隐藏巨大灾难的大门。其中显而易见的代表是爱因斯坦的标志性方程 $E = mc^2$，它揭示了潜藏于寻常物质中的巨大能量。相反，我关注的是那些当我们不了解时才释放其危险性的方程。这些方程若常备于心，能助我们洞悉事理；但若缺失，则会使我们陷入危险的无知境地。”

他所指的方程是棣莫弗方程：

$
SE = \dfrac{\sigma}{\sqrt{n}} 
$

其中 $SE$ 代表均值的标准误差，$\sigma$ 为标准差，$n$ 为样本容量。这听起来像是求真敢为者应当掌握的数学知识，让我们开始学习吧。

为了说明不了解这个公式为何极其危险，我们来看一些教育数据。我整理了三年间不同学校的 ENEM 成绩（巴西标准化高中考试分数，类似 SAT），并清理数据仅保留相关部分。原始数据可在 [Inep 官网](http://portal.inep.gov.br/web/guest/microdados#)下载。

观察表现最优的学校时，有一点引人注目：这些学校的学生数量相对较少。

```{dropdown} 查看 Stata 代码
```stata
* Clear any existing data from memory
clear

* Import the CSV file from local path
import delimited "./data/enem_scores.csv", encoding(UTF-8) 

* Sort the dataset by avg_score in descending order
gsort -avg_score

* Display the top 10 observations
list in 1/10, clean noobs

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from scipy import stats
import seaborn as sns
from matplotlib import pyplot as plt
from matplotlib import style
style.use("fivethirtyeight")
```

```python
df = pd.read_csv("data/enem_scores.csv")
df.sort_values(by="avg_score", ascending=False).head(10)
```

换一个角度来看，我们可以仅分离出前 1%的顶尖学校进行研究。它们有何特点？或许我们能从这些佼佼者身上学到些什么，并在其他地方复制其成功经验。果然，当我们观察这前 1%的学校时，发现它们平均而言学生数量较少。

```{dropdown} 查看 Stata 代码
```stata
* Calculate percentiles for thresholds
sum avg_score, detail
local top_score_p99 = r(p99)  // 99th percentile of avg_score

sum number_of_students, detail
local students_p98 = r(p98)   // 98th percentile of number_of_students

* Create top_school indicator (1 = top 1% school, 0 = others)
gen top_school = (avg_score >= `top_score_p99') if !missing(avg_score)
label define top_school_label 0 "Other Schools" 1 "Top 1% Schools"
label values top_school top_school_label

* Filter data (remove outliers in number_of_students)
preserve
    keep if number_of_students < `students_p98' & !missing(number_of_students)
    
    * Create boxplot
    graph box number_of_students, over(top_school) ///
        title("Number of Students of 1% Top Schools (Right)") ///
        ytitle("Number of Students") ///
        box(1, color(blue)) ///
        box(2, color(orange)) ///
        graphregion(color(white)) ///
        plotregion(color(white)) ///
        xsize(6) ysize(6)
restore


```python
plot_data = (df
             .assign(top_school = df["avg_score"] >= np.quantile(df["avg_score"], .99))
             [["top_school", "number_of_students"]]
             .query(f"number_of_students<{np.quantile(df['number_of_students'], .98)}")) # remove outliers

plt.figure(figsize=(6,6))
sns.boxplot(x="top_school", y="number_of_students", data=plot_data)
plt.title("Number of Students of 1% Top Schools (Right)");
```

一个自然而然的结论是，小规模学校能带来更高的学业表现。这在直觉上说得通，因为我们相信每位教师对应的学生越少，教师就能给予每个学生更多关注。但这与棣莫弗方程有何关联？又为何说这种做法存在风险？

然而，一旦人们开始依据这些信息做出重要且代价高昂的决策，情况就变得危险了。霍华德在文章中继续写道：

“20 世纪 90 年代，提倡缩小学校规模的做法盛行一时。众多慈善组织和政府机构资助拆分大型学校，因为小规模学校的学生在考试成绩优异群体中占比过高。”

人们却忽视了去关注成绩最差的 1%学校。当我们这样做时，瞧！这些学校的学生人数同样寥寥无几！

```{dropdown} 查看 Stata 代码
```stata
* Calculate percentiles
sum avg_score, detail
local q_99 = r(p99)  // 99th percentile
local q_01 = r(p1)    // 1st percentile

* Take random sample and create groups
preserve
    if _N > 10000 {
        sample 10000, count
    }
    
    gen Group = "Middle"
    replace Group = "Top" if avg_score > `q_99' & !missing(avg_score)
    replace Group = "Bottom" if avg_score < `q_01' & !missing(avg_score)
    
    * Create scatterplot with legend inside
    twoway (scatter avg_score number_of_students if Group == "Top", mcolor(red) msymbol(Oh)) ///
           (scatter avg_score number_of_students if Group == "Middle", mcolor(blue) msymbol(Oh)) ///
           (scatter avg_score number_of_students if Group == "Bottom", mcolor(green) msymbol(Oh)), ///
           title("ENEM Score by Number of Students in the School") ///
           ytitle("Average ENEM Score") ///
           xtitle("Number of Students in School") ///
           legend(order(1 "Top 1%" 2 "Middle 98%" 3 "Bottom 1%") ///
                  position(13) ring(0) cols(1) region(lcolor(none))) ///
           graphregion(color(white)) ///
           plotregion(color(white)) ///
           xsize(10) ysize(5)
restore


```python
q_99 = np.quantile(df["avg_score"], .99)
q_01 = np.quantile(df["avg_score"], .01)

plot_data = (df
             .sample(10000)
             .assign(Group = lambda d: np.select([d["avg_score"] > q_99, d["avg_score"] < q_01],
                                                 ["Top", "Bottom"], "Middle")))
plt.figure(figsize=(10,5))
sns.scatterplot(y="avg_score", x="number_of_students", hue="Group", data=plot_data)
plt.title("ENEM Score by Number of Students in the School");
```

上述现象正是莫瓦方程（Moivre’s equation）所预期的现象：随着学生人数的增加，平均成绩变得越来越精确。学校如果样本量很小，就可能因为纯粹的随机性而出现极高或极低的分数，这种情况在大规模学校中就不太可能发生。莫瓦方程揭示了一个关于现实中信息与数据记录的基本事实：数据永远是不精确的。那么问题就变成：它到底有多不精确？

统计学是一门处理这些不精确性的科学，以确保我们不会措手不及。正如塔勒布在其著作《被随机性欺骗》中所言：

> 概率不仅仅是对骰子或更复杂变体胜率的计算；它是对我们知识中确定性缺失的接纳，以及发展出应对无知的方法。

量化我们不确定性的一种方式是通过**估计值的方差**。方差告诉我们观测值与其中心及最可能值之间的偏离程度。正如莫瓦方程所示，这种不确定性随着我们观察数据量的增加而减小。这很合理，对吧？如果我们看到一所学校有许多学生表现优异，我们就能更有信心认为这确实是一所好学校。然而，如果我们看到一所学校仅有 10 名学生，其中 8 名表现良好，我们就需要更加怀疑。可能只是偶然，这所学校招到了一些高于平均水平的学生。

上方那幅美丽的三角形图正好讲述了这个故事。它展示出，当样本量很小时，我们对学校表现的估计具有极大的方差；而随着样本量的增加，这种方差逐渐缩小。这种现象不仅适用于学校的平均成绩，同样也适用于我们手中拥有的任何汇总统计量，包括我们常常希望估计的平均处理效应（ATE）。

##  估计值的标准误差

由于这仅是对统计学的回顾，我将适当加快讲解节奏。若您对分布、方差及标准误等概念尚不熟悉，请继续阅读，但需注意可能需要额外参考资料。建议您搜索麻省理工学院（MIT）的任何统计学入门课程，这些课程通常都讲得非常不错。

在前一节中，我们通过比较处理组与未处理组的均值差异（$E[Y|T=1]-E[Y|T=0]$），估计了平均处理效应 $E[Y_1-Y_0]$  。以在线课程为例，我们计算出了 $ATE$，并观察到负面影响：在线课程使学生成绩比面授课程学生低约 5 分。现在，我们将检验这一影响是否具有统计学显著性。

为此，我们需要估计 $SE$。已知样本量 $n$，为计算标准差的估计值，可采用如下方法：

$$
\hat{\sigma}=\sqrt{\frac{1}{N-1}\sum_{i=1}^N (x_i-\bar{x})^2}
$$

其中 $\bar{x}$ 是 $x$ 的平均值。幸运的是，大多数编程软件已经实现了这一点。在 Pandas 中，我们可以使用 [std](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.std.html) 方法。

```{dropdown} 查看 Stata 代码
```stata
* Clear any existing data from memory
clear

* Import the CSV file from local path
import delimited "./data/online_classroom.csv", encoding(UTF-8) 

* Calculate standard error for online group (format_ol==1)
sum falsexam if format_ol == 1
local se_online = r(sd)/sqrt(r(N))
display "SE for Online: " `se_online'

* Calculate standard error for face-to-face group (format_ol==0 & format_blended==0)
sum falsexam if format_ol == 0 & format_blended == 0
local se_ftf = r(sd)/sqrt(r(N))
display "SE for Face to Face: " `se_ftf'

* Alternative approach using Stata's built-in standard error calculation:
* For online group
ci mean falsexam if format_ol == 1
local se_online = r(se)
display "SE for Online: " `se_online'

* For face-to-face group
ci mean falsexam if format_ol == 0 & format_blended == 0
local se_ftf = r(se)
display "SE for Face to Face: " `se_ftf'


```python
data = pd.read_csv("./data/online_classroom.csv")
online = data.query("format_ol==1")["falsexam"]
face_to_face = data.query("format_ol==0 & format_blended==0")["falsexam"]

def se(y: pd.Series):
    return y.std() / np.sqrt(len(y))

print("SE for Online:", se(online))
print("SE for Face to Face:", se(face_to_face))
```

## 置信区间

我们估计的标准误差是衡量置信度的一个指标。为了准确理解其含义，我们需要深入探讨充满争议且复杂的统计学领域。从统计学的一种观点——频率学派的视角来看，我们会认为数据不过是精确数据生成过程的表现形式。这个过程抽象而理想，由真实且恒定的参数所支配，但这些参数对我们而言却是未知的。以学生测试为例，如果我们能进行多次实验并收集多个数据集，所有这些数据集都会近似于真实的数据生成过程，但不会与之完全相同。这与柏拉图关于“理念”（Forms）的论述极为相似：

> 每一个本质的理念（essential form），都在与行为、与物质事物、以及彼此之间的各种组合中呈现出来，每一个理念看起来仿佛是多样的、纷繁的。

为了更好地理解这个概念，假设我们有一个学生考试成绩的真实抽象分布。这个分布是一个均值为 74、标准差为 2 的正态分布。基于这个分布，我们可以进行 10,000 次实验，每次从中抽取 500 个样本。如果我们将这些实验中每次的样本均值绘制成直方图，就会发现它们围绕着真实均值呈现分布。有些实验的样本均值会低于真实均值，而有些则会高于真实均值。

```{dropdown} 查看 Stata 代码
```stata
clear all
set seed 42

* Parameters
local true_mean = 74
local true_std = 2
local n = 500

* Define the experiment program correctly
capture program drop run_experiment
program define run_experiment, rclass
    quietly {
        clear
        set obs `n'
        gen x = rnormal(`true_mean', `true_std')
        summarize x
        return scalar mean = r(mean)
    }
end

* Run the simulation
simulate mean=r(mean), reps(10000) nodots: run_experiment

* Create the histogram
hist mean, bin(40) ///
    title("Distribution of Sample Means") ///
    xtitle("Sample Mean") ///
    ytitle("Frequency") ///
    xline(`true_mean', lpattern(dash) lcolor(orange)) ///
    legend(label(1 "Experiment Means") label(2 "True Mean")) ///
    graphregion(color(white)) ///
    plotregion(color(white)) ///
    xsize(8) ysize(5)

```python
true_std = 2
true_mean = 74

n = 500
def run_experiment(): 
    return np.random.normal(true_mean,true_std, 500)

np.random.seed(42)

plt.figure(figsize=(8,5))
freq, bins, img = plt.hist([run_experiment().mean() for _ in range(10000)], bins=40, label="Experiment Means")
plt.vlines(true_mean, ymin=0, ymax=freq.max(), linestyles="dashed", label="True Mean", color="orange")
plt.legend();
```

要注意，我们这里讨论的是“均值的均值”。也就是说，由于随机性，我们可能会遇到某次实验的样本均值略低于或略高于真实的均值。换句话说，我们永远无法确定某次实验得到的样本均值就恰好等于那个柏拉图式、理想化的真实均值。不过，**借助标准误（standard error），我们可以构造一个置信区间，这个区间在 95% 的情况下会包含真实均值**。

现实中，我们无法奢侈地用多个数据集重复模拟同一实验，通常仅有一个数据集可用。但我们可以借鉴上述思路来构建所谓的**置信区间**。置信区间附带有概率指标，最常见的是 95%置信度。这一概率表明，在不同研究中构建的假设性置信区间有多少能包含真实均值。例如，基于类似研究计算的 95%置信区间，将有 95%的几率涵盖真实均值。

为计算置信区间，我们采用**中心极限定理**。该定理指出，实验**均值的分布服从正态分布**。根据统计学理论，我们已知正态分布 95%的质量集中于均值上下两个标准差范围内。严格而言应为 1.96 倍标准差，但近似取 2 已足够精确。

![normal_density](./images/16/normal_dist.jpeg)

均值标准误可作为实验均值分布的估计量。因此，若将其乘以 2 后与单次实验均值相加减，即可构建出真实均值 95%置信区间。

```{dropdown} 查看 Stata 代码
```stata
clear	
* Set random seed for reproducibility
set seed 321

* Parameters (same as before)
local true_mean = 74
local true_std = 2
local n = 500

* Run one experiment
clear
set obs `n'
gen x = rnormal(`true_mean', `true_std')

* Calculate statistics
sum x
local exp_mu = r(mean)
local exp_se = r(sd)/sqrt(r(N))
local ci_lower = exp_mu - 2 * exp_se
local ci_upper = exp_mu + 2 * exp_se

* Display results
display "95% Confidence Interval: (" %4.2f ci_lower ", " %4.2f ci_upper ")"	

```python
np.random.seed(321)
exp_data = run_experiment()
exp_se = exp_data.std() / np.sqrt(len(exp_data))
exp_mu = exp_data.mean()
ci = (exp_mu - 2 * exp_se, exp_mu + 2 * exp_se)
print(ci)
```

```{dropdown} 查看 Stata 代码
```stata
* Generate x values (4 SEs around mean)
drop x
range x `=exp_mu - 4*exp_se' `=exp_mu + 4*exp_se' 100

* Calculate normal PDF values
gen y = normalden(x, `=exp_mu', `=exp_se')

* Create the plot
twoway (line y x, lcolor(blue)) ///
       (function y = 0, range(`=ci_lower' `=ci_upper') lcolor(none) ///
           recast(area) color(gs12) legend(label(2 "95% CI"))) ///
       , ///
       title("Sampling Distribution") ///
       ytitle("Density") ///
       xtitle("Sample Mean") ///
       legend(order(1 "Normal PDF" 2 "95% CI")) ///
       graphregion(color(white)) ///
       plotregion(color(white))	

```python
x = np.linspace(exp_mu - 4*exp_se, exp_mu + 4*exp_se, 100)
y = stats.norm.pdf(x, exp_mu, exp_se)
plt.plot(x, y)
plt.vlines(ci[1], ymin=0, ymax=1)
plt.vlines(ci[0], ymin=0, ymax=1, label="95% CI")
plt.legend()
plt.show()
```

当然，我们不必局限于 95%的置信区间。通过确定需要将标准差乘以多少才能使区间包含正态分布 99%的质量，我们可以生成 99%的区间。

Python 中的函数 `ppf` 提供了累积分布函数（CDF）的逆运算。不同于我们为求 95%置信区间（CI）时将标准误差乘以 2 的做法，此处我们将乘以 `z` ，从而得到 99%的置信区间。因此， `ppf(0.5)` 将返回 0.0，表明标准正态分布（均值为 0，标准差为 1）中有 50%的质量位于 0.0 以下。同理，若输入 99.5%，我们将得到值 `z` ，这意味着 99.5%的分布质量低于此值。换言之，仅有 0.5%的质量高于该值。

```python
from scipy import stats
z = stats.norm.ppf(.995)
print(z)
ci = (exp_mu - z * exp_se, exp_mu + z * exp_se)
ci
```

```python
x = np.linspace(exp_mu - 4*exp_se, exp_mu + 4*exp_se, 100)
y = stats.norm.pdf(x, exp_mu, exp_se)
plt.plot(x, y)
plt.vlines(ci[1], ymin=0, ymax=1)
plt.vlines(ci[0], ymin=0, ymax=1, label="99% CI")
plt.legend()
plt.show()
```

回到我们的课堂实验，我们可以为在线和面对面学生群体的平均考试成绩构建置信区间

```python
def ci(y: pd.Series):
    return (y.mean() - 2 * se(y), y.mean() + 2 * se(y))

print("95% CI for Online:", ci(online))
print("95% for Face to Face:", ci(face_to_face))
```

我们可以观察到，各组的 95%置信区间（CI）并未重叠。面对面授课班级 CI 的下限高于在线课程 CI 的上限。这一结果表明，我们的发现并非偶然，且面对面课堂学生的真实均值确实高于在线课程学生。换言之，从面对面教学转向在线课程会导致学业成绩出现显著的因果性下降。

概括而言，置信区间是为我们的估计值提供不确定性范围的一种方法。样本量越小，标准误差越大，置信区间也越宽。由于置信区间计算极为简便，未提供置信区间可能意味着存在不良意图或仅仅是知识匮乏，这两者同样令人担忧。最后，对于没有任何不确定性度量的测量结果，你应始终保持怀疑态度。

![img](images/16/ci_xkcd.png)

最后要提醒一点，置信区间的解读比初看起来更为复杂。例如，我`不能`说这个特定的 95%置信区间有 95%的概率包含真实的总体均值。在使用置信区间的频率统计中，总体均值被视为一个固定的真实总体参数。因此，它要么在我们特定的置信区间内，要么不在。换句话说，我们具体的置信区间要么包含真实均值，要么不包含。如果包含，那么包含的概率就是 100%，而非 95%；如果不包含，概率则为 0%。实际上，置信区间中的 95%指的是在多次研究中计算出的此类置信区间包含真实均值的频率。95%是我们对用于计算 95%置信区间的算法的信心，而非针对某个特定区间本身。

话虽如此，作为一名经济学家（统计学家们现在可以回避了），我认为这种纯粹主义并无太大实际意义。在实践中，你会听到人们说特定置信区间在 95%的情况下包含真实均值。尽管这种表述并不准确，但它并无大碍，因为它依然为我们的估计设定了一个明确的不确定性程度。此外，如果我们转向贝叶斯统计并使用概率区间而非置信区间，我们就能说该区间在 95%的情况下包含分布均值。而且根据我的实际观察，在样本量足够的情况下，贝叶斯概率区间与置信区间的相似度，比贝叶斯学派和频率学派愿意承认的要高得多。因此，如果我的意见有任何分量，你大可以随心所欲地描述你的置信区间。即便你说它们在 95%的情况下包含真实均值，我也不介意。但请永远记得将区间估计值标注出来，否则你会显得很可笑。


## 假设检验

另一种纳入不确定性的方法是陈述一个假设检验：均值差异在统计上是否显著不同于零（或任何其他值）？我们将回顾，两个独立正态分布的和或差也是正态分布。所得均值将是两个分布的和或差，而方差始终是两者方差之和：

$
N(\mu_1, \sigma_1^2) - N(\mu_2, \sigma_2^2) = N(\mu_1 - \mu_2, \sigma_1^2 + \sigma_2^2)
$

$
N(\mu_1, \sigma_1^2) + N(\mu_2, \sigma_2^2) = N(\mu_1 + \mu_2, \sigma_1^2 + \sigma_2^2)
$

如果你不记得了也没关系。我们总可以用代码和模拟数据来验证：

```python
np.random.seed(123)
n1 = np.random.normal(4, 3, 30000)
n2 = np.random.normal(1, 4, 30000)
n_diff = n2 - n1
sns.distplot(n1, hist=False, label="$N(4,3^2)$")
sns.distplot(n2, hist=False, label="$N(1,4^2)$")
sns.distplot(n_diff, hist=False, label=f"$N(1,4^2) - (4,3^2) = N(-3, 5^2)$")
plt.legend()
plt.show()
```

如果我们取两组均值的分布并将一个减去另一个，将得到第三个分布。这个最终分布的均值将是两组均值的差异，而其标准差则是各标准差之和的平方根。

$
\mu_{diff} = \mu_1 - \mu_2
$

$
SE_{diff} = \sqrt{SE^2_1 + SE^2_2} = \sqrt{\sigma_1^2/n_1 + \sigma_2^2/n_2}
$

让我们回到课堂的例子。我们将构建这个差异的分布。当然，一旦有了这个分布，构建 95%的置信区间就变得简单直接了。

```python
diff_mu = online.mean() - face_to_face.mean()
diff_se = np.sqrt(face_to_face.var()/len(face_to_face) + online.var()/len(online))
ci = (diff_mu - 1.96*diff_se, diff_mu + 1.96*diff_se)
print(ci)
```

```python
x = np.linspace(diff_mu - 4*diff_se, diff_mu + 4*diff_se, 100)
y = stats.norm.pdf(x, diff_mu, diff_se)
plt.plot(x, y)
plt.vlines(ci[1], ymin=0, ymax=.05)
plt.vlines(ci[0], ymin=0, ymax=.05, label="95% CI")
plt.legend()
plt.show()
```

有了这个数据，我们可以有 95%的把握认为在线组与面对面组之间的真实差异落在-8.37 到-1.44 之间。此外，我们还可以通过将均值差除以差异的 $SE$ 来构建一个 **z 统计量**。

$
z = \dfrac{\mu_{diff} - H_{0}}{SE_{diff}} = \dfrac{(\mu_1 - \mu_2) - H_{0}}{\sqrt{\sigma_1^2/n_1 + \sigma_2^2/n_2}}
$

其中 $H_0$ 是我们想要测试差异的基准值。

z 统计量是衡量观察到的差异极端程度的指标。我们将采用反证法来检验均值差异在统计上是否显著不为零的假设。首先，我们假设相反的情况成立，即差异为零，这被称为零假设或 $H_0$。接着，我们自问：“如果真实差异为零，观察到如此大差异的可能性有多大？”用统计学术语来说，这个问题可转化为检验我们的 z 统计量距离零有多远。

在 $H_0$条件下，z 统计量服从标准正态分布。因此，若差异确实为零，我们有 95%的概率观察到 z 统计量位于均值两侧 2 个标准差范围内。其直接推论是，若 z 值超出 ±2 个标准差范围，我们就能以 95%的置信度拒绝零假设。

让我们看看这在我们课堂示例中是什么样子。

```python
z = diff_mu / diff_se
print(z)
```

```python
x = np.linspace(-4,4,100)
y = stats.norm.pdf(x, 0, 1)
plt.plot(x, y, label="Standard Normal")
plt.vlines(z, ymin=0, ymax=.05, label="Z statistic", color="C1")
plt.legend()
plt.show()
```

这个数值看起来相当极端。实际上，它超过了 2，这意味着如果两组之间没有差异，我们观察到如此极端值的概率将低于 5%。这再次引导我们得出结论：从面对面授课转为在线课程会导致学术成绩在统计上显著下降。

关于假设检验最后一件有趣的事情是，它比检查处理组和未处理组的 95%置信区间是否重叠更为宽松。换言之，即便两组间的置信区间存在重叠，结果仍可能在统计学上显著。例如，假设面对面组的平均得分为 80，标准误差为 4，而在线组的平均得分为 71，标准误差为 2。

```python
cont_mu, cont_se =  (71, 2)
test_mu, test_se = (80, 4)

diff_mu = test_mu - cont_mu
diff_se = np.sqrt(cont_se**2 + test_se**2)

print("Control 95% CI:", (cont_mu-1.96*cont_se, cont_mu+1.96*cont_se))
print("Test 95% CI:", (test_mu-1.96*test_se, test_mu+1.96*test_se))
print("Diff 95% CI:", (diff_mu-1.96*diff_se, diff_mu+1.96*diff_se))
```

如果我们为这些组构建置信区间，它们会相互重叠。在线组的 95%置信区间上限为 74.92，而面对面组的下限为 72.16。然而，一旦我们计算两组间差异的 95%置信区间，就会发现该区间不包含零。尽管各自的置信区间有重叠，差异仍可能在统计上显著不为零。

## P-values

此前我曾提到，如果线上与面对面小组之间的差异实际为零，我们观察到如此极端值的概率不足 5%。但我们能否精确估算这一概率？观察到如此极端值的可能性究竟有多大？这就引入了 p 值！

与置信区间（事实上，大多数频率统计方法也是如此）一样，p 值的真实定义可能非常令人困惑。因此，为了不冒任何风险，我将引用维基百科上的定义：“p 值是在假设零假设正确的前提下，获得至少与实际观测到的测试结果一样极端的测试结果的概率”。

简而言之，p 值是在零假设为真的情况下观察到此类数据的概率。它衡量的是在零假设成立时，你所看到的测量结果有多么不可能。很自然地，这常常被误认为是零假设为真的概率。请注意这里的区别：p 值并非 $P(H_0|data)$，而是 $P(data|H_0)$。

但别被这种复杂性所迷惑。实际上，它们使用起来相当简单直接。

![p_value](./images/16/p_value.png)

要获得 p 值，我们需要计算标准正态分布在 z 统计量之前或之后的面积。幸运的是，我们可以借助计算机来完成这一计算。只需将 z 统计量代入标准正态分布的累积分布函数(CDF)中即可。

```python
print("P-value:", stats.norm.cdf(z))
```

注意到 p 值的有趣之处在于它使我们无需指定如 95%或 99%这样的置信水平。然而，如果我们希望报告一个置信水平，通过 p 值，我们可以精确知道在哪个置信度下我们的检验会通过或失败。例如，当 p 值为 0.0027 时，我们发现显著性水平可达 0.2%。因此，虽然差异的 95%置信区间和 99%置信区间都不会包含零，但 99.9%的置信区间会包含零。这意味着如果差异为零，观察到如此极端的 z 统计量仅有 0.2%的概率。

```python
diff_mu = online.mean() - face_to_face.mean()
diff_se = np.sqrt(face_to_face.var()/len(face_to_face) + online.var()/len(online))
print("95% CI:", (diff_mu - stats.norm.ppf(.975)*diff_se, diff_mu + stats.norm.ppf(.975)*diff_se))
print("99% CI:", (diff_mu - stats.norm.ppf(.995)*diff_se, diff_mu + stats.norm.ppf(.995)*diff_se))
print("99.9% CI:", (diff_mu - stats.norm.ppf(.9995)*diff_se, diff_mu + stats.norm.ppf(.9995)*diff_se))
```

## 核心要点

我们已了解掌握棣莫弗方程的重要性，并运用它为我们的估计赋予了一定程度的确定性。具体而言，我们发现与面对面授课相比，在线课程会导致学业成绩下降。同时，这一结果在统计学上具有显著性。我们通过比较两组均值的置信区间、观察差异的置信区间、进行假设检验以及查看 p 值来完成这一分析。现在，让我们将所有步骤整合到一个函数中，以便执行类似上述的 A/B 测试比较。

```python
def AB_test(test: pd.Series, control: pd.Series, confidence=0.95, h0=0):
    mu1, mu2 = test.mean(), control.mean()
    se1, se2 = test.std() / np.sqrt(len(test)), control.std() / np.sqrt(len(control))
    
    diff = mu1 - mu2
    se_diff = np.sqrt(test.var()/len(test) + control.var()/len(control))
    
    z_stats = (diff-h0)/se_diff
    p_value = stats.norm.cdf(z_stats)
    
    def critial(se): return -se*stats.norm.ppf((1 - confidence)/2)
    
    print(f"Test {confidence*100}% CI: {mu1} +- {critial(se1)}")
    print(f"Control {confidence*100}% CI: {mu2} +- {critial(se2)}")
    print(f"Test-Control {confidence*100}% CI: {diff} +- {critial(se_diff)}")
    print(f"Z Statistic {z_stats}")
    print(f"P-Value {p_value}")
        
AB_test(online, face_to_face)
```

由于我们的函数足够通用，可以测试其他零假设。例如，我们能否尝试否定在线课程与面对面课程表现之间的差异为-1？根据所得结果，我们可以有 95%的把握说差异比-1 更为显著。但我们无法以 99%的置信度断言这一点：

```python
AB_test(online, face_to_face, h0=-1)
```

## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 04 - 图形因果模型


## 思考因果关系

你是否注意到 YouTube 视频中的厨师们多么擅长描述食物？“将酱汁收至丝绒般浓稠”。如果你是刚开始学做饭的菜鸟，根本不知道这是什么意思，只想问一句：到底要在锅里煮几分钟？因果推断也是一样的。假设你走进一家酒吧，刚好旁边是经济系，你听到一群人正在讨论因果性。他们可能在说：“因为收入的混杂因素，很难识别移民对这个社区的影响，所以我们用了工具变量。”到这时候，你可能根本听不懂他们在讲什么。但至少从现在开始，我会帮你把这些事情讲清楚一点。

图形模型是因果关系的语言。它们不仅是你与其他因果关系爱好者交流的工具，更能让你的思考过程更加清晰透明。

作为起点，我们以潜在结果的条件独立性为例。这是进行因果推断时需要满足的主要假设之一：

$
(Y_0, Y_1) \perp T | X
$

条件独立性使我们能够仅衡量处理对结果的影响，而不受其他潜在变量的干扰。经典的例子是药物对患病患者的效果。如果只有病情严重的患者接受药物治疗，甚至可能看起来服药反而降低了患者的健康水平。这是因为疾病严重程度的影响与药物效果混杂在一起。若我们将患者按病情严重与否分组，并在每个亚组中分析药物的影响，就能更清晰地了解实际效果。这种根据特征对人群进行分组的方法称为控制或条件化于 X。通过对重症病例进行条件化处理，治疗机制变得如同随机分配。在重症组内，患者是否接受药物仅由偶然性决定，而不再取决于病情严重程度，因为所有患者在这一维度上是相同的。而只要处理在组内是随机的，那我们就可以说：处理与潜在结果是条件独立的（conditionally independent）。

独立性和条件独立性是因果推断中的核心概念，但要真正理解它们的含义，其实并不容易。不过，如果我们用一种恰当的语言来描述这个问题，情况就会有所不同。这正是 **因果图模型（causal graphical models）** 发挥作用的地方。因果图模型提供了一种方式，用以表示“谁导致了谁”的因果结构，从而帮助我们更清晰地理解因果机制是如何运作的。

一个图模型看起来像这样

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
import graphviz as gr
from matplotlib import style
import seaborn as sns
from matplotlib import pyplot as plt
style.use("fivethirtyeight")
```

```python
g = gr.Digraph()
g.edge("Z", "X")
g.edge("U", "X")
g.edge("U", "Y")

g.edge("medicine", "survived")
g.edge("severeness", "survived")
g.edge("severeness", "medicine")

g
```

每个节点代表一个随机变量。我们用箭头或边来表示一个变量是否导致另一个变量。在上面的第一个图模型中，我们表示 Z 导致 X，而 U 同时导致 X 和 Y。举一个更具体的例子，我们可以将关于药物对患者生存影响的思考转化为上述第二个图。病情严重程度同时影响药物使用和生存率，而药物使用也直接影响生存率。正如我们将看到的，这些因果图模型的语言将有助于使我们对因果关系的思考更加清晰，因为它阐明了我们对世界运作方式的信念。

## 因果图模型速成

关于图形模型，有[整整一学期的课程](https://www.coursera.org/specializations/probabilistic-graphical-models)专门讲解。但就我们目前的目的而言，最重要的是：我们必须清楚图形模型中所隐含的“独立性”和“条件独立性”假设。正如我们即将看到的，独立性在图形模型中的传播，就像水在溪流中流动一样。我们可以通过对变量的处理，阻断或开启这种“因果信息的流动”。为了理解这一点，我们将考察一些常见的图形结构和例子。它们看起来会非常直观，但足以作为构建理解的基石，让我们掌握图形模型中关于独立性与条件独立性的所有核心概念。

首先，看这个非常简单的图。A 导致 B，B 导致 C。或者说 X 导致 Y，Y 导致 Z。

```python
g = gr.Digraph()
g.edge("A", "B")
g.edge("B", "C")

g.edge("X", "Y")
g.edge("Y", "Z")
g.node("Y", "Y", color="red")


g.edge("causal knowledge", "solve problems")
g.edge("solve problems", "job promotion")

g
```

在第一幅图中，依赖关系沿箭头方向流动。需注意的是，依赖关系是对称的，尽管这一点稍显反直觉。举个更具体的例子，假设了解因果推断是解决商业问题的唯一途径，而解决这些问题又是获得职位晋升的唯一方式。因此，因果知识意味着能解决导致晋升的问题。这里我们可以说，职位晋升依赖于因果知识。因果专业知识越丰富，获得晋升的机会就越大。同样，晋升机会越大，拥有因果知识的可能性也越高。否则，晋升将变得困难。

现在，假设我对中介变量进行条件化处理。在这种情况下，依赖关系被阻断。因此，给定 Y 时，X 与 Z 相互独立。在上图中，红色表示 Y 是一个被条件化的变量。同理，在我们的例子中，如果我知道你擅长解决问题，那么了解你掌握因果推断的知识并不会为你获得职位晋升的机会提供更多信息。用数学术语表达就是 $E[Promotion|Solve \ problems, Causal \ knowledge]=E[Promotion|Solve \ problems]$。反之亦然；一旦我知道你解决问题的能力如何，了解你的晋升状况并不能让我进一步获知你掌握因果推断的可能性。

一般而言，当我们对中介变量 B 进行条件化时，从 A 到 C 的直接路径中的依赖流会被阻断。或者说，

$A \not \perp C$

以及

$
A \perp C | B
$

现在，让我们考虑一个分叉结构。同一个变量在图的下游引起另外两个变量。这种情况下，依赖关系会沿着箭头反向流动，形成一条**后门路径**。我们可以通过对共同原因进行条件化来关闭后门路径，从而切断依赖关系。

```python
g = gr.Digraph()
g.edge("C", "A")
g.edge("C", "B")

g.edge("X", "Y")
g.edge("X", "Z")
g.node("X", "X", color="red")

g.edge("statistics", "causal inference")
g.edge("statistics", "machine learning")

g
```

举例来说，假设你对统计学的了解使你在因果推断和机器学习方面更为精通。若我不清楚你的统计学水平，那么得知你擅长因果推断会提高我认为你也擅长机器学习的概率。这是因为即便我不了解你的统计学知识程度，也能从你的因果推断能力中推断出来。如果你精于因果推断，很可能统计学也相当扎实，从而更有可能在机器学习领域表现优异。

现在，若以你的统计学知识水平为条件，那么你对机器学习的了解程度就与对因果推断的掌握相互独立。知晓你的统计学水平已为我提供了推断你机器学习技能所需的全部信息。可见，在这种情况下，了解你的因果推断能力不会带来额外信息。

一般而言，具有共同原因的两个变量是相互依赖的，但当我们以该共同原因为条件时，它们便相互独立。或者说

$A \not \perp  B$

和

$
A \perp B | C
$

唯一缺失的结构是对撞节点。对撞节点指的是两个箭头交汇于同一个变量的情况。可以说，在这种情况下，这两个变量共享一个共同的影响结果。

```python
g = gr.Digraph()
g.edge("B", "C")
g.edge("A", "C")

g.edge("Y", "X")
g.edge("Z", "X")
g.node("X", "X", color="red")

g.edge("statistics", "job promotion")
g.edge("flatter", "job promotion")

g
```

举例来说，假设有两种方式可以获得职位晋升：要么擅长统计学，要么善于奉承上司。如果我不以你是否获得晋升为条件，即我对你是否会晋升一无所知，那么你的统计学水平和奉承能力是相互独立的。换句话说，了解你在统计学上的造诣并不能告诉我你奉承上司的水平如何。然而，一旦你确实获得了晋升，情况就截然不同——知道你的统计学水平就能推断出你的奉承能力。如果你统计学能力欠佳却仍获得晋升，那么你很可能是精通奉承之道；反之则难以晋升。同样地，若你不善奉承，则必然在统计学方面表现优异。这种现象有时被称为 **“解释消除”** ，因为一个原因已经充分解释了结果，使得另一个原因的可能性降低。

一般而言，对撞子条件化会打开依赖路径，不对其条件化则保持路径关闭。或者

$A \perp B$

并且

$
A \not \perp B | C
$

了解这三种结构后，我们可以推导出一条更为普遍的规则：路径被阻断当且仅当：
1. 路径中包含一个已被条件化的非对撞子
2. 它包含一个未被条件化且其后代也未被条件化的对撞变量。

这里有一份关于图中依赖流向的速查表，我摘自 Mark Paskin 在[斯坦福大学的演讲](http://ai.stanford.edu/~paskin/gm-short-course/lec2.pdf)。箭头末端带横线表示独立关系，不带横线的箭头则表示依赖关系。

![img](images/04/graph-flow.png)

作为最后一个示例，尝试分析以下因果图中的一些独立与依赖关系。
1. Is $D \perp C$?
2. Is $D \perp C| A $ ?
3. Is $D \perp C| G $ ?
4. Is $A \perp F $ ?
5. Is $A \perp F|E $ ?
6. Is $A \perp F|E,C $ ?

```python
g = gr.Digraph()
g.edge("C", "A")
g.edge("C", "B")
g.edge("D", "A")
g.edge("B", "E")
g.edge("F", "E")
g.edge("A", "G")

g
```

**答案**:
1. $D \perp C$。它包含了一个未被控制的对撞变量。
2. $D \not\perp C| A $ 。它包含了一个已被控制的对撞变量。
3. $D \not\perp C| G $。它包含了一个已被条件化的对撞因子（collider）的后代。此处可将 G 视为 A 的某种代理变量。
4. $A \perp F $。它包含了一个未被条件化的对撞因子 B->E<-F。
5. $A \not\perp F|E $。它包含了一个已被条件化的对撞因子 B->E<-F。
6. $A \perp F|E, C $ 。它包含了一个已被条件化的对撞因子 B->E<-F，但同时存在一个被条件化的非对撞因子。对 E 进行条件化会打开路径，而对 C 进行条件化则会再次关闭该路径。

了解因果图模型能让我们认识到因果推断中出现的问题。正如所见，这些问题归根结底都源于偏误。

$
E[Y|T=1] - E[Y|T=0] = \underbrace{E[Y_1 - Y_0|T=1]}_{ATET} + \underbrace{\{ E[Y_0|T=1] - E[Y_0|T=0] \}}_{BIAS}
$

图形模型帮助我们诊断所面临的偏误类型，并确定纠正这些偏误所需的工具。

## 混杂偏误

![img](./images/04/both_crap.png)

偏误的首要重要原因是混杂因素。当处理因素与结果存在共同原因时，就会发生这种情况。例如，假设处理因素是教育程度，结果是收入水平。由于两者共享一个共同原因——智力水平，我们很难确定教育对工资的因果效应。因此可以认为，受教育程度更高的人收入更高仅是因为他们更聪明，而非接受了更多教育。要识别真正的因果效应，必须阻断处理因素与结果之间的所有后门路径。这样处理后，唯一剩下的效应就是直接效应 T→Y。在上述例子中，若我们控制智力变量（即比较智力水平相同但教育程度不同的群体），结果的差异将仅源于教育程度的差别，因为所有人的智力水平都相同。纠正混杂偏误需要控制处理因素与结果的所有共同原因。

```python
g = gr.Digraph()
g.edge("X", "T")
g.edge("X", "Y")
g.edge("T", "Y")

g.edge("Intelligence", "Educ"),
g.edge("Intelligence", "Wage"),
g.edge("Educ", "Wage")
g
```

遗憾的是，并非总能控制所有常见因素。有时存在未知因素或我们无法衡量的已知因素，智力便是后者之一。尽管科学家们付出诸多努力，至今仍未找到准确衡量智力的方法。此处我将用 U 表示未测量的变量。现在假设智力无法直接影响教育程度——它虽影响 SAT 成绩，但决定教育水平的是 SAT 分数，因为高分能打开优质大学的大门。即便我们无法控制不可测量的智力因素，通过控制 SAT 分数仍可阻断这条后门路径。

```python
g = gr.Digraph()
g.edge("X1", "T")
g.edge("T", "Y")
g.edge("X2", "T")
g.edge("X1", "Y")
g.edge("U", "X2")
g.edge("U", "Y")

g.edge("Family Income", "Educ")
g.edge("Educ", "Wage")
g.edge("SAT", "Educ")
g.edge("Family Income", "Wage")
g.edge("Intelligence", "SAT")
g.edge("Intelligence", "Wage")
g
```

在下述图表中，以 X1 和 X2（即 SAT 分数与家庭收入）为条件，足以阻断处理变量与结果之间所有后门路径。换言之，$(Y_0, Y_1) \perp T | X1, X2$。因此，即便无法测量所有共同原因，只要控制那些可观测变量——它们能中介未测量变量对处理的影响——仍可实现条件独立性。需注意，此处还存在 $(Y_0, Y_1) \perp T | X1, U$，但由于 U 不可观测，我们无法以其为条件。

但若非如此呢？倘若未测量变量直接同时影响处理变量与结果会怎样？在接下来的示例中，智力因素同时影响教育程度与收入水平，导致教育（处理变量）与工资（结果变量）间存在混杂。这种情况下，由于混杂因子不可测量，我们无法直接控制它。然而，其他可测变量可作为混杂因子的代理变量——这些变量虽不在后门路径上，但控制它们有助于降低偏误（尽管不能完全消除）。这类变量有时被称为替代混杂因子。

在我们的示例中，虽然无法直接测量智力水平，但可以测量其部分成因（如父母的教育程度）及部分表现（如智商或 SAT 分数）。控制这些替代变量虽不足以完全消除偏误，却有所助益。

```python
g = gr.Digraph()
g.edge("X", "U")
g.edge("U", "T")
g.edge("T", "Y")
g.edge("U", "Y")

g.edge("Intelligence", "IQ")
g.edge("Intelligence", "SAT")
g.edge("Father's Educ", "Intelligence")
g.edge("Mother's Educ", "Intelligence")

g.edge("Intelligence", "Educ")
g.edge("Educ", "Wage")
g.edge("Intelligence", "Wage")

g
```

## 选择偏误

或许您认为将所有可测量的变量纳入模型是避免混淆偏误的好方法。然而，请三思而行。

![image.png](./images/04/selection_bias.png)

第二个重要的偏误来源是我们所称的选择偏误。在此，我认为区分它与混淆偏误是有建设性的，因此我将坚持这一区分。如果说混淆偏误发生在我们未能控制一个共同原因时，那么选择偏误更多与效应相关。这里需要提醒的是，经济学家倾向于将所有类型的偏误都称为选择偏误。

通常情况下，选择偏误发生在我们控制了过多变量时。可能出现的情况是，处理与潜在结果在边际上独立，但一旦我们以对撞机为条件，它们就变得相互依赖。

设想一下，借助某种奇迹，你终于能够随机分配教育以衡量其对工资的影响。但为了确保没有混淆因素，你控制了许多变量，其中包括投资。然而，投资并非教育与工资的共同原因，而是两者的结果。受教育程度更高的人不仅收入更多，投资也更多；同样，收入更高的人投资也更多。由于投资是一个碰撞变量，通过对其加以控制，你实际上在治疗与结果之间开辟了第二条路径，这将使得直接效应的测量变得更加困难。一种理解方式是，通过控制投资，你观察的是投资相同的小群体，然后在这些群体中寻找教育的影响。但这样做时，你也在无意间间接限制了工资的应有变化。结果，你将无法看清教育如何改变工资，因为你阻止了工资按照其自然规律变动。

```python
g = gr.Digraph()
g.edge("T", "X")
g.edge("T", "Y")
g.edge("Y", "X")
g.node("X", "X", color="red")

g.edge("Educ", "Investments")
g.edge("Educ", "Wage")
g.edge("Wage", "Investments")

g
```

假设投资与教育仅取两个值以说明此情形：人们要么投资，要么不投资；他们要么受过教育，要么没有。最初，当我们不控制投资变量时，由于教育是随机分配的，偏误项为零（$E[Y_0|T=1] - E[Y_0|T=0] = 0$ ）。这意味着无论是否接受教育干预，人们未受教育时的工资（$Wage_0$）是相同的。但若我们以投资为条件进行观察，会发生什么？

观察那些进行投资的人群，很可能存在 $E[Y_0|T=0, I=1] > E[Y_0|T=1, I=1]$的情况。换言之，在投资者中，那些无需教育也能成功投资的人更可能获得高收入，无论其教育水平如何。因此，这些人的工资 $Wage_0|T=0$ 很可能高于受过教育群体若未受教育时的工资 $Wage_0|T=1$。类似逻辑适用于不投资者群体，我们很可能同样发现 $E[Y_0|T=0, I=0] > E[Y_0|T=1, I=0]$。那些即使受过教育也不投资的人，若未获得教育，其工资可能低于未受教育也未投资的群体。

从纯图形论证的角度来看，若某人决定投资，且已知其受教育程度较高，这便排除了第二个影响因素——工资。在投资条件下，更高的教育水平与较低的工资相关联，从而产生负向偏误 $E[Y_0|T=0, I=i] > E[Y_0|T=1, I=i]$。

附带说明的是，如果我们以共同效应的任一后裔为条件，所讨论的这些内容均成立。

```python
g = gr.Digraph()
g.edge("T", "X")
g.edge("T", "Y")
g.edge("Y", "X")
g.edge("X", "S")
g.node("S", "S", color="red")
g
```

类似的情况也发生在我们以处理的中介变量为条件时。中介变量是介于处理与结果之间的变量，它实际上起着调节因果效应的作用。例如，再次假设你可以随机分配教育程度，但为了确保效果，你决定控制个体是否拥有白领工作。这样一来，这种条件设置再次导致了因果效应估计的偏误。这次偏误的产生并非因为它开启了带有碰撞器的前门路径，而是因为它关闭了处理作用的一个渠道。在我们的例子中，获得白领工作是更多教育带来更高收入的途径之一。通过控制这一变量，我们关闭了这一渠道，仅保留了教育对工资的直接影响。

```python
g = gr.Digraph()
g.edge("T", "X")
g.edge("T", "Y")
g.edge("X", "Y")
g.node("X", "X", color="red")

g.edge("educ", "white collar")
g.edge("educ", "wage")
g.edge("white collar", "wage")

g
```

从潜在结果的角度论证，我们知道由于随机化处理，偏误为零 $E[Y_0|T=0] - E[Y_0|T=1] = 0$。然而，若以白领个体为条件，则存在 $E[Y_0|T=0, WC=1] > E[Y_0|T=1, WC=1]$ 的情况。这是因为那些即便未受教育也能获得白领职位的人，可能比需要教育辅助才能获得相同职位者更为勤奋。同理，$E[Y_0|T=0, WC=0] > E[Y_0|T=1, WC=0]$ 因为那些即便受过教育仍未能获得白领职位的人，可能比未受教育也未获此职位者更不勤奋。

在本研究中，以中介变量为条件会引入负向偏误。这会使教育的影响显得低于实际水平。之所以如此，是因为因果效应本身为正。若效应为负，则对中介变量进行条件化处理将产生正向偏误。无论何种情况，此类条件化操作都会弱化实际效应。

用更通俗的话来说，假设你需要在两位求职者中为公司挑选一位。两人专业成就同样出色，但其中一位没有高等教育学历。你该选谁？当然应该选择那位没有高学历的候选人，因为他在条件不利的情况下仍取得了与另一位同等的成就。

![image.png](./images/04/three_bias.png)

## 核心要点

我们研究了图形模型作为一种语言，以更好地理解和表达因果关系概念。我们快速总结了图上条件独立性的规则，这帮助我们随后探讨了可能导致偏误的三种结构。

首先是混杂因素，当处理与结果存在未被考虑或控制的共同原因时发生。其次是由于对共同效应进行条件化而导致的选取偏误。第三种结构同样是选取偏误的一种形式，这次源于对中介变量的过度控制。即使处理是随机分配的，这种过度控制也可能导致偏误。选取偏误往往可以通过不作为来纠正，这正是其危险之处。由于我们倾向于采取行动，常将控制变量的方法视为巧妙之举，实则可能弊大于利。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 05 - 线性回归的惊人有效性


## 万物皆可回归

在处理因果推断时，我们观察到每个个体存在两种潜在结果：$Y_0$ 表示个体未接受处理时的结果，$Y_1$ 则表示其接受处理后的结果。将处理变量 $T$ 设定为 0 或 1 的行为，会具体化其中一种潜在结果，并使我们永远无法知晓另一种结果。这导致个体处理效应 $\tau_i = Y_{1i} - Y_{0i}$ 成为不可知量。

$
Y_i = Y_{0i} + T_i(Y_{1i} - Y_{0i}) = Y_{0i}(1-T_i) + T_i Y_{1i}
$

因此，目前让我们聚焦于估算平均因果效应这一更简单的任务。基于此，我们既承认不同个体对处理的反应存在差异，也接受无法具体识别哪些个体反应更佳的事实。相反，我们将仅试图从**平均意义上**验证处理是否有效。

$
ATE = E[Y_1 - Y_0]
$

这将为我们提供一个简化模型，其中处理效应为常数 $Y_{1i} = Y_{0i} + \kappa$。若 $\kappa$ 为正，则表明该处理平均而言具有积极效果。即便部分个体反应不佳，整体影响仍呈正面。

还需注意的是，由于存在偏误，我们无法直接通过均值差 $E[Y|T=1] - E[Y|T=0]$ 来估计 $E[Y_1 - Y_0]$。当处理组与对照组因非处理因素存在差异时，常会产生此类偏误。理解这一点可观察其在潜在结果 $Y_0$ 上的差异表现。

$
E[Y|T=1] - E[Y|T=0] = \underbrace{E[Y_1 - Y_0|T=1]}_{ATET} + \underbrace{\{ E[Y_0|T=1] - E[Y_0|T=0]\}}_{BIAS}
$

此前，我们探讨了如何通过随机实验——有时也被称为**随机对照试验（RCT）**——来消除偏误。RCT 通过强制使处理组与未处理组条件均等，从而令偏误消失。我们还学习了如何围绕处理效应的估计值设置不确定性水平。具体而言，我们分析了线上与面对面课堂的案例，其中 $T=0$ 代表面对面授课，$T=1$ 代表在线授课。学生被随机分配至这两种授课方式之一，随后对其考试成绩进行评估。我们构建了一个 A/B 测试函数，能够比较两组差异，提供平均处理效应，并为其建立置信区间。

现在，是时候来看看我们如何用 **因果推断的主力工具——线性回归（Linear Regression）** 来完成这一切了！可以这样想：如果说简单地比较处理组与对照组的均值是饭后的一颗苹果，那么线性回归就是一份冰凉柔滑的提拉米苏。又或者，如果前者是一片陈旧单调的白面包片，那么线性回归就是一块外壳酥脆、内部松软、由查德·罗伯逊亲手烘焙的乡村酸面包。

![img](./images/05/you_vs.png)

让我们来看看这个“优雅的工具”是如何运作的。在下面的代码中，我们将重复之前的分析——比较线上课程与线下课程的效果。但这一次，我们不再手动计算置信区间等数学内容，而是直接运行一个回归模型。更具体地说，我们要估计如下这个模型：

$
exam_i = \beta_0 + \kappa \ Online_i + u_i
$

这意味着我们将考试成绩建模为基线 $\beta_0$ 加上在线课程时的 $\kappa$。当然，考试成绩还受其他变量影响（如考试当天学生情绪、学习时长等）。但我们并不真正关心这些关系的解析。因此，我们用 $u_i$ 项代表所有我们不关心的其他因素，这被称为模型误差。

注意 $Online$ 是我们的处理指示变量，因此是一个虚拟变量。当面授课时其值为 0，在线课程时为 1。基于此，我们可以看到线性回归将恢复 $E[Y|T=0] = \beta_0$ 和 $E[Y|T=1] = \beta_0 + \kappa $。$\kappa$ 将成为我们的平均处理效应（ATE）。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/online_classroom.csv", clear

* Filter to exclude blended format
keep if format_blended == 0

* Run OLS regression
regress falsexam format_ol
eststo online_model
* Display in results window
esttab online_model, cells("b(fmt(3) star)") ///
    star(* 0.05 ** 0.01 *** 0.001) ///
    stats(N r2_a, labels("Observations" "Adj. R-squared"))  ///
    mtitle("Online vs Face-to-Face") ///
    nobaselevels ///
    interaction(" × ")
	
* Calculate mean falsexam by format_ol
collapse (mean) falsexam, by(format_ol)

* Display results with labels
list format_ol falsexam, noobs clean

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
import statsmodels.formula.api as smf
import graphviz as gr
%matplotlib inline
```

```python
data = pd.read_csv("data/online_classroom.csv").query("format_blended==0")

result = smf.ols('falsexam ~ format_ol', data=data).fit()
result.summary().tables[1]
```

这真是太棒了！我们不仅能估计 ATE，还能顺带获得置信区间和 P 值！不仅如此，我们还能看到回归分析正在精确执行其应有的功能：比较 $E[Y|T=0]$ 和 $E[Y|T=1]$。截距项恰好对应 $T=0$ 时的样本均值 $E[Y|T=0]$，而线上形式的系数则正好是均值差异的样本估计 $E[Y|T=1] - E[Y|T=0]$。不相信我？没关系，你可以自己验证：

```{dropdown} 查看 Stata 代码
```stata
* Load data and keep needed variables
import delimited "./data/online_classroom.csv", clear

* keep falsexam format_ol
keep if format_blended == 0

* Create intercept column
gen intercept = 1

* Prepare matrices
mkmat falsexam, matrix(y)
mkmat format_ol intercept, matrix(X)

* Calculate regression coefficients using matrix algebra
matrix XpX = X'*X
matrix Xpy = X'*y
matrix beta = invsym(XpX)*Xpy

* Display results
matrix list beta

```python
(data
 .groupby("format_ol")
 ["falsexam"]
 .mean())
```

正如预期。如果将截距与 ATE（即在线格式的参数估计值）相加，你将得到处理组的样本均值： $78.5475 + (-4.9122) = 73.635263$.

## 回归理论

我并不打算深入探讨线性回归的构建与估计方法。不过，少许理论基础将极大地有助于解释其在因果推断中的强大作用。首先，回归解决的是一个理论上的最佳线性预测问题。设 $\beta^*$ 为参数向量：

$
\beta^* =\underset{\beta}{argmin} \ E[(Y_i - X_i'\beta)^2]
$

线性回归寻找能够最小化均方误差（MSE）的参数。

若对其求导并设为零，你会发现该问题的线性解由下式给出

$
\beta^* = E[X_i'X_i]^{-1}E[X_i' Y_i]
$

我们可以用样本的形式来估计这个 beta值 ，即：

$
\hat{\beta} = (X'X)^{-1}X' Y
$

但别只听我的一面之词。如果你属于那种更懂代码而非公式的人，不妨亲自尝试：

```{dropdown} 查看 Stata 代码
```stata
* Calculate covariance and variance
correlate falsexam format_ol, covariance
matrix C = r(C)
scalar cov_falsexam_format = C[2,1]

summarize format_ol
scalar var_format = r(Var)

```python
X = data[["format_ol"]].assign(intercep=1)
y = data["falsexam"]

def regress(y, X): 
    return np.linalg.inv(X.T.dot(X)).dot(X.T.dot(y))

beta = regress(y, X)
beta
```

上述公式相当通用。然而，仅研究单一回归变量的情形颇具价值。在因果推断中，我们常需估计变量 $T$ 对结果 $y$ 的因果影响。因此，我们采用仅含该变量的回归模型来估算这一效应。即便模型中纳入其他变量，这些变量通常仅起辅助作用。添加其他变量有助于我们估计处理的因果效应，但我们对其参数估计并无太大兴趣。

在仅有一个回归变量 $T$ 的情况下，与之相关的参数将由

$
\beta_1 = \dfrac{Cov(Y_i, T_i)}{Var(T_i)} 
$

若 $T$ 被随机分配，则 $\beta_1$ 即为平均处理效应（ATE）。

```{dropdown} 查看 Stata 代码
```stata
* Compute kappa (regression coefficient)
scalar kappa = cov_falsexam_format / var_format

* Display result
display "Kappa (regression coefficient) = " %5.3f kappa

```python
kapa = data["falsexam"].cov(data["format_ol"]) / data["format_ol"].var()
kapa
```

若存在多个回归变量，可扩展以下公式以适应此情况。假设其他变量仅为辅助性质，我们真正关注的仅是估计与 $T$ 相关联的参数 $\kappa$。

$
y_i = \beta_0 + \kappa T_i + \beta_1 X_{1i} + ... +\beta_k X_{ki} + u_i
$

$\kappa$ 可通过以下公式求得

$
\kappa = \dfrac{Cov(Y_i, \tilde{T_i})}{Var(\tilde{T_i})} 
$

其中 $\tilde{T_i}$ 为其他所有协变量 $X_{1i} + ... + X_{ki}$ 对 $T_i$ 回归后的残差。现在，让我们体会这一方法的精妙之处：这意味着多元回归中的系数，是在控制模型中其他变量影响后，同一回归变量的双变量系数。用因果推断术语表述，$\kappa$ 是在利用所有其他变量预测 $T$ 后，$T$ 的双变量系数。

这背后有一个直观的解释。如果我们能通过其他变量预测 $T$ ，意味着它并非随机。然而，我们可以通过控制其他可用变量，使 $T$ 表现得如同随机。为此，我们使用线性回归基于其他变量进行预测，并取其回归残差 $\tilde{T}$。根据定义，$\tilde{T}$ 无法被已用于预测 $T$ 的其他变量 $X$ 所预测。巧妙之处在于，$\tilde{T}$ 是处理后的版本，与 $X$ 中的任何其他变量均无关联。

顺便一提，这也是线性回归的一个特性。残差总是与生成它的模型中任何变量正交或不相关：

```{dropdown} 查看 Stata 代码
```stata
* 1. Calculate residuals
predict e, resid

* 2. Verify orthogonality (dot product should be zero)
matrix accum Xe = format_ol intercept e, noconstant
matrix list Xe

* 3. Display orthogonality check results
display "Orthogonality check - dot product of residuals and:"
display " format_ol: " %9.6f Xe[1,3]
display " intercept: " %9.6f Xe[2,3]

* 4. Calculate correlation between residuals and format_ol
correlate format_ol e
matrix list r(C)

* 5. Alternative table format (similar to pandas .corr())
estpost correlate format_ol e, matrix
esttab, unstack not noobs compress

```python
e = y - X.dot(beta)
print("Orthogonality imply that the dot product is zero:", np.dot(e, X))
X[["format_ol"]].assign(e=e).corr()
```

更酷的是，这些属性不依赖于任何条件！无论数据呈现何种形态，它们都是数学上的真理。

## 非随机数据的回归分析

到目前为止，我们处理的是随机实验数据，但众所周知，这类数据难以获取。进行实验成本高昂，甚至根本不可行。要说服麦肯锡公司无偿随机提供服务，以便一劳永逸地区分其咨询服务带来的价值与那些有能力支付服务费用的公司本就经营良好的事实，是极其困难的。

因此，我们现在将深入探讨非随机或观察性数据。在接下来的例子中，我们将尝试估算额外一年教育对时薪的影响。正如你可能猜到的，进行教育实验极其困难。你不能简单地将人们随机分配到 4 年、8 年或 12 年的教育中。在这种情况下，观察性数据是我们唯一拥有的。

首先，我们估计一个非常简单的模型。我们将以受教育年限为自变量，对小时工资的对数进行回归。这里使用对数是为了让参数估计具有百分比解释（如果你从未听说过对数的这一神奇特性并想了解原因，请查看[此链接](https://stats.stackexchange.com/questions/244199/why-is-it-that-natural-log-changes-are-percentage-changes-what-is-about-logs-th))）。通过这种方式，我们将能够说明每多受一年教育，工资会增加 x%。

$
log(hwage)_i = \beta_0 + \beta_1 educ_i + u_i
$

```{dropdown} 查看 Stata 代码
```stata
* Load data and keep needed variables
import delimited "./data/wage.csv", clear

* Remove observations with missing values
drop if missing(wage, hours, educ)

* Create hourly wage variable (annual wage divided by hours worked)
gen hwage = wage/hours

* Create natural log of hourly wage for regression
gen lnhwage = ln(hwage)

/*
Run OLS regression:
Dependent variable: log hourly wage
Independent variable: years of education
*/
regress lnhwage educ

```python
wage = pd.read_csv("./data/wage.csv").dropna()
model_1 = smf.ols('np.log(hwage) ~ educ', data=wage.assign(hwage=wage["wage"]/wage["hours"])).fit()
model_1.summary().tables[1]
```

$\beta_1$ 的估计值为 0.0536，其 95%置信区间为(0.039, 0.068)。这意味着该模型预测，每增加一年教育年限，工资将增长约 5.3%。这一百分比增幅符合教育以指数方式影响工资的普遍认知：我们预期从 11 年教育（高中毕业平均水平）增加到 12 年教育的回报，会低于从 14 年（大学毕业平均水平）增加到 16 年教育的回报。

```{dropdown} 查看 Stata 代码
```stata
* Generate education years range (5-19)
range educ_vals 5 19 15

* Calculate predicted log hourly wage using regression coefficients
predict yhat
gen pred_wage = exp(yhat) if educ == 5  // Initialize with first value

* Calculate predicted wages for all education levels
forvalues i = 6/19 {
    replace pred_wage = exp(_b[_cons] + _b[educ]*`i') if educ_vals == `i'
}

* Create the plot
twoway (line pred_wage educ_vals), ///
       title("Impact of Education on Hourly Wage") ///
       xtitle("Years of Education") ///
       ytitle("Hourly Wage")

```python
from matplotlib import pyplot as plt
from matplotlib import style
style.use("fivethirtyeight")

x = np.array(range(5, 20))
plt.plot(x, np.exp(model_1.params["Intercept"] + model_1.params["educ"] * x))
plt.xlabel("Years of Education")
plt.ylabel("Hourly Wage")
plt.title("Impact of Education on Hourly Wage")
plt.show()
```

当然，并非因为我们能估计这个简单模型就意味着它是正确的。请注意我措辞谨慎，说的是它根据教育程度**预测**工资。我从未说过这种预测具有因果性。事实上，此刻你很可能有充分理由认为该模型存在偏误。由于我们的数据并非来自随机实验，我们无法判断受教育年限较长者与较短者是否具有可比性。更进一步说，基于我们对现实世界的认知，可以非常确定二者并不具备可比性。具体而言，我们可以论证受教育年限更长的人可能拥有更富裕的父母，而我们所观察到的教育年限增加伴随工资上升的现象，只是反映了家庭财富与受教育年限之间的关联。用数学术语表达，我们认为 $E[Y_0|T=0] < E[Y_0|T=1]$，即无论如何，那些受教育更多的人即便没有接受这么多年的教育，收入也会更高。若对教育持极端悲观态度，你甚至可以认为教育会因使人脱离劳动力市场并降低工作经验而减少工资。

幸运的是，在我们的数据中，我们可以获取许多其他变量。我们可以看到父母的教育程度 `meduc` 、 `feduc` ，个人的 `IQ` 分数，工作经验年限 `exper` 以及该人员当前公司的在职时间 `tenure` 。我们甚至还有一些关于婚姻状况和黑人种族的虚拟变量。

```{dropdown} 查看 Stata 代码
```stata
list in 1/5

```python
wage.head()
```

我们可以将所有额外的变量纳入模型并进行估计：

$
log(hwage)_i = \beta_0 + \kappa \ educ_i + \pmb{\beta}X_i + u_i
$

要理解这如何有助于解决偏误问题，让我们回顾多元线性回归的双变量分解。

$
\kappa = \dfrac{Cov(Y_i, \tilde{T_i})}{Var(\tilde{T_i})} 
$

该公式表明，我们可以通过父母的教育程度、智商、经验等因素来预测 `educ` 。完成这一步后，我们将得到一个与之前包含的所有变量均不相关的 `educ` 版本，即 $\tilde{educ}$。这将瓦解诸如“受教育年限更长的人之所以如此，是因为他们拥有更高的智商。教育并不会带来更高的工资，只是与智商相关，而智商才是驱动工资的因素”之类的论点。那么，如果我们在模型中纳入智商变量，那么 $\kappa$ 就代表在保持智商不变的情况下，每多接受一年教育所带来的回报。稍作停顿，思考一下这意味着什么。即便我们无法通过随机对照试验使处理组和对照组的其他因素保持一致，回归分析也能通过将这些相同因素纳入模型来实现这一点，即使数据并非随机！

```{dropdown} 查看 Stata 代码
```stata
// Define control variables
global controls iq exper tenure age married black south urban sibs brthord meduc feduc

// Create constant term (intercept)
gen intercep = 1

// Step 1: Auxiliary regression of education (endogenous treatment) on controls
reg educ $controls intercep

// Step 2: Compute residuals from auxiliary regression
predict t_tilde, residuals

// Step 3: Calculate covariance between residuals and outcome variable (lhwage)
corr t_tilde lhwage, covariance
scalar cov_ty = r(cov_12)  // Stores covariance in scalar

// Step 4: Calculate variance of residuals
sum t_tilde
scalar var_t = r(Var)  // Stores residual variance in scalar

// Step 5: Compute kappa estimator (local treatment effect)
scalar kappa = cov_ty/var_t

// Display the final result
display "Local average treatment effect (kappa) = " kappa


```python
controls = ['IQ', 'exper', 'tenure', 'age', 'married', 'black',
            'south', 'urban', 'sibs', 'brthord', 'meduc', 'feduc']

X = wage[controls].assign(intercep=1)
t = wage["educ"]
y = wage["lhwage"]

beta_aux = regress(t, X)
t_tilde = t - X.dot(beta_aux)

kappa = t_tilde.cov(y) / t_tilde.var()
kappa
```

我们刚刚估算的这个系数表明，对于智商、经验、任期、年龄等条件相同的人群，每多接受一年教育，预计每小时工资将增加 4.11%。这证实了我们最初的怀疑：仅包含 `educ` 的简单初始模型存在偏误。同时，该结果也验证了这种偏误高估了教育的影响——在控制其他变量后，教育对工资的估计效应有所下降。

如果我们更明智地利用他人编写的软件而非事必躬亲地编码，甚至可以为这一估计值设置一个置信区间。

```{dropdown} 查看 Stata 代码
```stata
// Run OLS regression with estout output
reg lhwage educ $controls
eststo ols_model

// Display results using esttab
* ssc install estout, replace
esttab ols_model, ///
    cells(b(star fmt(3)) se(par fmt(2))) ///
    stats(N r2, fmt(0 3)) ///
    title("OLS Estimation Results") ///
    label

```python
model_2 = smf.ols('lhwage ~ educ +' + '+'.join(controls), data=wage).fit()
model_2.summary().tables[1]
```

## 遗漏变量与混杂偏误

剩下的问题是：我们估计的这个参数是否具有因果性？遗憾的是，我们无法确定。可以认为，最初仅将工资对教育程度进行回归的简单模型很可能并不具备因果性。该模型遗漏了与教育程度和工资均相关的重要变量。若不对这些变量加以控制，教育程度的估计效应实际上也包含了模型中未纳入的其他变量的影响。

为了更好地理解这种偏误如何运作，假设教育影响工资的真实模型大致如下

$
Wage_i = \alpha + \kappa \ Educ_i + A_i'\beta + u_i
$

工资受教育影响，其影响程度由 $\kappa$ 的大小衡量，同时还受其他能力因素（记为向量$A$）的作用。若在模型中忽略能力变量，对 $\kappa$ 的估计将呈现如下形式：

$
\dfrac{Cov(Wage_i, Educ_i)}{Var(Educ_i)} = \kappa + \beta'\delta_{Ability}
$

其中 $\delta_{A}$ 是 $A$ 对 $Educ$ 进行回归所得的系数向量

这里的关键在于，我们得到的并非完全是我们想要的 $\kappa$ ，而是附带了一个令人烦恼的额外项 $\beta'\delta_{A}$。这一项是被省略的 $A$ 对 $Wage$的影响，乘以被省略项对已包含项 $Educ$的影响（$\beta$）。 对于经济学家来说，这是一个非常重要的概念，以至于 Joshua Angrist 把它变成了一种“口头禅”，让学生们可以在冥想中反复默念它。

```
“简约（短）等于完整（长），
加上遗漏变量的影响，
再乘遗漏对包含变量的回归。”
```

在此，短回归是指省略变量的模型，而长回归则包含所有变量。这一公式或原则为我们进一步揭示了偏误的本质。首先，如果被忽略的变量对因变量没有影响 $Y$，那么偏误项将为零。这完全合乎逻辑——在试图理解教育对工资影响时，无需控制与之无关的因素（比如田野百合的高度）。其次，若被忽略的变量对处理变量也无影响，偏误项同样为零。这一点也直观易懂：如果模型中已包含所有影响教育的因素，那么教育对工资的估计影响就不可能混杂着教育与其他同样影响工资变量之间的相关性。

![img](images/05/confused_cat.png)

简而言之，**若模型中已纳入所有混淆变量，则可认为不存在遗漏变量偏误（OVB）**。我们亦可借助因果图的知识来理解这一点。混淆变量是指**同时影响处理变量和结果的变量**。以工资为例，智商（IQ）便是一个混淆因素。高智商者往往能完成更多年的教育，因为这对他们来说更为轻松，因此可以说智商影响了教育程度。同时，高智商者通常天生具有更高生产力，因而工资也更高，故智商亦影响工资。由于混淆变量会同时作用于处理变量和结果变量，我们用指向 T 和 Y 的箭头加以标注（此处以 $W$ 表示）。此外，正因果关系用红色标示，负因果关系则以蓝色标示。

```python
g = gr.Digraph()

g.edge("W", "T"), g.edge("W", "Y"), g.edge("T", "Y")

g.edge("IQ", "Educ", color="red"), g.edge("IQ", "Wage", color="red"), g.edge("Educ", "Wage", color="red")

g.edge("Crime", "Police", color="red"), g.edge("Crime", "Violence", color="red"), 
g.edge("Police", "Violence", color="blue")

g
```

因果图极佳地描绘了我们对世界的理解，并帮助我们理解混杂偏误如何运作。在我们的第一个例子中，有一个图表显示教育影响工资：更多教育带来更高的工资。然而，智商同样影响工资，并且也影响教育：高智商既导致更多教育，也导致更高工资。如果我们在模型中不考虑智商，它对工资的部分影响将通过教育相关性传递，这将使得教育的影响看起来比实际更大。这是一个正向偏误的例子。

再举一个带有负偏误的例子，考虑警察对城市暴力影响的因果图。我们通常在现实中看到的是，警力更强的城市暴力事件也更多。这是否意味着警察导致了暴力？或许吧，但在此我认为不值得深入讨论。然而，更有可能存在一个混杂变量，使我们看到警察对暴力影响的偏颇版本。可能是增加警力减少了暴力，但第三个变量——犯罪率——既导致了更多暴力，也促使了警力增强。若不加以控制，犯罪对暴力的影响将通过警力体现，使其看似增加了暴力。这便是负偏误的一个例证。

因果图还能向我们展示回归和随机对照试验如何正确应对混杂偏误。随机对照试验通过切断混杂因素与处理变量的联系来实现这一点。通过使处理变量$T$随机化，根据定义，没有任何因素能导致其变化。

```python
g = gr.Digraph()

g.edge("W", "Y"), g.edge("T", "Y")

g.edge("IQ", "Wage", color="red"), g.edge("Educ", "Wage", color="red")

g
```

另一方面，回归分析通过比较 $T$ 的影响来实现这一点，同时将混杂因素 $W$ 固定在一个水平上。回归分析并不意味着 W 不再影响 T 和 Y，而是将其固定，使其无法影响 T 和 Y 的变化。

```python
g = gr.Digraph()

g.node("W=w"), g.edge("T", "Y")
g.node("IQ=x"), g.edge("Educ", "Wage", color="red")

g
```

现在回到我们的问题上来，我们估计的 `educ` 对工资影响的参数是否具有因果性？很遗憾地告诉大家，这取决于我们能否论证模型已囊括所有混杂因素。我个人认为并未完全包括。例如，我们未将家庭财富纳入考量。即便加入了家庭教育背景，那也只能视为财富的替代指标。此外，我们还未考虑个人抱负等因素。可能是抱负既促使受教育年限增加又带来更高工资，因此它是一个混杂因素。这表明，**对于非随机或观察性数据的因果推断，我们应始终保持审慎态度**。永远无法确定所有混杂因素是否已被完全控制。

## 核心要点

我们通过回归分析探讨了诸多内容。我们了解到回归如何用于执行 A/B 测试，以及它如何便捷地提供置信区间。接着，我们研究了回归如何解决预测问题，并作为条件期望函数（CEF）的最佳线性近似。我们还讨论了在双变量情况下，回归处理系数是处理与结果之间的协方差除以处理的方差。扩展到多变量情形时，我们弄清了回归如何赋予处理系数一种“部分剔除”解释：即在保持其他所有包含变量不变的情况下，结果随处理的变化量。这正是经济学家喜欢称之为“其他条件不变”（ceteris paribus）的概念。

最后，我们转向了对偏误的理解。我们学习了那句口诀：

>短回归 = 长回归 + 遗漏变量的效应 × 遗漏变量对包含变量的回归系数

这让我们看清了偏误产生的机制。我们发现，遗漏变量偏误的根源是混杂变量：即同时影响处理和结果的变量。最后，我们借助因果图，看到了随机对照试验（RCT）和回归如何在不同条件下解决混杂问题。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 06 - 分组与虚拟变量回归


## 分组数据回归

并非所有数据点生而平等。再次审视我们的 ENEM 数据集时，相较于小型学校的成绩，我们更信任大规模学校的成绩。这并非意味着大型学校更优越，而是因其规模庞大意味着方差更小。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "/Users/xuwenli/Library/CloudStorage/OneDrive-个人/DSGE建模及软件编程/教学大纲与讲稿/应用计量经济学讲稿/python-causality-handbook/causal-inference-for-the-brave-and-true/data/enem_scores.csv", clear

* Create basic scatterplot	   
* Find max values
sum number_of_students, meanonly
local max_students = r(max)
sum avg_score, meanonly
local max_score = r(max)

twoway (scatter avg_score number_of_students) ///
       (scatter avg_score number_of_students if number_of_students == `max_students', ///
           mlabel(school_id) mlabcolor(black)) ///
       (scatter avg_score number_of_students if avg_score == `max_score', ///
           mlabel(school_id) mlabcolor(black)), ///
       title("ENEM Score by Number of Students") ///
       legend(off)

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from scipy import stats
from matplotlib import style
import seaborn as sns
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf

style.use("fivethirtyeight")
```

```python
np.random.seed(876)
enem = pd.read_csv("./data/enem_scores.csv").sample(200)
plt.figure(figsize=(8,4))
sns.scatterplot(y="avg_score", x="number_of_students", data=enem)
sns.scatterplot(y="avg_score", x="number_of_students", s=100, label="Trustworthy",
                data=enem.query(f"number_of_students=={enem.number_of_students.max()}"))
sns.scatterplot(y="avg_score", x="number_of_students", s=100, label="Not so Much",
                data=enem.query(f"avg_score=={enem.avg_score.max()}"))
plt.title("ENEM Score by Number of Students in the School");
```

在上述数据中，直观上，左侧的点对模型的影响应小于右侧的点。本质上，右侧的点实际上是众多其他数据点聚合而成的单一数据点。若能将其解构并对未分组数据执行线性回归，它们对模型估计的贡献确实会远超左侧单独解构的点。

这种现象，即存在一个低方差区域与另一个高方差区域并存的情况，被称为**异方差性**。简而言之，异方差性是指特征值在不同取值上方差不恒定的现象。在上述例子中，我们可以看到随着特征样本量的增加，方差逐渐减小。再举一个存在异方差的例子：若绘制工资随年龄变化的图表，会发现年长者的工资方差明显高于年轻群体。然而，迄今为止，导致方差差异最常见的原因仍是分组数据。

类似上述的分组数据在数据分析中极为常见。其中一个原因是保密性要求。政府和企业无法公开个人数据，否则将违反其必须遵守的数据隐私法规。当需要向外部研究人员提供数据时，他们只能通过分组方式导出数据。这种方法使得个体被归入群体之中，从而无法单独识别具体身份。

幸运的是，回归分析能很好地处理这类数据。为了理解其原理，我们首先以未分组的数据为例，比如之前提到的工资与教育年限数据。该数据集每位工人对应一行记录，因此我们不仅知道其中每个人的工资，还了解其受教育年限。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/wage.csv", clear	   
	   
list in 1/5	  

```python
wage = pd.read_csv("./data/wage.csv")[["wage", "lhwage", "educ", "IQ"]]

wage.head()
```

当我们运行回归模型来探究教育年限与小时工资对数之间的关联时，得到了以下结果。

```{dropdown} 查看 Stata 代码
```stata
* Run regression and store results
regress lhwage educ
eststo model1

* Export publication-ready table (English output)
esttab model1 using "wage_regression.rtf", ///
    replace ///
    b(3) se(3) ///
    star(* 0.05 ** 0.01 *** 0.001) ///
    stats(N r2_a, fmt(0 3) labels("Observations" "Adj. R-squared")) ///
    title("Returns to Schooling: Wage Regression Results") ///
    label ///
    varwidth(20) ///
    addnotes("*** p<0.001, ** p<0.01, * p<0.05") ///
    nogaps

* Simplified console output
esttab model1, cells("b(fmt(3) star)") ///
    star(* 0.05 ** 0.01 *** 0.001) ///
    stats(N r2_a, fmt(0 3)) ///
    mtitle("Model (1)") ///
    noomitted


```python
model_1 = smf.ols('lhwage ~ educ', data=wage).fit()
model_1.summary().tables[1]
```

现在，假设这些数据受到保密条款限制。数据提供方无法给出个体层面的信息。于是我们要求其按教育年限分组，仅提供每组的平均小时工资对数及组内人数。这样我们最终只得到 10 个数据点。

```{dropdown} 查看 Stata 代码
```stata
* Alternative using tabstat
tabstat lhwage, by(educ) stat(mean count) nototal	

```python
group_wage = (wage
              .assign(count=1)
              .groupby("educ")
              .agg({"lhwage":"mean", "count":"count"})
              .reset_index())

group_wage
```

别担心！回归分析并不依赖大数据也能发挥作用！我们可以为线性回归模型赋予权重，这样它会更重视样本量较大的组别，而非小样本组。注意我已将 `smf.ols` 替换为 `smf.wls` ，以实现加权最小二乘法。这一改动虽不易察觉，却将带来显著差异。

```{dropdown} 查看 Stata 代码
```stata
* Correct way to collapse with counts
preserve
    * First create an ID variable for counting
    gen id = 1
    
    * Then collapse
    collapse (mean) lhwage (sum) count=id, by(educ)
    
    * Now run WLS regression
    regress lhwage educ [aweight=count]
    eststo model2
    * Display results
    esttab model2, cells("b(star fmt(3))") ///
        stats(N r2_a, labels("N" "Adj. R2"))
restore	
	
* Display both models
esttab model1 model2, ///
    mtitle("OLS" "WLS") ///
    stats(N r2_a, fmt(0 3))	   

```python
model_2 = smf.wls('lhwage ~ educ', data=group_wage, weights=group_wage["count"]).fit()
model_2.summary().tables[1]
```

请注意分组模型中 `educ` 的参数估计值与未分组数据中的结果极为接近（本例中两者实际完全相同）。即便仅有 10 个数据点，我们仍成功获得了统计显著的系数。这是因为虽然数据点减少，但分组大幅降低了方差。同时注意标准误略小、t 统计量略大的现象——这是由于部分方差信息丢失，我们需要采取更保守的估计。数据分组后，我们无法获知组内方差的具体大小。请将上述结果与下方非加权模型的结果进行对比。

```{dropdown} 查看 Stata 代码
```stata
* First create the grouped data
preserve
    gen id = 1
    collapse (mean) lhwage (sum) count=id, by(educ)
    
    * Run OLS on group means (unweighted)
    regress lhwage educ
    
    * Display formatted results
    eststo model3
    esttab model3, cells("b(star fmt(3))") ///
        stats(N r2, labels("Groups" "R-squared")) ///
        title("OLS on Group Means") ///
        varwidth(15)
restore	 


esttab model1 model2 model3, ///
    mtitle("Original OLS" "WLS" "Grouped OLS") ///
    stats(N r2, fmt(0 3))  

```python
model_3 = smf.ols('lhwage ~ educ', data=group_wage).fit()
model_3.summary().tables[1]
```

参数估计值较小。此处的情况是，回归分析对所有数据点赋予同等权重。若将模型与分组点一同绘制，可见未加权模型过度重视了左下角的小点群，导致拟合直线的斜率偏低。

```{dropdown} 查看 Stata 代码
```stata
* Create grouped data if not already done
preserve
    gen id = 1
    collapse (mean) lhwage (sum) count=id, by(educ)
    
    * Run both regressions
    regress lhwage educ [aweight=count]  // Weighted
    predict weighted_pred
    regress lhwage educ                  // Unweighted
    predict unweighted_pred
    
    * Create the plot
    twoway (scatter lhwage educ [weight=count], ///
               msymbol(Oh) msize(*.5) mcolor(blue)) ///
           (line weighted_pred educ, lcolor(orange) lwidth(medthick)) ///
           (line unweighted_pred educ, lcolor(green) lwidth(medthick)), ///
           title("Log Wage by Education") ///
           xtitle("Years of Education") ///
           ytitle("Log Hourly Wage") ///
           legend(order(2 "Weighted" 3 "Non Weighted") pos(6) row(1)) ///
           graphregion(color(white)) ///
           plotregion(color(white)) ///
           xlabel(8(2)20) ///
           ylabel(, angle(horizontal))
restore	   

```python
sns.scatterplot(x="educ", y = "lhwage", size="count", legend=False, data=group_wage, sizes=(40, 400))
plt.plot(wage["educ"], model_2.predict(wage["educ"]), c="C1", label = "Weighted")
plt.plot(wage["educ"], model_3.predict(wage["educ"]), c="C2", label = "Non Weighted")
plt.xlabel("Years of Education")
plt.ylabel("Log Hourly Wage")
plt.legend();
```

关键在于回归分析这一卓越工具既适用于个体数据也适用于聚合数据，但在后者情况下必须使用加权处理。加权回归需要均值统计量——既非总和，也非标准差或中位数，而是协变量与因变量的均值！需注意，分组数据的加权回归结果虽不会与未分组数据的回归完全一致，但会非常接近。

![img](./images/06/heterosk.png)

我将以一个在分组数据模型中引入额外协变量的最终示例作为结束。

```{dropdown} 查看 Stata 代码
```stata
* Create grouped data with means and counts
preserve
    gen count = 1
    collapse (mean) lhwage iq (sum) count, by(educ)
    
    * Run WLS regression
    regress lhwage educ iq [aweight=count]
    
    * Display results
    estimates store model4
    display "Number of observations: " e(N)
    esttab model4, cells("b(star fmt(3))") ///
        stats(N r2_a, labels("Observations" "Adj. R-squared")) ///
        title("WLS Results with IQ") ///
        varwidth(20)
restore	   
	   
esttab model1 model2 model3 model4, ///
    mtitle("Original OLS" "WLS" "Grouped OLS" "With Covariate") ///
    stats(N r2, fmt(0 3))	   

```python
group_wage = (wage
              .assign(count=1)
              .groupby("educ")
              .agg({"lhwage":"mean", "IQ":"mean", "count":"count"})
              .reset_index())

model_4 = smf.wls('lhwage ~ educ + IQ', data=group_wage, weights=group_wage["count"]).fit()
print("Number of observations:", model_4.nobs)
model_4.summary().tables[1]
```

在此例中，除了先前加入的教育年限外，我们还把智商作为特征纳入。操作机制大体相同：获取均值与计数，以均值为回归目标，并将计数作为权重。

## 虚拟变量回归入门

虚拟变量是经过二进制列编码的分类变量。例如，假设你有一个希望纳入模型的性别变量，该变量被编码为三类：男性、女性及其他性别。

|gender（性别）|
|------|
|male  |
|female|
|female|
|other |
|male  |

由于我们的模型仅接受数值输入，需将此分类转换为数字。在线性回归中，我们采用虚拟变量实现这一转换。具体做法是将每个变量编码为 0/1 列，表示该分类是否存在。同时，我们会省略其中一个分类作为基准类别。这是因为最后一个类别实际上是其他类别的线性组合。换言之，若已知其他类别的信息，即可推导出最后一个类别。以本例而言，若某人既非女性也非其他性别，则可推断其分类为男性。

|gender（性别）|female|other|
|------|:-----|:----|
|male  |0|0|
|female|1|0|
|female|1|0|
|other |0|1|
|male  |0|0|

在讨论 A/B 测试时，我们已经处理过虚拟回归的一种简单形式。更一般地说，当我们处理二元处理变量时，会将其表示为虚拟变量。这种情况下，**该虚拟变量的回归系数即为回归线截距的增量**，或者说处理组与未处理组均值之差。

为了更具体地说明这一点，让我们考虑一个估计完成 12 年级教育对时薪影响的例子（暂且忽略混杂因素）。在下面的代码中，我们创建了一个处理虚拟变量 `T` ，用于指示受教育年限是否超过 12 年。

```{dropdown} 查看 Stata 代码
```stata
* Create hourly wage (wage divided by hours)
gen hwage = wage / hours

* Create treatment indicator (education > 12 years)
gen T = (educ > 12) if !missing(educ)

* Label variables
label variable hwage "Hourly wage"
label variable T "Higher education (educ > 12)"

* Display first 5 observations of selected variables
list hwage iq T in 1/5, noobs clean	   

```python
wage = (pd.read_csv("./data/wage.csv")
        .assign(hwage=lambda d: d["wage"] / d["hours"])
        .assign(T=lambda d: (d["educ"] > 12).astype(int)))

wage[["hwage", "IQ", "T"]].head()
```

该虚拟变量起到一种开关的作用。在我们的例子中，如果虚拟变量开启，预测值就是截距加上虚拟变量的系数；如果关闭，预测值则仅为截距。

```{dropdown} 查看 Stata 代码
```stata
* Run OLS regression of hourly wage on treatment indicator
regress hwage T

* Display formatted coefficient table
eststo model5

* Alternative using esttab for publication-quality output
esttab model5, cells("b(star fmt(3))") ///
    stats(N r2_a, labels("Observations" "Adj. R-squared")) ///
    title("Treatment Effect of Higher Education on Hourly Wage") ///
    varwidth(20)	

```python
smf.ols('hwage ~ T', data=wage).fit().summary().tables[1]
```

在此案例中，当个体未完成 12 年级学业（虚拟变量关闭）时，其平均收入为 19.9。若完成 12 年级学业（虚拟变量开启），预测值即平均收入则为 24.8449（19.9405 + 4.9044）。因此，虚拟变量的系数捕捉了均值差异，本例中该差异值为 4.9044。

更正式地说，当自变量为二分变量时（如处理指标常见的情形），回归能完美捕捉平均处理效应（ATE）。这是因为回归是对条件期望函数（CEF）$E[Y|X]$ 的线性近似，而在此特定情境下，CEF 本身就是线性的。具体而言，我们可以定义 $E[Y_i|T_i=0]=\alpha$ 和 $E[Y_i|T_i=1] = \alpha + \beta$，从而导出如下 CEF 表达式

$
E[Y_i|T_i] =  E[Y_i|T_i=0] + \beta T_i = \alpha + \beta T_i
$

而 $\beta$ 在随机数据情形下即为均值差异或平均处理效应（ATE）

$
\beta = [Y_i|T_i=1] - [Y_i|T_i=0]
$

若引入额外变量，虚拟变量的系数即转化为**条件均值差**。例如，假设我们在原有模型中添加智商（IQ）这一变量。此时，虚拟系数表示在**智商固定**的情况下，完成 12 年级学业预期带来的增长幅度。若绘制预测图，我们将看到两条平行线。从一条线跃升至另一条线的高度，即为完成 12 年级学业所预期的提升量。这也说明效应具有恒定性——无论智商高低，完成 12 年级学业对所有人的益处均等。

```{dropdown} 查看 Stata 代码
```stata
* Run OLS regression with treatment and IQ
regress hwage T iq

* Store fitted values
predict y_hat, xb

* Create separate prediction lines
twoway (line y_hat iq if T == 1, lcolor(orange) lwidth(medthick)) ///
       (line y_hat iq if T == 0, lcolor(green) lwidth(medthick)), ///
       title("Treatment Effect Conditional on IQ") ///
       subtitle("E[T=1|IQ] - E[T=0|IQ] = " + string(_b[T], "%4.2f")) ///
       ytitle("Predicted Hourly Wage") ///
       xtitle("IQ Score") ///
       legend(order(1 "T=1 (educ >12)" 2 "T=0 (educ ≤12)")) ///
       graphregion(color(white)) ///
       plotregion(color(white))	   

```python
m = smf.ols('hwage ~ T+IQ', data=wage).fit()
plt_df = wage.assign(y_hat = m.fittedvalues)

plt.plot(plt_df.query("T==1")["IQ"], plt_df.query("T==1")["y_hat"], c="C1", label="T=1")
plt.plot(plt_df.query("T==0")["IQ"], plt_df.query("T==0")["y_hat"], c="C2", label="T=0")
plt.title(f"E[T=1|IQ] - E[T=0|IQ] = {round(m.params['T'], 2)}")
plt.ylabel("Wage")
plt.xlabel("IQ")
plt.legend();
```

若将此模型转化为方程，原因便显而易见：

$
wage_i = \beta_0 + \beta_1T_i + \beta_2 IQ_i + e_i
$

此处， $\beta_1$ 代表条件均值差，在我们的案例中它是一个恒定值 3.16。通过添加交互项，我们可以使该模型更具灵活性。

$
wage_i = \beta_0 + \beta_1T_i + \beta_2 IQ_i + \beta_3 IQ_i * T_i  + e_i
$

情况变得稍微复杂一些，让我们看看这个模型中每个参数的含义。首先是截距 $\beta_0$。这个参数本身并没有特别有趣的解释，它表示当处理变量为零（即个人未完成 12 年级教育）且智商为零时的预期工资。由于我们预期任何人的智商都不会为零，因此这一参数的意义不大。至于 $\beta_1$，情况类似。该参数表示在**智商为零**时，完成 12 年级教育对工资的预期增加量。同样，因为智商不可能为零，所以这一参数也没有特别的实际意义。现在来看 $\beta_2$ ，它稍微有趣一些。它告诉我们对于**未接受处理**的群体，智商每增加一点对工资的影响。在我们的例子中，这个值大约是 0.11，意味着每增加 1 个智商点，未完成 12 年级教育的人每小时工资预计会增加 11 美分。最后，最有趣的参数是 $\beta_3$。它揭示了智商如何放大完成 12 年级教育的效果。在我们的案例中，该参数为 0.024，表示每增加 1 个智商点，完成 12 年级教育能额外带来 2 美分的工资增长。 这看起来或许不多，但比较一下智商 60 和 140 的人。前者工资将增加 1.44（60 乘以 0.024），而智商 140 的人在完成 12 年级学业时，收入将额外增加 3.36 美元（140 乘以 0.024）。

用简单的建模术语来说，这一交互项允许处理效应随特征水平（本例中仅为智商）而变化。结果是，如果我们绘制预测线，会发现它们不再平行，且那些完成 12 年级学业（T=1）的个体在智商维度上具有更高的斜率——高智商者比低智商者从毕业中获益更多。这种现象有时被称为效应修饰或异质性处理效应。

```{dropdown} 查看 Stata 代码
```stata
* 1. Run regression with interaction term
regress hwage c.T##c.iq

* 2. Store coefficients for dynamic title
scalar T_effect = _b[T]
scalar IQ_effect = _b[iq]
scalar interaction = _b[T#c.iq]

* 3. Generate predicted values
predict y_hat1, xb

* 4. Create plot with interaction lines
twoway (line y_hat1 iq if T == 1, lcolor(orange) lwidth(medthick)) ///
       (line y_hat1 iq if T == 0, lcolor(green) lwidth(medthick)), ///
       title("Treatment Effect with IQ Interaction") ///
       ytitle("Predicted Hourly Wage") ///
       xtitle("IQ Score") ///
       legend(order(1 "T=1 (educ >12)" 2 "T=0 (educ ≤12)")) ///
       graphregion(color(white)) ///
       plotregion(color(white))	   

```python
m = smf.ols('hwage ~ T*IQ', data=wage).fit()
plt_df = wage.assign(y_hat = m.fittedvalues)

plt.plot(plt_df.query("T==1")["IQ"], plt_df.query("T==1")["y_hat"], c="C1", label="T=1")
plt.plot(plt_df.query("T==0")["IQ"], plt_df.query("T==0")["y_hat"], c="C2", label="T=0")
plt.title(f"E[T=1|IQ] - E[T=0|IQ] = {round(m.params['T'], 2)}")
plt.ylabel("Wage")
plt.xlabel("IQ")
plt.legend();
```

最后，我们来看模型中所有变量均为虚拟变量的情况。为此，我们将智商（IQ）离散化为 4 个区间，并将受教育年限视为类别变量。

```{dropdown} 查看 Stata 代码
```stata
* Create IQ quartile bins (4 equal-sized groups)
xtile iq_bins = iq, nq(4)

* Label the bins for clarity
label define iq_bins 1 "Q1 (Lowest)" 2 "Q2" 3 "Q3" 4 "Q4 (Highest)"
label values iq_bins iq_bins

* Keep only needed variables
keep hwage educ iq_bins

* Display first 5 observations
list in 1/5, noobs clean

```python
wage_ed_bins = (wage
                .assign(IQ_bins = lambda d: pd.qcut(d["IQ"], q=4, labels=range(4)))
                [["hwage", "educ", "IQ_bins"]])

wage_ed_bins.head()
```

将教育视为类别变量后，我们不再将教育的影响限制于单一参数，而是允许每一年的教育都有其独特的影响。这样做增加了模型的灵活性，因为教育效应不再受参数化约束。该模型仅计算每一年教育对应的平均工资。

```{dropdown} 查看 Stata 代码
```stata
* Run regression with education category dummies
regress hwage i.educ

* Display formatted coefficient table
eststo model6

* Alternative using esttab for publication-quality output
esttab model6, cells("b(star fmt(3))") ///
    stats(N r2_a, labels("Observations" "Adj. R-squared")) ///
    title("Hourly Wage by Education Level") ///
    varwidth(20)
	
	
predict y_hat2, xb

```python
model_dummy = smf.ols('hwage ~ C(educ)', data=wage).fit()
model_dummy.summary().tables[1]
```

```{dropdown} 查看 Stata 代码
```stata
* Create scatterplot with regression line
twoway (scatter hwage educ, mcolor(blue%50) msymbol(Oh)) ///
       (line y_hat2 educ, sort lcolor(orange) lwidth(medthick)), ///
       title("Hourly Wage by Education") ///
       xtitle("Years of Education") ///
       ytitle("Hourly Wage") ///
       legend(off) ///
       graphregion(color(white)) ///
       plotregion(color(white)) ///
       xlabel(8(2)20)

```python
plt.scatter(wage["educ"], wage["hwage"])
plt.plot(wage["educ"].sort_values(), model_dummy.predict(wage["educ"].sort_values()), c="C1")
plt.xlabel("Years of Education")
plt.ylabel("Hourly Wage");
```

首先，注意到这种方法消除了关于教育如何影响工资的函数形式的任何假设。我们不再需要担心对数转换。本质上，这个模型是完全非参数的。它所做的只是计算每一年教育年限对应的工资样本均值。这可以从上图中看出，拟合线并不具有特定形式，而是对每一年教育年限样本均值的插值。我们还可以通过重构一个参数来验证这一点，例如 17 年教育年限的参数。对于此模型，该参数值为 `9.5905` 。下方可见，它仅是基准教育年限（9 年）与拥有 17 年教育年限个体之间的差异。

$
\beta_{17} = E[Y|T=17]-E[Y|T=9]
$

代价是当我们允许如此大的灵活性时，会丧失统计显著性。注意某些年份的 p 值有多大。

```{dropdown} 查看 Stata 代码
```stata
* Calculate means for educ==9 and educ==17
sum hwage if educ == 9
scalar t0_mean = r(mean)
display "E[Y|T=9]: " %5.2f t0_mean

sum hwage if educ == 17
scalar t1_mean = r(mean)
scalar diff = t1_mean - t0_mean
display "E[Y|T=17]-E[Y|T=9]: " %5.2f diff

```python
t1 = wage.query("educ==17")["hwage"]
t0 = wage.query("educ==9")["hwage"]
print("E[Y|T=9]:", t0.mean())
print("E[Y|T=17]-E[Y|T=9]:", t1.mean() - t0.mean())
```

若在模型中纳入更多虚拟协变量，教育参数的估计值将转化为各虚拟组别效应的加权平均值：

$
E\{ \ (E[Y_i|T=1, Group_i] - E[Y_i|T=0, Group_i])w(Group_i) \ \}
$

$w(Group_i)$ 并不完全等同于组 $Var(T_i|Group_i)$ 中处理变量的方差，而是与之成比例。由此自然产生的一个问题是：为什么不使用完全非参数估计量，其中组的权重就是样本量？这确实是一个有效的估计量，但回归分析并不采用这种方法。通过利用处理变量的方差，回归分析赋予处理变量变化较大的组更多权重。这在直觉上是有道理的。如果处理变量几乎恒定（例如只有一人接受处理而其他人都未处理），那么无论样本量多大，该组提供的信息对于处理效应的了解都极为有限。

```{dropdown} 查看 Stata 代码
```stata
* Run regression with both sets of dummies
regress hwage i.educ i.iq_bins

* Store results and display formatted table
eststo model_dummy2
esttab model_dummy2, cells("b(star fmt(3))") ///
    stats(N r2_a, labels("Observations" "Adj. R-squared")) ///
    title("Wage Regression with Education and IQ Dummies") ///
    varwidth(25)	   

```python
model_dummy_2 = smf.ols('hwage ~ C(educ) + C(IQ_bins)', data=wage_ed_bins).fit()
model_dummy_2.summary().tables[1]
```

![img](./images/06/you_little_shit.png)

## 核心要点

本节开篇，我们探讨了为何某些数据点比其他数据点更为关键。具体而言，在估计线性模型时，样本量更大、方差更小的数据点应被赋予更高权重。随后，我们研究了线性回归如何巧妙处理分组匿名数据，前提是在模型中运用样本权重。

接着，我们转向虚拟变量回归。了解到通过将其构建为非参数模型，可完全不对处理效应与结果之间的函数形式做任何假设。之后，我们深入剖析了虚拟变量回归背后的直观逻辑。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 07 - 超越混杂因素


## 有效控制变量

我们已经了解到，在回归模型中添加额外的控制变量有助于识别因果效应。若控制变量为混杂因素，将其纳入模型不仅是锦上添花，更是必要条件。当缺乏经验者见此情形，常会本能地将所有可测变量塞入模型。在当今大数据时代，这轻易就能超过 1000 个变量。事实证明，这不仅无必要，反而可能损害因果识别。现在，我们将关注点转向非混杂因素的控制变量。首先探讨有益的控制变量，随后再深入剖析有害的控制变量。

以一个激励性案例为例，假设你是某金融科技公司催收团队的数据科学家。你的下一项任务是评估发送债务协商邮件的影响。你的响应变量是逾期客户的还款金额。

为解答此问题，您的团队从逾期客户库中随机选取 5000 名客户进行一项随机测试。对每位客户，您抛掷硬币决定：若为正面，则向该客户发送邮件；反之，则将其留作对照组。通过此测试，您期望量化该邮件能额外产生多少收益。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/collections_email.csv", clear


list in 1/5

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from scipy import stats
from matplotlib import style
import seaborn as sns
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf
import graphviz as gr

style.use("fivethirtyeight")
```

```python
data = pd.read_csv("./data/collections_email.csv")
data.head()
```

由于数据是随机生成的，可知简单的均值差异即可估计平均处理效应。换言之，除了随机分配外，没有任何因素能导致处理，因此潜在结果独立于处理：$(Y_0, Y_1)\perp T$。 

$
ATE = E[Y|T=1] - E[Y|T=0]
$

鉴于你足够聪明，并希望为估计值设置一个置信区间，于是采用了线性回归方法。

```{dropdown} 查看 Stata 代码
```stata
* Calculate difference in means
sum payments if email == 1
scalar mean_treat = r(mean)

sum payments if email == 0
scalar mean_control = r(mean)

display "Difference in means: " %5.2f (mean_treat - mean_control)

* Run regression (equivalent to t-test)
regress payments email

```python
print("Difference in means:",
      data.query("email==1")["payments"].mean() - data.query("email==0")["payments"].mean())

model = smf.ols('payments ~ email', data=data).fit()
model.summary().tables[1]
```

遗憾的是，估算的平均处理效应（ATE）为-0.62，这一结果相当反常。发送电子邮件怎会导致逾期客户的付款金额低于平均水平？不过，由于 P 值过高，这一结果可能并无实际意义。接下来你该怎么做？是垂头丧气地回到团队宣布测试无定论、需要更多数据？且慢。

注意你的数据中还包含其他有趣的列。例如， `credit_limit` 代表客户逾期前的信用额度， `risk_score` 对应邮件发送前对客户风险的评估值。可以合理推测，信用额度和风险等级很可能是预测还款情况的有效指标。但这些信息如何发挥作用呢？

首先，我们需要理解为何即使处理效果确实存在，我们仍可能无法在统计上发现其显著性。原因或许如本例所示，处理对结果的影响微乎其微。细想之下，促使人们偿还债务的主要因素大多超出了催收部门的控制范围——人们还款是因为找到了新工作、改善了财务状况或收入等。用统计学术语来说，**付款行为的变异性更多是由电子邮件之外的其他因素所解释的**。

为了直观理解这一点，我们可以绘制付款金额与处理变量（电子邮件）的散点图。图中红色线条展示了模型的拟合线。为增强可视化效果，我在电子邮件变量中加入了少量噪声，以避免数据点完全集中在 0 或 1 的位置。

```{dropdown} 查看 Stata 代码
```stata
* Create jittered email variable
gen email_jitter = email + rnormal(0, 0.01)

* Create scatterplot with jitter
twoway (scatter payments email_jitter, mcolor(blue%80) jitter(5)) ///
       (function y = _b[_cons] + _b[email]*x, range(-0.2 1.2) lcolor(orange) lwidth(medthick)), ///
       title("Payment by Email Treatment") ///
       xtitle("Email Treatment") ///
       ytitle("Payments") ///
       legend(off) ///
       graphregion(color(white)) ///
       plotregion(color(white)) ///
       xlabel(0 "Control" 1 "Treated", noticks) ///
       xscale(range(-0.3 1.3))

```python
sns.scatterplot(x="email", y="payments", 
                alpha=0.8,
                data=data.assign(email=data["email"] + np.random.normal(0, 0.01, size=len(data["email"]))))
plt.plot(np.linspace(-0.2, 1.2), model.params[0] + np.linspace(-1, 2) * model.params[1], c="C1")
plt.xlabel("Email")
plt.ylabel("Payments");
```

从图中可见，单一处理组内的付款金额波动极为剧烈。视觉上，两组数据都呈现出从略低于 400 到 1000 的大幅跨度。如果电子邮件的影响仅在 5.00 或 10.00 雷亚尔量级，那么在如此巨大的变异性中难以察觉其作用也就不足为奇了。

幸运的是，回归分析能帮助我们降低这种变异性。关键在于运用额外的控制变量。**若某一变量能有效预测结果，它将解释结果中的大量方差**。如果风险和信用额度是付款行为的良好预测指标，通过控制这些变量，我们便能更轻松地识别电子邮件对付款行为的影响。回顾回归分析的工作原理，这一点便有了直观的解释。在回归模型中添加额外变量意味着在考察处理效应时保持这些变量恒定。因此，逻辑上，当我们观察风险和信用额度水平相近的客户时，响应变量 `payments` 的方差应当更小。换言之，若风险和信用额度能准确预测付款行为，那么具有相似风险和信用额度的客户其付款水平也应相近，从而展现出更小的方差。

![img](./images/07/y-pred.png)

为了说明这一点，我们采用部分剔除的方法将回归分解为两个步骤。首先，我们将处理变量（电子邮件）和结果变量（付款）对额外控制变量（信用额度和风险评分）进行回归。接着，在第一步得到的基础上，我们将处理变量的残差对付款的残差进行回归。（这纯粹是教学性质的，实际操作中无需如此繁琐）。

```{dropdown} 查看 Stata 代码
```stata
* Step 1: Regress email on covariates
regress email credit_limit risk_score
predict res_email, resid

* Step 2: Regress payments on same covariates
regress payments credit_limit risk_score
predict res_payments, resid

* Step 3: Regress payment residuals on email residuals
regress res_payments res_email

```python
model_email = smf.ols('email ~ credit_limit + risk_score', data=data).fit()
model_payments = smf.ols('payments ~ credit_limit + risk_score', data=data).fit()

residuals = pd.DataFrame(dict(res_payments=model_payments.resid, res_email=model_email.resid))

model_treatment = smf.ols('res_payments ~ res_email', data=residuals).fit()
```

这降低了因变量的方差。通过将支付金额对信用额度和风险进行回归并获取该模型的残差，我们创建了一个比原始变量变异性小得多的新因变量。最后一个模型还揭示了具有有效标准误差估计的 `ATE` 。

出于好奇，我们还可以验证预测处理方式的模型不应能降低其方差。这是因为邮件设计上就是随机的，因此无法被任何因素预测。

```python
print("Payments Variance", np.var(data["payments"]))
print("Payments Residual Variance", np.var(residuals["res_payments"]))

print("Email Variance", np.var(data["email"]))
print("Email Residual Variance", np.var(residuals["res_email"]))

model_treatment.summary().tables[1]
```

注意到支付金额的方差从 10807 降至 5652。在控制了风险和信用额度后，我们几乎将其减少了一半。同时也要注意到，我们未能降低处理邮件的变异性。这是合理的，因为风险和信用额度并不能预测邮件（根据随机性的定义，没有任何因素可以做到）。

现在，我们看到了更为合理的结果。这一新的估计值告诉我们，预计收到邮件的顾客平均比对照组多支付 4.4 雷亚尔。此估计值如今在统计上显著区别于零。我们还可以观察到各对照组内的方差现在有所降低。

```{dropdown} 查看 Stata 代码
```stata
* Create the residuals plot
twoway (scatter res_payments res_email, mcolor(blue%50) msymbol(Oh)) ///
       (lfit res_payments res_email, lcolor(orange) lwidth(medthick)), ///
       title("Partial Regression Plot") ///
       xtitle("Email Residuals (orthogonal to covariates)") ///
       ytitle("Payments Residuals (orthogonal to covariates)") ///
       legend(off) ///
       graphregion(color(white)) ///
       plotregion(color(white)) ///
       xlabel(-0.7(0.2)1) ///
       ylabel(, angle(horizontal))

```python
sns.scatterplot(x="res_email", y="res_payments", data=residuals)
plt.plot(np.linspace(-0.7, 1), model_treatment.params[0] + np.linspace(-1, 2) * model_treatment.params[1], c="C1")
plt.xlabel("Email Residuals")
plt.ylabel("Payments Residuals");
```

如前所述，我们这样做是出于教学目的。实际操作中，您只需将控制变量与处理变量一同加入回归模型，所得估计值将完全相同。

```{dropdown} 查看 Stata 代码
```stata
regress payments email credit_limit risk_score

```python
model_2 = smf.ols('payments ~ email + credit_limit + risk_score', data=data).fit()
model_2.summary().tables[1]
```

总结来说，任何时候如果我们有一个对结果有良好预测性的控制变量，即便它不是混杂因素，将其纳入模型都是明智之举。这有助于降低我们处理效应估计的方差。以下是这种情况在因果图中的示意图。

```python
g = gr.Digraph()
g.edge("X", "Y"), g.edge("T", "Y")
g.node("T", color="gold")

g.node("email", color="gold")
g.edge("credit_limit", "payments")
g.edge("risk_score", "payments")
g.edge("email", "payments")

g
```

## 多数有害的控制变量

作为第二个激励性例子，让我们考虑一个涉及两家医院的药物测试场景。这两家医院都在对一种处理特定疾病的新药进行随机试验，关注的结果是住院天数。若处理有效，将减少患者的住院时间。其中一家医院的政策是向 90%的患者提供真实药物，而 10%接受安慰剂；另一家医院则采取不同策略：随机为 10%的患者提供药物，其余 90%接受安慰剂。你还被告知，通常给予 90%真实药物和 10%安慰剂的那家医院接收的病例病情更为严重。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/hospital_treatment.csv", clear


list in 1/5

reg days treatment

reg days treatment if hospital == 0

reg days treatment if hospital == 1

reg days treatment severity

reg days treatment severity hospital


```python
hospital = pd.read_csv("./data/hospital_treatment.csv")
hospital.head()
```

由于你正在处理随机化数据，你的第一反应是简单地对结果与处理进行回归分析。

```python
hosp_1 = smf.ols('days ~ treatment', data=hospital).fit()
hosp_1.summary().tables[1]
```

但你发现了一些反直觉的结果。处理为何会增加住院天数？答案在于我们实际上在进行两项不同的实验。病情严重程度与住院时间呈正相关，而由于收治重症患者更多的医院也倾向于使用更多药物，药物便与更长的住院时间产生了正相关。当我们综合考察两家医院时，发现 $E[Y_0|T=0]<E[Y_0|T=1]$ ——即未接受处理者的潜在结局平均低于接受处理者，因为轻症医院中未处理患者比例更高。换言之，病情严重程度作为混杂因素，既决定了患者入住的医院，也影响了接受药物处理的概率。

解决这个问题有两种方法。第一种是单独查看每家医院的 ATE，但这违背了利用两家医院数据的初衷。

```python
hosp_2 = smf.ols('days ~ treatment', data=hospital.query("hospital==0")).fit()
hosp_2.summary().tables[1]
```

```python
hosp_3 = smf.ols('days ~ treatment', data=hospital.query("hospital==1")).fit()
hosp_3.summary().tables[1]
```

在这种情况下，我们确实得到了平均处理效应（ATE）的直观结果。看起来现在药物实际上减少了住院天数。然而，由于我们分别考察每家医院的数据，数据点不足，因此无法得出具有统计学显著性的结果。

另一种方法是利用回归的力量，通过将严重程度纳入模型来控制它。

```python
hosp_4 = smf.ols('days ~ treatment + severity', data=hospital).fit()
hosp_4.summary().tables[1]
```

接下来出现的问题是，我们是否也应该将医院纳入模型中？毕竟，我们知道医院决定了处理方式，对吧？确实如此，但一旦我们控制了病情严重程度，医院与住院天数这一结果就不再相关。而我们知道，要成为混杂因素，变量必须同时影响处理和结果。在此案例中，我们有一个仅影响处理的变量。

但或许控制它确实能降低方差，对吧？然而，事实并非如此。要使控制变量能降低方差，它必须是对结果而非处理有良好预测性的变量，而此处的情况恰恰相反。

不过，我们或许还是想控制它，对吧？这总不会有坏处……还是说，真的会有？

```{dropdown} 查看 Stata 代码
```stata
* Step 1: Regress treatment on covariates
regress treatment severity i.hospital
predict res_treatment, resid

* Step 2: Regress days on same covariates
regress days severity i.hospital
predict res_days, resid

* Step 3: Regress days residuals on treatment residuals
regress res_days res_treatment

```python
hosp_5 = smf.ols('days ~ treatment + severity + hospital', data=hospital).fit()
hosp_5.summary().tables[1]
```

出乎意料的是，可能真的有坏处！

![img](./images/07/shocked.png)

在严重程度之上增加医院作为控制变量，反而给我们的平均处理效应（ATE）估计量引入了更多的方差。这是怎么回事？答案在于回归系数标准误的公式中。

$
\hat{\sigma}^2 = \dfrac{1}{n-2} \sum( y_i - \hat{y}_i )^2
$

$
\text{Var}(\hat{\beta}_2) = \dfrac{\sigma^2}{\sum(x_i - \bar{x})^2}
$

从该公式可以看出，标准误差与变量 $X$ 的方差成反比。这意味着，如果 $X$ 变化不大，将难以估计其对结果的影响。这一点在直觉上也讲得通。举个极端例子，假设你想评估某种药物的效果，于是对 10000 人进行测试，但其中仅 1 人接受了处理。这将使寻找平均处理效应（ATE）变得极为困难，我们不得不依赖将单个个体与其余所有人进行比较。换言之，我们需要处理变量存在大量变异，才能更容易发现其影响。

至于为什么将医院纳入模型会增加我们估计的误差，是因为医院是处理的一个良好预测变量，但并不是结果的预测变量（在控制了疾病严重程度之后）。因此，通过预测处理，医院实际上降低了模型的方差！再次地，我们可以将回归模型分解为两个步骤来观察这一点。

```{dropdown} 查看 Stata 代码
```stata
* Calculate raw treatment variance
sum treatment
display "Treatment Variance: " %6.3f r(Var)

* Calculate residualized treatment variance
sum res_treatment
display "Treatment Residual Variance: " %6.3f r(Var)

* Calculate variance explained by covariates
display "Variance Explained by Controls: " %6.3f (`r(Var)' - r(Var))

* Calculate sigma_hat (MSE)
quietly regress res_days res_treatment
scalar sigma_hat = e(rmse)^2  // MSE = RMSE squared

* Calculate sum of squared residuals for treatment
quietly sum res_treatment
scalar ssr_treatment = r(Var)*(r(N)-1)  // Var*N = sum of squares

* Compute variance and SE of coefficient
scalar var_beta = sigma_hat/ssr_treatment
display "SE of the Coefficient: " %6.4f sqrt(var_beta)

* Verification against Stata's built-in calculation
matrix list e(V)  // Shows full variance-covariance matrix
display "Stata's SE: " %6.4f sqrt(e(V)[1,1])

```python
model_treatment = smf.ols('treatment ~ severity + hospital', data=hospital).fit()
model_days = smf.ols('days ~ severity + hospital', data=hospital).fit()

residuals = pd.DataFrame(dict(res_days=model_days.resid, res_treatment=model_treatment.resid))

model_treatment = smf.ols('res_days ~ res_treatment', data=residuals).fit()

model_treatment.summary().tables[1]
```

```python
print("Treatment Variance", np.var(hospital["treatment"]))
print("Treatment Residual Variance", np.var(residuals["res_treatment"]))
```

另外，不必仅听信我的一面之词！你可以自行验证上述 SE 公式的正确性：

```python
sigma_hat = sum(model_treatment.resid**2)/(len(model_treatment.resid)-2)
var = sigma_hat/sum((residuals["res_treatment"] - residuals["res_treatment"].mean())**2)
print("SE of the Coeficient:", np.sqrt(var))
```

所以关键在于，我们应当加入那些既与处理变量又与结果变量相关的控制变量（即混杂因素），如上文模型中的严重程度。同时，也应纳入能良好预测结果变量的控制项，即便它们并非混杂因素，因其有助于降低估计的方差。然而，**切勿**仅因变量能有效预测处理变量就将其加入控制，这反而会增大估计的方差。

这是用因果图表示该情况的示意图。

```python
g = gr.Digraph()

g.edge("X", "T"), g.edge("T", "Y")
g.node("T", color="gold")

g.node("treatment", color="gold")
g.edge("severity", "hospital")
g.edge("severity", "days")
g.edge("hospital", "treatment")
g.edge("treatment", "days")

g
```

## 有害控制变量 - 选择偏误

让我们回到催收邮件的例子。记得邮件是随机分配给客户的。我们已经解释了 `credit_limit` 和 `risk_score` 的含义。现在，来看剩下的变量。 `opened` 是一个虚拟变量，表示客户是否打开了邮件。 `agreement` 是另一个虚拟变量，标记客户在收到邮件后是否联系了催收部门协商债务。你认为以下哪种模型更合适？第一种是包含处理变量加上 `credit_limit` 和 `risk_score` 的模型；第二种则额外加入了 `opened` 和 `agreement` 虚拟变量。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/collections_email.csv", clear


list in 1/5

* Run the regression
regress payments email credit_limit risk_score
* Store results and display formatted table
estimates store email_model
esttab email_model, cells("b(star fmt(3)) se(par fmt(3))") ///
    stats(N r2_a, labels("Observations" "Adj. R-squared")) ///
    title("Email Treatment Effect with Controls") ///
    varwidth(25)

* Run regression with full controls
regress payments email credit_limit risk_score opened agreement

* Store and display results
estimates store email_full
esttab email_full, cells("b(star fmt(3)) se(par fmt(3))") ///
    stats(N r2_a, labels("Observations" "Adj. R-squared")) ///
    title("Email Treatment Effect with Full Controls") ///
    varwidth(25) ///
    order(email credit_limit risk_score opened agreement)

* Display both models
esttab email_model email_full, ///
    mtitle("email_model" "email_full") ///
    stats(N r2_a, fmt(0 3))	

```python
email_1 = smf.ols('payments ~ email + credit_limit + risk_score', data=data).fit()
email_1.summary().tables[1]
```

```python
email_2 = smf.ols('payments ~ email + credit_limit + risk_score + opened + agreement', data=data).fit()
email_2.summary().tables[1]
```

第一个模型在电子邮件方面发现了统计学上显著的结果，而第二个模型则没有。但或许第二个模型才是正确的，电子邮件实际上并无效果。毕竟，这个模型控制了更多因素，理应更为稳健，对吧？此刻你大概已明白事实并非如此。剩下的问题便是探究真相究竟为何。

我们知道必须加入混杂变量，即那些同时影响处理和结果的变量。我们也清楚，添加能很好预测结果的控制变量是个不错的做法，虽非必须，但有益无害。同样，我们明白仅能预测处理的控制变量不宜加入，这虽非致命错误，但最好避免。那么， `opened` 和 `agreement` 属于哪类控制变量呢？实际上，它们两者皆非上述类型。

仔细想想， `opened` 和 `agreement` 肯定与邮件有关联。毕竟，若未收到邮件，你便无法打开它；而且我们也提到过，协议仅考虑邮件发出后发生的重新协商。但**它们并非导致邮件的原因！相反，它们是由邮件引起的！**

每当我需要理解正在处理的变量类型时，我总是喜欢思考它们的因果图。让我们在这里进行这一过程。

```python
g = gr.Digraph()

g.edge("email", "payments")
g.edge("email", "opened")
g.edge("email", "agreement")
g.edge("opened", "payments")
g.edge("opened", "agreement")
g.edge("agreement", "payments")

g.edge("credit_limit", "payments")
g.edge("credit_limit", "opened")
g.edge("credit_limit", "agreement")
g.edge("risk_score", "payments")
g.edge("risk_score", "opened")
g.edge("risk_score", "agreement")

g
```

我们明白电子邮件本身并无诱因，因其设计初衷就是随机发送。同时，我们确信（或至少有充分理由相信）信用额度和风险会引发支付行为。此外，我们认为电子邮件确实能促使支付发生。就 `opened` 而言，我们认定它对支付具有因果关系。直观上，打开催收邮件的人更倾向于协商并偿还债务。同理，我们认为 `opened` 能促成协议，原因与促使支付相同。再者，已知 `opened` 由电子邮件触发，且有理由推测不同风险等级和信用额度的人群打开邮件的比率各异，因此信用额度和风险同样影响邮件打开率。至于协议，我们也认为它受 `opened` 驱动。若以支付响应变量为考量，可视其为一个漏斗过程的最终结果：

$
email -> opened -> agreement -> payment 
$

我们还认为，不同风险等级和业务线的个体达成协议的倾向性各异，因此我们也将这些因素标记为促成协议的原因。至于电子邮件与协议的关系，可以提出一种观点，即有些人仅阅读邮件主题便更倾向于作出协议承诺。关键在于，电子邮件即便未经打开阅读，同样可能促成协议的达成。

从这张图表中我们注意到，opened 和 agreement 都处于从 email 到 payments 的因果路径上。因此，如果我们通过回归分析控制这两个变量，相当于在说“这是在保持 `opened` 和 `agreement` 不变的情况下 email 的效果”。然而，这两者都是 email 因果效应的一部分，所以我们不希望固定它们。相反，我们可以认为 email 之所以能增加 payments，正是因为它提高了 agreement 率。如果我们固定这些变量，就会从 email 变量中移除部分真实效应。

利用潜在结果表示法，我们可以说，由于随机化 $E[Y_0|T=0] = E[Y_0|T=1]$。然而，即便存在随机化，当我们控制一致性时，处理组和对照组便不再具有可比性。实际上，通过一些直观的思考，我们甚至能推测出它们之间的差异所在：


$
E[Y_0|T=0, Agreement=0] > E[Y_0|T=1, Agreement=0]
$

$
E[Y_0|T=0, Agreement=1] > E[Y_0|T=1, Agreement=1]
$

第一个方程明确指出，我们认为那些未收到邮件且未达成协议的人比那些收到邮件但未达成协议的人表现更优。这是因为，若处理产生积极效应，那些**即便收到邮件**仍未达成协议的个体在付款方面可能比同样未达成协议且未获得邮件额外激励的个体表现更差。至于第二个方程，那些即使未接受处理也达成协议的个体，可能比那些达成协议但受到邮件额外激励的个体表现更好。

初次阅读时可能会感到非常困惑（至少我是如此），但请务必理解其含义。如有必要，不妨再读一遍。接着，可以对已打开的变量进行类似的推理。尝试自己动手实践。

这种偏误如此普遍，以至于它有了自己的名称。混淆偏误源于未能控制一个共同原因，而选择偏误则是当我们控制了从原因到结果路径上的共同效应或中间变量时产生的。根据经验法则，模型中应始终包含混杂因素和对 $Y$ 有良好预测能力的变量。始终排除仅对 $T$ 有良好预测能力的变量、处理与结果之间的中介变量或处理与结果的共同效应变量。

![img](./images/07/selection.png)

选择偏误如此普遍，以至于连随机化也无法完全消除。更甚者，它常因不当操作而被引入，即便在随机数据中也是如此！识别和避免选择偏误更多依赖于实践而非技巧。它们往往隐藏在某些看似聪明的想法之下，使得揭露它们变得更加困难。以下是我遇到过的一些选择偏误的例子：

    1. 在估计催收策略对付款影响时，添加一个指示变量，用于表示是否支付了全部债务。
    2. 在估计教育对收入的影响时，控制白领与蓝领工作之间的差异。
    3. 在估计利率对贷款期限的影响时，控制转化率。
    4. 在估计孩子对婚外情影响时，控制婚姻幸福感。
    5. 将支付建模 E[Payments] 拆分成两个模型，一个是预测付款是否会发生的二元模型，另一个是预测在某些支付会发生的情况下，支付金额是多少：E[Payments|Payments>0]P(Payments>0)。
    
值得注意的是，所有这些观点听起来都相当合理。选择偏误往往如此。这应被视为一个警示。事实上，在认识到其危害性之前，我自己也曾多次陷入上述陷阱。其中最后一个尤为值得详述，因为它看似巧妙却让许多数据科学家措手不及。这种现象如此普遍，以至于它有了专属名称：**The Bad COP**！

### Bad COP

情况通常如下：你需要预测一个连续变量，但其分布在零值处存在过度集中现象。例如，若建立消费者支出模型，你会得到类似伽马分布但带有大量零值的数据分布。

```{dropdown} 查看 Stata 代码
```stata
* Generate the mixed distribution
clear
set obs 1700
gen spend = rgamma(5,50) in 1/1000  // Gamma-distributed spends
replace spend = 0 in 1001/1700       // Zero-spend customers

* Create histogram
hist spend, bin(20) ///
    title("Distribution of Customer Spend") ///
    xtitle("Customer Spend") ///
    ytitle("Frequency") ///
    graphregion(color(white)) ///
    plotregion(color(white))	///
    xlabel(0(100)800) ///
    note("Note: 700 zero-spend customers", size(small))

```python
plt.hist(np.concatenate([
    np.random.gamma(5, 50, 1000), 
    np.zeros(700)
]), bins=20)
plt.xlabel("Customer Spend")
plt.title("Distribution Customer Spend");
```

当数据科学家看到这一点时，脑海中首先浮现的想法是将建模过程分解为两个步骤。第一步是参与度，即 $Y > 0$ 的概率。在我们的消费示例中，这将建模客户是否决定消费。第二部分则为那些决定参与的客户建模 $Y$ ，即条件正向效应。在本例中，这指的是客户在决定消费后实际花费的金额。如果我们想估计处理 $T$ 对支出的影响，其表现形式大致如下：
 
$
E[Y|T] = E[Y|Y>0, T]P(Y>0|T)
$
 
参与模型 $P(Y_i>0|T_i)$ 并无不妥。实际上，若 $T$ 被随机分配，它将捕捉到由于处理导致的支出概率上升。此外，上述分解方式也无可指摘——根据全概率法则，这在数学上是成立的。
 
问题出在对 COP 部分的估计上。**即便在随机分配条件下**，这部分估计仍会产生偏误。直观而言，认为某些单元仅因未接受处理而为零值并非无稽之谈——处理本可使其脱离零值状态。另一方面，有些单元则永远不会呈现零值：处理或许能提升其观测结果，但即便未经处理，它们也不会归零。关键在于理解这两类单元不具备可比性：那些永不归零的单元相较于未经处理时为零的单元，其 $Y_0$ 值更高。事实上，对后者而言，$Y_0=0$。 
 
了解这一点后，如果我们剔除零值样本，处理组和对照组中将仅保留始终非零的个体。然而，这会从对照组中移除那些在干预下从零转为非零的样本，导致处理组与对照组不再具有可比性。因为对照组仅包含始终非零且具有更高 $Y_0$ 值的个体，而处理组则同时包含两种类型的单元。

在直观理解问题的基础上，让我们从数学角度进行验证。为此，我们将干预效应分解来看。在随机分配条件下，它等于均值差异
 
$$
\begin{align*} 
&E[Y|T=1] - E[Y|T=0]\\
&=E[Y|Y>0, T=1]P(Y>0|T=1) - E[Y|Y>0, T=0]P(Y>0|T=0)\\
&=\underbrace{\{P(Y>0|T=1) - P(Y>0|T=0)\}}_{Participation \ Effect} * E[Y|Y>0, T=1]\\
&+\underbrace{\{E[Y|Y>0, T=1] - E[Y|Y>0, T=0]\}}_{COP \ Effect} * P(Y>0|T=0)
\end{align*} 
$$
 
最后一个等式成立的原因在于加上并减去 $E[Y_i|Y_i>0, T_i=1]P(Y_i>0|T_i=0)$ 后重新排列了各项。这意味着平均值的差异由两部分构成：首先，是结果 $y$ 为正的概率差异，这被称为参与效应，因为它衡量了客户参与消费概率的增加。其次，是参与条件下结果的差异，即 COP 效应。到目前为止一切顺利。这一点并无错误，它是一个数学上的事实。问题出现在我们试图分别估计每一部分时。
 
如果我们进一步分析 COP 效应，这一点将变得更加明显。
 
$$
\begin{align*} 
E[Y|Y>0, T=1] - E[Y|Y>0, T=0]&=E[Y_{1}|Y_{1}>0]-E[Y_{0}|Y_{0}>0] \\
&=\underbrace{E[Y_{1} - Y_{0}|Y_{1}>0]}_{Causal \ Effect} + \underbrace{\{ E[Y_{0}|Y_{1}>0] - E[Y_{0}|Y_{0}>0] \}}_{Selection \ Bias}
\end{align*} 
$$
 
第二个等式是在我们加减 $E[Y_{i0}|Y_{i1}>0]$后得出的。当我们分解 COP 效应时，首先得到的是对参与者子群体的因果效应。在我们的例子中，这将是对那些决定花费一些钱的人的因果效应。其次，我们得到一个偏误项，即那些被分配到处理组时决定参与的人（$E[Y_{i0}|Y_{i1}>0]$）与即使没有处理也参与的人（$E[Y_{i0}|Y_{i0}>0]$）在$Y_{0}$上的差异。 在我们的案例中，这个偏误可能是负的，因为那些在处理组中花费的人，如果没有接受处理，可能会比那些即使没有处理也花费的人花费得更少 $E[Y_{i0}|Y_{i1}>0] < E[Y_{i0}|Y_{i0}>0]$。
 
![img](./images/07/cop.png)
 
现在，我知道 COP 偏误初看之下非常反直觉，因此我认为通过一个视觉示例来理解是值得的。假设我们想评估一场营销活动如何增加人们在我们的产品上的支出。这场营销活动是随机进行的，所以我们无需担心混杂因素。在这个例子中，我们可以将客户分为两个群体。首先，是那些只有在看到营销活动时才会购买我们产品的客户。我们称这些客户为节俭型。他们不会花钱，除非我们额外推动。其次是那些即使没有营销活动也会消费的客户。营销活动让他们花得更多，但即使没有看到活动，他们原本也会消费。我们称他们为富裕型客户。在图中，我用浅色和虚线展示了反事实情况。
 
![img](./images/07/cop-ex1.png)
 
要估算活动的平均处理效应（ATE），由于我们进行了随机化处理，只需比较处理组与未处理组即可。但是，假设我们采用 COP 框架，将估计分为两个模型：一个参与模型用于估计 $P(Y_i>0|T_i)$ ，另一个 COP 模型用于估计 $E[Y_i|Y_i>0]$。这样就从分析中排除了所有未消费的个体。
 
![img](./images/07/cop-ex2.png)
 
当我们这样做时，处理组和对照组不再具有可比性。可以看出，未处理组现在仅由那些即使没有活动也会消费的客户群体组成。还需注意的是，我们甚至能预知偏误的方向，它将是 $E[Y_{i0}|Y_{i1}>0] - E[Y_{i0}|Y_{i0}>0]$ 或 $E[Y_{i0}|\text{Frugal and Rich}] - E[Y_{i0}|Rich]$。这显然是负面的，因为富裕客户比节俭客户花费更多。结果，一旦我们仅筛选参与人群，即使最初由于随机化没有偏误，我们对平均处理效应（ATE）的估计也会产生偏误。我真诚地希望这能说服你像躲避瘟疫一样避免 COP。我看到太多数据科学家进行这种分离估计，却未意识到它带来的问题。

总结选择偏误时，我们必须时刻提醒自己，切勿控制那些位于处理与结果之间的变量，或是结果与处理共同影响的变量。用图形语言表述，错误控制的表现如下：

```python
g = gr.Digraph()

g.edge("T", "X_1"), g.node("T", color="gold"), g.edge("X_1", "Y"), g.node("X_1", color="red")
g.edge("T", "X_2"), g.edge("Y", "X_2"), g.node("X_2", color="red")

g
```

## 核心要点

在本节中，我们探讨了非混淆变量的情况，以及是否应将它们加入因果识别模型。我们发现，即使某些变量不预测处理$T$（即非混淆变量），若它们能良好预测结果 $y$，也应纳入模型。这是因为预测 $Y$ 可降低其方差，从而在估计因果效应时更可能获得统计显著性结果。接着，我们了解到，加入那些仅预测处理而非结果的变量是不明智的。这些变量会减少处理的变异性，加大识别因果效应的难度。最后，我们审视了选择偏误问题——这种偏误源于控制了从处理到结果的因果路径中的变量，或是处理与结果的共同效应变量。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 08 - 工具变量

## 规避遗漏变量偏误

控制遗漏变量偏误（OVB）的一种方法是直接将遗漏变量加入模型中。然而，这并非总是可行，主要原因在于我们往往缺乏关于遗漏变量的数据。例如，回到关于教育对工资影响的模型：

$
\log(\mathrm{wage})_i = \beta_0 + \kappa \ \mathrm{educ}_i + \pmb{\beta}\mathrm{Ability}_i + u_i
$

要确定教育 $\kappa$ 对 $\log\mathrm{(wage)}$  的因果效应，我们需要控制能力因素 $\mathrm{Ability}_i$。若不这样做，很可能会产生偏误，毕竟能力很可能是一个混杂因素，既影响处理变量（教育），也影响结果变量（收入）。

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from scipy import stats
from matplotlib import style
import seaborn as sns
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf
import graphviz as gr
from linearmodels.iv import IV2SLS

%matplotlib inline

pd.set_option("display.max_columns", 5)
style.use("fivethirtyeight")
```

```python
g = gr.Digraph()

g.edge("ability", "educ")
g.edge("ability", "wage")
g.edge("educ", "wage")
g
```

避免这一问题的一种方法是在衡量教育对工资影响时，控制能力水平，保持其恒定。我们可以通过在线性回归模型中纳入能力变量来实现这一点。然而，我们缺乏对能力的良好测量指标。目前最好的替代是一些非常存疑的代理变量，比如智商。

但并非全无希望。这时工具变量（Instrumental Variables）便派上用场。IV 的核心思想是找到另一个变量，该变量能影响处理变量，且仅通过处理变量与结果相关。换言之，该工具变量 $Z_i$ 与 $Y_0$ 不相关，但与 $T$ 相关。这一条件有时被称为排他性约束。

```python
g = gr.Digraph()

g.edge("ability", "educ")
g.edge("ability", "wage")
g.edge("educ", "wage")
g.edge("instrument", "educ")
g
```

若存在这样的变量，我们便可通过 IV 公式得到因果效应 $\kappa$。为此，让我们先构想理想情况下希望建立的方程。使用更通用的术语，如以 $T$ 表示处理变量、$W$  表示混杂因素，我们期望的模型如下：

$
Y_i = \beta_0 + \kappa \ T_i + \pmb{\beta}W_i + u_i
$

然而，我们缺乏关于 $W$ 的数据，因此实际只能建立如下模型：

$
Y_i = \beta_0 + \kappa\ T_i + v_i
$

$
v_i = \pmb{\beta}W_i + u_i
$

由于 $W$ 是一个混杂因素，$\mathrm{Cov}(T, v) \neq 0$。我们有一个简短而非冗长的方程。在我们的例子中，这意味着能力与教育程度相关。如果情况如此，运行短回归将由于遗漏变量而对 $\kappa$ 产生有偏估计。

现在，见证工具变量（IV）的神奇之处！由于工具 Z 仅通过 T 与结果相关，这意味着 $\mathrm{Cov}(Z,v) = 0$，否则会存在第二条通过 W 从 Z 到 Y 的路径。考虑到这一点，我们可以写出

$
\mathrm{Cov}(Z,Y) = \mathrm{Cov}(Z,\beta_0 + \kappa\ T_i + v_i) = \kappa \mathrm{Cov}(Z,T) + \mathrm{Cov}(Z, v) = \kappa \mathrm{Cov}(Z,T)
$

两边同时除以 $V(Z_i)$ 并重新排列各项后，我们得到

$
\kappa = \dfrac{\mathrm{Cov}(Y_i, Z_i)/V(Z_i)}{\mathrm{Cov}(T_i, Z_i)/V(Z_i)} = \dfrac{\text{Reduced Form}}{\text{1st Stage}} 
$

注意分子和分母都是回归系数（协方差除以方差）。分子是 Y 对 Z 回归的结果。换句话说，这是 Z 对 Y 的“影响”。记住，这并不是说 Z 导致了 Y，因为我们有要求 Z 仅通过 T 影响 Y。相反，它仅捕捉了 Z 通过 T 对 Y 的影响有多大。这个分子非常著名，甚至有自己的名字：缩减形式系数。

分母同样是一个回归系数，但此处表示的是 T 对 Z 的回归。这一回归捕捉了 Z 对 T 的影响，因其重要性而被广泛称为第一阶段系数。

理解该方程的另一个巧妙方式是通过偏导数的视角。我们可以证明 T 对 Y 的影响等于 Z 对 Y 的影响，按 Z 对 T 的影响进行缩放：

$
\kappa = \dfrac{\frac{\partial y}{\partial z}}{\frac{\partial T}{\partial z}} = \dfrac{\partial y}{\partial z} * \dfrac{\partial z}{\partial T} =  \dfrac{\partial y}{\partial T}
$

这一公式揭示的内涵比多数人理解的更为精妙，其精妙程度也远超普遍认知。通过工具变量（IV）的这种表述方式，我们实际上在表达：“由于混杂因素的存在，直接测量 T 对 Y 的影响十分困难。但 Z 对 Y 的影响则易于获取，因为 Z 与 Y 之间不存在共同成因（排他性约束）。然而，我们真正关注的是 T 而非 Z 对 Y 的效应。因此，我将先估算 Z 对 Y 的缩减形式效应，再**通过 Z 对 T 的效应进行尺度转换**，从而将效应从 Z 转换为 T。”

在工具变量为虚拟变量的简化情形中，我们还能看到工具变量估计量进一步简化为两组均值差的比值。

$
\kappa = \dfrac{E[Y|Z=1]-E[Y|Z=0]}{E[T|Z=1]-E[T|Z=0]}
$

这一比率有时被称为 **沃尔德估计量（Wald Estimator）**。重申一下，我们可以通过工具变量（IV）的故事来理解：我们关注的是 T 对 Y 的影响，这难以直接获取，因此转而研究 Z 对 Y 的效应，后者更为容易。根据定义，Z 仅通过 T 影响 Y，因此我们可以将 Z 对 Y 的作用转化为 T 对 Y 的作用。具体方法是将 Z 对 Y 的效应按 Z 对 T 的效应比例进行缩放。

## 出生季度与教育对工资的影响

迄今为止，我们一直将这些工具变量视为某种神奇变量 $Z$，它们拥有仅通过处理变量影响结果的奇妙特性。坦白说，优质工具变量如此难寻，几乎可被视为奇迹。可以说，这绝非胆小者所能轻易尝试。传闻中，芝加哥经济学院的精英们常在酒吧里讨论他们是如何灵光一现想出这样或那样的工具变量。 

![img](./images/08/good-iv.png)

不过，我们确实有一些有趣的工具变量例子，可以让事情更具体一些。我们将再次尝试估计教育对工资的影响。为此，我们将使用个人的出生季度作为工具变量 Z。

这一观点利用了美国的义务教育法。通常，法律规定孩子在入学年份的 1 月 1 日之前必须年满 6 岁。因此，年初出生的孩子入学时年龄会更大。义务教育法还要求学生必须在校学习直至 16 岁，届时他们可以合法辍学。结果是，平均而言，一年中较晚出生的孩子比年初出生的孩子接受教育的年限更长。

![img](./images/08/qob.png)

如果我们承认出生季度与能力因素无关，即它不会混淆教育对工资的影响，那么我们可以将其作为工具变量。换句话说，我们需要相信出生季度除了通过影响教育外，对工资没有其他影响。如果你不相信占星术，这是一个非常有说服力的论点。

```python
g = gr.Digraph()

g.edge("ability", "educ")
g.edge("ability", "wage")
g.edge("educ", "wage")
g.edge("qob", "educ")
g
```

为了进行这一分析，我们可以使用来自三次十年一度的人口普查数据，这些数据与[Angrist and Krueger](https://economics.mit.edu/faculty/angrist/data1/data/angkru1991)在其关于工具变量的文章中所使用的相同。该数据集包含对数工资（我们的结果变量）和受教育年限（我们的处理变量）的信息。此外，它还提供了出生季度（我们的工具变量）以及其他控制变量，如出生年份和出生州的数据。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/ak91.csv", clear


list in 1/5

```python
data = pd.read_csv("./data/ak91.csv")
data.head()
```

## 第一阶段

在使用出生季度作为工具变量前，我们必须确保其有效性。这意味着需要论证支持工具变量的两个假设条件：

1. $\mathrm{Cov}(Z, T) \neq 0$。这表明我们应具备强第一阶段，即工具变量确实能影响处理变量。
2. $Y \perp Z | T $。这就是排他性约束，表明工具变量 Z 仅通过处理 T 影响结果 Y。 

幸运的是，第一个假设是可验证的。从数据中我们可以看出 $\mathrm{Cov}(Z, T)$ 不为零。在我们的例子中，如果出生季度确实如我们所说的那样是一个工具变量，那么我们应该预期出生在一年最后一季度的人比年初出生的人受教育时间略长。在运行任何统计检验来验证这一点之前，让我们先绘制数据并用肉眼观察。

```{dropdown} 查看 Stata 代码
```stata
* 1. Create grouped data
preserve
    collapse (mean) log_wage years_of_schooling, by(year_of_birth quarter_of_birth)
    gen time_of_birth = year_of_birth + (quarter_of_birth-1)/4
    
* 2. Create the plot
    #delimit ;
    twoway (line years_of_schooling time_of_birth, lcolor(gs8) lwidth(medthin))
           (scatter years_of_schooling time_of_birth if quarter_of_birth==1, 
               msymbol(S) msize(large) mcolor(blue))
           (scatter years_of_schooling time_of_birth if quarter_of_birth==2, 
               msymbol(S) msize(large) mcolor(orange))
           (scatter years_of_schooling time_of_birth if quarter_of_birth==3, 
               msymbol(S) msize(large) mcolor(green))
           (scatter years_of_schooling time_of_birth if quarter_of_birth==4, 
               msymbol(S) msize(large) mcolor(red))
           (scatter years_of_schooling time_of_birth, 
               mlabel(quarter_of_birth) mlabcolor(white) mlabsize(medium) 
               msymbol(none) msize(zero)),
           title("Years of Education by Quarter of Birth (first stage)")
           xtitle("Year of Birth")
           ytitle("Years of Schooling")
           legend(off)
           graphregion(color(white))
           plotregion(color(white))
           xsize(15) ysize(6)
           xlabel(, grid)
           ylabel(, grid);
    #delimit cr
restore

* Create dummy variables for each quarter
foreach q in 1 2 3 4 {
    gen q`q' = (quarter_of_birth == `q') if !missing(quarter_of_birth)
    label variable q`q' "Quarter `q' of birth"
}

```python
group_data = (data
              .groupby(["year_of_birth", "quarter_of_birth"])
              [["log_wage", "years_of_schooling"]]
              .mean()
              .reset_index()
              .assign(time_of_birth = lambda d: d["year_of_birth"] + (d["quarter_of_birth"])/4))
```

```python
plt.figure(figsize=(15,6))
plt.plot(group_data["time_of_birth"], group_data["years_of_schooling"], zorder=-1)
for q in range(1, 5):
    x = group_data.query(f"quarter_of_birth=={q}")["time_of_birth"]
    y = group_data.query(f"quarter_of_birth=={q}")["years_of_schooling"]
    plt.scatter(x, y, marker="s", s=200, c=f"C{q}")
    plt.scatter(x, y, marker=f"${q}$", s=100, c=f"white")

plt.title("Years of Education by Quarter of Birth (first stage)")
plt.xlabel("Year of Birth")
plt.ylabel("Years of Schooling")
plt.show() ;
```

值得注意的是，受教育年限存在与出生季度相符的季节性模式。从视觉上看，我们可以发现，出生在一年第一季度的人几乎总是比最后一季度出生的人受教育程度低（毕竟，在控制了出生年份后，一般来说，出生年份较晚的人受教育程度更高）。

为了更加严谨，我们可以将第一阶段作为线性回归来运行。首先，我们将出生季度转换为虚拟变量：

```{dropdown} 查看 Stata 代码
```stata
* Display first 5 observations with new dummies
list year_of_birth quarter_of_birth q1 q2 q3 q4 in 1/5, noobs clean

```python
factor_data = data.assign(**{f"q{int(q)}": (data["quarter_of_birth"] == q).astype(int)
                             for q in data["quarter_of_birth"].unique()})

factor_data.head()
```

为简化起见，我们暂且仅使用最后一个季度（第四季度）作为工具变量。我们将对受教育年限（处理变量）与出生季度（工具变量）进行回归分析，以验证是否如前述图表所示，出生季度确实对教育时长产生正向影响。在此过程中，还需控制出生年份，并额外加入出生州作为控制变量。

```{dropdown} 查看 Stata 代码
```stata
* Run first-stage regression
regress years_of_schooling i.year_of_birth i.state_of_birth q4

```python
first_stage = smf.ols("years_of_schooling ~ C(year_of_birth) + C(state_of_birth) + q4", data=factor_data).fit()

print("q4 parameter estimate:, ", first_stage.params["q4"])
print("q4 p-value:, ", first_stage.pvalues["q4"])
```

数据显示，相较于其他季度出生的人群，年末第四季度出生者平均多接受 0.1 年的教育，且 p 值趋近于零。这为"出生季度是否导致受教育年限差异"的争论提供了明确结论。

![img](./images/08/incomplete-files.png)

## 缩减形式

遗憾的是，我们无法验证工具变量（IV）第二个假设条件，只能为其合理性进行讨论。我们可以表达这样一种信念：出生季度不会影响潜在收入。换言之，除教育影响外，人们的出生时间并不能反映其个人能力或其他可能导致收入差异的因素。一个有力的论证方式是说明，当我们考虑出生季度对收入的影响时，其分配效果近乎随机（实际上并非真正随机。有证据表明人们倾向于在夏末或某些节假日前后受孕。但我无法想象这种模式会通过教育以外的任何途径影响收入）。

在论证了排除限制条件的合理性后，我们可以着手进行缩减形式的分析。缩减形式旨在揭示工具变量如何影响结果变量。根据假设，这种影响完全通过对处理变量的作用实现，从而间接展现处理变量对结果变量的影响机制。让我们再次通过可视化评估这一关系，然后再进行严谨的回归分析。

```{dropdown} 查看 Stata 代码
```stata
* 1. Prepare grouped data if not already done
preserve
    collapse (mean) log_wage, by(year_of_birth quarter_of_birth)
    gen time_of_birth = year_of_birth + (quarter_of_birth-1)/4
    
    * 2. Create the plot
    #delimit ;
    twoway (line log_wage time_of_birth, lcolor(gs8) lwidth(medthin))
           (scatter log_wage time_of_birth if quarter_of_birth==1, 
               msymbol(S) msize(large) mcolor(blue))
           (scatter log_wage time_of_birth if quarter_of_birth==2, 
               msymbol(S) msize(large) mcolor(orange))
           (scatter log_wage time_of_birth if quarter_of_birth==3, 
               msymbol(S) msize(large) mcolor(green))
           (scatter log_wage time_of_birth if quarter_of_birth==4, 
               msymbol(S) msize(large) mcolor(red))
           (scatter log_wage time_of_birth, 
               mlabel(quarter_of_birth) mlabcolor(white) mlabsize(medium) 
               msymbol(none) msize(zero)),
           title("Average Weekly Wage by Quarter of Birth")
           subtitle("Reduced Form")
           xtitle("Year of Birth")
           ytitle("Log Weekly Earnings")
           legend(off)
           graphregion(color(white))
           plotregion(color(white))
           xsize(15) ysize(6)
           xlabel(, grid)
           ylabel(, grid angle(horizontal));
    #delimit cr
restore

```python
plt.figure(figsize=(15,6))
plt.plot(group_data["time_of_birth"], group_data["log_wage"], zorder=-1)
for q in range(1, 5):
    x = group_data.query(f"quarter_of_birth=={q}")["time_of_birth"]
    y = group_data.query(f"quarter_of_birth=={q}")["log_wage"]
    plt.scatter(x, y, marker="s", s=200, c=f"C{q}")
    plt.scatter(x, y, marker=f"${q}$", s=100, c=f"white")

plt.title("Average Weekly Wage by Quarter of Birth (reduced form)")
plt.xlabel("Year of Birth")
plt.ylabel("Log Weekly Earnings")
plt.show();
```

我们再次观察到按出生季度划分的收入呈现季节性模式。年末出生者的收入略高于年初出生者。为验证这一假设，我们将再次对工具变量 q4 与对数工资进行回归分析，并如第一阶段那样加入相同的额外控制变量：

```{dropdown} 查看 Stata 代码
```stata
* Run reduced form regression
regress log_wage i.year_of_birth i.state_of_birth q4

```python
reduced_form = smf.ols("log_wage ~ C(year_of_birth) + C(state_of_birth) + q4", data=factor_data).fit()

print("q4 parameter estimate:, ", reduced_form.params["q4"])
print("q4 p-value:, ", reduced_form.pvalues["q4"])
```

我们再次得到了一个显著的结果。那些在一年最后三个月出生的人，平均工资要高出 0.8%。这次的 p 值虽然不像之前那样接近于零，但仍然相当显著，仅为 0.0015。

## 手工操作工具变量

既然我们已经有了简化形式和第一阶段的结果，现在可以通过简化形式来缩放第一阶段的影响。由于第一阶段的系数大约是 0.1，这将使简化形式系数的影响几乎放大 10 倍。这将为我们提供无偏的工具变量估计，即平均因果效应：

$
\mathrm{ATE}_{IV} = \dfrac{\text{Reduced Form}}{\text{1st Stage}} 
$

```python
reduced_form.params["q4"] / first_stage.params["q4"]
```

这意味着，我们预计每多接受一年教育，工资将增加 8%。

另一种获取工具变量估计值的方法是采用两阶段最小二乘法（**2SLS**）。按照这一程序，我们首先进行与之前相同的第一阶段操作，然后在第二阶段中将处理变量替换为第一阶段的拟合值。

$
\mathrm{educ}_i = \gamma_0 + \gamma_1 \times \mathrm{q4}_i + \gamma_2\times \mathrm{yob}_i + \gamma_3\times \mathrm{sob}_i + v_i
$

$
\log(\mathrm{wage})_i = \beta_0 + \beta_1\times \mathrm{educ}_i + \beta_2\times \mathrm{yob}_i + \beta_3\times \mathrm{sob}_i + u_i
$

$
\log(\mathrm{wage})_i = \beta_0 + \beta_1 [\gamma_0 + \gamma_1 \times \mathrm{q4}_i + \gamma_2\times \mathrm{yob}_i + \gamma_3\times \mathrm{sob}_i + v_i ]  + \beta_2\times \mathrm{yob}_i + \beta_3\times \mathrm{sob}_i + u_i
$

需要注意的是，**在实施工具变量法时，我们对第二阶段添加的任何额外控制变量也应同样添加到第一阶段**。

```{dropdown} 查看 Stata 代码
```stata
* Full 2SLS (preferred approach)
ivregress 2sls log_wage (years_of_schooling = q4) i.year_of_birth i.state_of_birth

* Run 2SLS with multiple instruments
ivregress 2sls log_wage i.year_of_birth i.state_of_birth (years_of_schooling = q1 q2 q3)

* Run OLS with categorical controls
regress log_wage years_of_schooling i.state_of_birth i.year_of_birth i.quarter_of_birth

* Show first-stage F-stat for comparison (if needed)
quietly regress years_of_schooling i.quarter_of_birth i.state_of_birth i.year_of_birth
display "First-stage F-stat: " %5.2f e(F)

```python
iv_by_hand = smf.ols("log_wage ~ C(year_of_birth) + C(state_of_birth) + years_of_schooling_fitted",
                     data=factor_data.assign(years_of_schooling_fitted=first_stage.fittedvalues)).fit()

iv_by_hand.params["years_of_schooling_fitted"]
```

如您所见，这些参数完全一致。从直觉角度出发，这种理解工具变量（IV）的第二方式颇具启发性。在两阶段最小二乘法（2SLS）中，第一阶段会生成一个经过遗漏变量偏误净化处理的新处理变量版本。随后，我们在线性回归中使用这一净化后的处理变量——即第一阶段的拟合值。

然而实际操作中，我们并不手动进行工具变量分析。并非因其繁琐，而是由于第二阶段所得标准误存在些许偏误。正确的做法是始终让计算机代劳。在 Python 中，我们可以借助 [linearmodels](https://bashtage.github.io/linearmodels/) 库以规范方式运行 2SLS。

2SLS 的公式设定略有不同。我们需在公式内的方括号\[\]间加入第一阶段。本例中我们添加 `years_of_schooling ~ q4` 。额外控制变量无需显式加入第一阶段，因为只要它们在第二阶段被包含，计算机会自动处理。因此，我们将 `year_of_birth` 和 `state_of_birth` 置于第一阶段公式之外。

```python
def parse(model, exog="years_of_schooling"):
    param = model.params[exog]
    se = model.std_errors[exog]
    p_val = model.pvalues[exog]
    print(f"Parameter: {param}")
    print(f"SE: {se}")
    print(f"95 CI: {(-1.96*se,1.96*se) + param}")
    print(f"P-value: {p_val}")
    
formula = 'log_wage ~ 1 + C(year_of_birth) + C(state_of_birth) + [years_of_schooling ~ q4]'
iv2sls = IV2SLS.from_formula(formula, factor_data).fit()
parse(iv2sls)
```

再次可见，该参数与我们之前所得完全一致。额外的优势在于，我们现在拥有了有效的标准误差。基于此，可以说，我们预计平均每多接受一年教育，工资将增长 8.5%。

## 多重工具变量

利用计算机运行 2SLS 的另一项优势在于，能够便捷地引入多重工具变量。在本例中，我们将所有出生季度的虚拟变量作为教育年限的工具变量使用。

```python
formula = 'log_wage ~ 1 + C(year_of_birth) + C(state_of_birth) + [years_of_schooling ~ q1+q2+q3]'
iv_many_zs = IV2SLS.from_formula(formula, factor_data).fit()
parse(iv_many_zs)
```

在包含所有三个虚拟变量的情况下，教育回报的估计值现为 0.1，这意味着每多接受一年教育，我们预期收入平均将增加 10%。让我们将此与传统 OLS 估计进行比较。为此，我们可以再次使用 2SLS，但这次无需第一阶段。

```python
formula = "log_wage ~ years_of_schooling + C(state_of_birth) + C(year_of_birth) + C(quarter_of_birth)"
ols = IV2SLS.from_formula(formula, data=data).fit()
parse(ols)
```

教育回报率的估计值在普通最小二乘法（OLS）中低于两阶段最小二乘法（2SLS）。这表明遗漏变量偏误（OVB）可能不如最初设想的那样严重。同时，注意置信区间的差异：2SLS 的置信区间较 OLS 估计值显著更宽。我们将进一步探讨这一现象。

## 工具变量的弱点

![img](./images/08/weak-iv.png)

在使用工具变量（IV）时，需谨记我们是在间接估计平均处理效应（ATE）。估计结果同时依赖于第一阶段和第二阶段。若处理对结果的影响确实强烈，第二阶段也会表现强劲。然而，若第一阶段较弱，无论第二阶段多强都无济于事。较弱的第一阶段意味着工具变量与处理仅存在极微弱的相关性，因此我们无法通过工具变量获取关于处理的足够信息。

工具变量标准误的公式较为复杂且不够直观，为此我们将采用其他方法理解该问题。我们将模拟一组数据：处理变量 T 对结果 Y 具有 2.0 的效应，存在未观测混杂因素 U 及额外控制变量 X，并模拟多个强度不同的第一阶段工具变量。

$$
\begin{align}
X \sim & N(0, 2^2)\\
U \sim & N(0, 2^2)\\
T \sim & N(1+0.5U, 5^2)\\
Y \sim & N(2+ X - 0.5U + 2T, 5^2)\\
Z \sim & N(T, \sigma^2) \text{ for }\sigma^2 \text{ in 0.1 to 100}
\end{align}
$$

```{dropdown} 查看 Stata 代码
```stata
clear
set obs 10000
set seed 12

* Generate base variables
gen X = rnormal(0, 2)
gen U = rnormal(0, 2)
gen T = rnormal(1 + 0.5*U, 5)
gen Y = rnormal(2 + X - 0.5*U + 2*T, 5)

* Create 50 instruments with decreasing strength
forvalues i = 1/50 {
    local s = 0.1 + (100-0.1)*(`i'-1)/49  // Linear spacing from 0.1 to 100
    gen Z_`i' = rnormal(T, `s')
    label variable Z_`i' "Instrument (SD=`=round(`s',0.1)')"
}

* Display first 5 observations
list U T Y Z_1 Z_2 in 1/5, noobs clean

```python
np.random.seed(12)
n = 10000
X = np.random.normal(0, 2, n) # observable variable
U = np.random.normal(0, 2, n) # unobservable (omitted) variable
T = np.random.normal(1 + 0.5*U, 5, n) # treatment
Y = np.random.normal(2 + X - 0.5*U + 2*T, 5, n) # outcome

stddevs = np.linspace(0.1, 100, 50)
Zs = {f"Z_{z}": np.random.normal(T, s, n) for z, s in enumerate(stddevs)} # instruments with decreasing \mathrm{Cov}(Z, T)

sim_data = pd.DataFrame(dict(U=U, T=T, Y=Y)).assign(**Zs)

sim_data.head()
```

只是为了再次确认，我们可以看到 Z 和 T 之间的相关性确实在下降。

```{dropdown} 查看 Stata 代码
```stata
* Verify instrument strength pattern
foreach z in 1 10 20 30 40 50 {
    corr T Z_`z'
    display "Z_`z' Cov(T,Z): " %5.3f r(cov_12)
}

```python
corr = (sim_data.corr()["T"]
        [lambda d: d.index.str.startswith("Z")])

corr.head()
```

现在，我们将为每种工具运行一个 IV 模型，并收集 ATE 估计值和标准误差。

```{dropdown} 查看 Stata 代码
```stata
* 1. Clear memory and set random seed
clear all
set seed 12
set obs 10000

* 2. Generate base variables (identical to Python code)
gen X = rnormal(0, 2)  // Observable covariate
gen U = rnormal(0, 2)  // Unobserved confounder
gen T = rnormal(1 + 0.5*U, 5)  // Endogenous treatment
gen Y = rnormal(2 + X - 0.5*U + 2*T, 5)  // Outcome

* 3. Create 50 instruments with decreasing strength
forvalues i = 1/50 {
    local s = 0.1 + (100-0.1)*(`i'-1)/49  // Linear spacing of SDs from 0.1 to 100
    gen Z_`i' = rnormal(T, `s')  // Instruments with increasing noise
    label var Z_`i' "Instrument `i' (SD=`=round(`s',0.1)')"
}

* 4. Initialize postfile for storing results
capture postclose results
postfile results z_num b_T se_T corr using iv_simulation, replace

* 5. Run IV regressions and store results
forvalues z = 1/50 {
    * Run 2SLS regression
    quietly ivregress 2sls Y X (T = Z_`z')
    
    * Calculate correlation between T and current Z
    quietly corr T Z_`z'
    local curr_corr = r(rho)
    
    * Post results to file
    post results (`z') (_b[T]) (_se[T]) (`curr_corr')
    
    * Display progress
    display "Processed instrument Z_`z' (Corr = " %4.2f `curr_corr' ")"
}

* 6. Close postfile and load results
postclose results
use iv_simulation, clear

* 7. Create diagnostic plot
twoway (scatter se_T corr, mcolor(blue%80) msymbol(Oh)), ///
       title("IV Standard Errors by Instrument Strength") ///
       subtitle("As Cov(Z,T) decreases, SE increases") ///
       xtitle("Correlation between Z and T") ///
       ytitle("Standard Error of Treatment Effect") ///
       graphregion(color(white)) ///
       plotregion(color(white)) ///
       xlabel(0(0.2)1, grid) ///
       ylabel(, grid angle(horizontal))

* 8. Save plot
graph export "iv_diagnostic_plot.png", replace width(2000)

* 9. Show strongest instruments
gsort -corr
list in 1/10, noobs clean

```python
se = []
ate = []
for z in range(len(Zs)):
    formula = f'Y ~ 1 + X + [T ~ Z_{z}]'
    iv = IV2SLS.from_formula(formula, sim_data).fit()
    se.append(iv.std_errors["T"])
    ate.append(iv.params["T"])
```

```python
plot_data = pd.DataFrame(dict(se=se, ate=ate, corr=corr)).sort_values(by="corr")

plt.scatter(plot_data["corr"], plot_data["se"])
plt.xlabel("Corr(Z, T)")
plt.ylabel("IV Standard Error");
plt.title("Variance of the IV Estimates by 1st Stage Strength")
plt.show() ;
```

```{dropdown} 查看 Stata 代码
```stata
* Load the simulation results if not already in memory
use iv_simulation, clear

* Generate confidence interval bounds
gen ci_upper = b_T + 1.96*se_T
gen ci_lower = b_T - 1.96*se_T

* Create the enhanced scatterplot with CIs
twoway (rarea ci_upper ci_lower corr, fcolor(blue%30) lcolo(blue%30)) ///
       (scatter b_T corr, mcolor(blue) msymbol(Oh)) ///
	   (function y = 2, range(0 1) lcolor(red) lpattern(dash)), ///
       title("IV ATE Estimates by 1st Stage Strength") ///
       xtitle("Correlation between Z and T") ///
       ytitle("ATE Estimate") ///
       legend(off) ///
       graphregion(color(white)) ///
	   plotregion(color(white)) ///
       xlabel(, grid) ///
       ylabel(, grid angle(horizontal)) ///
       note("Shaded area represents 95% confidence interval", size(small))

```python
plt.scatter(plot_data["corr"], plot_data["ate"])
plt.fill_between(plot_data["corr"],
                 plot_data["ate"]+1.96*plot_data["se"],
                 plot_data["ate"]-1.96*plot_data["se"], alpha=.5)
plt.xlabel("Corr(Z, T)")
plt.ylabel("$\hat{ATE}$");
plt.title("IV ATE Estimates by 1st Stage Strength")
plt.show();
```

如上图所示，当 T 与 Z 之间的相关性较弱时，估计值波动极大。这是因为在相关性较低的情况下，标准误（SE）也会显著增加。

另一个需要注意的现象是，**两阶段最小二乘法（2SLS）存在偏误**！即便在高度相关的情况下，参数估计值仍未达到真实平均处理效应（ATE）2.0。实际上，2.0 甚至不在 95%置信区间内！2SLS 仅具有一致性，这意味着当样本量足够大时，它会趋近于真实参数值。然而，我们无法确切知道“足够大”的具体标准，只能依据经验法则来理解这种偏误的表现规律：

1. 2SLS 的偏误方向与普通最小二乘法（OLS）相同。即若 OLS 存在负/正偏误，2SLS 也会呈现相同偏误。2SLS 的优势在于，在存在遗漏变量的情况下，它至少具有一致性（而 OLS 不具备）。上例中，未观测变量 U 对结果产生负向影响，但与处理变量正相关，这将导致负偏误。因此我们看到的 ATE 估计值低于真实值（负偏误）。

2. 偏误会随着工具变量数量的增加而增大。若添加过多工具变量，2SLS 会越来越接近 OLS 的估计结果。

除了理解这种偏误的特性外，最后还要提醒**在实施工具变量法时应避免以下常见错误**：

1. 手动进行工具变量（IV）分析。正如我们所观察到的，手动操作 IV 虽可能得到正确的参数估计，但标准误差（SE）会出错。标准误差虽不至于完全偏离，但既然使用软件能获得准确的 SE，何必手动冒险？

2. 在第一阶段采用非普通最小二乘法（OLS）的其他方法。许多数据科学家接触工具变量时，常自信能优化传统做法。例如，面对虚拟处理变量时，他们可能考虑用逻辑回归替代第一阶段的 OLS，毕竟是在预测二元变量，对吧？问题在于这种做法根本错误。IV 的稳健性依赖于 OLS 独有的残差正交特性，任何非 OLS 的第一阶段方法都将导致估计偏误。（注：虽有现代技术尝试将机器学习应用于 IV，但其效果至今最多只能说是存疑）。

## 核心要点

我们在此花费了一些时间来探讨，在拥有工具变量的情况下如何规避遗漏变量偏误。工具变量是指与处理变量相关（存在第一阶段效应），但仅通过处理变量影响结果（满足排他性约束）的变量。我们以出生季度作为工具变量来估计教育对收入影响的例子进行了说明。

随后我们深入研究了利用工具变量（IV）估计因果效应的机制，即采用两阶段最小二乘法（2SLS）。同时我们也认识到，工具变量并非万能良药。当第一阶段效应较弱时，该方法可能带来较大困扰。此外，尽管 2SLS 具有一致性，但它仍是一种存在偏误的因果效应估计方法。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 09 - 不依从性与局部平均处理效应

## 初探异质性世界

此前，我们以更为传统的视角审视了工具变量（IV）。它被视为一种可利用的自然实验。而现代工具变量实践则大量借鉴了医学的观点，根据个体对工具变量的响应方式，将世界划分为四种类型的测试者。

1. 依从者
2. 永不接受者
3. 总是接受者
4. 违抗者

这一命名源自药物科学。设想你正在进行一项实验，测试某种新药对某种疾病的效果。每位测试者被分配到一种治疗方式：药物或安慰剂。依从者是指那些严格遵循分配方案的测试者——若分配到安慰剂则服用安慰剂，若分配到药物则服用药物。永不接受者则是那些拒绝服用药物的测试者，即使被分配了新药也拒不服用。而总是接受者则能通过某种方式获取新药，即便他们被分配的是安慰剂。最后，违抗者是指那些被分配到对照组却接受治疗、被分配到治疗组却选择对照的人，可以将其想象成总是与指令对着干的顽童。实际上，这类情况（指违抗者，而非孩童）并不常见，因此我们通常会忽略他们。

![img](./images/09/defiers.png)

现代工具变量法将工具视为准实验设计，其中依从性并不完美。通过这种方式，它区分了内部有效与外部有效的因果效应。提醒一下，内部有效效应是我们能够识别的效应，在特定数据和环境下是有效的。在工具变量法中，这指的是那些因工具而改变处理的个体的处理效应。另一方面，外部有效性关注的是该因果效应的预测能力，即我们能否将从样本中发现的效应推广到其他群体。例如，假设你在大学里进行了一项随机对照试验，以探究人们在有捐赠激励时是否慷慨。实验设计良好，但仅邀请经济学学生参与。结果发现他们全是自私的家伙。这是内部有效的结论，对那组数据点有效。但你能从该实验推断出人类本性自私吗？这几乎不可能。因此，我们会质疑你的实验是否具备将其结果推广的外部有效性。言归正传，回到工具变量法。

为了更具体地说明，让我们考虑一个你想通过应用内购买提升用户参与度的案例。实现这一目标的一种方法，是要求你的市场营销部门设计一条推送消息来吸引用户。他们提出了一个绝妙的设计和非常精致的用户互动方案。基于这条推送，你着手设计一项随机试验。你随机选取了 10000 名客户，并给每个人分配了 50%的概率接收这条推送。然而，在执行测试时，你注意到一些被分配接收推送的客户实际上并未收到。当你与工程师沟通时，他们解释说，这可能是因为这些客户使用的是较旧的手机，不支持营销团队设计的那种推送功能。

起初，你可能认为这没什么大不了的。你可以直接使用实际接收到的处理作为处理变量，而不是使用分配的处理，对吧？但事实证明事情并不那么简单。如果你绘制整个情况的因果图，它看起来会像这样：

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from scipy import stats
from matplotlib import style
import seaborn as sns
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf
from linearmodels.iv import IV2SLS
import graphviz as gr

%matplotlib inline

style.use("fivethirtyeight")
```

```python
g = gr.Digraph()

g.edge("push assigned", "push delivered")
g.edge("push delivered", "in app purchase")
g.edge("income", "in app purchase")
g.edge("income", "push delivered")
g.node("income", color="blue")
g
```

在因果图的上游，是推送分配环节。这一设计本身具有随机性，因此不存在任何前因影响。接着，有一个节点表示推送是否成功送达。并非所有被分配接收推送的用户都能实际收到，此处存在不依从情况。更具体地说，存在"永不接受者"：即使被分配接受干预也始终未获处理的人群。且有理由认为这种不依从并非偶然——由于使用老旧手机的用户往往无法接收推送，可以论证收入水平也影响着推送送达率。用户越富裕，拥有高性能手机的可能性越高，进而接收推送的概率也越大。最后是结果变量，即应用内购买行为。需注意我们无法观测收入数据，故无法对其进行控制。基于此背景，让我们分别考察以下两种处理方式的影响：以推送分配作为处理变量，与以实际推送送达作为处理变量。

第一种情况下，我们将通过以下均值差分来估计因果效应：

$
ATE = E[Y | pushAssigned=1] - E[Y | pushAssigned=0]
$

正如我们目前所深刻认识到的，只有当偏误 $E[Y_1] - E[Y_0]$ 为零时，这才是对 $E[Y_0|pushAssigned=0] - E[Y_0|pushAssigned=1]$ 的无偏估计。由于 `pushAssigned` 是随机的，我们知道偏误为零。那么问题就此解决了吗？并非完全如此。要知道，如果我们这样做，实际上是在回答一个与我们初衷不同的问题。我们将找到的是**处理分配而非处理本身的因果效应**。但它们是否不同，或者我们能否将处理分配的因果效应外推至平均处理效应（ATE）？换言之，处理分配的因果效应是否是对 ATE 的无偏估计？

事实证明，并非如此。由于不依从性，被分配到处理组的个体的结果会向对照组的结果方向偏移。不依从性无意中反转了处理，使得处理组和对照组在结果上更为相似。请不要将此与变量上的相似性混淆。我们希望处理组和对照组在变量上相似，这将使它们具有可比性。我们不希望的是，如果确实存在处理效果，它们在结果上却表现相似。

要理解这一点，首先假设我们有总是接受者。其中一些人会随机被分配到对照组。但即便如此，他们仍会接受处理。这使得他们本质上成为与对照组混杂的处理群体。由于这种混杂，当我们遇到不依从情况时，因果效应将更难被发现。

![img](./images/09/always_takers.png)

同理，永不接受者会使那些被分配到处理组的人看起来有点像未接受处理的人，因为他们即使被分配到处理组也不会接受处理。从这个意义上说，处理分配的因果效应偏向于零，因为不依从性缩小了可检测到的影响。另一种理解方式是设想一个极端情况。假设不依从性非常高。处理分配与处理接受无关。在这种情况下，处理接受完全是随机的。用工具变量的术语来说，这意味着我们有一个非常弱的第一阶段。用 Z 表示处理分配，我们将得到

$
E[Y|Z=1] - E[Y|Z=0] = 0
$

处理分配与结果之间将不再存在因果联系。Z 将只是一个毫无意义的随机变量漂浮在那里。 

```python
g = gr.Digraph()

g.node("push assigned")
g.edge("push delivered", "in app purchase")
g.edge("income", "in app purchase")
g.edge("income", "push delivered")
g.node("income", color="blue")
g
```

好吧，我们已经排除了使用分配的因果效应来估计处理的因果效应。那么，直接使用实际接受的处理来做估计怎么样？

$
\mathrm{ATE} = E[Y | \mathrm{push}=1] - E[Y | \mathrm{push}=0]
$

再次，我们需要思考这是否存在偏误，或者是否 $E[Y_0|\mathrm{push}=0] = E[Y_0|\mathrm{push}=1]$。仅通过观察上方的因果图，我们就知道情况并非如此。那个未测量的混杂因素——收入，潜伏在周围，肯定会把事情搞砸。正如我们之前所说，推送失败在我们的案例中是由客户使用旧手机引起的。这意味着我们很可能 $E[Y_0|\mathrm{push}=0] < E[Y_0|\mathrm{push}=1]$。我们认为情况如此，因为资金较少的客户既拥有更旧的手机，这将导致 $\mathrm{push}=0$ ，同时也降低了应用内购买的可能性 $Y_0$。

真糟糕！我们既不能用分配的处理，也不能用实际接受的处理来估计我们的平均处理效应（ATE）。但幸运的是，我们知道可以用什么：工具变量。在这里，分配的处理就是一个完美的工具变量。它几乎是随机的，并且只通过实际处理才会影响应用内购买。

## 局部平均处理效应（LATE）

局部平均处理效应明确指出了我们能够估计因果效应的群体。这也是理解工具变量（IV）的另一种方式，为我们提供了更多可用的直观见解。在现代工具变量理论中，我们将工具变量视为启动一条因果链：Z 导致 T，T 进而导致 Y。在此背景下，排他性约束意味着 Z 不会直接影响 Y，除非通过其对 T 的作用。第一阶段现在被视为 Z 对 T 的因果效应。我们还采用双重索引符号重写潜在结果，其中第一个索引表示工具变量的反事实情况，第二个索引则表示处理的反事实情况。

$
\text{Potential Outcome}=\begin{cases}
Y_i(1, 1) \ \text{if } T_i=1, \ Z_i=1\\
Y_i(1, 0) \ \text{if } T_i=1, \ Z_i=0\\
Y_i(0, 1) \ \text{if } T_i=0, \ Z_i=1\\
Y_i(0, 0) \ \text{if } T_i=0, \ Z_i=0\\
\end{cases}
$

从某种意义上说，在第一阶段中，处理变量变成了结果变量。这意味着我们也可以用潜在结果符号来表示它：

$
\text{Potential Treatment}=\begin{cases}
T_0 \ \text{if } Z_i=0 \\
T_1 \ \text{if } Z_i=1
\end{cases}
$

![img](./images/09/double_index.png)

工具变量假设现在可以重写如下：

1. $T_{0i}, T_{1i} \perp Z_i $ 和 $Y_i(T_{1i},1), Y_i(T_{0i},0) \perp Z_i $。这是独立性假设。它表明工具变量如同随机分配一般有效。换句话说，工具变量 Z 与潜在处理无关，这意味着不同工具变量组的人群具有可比性。

2. $Y_i(1, 0)=Y_i(1, 1)=Y_{i1}$ 和 $Y_i(0, 0)=Y_i(0, 1)=Y_{i0}$。这是排他性约束。它指出，若考察接受处理者的潜在结果，两个工具变量组的结果是相同的。换言之，工具变量不影响潜在结果，即工具变量仅通过处理影响最终结果。

3. $E[T_{1i}-T_{0i}] \neq 0$。这表示第一阶段的存在性。其含义是，第一阶段的潜在结果（即潜在处理）并不相同。换言之，工具变量确实对处理产生了影响。

4. $T_{i1} > T_{i0}$。这是单调性假设。其含义是，若对所有人启用工具变量，其处理水平将高于对所有人群禁用该工具变量时的处理水平。 

现在，让我们回顾一下沃尔德估计量，以进一步理解工具变量（IV）的直观含义：

$
ATE = \dfrac{E[Y|Z=1]-E[Y|Z=0]}{E[T|Z=1]-E[T|Z=0]}
$

首先，我们来看其中的第一部分，$E[Y|Z=1]$。利用排除限制条件，我们可以将 Y 用潜在结果的形式重写如下。

$
E[Y_i|Z_i=1]=E[Y_{i0} + T_{i1}(Y_{i1} - Y_{i0})|Z=1]
$

基于独立性假设，我们可以移除对 Z 的条件限制。

$
E[Y_i|Z_i=1]=E[Y_{i0} + T_{i1}(Y_{i1} - Y_{i0})]
$

通过类似的论证，我们得出

$
E[Y_i|Z_i=0]=E[Y_{i0} + T_{i0}(Y_{i1} - Y_{i0})]
$

我们现在可以将沃尔德估计量的分子部分重写如下

$
E[Y|Z=1]-E[Y|Z=0] = E[(Y_{i1}-Y_{i0})(T_{i1}-T_{i0})]
$

利用单调性，我们可知 $T_{i1}-T_{i0}$ 为 0 或 1，因此

$
E[(Y_{i1}-Y_{i0})(T_{i1}-T_{i0})] = E[(Y_{i1}-Y_{i0})|T_{i1}>T_{i0}]P(T_{i1}>T_{i0})
$

采用类似方法处理分母部分，我们得到

$
E[T|Z=1]-E[T|Z=0]=E[T_{i1}-T_{i0}]=P(T_{i1}>T_{i0})
$

综上所述，我们可以这样理解沃尔德估计量：

$
ATE = \dfrac{E[(Y_{i1}-Y_{i0})|T_{i1}>T_{i0}]P(T_{i1}>T_{i0})}{P(T_{i1}>T_{i0})}=E[(Y_{i1}-Y_{i0})|T_{i1}>T_{i0}]
$

也就是说，通过工具变量（IV）估计的平均处理效应（ATE）是在子群体 $T_{i1}>T_{i0}$ 上的 ATE。若从依从性角度考虑，这一子群体指代的是哪些人？这是指那些在工具开启时接受处理水平高于工具关闭时的群体。换言之，即依从者群体。为了便于记忆，

1. 依从者意味着 $T_{i1}>T_{i0}$
2. 永不接受者 $T_{i1}=T_{i0}=0$
3. 总是接受者 $T_{i1}=T_{i0}=1$

由此得出的结论是，工具变量(IV)对于永不接受者、总是接受者或违抗者的效应未提供任何信息，因为对于这些群体而言，处理状态并未改变！**工具变量仅能识别出依从者的处理效应**。

## 对参与度的影响

让我们通过一个案例研究来观察这一切如何展开，该研究旨在评估推送通知对应用内购买的影响。因果图即为上文所展示的，此处不再重复。我们拥有的数据既包括随机分配的工具变量——推送通知的发送状态，也包括实际接收到的处理变量——推送通知的送达情况。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/app_engagement_push.csv", clear


list in 1/5

```python
data = pd.read_csv("./data/app_engagement_push.csv")
    
data.head()
```

首先，我们运行普通最小二乘法（OLS）看看结果如何。

```{dropdown} 查看 Stata 代码
```stata
* Run OLS regression
regress in_app_purchase push_assigned push_delivered

```python
ols = IV2SLS.from_formula("in_app_purchase ~ 1 + push_assigned + push_delivered", data).fit()
ols.summary.tables[1]
```

普通最小二乘法（OLS）显示处理效应为 27.60 巴西雷亚尔，即推送使应用内购买增加了 27.6 雷亚尔。然而，我们有理由认为这一估计存在偏误。已知旧款手机在接收推送时存在问题，因此更可能的是，拥有新款手机的较富裕用户构成了合规群体。由于接受处理的群体同时具备更高消费能力，我们认为这种偏误呈正向，推送的真实影响应低于当前估值。换言之，我们很可能面临 $E[Y_0|T=0] < E[Y_0|T=1]$ 的情况。

现在，让我们尝试用工具变量法来估计这一效应。首先，进行第一阶段回归分析。

```{dropdown} 查看 Stata 代码
```stata
* First-stage regression
regress push_delivered push_assigned

```python
first_stage = IV2SLS.from_formula("push_delivered ~ 1 + push_assigned", data).fit()
first_stage.summary.tables[1]
```

看起来我们的第一阶段效果显著。被分配到推送任务的群体中，有 71.8%确实接收到了推送。这意味着大约 28%的用户属于“永不接受者”。此外，我们有充分理由相信不存在“总是接受者”，因为截距参数估计值为零。这表明若未被分配推送任务，则无人会接收到推送。鉴于实验设计，这一结果在预期之中。

现在让我们运行缩减形式：

```{dropdown} 查看 Stata 代码
```stata
* Reduced form regression
regress in_app_purchase push_assigned

```python
reduced_form = IV2SLS.from_formula("in_app_purchase ~ 1 + push_assigned", data).fit()
reduced_form.summary.tables[1]
```

缩减形式表明，处理分配的因果效应为 2.36。这意味着将某人分配到接收推送通知组，会使应用内购买增加 2.36 雷亚尔。

若将缩减形式除以前一阶段结果，我们通过处理单位的尺度调整工具变量的效应，得到 $2.3636/0.7176=3.29$ 。运行两阶段最小二乘法(2SLS)后，我们获得了相同的估计值，并额外得到了正确的标准误。

```{dropdown} 查看 Stata 代码
```stata
* Run 2SLS regression (recommended approach)
ivregress 2sls in_app_purchase (push_delivered = push_assigned)

```python
iv = IV2SLS.from_formula("in_app_purchase ~ 1 + [push_delivered ~ push_assigned]", data).fit()
iv.summary.tables[1]
```

这表明 2SLS 的结果远低于 OLS 估计值：3.29 对比 27.60。这是合理的，因为 OLS 估计的因果效应存在正向偏误。此外还需注意局部平均处理效应(LATE)。3.29 是依从者的平均因果效应。遗憾的是，我们无法对永不接受者做出任何推断。这意味着我们估计的是拥有较新手机的较富裕人群的效应。

## 核心要点

在此，我们探讨了工具变量的一种更为现代的视角。我们了解到工具变量可被视为一条因果链，其中工具引发处理，处理进而影响结果。通过这一视角，我们考察了依从性以理解工具变量估计中的平均处理效应（ATE），并发现它实际上是依从者的局部平均处理效应（LATE）。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 10 - 匹配法

## 回归到底在做什么？

正如我们目前所看到的，当我们进行处理组与对照组比较时，回归在控制额外变量方面表现得非常出色。若我们具备独立性 $(Y_0, Y_1)\perp T | X$，那么回归能通过控制 X 来识别平均处理效应(ATE)。回归实现这一过程的方式近乎神奇。为获得直观理解，让我们回顾所有 X 变量均为虚拟变量的情形。此时，回归将数据划分至各虚拟单元，并计算处理组与对照组间的均值差异。由于是在固定的 X 虚拟单元内进行，这种均值差异能保持 X 恒定。这相当于执行 $E[Y|T=1] - E[Y|T=0] | X=x$，其中 $x$ 代表一个虚拟单元（例如所有虚拟变量设为 1）。随后，回归通过加权整合各单元估计值以生成最终 ATE，其权重分配与各处理组内处理变量的方差成比例。

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from matplotlib import style
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf

import graphviz as gr

%matplotlib inline

style.use("fivethirtyeight")
```

举个例子，假设我正在估计一种药物的效果，并且我有6名男性和4名女性。我的因变量是住院天数，我希望这款药物能缩短住院时间。对于男性，药物的真实因果效应是 -3，也就是说药物可以使他们的住院时间减少3天；而对女性，效应是 -2。

让情况更复杂一点，男性受这种疾病影响更严重，因此住院时间更长，并且他们更有可能接受这种药物——6个男性中只有1人没有服药。相对地，女性对这种疾病的抵抗力更强，住院时间也更短，且她们中有50%的人服用了药物。

```python
drug_example = pd.DataFrame(dict(
    sex= ["M","M","M","M","M","M", "W","W","W","W"],
    drug=[1,1,1,1,1,0,  1,0,1,0],
    days=[5,5,5,5,5,8,  2,4,2,4]
))
```

注意，简单地比较处理组和对照组会得出一个带有负偏误的估计结果，也就是说，药物看起来没有它实际那么有效。这是可以预期的，因为我们遗漏了“性别”这个混杂变量。在这种情况下，估计的平均处理效应（ATE）小于真实值，因为男性更可能接受药物治疗，同时也更容易受到疾病的影响。

```python
drug_example.query("drug==1")["days"].mean() - drug_example.query("drug==0")["days"].mean()
```

鉴于男性真实效应为-3 而女性为-2，平均处理效应（ATE）应为

$
ATE=\dfrac{(-3*6) + (-2*4)}{10}=-2.6
$

这个估计是通过以下三个步骤完成的：1）将数据按混杂变量划分为不同的单元，在这个例子中就是男性组和女性组；2）在每个单元内分别估计处理效应；3）将这些效应按加权平均进行组合，其中权重是每个单元或协变量组的样本量。如果我们的数据中男性和女性的数量完全相等，那么估计出的平均处理效应（ATE）就会正好是两组效应的中间值，也就是 -2.5。由于数据中男性多于女性，最终的ATE估计会更接近男性组的效应值。这种估计方法被称为非参数估计，因为它对数据的生成过程没有做出任何特定假设。

如果我们使用回归来控制性别变量，就等于在模型中加入了线性关系的假设。回归同样会将数据划分为男性和女性两个组，并分别估计它们各自的效应。这一切看起来都没问题。然而，在将各组效应合并时，回归并不是根据样本数量加权的。相反，回归使用的权重与各组中处理变量的方差成正比。在我们的例子中，由于只有一个男性属于对照组，男性组中处理变量的方差小于女性组。更具体地说，男性的处理变量 T 的方差为 $0.139=1/6*(1 - 1/6)$ ，而女性的为 $0.25=2/4*(1 - 2/4)$。因此，在这个例子中，回归会对女性组给予更高的权重，最终估计的 ATE 会更接近女性的效应值 -2。

```python
smf.ols('days ~ drug + C(sex)', data=drug_example).fit().summary().tables[1]
```

虚拟变量能更直观呈现该结果，但回归分析以其独特方式在估算效应时同样保持连续变量的恒定。对于连续变量，平均处理效应(ATE)也会向协变量方差更大的方向偏移。

由此我们了解到回归分析有其独特性。它是线性的、参数化的，偏好高方差特征……这既可能是优点也可能是缺点，取决于具体情境。正因如此，掌握其他控制混杂因素的技术至关重要。这些方法不仅是因果分析工具箱中的额外工具，理解处理混杂因素的不同方式更能拓宽我们对问题的认知。基于这一考量，现在我要向各位介绍**子类估计量**！


## 子类估计量

![img](./images/10/explain.png)

当我们需要估计某项因果效应（如职业培训对收入的影响）而处理变量并非随机分配时，必须警惕混杂因素。可能存在只有积极性更高的人群才会参加培训的情况，且无论是否参加培训这些人群原本就具有更高的收入水平。我们需要在动机水平及其他潜在混杂因素基本相似的小群体中，评估培训项目的真实效果。

更一般地说，如果我们想估计某种因果效应，但由于某些变量 X 的混杂而难以实现，我们需要做的是在 X 相同的小群体内进行处理组与对照组的比较。若我们具备条件独立性  $(Y_0, Y_1)\perp T | X$，则可按下式表达平均处理效应（ATE）。

$
ATE = \int(E[Y|X, T=1] - E[Y|X, T=0])dP(x)
$

该积分的作用是遍历特征 X 分布的所有空间，计算所有这些微小空间内的均值差异，并将所有结果综合为平均处理效应。另一种理解方式是考虑一组离散特征。此时，我们可以说特征 X 分布在 K 个不同的单元 $\{X_1, X_2, ..., X_k\}$ 中，我们所做的是计算每个单元内的处理效应，并将它们合并为 ATE。在此离散情形下，将积分转换为求和，即可推导出子类估计量。


$
\hat{ATE} = \sum^K_{k=1}(\bar{Y}_{k1} - \bar{Y}_{k0}) * \dfrac{N_k}{N}
$

其中横线代表处理组结果的平均值 $Y_{k1}$, 与非处理组 $Y_{k0}$ 在第 k 个单元中的均值，$N_{k}$ 表示该单元内的观测数量。可以看出，我们为每个单元计算局部平均处理效应（ATE），并通过加权平均进行合并，权重即为各单元的样本量。在上述医学案例中，该方法的首次估计值为−2.6。

## 匹配估计量

![img](./images/10/its-a-match.png)

子类估计量在实际中应用较少（稍后将揭示原因——维度灾难问题），但它清晰展示了因果推断估计量应具备的功能：如何有效控制混杂变量。这为我们探索其他类型的估计量（如匹配估计量）提供了理论基础。

其核心理念高度相似。由于某些混杂变量 X 导致处理组与对照组初始不可比，可通过为**每个处理单元匹配相似的对照单元实现可比性**。这相当于为每个处理单元寻找未处理的"双胞胎"。通过这种配对比较，处理组与对照组重新获得可比性。

举例而言，假设我们正试图评估一项培训项目对收入的影响。以下是受训者的基本情况

```python
trainee = pd.read_csv("./data/trainees.csv")
trainee.query("trainees==1")
```

以下是非培训人员：

```python
trainee.query("trainees==0")
```

若进行简单的均值比较，我们会发现受训者的收入低于未参与该项目的人员。

```python
trainee.query("trainees==1")["earnings"].mean() - trainee.query("trainees==0")["earnings"].mean()
```

然而，观察上表可见，受训者年龄普遍小于非受训者，这表明年龄可能是一个混淆变量。为此，我们将采用年龄匹配法进行调整。具体操作如下：将处理组的第 1 单元与非处理组的第 27 单元配对（两者均为 28 岁），第 2 单元与第 34 单元配对，第 3 单元与第 37 单元配对，第 4 单元则与第 35 单元配对……当处理到第 5 单元时，需从非处理组中寻找同为 29 岁的匹配对象，但符合条件的第 37 单元已被使用。这并不构成问题，因为同一单元可重复使用。若存在多个匹配单元，则可随机选择其中之一进行配对。

这是前 7 个单元的匹配数据集样貌

```python
# make dataset where no one has the same age
unique_on_age = (trainee
                 .query("trainees==0")
                 .drop_duplicates("age"))

matches = (trainee
           .query("trainees==1")
           .merge(unique_on_age, on="age", how="left", suffixes=("_t_1", "_t_0"))
           .assign(t1_minuts_t0 = lambda d: d["earnings_t_1"] - d["earnings_t_0"]))

matches.head(7)
```

注意最后一列显示的是处理组与其匹配的未处理组在收入上的差异。若取该列的平均值，我们便得到在控制年龄因素后的平均处理效应估计值（ATET）。与之前仅采用简单均值差的方法相比，可见此时的估计结果显著偏向正值。

```python
matches["t1_minuts_t0"].mean()
```

但这只是个刻意设计的示例，仅为引入匹配概念。现实中，我们通常面对多个特征且单元间无法完美匹配的情形。此时，需定义某种邻近度度量来比较单元间的相似程度。常用的度量之一是欧几里德范数 $||X_i - X_j||$。然而，该差异对特征尺度并不具有不变性。这意味着像年龄这样取值在十这个量级的特征，在计算范数时其重要性将远低于收入这类百位量级的特征。因此，在应用范数前，需对特征进行缩放处理，使它们大致处于同一量级。

在定义了距离度量之后，我们现在可以将匹配定义为与待匹配样本最接近的邻居。用数学术语表达，匹配估计量可以写作如下形式

$
\hat{ATE} = \frac{1}{N} \sum^N_{i=1} (2T_i - 1)\big(Y_i - Y_{jm}(i)\big)
$

其中 $Y_{jm}(i)$ 代表来自另一处理组中与 $Y_i$ 最为相似的样本。我们进行 $2T_i - 1$ 双向匹配：既将处理组与对照组匹配，也将对照组与处理组匹配。

为验证该估计量，我们以药物为例进行说明。再次强调，我们的目标是探究药物对康复天数的影响。然而，该效应受到病情严重程度、性别及年龄的混杂影响。我们有理由相信，病情更严重的患者接受药物治疗的概率更高。

```python
med = pd.read_csv("./data/medicine_impact_recovery.csv")
med.head()
```

如果我们观察一个简单的均值差异 $E[Y|T=1]-E[Y|T=0]$，会发现接受治疗的患者平均比未治疗者多需要 16.9 天恢复。这很可能是由于混杂因素造成的，因为我们并不预期药物会对患者造成伤害。

```python
med.query("medication==1")["recovery"].mean() - med.query("medication==0")["recovery"].mean()
```

为了纠正这种偏误，我们将通过匹配来控制变量 X。首先，需要记得对特征进行标准化处理，否则在计算点间距离时，像年龄这样的特征会比严重程度等特征具有更高权重。为此，我们可以对特征进行标准化。

```python
# scale features
X = ["severity", "age", "sex"]
y = "recovery"

med = med.assign(**{f: (med[f] - med[f].mean())/med[f].std() for f in X})
med.head()
```

现在，进入匹配本身。我们不会编写匹配函数，而是使用 [Sklearn](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsRegressor.html) 中的 K 最近邻算法。该算法通过寻找估计或训练集中最近的数据点来进行预测。

在匹配过程中，我们需要其中的两个。其一， `mt0` ，将存储未经处理的点，并在被要求时在未处理数据中寻找匹配项。另一个， `mt1` ，则存储经过处理的点，并在需要时在已处理数据中寻找匹配项。完成此拟合步骤后，我们可以利用这些 KNN 模型进行预测，这些预测即为我们的匹配结果。

```python
from sklearn.neighbors import KNeighborsRegressor

treated = med.query("medication==1")
untreated = med.query("medication==0")

mt0 = KNeighborsRegressor(n_neighbors=1).fit(untreated[X], untreated[y])
mt1 = KNeighborsRegressor(n_neighbors=1).fit(treated[X], treated[y])

predicted = pd.concat([
    # find matches for the treated looking at the untreated knn model
    treated.assign(match=mt0.predict(treated[X])),
    
    # find matches for the untreated looking at the treated knn model
    untreated.assign(match=mt1.predict(untreated[X]))
])

predicted.head()
```

有了这些匹配项，我们现在可以应用匹配估计器公式

$
\hat{ATE} = \frac{1}{N} \sum^N_{i=1} (2T_i - 1)\big(Y_i - Y_{jm}(i)\big)
$

```python
np.mean((2*predicted["medication"] - 1)*(predicted["recovery"] - predicted["match"]))
```

通过这种匹配方式，我们可以看出药物的效果已不再积极。这意味着，在控制变量 X 的情况下，该药物平均能将康复时间缩短约 1 天。相较于原先存在偏误、预测康复时间会增加 16.9 天的估计，这已然是一个巨大的进步。

然而，我们还能做得更好。

## 匹配偏误

我们发现，如上设计的匹配估计量实际上是有偏的。为了理解这一点，让我们考虑 ATET（处理组平均处理效应）估计量而非 ATE（平均处理效应），仅因其表达更为简洁。这一直觉同样适用于 ATE。

$
\hat{ATET} = \frac{1}{N_1}\sum (Y_i - Y_j(i))
$

其中 $N_1$ 代表接受处理的个体数量，$Y_j(i)$ 为处理单元 i 的未处理匹配项。为检验偏误，我们所做的是期望能够应用中心极限定理，使得下方内容收敛于均值为零的正态分布。

$
\sqrt{N_1}(\hat{ATET} - ATET)
$

然而，这并非总是发生。若我们定义给定 X 下未处理组的平均结果为 $\mu_0(x)=E[Y|X=x, T=0]$，则有（顺便提一下，此处我省略了证明过程，因其稍偏离主题）。

$
E[\sqrt{N_1}(\hat{ATET} - ATET)] = E[\sqrt{N_1}(\mu_0(X_i) - \mu_0(X_j(i)))]
$

现在，$\mu_0(X_i) - \mu_0(X_j(i))$ 并不那么容易理解，因此我们需要更仔细地审视它。$\mu_0(X_i)$ 是处理单元 $i$ 若未接受处理时的结果 Y 值，即单元 i 的反事实结果  $Y_0$。$\mu_0(X_j(i))$ 则是未处理单元 $j$ ——即单元 $i$ 的匹配对象——的结果。因此，它同样是 $Y_0$ ，但这次针对的是单元 $j$。只不过这一次是实际结果，因为 $j$ 属于未处理组。由于 $j$ 与 $i$ 仅是相似而非完全相同，这一差值很可能不为零。换言之，$X_i \approx X_j $。所以，$Y_{0i} \approx Y_{0j} $。

随着样本量的增加，可匹配的单元数量增多，单元 $i$ 与其匹配单元 $j$ 之间的差异也会缩小。但这种差异收敛至零的速度较慢。因此，$E[\sqrt{N_1}(\mu_0(X_i) - \mu_0(X_j(i)))]$ 可能不会收敛至零，因为 $\sqrt{N_1}$ 的增长速度快于 $(\mu_0(X_i) - \mu_0(X_j(i)))$ 的减小速度。

当匹配差异巨大时，偏误便会产生。幸运的是，我们知晓如何修正这一问题。每项观测数据对偏误的贡献为 $(\mu_0(X_i) - \mu_0(X_j(i)))$ ，因此只需在估计量中从每次匹配比较中减去这一数值即可。为此，我们可以用该数值 $\mu_0(X_j(i))$ 的某种估计量（如通过线性回归等模型获得）来替代 $\hat{\mu_0}(X_j(i))$。这将 ATET 估计量更新为以下方程式

$
\hat{ATET} = \frac{1}{N_1}\sum \big((Y_i - Y_{j(i)}) - (\hat{\mu_0}(X_i) - \hat{\mu_0}(X_{j(i)}))\big)
$

其中 $\hat{\mu_0}(x)$ 为 $E[Y|X, T=0]$ 的某种估计值，例如仅基于未处理样本拟合的线性回归结果

```python
from sklearn.linear_model import LinearRegression

# fit the linear regression model to estimate mu_0(x)
ols0 = LinearRegression().fit(untreated[X], untreated[y])
ols1 = LinearRegression().fit(treated[X], treated[y])

# find the units that match to the treated
treated_match_index = mt0.kneighbors(treated[X], n_neighbors=1)[1].ravel()

# find the units that match to the untreatd
untreated_match_index = mt1.kneighbors(untreated[X], n_neighbors=1)[1].ravel()

predicted = pd.concat([
    (treated
     # find the Y match on the other group
     .assign(match=mt0.predict(treated[X])) 
     
     # build the bias correction term
     .assign(bias_correct=ols0.predict(treated[X]) - ols0.predict(untreated.iloc[treated_match_index][X]))),
    (untreated
     .assign(match=mt1.predict(untreated[X]))
     .assign(bias_correct=ols1.predict(untreated[X]) - ols1.predict(treated.iloc[untreated_match_index][X])))
])

predicted.head()
```

随即产生的一个直接问题是：这样做难道不会违背匹配的初衷吗？如果无论如何我都得运行线性回归，为何不直接使用它，而要采用这个复杂的模型。这是个合理的质疑，因此我需要花些时间来解答。

![img](./images/10/ubiquitous-ols.png)

首先，我们所拟合的线性回归并不通过外推处理维度来获取处理效应。相反，其目的仅在于校正偏误。此处的线性回归具有局部性，即它并不试图考察若处理组看起来像未处理组时会如何。它完全不进行此类外推。这部分工作留给了匹配环节。估计量的核心仍是匹配部分。我想在此强调的是，对于这一估计量而言，普通最小二乘法（OLS）处于次要地位。

第二点在于，匹配是一种非参数估计方法。它不假设线性或任何参数模型。因此，相较于线性回归，匹配更具灵活性，能在非线性特征极其显著而线性回归失效的场景中发挥作用。

这是否意味着你只应使用匹配法？嗯，这是个棘手的问题。阿尔贝托·阿巴迪主张确实应该如此，认为这种方法更为灵活，且一旦掌握了代码，操作起来同样简单。我对此并不完全信服。举例来说，阿巴迪投入了大量时间研究并开发这一估计方法（没错，他是推动匹配法发展至今日形态的科学家之一），显然他对这种方法有着个人情感上的投入。其次，线性回归的简洁性中有某些特质是匹配法所不具备的。“保持其他条件不变”的偏导数数学原理，在线性回归中比在匹配法中更易于理解。但这仅是我的个人偏好。老实说，这个问题并没有明确的答案。无论如何，回到我们的例子中来。

使用偏误校正公式后，我得到了如下的平均处理效应（ATE）估计值。

```python
np.mean((2*predicted["medication"] - 1)*((predicted["recovery"] - predicted["match"])-predicted["bias_correct"]))
```

当然，我们还需要围绕这一测量值设置置信区间，但数学理论就暂且讨论到这里。实际操作中，我们可以直接借用他人编写的代码，导入一个匹配估计器。比如，这是来自因果推断库 [causalinference](https://github.com/laurencium/causalinference) 中的一个例子。

```python
from causalinference import CausalModel

cm = CausalModel(
    Y=med["recovery"].values, 
    D=med["medication"].values, 
    X=med[["severity", "age", "sex"]].values
)

cm.est_via_matching(matches=1, bias_adj=True)

print(cm.estimates)
```

最终，我们可以有把握地说，我们的药物确实缩短了患者住院时间。由于 knn sklearn 实现与 causalinference Python 包在匹配平局处理上的差异，ATE 估计值略低于我的计算结果。

在结束这个话题之前，我想再稍微讨论一下匹配偏误的成因。我们发现当个体与其匹配对象相似度不足时，匹配会产生偏误。但究竟是什么导致它们如此不同？


## 纬度灾难

事实证明，答案既简单又直观。基于少数特征（如性别）匹配个体很容易，但当我们增加年龄、收入、出生城市等更多特征时，寻找匹配对象就变得越来越困难。更概括地说，特征维度越高，个体与其匹配对象之间的距离就越大。

这不仅影响了匹配估计器的表现，还与之前提到的子类估计器密切相关。回顾早期那个关于药物效果在男女性别上差异的人为设计例子，构建子分类估计器相对简单，因为我们仅有两个类别：男性和女性。然而，若类别增多呢？假设我们有两个连续变量，如年龄和收入，并将它们各自离散化为 5 个区间，这将生成 25 个单元格，即 $5^2$。更进一步，如果有 10 个协变量，每个分 3 个区间，看似不多，对吧？但实际上，这将产生 59049 个单元格，即 $3^{10}$。显而易见，问题规模会迅速膨胀。这一现象在数据科学中普遍存在，被称为 **“维度灾难”** ！！！

![img](./images/10/curse-of-dimensionality.png)
图片来源： https://deepai.org/machine-learning-glossary-and-terms/curse-of-dimensionality

尽管这个名称听起来吓人且故作高深，但它实际上仅意味着填满特征空间所需的数据点数量会随着特征或维度的增加呈指数级增长。例如，若填满 3 个特征空间需要 X 个数据点，那么填满 4 个特征的空间则需要指数级更多的点。

在子类估计器的语境下，维度灾难意味着当特征数量庞大时，其性能将受到影响。大量特征意味着 X 中存在众多单元格。若单元格过多，部分单元格将仅有极少数据，甚至可能仅包含处理组或对照组，导致无法在该处估计平均处理效应（ATE），从而破坏估计器的有效性。在匹配情境中，这意味着特征空间将极为稀疏，单元间距离会非常遥远，这将增大匹配对之间的距离并引发偏误问题。

至于线性回归，它实际上能很好地处理这个问题。其做法是将所有特征 X 投影至单一的 Y 维度上，然后在该投影上进行处理组与对照组的比较。因此，从某种意义上说，线性回归通过某种降维方式来估计平均处理效应（ATE），这一方法相当精妙。

大多数因果模型也具备应对维度灾难的方法。虽然我不再赘述，但你在研究这些模型时应时刻牢记这一点。例如，在下一节讨论倾向得分时，不妨观察它是如何解决这一问题的。

## 核心要点

本节伊始，我们理解了线性回归的功能及其如何助力识别因果关系。具体而言，我们认识到回归可视为将数据集划分为多个单元，计算每个单元内的 ATE，再将各单元的 ATE 整合为整个数据集的单一 ATE 值。

由此，我们推导出了一个基于子类的非常通用的因果推断估计量。虽然该估计量在实际应用中并不十分实用，但它为我们提供了关于如何解决因果推断估计问题的一些有趣见解。这进而引导我们探讨了匹配估计量的概念。

匹配法通过为每个处理单元寻找一个与之极为相似的非处理单元作为对照（反之亦然），从而控制混杂变量的影响。我们学习了如何利用 KNN 算法实现这一方法，以及如何通过回归校正其偏误。最后，我们比较了匹配法与线性回归的区别，认识到匹配作为一种非参数估计方法，不像线性回归那样依赖于线性假设。

最后，我们深入探讨了高维数据集带来的挑战，并分析了因果推断方法在此类数据中可能面临的困境。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 11 - 倾向得分

## 成长心理学

积极心理学这一领域研究哪些人类行为能引导我们过上更好的人生。你可以把它理解为“自助书籍”和“统计学严谨性”的交叉点。积极心理学中一个著名的发现是“成长型心态”。这个概念认为，人们可以拥有固定型心态或成长型心态。
- 如果你拥有固定型心态，你会认为能力是在出生或早期童年就被决定的。因此，智力是固定的，终生无法改变。如果你现在没有某项能力，那你将永远无法获得它。这种想法的推论是：你不应该在自己不擅长的领域浪费时间，因为你永远也学不会。
- 而如果你拥有成长型心态，你会相信智力是可以被培养的。其直接的结果是：你不会将失败视为缺乏毅力，而是将其视为学习过程中的一部分。

我并不想争论这两种心态中哪一种才是正确的（尽管答案或许介于两者之间）。就我们的目的而言，这并不重要。真正重要的是，心理学家发现拥有成长型心态的人往往在生活中表现更佳。他们更有可能实现自己设定的目标。

尽管我们精通因果推断，但已学会以怀疑态度看待这些论断。究竟是成长型心态促使人们取得更多成就？还是仅仅因为取得更多成就的人容易因其成功而发展出成长型心态？孰因孰果，如同鸡与蛋的悖论？用潜在结果模型表述，我们有理由相信这些陈述存在偏误。$Y_0|T=1$ 很可能大于 $Y_0|T=0$，这意味着即使持有固定型心态，那些具备成长型心态的人原本也能取得更高成就。

为解决这一问题，研究人员设计了“[全国学习心态研究](https://mindsetscholarsnetwork.org/about-the-network/current-initatives/national-mindset-study/#)”。这是一项在美国公立高中开展的随机对照试验，旨在探究成长型心态的影响效果。其运作方式为：学校向学生提供旨在培养成长型心态的研讨会，随后追踪学生大学期间的学业表现以评估成效。这些测量结果被整合成标准化成就分数。出于保护学生隐私的考虑，该研究的真实数据未公开。但我们将使用 [Athey 和 Wager](https://arxiv.org/pdf/1902.07409.pdf) 提供的具有相同统计特性的模拟数据集作为替代。

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from matplotlib import style
from matplotlib import pyplot as plt
import seaborn as sns
import statsmodels.formula.api as smf
from causalinference import CausalModel

import graphviz as gr

%matplotlib inline

style.use("fivethirtyeight")
pd.set_option("display.max_columns", 6)
```

除处理变量与结果变量外，该研究还记录了以下特征：

* schoolid：学生所在学校的标识符；
* success_expect：自我报告的未来成功预期（作为学业基础能力的代理变量，在随机分配前测得）；
* ethnicity：学生种族/族群的分类变量；
* gender：学生自我认同性别的分类变量；
* frst_in_family：学生是否为家庭第一代大学生的分类变量，即家庭中首个上大学者；
* school_urbanicity：学校层面的城乡属性分类变量，如农村、郊区等；
* school_mindset：随机分配前报告的学生固定型思维模式的校级平均值，标准化处理；
* school_achievement：基于前四届学生测试成绩及大学预备情况的衡量指标，标准化处理；
* school_ethnic_minority：学校种族/族裔少数群体比例，即黑人、拉丁裔或美洲原住民学生占比，标准化处理；
* school_poverty：家庭收入低于联邦贫困线标准的学生百分比，标准化处理；
* school_size：该校四个年级学生总数的标准化值。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/learning_mindset.csv", clear


list in 1/5

```python
data = pd.read_csv("./data/learning_mindset.csv")
data.sample(5, random_state=5)
```

尽管该研究采用了随机化设计，但数据似乎仍存在混杂因素。若审视额外特征，我们会发现它们在处理组与对照组之间存在系统性差异。一个可能的原因是处理变量以学生是否参与研讨会来衡量。因此，虽然参与机会是随机的，但实际参与行为并非如此。此处我们面临的是不依从性问题。一个证据是学生的成功预期与研讨会参与度呈相关性——自我报告成功预期较高的学生更可能参加了成长心态研讨会。

```{dropdown} 查看 Stata 代码
```stata
* Calculate mean intervention rate by success_expect categories
table success_expect, stat(mean intervention)

```python
data.groupby("success_expect")["intervention"].mean()
```

不过，我们仍可观察均值差异 $E[Y|T=1] - E[Y|T=0]$ 的情况，这将作为后续比较的有用基准。

```{dropdown} 查看 Stata 代码
```stata
* run ols 
reg achievement_score intervention

```python
smf.ols("achievement_score ~ intervention", data=data).fit().summary().tables[1]
```

仅通过比较接受干预与未接受干预的群体，我们可以发现，接受干预者的成绩分数平均比标准化平均分（即零分）高出 0.3185（0.4723 减去 0.1538）。但这个差异是大是小？我明白解读标准化结果可能颇具挑战，但请稍安勿躁。我认为值得深入探讨，因为标准化分数的出现绝非仅此一次。
 
被标准化的结果变量意味着它是以标准差为单位来衡量的。因此，处理组比未处理组高出 0.3185 个标准差。这就是此处的含义。至于这个差异是大是小，让我们回顾一下正态分布的一些特性。我们知道其 95%的质量落在两个标准差之间，两端各剩 2.5%。这也意味着，若某人高于均值两个标准差，那么 97.5%（95%加上左侧 2.5%的尾部）的个体都位于此人之下。通过查看正态累积分布函数，我们还知道约 85%的质量低于一个标准差，70%低于 0.5 个标准差。由于处理组的平均标准化得分约为 0.5，这意味着他们在个人成就方面超过了 70%的个体。换言之，他们属于成就更高的前 30%人群。下图直观展示了这一情况。

```{dropdown} 查看 Stata 代码
```stata
* Set graph style
set scheme s1color
color_style tableau

* Create combined histogram
twoway (histogram achievement_score, bin(20) color(blue%30) legend(label(1 "All"))) ///
       (histogram achievement_score if intervention == 0, bin(20) color(green%30) legend(label(2 "Untreated"))) ///
       (histogram achievement_score if intervention == 1, bin(20) color(red%30) legend(label(3 "Treated"))) ///
       (function y = 0, range(-4 4) lcolor(green) lpattern(solid) lwidth(0.4) legend(label(4 "Untreated Mean"))) ///
       (function y = 0, range(-4 4) lcolor(red) lpattern(solid) lwidth(0.4) legend(label(5 "Treated Mean"))), ///
       title("Achievement Score Distribution") ///
       xtitle("Achievement Score") ///
       ytitle("Frequency") ///
       legend(position(6) rows(1)) ///
       graphregion(color(white)) ///
       plotregion(color(white)) ///
       xlabel(-4(1)4, grid) ///
       ylabel(, grid)

```python
plt.hist(data["achievement_score"], bins=20, alpha=0.3, label="All")
plt.hist(data.query("intervention==0")["achievement_score"], bins=20, alpha=0.3, color="C2")
plt.hist(data.query("intervention==1")["achievement_score"], bins=20, alpha=0.3, color="C3")
plt.vlines(-0.1538, 0, 300, label="Untreated", color="C2")
plt.vlines(-0.1538+0.4723, 0, 300, label="Treated", color="C3")
plt.legend()
plt.show() ;
```

当然，我们仍认为这一结果存在偏误。处理组与未处理组之间的差异可能比这小，因为我们推测偏误为正。我们已经看到，更有抱负的人更愿意参加研讨会，因此即便未参与，他们可能也会取得更高成就。为控制这一偏误，可采用回归或匹配方法，但现在该学习一项新技术了。

## 倾向得分

倾向得分源于一个认识：无需直接控制混杂变量 X 即可实现条件独立性 $(Y_1, Y_0) \perp T | X$。实际上，控制一个平衡得分 $E[T|X]$ 就足够了。该平衡得分通常是处理的条件概率 $P(T|X)$，亦称倾向得分 $e(x)$。倾向得分使得不必对 X 整体进行条件限制就能实现潜在结果对处理的独立性，仅需对这一单一变量（即倾向得分）进行条件控制即可：

$
(Y_1, Y_0) \perp T | e(x)
$

关于这一点有一个正式的证明，但我们现在可以暂时忽略它，以更直观的方式来探讨这个问题。倾向得分是接受处理的条件概率，对吧？因此，我们可以将其视为某种将 X 转化为处理 T 的函数。倾向得分在变量 X 和处理 T 之间起到了桥梁的作用。如果我们在因果图中展示这一点，它看起来会是这样的。

```python
g = gr.Digraph()
g.edge("T", "Y")
g.edge("X", "Y")
g.edge("X", "e(x)")
g.edge("e(x)", "T")
g
```

若已知 e(x)的值，仅凭 X 本身无法提供更多有助于预测 T 的信息。这意味着控制 e(x)的效果等同于直接控制 X。以我们的心态项目为例，处理组与对照组最初不具备可比性，因为更具雄心的人既更可能接受处理，也更容易在生活中取得更大成就。然而，若从处理组和对照组中各取一人，且这两人具有相同的处理接收概率时，他们便具有可比性。试想：若两人接受处理的概率完全相同，那么其中一人接受处理而另一人未接受的唯一原因纯属偶然。保持倾向得分不变的作用，就在于使数据看起来如同随机分配一般。

既然直观理解已建立，现在让我们审视数学证明。我们需要证明 $(Y_1, Y_0) \perp T | e(x)$ 等价于以下表述：

$
E[T|e(x), X] = E[T|e(x)] 
$

这实质上表明，在给定 $e(x)$ 的条件下，X 无法为 $T$ 提供任何额外信息。该证明过程颇为奇特：我们将通过将上述等式转化为一个不言自明的陈述来完成证明。首先观察等式左侧的 $E[T|e(x), X]$。

$
E[T|e(x), X] = E[T|X] = e(x)
$

我们利用 $e(x)$ 仅是 X 的函数这一事实，因此在已对 X 本身进行条件化后，再对其条件化不会提供额外信息。接着，我们运用倾向得分的定义 $E[T|X]$。

对于右侧，我们将使用迭代期望定律 $E[A] = E[E[A|B]]$。该定律表明，可以通过分解 B 后 A 的值来计算 A 的期望值，然后取其平均值。

$
E[T|e(x)] = E[E[T|e(x),X]|e(x)] = E[e(x)|e(x)] = e(x)
$

第一个等式源于迭代期望定律。第二个等式则来自我们在处理左侧时得出的结论。由于等式左右两边均等于 $e(x)$，故此等式显然成立。

## 倾向性加权

![img](./images/11/balance.png)

好的，我们得到了倾向得分。接下来呢？就像我说的，我们只需要以它为条件进行分析。例如，我们可以运行一个仅以倾向得分为条件的线性回归，而不是所有的 X 变量。现在，让我们看看一种仅使用倾向得分的技术。其核心思想是利用倾向得分来表达条件均值差异。

$
E[Y|X,T=1]-E[Y|X,T=0] = E\bigg[\dfrac{Y}{e(x)}|X,T=1\bigg]P(T) - E\bigg[\dfrac{Y}{(1-e(x))}|X,T=0\bigg](1-P(T))
$

我们还可以进一步简化这个表达式，但先以这种方式来看，它能帮助我们直观理解“倾向得分”到底在做什么。第一个项是在估计 $Y_1$，它是将所有接受处理的个体按其接受处理的概率的倒数进行加权。这个操作的效果是：那些接受处理但原本不太可能接受处理的个体被赋予更高的权重。

这很合理，对吧？如果一个人接受处理的概率很低，说明他更像是未接受处理的个体；然而他实际上却接受了处理——这就很有价值。**我们得到了一个“看起来像未处理人群”的受处理个体，因此要赋予他更高的权重**。通过这种方式，我们构造出一个“每个人都接受了处理”的人群，且这个人群的分布和原始人群相同。

同样地，第二个项处理的是未接受处理的个体，并且对那些“看起来像处理组”的个体赋予更高的权重。

这种估计方法被称为 **倾向得分逆概率加权法（IPTW, Inverse Probability of Treatment Weighting）**，因为它根据个体**实际接受的处理的概率的倒数**对每一个样本单位进行加权。

用图像展示，这就是加权所实现的效果。

![img](./images/11/iptw.png)

左上角的图表展示了原始数据。蓝点代表未经处理的样本，红点代表经过处理的样本。底部图表显示了倾向得分 $e(x)$。注意其值介于 0 到 1 之间，并随着 X 的增加而增长。最后，右上角的图表是加权后的数据。可以看到，位于左侧（倾向得分较低）的红点（处理组）具有更高的权重；同样，位于右侧的蓝点（对照组）也拥有更高的权重。

现在我们已经理解了直观概念，可以将上述表达式简化为

$$
\begin{align}
E[Y|X,T=1]-E[Y|X,T=0] &= E\bigg[\dfrac{Y}{e(x)}|X,T=1\bigg]P(T) - E\bigg[\dfrac{Y}{(1-e(x))}|X,T=0\bigg](1-P(T)) \\
&=E\bigg[\dfrac{YT}{e(x)}\bigg|X\bigg] - E\bigg[\dfrac{Y(1-T)}{(1-e(x))}\bigg|X\bigg] \\
&=E\bigg[\dfrac{YT}{e(x)} - \dfrac{Y(1-T)}{(1-e(x))}\bigg|X\bigg] \\
&=E\bigg[Y\dfrac{T(1-e(x)) - e(x)(1-T)}{e(x)(1-e(x))}\bigg|X\bigg] \\
&=E\bigg[Y \dfrac{T-e(x)}{e(x)(1-e(x))}\bigg|X\bigg]
\end{align}
$$

若对 X 进行积分，该式即成为我们的倾向得分加权估计量。

$
E\bigg[Y \dfrac{T-e(x)}{e(x)(1-e(x))}\bigg]
$

注意，此估计量要求 $e(x)$ 和 $1-e(x)$ 必须大于零。换言之，这意味着每个人都需有一定概率接受处理和不接受处理。另一种表述是处理组与未处理组的分布需存在重叠，这正是因果推断中的**正值性假设**。这一假设在直观上也合乎逻辑——若处理组与未处理组无重叠，则表明两者差异极大，此时无法将一组的效应外推至另一组。虽然这种外推并非不可能（回归分析即采用此法），但其风险极高。这如同仅在男性群体中试验新药后，便假定女性群体会有同等疗效。


## 倾向得分估计

理想情况下，我们应掌握真实的倾向得分 $e(x)$。但实际上，处理分配机制未知，需用其估计值 $\hat{e}(x)$ 替代。常用方法是逻辑回归，但也可采用梯度提升等机器学习方法（尽管需额外步骤防止过拟合）。 

在此，我将坚持使用逻辑回归。这意味着我需要将数据集中的分类特征转换为虚拟变量。

```{dropdown} 查看 Stata 代码
```stata
* 1. Create dummy variables for categorical features
foreach var in ethnicity gender school_urbanicity {
    tab `var', gen(`var'_)
    label var `var'_1 "`var' category 1"
    * Drop reference category if needed (uncomment)
    * drop `var'_1
}

* 2. Keep only continuous variables and new dummies
keep achievement_score intervention ///
     school_mindset school_achievement school_ethnic_minority ///
     school_poverty school_size ethnicity_* gender_* school_urbanicity_* ///
	 schoolid frst_in_family schoolid success_expect

* 3. Display new dataset structure
describe
display "New dataset dimensions: " c(N) " observations, " c(k) " variables"

```python
categ = ["ethnicity", "gender", "school_urbanicity"]
cont = ["school_mindset", "school_achievement", "school_ethnic_minority", "school_poverty", "school_size"]

data_with_categ = pd.concat([
    data.drop(columns=categ), # dataset without the categorical features
    pd.get_dummies(data[categ], columns=categ, drop_first=False)# categorical features converted to dummies
], axis=1)

print(data_with_categ.shape)
```

现在，让我们使用逻辑回归来估计倾向得分。

```{dropdown} 查看 Stata 代码
```stata
* 1. Run high-regularization logistic regression 
logit intervention school_mindset school_achievement school_ethnic_minority ///
                school_poverty school_size ethnicity_* gender_* school_urbanicity_*

* 2. Predict propensity scores
predict propensity_score, pr

* 3. Create new dataset with key variables
preserve
    keep schoolid intervention achievement_score propensity_score
    order schoolid intervention achievement_score propensity_score
    
    * Display first 5 observations
    list in 1/5, noobs clean
    
    * Save for analysis
    save propensity_scores, replace
restore

```python
from sklearn.linear_model import LogisticRegression

T = 'intervention'
Y = 'achievement_score'
X = data_with_categ.columns.drop(['schoolid', T, Y])

ps_model = LogisticRegression(C=1e6).fit(data_with_categ[X], data_with_categ[T])

data_ps = data.assign(propensity_score=ps_model.predict_proba(data_with_categ[X])[:, 1])

data_ps[["intervention", "achievement_score", "propensity_score"]].head()
```

首先，我们可以确保倾向得分权重确实重构了一个所有人都接受处理的人群。通过生成权重 $1/e(x)$，它创建了一个所有人都接受处理的群体；而提供权重 $1/(1-e(x))$ 则创建了一个所有人都未接受处理的群体。

```{dropdown} 查看 Stata 代码
```stata
* 1. Calculate inverse probability weights
gen ipw = .
replace ipw = 1/propensity_score if intervention == 1
replace ipw = 1/(1-propensity_score) if intervention == 0

* 2. Calculate and display sample sizes
count
display "Original Sample Size: " r(N)

sum ipw if intervention == 1
display "Treated Population Sample Size: " r(sum)

sum ipw if intervention == 0
display "Untreated Population Sample Size: " r(sum)

```python
weight_t = 1/data_ps.query("intervention==1")["propensity_score"]
weight_nt = 1/(1-data_ps.query("intervention==0")["propensity_score"])
print("Original Sample Size", data.shape[0])
print("Treated Population Sample Size", sum(weight_t))
print("Untreated Population Sample Size", sum(weight_nt))
```

我们还可以利用倾向得分来寻找混杂因素的证据。如果某一人群分段的倾向得分高于另一分段，这意味着存在非随机因素导致处理分配。若该因素同时影响结果变量，则存在混杂。在本例中，我们发现自报更具雄心的学生参加成长心态研讨会的概率也更高。

```{dropdown} 查看 Stata 代码
```stata
* Set modern graph style
set scheme s2color
color_style tableau

* Create boxplot with enhanced formatting
graph box propensity_score, over(success_expect) ///
    title("Confounding Evidence", size(medium)) ///
    ytitle("Propensity Score", size(small)) ///
    box(1, color(blue%50)) ///
    box(2, color(orange%50)) ///
    box(3, color(green%50)) ///
    graphregion(color(white)) ///
    plotregion(color(white)) ///
    ylabel(0.2(0.05)0.5, grid gmin gmax) ///
    legend(off)

```python
sns.boxplot(x="success_expect", y="propensity_score", data=data_ps)
plt.title("Confounding Evidence")
plt.show();
```

我们还需检查处理组与未处理组之间是否存在重叠。为此，可以观察未处理组和处理组的倾向得分经验分布。从下图可见，无人具有零倾向得分，即使在倾向得分较低的区域也能同时找到处理与未处理的个体。这正是我们所说的处理组与未处理组良好平衡的状态。

```{dropdown} 查看 Stata 代码
```stata
*  Check propensity score distribution
twoway (histogram propensity_score if intervention==1, color(red%30)) ///
       (histogram propensity_score if intervention==0, color(blue%30)), ///
       legend(order(1 "Treated" 2 "Untreated")) ///
       title("Propensity Score Distribution") ///
       xtitle("Propensity Score") ///
       graphregion(color(white))

```python
sns.distplot(data_ps.query("intervention==0")["propensity_score"], kde=False, label="Non Treated")
sns.distplot(data_ps.query("intervention==1")["propensity_score"], kde=False, label="Treated")
plt.title("Positivity Check")
plt.legend()
plt.show() ;
```

最终，我们可以利用倾向得分加权估计量来评估平均处理效应。

```{dropdown} 查看 Stata 代码
```stata
* 1. Calculate normalized IP weights
gen ipw1 = (intervention - propensity_score) / (propensity_score * (1 - propensity_score))

* 2. Calculate potential outcomes
sum achievement_score [aw=ipw1] if intervention == 1
scalar y1 = r(mean)

sum achievement_score [aw=ipw1] if intervention == 0
scalar y0 = r(mean)

* 3. Calculate ATE
scalar ate = y1 - y0

* 4. Display results with 95% CIs
display "Potential Outcomes and ATE:"
display "Y1: " %5.3f y1 " [95% CI: " %5.3f (y1 - invttail(r(N)-1,0.025)*r(sd)/sqrt(r(N))) ", " %5.3f (y1 + invttail(r(N)-1,0.025)*r(sd)/sqrt(r(N))) "]"
display "Y0: " %5.3f y0 " [95% CI: " %5.3f (y0 - invttail(r(N)-1,0.025)*r(sd)/sqrt(r(N))) ", " %5.3f (y0 + invttail(r(N)-1,0.025)*r(sd)/sqrt(r(N))) "]"
display "ATE: " %5.3f ate " [95% CI: " %5.3f (ate - invttail(r(N)-1,0.025)*r(sd)/sqrt(r(N))) ", " %5.3f (ate + invttail(r(N)-1,0.025)*r(sd)/sqrt(r(N))) "]"

* 5. Recommended: Use teffects for proper inference
teffects ipw (achievement_score) (intervention, logit), ///
    osample(overlap) ///
    vce(robust)

```python
weight = ((data_ps["intervention"]-data_ps["propensity_score"]) /
          (data_ps["propensity_score"]*(1-data_ps["propensity_score"])))

y1 = sum(data_ps.query("intervention==1")["achievement_score"]*weight_t) / len(data)
y0 = sum(data_ps.query("intervention==0")["achievement_score"]*weight_nt) / len(data)

ate = np.mean(weight * data_ps["achievement_score"])

print("Y1:", y1)
print("Y0:", y0)
print("ATE", ate)
```

倾向得分加权法表明，就成就而言，我们应预期接受处理的个体比未接受处理的同伴高出 0.38 个标准差。同时可见，若无人接受处理，整体成就水平预计将比现状低 0.12 个标准差。同理，若让所有人参与研讨会，预期整体成就水平将提升 0.25 个标准差。这与简单对比处理组和未处理组得出的 0.47 平均处理效应估计值形成对比，证明存在的偏误确实为正，且通过控制变量 X 后，对成长心态影响的估计更为保守。

## 标准误差

![img](./images/11/bootstrap.png)

为计算逆概率加权估计量的标准误，可采用加权平均方差公式。

$
\sigma^2_w = \dfrac{\sum_{i=1}^{n}w_i(y_i-\hat{\mu})^2}{\sum_{i=1}^{n}w_i}
$

然而，仅当我们拥有真实的倾向得分时才能使用此方法。若采用其估计版本 $\hat{P}(x)$，则需考虑这一估计过程中的误差。最简便的做法是对整个流程进行自助法（bootstrap）处理，即通过有放回地从原始数据中抽样，并如上所述计算平均处理效应（ATE）。随后多次重复此过程，以获取 ATE 估计值的分布。

```python
from joblib import Parallel, delayed # for parallel processing

# define function that computes the IPTW estimator
def run_ps(df, X, T, y):
    # estimate the propensity score
    ps = LogisticRegression(C=1e6, max_iter=1000).fit(df[X], df[T]).predict_proba(df[X])[:, 1]
    
    weight = (df[T]-ps) / (ps*(1-ps)) # define the weights
    return np.mean(weight * df[y]) # compute the ATE

np.random.seed(88)
# run 1000 bootstrap samples
bootstrap_sample = 1000
ates = Parallel(n_jobs=4)(delayed(run_ps)(data_with_categ.sample(frac=1, replace=True), X, T, Y)
                          for _ in range(bootstrap_sample))
ates = np.array(ates)
```

ATE 即为自助样本的均值。要获得置信区间，可考察自助分布的百分位数：对于 95%置信区间，采用 2.5%和 97.5%分位数。

```python
print(f"ATE: {ates.mean()}")
print(f"95% C.I.: {(np.percentile(ates, 2.5), np.percentile(ates, 97.5))}")
```

我们还可以直观展示自助样本的分布形态及其置信区间。

```python
sns.distplot(ates, kde=False)
plt.vlines(np.percentile(ates, 2.5), 0, 30, linestyles="dotted")
plt.vlines(np.percentile(ates, 97.5), 0, 30, linestyles="dotted", label="95% CI")
plt.title("ATE Bootstrap Distribution")
plt.legend()
plt.show();
```

## 倾向得分法的常见问题

作为一名数据科学家，我深知利用机器学习工具包的全部力量来尽可能精确地估计倾向得分是多么诱人。你可能会迅速沉迷于 AUC 优化、交叉验证和贝叶斯超参数调优之中。我并不是说你不该这么做。事实上，关于倾向得分与机器学习的所有理论都非常新近，我们仍有许多未知领域。但首先理解一些基本概念是值得的。

首要一点是，倾向得分的预测质量并不等同于其平衡特性。来自机器学习领域的学者在熟悉因果推断时，最富挑战性的方面之一便是放弃将一切视为预测问题的习惯。实际上，最大化倾向得分的预测能力甚至可能损害因果推断的目标。 **倾向得分无需非常精确地预测处理分配，它只需涵盖所有混杂变量。** 若纳入那些虽能有效预测处理分配但与结果无关的变量，反而会增加倾向得分估计量的方差。这与线性回归中纳入与处理相关但与结果无关变量时所面临的问题类似。

![img](./images/11/ml-trap.png)

为了理解这一点，请考虑以下示例（改编自 Hernán 的著作）。假设有两所学校，其中一所对 99%的学生实施了成长心态研讨会，另一所仅对 1%的学生实施。进一步假设学校本身对处理效应无直接影响（除非通过处理本身），因此无需对其进行控制。若将学校变量加入倾向得分模型，该变量将具有极高的预测力。然而，偶然情况下，我们可能获得一个样本，其中学校 A 的所有学生均接受了处理，导致该校倾向得分为 1，进而引发方差无限大的问题。此虽为极端案例，但我们将通过模拟数据展示其运作机制。

```{dropdown} 查看 Stata 代码
```stata
clear all
set seed 42  // Set random seed for reproducibility (matches Python's np.random.seed(42))

// Create data for School A
set obs 400  // Create 400 observations
gen school = 0  // School identifier (0 for School A)
gen intercept = 1  // Constant/intercept term
gen T = rbinomial(1, 0.99)  // Generate treatment variable (99% probability of 1)
tempfile school_a  // Create temporary file to store School A data
save `school_a'  // Save School A data

// Create data for School B
clear  // Clear memory
set obs 400  // Create 400 observations
gen school = 1  // School identifier (1 for School B)
gen intercept = 1  // Constant/intercept term
gen T = rbinomial(1, 0.01)  // Generate treatment variable (1% probability of 1)
tempfile school_b  // Create temporary file to store School B data
save `school_b'  // Save School B data

// Combine datasets (equivalent to pd.concat in Python)
use `school_a', clear  // Load School A data
append using `school_b'  // Append School B data

// Generate outcome variable y (equivalent to .assign() in Python)
gen y = rnormal(1 + 0.1 * T)  // y ~ N(1 + 0.1*T, 1)

// Display first 5 observations (equivalent to .head() in Python)
list in 1/5	

```python
np.random.seed(42)
school_a = pd.DataFrame(dict(T=np.random.binomial(1, .99, 400), school=0, intercept=1))
school_b = pd.DataFrame(dict(T=np.random.binomial(1, .01, 400), school=1, intercept=1))
ex_data = pd.concat([school_a, school_b]).assign(y = lambda d: np.random.normal(1 + 0.1 * d["T"]))
ex_data.head()
```

完成数据模拟后，我们使用倾向得分算法进行了两次自助法（bootstrap）分析。第一次将学校作为特征纳入倾向得分模型，第二次则未将学校变量纳入模型。

```{dropdown} 查看 Stata 代码
```stata
// Define program to estimate ATE
capture program drop run_ps
program define run_ps, rclass
    syntax varlist, TREATment(varname) OUTcome(varname)
    
	// Remove existing ps variable if it exists
    capture drop ps
    capture drop weight
	
    // Logistic regression for propensity score
    logit `treatment' `varlist'
    predict ps, pr  // Get propensity scores
    
    // IPW estimation (inverse probability weighting)
    tempvar weight
    gen `weight' = `treatment'/ps + (1-`treatment')/(1-ps)
    
    // Weighted regression for ATE
    reg `outcome' `treatment' [pw=`weight']
    
    // Return coefficient on treatment
    return scalar ate = _b[`treatment']
end

// Initialize matrices to store results (equivalent to np.array)
matrix ate_w_f = J(500, 1, .)  // With school fixed effect
matrix ate_wo_f = J(500, 1, .)  // Without school fixed effect

// Bootstrap loop (500 replications)
forvalues i = 1/500 {
    preserve  // Preserve original data
    
    // Resample with replacement (equivalent to sample(frac=1, replace=True))
    bsample
    
    // Run estimation with school fixed effect
    run_ps school, treatment(T) outcome(y)
    matrix ate_w_f[`i', 1] = r(ate)
    
    // Run estimation with only intercept
    run_ps intercept, treatment(T) outcome(y)
    matrix ate_wo_f[`i', 1] = r(ate)
    
    restore  // Restore original data
}

// Convert matrices to Stata variables for analysis
clear
svmat ate_w_f, names(ate_with_fixed)
svmat ate_wo_f, names(ate_without_fixed)

// Summarize results
summarize ate_with_fixed ate_without_fixed

// Create kernel density plots with histograms
twoway ///
    (histogram ate_with_fixed, color(blue%30) bin(30) legend(label(1 "PS W School"))) ///
    (histogram ate_without_fixed, color(red%30) bin(30) legend(label(2 "PS W/O School"))) ///
    (kdensity ate_with_fixed, lcolor(blue) lwidth(medthick)) ///
    (kdensity ate_without_fixed, lcolor(red) lwidth(medthick)), ///
    legend(order(1 2) position(6) rows(1)) ///
    title("Distribution of ATE Estimates") ///
    xtitle("ATE Estimate") ytitle("Density") ///
    graphregion(color(white)) plotregion(color(white))

```python
ate_w_f = np.array([run_ps(ex_data.sample(frac=1, replace=True), ["school"], "T", "y") for _ in range(500)])
ate_wo_f = np.array([run_ps(ex_data.sample(frac=1, replace=True), ["intercept"], "T", "y") for _ in range(500)])
```

```python
sns.distplot(ate_w_f, kde=False, label="PS W School")
sns.distplot(ate_wo_f, kde=False, label="PS W/O School")
plt.legend()
plt.show() ;
```

如你所见，加入学校特征的倾向得分估计器方差极大，而未加入该特征的模型则表现更为稳定。此外，由于学校并非混淆变量，未包含它的模型也不存在偏误。正如我所说，这并非单纯预测处理效应的问题，关键在于构建预测模型时必须控制混杂因素，而非仅着眼于预测处理本身。

这引出了倾向得分方法中常见的另一个问题。在我们的案例中，数据最终呈现高度平衡状态，但情况并非总是如此。某些情况下，接受处理者的处理概率远高于未处理者，且倾向得分分布的重叠区域非常有限。

```{dropdown} 查看 Stata 代码
```stata
// Set seed for reproducibility
set seed 42

// Generate Beta-distributed data (500 obs each)
set obs 500
gen non_treated = rbeta(4,1)  // Beta(4,1) distribution
gen treated = rbeta(1,3)      // Beta(1,3) distribution

// Create histogram plot for positivity check
twoway ///
    (histogram non_treated, color(blue%30) bin(30) legend(label(1 "Treated"))) ///
    (histogram treated, color(red%30) bin(30) legend(label(2 "Non-Treated"))), ///
    title("Positivity Check") ///
    xtitle("Propensity Score") ytitle("Frequency") ///
    legend(order(1 2) position(6) rows(1)) ///
    graphregion(color(white)) plotregion(color(white))	   

```python
sns.distplot(np.random.beta(4,1,500), kde=False, label="Non Treated")
sns.distplot(np.random.beta(1,3,500), kde=False, label="Treated")
plt.title("Positivity Check")
plt.legend()
plt.show() ;
```

若出现这种情况，意味着正向效应并不十分显著。假设某处理个体的倾向得分为 0.9，而未处理个体的最大倾向得分为 0.7，我们将无法找到任何未处理个体与这位 0.9 分的个体进行对比。这种匹配失衡可能引发偏误，因为我们必须将处理效应外推至未知区域。不仅如此，具有极高或极低倾向得分的实体权重过大，这会增加方差。根据经验法则，当任何权重超过 20 时（例如未处理个体倾向得分达 0.95 或处理个体倾向得分低至 0.05 时），就会面临问题。

另一种方法是将权重限制在最大值为 20。这样做会降低方差，但实际上会增加偏误。坦白说，尽管这是减少方差的常见做法，我并不十分赞同。你无法确定通过截断引入的偏误是否过大。此外，若分布间无重叠，数据可能本就不足以得出因果结论。为了更深入理解这一点，我们可以考察一种结合倾向得分与匹配的技术。

## 倾向得分匹配

如前所述，在拥有倾向得分的情况下，无需直接控制变量 X，仅控制倾向得分即可。因此，可将倾向得分视为对特征空间的一种降维操作——它将 X 中的所有特征压缩至单一的处理分配维度。基于此，我们可将倾向得分作为其他模型（例如回归模型）的输入特征。

```{dropdown} 查看 Stata 代码
```stata
reg achievement_score intervention propensity_score,vce(robust)	   
	   
* Recommended: Use teffects for proper inference
teffects ipw (achievement_score) (intervention, logit), ///
    osample(overlap) ///
    vce(robust)	

```python
smf.ols("achievement_score ~ intervention + propensity_score", data=data_ps).fit().summary().tables[1]
```

若我们控制了倾向得分，现估计出的平均处理效应（ATE）为 0.39，低于此前未控制倾向得分的回归模型所得结果 0.47。此外，我们还可基于倾向得分进行匹配。此次，无需寻找所有 X 特征均相似的匹配对象，仅需确保其倾向得分相同即可。

这是对匹配估计量的重大改进，因其解决了维度灾难问题。再者，若某特征对处理分配影响甚微，倾向得分模型会识别这一点，并在拟合处理机制时降低其权重。相比之下，基于特征的匹配仍会试图在个体间这一无关紧要的特征上寻找相似性。

```python
cm = CausalModel(
    Y=data_ps["achievement_score"].values, 
    D=data_ps["intervention"].values, 
    X=data_ps[["propensity_score"]].values
)

cm.est_via_matching(matches=1, bias_adj=True)

print(cm.estimates)
```

如我们所见，此处亦获得 0.38 的平均处理效应（ATE），这与先前采用倾向得分加权法所得结果更为吻合。通过倾向得分匹配，我们还能直观理解为何处理组与对照组间倾向得分重叠区域过小会带来风险。一旦出现这种情况，倾向得分差异导致的匹配偏误将显著增大，正如我们在匹配章节中已探讨的那样，这会引入估计偏误。

最后需要特别提醒的是，上述标准误计算存在错误，因其未考虑倾向得分估计过程中的不确定性。遗憾的是，[自助法（bootstrap）并不适用于匹配场景](https://economics.mit.edu/sites/default/files/publications/ON%20THE%20FAILURE%20OF%20THE%20BOOTSTRAP%20FOR.pdf)。此外，相关理论提出时间较近，目前尚无 Python 库能提供含正确标准误的倾向得分方法实现。正因如此，Python 生态中鲜见倾向得分匹配的应用案例。

## 核心要点

在本节中，我们学到了“接受处理的概率”被称为倾向得分（propensity score），并且它可以作为一种平衡得分（balancing score）来使用。
这意味着，如果我们已经得到了倾向得分，就不需要再直接控制所有混杂变量了——只需控制倾向得分本身，就足以识别因果效应。我们还看到，倾向得分在某种程度上起到了对混杂变量空间进行降维**的作用。

正是由于这些性质，我们推导出了一个基于加权的因果推断估计量（如 IPTW）。不仅如此，我们还看到倾向得分可以与其他方法结合使用，以控制混杂偏误。

接着，我们讨论了一些倾向得分和因果推断中常见的问题。第一个问题是：当我们过于关注拟合处理机制时，可能会出错。我们发现，有一个非常违反直觉（因此也容易出错）的现象是：提升处理机制的预测性能，并不意味着更好的因果效应估计，反而可能会增加估计的方差。

最后，我们还探讨了当处理组与对照组之间的倾向得分分布没有良好重叠时，可能会出现的外推问题。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 12 - 双重稳健估计

## 别把所有鸡蛋放在同一个篮子里

我们已经学习了如何使用线性回归和倾向得分加权来估计 $E[Y|T=1] - E[Y|T=0] | X$。但应该选择哪一种方法，何时使用呢？当不确定时，不妨两者并用！双重稳健估计是一种将倾向得分与线性回归结合的方法，使你不必完全依赖其中任何一种。

为了理解这一方法如何运作，让我们以思维模式实验为例。这是一项在美国公立高中进行的随机研究，旨在探究成长型思维模式的影响。具体操作是学校为学生提供研讨会，以培养其成长型思维，随后在大学阶段跟踪调查学生的学业表现。这些测量结果被汇总成标准化后的成就分数。出于保护学生隐私的考虑，该研究的真实数据未公开。不过，我们使用了 [Athey 和 Wager](https://arxiv.org/pdf/1902.07409.pdf) 提供的具有相同统计特性的模拟数据集作为替代。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/learning_mindset.csv", clear


list in 1/5

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from matplotlib import style
from matplotlib import pyplot as plt
import seaborn as sns

from sklearn.linear_model import LogisticRegression, LinearRegression

%matplotlib inline

style.use("fivethirtyeight")
pd.set_option("display.max_columns", 6)
```

```python
data = pd.read_csv("./data/learning_mindset.csv")
data.sample(5, random_state=5)
```

尽管该研究采用了随机化设计，但数据似乎仍无法完全避免混杂因素的影响。一个可能的原因是处理变量由学生是否参加研讨会来衡量。因此，虽然参与机会是随机的，但实际参与行为并非如此。我们在此面临的是一个不依从性问题。其中一个证据是学生的成功预期与参与研讨会之间存在相关性。自我报告有较高期望的学生更有可能参加了成长心态研讨会。

```{dropdown} 查看 Stata 代码
```stata
* Calculate mean intervention rate by success_expect categories
table success_expect, stat(mean intervention)

```python
data.groupby("success_expect")["intervention"].mean()
```

截至目前我们已了解，可以通过线性回归或使用逻辑回归估计倾向得分模型来对此进行调整。但在进行这些操作之前，我们需要将分类变量转换为虚拟变量。

```{dropdown} 查看 Stata 代码
```stata
* 1. Create dummy variables for categorical features
foreach var in ethnicity gender school_urbanicity {
    tab `var', gen(`var'_)
    label var `var'_1 "`var' category 1"
    * Drop reference category if needed (uncomment)
    * drop `var'_1
}

* 2. Keep only continuous variables and new dummies
keep achievement_score intervention ///
     school_mindset school_achievement school_ethnic_minority ///
     school_poverty school_size ethnicity_* gender_* school_urbanicity_* ///
	 schoolid frst_in_family schoolid success_expect

* 3. Display new dataset structure
describe
display "New dataset dimensions: " c(N) " observations, " c(k) " variables"

```python
categ = ["ethnicity", "gender", "school_urbanicity"]
cont = ["school_mindset", "school_achievement", "school_ethnic_minority", "school_poverty", "school_size"]

data_with_categ = pd.concat([
    data.drop(columns=categ), # dataset without the categorical features
    pd.get_dummies(data[categ], columns=categ, drop_first=False) # categorical features converted to dummies
], axis=1)

print(data_with_categ.shape)
```

我们现在已经准备好理解双重稳健估计是如何工作的了。

## 双重稳健估计

![img](./images/12/double.png)

与其推导估计量，我将先展示给你看，然后再告诉你它为何如此出色。

$
\hat{ATE} = \frac{1}{N}\sum \bigg( \dfrac{T_i(Y_i - \hat{\mu_1}(X_i))}{\hat{P}(X_i)} + \hat{\mu_1}(X_i) \bigg) - \frac{1}{N}\sum \bigg( \dfrac{(1-T_i)(Y_i - \hat{\mu_0}(X_i))}{1-\hat{P}(X_i)} + \hat{\mu_0}(X_i) \bigg)
$

其中 $\hat{P}(x)$ 是倾向得分的估计（例如使用逻辑回归），$\hat{\mu_1}(x)$ 是 $E[Y|X, T=1]$ 的估计（例如使用线性回归），而 $\hat{\mu_0}(x)$ 是 $E[Y|X, T=0]$ 的估计。正如你可能已经猜到的，双重稳健估计的第一部分估计 $E[Y_1]$，第二部分估计 $E[Y_0]$。让我们先考察第一部分，因为所有直觉同样可以通过类比应用于第二部分。

既然我知道这个公式初看之下令人畏惧（但别担心，你会发现它其实非常简单），我将首先展示如何编写这个估计量的代码。我感觉有些人面对代码时的恐惧感比面对公式要少。让我们看看这个估计器在实践中是如何运作的，好吗？

```{dropdown} 查看 Stata 代码
```stata
global contvar "school_mindset school_achievement school_ethnic_minority school_poverty school_size"

global categvar "ethnicity_* gender_* school_urbanicity_*"

* Step 1: Estimate propensity scores (treatment model)
logit intervention $contvar i.${categvar}  // Include all covariates
predict ps, pr  // Generate propensity scores

* Step 2: Create inverse probability weights
gen ipw = intervention/ps + (1-intervention)/(1-ps)

* Step 3: Estimate weighted outcome model (outcome regression)
reg achievement_score intervention $contvar i.${categvar} [pw=ipw]

* Step 4: Extract ATE estimate
lincom intervention  // Get average treatment effect with confidence intervals

* Clean up
drop ps ipw

* teffects：ipwra - double robust estimator, the result as same as above
teffects ipwra (achievement_score $contvar i.${categvar}) ///
               (intervention $contvar i.${categvar}, logit)

```python
def doubly_robust(df, X, T, Y):
    ps = LogisticRegression(C=1e6, max_iter=1000).fit(df[X], df[T]).predict_proba(df[X])[:, 1]
    mu0 = LinearRegression().fit(df.query(f"{T}==0")[X], df.query(f"{T}==0")[Y]).predict(df[X])
    mu1 = LinearRegression().fit(df.query(f"{T}==1")[X], df.query(f"{T}==1")[Y]).predict(df[X])
    return (
        np.mean(df[T]*(df[Y] - mu1)/ps + mu1) -
        np.mean((1-df[T])*(df[Y] - mu0)/(1-ps) + mu0)
    )
```

```python
T = 'intervention'
Y = 'achievement_score'
X = data_with_categ.columns.drop(['schoolid', T, Y])

doubly_robust(data_with_categ, X, T, Y)
```

双重稳健估计量表明，就成就而言，我们应预期参加心态研讨会的个体比未接受处理的同伴高出 0.388 个标准差。再次强调，我们可以利用自助法构建置信区间。

```python
from joblib import Parallel, delayed # for parallel processing

np.random.seed(88)
# run 1000 bootstrap samples
bootstrap_sample = 1000
ates = Parallel(n_jobs=4)(delayed(doubly_robust)(data_with_categ.sample(frac=1, replace=True), X, T, Y)
                          for _ in range(bootstrap_sample))
ates = np.array(ates)
```

```python
print(f"ATE 95% CI:", (np.percentile(ates, 2.5), np.percentile(ates, 97.5)))
```

```python
sns.distplot(ates, kde=False)
plt.vlines(np.percentile(ates, 2.5), 0, 20, linestyles="dotted")
plt.vlines(np.percentile(ates, 97.5), 0, 20, linestyles="dotted", label="95% CI")
plt.title("ATE Bootstrap Distribution")
plt.legend()
plt.show() ;
```

既然我们已经初步了解了双重稳健估计量，接下来让我们探讨其卓越之处。首先，它之所以被称为双重稳健，是因为只需模型 $\hat{P}(x)$ 或 $\hat{\mu}(x)$其中之一被正确设定即可。为了理解这一点，我们来看估计 $E[Y_1]$ 的第一部分，并仔细分析它。

$
\hat{E}[Y_1] = \frac{1}{N}\sum \bigg( \dfrac{T_i(Y_i - \hat{\mu_1}(X_i))}{\hat{P}(X_i)} + \hat{\mu_1}(X_i) \bigg)
$

假设 $\hat{\mu_1}(x)$ 是正确的。如果倾向得分模型有误，我们无需担忧。因为若 $\hat{\mu_1}(x)$ 正确，那么 $E[T_i(Y_i - \hat{\mu_1}(X_i))]=0$。这是由于乘以 $T_i$ 仅筛选出处理组，而根据定义，$\hat{\mu_1}$ 在处理组上的残差均值为零。这导致整个表达式简化为 $\hat{\mu_1}(X_i)$ ，根据假设，$E[Y_1]$ 的估计是正确的。因此，您看，$\hat{\mu_1}(X_i)$ 的正确性消除了倾向得分模型的相关性。我们可以运用相同的逻辑来理解 $E[Y_0]$ 的估计量。

但别只听我的一面之词。让代码为你指明方向！在接下来的估计器中，我将用于估算倾向得分的逻辑回归替换为一个从 0.1 到 0.9 的随机均匀变量（我不希望极小的权重导致倾向得分方差激增）。由于这一变量是随机的，它绝不可能成为一个优质的倾向得分模型，但我们将会看到，双重稳健估计器仍能生成与使用逻辑回归估算倾向得分时极为接近的估计结果。

```{dropdown} 查看 Stata 代码
```stata
* (1) wrong propensity score
* Set random seed for reproducibility
set seed 654

* Generate wrong propensity scores (uniform between 0.1 and 0.9)
gen ps = runiform(0.1, 0.9)

* View summary statistics to verify
summarize ps

* Step 1: Create inverse probability weights
gen ipw = intervention/ps + (1-intervention)/(1-ps)

* Step 2: Estimate weighted outcome model (outcome regression)
reg achievement_score intervention $contvar i.${categvar} [pw=ipw]

* Step 3: Extract ATE estimate
lincom intervention  // Get average treatment effect with confidence intervals

* Clean up
drop ps ipw

```python
from sklearn.linear_model import LogisticRegression, LinearRegression

def doubly_robust_wrong_ps(df, X, T, Y):
    # wrong PS model
    np.random.seed(654)
    ps = np.random.uniform(0.1, 0.9, df.shape[0])
    mu0 = LinearRegression().fit(df.query(f"{T}==0")[X], df.query(f"{T}==0")[Y]).predict(df[X])
    mu1 = LinearRegression().fit(df.query(f"{T}==1")[X], df.query(f"{T}==1")[Y]).predict(df[X])
    return (
        np.mean(df[T]*(df[Y] - mu1)/ps + mu1) -
        np.mean((1-df[T])*(df[Y] - mu0)/(1-ps) + mu0)
    )
```

```python
doubly_robust_wrong_ps(data_with_categ, X, T, Y)
```

如果我们使用自助法（bootstrap），可以看到方差比用逻辑回归估计倾向得分时略高。

```python
np.random.seed(88)
parallel_fn = delayed(doubly_robust_wrong_ps)
wrong_ps = Parallel(n_jobs=4)(parallel_fn(data_with_categ.sample(frac=1, replace=True), X, T, Y)
                              for _ in range(bootstrap_sample))
wrong_ps = np.array(wrong_ps)
```

```python
print(f"Original ATE 95% CI:", (np.percentile(ates, 2.5), np.percentile(ates, 97.5)))

print(f"Wrong PS ATE 95% CI:", (np.percentile(wrong_ps, 2.5), np.percentile(wrong_ps, 97.5)))
```

正如我们所见，搞砸倾向得分会导致略有不同的平均处理效应（ATE），但差异不大。这涵盖了倾向模型错误但结果模型正确的情况。那么另一种情形又如何呢？让我们再次仔细审视估计量的第一部分，不过这次让我们重新排列一些项。

$
\hat{E}[Y_1] = \frac{1}{N}\sum \bigg( \dfrac{T_i(Y_i - \hat{\mu_1}(X_i))}{\hat{P}(X_i)} + \hat{\mu_1}(X_i) \bigg)
$

$
\hat{E}[Y_1] = \frac{1}{N}\sum \bigg( \dfrac{T_iY_i}{\hat{P}(X_i)} - \dfrac{T_i\hat{\mu_1}(X_i)}{\hat{P}(X_i)} + \hat{\mu_1}(X_i) \bigg)
$

$
\hat{E}[Y_1] = \frac{1}{N}\sum \bigg( \dfrac{T_iY_i}{\hat{P}(X_i)} - \bigg(\dfrac{T_i}{\hat{P}(X_i)} - 1\bigg) \hat{\mu_1}(X_i) \bigg)
$

$
\hat{E}[Y_1] = \frac{1}{N}\sum \bigg( \dfrac{T_iY_i}{\hat{P}(X_i)} - \bigg(\dfrac{T_i - \hat{P}(X_i)}{\hat{P}(X_i)}\bigg) \hat{\mu_1}(X_i) \bigg)
$

现在，假设倾向得分 $\hat{P}(X_i)$ 被正确设定。在这种情况下，$E[T_i - \hat{P}(X_i)]=0$ 消除了依赖于 $\hat{\mu_1}(X_i)$ 的部分。这使得双重稳健估计量简化为倾向得分加权估计量 $\frac{T_iY_i}{\hat{P}(X_i)}$，根据假设这是正确的。因此，即使 $\hat{\mu_1}(X_i)$ 是错误的，只要倾向得分设定正确，估计量仍将保持正确。

如果你更相信代码而非公式，那么这里提供了实际的验证。在下面的代码中，我将两个回归模型都替换为一个随机正态变量。毫无疑问，$\hat{\mu}(X_i)$ 并**未正确设定**。尽管如此，我们将看到双重稳健估计仍能恢复之前所见的大约 0.38 的 $\hat{ATE}$。

```{dropdown} 查看 Stata 代码
```stata
* (2) wrong mu(x) model:wrong linear model
* Step 1: Estimate propensity scores (treatment model)
logit intervention $contvar i.${categvar}  // Include all covariates
predict ps, pr  // Generate propensity scores

* Step 2: Create inverse probability weights
gen ipw = intervention/ps + (1-intervention)/(1-ps)

* Step 3: Estimate weighted outcome model (outcome regression)
reg achievement_score intervention [pw=ipw]

* Step 4: Extract ATE estimate
lincom intervention  // Get average treatment effect with confidence intervals

* Clean up
drop ps ipw

```python
from sklearn.linear_model import LogisticRegression, LinearRegression

def doubly_robust_wrong_model(df, X, T, Y):
    np.random.seed(654)
    ps = LogisticRegression(C=1e6, max_iter=1000).fit(df[X], df[T]).predict_proba(df[X])[:, 1]
    
    # wrong mu(x) model
    mu0 = np.random.normal(0, 1, df.shape[0])
    mu1 = np.random.normal(0, 1, df.shape[0])
    return (
        np.mean(df[T]*(df[Y] - mu1)/ps + mu1) -
        np.mean((1-df[T])*(df[Y] - mu0)/(1-ps) + mu0)
    )
```

```python
doubly_robust_wrong_model(data_with_categ, X, T, Y)
```

我们可以再次使用自助法（bootstrap），发现方差只是略高一些。

```python
np.random.seed(88)
parallel_fn = delayed(doubly_robust_wrong_model)
wrong_mux = Parallel(n_jobs=4)(parallel_fn(data_with_categ.sample(frac=1, replace=True), X, T, Y)
                               for _ in range(bootstrap_sample))
wrong_mux = np.array(wrong_mux)
```

```python
print(f"Original ATE 95% CI:", (np.percentile(ates, 2.5), np.percentile(ates, 97.5)))
print(f"Wrong Mu ATE 95% CI:", (np.percentile(wrong_mux, 2.5), np.percentile(wrong_mux, 97.5)))
```

再次，仅对条件均值模型进行干扰，所得的平均处理效应(ATE)差异微乎其微。我希望我已让你信服双重稳健估计的强大之处。其神奇效果源于因果推断中消除估计偏误的两种途径：要么建模处理机制，要么建模结果机制。只要其中任一模型正确，便能确保估计的有效性。

有一点需要注意的是，在实践中，精确建模这两者中的任何一个都非常困难。更常见的情况是，倾向得分和结果模型都不完全正确。它们都存在错误，但错误的方式不同。当这种情况发生时，究竟是使用单一模型还是双重稳健估计更为优越，目前尚未有定论[\[1\]](https://www.stat.cmu.edu/~ryantibs/journalclub/kang_2007.pdf) [\[2\]](https://arxiv.org/pdf/0804.2969.pdf) [\[3\]](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2798744/)。就我个人而言，我仍然倾向于使用它们，因为这至少提供了两种可能正确的途径。

## 核心要点

在此，我们介绍了一种将线性回归与倾向得分相结合以生成双重稳健估计量的简便方法。该估计量之所以得名，是因为它仅需其中一个模型正确即可发挥作用。若倾向得分模型正确，即便结果模型有误，我们仍能识别因果效应；反之，若结果模型正确，即使倾向得分模型存在偏误，因果效应的识别同样可行。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 13 - 双重差分法

## 巴西南部的三块广告牌

记得我曾从事市场营销工作时，网络广告是一种极佳的手段。并非因其效率极高（尽管确实如此），而是因为它能轻易判断效果好坏。在线营销让你知晓哪些客户看到了广告，并通过 cookie 追踪他们是否最终访问了你的着陆页或点击了下载按钮。你还可以运用机器学习寻找与现有客户高度相似的潜在客户，仅向他们展示广告。从这个角度看，在线营销极为精准：你能精准定位目标人群，并清晰看到他们是否如预期般做出回应。

但并非所有人都易受网络营销的影响。有时，你不得不采用不那么精准的技术手段，比如电视广告或在街头竖立广告牌。通常，营销部门追求的是营销渠道的多样性。不过，如果将网络营销比作捕捉特定金枪鱼的专业钓竿，那么广告牌和电视广告则像是撒向鱼群的巨网，希望能至少捕获一些大鱼。广告牌和电视广告的另一个问题在于，其效果更难衡量。当然，你可以在某个地方投放广告牌前后，测量购买量或任何你想推动的指标。如果有所增长，就有证据表明营销是有效的。但你怎么知道这种增长不是产品认知度自然上升的趋势呢？换句话说，如果你一开始就没有设置广告牌，情况会如何发展——你如何知晓这一反事实 $Y_0$？

![img](./images/13/secrets.png)

解答此类问题的一种方法是双重差分法（Difference-in-Difference），简称 diff-in-diff。该方法常用于评估宏观干预措施的效果，例如移民对失业率的影响、枪支法律变更对犯罪率的影响，或是单纯衡量营销活动带来的用户参与度差异。这些案例中，干预前后各有一个时间段，研究者需要从总体趋势中剥离出干预措施的实际影响。以一个我亲身处理过的类似问题为例进行说明。

为评估广告牌作为营销渠道的效果，我们在南里奥格兰德州首府POA（阿雷格里港市）设置了 3 块广告牌，旨在观察此举是否能促进储蓄账户存款增长。需补充说明的是，对巴西地理不太熟悉的读者应注意：南里奥格兰德州位于该国南部，属于最发达地区之一。

考虑到这一点，我们决定同时分析来自南方另一座首府城市——圣卡塔琳娜州首府FL的数据。我们的设想是将FL作为对照组样本，用以估算与POA相比的反事实 $Y_0$ （需要说明的是，这并非真实的实验，真实实验数据保密，但思路非常相似）。我们在POA投放了整六月的广告牌，所获数据如下：

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/billboard_impact.csv", clear


list in 1/5

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from matplotlib import style
from matplotlib import pyplot as plt
import seaborn as sns
import statsmodels.formula.api as smf


%matplotlib inline

style.use("fivethirtyeight")
```

```python
data = pd.read_csv("data/billboard_impact.csv")
data.head()
```

需谨记，存款量是我们的结果变量，即我们希望通过广告牌提升的指标。POA 是POA（阿雷格里港市）的虚拟指示变量。当其值为零时，表示样本来自FL市。Jul 是代表七月或干预后时期的虚拟变量。当其值为零时，则指代五月这一干预前时期的样本。

## DID 估计量

为避免时间和处理变量混淆，下文将用 D 表示处理变量，T 表示时间变量。设 $Y_D(T)$ 为时期 T 下处理 D 的潜在结果。在能够观测反事实的理想情况下，我们将通过以下方式估算干预的处理效应：

$
\hat{ATET} = E[Y_1(1) - Y_0(1)|D=1]
$

换言之，因果效应是干预后时期接受处理的结果减去同期未接受处理的结果。当然，由于 $Y_0(1)$ 属于反事实范畴，我们无法实际测量这一效应。

一种解决方法是进行前后对比分析。

$
\hat{ATET} = E[Y(1)|D=1] - E[Y(0)|D=1]
$

在我们的例子中，我们将比较 POA 在广告牌设置前后的平均存款额。

```{dropdown} 查看 Stata 代码
```stata
* 用处理组处理前作为反事实结果
* Calculate mean deposits for POA group before event (jul==0)
quietly summarize deposits if poa == 1 & jul == 0
local poa_before = r(mean)

* Calculate mean deposits for POA group after event (jul==1)
quietly summarize deposits if poa == 1 & jul == 1
local poa_after = r(mean)

* Calculate the difference
local diff = `poa_after' - `poa_before'

* Display results
display "POA Before: " `poa_before'
display "POA After: " `poa_after'
display "Difference (After - Before): " `diff'

```python
poa_before = data.query("poa==1 & jul==0")["deposits"].mean()

poa_after = data.query("poa==1 & jul==1")["deposits"].mean()

poa_after - poa_before
```

该估计值表明，干预后存款预计会增加 41.04 雷亚尔。但我们能相信这一结果吗？

注意到 $E[Y(0)|D=1]=E[Y_0(0)|D=1]$，即处理单元在干预前的观测结果与其反事实结果相同。由于我们正利用这一点来估计干预后的反事实结果 $E[Y_0(1)|D=1]$，上述估计隐含了 $E[Y_0(1)|D=1] = E[Y_0(0)|D=1]$ 的假设。

它指出，在没有干预的情况下，后期的结果将与起始时期的结果相同。如果结果变量遵循任何趋势，这一说法显然不成立。例如，假设存款在 POA 中呈上升趋势，$E[Y_0(1)|D=1] > E[Y_0(0)|D=1]$，这意味着即使没有干预，后期的结果也会大于起始时期的结果。同理，若 Y 呈下降趋势，$E[Y_0(1)|D=1] < E[Y_0(0)|D=1]$。这表明这种前后对比的方法并非一个良好的估计量。

另一个想法是将处理组与未接受干预的未处理组进行比较：

$
\hat{ATET} = E[Y(1)|D=1] - E[Y(1)|D=0]
$

在我们的例子中，就是要比较干预后POA与FL的存款情况。

```{dropdown} 查看 Stata 代码
```stata
* 用控制组处理后作为反事实结果
* Calculate mean deposits for FL group after event (jul==1)
quietly summarize deposits if poa == 0 & jul == 1
local fl_after = r(mean)

* Calculate mean deposits for POA group after event (jul==1)
quietly summarize deposits if poa == 1 & jul == 1
local poa_after = r(mean)

* Calculate the difference
local diff = `poa_after' - `fl_after'

* Display results
display "FL After: " `fl_after'
display "POA After: " `poa_after'
display "Difference (After - Before): " `diff'


```python
fl_after = data.query("poa==0 & jul==1")["deposits"].mean()
poa_after - fl_after
```

该估计量告诉我们，营销活动产生了负面影响，客户存款将减少 119.10 雷亚尔。

注意 $E[Y(1)|D=0]=E[Y_0(1)|D=0]$。由于我们使用 $E[Y(1)|D=0]$ 来估算干预后处理组的反事实情况，我们假设可以这样替代缺失的反事实：$E[Y_0(1)|D=0] = E[Y_0(1)|D=1]$。但需注意，这仅在两组基线水平极为相似时才成立。例如，若FL的存款远多于POA，此假设便不成立，因为 $E[Y_0(1)|D=0] > E[Y_0(1)|D=1]$。反之，若FL的存款水平较低，则会有 $E[Y_0(1)|D=0] < E[Y_0(1)|D=1]$。

重申一次，这不是个好主意。为解决此问题，我们可以结合空间与时间比较。这就是双重差分法的核心思想，其通过以下方式替代缺失的反事实：

$
E[Y_0(1)|D=1] = E[Y_0(0)|D=1] + (E[Y_0(1)|D=0] - E[Y_0(0)|D=0])
$

这个方法的做法是：取处理组在干预前的观测值，并加上一个趋势项，而这个趋势是通过对照组估计出来的，即：$E[Y_0(1)|D=0] - E[Y_0(0)|D=0]$。换句话说，它的含义是：
若处理组在干预后没有接受干预，它的表现将会像是“干预前的自己”再加上一个“与对照组增长趋势相同的增长因子”。

需注意的是，这一方法假设处理组与对照组的趋势相同：

$
E[Y_0(1) − Y_0(0)|D=1] = E[Y_0(1) − Y_0(0)|D=0]
$

其中左侧为反事实趋势。现在，我们可以在处理效应定义中替换估计的反事实 $E[Y_1(1)|D=1] - E[Y_0(1)|D=1]$

$
\hat{ATET} = E[Y(1)|D=1] - (E[Y(0)|D=1] + (E[Y(1)|D=0] - E[Y(0)|D=0])
$

若重新排列各项，即可得到经典的双重差分估计量。

$
\hat{ATET} = (E[Y(1)|D=1] - E[Y(1)|D=0]) - (E[Y(0)|D=1] - E[Y(0)|D=0])
$

之所以得此名，是因为它捕捉的是处理前后处理组与对照组之间差异的差值。

以下是其在代码中的表现形式。

```{dropdown} 查看 Stata 代码
```stata
* DID
* Calculate mean deposits for POA group before event (jul==0)
quietly summarize deposits if poa == 1 & jul == 0
local poa_before = r(mean)

* Calculate mean deposits for POA group after event (jul==1)
quietly summarize deposits if poa == 1 & jul == 1
local poa_after = r(mean)

* Calculate mean deposits for FL group before event (jul==0)
quietly summarize deposits if poa == 0 & jul == 0
local fl_before = r(mean)

* Calculate mean deposits for FL group after event (jul==1)
quietly summarize deposits if poa == 0 & jul == 1
local fl_after = r(mean)

* Calculate the difference in difference
local did = (`poa_after' - `poa_before') - (`fl_after' - `fl_before')


* Display results
display "FL trends: " `fl_after' - `fl_before'
display "POA trends: " `poa_after' - `poa_before'
display "Difference in difference: " `did'

```python
fl_before = data.query("poa==0 & jul==0")["deposits"].mean()

diff_in_diff = (poa_after-poa_before)-(fl_after-fl_before)
diff_in_diff
```

双重差分法告诉我们，预计每位客户的存款将增加 6.52 雷亚尔。值得注意的是，双重差分法所做的假设比另外两种估计量更为合理。它仅假设两座城市的增长模式相同，既不要求它们具有相同的基准水平，也不要求增长趋势为零。

为了直观展示双重差分法的作用，我们可以将未处理组的增长趋势投射到处理组中，以观察反事实——即在没有干预的情况下，我们预期会看到的存款数量。

```{dropdown} 查看 Stata 代码
```stata
clear
input period str3 month fl_values poa_values counterfactual
1 "May" 171.642308 46.016 46.016
2 "Jul" 206.1655 87.06375 80.539192
end

* Create the plot with proper legend labeling
twoway ///
    (connected fl_values period, lwidth(2) lcolor(blue) mcolor(blue)) ///
    (connected poa_values period, lwidth(2) lcolor(red) mcolor(red)) ///
    (line counterfactual period, lwidth(2) lcolor(green) lpattern(dash)), ///
    xlabel(1 "May" 2 "Jul") ///
    xtitle("Month") ytitle("Deposits") ///
    title("Deposit Trends Comparison") ///
    legend(order(1 "FL" 2 "POA" 3 "Counterfactual") position(6) rows(1)) ///
    graphregion(color(white)) plotregion(color(white))

* diff_plot: A Stata Module to Visualize Two-Period, Two-Group Difference-In-Differences
* ssc install elabel, replace //installing elabel 
* ssc install diff_plot //installing diff_plot


```python
plt.figure(figsize=(10,5))
plt.plot(["May", "Jul"], [fl_before, fl_after], label="FL", lw=2)
plt.plot(["May", "Jul"], [poa_before, poa_after], label="POA", lw=2)

plt.plot(["May", "Jul"], [poa_before, poa_before+(fl_after-fl_before)],
         label="Counterfactual", lw=2, color="C2", ls="-.")

plt.legend()
plt.show() ;
```

注意到红色与黄色虚线之间的细微差别了吗？若仔细观察，你会发现POA存在微小的处理效应。

![img](./images/13/cant-read.png)


此刻你可能在自问：“我该多大程度上信任这个估计量？我有权要求查看标准误报告！”这合情合理，因为缺乏标准误的估计量显得不够严谨。为此，我们将运用一个巧妙的回归技巧。具体而言，我们将估计以下线性模型：

$
Y_i = \beta_0 + \beta_1 POA_i + \beta_2 Jul_i + \beta_3 POA_i*Jul_i + e_i
$

注意，$\beta_0$ 是控制的基线。在我们的案例中，即FL五月份的存款水平。若开启处理城市虚拟变量，则得到 $\beta_1$。因此，$\beta_0 + \beta_1$ 代表干预前五月POA的基线，而 $\beta_1$ 是在FL基础上POA的基线增量。关闭POA虚拟变量并开启七月虚拟变量时，得到 $\beta_0 + \beta_2$，即干预期后七月份FL的水平。$\beta_2$ 则是控制的趋势，因为我们在基线之上加上它以得到干预后时期的控制水平。简而言之，$\beta_1$ 是从控制组转向处理组时的增量，$\beta_2$ 是从干预前到干预后时期的增量。最后，若同时开启两个虚拟变量，则得到 $\beta_3$。$\beta_0 + \beta_1 + \beta_2 + \beta_3$是干预后POA的水平。因此，$\beta_3$ 是从五月到七月、从FL到波阿时的增量影响。换言之，这就是双重差分估计量。

若您对我的说法存疑，不妨亲自验证一番。您将得到与我们先前完全一致的结果数值，同时也会注意到我们如何获得了梦寐以求的标准误差。

```{dropdown} 查看 Stata 代码
```stata
* TWFE 
reg deposits i.poa##i.jul

```python
smf.ols('deposits ~ poa*jul', data=data).fit().summary().tables[1]
```

## 非平行趋势

双重差分法（Diff-in-Diff）一个显而易见的问题是无法满足平行趋势假设。若处理组的增长趋势与对照组不同，双重差分估计将产生偏误。这在非随机数据中尤为常见，例如决定对某地区实施干预是基于其可能对干预反应良好的潜力，或是干预针对的是表现欠佳的地区。以我们的营销案例为例，我们选择在 POA 测试广告牌，并非为了检验广告牌的一般效果，而是因为该地区销售业绩不佳。或许线上营销在当地效果不彰。这种情况下，若未设置广告牌， POA 的增长可能低于其他城市的观测值，这将导致我们低估广告牌在该地的效果。

一种验证这种情况是否发生的方法是绘制过去时期的趋势图。例如，假设 POA 呈现小幅下降趋势，而 FL 则处于急剧上升阶段。此时，展示前期数据将揭示这些趋势，从而让我们明白双重差分法并非可靠的估计量。

```{dropdown} 查看 Stata 代码
```stata
* Non PT
* Create temporary dataset for plotting
clear
input period str3 month fl_values poa_values counterfactual
1 "Jan" 120 60 .
2 "Mar" 150 50 .
3 "May" 171.642308 46.016 46.016
4 "Jul" 206.1655 87.06375 80.539192
end

* Create the plot
twoway ///
    (connected fl_values period, lwidth(2) lcolor(blue) mcolor(blue)) ///
    (connected poa_values period, lwidth(2) lcolor(red) mcolor(red)) ///
    (line counterfactual period if period >= 3, lwidth(2) lcolor(green) lpattern(dash)), ///
    xline(3, lpattern(dash) lcolor(gs10) lwidth(1)) ///
	xlabel(1 "Jan" 2 "Mar" 3 "May" 4 "Jul", angle(45)) ///
    xtitle("") ytitle("Deposits") ///
    title("Deposit Trends Comparison", size(medium)) ///
    legend(order(1 "FL" 2 "POA" 3 "Counterfactual") position(6) rows(1)) ///
    graphregion(color(white)) plotregion(color(white)) ///
    scheme(s1color) ///
    xsize(10) ysize(5)  // Sets figure size to 10x5 inches

```python
plt.figure(figsize=(10,5))
x = ["Jan", "Mar", "May", "Jul"]

plt.plot(x, [120, 150, fl_before,  fl_after], label="FL", lw=2)
plt.plot(x, [60, 50, poa_before, poa_after], label="POA", lw=2)

plt.plot(["May", "Jul"], [poa_before, poa_before+(fl_after-fl_before)], label="Counterfactual", lw=2, color="C2", ls="-.")

plt.legend()
plt.show() ;
```

我们将探讨如何利用合成控制法解决这一问题。该方法通过整合多个城市的数据构建一个合成城市，使其趋势与目标城市高度吻合。但需谨记，在应用双重差分法时，必须始终验证平行趋势假设是否成立。

![img](./images/13/non-parallel.png)

最后值得指出的是，若仅有聚合数据，则无法为双重差分估计量构建置信区间。例如，当缺乏 FL 或 POA 每位客户的具体行为数据，仅掌握干预前后两城市存款均值时，虽仍可通过双重差分估计因果效应，但无法获知其方差。这是因为数据中的所有变异性在聚合过程中被完全消除。

## 核心要点

我们探讨了一种广泛应用于估计更宏观实体（如学校、城市、州、国家等）因果效应的技术——双重差分法。该方法通过比较处理单元在干预前后的结果趋势与控制单元的趋势差异，来评估处理效应。在此案例中，我们看到了如何利用这一方法来估计城市特定营销活动的效果。

最后，我们分析了当处理单元与控制单元的趋势不一致时，双重差分法为何会失效。同时，我们也了解到，若仅有聚合数据，双重差分法的应用将面临显著问题。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 14 - 面板数据与固定效应

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from matplotlib import style
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf
import graphviz as gr
from linearmodels.datasets import wage_panel

%matplotlib inline
pd.set_option("display.max_columns", 6)
style.use("fivethirtyeight")
```

在前一章中，我们探讨了一个非常简单的双重差分（Diff-in-Diff）设定，其中包含一个处理组和一个对照组（分别为POA市和FLN市），并且只观察两个时间点——干预前和干预后。但如果我们有更多时间点，或者更多的城市，会发生什么？事实上，这类设定在因果推断中非常常见，也非常有用，以至于它有一个专门的名称：面板数据（panel data）。面板数据是指对同一单位在多个时间段内进行重复观测。这在政府政策评估中极为常见，例如我们可以对多个城市或州在若干年间的情况进行跟踪；在商业中也很常见，企业会持续记录用户在多个时间段的行为数据。
 
为了理解如何利用这种数据结构，我们继续之前的例子，即我们试图评估在POA市设置广告牌（作为处理）是否会提高用户对我们投资产品的使用率。具体而言，我们希望评估，在有广告牌的情况下，投资账户中的存款会增加多少。
 
在前一章中，我们将双重差分（DiD）估计量作为一种插补策略（反事实推测方法），用于推测若未在POA市设置广告牌，该城市可能发生的情况。我们提出，干预（设置广告牌）后POA市的反事实结果Y_0可通过干预前该市的存款数量加上一个增长因子来估算。这一增长因子是通过对照城市FLN市估计得出的。为回顾相关符号表示，以下是估算该反事实结果的方法
 
$$
\underbrace{\widehat{Y_0(1)|D=1}}_{\substack{\text{如果POA在干预后}} \\ \substack{\text{没有设置广告牌的反事实结果}}} 
= \underbrace{Y_0(0)|D=1}_{\substack{\text{POA在干预前的}} \\ \substack{\text{实际结果}}} 
+ \big( \underbrace{Y_0(1)|D=0}_{\substack{\text{FLN在干预后的}} \\ \substack{\text{实际结果}}} 
- \underbrace{Y_0(0)|D=0}_{\substack{\text{FLN在干预前的}} \\ \substack{\text{实际结果}}} \big)
$$
 
其中$t$代表时间，$D$代表处理（因为$t$已被采用），$Y_D(t)$表示在$t$时期接受处理$D$的潜在结果（例如，$Y_0(1)$是第1时期对照组的潜在结果）。现在，若采用这一插补的潜在结果，我们可按如下方式还原POA的处理效应（ATT）
 
$$
\widehat{ATT} = \underbrace{Y_1(1)|D=1}_{\substack{\text{POA在干预后的} \\ \text{实际结果}}} - \widehat{Y_0(1)|D=1}
$$
 
换言之，POA市设置广告牌的效应，是在POA市设置广告牌后观察到的结果减去估计的若不设置广告牌时的反事实结果。此外，回顾一下，双重差分（DiD）方法的优势在于，估计该反事实结果只需要假设POA的存款增长与FLN的存款增长相匹配。这就是关键的平行趋势假设。我们需要对这一假设进行更深入的讨论，因为它在后续的分析中将发挥非常重要的作用。
 
 
## 平行趋势
 
我们可以把“平行趋势假设”理解为一种独立性假设。如果你还记得前面章节提到的内容，独立性假设指的是：处理分配与潜在结果是独立的，也就是说：
 
$$
Y_d \perp  D
$$
 
这意味着我们在分配处理时，不能因为某些单位（比如城市）的结果本身就更高（这会导致高估处理效应），或者本身就更低（这会导致低估处理效应），而给它们更多或更少的处理。举个更具体的例子：假设你的市场经理只在那些原本就有很高存款的城市投放广告牌，这样他就可以之后夸张地说：“看，投放广告牌的城市存款增长了很多，营销活动非常成功！”——这显然违反了独立性假设，因为我们正在向存款高的城市提供处理。此外，独立性假设还有一个自然的延伸版本，叫做条件独立性假设。它允许潜在结果与处理有关，但只要我们控制了混杂因素$X$，这种相关性就会消失，即：
 
$
Y_d \perp D | X
$
 
 
这些你都已了然于心。但具体而言，这与双重差分法（DiD）和平行趋势假设有何关联？如果说传统的独立性假设要求“处理分配不能与潜在结果的水平有关”，那么平行趋势假设要求的是“处理分配不能与潜在结果的增长趋势有关”。换句话说，不能是增长快的地方才被分配到处理。平行趋势假设的一种表述方式如下：
 
$
\big(Y_d(t) - Y_d(t-1) \big)  \perp D
$
 
 
用更直白的话来说，这一假设意味着我们可以将处理分配给那些结果水平高或低的单位，这没有问题。但我们不能根据“增长速度”来决定处理分配。以广告牌为例，如果我们只在原本存款就高的城市投放广告牌，这是可以接受的；但如果我们只在那些存款增长最快的城市投放广告牌，那就违反了平行趋势假设。这其实非常好理解，因为双重差分法的核心就是用对照组的“增长”来估计处理组在没有接受处理时会发生的增长。如果处理组的增长本来就和对照组不同，那我们就很难得出可信的因果结论。

## 控制不可见因素
 
倾向得分（propensity score）、线性回归、匹配法（matching）等方法在处理非随机数据中的混杂因素时很有用，但它们依赖一个关键假设：条件无混淆性（Conditional Unconfoundedness）：
 
$
(Y_0, Y_1) \perp T | X
$
 
简单来说，它们要求所有混杂因素都是已知且可测量的，这样我们就能对它们进行控制，使得处理分配就像是随机的一样。这类方法的一个主要问题在于，有时我们根本无法测量某些混杂因素。例如，在劳动经济学中有一个经典问题是：婚姻是否会影响男性的收入。经济学研究普遍发现，已婚男性比未婚男性收入更高。但这个现象到底是因果关系，还是只是相关性呢？

可能的解释是：受教育程度更高的男性既更容易结婚，也更可能获得高薪工作。如果是这样，教育水平就是婚姻对收入影响的混杂因素。对于这种情况，我们可以测量受教育程度，并在回归中加以控制。

但还有一些因素，比如外貌，也可能同时影响结婚和收入（长得更帅的男性可能更容易结婚，也更容易被雇佣拿到高薪）。遗憾的是，外貌与智力类似，属于我们难以精确测量的特征之一。

这使我们陷入了一个困境，因为若存在未测量的混杂因素，就会产生偏误。之前提到的一种解决方法是使用工具变量，但要找到合适的工具变量并非易事，需要极大的创造力。此处，我们不妨利用面板数据结构来应对这一挑战。

我们已经知道，面板数据可以让我们用“平行趋势假设”来替代“条件无混淆性假设”。但它到底是如何帮助应对未测量的混杂因素？首先，让我们看看代表这种设置的因果图，其中我们拥有跨时间的重复观测。这里，我们追踪同一观测对象跨越4个时间段。婚姻（处理）和收入（结果）随时间变化。具体而言，婚姻在第3和第4期启动（从0变为1），而收入在同一时期增加。外貌这一未测量的混杂因素在所有时期保持不变（这是一个大胆的假设，但如果时间跨度仅为几年则合理）。那么，我们如何知道收入增加是由于婚姻而非仅仅因为外貌混杂因素的增加？更重要的是，我们如何控制这个看不见的混杂因素？

 
![img](./images/14/fe-graph.png)
 
关键在于认识到，只要我们“聚焦”在某一个单位（比如一个人、一座城市），并追踪其随时间的变化，我们实际上就已经控制了所有“随时间保持不变”的因素。这其中就包括那些无法观测但在时间上固定的混杂变量。例如，再上图中，我们可以确定收入的增长不可能源于外貌的提升，因为外貌本身保持不变（它是“时间固定”变量）。核心观点是，尽管我们无法直接控制外貌这一不可测量的变量，但借助于面板数据结构，这一问题便迎刃而解。

换一种方式理解，我们可以把这些“时间固定的混杂变量”看作是每个单位所特有的属性。这相当于在因果图中添加一个中间单位节点。这时候，控制单位本身就已经阻断了结果与任何未被观测但时间固定的混杂变量之间的后门路径。

 
![img](./images/14/control-unit.png)
 
试想一下，我们虽无法直接衡量诸如外貌与智力这类特质，但可以确认拥有这些特质的个体在时间维度上具有同一性。实施这种控制的操作机制其实相当简单：只需创建代表该个体的虚拟变量，并将其纳入线性模型即可。所谓“控制个体本身”，即指在模型中添加一个（此处为虚拟）变量以标识特定个体。当模型中包含此人虚拟变量来估算婚姻对收入的影响时，回归分析能在保持个体变量恒定的条件下得出婚姻的效应。这种添加单位虚拟变量的方法，正是我们所说的固定效应模型。

## 固定效应

为使表述更规范化，首先审视现有数据结构。延续前例，我们将尝试估算婚姻对收入的影响。数据包含多个个体（ **nr**）在多个年份的这两个变量—— **married**与 **lwage**（工资的对数形式），此外还包括当年工作小时数、受教育年限等其他控制变量。

```{dropdown} 查看 Stata 代码
```stata
* 无法获得原书中的wage_panel数据
webuse nlswork, clear  // Similar panel wage data in Stata

list year msp in 1/5

```python
data = wage_panel.load()
data.head()
```

一般而言，固定效应模型定义为

$
y_{it} = \beta X_{it} + \gamma U_i + e_{it}
$

其中：
 - $y_{it}$：表示个体$i$在时间$t$的结果（例如对数工资）；
 - $X_{it}$：是与时间相关的可观测变量（例如婚姻状态、工作经验等）；
 - $U_i$：是个体$i$的一组不可观测但在时间上保持不变的特征（比如外貌、智力等）；
 - $e_{it}$：是误差项。



现在，请回想我曾说过，在固定效应模型中使用面板数据就像为实体添加虚拟变量一样简单。这个说法在原理上是对的，但在实际操作中我们并不会真的这么做。想象一个包含100万客户的数据集，若为每位客户添加一个虚拟变量，最终将产生100万列，这显然不现实。取而代之的是，我们采用将线性回归拆分为两个独立模型的技巧。此前我们已有所了解，现在是时候回顾一下。假设你有一个线性回归模型，其中包含一组特征$X_1$和另一组特征$X_2$。$\hat{\beta_1}$

$
\hat{Y} = \hat{\beta_1} X_1 + \hat{\beta_2} X_2
$

其中$X_1$和$X_2$是特征矩阵（每行代表一个特征，每列代表一个观测值），$\hat{\beta_1}$和 $\hat{\beta_2}$是行向量。

你可以通过以下四个步骤得到和直接回归相同的$\hat{\beta_1}$结果：

1. 将结果变量$y$对第二组特征$X_2$进行回归：$\hat{y^*} = \hat{\gamma_1} X_2$
2. 将第一组特征$X_1$对第二组特征$X_2$进行回归：$\hat{X_1} = \hat{\gamma_2} X_2$
3. 计算残差 $\tilde{X}_1 = X_1 - \hat{X_1}$ 和 $\tilde{y}_1 = y_1 - \hat{y^*}$
4. 将结果变量的残差对特征的残差进行回归：$\hat{y} = \hat{\beta_1} \tilde{X}_1$

最后这一步回归所得到的参数，与使用所有特征一次性进行回归所得到的结果是完全一致的。但这对我们到底有什么帮助呢？实际上，我们可以把包含实体虚拟变量（entity dummies）的模型估计过程拆分为两步：第一步，我们利用虚拟变量分别去预测结果变量和解释变量——这正是上面步骤1和步骤2所做的事情。

现在回忆一下：在虚拟变量上运行回归，其实就等同于计算该组的平均值，你还记得吗？如果不记得，没关系，我们可以用数据来说明这一点。我们来运行一个模型，用年份的虚拟变量来预测工资，看看它是如何反映每一年的平均工资的。


```{dropdown} 查看 Stata 代码
```stata
reg ln_wage i.year

```python
mod = smf.ols("lwage ~ C(year)", data=data).fit()
mod.summary().tables[1]
```

注意此模型预测1980年的平均收入为1.3935，1981年为1.5129（即1.3935+0.1194），依此类推。若按年度计算平均值，我们将得到完全相同的结果。（需注意基准年1980对应截距项，因此需将截距与其他年份参数相加才能得到该年度的均值`lwage`）。

```{dropdown} 查看 Stata 代码
```stata
tabstat ln_wage, by(year) stat(mean)

```python
data.groupby("year")["lwage"].mean()
```

这意味着如果我们计算面板数据中每个个体的平均值，实质上是在将个体虚拟变量对其他变量进行回归。这引出了以下估计步骤：

1. 通过减去个体均值来创建时间去均值变量：
$\ddot{Y}_{it} = Y_{it} -  \bar{Y}_i$  
$\ddot{X}_{it} = X_{it} -  \bar{X}_i$

2. 将 $\ddot{Y}_{it}$ 对 $\ddot{X}_{it}$ 进行回归


注意，这样处理后，不可观测的个体效应 $U_i$ 消失了。由于 $U_i$ 在时间上是恒定的，所以 $\bar{U_i}{=U}_i$ 。如果我们有以下两个方程组成的系统：

$$
\begin{align}
Y_{it} & = \beta X_{it} + \gamma U_i + e_{it} \\
\bar{Y}_{i} & = \beta \bar{X}_{it} + \gamma \bar{U}_i + \bar{e}_{it} \\
\end{align}
$$

然后两式相减，得到：

$$
\begin{align}
(Y_{it} - \bar{Y}_{i}) & = (\beta X_{it} - \beta \bar{X}_{it}) + (\gamma U_i - \gamma U_i) + (e_{it}-\bar{e}_{it}) \\
(Y_{it} - \bar{Y}_{i}) & = \beta(X_{it} - \bar{X}_{it}) + (e_{it}-\bar{e}_{it}) \\
\ddot{Y}_{it} & = \beta \ddot{X}_{it} + \ddot{e}_{it} \\
\end{align}
$$

通过这个过程，所有在时间上恒定的未观测变量都会被“抵消”掉。实际上，不仅未观测变量会消失，所有随时间不变的变量都会如此。因此，你不能包含任何跨时间恒定的变量，因为它们将成为虚拟变量的线性组合，导致模型无法运行。

![img](./images/14/demeaned.png)

为了检查哪些变量在时间上是恒定的，我们可以按个体对数据进行分组，并计算每个变量的标准差之和。若结果为零，则意味着该变量对所有个体而言均不随时间变化。、

```{dropdown} 查看 Stata 代码
```stata
collapse (sum) sd_lwage=lwage sd_educ=educ sd_exper=exper
list

```python
data.groupby("nr").std().sum()
```

在处理我们的数据时，需剔除种族虚拟变量 ``black`` 和 ``hisp`` ，因为它们对个体而言是恒定的。同时，还需移除教育程度变量。此外，我们也将不使用职业信息，因为这可能中介了婚姻对工资的影响（例如，单身男性或许能承担时间要求更高的职位）。选定特征后，即可开始模型估计。

为了运行固定效应模型，首先需获取均值数据。这可通过按个体分组并计算均值实现。


```python
Y = "lwage"
T = "married"
X = [T, "expersq", "union", "hours"]

mean_data = data.groupby("nr")[X+[Y]].mean()
mean_data.head()
```

为了对数据进行去均值处理，我们需要将原始数据的索引设置为个体标识符 nr 。然后，只需从均值数据框中减去原始数据框即可。

```python
demeaned_data = (data
               .set_index("nr") # set the index as the person indicator
               [X+[Y]]
               - mean_data) # subtract the mean data

demeaned_data.head()
```

最后，我们可以在去均值后的数据上运行固定效应模型。

```python
mod = smf.ols(f"{Y} ~ {'+'.join(X)}", data=demeaned_data).fit()
mod.summary().tables[1]
```

如果我们认为固定效应消除了所有遗漏变量偏误，那么该模型告诉我们，婚姻会使男性的工资增加11%。而且这个结果具有高度统计显著性。这里的一个细节是，对于固定效应模型，标准误差需要进行聚类处理。因此，与其手动完成所有估计（这仅适用于教学目的），我们可以使用库 ``linearmodels`` 并将参数 ``cluster_entity`` 设置为 True。

```python
from linearmodels.panel import PanelOLS
mod = PanelOLS.from_formula("lwage ~ expersq+union+married+hours+EntityEffects",
                            data=data.set_index(["nr", "year"]))

result = mod.fit(cov_type='clustered', cluster_entity=True)
result.summary.tables[1]
```

注意，参数估计值与我们在去均值数据上得到的结果完全相同。唯一的区别是标准误差略微变大了一些。现在，将此与不考虑数据时间结构的简单OLS模型进行比较。对于这个模型，我们重新加入了那些在时间上恒定的变量。

```python
mod = smf.ols("lwage ~ expersq+union+married+hours+black+hisp+educ", data=data).fit()
mod.summary().tables[1]
```

该模型指出，婚姻会使男性的工资增长14%。这一效应比我们在固定效应模型中发现的结果略大。这表明由于未将固定个体因素（如外貌与智力）纳入模型，存在一定的遗漏变量偏误。

## 固定效应可视化

为了进一步加深对固定效应模型运作原理的理解，我们暂且转向另一个例子。假设你在一家大型科技公司工作，想要评估广告牌营销活动对应用内购买的影响。回顾历史数据时，你会发现市场部门倾向于在购买水平较低的城市增加广告牌投放预算。这很合理，对吧？如果销售额已经飙升，自然无需大量广告投入。若对此数据运行回归模型，表面上看似更高的营销成本导致了更少的应用内购买，但这仅仅是因为营销投资偏向了低消费区域。

```python
toy_panel = pd.DataFrame({
    "mkt_costs":[5,4,3.5,3, 10,9.5,9,8, 4,3,2,1, 8,7,6,4],
    "purchase":[12,9,7.5,7, 9,7,6.5,5, 15,14.5,14,13, 11,9.5,8,5],
    "city":["C0","C0","C0","C0", "C2","C2","C2","C2", "C1","C1","C1","C1", "C3","C3","C3","C3"]
})

m = smf.ols("purchase ~ mkt_costs", data=toy_panel).fit()

plt.scatter(toy_panel.mkt_costs, toy_panel.purchase)
plt.plot(toy_panel.mkt_costs, m.fittedvalues, c="C5", label="Regression Line")
plt.xlabel("Marketing Costs (in 1000)")
plt.ylabel("In-app Purchase (in 1000)")
plt.title("Simple OLS Model")
plt.legend()
plt.show();
```

鉴于你在因果推断方面的丰富知识，你决定运行一个固定效应模型，将城市指标作为虚拟变量加入模型中。该固定效应模型控制了那些随时间不变的城市特定特征，因此如果某座城市对你的产品接受度较低，模型将捕捉到这一信息。当您运行该模型后，最终能够观察到更高的营销成本确实带来了应用内购买收入的提升。

```python
fe = smf.ols("purchase ~ mkt_costs + C(city)", data=toy_panel).fit()

fe_toy = toy_panel.assign(y_hat = fe.fittedvalues)

plt.scatter(toy_panel.mkt_costs, toy_panel.purchase, c=toy_panel.city)
for city in fe_toy["city"].unique():
    plot_df = fe_toy.query(f"city=='{city}'")
    plt.plot(plot_df.mkt_costs, plot_df.y_hat, c="C5")

plt.title("Fixed Effect Model")
plt.xlabel("Marketing Costs (in 1000)")
plt.ylabel("In-app Purchase (in 1000)")
plt.show();
```

花点时间理解上图向你展示的固定效应作用。你会发现，固定效应为每个城市拟合了一条回归线；而且这些线是平行的。线的斜率代表营销成本对应用内购买的影响。而固定效应模型假设因果效应在所有实体（本例中为城市）间是恒定的。这可以是一个优势，也可以是一个局限，取决于你的研究目的：

 - 如果你的目标是了解每个城市的因果效应是否不同，那么这就是一种限制，因为固定效应模型默认效应在城市之间不变，所以你无法识别这种差异；
 - 但如果你的目标是估计营销对整体应用内购买的平均影响，那么这种面板数据结构正是固定效应模型最能发挥作用的场景。

换句话说，固定效应模型牺牲了异质性（城市间效应的不同），换来了对个体固定特征的有效控制，这在因果推断中是非常有力的工具。


## 时间效应

正如我们在个体层面使用了固定效应一样，我们也可以对时间层面引入固定效应。

若为每个个体添加虚拟变量能控制个体固定特征，那么为时间添加虚拟变量则可控制那些在单个时间段内固定但可能随时间变化的变量。一个典型的例子就是通货膨胀。价格和工资往往随时间上涨，但每个时间段的通胀率对所有实体都是相同的。举个更具体的例子，假设结婚率随时间上升。若工资与结婚比例也随时间变化，时间就会成为混杂因素。由于通货膨胀同样导致工资随时间增长，我们观察到的婚姻与工资之间的正相关部分可能仅源于二者均随时间递增。为校正这一点，我们可为每个时间点添加虚拟变量。在 ``linear models`` 中，这只需将 ``TimeEffects`` 加入公式并将 ``cluster_time = True`` 即可实现。


```python
mod = PanelOLS.from_formula("lwage ~ expersq+union+married+hours+EntityEffects+TimeEffects",
                            data=data.set_index(["nr", "year"]))

result = mod.fit(cov_type='clustered', cluster_entity=True, cluster_time=True)
result.summary.tables[1]
```

在这个新模型中，婚姻对工资的影响从 ``0.1147`` 显著下降至 ``0.0476`` 。尽管如此，该结果在99%置信水平上仍具有统计显著性，因此男性仍可预期通过婚姻获得收入增长。

## 当面板数据无能为力时

使用面板数据和固定效应模型是进行因果推断的极其强大的工具。当你既没有随机数据也没有良好的工具变量时，固定效应对于非实验数据的因果推断来说是最具说服力的方法。然而，值得一提的是，它并非万能药。在某些情况下，即便是面板数据也无济于事。

最显而易见的情况是存在随时间变化的混淆变量。固定效应仅能消除个体属性中恒定的偏误。例如，假设通过阅读书籍和摄入大量优质脂肪可以提升智力水平，进而获得更高薪的工作并娶到妻子。由于此例中智力水平随时间变化，固定效应无法消除这种由未测量的智力混杂因素引起的偏误。

![img](./images/14/time-travel.png)

固定效应失效的另一种较不明显情形是存在反向因果关系。例如，假设并非婚姻导致收入增加，而是更高的收入提升了结婚概率。这种情况下二者会呈现正相关关系，但收入增长实为先行因素。二者随时间同向变动，因此固定效应无法对此进行控制。

## 核心要点

在本节中，我们探讨了如何使用面板数据——即对同一批个体在多个时间点进行多次测量的数据。当拥有此类数据时，可以使用固定效应模型，通过控制个体，将所有在时间上不变的个体特征保持固定。这种方法在控制混杂因素方面既强大又极具说服力，对于非随机数据而言已是最佳选择。

但最后我们也强调了：固定效应并不是万能的。它存在两种失效情形：一是存在反向因果关系时，二是未测量的混杂因素随时间变化时。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 15 - 合成控制法

## 揭示不可知之物的数学技巧

当我们研究双重差分法时，我们掌握了来自两个不同城市——POA和FL——多位客户的数据。这些数据跨越了两个时间段：在阿雷格里港实施营销干预以提升客户存款之前和之后。为了估计处理效应，我们进行了回归分析，得出了双重差分估计量及其标准误。

在那个案例中，由于数据是细分的，我们拥有大量样本。但如果我们手头只有城市层面的汇总数据呢？例如，假设我们仅掌握干预前后两个城市的平均存款水平。

|city|before|after|
|--|--|--|
|FL|171.64|206.16|
|POA|46.01|87.06|

我们仍能够计算双重差分估计量

$
(E[Y(1)|D=1] - E[Y(1)|D=0]) - (E[Y(0)|D=1] - E[Y(0)|D=0]) = (87.06 - 206.16) - (46.01 - 171.64) = 6.53
$

然而，需要注意的是，此处的样本量仅为 4，而这恰好也是我们双重差分模型中的参数数量。在这种情况下，标准误差的定义并不明确，那么我们该如何应对？另一个问题是，FL可能与我们所期望的与POA的相似度有所差距。例如，FL以其美丽的海滩和悠闲的居民而闻名，而POA则更因其烧烤文化和草原风光著称。这里的核心难题在于，我们永远无法完全确定所采用的控制组是否恰当。

为解决这一问题，我们将采用被誉为[**"过去几年政策评估文献中最重要创新"**](https://www.aeaweb.org/articles?id=10.1257/jep.31.2.3)的合成控制法（Synthetic Control）。 它基于一个简单却强大的思想：我们不需要在未处理单元中找到一个与处理单元极为相似的单一单位。相反，可以通过组合多个未处理单元来构建一个虚拟的合成控制单元。合成控制法因其卓越效果与直观逻辑，甚至得以在非学术期刊[《华盛顿邮报》](https://www.washingtonpost.com/news/wonk/wp/2015/10/30/how-to-measure-things-in-a-world-of-competing-claims/)上发表专题文章。

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from matplotlib import style
from matplotlib import pyplot as plt
import seaborn as sns
import statsmodels.formula.api as smf

%matplotlib inline

pd.set_option("display.max_columns", 6)
style.use("fivethirtyeight")
```

为了展示合成控制法的实际应用，我们来看一个经典问题：评估香烟税收对香烟消费的影响。

这个问题在经济学界争论已久。一方认为，提高香烟税会增加购买成本，从而抑制需求；另一方则指出，香烟具有成瘾性，即使价格上涨，需求也不会显著减少。用经济学术语说，后者认为香烟的价格弹性很低，加税只是政府增加财政收入的一种手段，其代价是由吸烟者承担。

为了弄清楚这个问题，我们将分析一组关于美国香烟消费的历史数据。

1988 年，加利福尼亚州通过了一项著名的《烟草税与健康保护法案》，也被称为 [Proposition 99](https://en.wikipedia.org/wiki/1988_California_Proposition_99)（第 99 号提案）。其主要内容是：对每包香烟征收 25 美分的州消费税，同时对雪茄、嚼烟等其他烟草产品的零售销售也施加了大致相当的税负。此外，法案还规定了多项限制措施，包括：禁止在青少年可进入的公共区域内设置香烟自动售货机；禁止单支香烟的零售销售。该法案所产生的税收专门用于多个环保和医疗项目，并资助反烟草广告活动。

为评估其效果，我们可以收集多个州多年间的香烟销售数据。在本案例中，我们获取了 1970 年至 2000 年间来自 39 个州的数据。其余那些在同期推行了类似烟草控制政策的州则被排除在分析之外。以下是我们的数据概况：

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/smoking.csv", clear

list in 1/5


```python
cigar = (pd.read_csv("data/smoking.csv")
         .drop(columns=["lnincome","beer", "age15to24"]))

cigar.query("california").head()
```

我们以 `state` 作为州索引，其中加利福尼亚州编号为 3。我们的协变量包括 `retprice` （香烟零售价格）和 `cigsale` （人均香烟销售量，以包计）。我们关注的结果变量是 `cigsale` 。最后，我们还设置了两个布尔变量作为辅助来标识加利福尼亚州及干预后时期。若绘制各州随时间变化的香烟销售情况，将得到如下图表。

```{dropdown} 查看 Stata 代码
```stata
* Create California/Other States indicator
gen state_group = "Other States"
replace state_group = "California" if california == "True"

encode state_group, gen(state_group1)  // Creates numeric version with value labels

* Calculate mean cigsale by year and state group
collapse (mean) cigsale, by(year state_group1)

* Reshape for plotting
reshape wide cigsale, i(year) j(state_group1)

twoway ///
    (connected cigsale1 year, lwidth(2) lcolor(blue) mcolor(blue)) /// 
    (connected cigsale2 year, lwidth(2) lcolor(red) mcolor(red)) ///
    (function y = 0, range(1988 1989) lcolor(black) lpattern(dash) lwidth(2)), ///
    title("Gap in per-capita cigarette sales (in packs)") ///
    ytitle("Cigarette Sales Trend") ///
    xline(1988.5,lp(dash) lwidth(2)) xtitle("Year") ///
    legend(order(1 "California" 2 "Other States" 3 "Proposition 99") position(6) rows(1)) ///
    graphregion(color(white)) plotregion(color(white)) ///
    xsize(10) ysize(5) ///
    ylabel(40(20)140)

```python
ax = plt.subplot(1, 1, 1)

(cigar
 .assign(california = np.where(cigar["california"], "California", "Other States"))
 .groupby(["year", "california"])
 ["cigsale"]
 .mean()
 .reset_index()
 .pivot(index="year", columns="california", values="cigsale")
 .plot(ax=ax, figsize=(10,5)))

plt.vlines(x=1988, ymin=40, ymax=140, linestyle=":", lw=2, label="Proposition 99")
plt.ylabel("Cigarette Sales Trend")
plt.title("Gap in per-capita cigarette sales (in packs)")
plt.legend()
plt.show();  
```

在现有数据覆盖的时间段内，加利福尼亚州的香烟购买量明显低于全国平均水平。此外，80 年代后香烟消费似乎呈现下降趋势。从图表观察推测，相比其他州，99 号提案通过后加利福尼亚州的下降趋势有所加速，但这一结论尚不确定，仅为基于图表观察的初步推断。

为了回答第 99 号提案是否对香烟消费产生影响的问题，我们将利用干预前的时期构建一个合成对照组。我们将**通过组合其他州来创建一个模拟州，其趋势与加利福尼亚州高度相似**。随后，我们将观察这一合成对照组在干预后的表现。

## 数据不够？让时间补上

为了使表述更为正式，假设我们拥有 $J+1$ 个单位，在不失一般性的情况下，设单位 1 为受干预影响的单位。单位 $j=2,...,J+1$ 构成一组未经处理的单位，我们称之为“控制组（donor pool）”。同时假设数据覆盖 T 个时间段，其中 $T_0$ 个为干预前的时期。对于每个单位 j 和每个时间点 t，我们观测到结果 $Y_{jt}$。对于每个单位 j 和时期 t，定义 $Y^N_{jt}$ 为无干预时的潜在结果，$Y^I_{jt}$ 为有干预时的潜在结果。那么，在时间 t（$t>T_0$）对受处理单位 $j=1$ 的效应定义为：

$
\tau_{1t} = Y^I_{1t} - Y^N_{1t}
$

由于单位 $j=1$ 是接受处理的那个， $Y^I_{1t}$ 是事实性的，但 $Y^N_{1t}$ 则不是。接下来的挑战在于如何估计 $Y^N_{1t}$。注意到处理效应是随时间定义的，这意味着它可以随时间变化，不必是即时的，可以累积或消散。形象地说，估计处理效应的问题归根结底是**估计单位 $j=1$ 如果未被处理，其结果将会如何的问题**。

![img](images/15/synth_img.png)

为了估计 $Y^N_{1t}$，我们记得控制组中单位的组合可能比任何单独未处理单位更能近似处理单位的特征。因此，合成控制被定义为控制组中单位的加权平均。给定权重 $\pmb{W}=(w_2, ..., w_{J+1})$ ，$Y^N_{1t}$ 的合成控制估计值为

$
\hat{Y}^N_{1t} = \sum^{J+1}_{j=2} w_j Y_{jt}
$

如果这些数学让你头疼，你并不孤单。但别担心，我们有很多例子可以让它更直观。这一次，我喜欢将合成控制视为回归的一种逆向思维方式。众所周知，线性回归也是通过变量的加权平均来获得预测的一种方法。现在，想想那些回归，就像在双重差分法例子中那样，每个变量都是一个时间段的虚拟变量。在这种情况下，回归可以表示为以下矩阵乘法：

![img](images/15/regr_time.png)

在合成控制的情形下，我们不是拥有很多单位（个体），而是拥有很多时间点。因此，我们的做法是将输入矩阵“翻转”过来。这样一来，各个“单位”就成了回归中的“变量”，而结果变量则表示为这些单位的加权平均，如下面的矩阵乘法所示：

![img](images/15/regr_space.png)

如果每个时间点有多个特征变量，我们可以像这样把这些特征堆叠起来。关键在于，使这个“回归”过程尝试用其他单位的数据来“预测”第 1 个被处理单位，从而以某种最优方式选择权重以达到所需的近似效果。我们甚至可以对特征进行不同比例的缩放，以赋予它们不同的重要性。

![img](images/15/regr_space_x.png)

那么，既然合成控制法可视为线性回归，是否意味着我们也能用普通最小二乘法（OLS）估计其权重呢？没错！实际上，我们现在就来实践这一点。


## 将合成控制法视作线性回归

![img](images/15/allways.png)

为用合成控制法估计处理效应，我们将尝试构建一个在干预前与处理单元相似的“虚拟单元”，然后观察该“虚拟单元”在干预后的表现。合成控制与其模拟单元之间的差异即为处理效应。

为此，我们将使用普通最小二乘法（OLS）确定权重，并最小化干预前时期控制组中单元加权平均值与处理单元之间的平方距离。

为此，首先需要将单元（本案例中指各州）转换为列，时间转换为行。由于我们有两个特征变量 `cigsale` 和 `retprice` ，我们将如上图所示将它们垂直堆叠。我们将构建一个在干预前时期与加利福尼亚州高度相似的合成控制组，观察其在干预后时期的表现。因此，仅选择干预前时期数据至关重要。此处各特征量纲相近，故无需处理。若特征量纲差异显著（如一个为千位数级，另一个为小数级），较大特征在差异最小化过程中将占据主导地位。为避免此问题，需先进行特征缩放。

```{dropdown} 查看 Stata 代码
```stata
* ==============================================================================
* SYNTHETIC CONTROL METHOD VIA LINEAR REGRESSION
* Objective: Find optimal weights for control units to match treated unit (California)
*            in the pre-treatment period using cigsale and retprice
* ==============================================================================

* Setup panel data structure
xtset state year  // Declare panel structure with state and year

* ------------------------------------------------------------------------------
* STEP 1: DATA PREPARATION
* ------------------------------------------------------------------------------
* Keep only pre-treatment period (before Proposition 99)
keep if after_treatment == "False"

* ------------------------------------------------------------------------------
* STEP 2: EXTRACT TREATED UNIT DATA (CALIFORNIA)
* ------------------------------------------------------------------------------
preserve
keep if state == 3  // California is state=3 (adjust if different)
mkmat cigsale retprice, matrix(Y_cal)  // Store outcomes as matrix
restore

* ------------------------------------------------------------------------------
* STEP 3: PREPARE CONTROL UNITS DATA (OTHER STATES)
* ------------------------------------------------------------------------------
* Drop California to create donor pool
drop if state == 3  

keep cigsale retprice year state
* Reshape control data to wide format (one column per state)
reshape wide cigsale retprice, i(year) j(state)

* ------------------------------------------------------------------------------
* STEP 4: LINEAR REGRESSION TO ESTIMATE WEIGHTS
* ------------------------------------------------------------------------------
/* 
Regression specification:
Y_cal = β1*Control1_cigsale + β2*Control2_cigsale + ... 
       + γ1*Control1_retprice + γ2*Control2_retprice + ...
*/
matrix Y = Y_cal'  // Transpose for conformability
mkmat cigsale* retprice*, matrix(X_controls)  // Predictor matrix

* Unconstrained OLS (may produce negative weights)
matrix XX = X_controls' * X_controls
matrix XY = X_controls' * Y'
matrix beta = invsym(XX) * XY 
matrix beta = beta'          
matrix list beta  // Display estimated weights

restore

* ------------------------------------------------------------------------------
* STEP 5: CREATE SYNTHETIC CONTROL AND VISUALIZE
* ------------------------------------------------------------------------------
* ReLoad data (assuming CSV is in current directory)
import delimited "./data/smoking.csv", clear

preserve
keep if state == 3  // California is state=3 (adjust if different)
mkmat cigsale retprice, matrix(Y_cal)  // Store outcomes as matrix
restore

* Drop California to create donor pool
drop if state == 3  

keep cigsale retprice year state
* Reshape control data to wide format (one column per state)
reshape wide cigsale retprice, i(year) j(state)

mkmat cigsale* retprice*, matrix(X_controls)  // Predictor matrix

*Calculate synthetic control using unconstrained weights
matrix Y_synth = X_controls * beta'

svmat Y_cal, names(cal_)  // Creates cal_1 (cigsale) and cal_2 (retprice)
svmat Y_synth, names(synth_)

* Plot comparison
twoway ///
    (line cal_1 year, lcolor(blue) lwidth(2)) ///
    (line synth_1 year, lcolor(red) lpattern(dash) lwidth(2)), ///
    legend(order(1 "California" 2 "Synthetic Control")) ///
    title("Cigarette Sales: California vs. Synthetic Control") ///
    xtitle("Year") ytitle("Cigarette Sales")

```python
features = ["cigsale", "retprice"]

inverted = (cigar.query("~after_treatment") # filter pre-intervention period
            .pivot(index='state', columns="year")[features] # make one column per year and one row per state
            .T) # flip the table to have one column per state

inverted.head()
```

现在，我们可以将加利福尼亚州定义为 Y 变量，其他州定义为 X 变量。

```python
y = inverted[3].values # state of california
X = inverted.drop(columns=3).values  # other states
```

接着，我们进行回归分析。包含截距项相当于添加了一个所有行值均为 1 的额外状态。虽然可以这样做，但我觉得这样会使问题复杂化，因此决定省略这一步骤。回归分析将返回一组权重，这些权重能最小化处理单元与供体池中单元之间的平方差异。

```python
from sklearn.linear_model import LinearRegression
weights_lr = LinearRegression(fit_intercept=False).fit(X, y).coef_
weights_lr.round(3)
```

这些权重向我们展示了如何构建合成控制组。我们将州 1 的结果乘以-0.436，州 2 的结果乘以-1.038，州 4 的结果乘以 0.679，以此类推。通过供体池中各州构成的矩阵与权重点积运算，即可实现这一目标。

```python
calif_synth_lr = (cigar.query("~california")
                  .pivot(index='year', columns="state")["cigsale"]
                  .values.dot(weights_lr))
```

现在我们已经得到了合成控制组，可以将其与加利福尼亚州的结果变量一同绘制成图。

```python
plt.figure(figsize=(10,6))
plt.plot(cigar.query("california")["year"], cigar.query("california")["cigsale"], label="California")
plt.plot(cigar.query("california")["year"], calif_synth_lr, label="Synthetic Control")
plt.vlines(x=1988, ymin=40, ymax=140, linestyle=":", lw=2, label="Proposition 99")
plt.ylabel("Gap in per-capita cigarette sales (in packs)")
plt.legend()
plt.show();
```

好的…似乎有些不对劲。这张图中首先吸引你注意的是什么？首先，干预后，合成控制组的香烟销量超过了加利福尼亚州。这表明干预措施在降低香烟需求方面是成功的。其次，注意到干预前期的拟合效果近乎完美。合成控制组能够精确匹配加利福尼亚州的数据。这预示着我们的合成控制模型可能存在对数据的过度拟合。另一个迹象是干预后合成控制组结果变量的巨大波动。观察其走势并非平稳，而是呈现出上下起伏的波动模式。

![img](images/15/out-of-sample.png)

若我们思考为何会出现这种情况，需记得我们的控制组中有 38 个州。因此，线性回归模型拥有 38 个参数可供调整，以期预处理池尽可能匹配处理组。此情形下，即便时间维度 T 较大，样本量 N 同样庞大，这赋予了线性回归模型过高的灵活性。若您熟悉正则化模型，可知可采用[岭回归](https://zh.wikipedia.org/wiki/岭回归)或 [Lasso回归](https://zh.wikipedia.org/wiki/Lasso算法)来修正此问题。此处，我们将探讨另一种更为传统的方法以避免过拟合。

## 切勿超出数据支持的范围

假设您拥有如下表所示的数据，并被要求构建一个合成控制组，通过控制单元的任何线性组合来复现处理单元。

|unit|sales|price|
|--|--|--|
|control 1|8|8|
|control 2|8|4|
|control 3|4|5|
|treated  |2|10|

由于存在 3 个单元但仅需匹配 2 个属性，此问题存在多种精确解，但一种巧妙的解法是将第一个对照组乘以 2.25，第二个乘以-2 后相加。注意第二个乘法操作会生成一个销售为-16、价格为-8 的虚拟单元。这种乘法运算将对照组 2 单元外推至数据中不太合理的区域，因为负价格和负销售额几乎不可能存在。第一个乘法同样属于外推，它将首个单元带至销售额和价格均为 18 的区域。这些数值远高于我们数据中的任何记录，因此构成了外推。

这就是当我们要求回归分析创建合成控制组时其背后的运作机制。从技术上讲外推并无错误，但在实践中具有风险。我们实际上是在假设未见数据的行为模式与现有数据相同。

一种更安全的做法是将合成控制限制为仅进行插值。为此，我们将约束权重为正且总和为一。此时，合成控制将成为供体池中单元的凸组合。在进行插值时，我们会将处理单元投影到由未处理单元定义的凸包内，如下图所示。

![img](images/15/extrapolation.png)

这里需要注意两点。首先，在此情况下，插值无法完美匹配处理单元。这是因为该处理单元具有最低的销售量和最高的价格。凸组合只能精确复制处于控制单元之间的特征。另一点值得注意的是插值具有稀疏性。我们会将处理单元投影到凸包的某个面上，而这个面仅由少数几个单元定义。因此，插值会给许多单元分配零权重。

这是基本思路，现在让我们稍作形式化。合成控制仍定义为 

$
\hat{Y}^N_{jt} = \sum^{J+1}_{j=2} w_j Y_{jt}
$

但现在，我们将使用最小化以下表达式的权重 $\pmb{W}=(w_2, ..., w_{J+1})$ 

$
||\pmb{X}_1 - \pmb{X}_0 \pmb{W}|| = \bigg(\sum^k_{h=1}v_h \bigg(X_{h1} - \sum^{J+1}_{j=2} w_j X_{hj} \bigg)^2 \bigg)^{\frac{1}{2}}
$

受限于 $w_2, ..., w_{J+1}$ 为正且总和为一的限制条件。注意，$v_h$ 反映了在最小化处理组与合成控制组差异时各变量的重要性。不同的 $v$ 会得出不同的最优权重。选择 $V$ 的一种方法是使每个变量均值为零且具有单位方差。更复杂的方式是根据变量对预测 $Y$ 能力的贡献赋予其相应的重要性。为保持代码简洁，我们将统一赋予各变量相同的重要性。

为实现此目标，首先需定义上述损失函数。

```python
from typing import List
from operator import add
from toolz import reduce, partial

def loss_w(W, X, y) -> float:
    return np.sqrt(np.mean((y - X.dot(W))**2))
```

由于我们对所有特征采用相同的重要性权重，因此无需考虑 $v$。

现在，为获取最优权重，我们将利用 scipy 的二次规划优化方法，并通过约束条件确保权重总和为 1。

```python 
lambda x: np.sum(x) - 1
```

此外，我们将优化边界设定在 0 到 1 之间。

```python
from scipy.optimize import fmin_slsqp

def get_w(X, y):
    
    w_start = [1/X.shape[1]]*X.shape[1]

    weights = fmin_slsqp(partial(loss_w, X=X, y=y),
                         np.array(w_start),
                         f_eqcons=lambda x: np.sum(x) - 1,
                         bounds=[(0.0, 1.0)]*len(w_start),
                         disp=False)
    return weights
```

实现这一点后，我们将获取定义合成控制的权重。

```python
calif_weights = get_w(X, y)
print("Sum:", calif_weights.sum())
np.round(calif_weights, 4)
```

利用这一权重，我们将状态 1、2 和 3 乘以零，状态 4 乘以 0.0852，以此类推。注意到权重是稀疏的，正如我们预测的那样。此外，所有权重之和为一且介于 0 与 1 之间，满足我们的凸组合约束条件。

现在，要得到合成控制，我们可以像之前处理回归权重那样，用这些权重乘以各状态。

```python
calif_synth = cigar.query("~california").pivot(index='year', columns="state")["cigsale"].values.dot(calif_weights)
```

若现在绘制合成控制法的结果，我们会得到一条更为平滑的趋势线。同时注意到，在干预前的时期，合成控制组不再精确复制处理组的表现。这是一个积极的信号，表明我们没有过度拟合。

```python
plt.figure(figsize=(10,6))
plt.plot(cigar.query("california")["year"], cigar.query("california")["cigsale"], label="California")
plt.plot(cigar.query("california")["year"], calif_synth, label="Synthetic Control")
plt.vlines(x=1988, ymin=40, ymax=140, linestyle=":", lw=2, label="Proposition 99")
plt.ylabel("Per-capita cigarette sales (in packs)")
plt.legend()
plt.show();
```

有了合成控制法后，我们可以通过处理组与合成控制组结果之间的差距来估计处理效应。

$
\tau_{1t} = Y^I_{jt} - Y^N_{jt}
$

在本案例中，随着时间的推移，效应呈现出逐渐扩大的趋势。

```python
plt.figure(figsize=(10,6))
plt.plot(cigar.query("california")["year"], cigar.query("california")["cigsale"] - calif_synth,
         label="California Effect")
plt.vlines(x=1988, ymin=-30, ymax=7, linestyle=":", lw=2, label="Proposition 99")
plt.hlines(y=0, xmin=1970, xmax=2000, lw=2)
plt.title("State - Synthetic Across Time")
plt.ylabel("Gap in per-capita cigarette sales (in packs)")
plt.legend()
plt.show();
```

截至 2000 年，第 99 号提案似乎使香烟销量减少了约 25 包。这一发现固然令人振奋，但你可能不禁要问：如何判断这一结果是否具有统计学显著性？

## 做出可信推论

由于我们的样本量非常小（39 个），在判断结果是否具有统计显著性而非仅由随机运气所致时，我们需要更加机智。这里，我们将运用[费希尔精确检验](https://zh.wikipedia.org/wiki/費雪正確概率檢定)的思想。其原理十分直观：我们穷尽所有可能对处理组和对照组进行排列组合。鉴于我们仅有一个处理单元，这意味着针对每个单元，我们假设其为处理组，而其他单元则作为对照组。

|iteration（迭代）|1|2|...|39|
|----|-|-|-|-|
|1|treated|0|0|0|
|2|0|treated|0|0|
|...|0|0|0|0|0|0|
|39|0|0|0|treated|

最终，我们将为每个州得到一个合成控制组及效应估计值。这一方法的原理是假设另一个非加利福尼亚州实际接受了处理，观察这一未真实发生的处理可能产生的估计效应。随后，我们将加利福尼亚州的处理效应与这些虚构处理效应进行比较，检验其是否显著更大。核心思想在于，对于那些实际未受处理的州，一旦我们假设它们接受了处理，应当无法检测到任何显著的处理效应。

为实现这一目标，我构建了一个函数，该函数以州名作为输入，计算该州的合成控制组。此函数返回一个数据框，包含州名、年份、实际结果 `cigsale` 以及该州合成结果四列数据。

```python
def synthetic_control(state: int, data: pd.DataFrame) -> np.array:
    
    features = ["cigsale", "retprice"]
    
    inverted = (data.query("~after_treatment")
                .pivot(index='state', columns="year")[features]
                .T)
    
    y = inverted[state].values # treated
    X = inverted.drop(columns=state).values # donor pool

    weights = get_w(X, y)
    synthetic = (data.query(f"~(state=={state})")
                 .pivot(index='year', columns="state")["cigsale"]
                 .values.dot(weights))

    return (data
            .query(f"state=={state}")[["state", "year", "cigsale", "after_treatment"]]
            .assign(synthetic=synthetic))
```

这是我们将其应用于初始状态时得到的结果。

```python
synthetic_control(1, cigar).head()
```

为获取所有状态的结果，我们通过 8 个进程并行计算。若您的计算机核心数不同，可调整此数值。此代码将返回如上所示的一系列数据框。

```python
from joblib import Parallel, delayed

control_pool = cigar["state"].unique()

parallel_fn = delayed(partial(synthetic_control, data=cigar))

synthetic_states = Parallel(n_jobs=8)(parallel_fn(state) for state in control_pool)
```

```python
synthetic_states[0].head()
```

通过为所有州构建合成控制组，我们能够估算各州合成数据与真实状态之间的差距。对加利福尼亚州而言，这一差距即处理效应；而对其他州来说，这类似于安慰剂效应——我们估算的是实际上并未实施处理的合成控制处理效应。若将所有安慰剂效应与加州处理效应绘制于同一图表，则得到下图所示结果。

```python
plt.figure(figsize=(12,7))
for state in synthetic_states:
    plt.plot(state["year"], state["cigsale"] - state["synthetic"], color="C5",alpha=0.4)

plt.plot(cigar.query("california")["year"], cigar.query("california")["cigsale"] - calif_synth,
        label="California");

plt.vlines(x=1988, ymin=-50, ymax=120, linestyle=":", lw=2, label="Proposition 99")
plt.hlines(y=0, xmin=1970, xmax=2000, lw=3)
plt.ylabel("Gap in per-capita cigarette sales (in packs)")
plt.title("State - Synthetic Across Time")
plt.legend()
plt.show();
```

该图表呈现两个显著特征。首先可见干预后的方差明显大于干预前，这符合预期，因为合成控制法的设计初衷正是最小化干预前时期的差异。另一有趣现象是，即便在干预前阶段，某些单元仍难以实现良好拟合。这同样可以预见，例如某些州卷烟消费量极高，其他州的任何凸组合都无法与之匹配。

鉴于这些单元的拟合效果极差，将其从分析中剔除是明智之举。客观的做法之一是为干预前误差设定阈值作为筛选标准。

$
MSE = \frac{1}{N}\sum\bigg(Y_t - \hat{Y}^{Synth}_t\bigg)^2
$

并剔除误差较大的单元。若按此步骤操作并绘制相同图表，我们将得到如下结果。

```python
def pre_treatment_error(state):
    pre_treat_error = (state.query("~after_treatment")["cigsale"] 
                       - state.query("~after_treatment")["synthetic"]) ** 2
    return pre_treat_error.mean()

plt.figure(figsize=(12,7))
for state in synthetic_states:
    
    # remove units with mean error above 80.
    if pre_treatment_error(state) < 80:
        plt.plot(state["year"], state["cigsale"] - state["synthetic"], color="C5",alpha=0.4)

plt.plot(cigar.query("california")["year"], cigar.query("california")["cigsale"] - calif_synth,
        label="California");

plt.vlines(x=1988, ymin=-50, ymax=120, linestyle=":", lw=2, label="Proposition 99")
plt.hlines(y=0, xmin=1970, xmax=2000, lw=3)
plt.ylabel("Gap in per-capita cigarette sales (in packs)")
plt.title("Distribution of Effects")
plt.title("State - Synthetic Across Time (Large Pre-Treatment Errors Removed)")
plt.legend()
plt.show();
```

消除噪声后，我们可以看出加利福尼亚州的效应值有多么极端。此图像表明，若假设处理发生在其他任何州，我们几乎不可能获得像加州那样极端的效应。

仅此图像本身即是一种推断形式，但我们还能从这些结果中推导出 P 值。只需统计我们得到的效应低于加州效应的次数即可。

```python
calif_number = 3

effects = [state.query("year==2000").iloc[0]["cigsale"] - state.query("year==2000").iloc[0]["synthetic"]
           for state in synthetic_states
           if pre_treatment_error(state) < 80] # filter out noise

calif_effect = cigar.query("california & year==2000").iloc[0]["cigsale"] - calif_synth[-1] 

print("California Treatment Effect for the Year 2000:", calif_effect)
np.array(effects)
```

若要检验"加州效应低于零"的单侧假设，我们可以将 P 值估计为加州效应大于所有估计效应的比例。

$
PV=\frac{1}{N}\sum \mathcal{1}\{\hat{\tau}_{Calif} > \hat{\tau}_j\}
$

结果表明，2000 年加利福尼亚州的处理效应为-24.8，意味着干预措施使得香烟消费量减少了近 25 包。在我们估算的所有其他 34 个安慰剂效应中，仅有一个高于在加利福尼亚州发现的效果。因此，p 值为 1/35。

```python
np.mean(np.array(effects) < calif_effect)
```

最后，我们可以展示效应分布，以便直观了解加利福尼亚州效应值的极端程度。 

```python
_, bins, _ = plt.hist(effects, bins=20, color="C5", alpha=0.5);
plt.hist([calif_effect], bins=bins, color="C0", label="California")
plt.ylabel("Frquency")
plt.title("Distribution of Effects")
plt.legend()
plt.show();
```

```{dropdown} 查看 Stata 代码
```stata
* SC
* Load data (assuming CSV is in current directory)
import delimited "./data/smoking.csv", clear

* Declare the dataset as a panel:
tsset state year

synth cigsale beer(1984(1)1988) lnincome retprice age15to24 cigsale(1988) cigsale(1980) cigsale(1975), trunit(3) trperiod(1989) fig


* Use allsynth exactly as you would use synth to reconstruct the estimate from the synth help file:
allsynth cigsale beer(1984(1)1988) lnincome retprice age15to24 cigsale(1988) cigsale(1980) cigsale(1975), trunit(3) trperiod(1989)  fig

* Use allsynth exactly as you would use synth to reconstruct the estimate from the synth help file:
allsynth cigsale beer(1984(1)1988) lnincome retprice age15to24 cigsale(1988) cigsale(1980) cigsale(1975), trunit(3) trperiod(1989)  gapfig(classic)

* Calculate, display, and save the classic RMSPE-ranked p-values from in-space placebo runs, and plot the dynamic paths of classic gaps for the treated unit and for each of the donor pool units (placebo treated units), with the dotted vertical line indicating the period immediately preceding treatment:
allsynth cigsale beer(1984(1)1988) lnincome retprice age15to24 cigsale(1988) cigsale(1980) cigsale(1975), trunit(3) trperiod(1989) gapfig(classic placebos lineback) pvalues(rmspe) keep(smokingresults) rep
	
* bias-corrected

allsynth cigsale beer(1984(1)1988) lnincome retprice age15to24 cigsale(1988) cigsale(1980) cigsale(1975), trunit(3) trperiod(1989) bcor(merge) gapfig(bcorrect placebos lineback) pvalues(rmspe) keep(smokingresults) replace

## 核心要点

我们了解到，如果仅拥有城市或州等实体的聚合层面数据，双重差分法将无法进行推断。此外，该方法还存在其他局限性，因为它需要定义一个对照单元，而单一对照单元可能不足以很好地代表处理单元的反事实情况。

为纠正这一问题，我们学会了构建一个综合对照组（控制组），通过组合多个对照单元使其更接近处理单元的特征。借助这一合成控制方法，我们得以观察在没有干预的情况下，处理单元可能发生的变化。

最后，我们探讨了如何利用费希尔精确检验（Fisher’s Exact Tests）进行合成控制的推断。具体而言，我们假设未受干预的单元实际上接受了处理，并计算了其效应。这些即为安慰剂效应：即使在没有干预的情况下也能观察到的效应。我们借此评估所估计的处理效应是否具有统计学显著性。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 16 - 断点回归设计


我们很少停下来深思，但大自然之流畅确实令人赞叹。没有萌芽，树木无从生长；瞬间移动不过是幻想，伤口愈合需时日。即便在社会领域，渐进亦是常态。一日之内无法壮大企业，积累财富需持之以恒与辛勤付出，掌握线性回归的原理亦需经年累月。通常情况下，大自然环环相扣，鲜有跳跃突变。


> 载营魄抱一，能无离乎？

\- 《道德经》，老子著。

这意味着，**当我们确实观察到跳跃和尖峰时，这些现象很可能是人为制造的、非自然的情境**。这类事件通常伴随着对常规情况的反事实假设：若发生异常现象，便能为我们提供一种视角，去探究若自然规律以不同方式运作时可能产生的后果。探索这些人造跳跃点正是断点回归设计的核心所在。

![img](images/16/smooth.png)

基本设定如下所述。设想存在一个处理变量 $T$ 及潜在结果 $Y_0$ 与 $Y_1$。处理变量 T 是关于观测运行变量 $R$ 间断函数，其表达式为

$
D_i = \mathcal{1}\{R_i>c\}
$

换言之，这表明当 $R$ 低于阈值 $c$ 时处理为零，否则为一。这意味着我们能在 $R>c$ 条件下观测到 $Y_1$，而在 $R<c$ 条件下观测到 $Y_0$。为理解这一机制，可将潜在结果视为两个无法完整观测的函数。虽然 $Y_0(R)$ 和 $Y_1(R)$ 同时存在，但我们无法同时观测。阈值如同一个切换开关，使我们得以观察其中一个函数而非两者，如下图所示：

![img](images/16/rdd.png)

断点回归法的核心思想是通过比较阈值两侧紧邻处的结果变量差异，以识别阈值处的处理效应。这种方法被称为**精确断点回归设计**，因为在阈值处接受处理的概率从 0 跃升至 1；但也可考虑**模糊断点回归设计**，其概率虽同样存在跳跃，但变化幅度较为平缓。

## 酒精正在危害你的生命吗？

一个极具现实意义的公共政策问题是：最低饮酒年龄应设定为多少？包括巴西在内的大多数国家将其定为 18 岁，而美国（大部分州）现行标准为 21 岁。那么，是美国过于谨慎而应当降低饮酒年龄下限，还是其他国家应当提高其法定饮酒年龄呢？

从[死亡率](https://www.aeaweb.org/articles?id=10.1257/app.1.1.164)的角度审视这一问题不失为一种方法（Carpenter 与 Dobkin，2009）。站在公共政策的立场上，可以主张我们应尽可能降低死亡率。若酒精消费显著推高死亡率，则不应下调最低饮酒年龄。此举符合减少酒精所致死亡的目标。

为评估酒精对死亡的影响，可利用法定饮酒年龄在自然状态下造成的断点现象。在美国，刚好未满21岁的人基本不喝酒（或喝得少得多），而刚满21岁的人则开始喝酒。这意味着在21岁这个年龄点，饮酒概率出现了跃升，而我们可以利用这一点运用断点回归设计（RDD）进行研究。

```note
你也可以认为，满21岁只是提高了饮酒的概率，因为即使在21岁之前，也有可能喝酒（尽管不合法）。从技术上讲，这使得这个案例属于“模糊回归不连续设计”（Fuzzy RD Design），我们将在本章稍后进一步探讨这一点。
```

```python
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
import numpy as np
from matplotlib import style
from matplotlib import pyplot as plt
import seaborn as sns
import statsmodels.formula.api as smf

%matplotlib inline

style.use("fivethirtyeight")
```

为此，我们可以获取按年龄分组的死亡率数据。每行数据代表某年龄段人群的平均年龄，以及全因死亡率（ `all` ）、机动车事故死亡率（ `mva` ）和自杀死亡率（ `suicide` ）的平均值。

```{dropdown} 查看 Stata 代码
```stata
* Load data (assuming CSV is in current directory)
import delimited "./data/drinking.csv", clear

list in 1/5

```python
drinking = pd.read_csv("data/drinking.csv")
drinking.head()[["agecell", "all", "mva", "suicide"]]
```

为了便于观察（以及出于我们稍后将看到的另一个重要原因），我们将运行变量 `agecell` 以阈值 21 为中心进行居中处理。

```python
drinking["agecell"] -= 21
```

如果我们在 x 轴上绘制多个结果变量（ `all` 、 `mva` 、 `suicide` ）与运行变量的关系图，可以直观地观察到在跨越法定饮酒年龄时死亡率存在某种跳跃。

```{dropdown} 查看 Stata 代码
```stata
//作散点图，观测跳跃
* 所有的死亡率
twoway scatter all agecell, xline(21,lp(dash)) saving(all)

* 交通死亡率
twoway scatter mva agecell, xline(21,lp(dash)) saving(mva)

* 自杀死亡率
twoway scatter suicide agecell,xline(21,lp(dash)) saving(suicide)

graph combine all.gph mva.gph suicide.gph,col(1)

```python
plt.figure(figsize=(8,8))
ax = plt.subplot(3,1,1)
drinking.plot.scatter(x="agecell", y="all", ax=ax)
plt.title("Death Cause by Age (Centered at 0)")

ax = plt.subplot(3,1,2, sharex=ax)
drinking.plot.scatter(x="agecell", y="mva", ax=ax)

ax = plt.subplot(3,1,3, sharex=ax)
drinking.plot.scatter(x="agecell", y="suicide", ax=ax)
plt.show()
```

虽然有一些迹象，但这还不够。在阈值处，饮酒对死亡率的确切影响是多少？而我们对这个估计的标准误是多少？

## RDD估计

RDD 方法依赖的关键假设是在阈值处潜在结果的平滑性。形式上，当运行变量从右侧和左侧趋近阈值时，潜在结果的极限应当相同。

$$
\lim_{r \to c^-} E[Y_{ti}|R_i=r] = \lim_{r \to c^+} E[Y_{ti}|R_i=r]
$$

若此条件成立，我们便能在阈值处找到因果效应

$$
\begin{align}
\lim_{r \to c^+} E[Y_{ti}|R_i=r] - \lim_{r \to c^-} E[Y_{ti}|R_i=r]=&\lim_{r \to c^+} E[Y_{1i}|R_i=r] - \lim_{r \to c^-} E[Y_{0i}|R_i=r] \\
=& E[Y_{1i}|R_i=r] - E[Y_{0i}|R_i=r] \\
=& E[Y_{1i} - Y_{0i}|R_i=r]
\end{align}
$$

从某种意义上说，这其实是一种局部平均处理效应（LATE），因为我们只能在阈值处识别这个效应。在这种设定下，我们可以将 RDD 理解为一个局部的随机试验。对于正好处在阈值附近的人来说，是否接受处理几乎像是随机的——有些人刚好低于阈值，有些人刚好高于。这就像在同一时刻，有些人刚满 21 岁，而有些人还差几天才满 21 岁，这种差别几乎是由出生日期的“偶然性”决定的。因此，RDD 提供了一个非常有说服力的因果识别方式。它虽然不是随机对照试验（RCT）那样的“黄金标准”，但已经非常接近了。

现在，为了估计阈值处的处理效应，我们只需估计上述公式中的两个极限并进行比较。最简单的方法是通过运行线性回归来实现。

![img](images/16/ols.png)

为了使其有效，我们将一个表示“是否高于阈值”的虚拟变量与运行变量进行交互。

$
y_i = \beta_0 + \beta_1 r_i + \beta_2 \mathcal{1}\{r_i>c\} + \beta_3 \mathcal{1}\{r_i>c\} r_i
$

本质上，这等同于在阈值之上和之下分别拟合一个线性回归。参数 $\beta_0$ 代表阈值以下回归的截距，而 $\beta_0+\beta_2$ 则是阈值以上回归的截距。

此处便是将运行变量在阈值处居中的技巧发挥作用的地方。经过这一预处理步骤后，阈值变为零。这使得截距 $\beta_0$ 成为阈值下方回归的预测值。换言之，$\beta_0=\lim_{r \to c^-} E[Y_{ti}|R_i=r]$ 。同理， $\beta_0+\beta_2$ 为上方结果之极限。这意味着，

$
\lim_{r \to c^+} E[Y_{ti}|R_i=r] - \lim_{r \to c^-} E[Y_{ti}|R_i=r]=\beta_2=E[ATE|R=c]
$

下面是对应的代码示例，用于估计在21岁时，饮酒对“所有原因导致的死亡”的影响。

```{dropdown} 查看 Stata 代码
```stata
// 回归法

gen d= agecell>=21

gen running=agecell-21

reg all running d i.d#c.running,r

twoway (scatter all agecell) (lfit all agecell if agecell<=21)(lfit all agecell if agecell>=21), xline(21,lp(dash)) saving(all1,replace)

twoway (scatter mva agecell) (lfit mva agecell if agecell<=21)(lfit mva agecell if agecell>=21), xline(21,lp(dash)) saving(mva1)

twoway (scatter suicide agecell) (lfit suicide agecell if agecell<=21)(lfit suicide agecell if agecell>=21), xline(21,lp(dash)) saving(suicide1)

graph combine all1.gph mva1.gph suicide1.gph,col(1)

```python
rdd_df = drinking.assign(threshold=(drinking["agecell"] > 0).astype(int))

model = smf.wls("all~agecell*threshold", rdd_df).fit()

model.summary().tables[1]
```

该模型向我们揭示，酒精消费导致死亡率上升 7.6627 个百分点。换言之，酒精使全因死亡概率增加了 8%（计算方式：100*((7.6627+93.6184)/93.6184 - 1)）。值得注意的是，该结果同时提供了因果效应估计的标准误差值。在本案例中，由于 p 值小于 0.01，该效应具有统计学显著性。

如果我们想要通过可视化来验证这个模型，可以在已有数据上展示预测值。你会发现，这就好像我们有两个回归模型：一个用于阈值以上的个体，一个用于阈值以下的个体。

```python
ax = drinking.plot.scatter(x="agecell", y="all", color="C0")
drinking.assign(predictions=model.fittedvalues).plot(x="agecell", y="predictions", ax=ax, color="C1")
plt.title("Regression Discontinuity")
plt.show()
```

如果我们对其他死亡原因也进行相同的分析，得到的结果如下。

```{dropdown} 查看 Stata 代码
```stata
// 散点拟合方法来观察跳跃

twoway (scatter all agecell, msymbol(+) msize(*0.4) mcolor(black*0.3)),   title("散点图") xline(21,lp(dash)) saving(scatter, replace)

twoway (scatter all agecell) (lfit all agecell if agecell<=21)(lfit all agecell if agecell>=21), xline(21,lp(dash)) saving(scatter1, replace)

rdplot all agecell, c(21) p(1) graph_options(title(线性拟合))
graph save rd1,replace // 线性拟合图

rdplot all agecell, c(21) p(2) graph_options(title(二次型拟合)) 
graph save rd2,replace //二次型拟合图

rdplot all agecell, c(21) p(3) graph_options(title(三次型拟合)) 
graph save rd3,replace //三次型拟合图


graph combine scatter.gph scatter1.gph rd1.gph rd2.gph rd3.gph

```python
plt.figure(figsize=(8,8))

for p, cause in enumerate(["all", "mva", "suicide"], 1):
    ax = plt.subplot(3,1,p)
    drinking.plot.scatter(x="agecell", y=cause, ax=ax)
    m = smf.wls(f"{cause}~agecell*threshold", rdd_df).fit()
    ate_pct = 100*((m.params["threshold"] + m.params["Intercept"])/m.params["Intercept"] - 1)
    drinking.assign(predictions=m.fittedvalues).plot(x="agecell", y="predictions", ax=ax, color="C1")
    plt.title(f"Impact of Alcohol on Death: {np.round(ate_pct, 2)}%")

plt.tight_layout()
plt.show()
```

RDD 分析显示，酒精会使自杀和交通事故导致的死亡风险增加 15%，这一比例相当显著。若我们的目标是尽可能降低死亡率，这些结果构成了反对降低饮酒年龄的有力论据。

### 核加权

断点回归高度依赖于线性回归的外推特性。由于我们关注的是两条回归线起点和终点的数值，必须确保这些极限值的准确性。可能出现的情况是，回归过度拟合其他数据点，导致在阈值处的拟合效果不佳。一旦发生这种情况，我们可能会得出错误的处理效应估计。

解决这一问题的一种方法是为靠近阈值的点赋予更高的权重。实现方式多种多样，其中一种常见做法是使用**三角核函数**对样本进行重新加权。

$
K(R, c, h) = \mathcal{1}\{|R-c| \leq h\} * \bigg(1-\frac{|R-c|}{h}\bigg)
$

该核函数的第一部分是一个指示函数，用于判断我们是否接近阈值。接近到什么程度？这由带宽参数 $h$ 决定。核函数的第二部分是一个加权函数。随着我们远离阈值，权重会变得越来越小。这些权重会被带宽除。如果带宽较大，权重减小的速度较慢；如果带宽较小，权重则会迅速趋近于零。

为便于理解，以下是该核函数应用于我们问题时的权重示例。此处我将带宽设为 1，意味着我们仅考虑年龄不超过 22 岁且不低于 20 岁的人群数据。

```python
def kernel(R, c, h):
    indicator = (np.abs(R-c) <= h).astype(float)
    return indicator * (1 - np.abs(R-c)/h)
```

```python
plt.plot(drinking["agecell"], kernel(drinking["agecell"], c=0, h=1))
plt.xlabel("agecell")
plt.ylabel("Weight")
plt.title("Kernel Weight by Age")
plt.show()
```

若我们将这些权重应用于原始问题，酒精的影响会变得更大，至少对所有原因而言如此。其数值从 7.6627 跃升至 9.7004，结果依然非常显著。此外，请注意我使用的是 `wls` 而非 `ols` 。

```python
model = smf.wls("all~agecell*threshold", rdd_df,
                weights=kernel(drinking["agecell"], c=0, h=1)).fit()

model.summary().tables[1]
```

```python
ax = drinking.plot.scatter(x="agecell", y="all", color="C0")
drinking.assign(predictions=model.fittedvalues).plot(x="agecell", y="predictions", ax=ax, color="C1")
plt.title("Regression Discontinuity (Local Regression)")
plt.show()
```

以下是其他死因的图示情况。请注意右侧的回归线为何呈现更负的斜率，因为它未将最右侧的点纳入考虑。

```python
plt.figure(figsize=(8,8))
weights = kernel(drinking["agecell"], c=0, h=1)

for p, cause in enumerate(["all", "mva", "suicide"], 1):
    ax = plt.subplot(3,1,p)
    drinking.plot.scatter(x="agecell", y=cause, ax=ax)
    m = smf.wls(f"{cause}~agecell*threshold", rdd_df, weights=weights).fit()
    ate_pct = 100*((m.params["threshold"] + m.params["Intercept"])/m.params["Intercept"] - 1)
    drinking.assign(predictions=m.fittedvalues).plot(x="agecell", y="predictions", ax=ax, color="C1")
    plt.title(f"Impact of Alcohol on Death: {np.round(ate_pct, 2)}%")

plt.tight_layout()
plt.show()
```

除了自杀这一项外，加入核加权后，饮酒所带来的负面影响似乎变得更大了。再次说明，如果我们的目标是降低死亡率，那么我们不应建议降低法定饮酒年龄，因为饮酒对死亡率的影响是明显存在的。

这个简单的案例涵盖了当回归断点设计完美运作时的情况。接下来，我们将探讨一些应进行的诊断方法，以评估对 RDD 的可信度，并讨论一个我们极为关注的话题：教育对收入的影响。

## 羊皮效应与模糊断点回归设计

在探讨教育对收入的影响时，经济学界存在两大主要观点。第一种广为人知的论点认为，教育能提升人力资本，进而提高生产力，最终增加收入。这一观点主张教育实质上使人变得更好。另一种观点则认为，教育仅是一种信号传递机制，它通过一系列艰难测试和学术任务来筛选人才。若能成功完成，即向市场传递出你是一名优秀员工的信号。依此看来，教育并未直接提升你的生产力，而是向市场揭示了你原有的生产力水平。关键在于文凭——持有文凭者将获得更高报酬。这种现象被称为 **“羊皮纸效应”** ，因过去文凭常印制于羊皮纸上而得名。

为了验证这一假设，[Clark and Martorell](https://faculty.smu.edu/millimet/classes/eco7321/papers/clark%20martorell%202014.pdf)采用断点回归法来衡量完成 12 年级学业对收入的影响。为此，他们需要设定一个临界变量，高于该值的学生能够毕业，而低于该值的学生则不能。他们在德克萨斯州的教育体系中找到了这样的数据。

为了在德克萨斯州毕业，学生必须通过一项考试。测试从十年级开始，学生可以多次尝试，但最终，他们在十二年级结束时面临最后一次机会考试。研究的构想是从参加这些最后一次机会考试的学生中获取数据，并将那些勉强未通过的学生与那些勉强通过的学生进行比较。这些学生将拥有非常相似的人力资本，但信号凭证不同。具体而言，那些勉强通过的学生将获得文凭。

```python
sheepskin = pd.read_csv("./data/sheepskin.csv")[["avgearnings", "minscore", "receivehsd", "n"]]
sheepskin.head()
```

再次强调，此数据按运行变量分组。它不仅包含运行变量（最低分数，已以零为中心）和结果（平均收入），还包含了该分数段获得文凭的概率及样本量（n）。例如，在分数阈值以下-30 分的单元格中，12 名学生里仅有 5 人成功取得文凭（12*0.416）。

这意味着在治疗分配上存在一定的滑移现象。一些未达到及格门槛的学生仍然设法获得了文凭。此处，回归断点呈现**模糊性**而非精确性。注意到获得文凭的概率并未在门槛处从零跃升至一，而是从约 50% 跃升至 90% 。

```python
sheepskin.plot.scatter(x="minscore", y="receivehsd", figsize=(10,5))
plt.xlabel("Test Scores Relative to Cut off")
plt.ylabel("Fraction Receiving Diplomas")
plt.title("Last-chance Exams")
plt.show()
```

我们可以将模糊断点回归视为一种不遵从行为。越过阈值本应让每个人都获得文凭，但有些学生，即那些从不接受者，并未得到它。同样，低于阈值本应阻止你获得文凭，但有些学生，即那些总能接受者，却设法无论如何都拿到了文凭。

就像我们拥有潜在结果一样，在此情境下我们也拥有潜在的处理状态。$T_1$ 代表若所有人都高于阈值时将接受的处理；$T_0$ i则是若所有人都低于阈值时将接受的处理。或许你已经注意到，我们可以将**阈值视为一个工具变量**。正如在工具变量法中，若我们简单地估计处理效应，结果会偏向于零。

![img](images/16/rdd_fuzzy.png)

即使在阈值以上，接受处理的概率仍然小于 1，这会导致我们观察到的结果低于真实的潜在结果 $Y_1$。同样地，在阈值以下，我们观察到的结果高于真实的潜在结果 $Y_0$。这使得阈值处的处理效应看起来比实际要小。为了纠正这种偏误，我们需要使用工具变量（IV）技术。

正如我们先前对潜在结果假设了平滑性，现在我们也对潜在处理做出同样的假设。此外，与工具变量（IV）方法中类似，我们需要假设单调性。若您已不记得，单调性指的是 $T_{i1}>T_{i0} \ \forall i$。这意味着从左至右跨越阈值仅会增加获得文凭的机会（或者说不存在违背者）。基于这两个假设，我们得到了局部平均处理效应（LATE）的沃尔德估计量。

$$
\dfrac{\lim_{r \to c^+} E[Y_i|R_i=r] - \lim_{r \to c^-} E[Y_i|R_i=r]}{\lim_{r \to c^+} E[T_i|R_i=r] - \lim_{r \to c^-} E[T_i|R_i=r]} = E[Y_{1i} - Y_{0i} | T_{1i} > T_{0i}, R_i=c]
$$

请注意，这里的估计在两种意义上是局部的。首先，它是局部的，因为它仅给出阈值 $c$处的处理效应。这是断点回归（RD）的局部性。其次，它是局部的，因为它仅估计了遵从者的处理效应。这是工具变量（IV）的局部性。

为了估计这一点，我们将使用两个线性回归。分子的估计可以像之前一样进行。至于分母，我们只需将结果变量替换为处理变量。但首先，我们需要进行一项合理性检验，以确保可以信任我们的断点回归设计（RDD）估计结果。

### 麦克雷利检验

可能破坏我们断点回归论证的一个情况是，人们能够操纵自己在阈值附近的位置。以文凭效应为例，如果刚好低于阈值的学生找到系统漏洞，将考试成绩略微提高，就会发生这种情况。另一个例子是，当家庭需要低于特定收入水平才能获得政府福利时，部分家庭可能会故意降低收入，刚好达到项目资格线。

在这些情况下，我们往往会观察到一种称为“聚集”的现象出现在运行变量的密度上。这意味着会有大量实体恰好位于阈值之上或之下。为了检验这一点，我们可以绘制运行变量的密度函数图，查看阈值附近是否存在尖峰。就我们的案例而言，密度由数据中的 `n` 列给出。

```python
plt.figure(figsize=(8,8))

ax = plt.subplot(2,1,1)
sheepskin.plot.bar(x="minscore", y="n", ax=ax)
plt.title("McCrary Test")
plt.ylabel("Smoothness at the Threshold")

ax = plt.subplot(2,1,2, sharex=ax)
sheepskin.replace({1877:1977, 1874:2277}).plot.bar(x="minscore", y="n", ax=ax)
plt.xlabel("Test Scores Relative to Cut off")
plt.ylabel("Spike at the Threshold")
plt.show()
```

第一幅图展示了我们数据密度的分布情况。如图所示，阈值附近并无峰值出现，这意味着不存在聚集现象。学生们并未操控自己在阈值上的位置分布。仅作说明之用，第二幅图展示了若学生能操控阈值位置时可能出现的聚集形态——我们会在略高于阈值的区间观察到密度峰值，因为会有大量学生恰好卡在这个及格线上。

在解决这个问题之后，我们可以回过头来估计羊皮纸效应。正如我之前所说，沃尔德估计量的分子可以像在锐利断点回归中那样进行估计。这里，我们将使用带宽为 15 的核函数作为权重。由于我们还拥有单元格大小，我们将核函数乘以样本量，从而得到该单元格的最终权重。

```python
sheepsking_rdd = sheepskin.assign(threshold=(sheepskin["minscore"]>0).astype(int))
model = smf.wls("avgearnings~minscore*threshold",
                sheepsking_rdd,
                weights=kernel(sheepsking_rdd["minscore"], c=0, h=15)*sheepsking_rdd["n"]).fit()

model.summary().tables[1]
```

这表明文凭的效应为-97.7571，但这一结果在统计上并不显著（P 值为 0.5）。若绘制这些结果，我们会在阈值处得到一条极为连续的线。受教育程度更高的人群确实收入更高，但在获得 12 年级文凭的节点上并未出现跳跃性变化。这一发现支持了教育通过提升个人生产力而非仅作为市场信号来增加收入的观点。换言之，不存在所谓的羊皮纸效应。

```python
ax = sheepskin.plot.scatter(x="minscore", y="avgearnings", color="C0")
sheepskin.assign(predictions=model.fittedvalues).plot(x="minscore", y="predictions", ax=ax, color="C1", figsize=(8,5))
plt.xlabel("Test Scores Relative to Cutoff")
plt.ylabel("Average Earnings")
plt.title("Last-chance Exams")
plt.show()
```

然而，正如我们从非依从性偏误机制中所知，该结果存在向零的偏误。为校正此问题，需通过第一阶段结果进行缩放并计算沃尔德估计量。遗憾的是，目前缺乏优质的 Python 实现方案，因此我们需手动完成这一过程，并采用自助法来获取标准误。

下方代码如同先前操作般运行沃尔德估计量的分子部分，同时通过将目标变量替换为处理变量 `receivehsd` 来构建分母。最后一步仅需将分子除以分母即可。

```python
def wald_rdd(data):
    weights=kernel(data["minscore"], c=0, h=15)*data["n"]
    denominator = smf.wls("receivehsd~minscore*threshold", data, weights=weights).fit()
    numerator = smf.wls("avgearnings~minscore*threshold", data, weights=weights).fit()
    return numerator.params["threshold"]/denominator.params["threshold"]
```

```python
from joblib import Parallel, delayed 

np.random.seed(45)
bootstrap_sample = 1000
ates = Parallel(n_jobs=4)(delayed(wald_rdd)(sheepsking_rdd.sample(frac=1, replace=True))
                          for _ in range(bootstrap_sample))
ates = np.array(ates)
```

利用自助法（bootstrap）生成的样本，我们可以绘制平均处理效应（ATE）的分布图，并观察其95%置信区间所在的位置。

```python
sns.distplot(ates, kde=False)
plt.vlines(np.percentile(ates, 2.5), 0, 100, linestyles="dotted")
plt.vlines(np.percentile(ates, 97.5), 0, 100, linestyles="dotted", label="95% CI")
plt.title("ATE Bootstrap Distribution")
plt.xlim([-10000, 10000])
plt.legend()
plt.show()
```

```{dropdown} 查看 Stata 代码
```stata
// 断点回归
rdplot all agecell,c(21)
* 局部线性回归
* ssc install rdrobust,replace
rdrobust all agecell,c(21) all

rdbwselect all agecell,c(21) all

rdplot all agecell,c(21) ci(95)

* 局部多项式回归

rdrobust all agecell,c(21) all p(1)

rdrobust all agecell,c(21) all p(2)

rdrobust all agecell,c(21) all p(3)

*全局多项式回归

sum agecell
local hvalueR=r(max)  
local hvalueL= abs(r(min))
 
rdrobust all agecell, c(21)  h(`hvalueL'  `hvalueR') all //自动选择阶数
rdrobust all agecell, c(21)  h(`hvalueL'  `hvalueR') all p(2) //二阶拟合
rdrobust all agecell, c(21)  h(`hvalueL'  `hvalueR') all p(3) //三阶拟合

* McCrary Test
rddensity agecell,c(21) plot bwselect(each)


// 模糊断点

import delimited "/Users/xuwenli/Library/CloudStorage/OneDrive-个人/DSGE建模及软件编程/教学大纲与讲稿/应用计量经济学讲稿/python-causality-handbook/causal-inference-for-the-brave-and-true/data/sheepskin.csv", clear

list in 1/5

twoway scatter receivehsd minscore, xline(0,lp(dash)) saving(all)

// 断点回归

rdrobust receivehsd minscore, all

rdbwselect receivehsd minscore, all

rdplot receivehsd minscore, ci(95)

rddensity minscore, all plot

如你所见，即便我们将效应按第一阶段进行缩放，其与零的统计差异仍不显著。这意味着教育并非通过简单的文凭效应增加收入，而是通过提升个人生产力来实现。

## 核心要点

我们学习了如何利用人为的不连续性来估计因果效应。其核心思想在于设定一个人为阈值，使得处理概率在此处发生跃变。例如，我们观察到年龄达到 21 岁时饮酒概率的骤增现象，借此可估算饮酒对死亡率的影响。该方法基于一个关键事实：在阈值附近，情境近乎随机实验。处于阈值边缘的个体本可能落入任一侧，最终归属实则随机。通过比较阈值两侧的个体，我们能够获得处理效应。我们还探讨了如何运用带核函数的加权线性回归实现这一分析，并自然地得到平均处理效应（ATE）的标准误。

接着，我们考察了模糊断点回归设计（fuzzy RD）中出现不遵从行为时的情况。研究发现，可以采用与工具变量（IV）法相似的思路来处理此类问题。




## 参考文献
我愿将这一系列作品视为对 Joshua Angrist、Alberto Abadie 和 Christopher Walters 杰出计量经济学课程的致敬。第一部分的大部分思想源自他们在美国经济学会授课的内容。在艰难的 2020 年，正是观看他们的课程视频让我保持了理智。

 - [Cross-Section Econometrics](https://www.aeaweb.org/conference/cont-ed/2017-webcasts)
 - [Mastering Mostly Harmless Econometrics](https://www.aeaweb.org/conference/cont-ed/2020-webcasts)

 我还想引用 Angrist 的精彩著作。它们向我展示了计量经济学（他们称之为“Metrics”）不仅极为实用，而且充满乐趣。
 - [Mostly Harmless Econometrics](https://www.mostlyharmlesseconometrics.com)
 - [Mastering ‘Metrics](https://www.masteringmetrics.com)

最后还要感谢 Miguel Hernán 和 Jamie Robins 的[《Causal Inference》](https://hsph.harvard.edu/profile/miguel-hernan/)一书。它是我在面对最棘手的因果问题时的可靠伙伴。

![img](./images/poetry.png)

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 第二部分 - 阴


---

# 17 - 预测模型入门

我们即将结束本书的第一部分。该部分涵盖了因果推断的核心内容，所涉及的技术广为人知且久经考验，经受住了时间的检验。第一部分为我们奠定了坚实的理论基础。用更专业的术语来说，第一部分着重定义了什么是因果推断，探讨了阻碍相关性转化为因果性的各种偏误，介绍了调整这些偏误的多种方法（回归分析、匹配法和倾向得分），以及经典的识别策略（工具变量法、双重差分法和断点回归设计）。简而言之，第一部分聚焦于我们用于识别平均处理效应 $E[Y_1 - Y_0]$ 的标准技术。
 
随着我们进入第二部分，内容将变得稍显不稳定。本部分将介绍因果推断文献中的最新进展、其与机器学习的关系，以及在产业界的实际应用。在这一过程中，我们在一定程度上以可操作性和经验主义为导向，适当放宽了对学术严谨性的要求。第二部分中介绍的一些方法尚缺乏坚实的理论支持，无法清晰解释其为何有效；然而，在实际应用中，它们往往展现出良好的效果。从这个角度来看，第二部分或许对希望在日常工作中运用因果推断的行业实践者更为有益，而非专注于探索世界中根本性因果关系的学术研究者。
 
第二部分的前几章将重点探讨异质性处理效应的估计。我们将从仅关注平均处理效应 $E[Y_1 - Y_0]$ 的世界，转向探究不同个体对处理如何产生差异化反应 $E[Y_1 - Y_0 | X]$ 的领域。在这个世界里，个性化至关重要。我们希望优先干预那些处理效应最显著的个体，同时避免对那些可能因干预而受到负面影响的人施加处理。从某种意义上说，我们也正在从一个关于“平均处理效应是多少”的实证性问题，转向一个规范性问题：“我们应当对谁进行处理？”
 
这正是大多数企业面临的核心问题，尽管表述略有差异：应当向哪些客户提供折扣？贷款应设定何种利率？该向此用户推荐什么商品？每位顾客应展示怎样的页面布局？这些均属于处理效应异质性问题，我们可通过第二部分介绍的工具予以解答。但在深入探讨之前，有必要先阐明机器学习对产业界的意义——这将成为后续因果推断的基础工具。


## 机器学习在行业中的应用
 
本章重点探讨**机器学习**在工业领域的常规应用方式。若您对机器学习尚不熟悉，可将本章视为机器学习速成课程。若从未接触过机器学习，我强烈建议您至少掌握基础知识，以便充分理解后续内容。但这并不意味着已有机器学习基础的读者应跳过本章，我仍认为通读本章将有所裨益。与其他机器学习资料不同，本章**不会**深入讨论决策树或神经网络等算法的细节，而是精准聚焦于**机器学习在现实世界中的应用实践**。
 
![img](./images/17/ml-meme.png)
 
首先，我想探讨的是为何我们要在一本因果推断的书中讨论机器学习？简而言之，是因为我认为理解因果关系的最佳方式之一，就是将其与机器学习带来的预测模型方法进行对比。长话短说，原因有二。其一，如果你已经读到本书此处，很可能对机器学习已有所了解。其二，即便你不熟悉，鉴于这些话题当前的热度，你可能也已对其有所耳闻。唯一的问题是，在机器学习被炒得沸沸扬扬的当下，我或许有必要让你回归现实，用非常实际的术语解释它究竟能做什么。最后，因果推断领域的最新进展大量运用了机器学习算法，这也是一个不容忽视的因素。
 
直截了当地说，机器学习是一种实现快速、自动且高质量预测的方法。这虽不涵盖其全部内涵，但可以说覆盖了其中约 90% 的核心内容。诸如计算机视觉、自动驾驶、语言翻译以及疾病诊断等诸多重要进展，主要都源自监督式机器学习领域。乍看之下，这些应用似乎并不像是“预测”任务。比如，语言翻译怎么会是一种预测呢？这正是机器学习的巧妙之处：我们可以将许多看似无关的问题转化为预测问题加以解决。以语言翻译为例，它可以被表述为一个预测问题：向模型输入一句话，模型需要“预测”出这句话在另一种语言中的等价表达。需要注意的是，这里所说的“预测”并不等同于传统意义上“预测未来”或“预见趋势”的概念。我们所指的预测，是指从一个明确的输入映射到一个起初未知但同样定义明确且可观测的输出。
 
![img](./images/17/translation.png)
 
机器学习真正做的事情，是学习一个输入到输出之间的映射函数——即便这个函数极其复杂。本质上，只要你能将一个问题表述为“输入到输出”的映射问题，那么机器学习就可能是一个合适的解决方案。以自动驾驶汽车为例，它并不是一个单一的预测问题，而是多个高度复杂的预测问题的集合：例如，从车辆前方传感器预测方向盘的转角、从车载摄像头预测刹车的力度、从 GPS 数据预测油门的压力。能够解决这些（以及更多）预测问题，正是实现自动驾驶的关键。

从更技术的角度来看，机器学习可以被理解为对期望函数（即使是非常复杂的函数）进行估计的过程：
 
$
E[Y|X]
$
 
其中 $Y$ 代表你想获取的信息（翻译后的句子、诊断结果），而 $X$ 则是已知条件（输入语句、X 光图像）。机器学习本质上就是估计该条件期望函数的一种方法。

好吧……现在你已经理解了，预测的力量可能远比我们最初设想的更强大。自动驾驶汽车和语言翻译固然令人兴奋，但除非你在 Google 或 Uber 这样的大型科技公司工作，这些场景通常距离我们的日常工作较远。为了使问题更贴近现实，我们不妨聚焦于几乎每家公司都会面临的一个问题：客户获取（即如何获得新客户）。

从客户获取的角度看，我们通常需要解决的核心问题是：如何识别那些真正有价值的客户。在这一问题中，每位客户都会带来一笔获取成本（如市场营销费用、注册引导成本、物流配送成本等），而我们希望客户在未来能为公司带来正向的现金流。举个例子，假设你是一家互联网服务提供商或燃气公司，你的典型客户可能会呈现出如下的现金流结构。
 
![img](./images/17/cashflow-1.png)
 
图中的每一个柱形代表了你与客户关系中的一个财务事件（monetary event）。例如，在获得客户的初期阶段，你需要进行市场营销投入。接着，当某人决定与你达成交易时，你可能还需要承担一定的引导成本（例如向客户说明产品如何使用）或安装成本。直到这些前期投入之后，客户才开始逐月为你带来收入。在某个时间点，客户可能需要售后支持，这会产生一定的维护费用。最后，如果客户决定终止合同，你还可能需要承担一些终止相关的额外成本。

为了评估某位客户是否带来了利润，我们可以将这些柱形图重排，形成一个所谓的“瀑布图（cascade plot）”。理想情况下，这些现金流事件的累计值应该显著高于零线，从而表明该客户总体上是盈利的。
 
![img](./images/17/cascade-1.png)
 
相反，也完全可能出现另一种情况：客户所带来的成本远高于其带来的收入。如果客户对你的产品使用频率很低，却频繁提出高维护需求，那么当我们将这些现金流事件加总起来时，最终的结果可能落在零线以下，意味着该客户整体上是亏损的。
 
![img](./images/17/cascade-2.png)
 
当然，这样的现金流结构可以更简单，也可能复杂得多，具体取决于你的业务类型。你甚至可以引入贴现率对未来现金流进行时间折现，把问题做得非常精细——但在这里，我们的重点已经很清楚了。

那么，我们能对此做些什么呢？如果你手头有大量盈利客户和非盈利客户的历史数据，你就可以训练一个机器学习模型来识别它们。通过这种方式，你可以将营销策略集中于那些更有可能带来利润的客户。或者，在合同允许的情况下，你也可以在客户产生更多成本之前选择终止合作关系。从本质上看，**你正在将一个商业决策问题表述为一个预测问题，以便用机器学习方法求解**：你希望预测或识别出哪些客户是盈利的，哪些是亏损的，从而只与前者建立联系。

```python
import pandas as pd
import numpy as np
from sklearn import ensemble
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.metrics import r2_score
import seaborn as sns
from matplotlib import pyplot as plt
from matplotlib import style
style.use("ggplot")
```

举个例子，假设你掌握了 10,000 名客户在过去 30 天内的交易数据，并且知道每位客户的获取成本 `cacq` 。这个获取成本可以是你在在线广告投放中为他们出价的金额，也可以是物流运输费用，或是你为帮助客户使用产品所进行的培训成本。

此外，为了简化问题（毕竟这是速成课，不是一学期的客户价值评估课程），我们暂且假设你对是否与客户建立合作拥有完全决策权。换句话说，即便客户想与你达成交易，你也可以选择拒绝。

在这种设定下，你的任务就是**提前识别哪些客户将会带来利润**，从而只选择与这些客户建立业务关系。

```python
transactions = pd.read_csv("data/customer_transactions.csv")
print(transactions.shape)
transactions.head()
```

现在我们需要做的，是根据这些交易数据将“好客户”和“坏客户”区分开来。为了简化问题，我们将所有交易金额与获取成本（CACQ）做总和处理。需要注意的是，这种做法掩盖了许多细节——比如，无法区分已经流失的客户和那些仅仅在两次购买之间暂时中断的客户。

接下来，我将这个总和定义为一个变量，称为 `net\_value`，并将其与客户的特征数据进行合并。由于我们的目标是在客户尚未转化之前预测其是否会带来利润，因此我们**只能使用获取之前可观察到的数据**。在本例中，这些特征包括年龄、所在地区和收入水平，保存在另一个 `CSV` 文件中。

```python
profitable = (transactions[["customer_id"]]
              .assign(net_value = transactions
                      .drop(columns="customer_id")
                      .sum(axis=1)))

customer_features = (pd.read_csv("data/customer_features.csv")
                     .merge(profitable, on="customer_id"))

customer_features.head()
```

很好！我们的任务正变得越来越具体：我们希望识别出那些 **净收益为正（`net_value > 0`）** 的盈利客户，并将其与不盈利客户区分开来。接下来我们会尝试不同的方法，看看哪种效果更好。但在此之前，我们需要快速了解一下机器学习的基本原理（如果你已经了解机器学习的工作机制，也可以跳过本节）。

## 机器学习速成课

就我们的目标而言，可以将机器学习理解为一种功能强大、适用于预测任务的工具。为了让机器学习发挥作用，你需要一组带有“标签”（也称为“真实值”）的数据。接着，你可以用这些已知标签的数据来训练机器学习模型，并将训练好的模型应用于那些标签未知的样本上，进行预测。

下图展示了机器学习的一般工作流程：
 
![img](./images/17/ml-flow.png)
 
首先，你需要一组观测值，其中的“真实值”（此处指 `net_value`）是已知的。接着，你可以利用这组数据来**估计**一个机器学习模型，使其通过特征变量（在本例中为地区、收入和年龄）预测 `net_value`。这一估计过程将产出一个预测函数，该函数可用于在尚未观测到 `net_value` 的情况下，对新样本进行预测。

在图像左侧所示的流程中，我们手中拥有一批新客户的特征数据（地区、收入、年龄），但尚未获得其 `net_value`。将这些变量输入先前估计的模型后，模型便会输出相应的 `net_value` 预测值。

如果你更倾向于技术性表述，机器学习的目标可以理解为**估计条件期望函数** $E[Y \mid X]$，其中 $Y$ 是因变量（也称为结果变量），$X$ 是特征变量。机器学习提供了一种强大的手段来构造 $\hat{E}[Y \mid X]$ 的估计，通常是通过最小化某种损失函数（loss function）或预测误差实现的。

需要注意的是，机器学习模型具有极强的灵活性，能够逼近几乎任意形式的函数。换句话说，模型可以强大到**对训练样本实现完全拟合**。因此，机器学习方法通常包含**复杂度超参数**（complexity hyperparameters），用于控制模型的灵活性与拟合程度。

如下图所示，左图展示了一个低复杂度的模型（拟合不足），中图为适中复杂度模型，右图则为高复杂度模型，可见其几乎对训练数据实现了完全拟合。
 
![img](./images/17/model-fit.png)
 
这就引出了一个问题：**在将模型应用于真实世界之前，我们如何判断它是否有效？**
一个常用的方法是：将模型的预测结果与我们已知真实值的数据进行比较。这类比较所用的指标被称为**拟合优度指标**（goodness-of-fit metrics），例如 $R^2$。

但请记住，模型可以变得足够复杂，以至于完全拟合训练数据。在这种情况下，预测值与真实值会完全吻合。然而，这种“完美拟合”是有问题的，因为它会使验证结果产生误导——模型之所以表现优异，仅仅是因为它足够复杂，而不是因为它具备良好的泛化能力。

此外，通常来说，**过于复杂的模型并不是好事**。你可能已经对这点有了直觉。回顾上图中的模型选择：你更倾向于哪个模型？是那个准确拟合所有数据点的复杂模型？大概不是。你可能更倾向于中间那个模型——它更加平滑、结构更简单，同时仍然具备良好的预测能力，即使不能完全拟合所有数据点。
 
![img](./images/17/overfitting.jpg)
 
你的直觉是正确的。如果一个模型过于复杂，它不仅会学习到数据中的系统性结构，还会“学习”到其中的随机噪声。但在真实世界中，噪声是变化的（毕竟它是随机的），所以你那个“完美”拟合训练集的模型在实际预测时往往会出现错误。在机器学习术语中，这种现象称为**过拟合（overfitting）**，即模型在训练集表现良好，但泛化能力较差。

那我们该怎么办？

一个常见的做法是：我们假装无法访问部分数据。具体做法是：将原始数据集（即我们拥有真实值的数据）划分为两个子集，一部分用于模型训练，另一部分用于验证预测效果。这种方法称为**交叉验证（cross-validation）**。
 
![img](./images/17/test.png)
 
如下图所示，在那部分模型从未见过的数据中，复杂模型的表现并不好；反而是中间复杂度的模型，预测效果更为稳健。为了选择合适的模型复杂度，我们可以估计多个不同复杂度的模型，并比较它们在未参与训练的数据上的预测表现。

**交叉验证的重要性不容忽视，值得我们花更多时间深入理解。**


## 交叉验证
 
交叉验证（cross validation）在选择模型复杂度时非常重要，但它的用途远不止于此。事实上，只要我们希望在模型实际应用前尝试多种方案，并评估它们在现实中的表现，都可以使用交叉验证。

交叉验证的基本思想是模拟现实场景：我们在已有数据上估计模型，但模型真正应用时，是在新的、未曾见过的数据上进行预测。保留一部分数据不用于训练，可以作为未来真实数据的近似，用于评估模型的泛化能力。

我们来看一下，如何将交叉验证应用到识别盈利客户的问题中。大致流程如下：

1. 我们有一部分现有客户的数据，并知道哪些客户是盈利的、哪些不是（即我们有真实值）。我们称这部分为训练集（training set）。
2. 利用训练集，我们学习一个规则，用于判断客户是否盈利。
3. 然后，将这一规则应用于训练过程中未使用的另一部分数据（测试集），这模拟了在一个数据集上构建规则并将其应用于另一个数据集的过程。在模型投入实际使用后，这种情况是不可避免的。

下图展示了交叉验证的基本流程：最右侧是真正未观测的数据，中间部分是我们在训练阶段假装“不可见”的数据。

![img](./images/17/cross-validation.png)

总结来说，我们会将内部数据划分为训练集和测试集。训练集用于构建判断客户是否盈利的模型，测试集用于验证模型的预测效果。这部分测试数据在训练过程中是不可用的，从而更好地评估模型的泛化性能。

顺便提一句，除了简单的训练-测试划分外，现实中还有很多更精细的交叉验证方式，例如 K 折交叉验证（k-fold cross-validation）或时间交叉验证（temporal cross-validation）。不过在这里，这种简单划分已经足够使用。

交叉验证的核心目的是模拟模型投入实际使用后的表现。通过这种方式，我们可以获得更接近现实的预测效果评估。

在本例中，我们采用最简单的方式：将数据划分为两部分，70% 用于训练模型识别盈利客户，30% 用于评估该模型的预测能力。

```python
train, test = train_test_split(customer_features, test_size=0.3, random_state=13)
train.shape, test.shape
```

## 预测与决策规则

![img](./images/17/profit.png)

前面我们一直在讨论识别盈利客户的方法与思路，现在是时候更精准地定义几个关键概念了。我们将引入两个术语：**预测（prediction）**和**策略（policy）**。

首先，预测是一个用来估计某个结果的数值，对应的是 $\hat{E}[y_i \mid X_i]$，即条件期望的估计值。例如，我们可以预测某个客户的预期盈利为 16 巴西雷亚尔（BRL），意思是我们预计该客户将为公司带来 16 BRL 的净收益。这里的重点在于：预测只是一个数值。

第二个概念是策略（policy），即一种自动化的决策规则。预测给出的是一个数，而策略对应的是一个决策。例如，我们可以设定这样一个策略：当客户收入高于 1000 时与其建立合作，低于则不合作。更常见的是，我们基于预测值来制定策略：例如，预测盈利大于 10 的客户我们选择合作，否则不合作，也即 $\hat{E}[y_i \mid X_i] > 10$。

通常来说，机器学习负责的是第一步，也就是产生预测值。但需要注意的是，仅有预测本身是不够的，我们必须将其与相应的决策规则（策略）结合起来，才能在实际中发挥作用。

我们既可以采用非常简单的模型与策略，也可以设计复杂的预测与决策系统。不论复杂与否，对于预测与策略，我们都必须使用交叉验证：在一部分数据上估计预测值或策略，并在另一部分数据上验证其效果。

由于我们已经将数据划分为训练集和测试集，现在可以开始进行模型构建与策略评估了。

## 单一特征策略

在我们用机器学习“火力全开”解决盈利性客户识别问题之前，不妨先从最简单的策略入手。也就是所谓的“用 20% 的努力获得 80% 的收益”的方法。这类方法常常效果显著，令人惊讶的是，许多数据科学家反而忽略了它们。

那么，最简单的策略是什么？很自然地，就是：与所有客户建立合作关系。与其花大量精力判断哪些客户是盈利的，不如干脆对所有客户一视同仁，期望盈利客户所带来的收益足以覆盖非盈利客户的损失。

要判断这个策略是否可行，我们可以计算所有客户的平均净值（average net value）。如果这个平均值为正，说明从整体上看，与客户合作是盈利的。尽管个体客户中会存在盈利与非盈利之分，但只要客户数量足够多，总体上我们是赚钱的。

相反，如果这个平均净值为负，就意味着如果与所有客户合作，我们总体上将会亏损。

```python
train["net_value"].mean()
```

这就有些让人失望了……如果我们与所有客户都建立合作关系，每位客户将平均带来约 30 雷亚尔的亏损。我们尝试的第一个、非常简单的策略失败了。如果不想让业务亏损，我们最好寻找一些更有希望的方法。

这里顺便插一句，别忘了这是一个教学示例。虽然“与所有人都合作”这种简单策略在这个例子中不奏效，但在现实中，它们常常是有效的。比如：向所有用户群发营销邮件通常比什么都不做要好，向所有客户发放优惠券往往比不发更有效。

那么，接下来我们能想到的最简单策略是什么？一个自然的思路是：**直接利用已有的特征变量，看它们是否能够区分盈利客户与非盈利客户**。比如说“收入”（`income`）这个变量——直觉上来说，收入越高的客户应该越有可能带来盈利，对吧？如果我们只与收入最高的一部分客户合作，会不会是个好主意？

要验证这个想法，我们可以将客户按照收入划分为若干分位组（quantiles）。分位组的好处在于，它可以将数据划分为大小相等的子组，因此具有很好的比较性。接着，我们可以对每个收入分位组计算其平均净值。

我们的期望是，尽管总体平均净值为负（即 $E[\text{NetValue}] < 0$），但在某些由收入定义的子群体中，可能存在净值为正的情况，即 $E[\text{NetValue} \mid \text{Income}=x] > 0$，尤其是高收入客户群体。

```python
plt.figure(figsize=(12,6))## seed because the CIs from seaborn uses boostrap
np.random.seed(123)

# pd.qcut create quantiles of a column
sns.barplot(data=train.assign(income_quantile=pd.qcut(train["income"], q=20)), 
            x="income_quantile", y="net_value",
            hue="income_quantile", palette="husl", legend=False)

plt.title("Profitability by Income")
plt.xticks(rotation=70);
```

但遗憾的是，结果依然不理想。无论收入水平如何，所有分位组的客户其平均净值仍为负值。尽管高收入客户“没那么差”，即其平均亏损相对较小，但总体而言，他们仍然是亏损客户。因此，收入变量在这里并未帮到我们太多。

那我们不妨看看其他变量，比如地区（region）。如果我们的大部分成本来源于为偏远地区的客户提供服务，那么我们就可以合理地假设：**地区变量可能有助于区分盈利客户与非盈利客户**。

由于“地区”本身就是一个类别变量，我们不需要像收入那样进行分位处理。我们可以直接查看各地区的平均净值，看看是否存在某些地区客户更可能盈利。

```python
plt.figure(figsize=(12, 6))
np.random.seed(123)

region_plot = sns.barplot(data=train, x="region", y="net_value", 
    hue="region", palette="husl", legend=False         
                         )

plt.title("Profitability by Region");
```

太好了！从图中我们可以清楚地看到，一些地区是盈利的，比如地区 2、17、39；而另一些地区则是亏损的，比如地区 0、9、29，尤其是表现特别差的地区 26。这个结果非常有希望！我们可以据此制定一个简单的策略：只与数据显示为盈利的地区客户开展业务。

值得注意的是，我们现在所做的事情，其实本质上就是机器学习模型所做的，只不过方式更为简单——我们正在估计每个地区的条件期望值，即：

$$
E[\text{NetValue} \mid \text{Region}]
$$

接下来，我们需要基于这个估计结果构造一个策略。我们将采用一个非常简单的方法：为每个地区的平均净值构造一个 95% 的置信区间。如果该置信区间的下限大于 0，我们就认为该地区的客户是盈利的，并选择与之开展业务。

下面的代码构造了一个字典，其中键是地区编号，值是该地区净值期望的 95% 置信区间的下限。然后，我们筛选出所有下限大于零的地区。这些地区即为我们根据当前数据决定开展业务的地区。

```python
# extract the lower bound of the 95% CI from the plot above
regions_to_net = train.groupby('region')['net_value'].agg(['mean', 'count', 'std'])

regions_to_net = regions_to_net.assign(
    lower_bound=regions_to_net['mean'] - 1.96*regions_to_net['std']/(regions_to_net['count']**0.5)
)

regions_to_net_lower_bound = regions_to_net['lower_bound'].to_dict()
regions_to_net = regions_to_net['mean'].to_dict()

# filters regions where the net value lower bound is > 0.
regions_to_invest = {region: net 
                     for region, net in regions_to_net_lower_bound.items()
                     if net > 0}

regions_to_invest
```

变量 `regions_to_invest` 包含了我们决定要开展业务的所有地区。接下来，我们来看一下这个策略在测试集上的表现——也就是我们“假装没有见过”的那部分数据。

这一步是评估策略效果的关键。原因在于：某个地区在训练集中看起来是盈利的，可能仅仅是由于样本波动或偶然性。如果这种盈利表现只是随机出现的，那么我们很可能无法在测试集中观察到相同的规律。

为此，我们会将测试集中仅属于这些被判断为“盈利”的地区的客户筛选出来，然后绘制这些客户的净收益分布图，并计算我们策略对应的平均净收益。

```python
region_policy = (test[test["region"]
                      # filter regions in regions_to_invest
                      .isin(regions_to_invest.keys())]) 

sns.histplot(data=region_policy, x="net_value")
# average has to be over all customers, not just the one we've filtered with the policy
plt.title("Average Net Income: %.2f" % (region_policy["net_value"].sum() / test.shape[0]));
```

## 将机器学习模型用于决策输入

如果你希望进一步提升策略效果，现在我们可以借助机器学习的强大能力。当然，需要注意的是，这通常会显著增加建模复杂度，而实际提升可能只是边际上的。但在某些情况下，边际提升也可能转化为可观的收益，这正是机器学习在现实中广受重视的原因之一。

在这里，我们将使用一种名为梯度提升（Gradient Boosting）的模型。这类模型从原理上来说相对复杂，但使用起来其实非常方便。就我们的目的而言，并不需要深入其内部机制，只需要回忆在“机器学习速成课”中学到的内容：机器学习模型本质上是一个强大的预测工具，用于估计条件期望 $E[Y \mid X]$。模型越复杂，其拟合能力越强，但如果复杂度过高，就可能发生过拟合，学习到噪声，导致在新数据上表现不佳。因此，我们仍需使用交叉验证，确保模型复杂度合适。

那么，问题来了：**我们该如何利用这些更好的预测结果来改进我们之前基于地区的简单策略，从而更有效地识别并接触盈利客户？**

我认为这里有两个主要的改进点。第一，逐个变量地寻找能够区分盈利客户和非盈利客户的特征，其实是一个非常繁琐的过程。在本例中，我们只有三个变量（年龄、收入和地区），工作量还不算大，但如果有上百个变量，显然就难以操作。此外，这样做还容易引发[多重检验（multiple testing）](https://en.wikipedia.org/wiki/Multiple_comparisons_problem)问题和假阳性率偏高的问题。

第二点是，客户的盈利能力往往不是由单一变量决定的。在我们的例子中，除了地区变量以外，收入和年龄也可能包含关于客户价值的重要信息。虽然我们先前发现收入单独预测效果不强，但如果只在“略微亏损”的那些地区中考虑高收入客户，或许仍有可能实现盈利。换句话说，我们是在提出一个更强的预测结构：

$$
E[\text{NetValue} \mid \text{Region}, \text{Income}, \text{Age}] > E[\text{NetValue} \mid \text{Region}]
$$

这个思路是合理的：在已有的地区基础上，加入收入和年龄的信息，应该可以提高我们对客户净值的预测精度。

然而，构造这样更复杂的策略，尤其是涉及多个变量交互关系的策略，其组合数量将呈指数增长，很难手动完成。更可行的方式是：将所有特征变量输入机器学习模型，让模型自动学习这些变量之间的关系和交互项。这正是我们接下来要做的事情。

我们的目标是建立一个预测 `net_value` 的模型，使用的特征包括 `region`、`income` 和 `age`。其中，“地区”是一个类别变量，我们需要先将其转换为数值型。这里我们采用的方法是：将每个地区替换为它在训练集中的平均净值。还记得我们之前构造的 `regions_to_net` 字典吗？只需用 `.replace()` 方法将该字典传入即可。

由于我们将多次进行这种替换操作，接下来会将其封装成一个函数。这个过程属于机器学习中常说的**特征工程（feature engineering）**，也就是为了帮助模型学习而对原始变量进行转换与处理。



```python
def encode(df): 
    return df.replace({"region": regions_to_net})
```

接下来，我们将从 [Sklearn](https://scikit-learn.org/stable/) 导入模型。Sklearn 中的所有模型都有非常标准的使用流程：

首先，需要实例化模型，并传入一些用于控制模型复杂度的参数。对于这个梯度提升模型，我们将设置估计器数量为 400，最大深度为 4，等等。一般来说，模型越深、估计器数量越多，模型的拟合能力就越强。当然，我们不能让模型过于强大，否则它可能会学习到训练数据中的噪声，即发生过拟合。

不过，你无需了解这些参数的具体含义，只要记住：这是一个预测性能很强的模型就足够了。

然后，要训练模型，只需调用 `.fit()` 方法，并将特征变量 `X` 和我们想要预测的因变量（即目标变量）`net_value` 作为输入即可。

```python
model_params = {'n_estimators': 400,
                'max_depth': 4,
                'min_samples_split': 10,
                'learning_rate': 0.01,
                'loss': 'squared_error'}

features = ["region", "income", "age"]
target = "net_value"

np.random.seed(123)

reg = ensemble.GradientBoostingRegressor(**model_params)

# fit model on the training set
encoded_train = train[features].pipe(encode)
reg.fit(encoded_train, train[target]);
```

模型现在已经训练完成，接下来我们需要评估它的效果。为此，我们要查看该模型**在测试集上的预测表现**。

用于评估机器学习模型预测性能的指标有很多种。这里我们采用其中一个常用指标：$R^2$（决定系数）。我们不需要深入讨论其技术细节，简单理解即可：
$R^2$ 通常用于评估对连续变量的预测模型，比如 `net_value`。

值得注意的是，$R^2$ 的取值范围可以从负无穷到 1。如果模型的预测效果甚至比直接用平均值还差，$R^2$ 就会为负值；如果模型能够完美预测，则 $R^2 = 1$。

从直观上说，$R^2$ 表示模型能够解释 `net_value` 变异程度的比例。

```python
train_pred = (encoded_train
              .assign(predictions=reg.predict(encoded_train[features])))

print("Train R2: ", r2_score(y_true=train[target], y_pred=train_pred["predictions"]))
print("Test R2: ", r2_score(y_true=test[target], y_pred=reg.predict(test[features].pipe(encode))))
```

在本例中，模型解释了训练集中约 71% 的 `net_value` 方差，而在测试集中只能解释约 69%。这是可以预期的：由于模型在训练时接触过训练集数据，其在该数据集上的表现往往会被高估。

顺带一提，如果你想更深入了解**过拟合（overfitting）**，可以尝试将模型的 `max_depth` 参数设为 14，然后观察结果。你很可能会发现训练集上的 $R^2$ 飙升，而测试集上的 $R^2$ 反而下降——这正是过拟合的典型表现。

接下来，为了制定我们的策略，我们将把模型在测试集上的预测结果存储在一个名为 `prediction` 的新列中。这个预测值即为条件期望的估计：

$$
\hat{E}[\text{NetValue} \mid \text{Age}, \text{Income}, \text{Region}]
$$

```python
model_policy = test.assign(prediction=reg.predict(test[features].pipe(encode)))

model_policy.head()
```

就像我们之前对 `region` 变量所做的那样，我们也可以根据模型的预测值来查看平均净值的分布。但由于模型预测结果是连续变量而不是类别变量，我们需要先将其**离散化**。

一种常用的做法是使用 pandas 中的 `pd.qcut` 函数（说实话，我非常喜欢这个函数！）。它可以根据模型的预测值将数据按分位数进行分组。

我们这里使用 50 个分位组，主要是因为我们之前的 `region` 变量正好也有 50 个取值。作为一种惯例，我倾向于将这些由模型预测值分组得到的分位组称为**模型带（model bands）**，这个称呼更直观地反映了：每个分组内的预测值落在某一个区间范围内，比如从 -10 到 200。

```python
plt.figure(figsize=(12,6))

n_bands = 50
bands = [f"band_{b}" for b in range(1,n_bands+1)]

np.random.seed(123)
model_plot = sns.barplot(data=model_policy
                         .assign(model_band = pd.qcut(model_policy["prediction"], q=n_bands)),
                         x="model_band", y="net_value",
                         hue="model_band", palette="husl", legend=False  
                        )
plt.title("Profitability by Model Prediction Quantiles")
plt.xticks(rotation=70);
```

我们可以观察到，有些模型带的净值非常为负，而另一些则非常为正；还有一些模型带的净值则处于不确定状态——我们无法明确判断其为正还是为负。

此外，值得注意的是，从左到右，净值呈现出上升趋势。这是符合预期的：既然我们是在预测净值，那么预测值越高，实际净值也应该相应更高，二者应具有一定的正相关关系。

现在，为了将基于机器学习模型的策略与我们之前仅使用地区变量的策略进行比较，我们可以绘制净收益的直方图，并同时展示测试集上的总净收益。

```python
plt.figure(figsize=(10,6))
model_plot_df = (model_policy[model_policy["prediction"]>0])
sns.histplot(data=model_plot_df, x="net_value", color="C2", label="model_policy")

region_plot_df = (model_policy[model_policy["region"].isin(regions_to_invest.keys())])
sns.histplot(data=region_plot_df, x="net_value", label="region_policy")

plt.title("Model Net Income: %.2f;    Region Policy Net Income %.2f." % 
          (model_plot_df["net_value"].sum() / test.shape[0],
           region_plot_df["net_value"].sum() / test.shape[0]))
plt.legend();
```

正如我们所看到的，机器学习模型确实构建出了一个比仅使用 `region` 特征更优的策略，但提升幅度不大。在测试集中，模型策略为每位客户带来的平均净收益约为 16.6 雷亚尔，而基于地区的策略仅为 15.5 雷亚尔。虽然差距不大，但如果客户数量非常庞大，这种边际提升已经足以证明使用机器学习模型的合理性。

## 更精细化的策略

回顾一下，我们目前为止测试了几种策略：

1. 最简单的策略是与所有客户合作，这相当于估计边际净收益：

   $$
   \hat{E}[\text{NetValue}] > 0
   $$

   由于这种策略的平均每客户净收益为负，因此效果不佳。

2. 接着我们尝试了一个基于单一特征的策略，即基于地区信息做决策：只在特定地区开展业务，对应形式为：

   $$
   \hat{E}[\text{NetValue} \mid \text{Region}] > 0
   $$

   这一策略带来了显著改进。

3. 然后我们引入机器学习，使用所有特征变量建立预测模型，对应表达为：

   $$
   \hat{E}[\text{NetValue} \mid \text{Region}, \text{Income}, \text{Age}] > 0
   $$

   基于这个模型，我们制定了策略：只与预测净值为正的客户开展合作。

上述所有策略的决策逻辑都是二元的：是否与客户建立合作关系。这类策略通常可以表达为如下形式：

```python
if prediction > 0:
    do business
else:
    don’t do business
```

这种策略被称为**阈值策略（thresholding）**：当预测值超过某个阈值（在本例中是 0）时采取某一行动，否则采取另一行动。

阈值策略在很多实际场景中都适用，尤其当决策是二元选择时非常有效。例如在交易欺诈识别中，如果欺诈模型的预测得分高于某个阈值 X，则拒绝交易，否则批准。

然而，在某些情况下，决策可能并不是简单的“做”或“不做”，而是程度型或连续型决策。例如，你可能愿意对预计特别盈利的客户投入更多营销预算，甚至将他们加入“重点客户名单”，给予额外服务（虽然这也意味着更高的服务成本）。

一旦考虑这种情况，决策就从**二元选择**转向**连续选择**：你不只是决定是否与客户合作，还要决定在每位客户身上投入多少资源。

下面这个示例中，假设你的决策不再是“是否合作”，而是“对每位客户应投入多少营销预算”。再进一步，假设你正与其他公司竞争——谁在某位客户身上花的营销费最多，谁就赢得该客户（类似于竞价机制）。那么，合理的策略是：**对高盈利客户投入多、对边际盈利客户投入少、对亏损客户不投入**。

实现该策略的一种方式是：将预测值离散化为分组（band）。我们之前已经使用过类似的方法来对预测结果进行可视化，但这次我们将它用于实际决策。

我们将把预测结果划分为 20 个分组，可以理解为 20 个分位组或等大小的客户群。第 1 组将包含预测净值最低的 5% 客户，第 2 组包含第 5% 到第 10% 的客户……而第 20 组则包含预测净值最高的客户。

请注意：分组边界必须基于训练集来确定，然后才能将其应用于测试集。因此，我们会使用 `pd.qcut` 在训练集上计算出这些分组边界（bins），接着用 `np.digitize` 将测试集中的预测值分配到对应的分组中。

```python
def model_binner(prediction_column, bins):
    # find the bins according to the training set
    bands = pd.qcut(prediction_column, q=bins, retbins=True)[1]
    
    def binner_function(prediction_column):
        return np.digitize(prediction_column, bands)
    
    return binner_function
    

# train the binning function
binner_fn = model_binner(train_pred["predictions"], 20)

# apply the binning
model_band = model_policy.assign(bands = binner_fn(model_policy["prediction"]))
model_band.head()
```

```python
plt.figure(figsize=(10,6))
sns.barplot(data=model_band, x="bands", y="net_value", hue="bands", palette="husl", legend=False )
plt.title("Model Bands");
```

有了这些分组（bands），我们就可以将大部分的营销预算集中投放在第 19 和第 20 组客户身上。请注意，我们的决策模式已经从最初的**二元选择（是否合作）**，演变为一个**连续决策问题**：即**对每位客户应投入多少营销资源**。

当然，你可以通过增加分组数量进一步细化策略；在极限情况下，甚至可以**不再进行分组**，而是**直接使用模型的预测值作为投资决策的依据**。比如：

```python
mkt_investments_i = model_prediction_i * 0.3
```

也就是说，对于每位客户 $i$，我们按照模型预测的净值投入其中 30% 的营销预算（这里的 30% 只是一个示意性的参数，具体数值可以根据实际情况调整）。

## 关键观点

在非常短的时间内，我们已经覆盖了大量内容，因此这一总结非常有必要，帮助我们理清目前的进展。

首先，我们学习了机器学习的大多数应用，其核心只是围绕一个目标：做出高质量的预测。所谓预测，可以理解为：从一个已知的输入变量，映射到一个起初未知但定义明确的输出变量。这种预测本质上就是估计条件期望函数 $E[Y \mid X]$。

当然，说“仅仅是做预测”可能并不完全公平。我们也看到了，良好的预测能够帮助我们解决许多超出直觉的问题，例如语言翻译、自动驾驶等。

随后，我们回到更贴近现实的商业应用场景，探讨了如何通过预测客户的盈利能力，制定谁值得我们投入资源、谁不值得的策略。具体而言，我们尝试预测客户净收益，并基于该预测值建立相应的策略。这只是预测模型的一个应用示例，类似的还有信用评分、欺诈检测、癌症诊断等领域——只要预测有用，机器学习就能发挥作用。

本章的核心结论是：

> **只要你能将一个业务问题表述为一个预测问题，那么机器学习很可能就是解决它的最佳工具。**

我必须特别强调这一点。在当前机器学习被大量炒作的背景下，人们常常忽略了这一基本原则，反而投入大量精力构建一些预测能力很强、但对实际业务无用的模型。

正确的顺序应当是：**先思考如何将业务问题转化为预测问题，然后再用机器学习工具去解决它**。而不是反过来：先构建一个预测模型，再去寻找有没有哪个业务问题能“凑合用”这个模型。后一种方式偶尔可能奏效，但更多时候只是“盲目试错”，最终变成“为了解决模型而找问题”的局面。

## 参考说明

本章内容主要基于作者个人的经验总结，许多观点和方法源自实践中的体会。因此，无法提供严格意义上的学术参考文献。与正规科研工作不同，这些内容没有经过系统性的学术审查，也未经过同行评议与理论论证。读者或许会注意到，本文更多讨论的是“实践中有效的方法”，而非详细阐述其背后的理论基础。这种方法可以被视为“来自一线的经验科学”（a sort of science from the streets）。尽管如此，既然这些内容已公开发布，也非常欢迎批评与反馈——如果读者发现其中有明显错误或不妥之处，欢迎提出 issue，作者会尽力回应。

最后，对于那些希望系统学习机器学习的读者，作者也意识到本章的讲解可能过于简略。坦率地说，作者认为自己真正能够贡献价值的领域是因果推断的教学，而非机器学习。机器学习领域已有大量优质的公开资源，远超作者所能提供的内容。其中的经典之作，就是 [Andrew Ng 的机器学习课程](https://www.coursera.org/learn/machine-learning)，如果读者是机器学习初学者，作者强烈推荐认真学习这门课程。


## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。


---

# 18 - 异质性处理效应与个性化

## 从预测到因果推断

在上一章中，我们简要地介绍了机器学习模型。机器学习模型是一种用于“预测”的工具，或者更技术化地说，是用于估计条件期望函数 $E[Y|X]$。换句话说，当你想将已知的输入 $X$（例如一句英文句子、本月的销售额、脑部扫描图像）映射到一个最初未知但定义明确的输出 $Y$（如日文句子、下月销售额或者癌症诊断）时，机器学习非常有用。因此，既然机器学习处理的是预测或估计 $E[Y|X]$，要让它发挥作用，你就必须把你想用机器学习解决的问题表述为预测问题——一个以估计 $E[Y|X]$ 为关键的问题。上一章中我们走过这样一个例子：我们要根据客户的特征预测客户盈利能力，即 $E[NetValue|Age, Income, Region]$。这一信息非常有用，因为它让我们能够把精力集中在与盈利客户互动上，同时不与不盈利的客户做生意。在这里，准确预测盈利能力是关键。

请注意，这是一种被动估计方式，意味着你把自己从数据生成过程剥离出来。在我们的例子中，我们假定客户盈利能力 `NetValue` 是既定的，我们所要做的只是估计它。换句话说，我们假设除了预测客户的盈利能力外，我们没有办法去提高或降低它。但这并不总是正确的。事实上，很多时候公司可以通过某些杠杆来提高客户盈利能力。这些杠杆可以是更高端或更低廉的客户服务、折扣、定价或市场营销。在行业界，我们往往参与在数据生成过程中，我们能够影响它。因此，作为在行业中工作的数据科学家，我们经常要回答什么是最佳行动方案或应做出什么干预才能优化某些业务指标，通常是盈利或某些中间指标，如转化率、成本或销售额。

在这个我们不再是被动观察者的世界里，仅仅估计 $E[Y|X]$ 并不完整。这就是我们进入因果推断的地方。我们需要在条件期望函数中再加入一项，这一项正是用来刻画我们对数据生成过程的影响。这一项就是处理变量：

$$
E[Y|X, T]
$$

现在我们必须区分情境或外生特征 $X$ 和处理变量 $T$。两者都会影响结果 $Y$，但我们无法控制 $X$，却可以决定 $T$ 的取值或至少对其进行干预。举个具体的例子，$Y$ 可以是某一天的销售额，$X$ 可以是我们不能控制但能提供关于销售信息的背景特征，如前几日的平均销售额，而 $T$ 是我们可以干预以提高销售的处理变量，如价格、库存水平或市场营销。因果推断就是在给定情境 $X$ 下，估计 $T$ 与 $Y$ 之间因果关系的过程。一旦我们完成了这一步，优化 $Y$ 就只是选择一个最优的处理值 $T$：

$$
\underset{T}{argmax} \ E[Y|X, T]
$$

从这个意义上讲，除了因果推断的积极方面，我们还有规范性的动机。

在第一部分中，我们试图回答的问题包括：接受教育的价值是多少？法律改革能否降低吸烟率？保持积极心态能否提升学术成就？酒精对死亡率的影响如何？所有这些问题从纯科学的角度看都很有趣，因为它们帮助我们理解世界是如何运作的。但这些问题背后也有实践动机。如果我们知道教育对收入的影响，我们就能理解为教育付出多少代价是合理的。从数学上看，我们所做的是估计教育的因果效应并对其进行优化：$\underset{Educ}{argmax} \ E[Income|X, Educ]$。

第一部分的重点是回答某种处理总体上是正的、强的还是为零。例如，我们想知道投资教育通常是否值得。同时在第一部分中，$X$ 的作用有两个方面。首先，$X$ 可能包含混杂变量，在这种情况下，只有当我们考虑或调整 $X$ 时，因果效应才是可识别的；或者，$X$ 可以用来减小因果估计的方差。如果 $X$ 是 $Y$ 的良好预测变量，我们可以用它来解释掉 $Y$ 的变异，使得因果效应更加明显。

现在，事情不再是非黑即白的了。我们想要的不仅仅是平均处理效应。我们将允许处理对某些人有积极影响，对另一些人却没有。情境特征 $X$ 将在定义不同单元的不同特征组合上发挥作用，而每种特征组合可能对处理有不同的反应。我们现在希望实现处理的个性化，只把处理给予对其反应最好的人群。我们从一个只关心平均处理效应的世界，走向了一个我们想要异质性处理效应的世界。


## 从平均处理效应到条件平均处理效应（CATE）

到目前为止，每次我们估计某一处理的因果影响时，得到的都是平均处理效应（有时是局部平均处理效应）：

$$
E[Y_1 - Y_0]
$$

或其对应的连续处理形式

$$
E[y'(t)]
$$

其中 $y'(t)$ 是响应函数或结果的处理导数。我们学习了若干技术来揭示处理的整体有效性。平均处理效应估计是因果推断的基石，它是一个非常有用的工具，用于我们称之为项目评价的决策问题——我们想知道是否应该将某项干预推向整个总体。不要被公共政策的术语迷惑，同样的技术也可以用来评估推出新产品对企业盈利的影响。这里关键要注意的是，我们要支持的决策是是否应当对所有人实施处理。

现在，我们要考虑另一种类型的决策：**应该对谁实施处理？** 现在，决策可以在不同个体之间变化。对某个个体施加处理可能是有利的，但对另一个则未必。我们希望个性化处理。更技术地说，我们要估计条件平均处理效应（CATE）

$$
E[Y_1 - Y_0 \ |\ X] \quad\text{或}\quad E[y'(t)\,|\,X]
$$

在 $X$ 上条件意味着我们现在允许处理效应根据每个单位的特征而不同。同样在这里，我们认为并非所有个体对处理反应相同，我们想利用这种异质性。我们希望只对正确的单位施加处理（在二元处理的情况下），或者确定每个单位的最优处理剂量（在连续处理的情况下）。

举个例子，如果你是一家银行，需要决定每个客户能够获得多大的贷款，你可以肯定地说盲目给每个人大量贷款不是好主意——虽然对于某些人可能合理。你必须聪明地设计你的处理（贷款金额）。或许，根据客户的信用评分（$X$），你可以找出合适的贷款额度。当然，你不必是大型机构才能利用个性化。此类例子不胜枚举：一年中的哪些天应该打折促销？某个产品应该定价多少？每个人应该运动多长时间才适合？

换个角度思考。你有一堆客户和一个处理变量（价格、折扣、贷款等）。你希望个性化处理，例如给不同的客户不同的折扣。

![img](./images/18/customers.png)

为此，你必须对客户进行分组。你创建了对处理反应不同的群组。例如，你要找出对折扣反应好的客户和反应差的客户。客户对处理的反应由条件处理效应 $\frac{\delta Y}{ \delta T}$ 给出。因此，我们可以设法为每个客户估计这一效应，然后把那些对处理反应强烈的（高处理效应）和那些反应不那么强烈的分到一起。如果这么做，我们将像下图那样划分客户空间。

![img](./images/18/elast-partition.png)

这非常棒，因为我们可以在每个分区中估计不同的处理效应或敏感度。注意，敏感度正是从 $T$ 到 $Y$ 的曲线或函数的斜率。因此，如果我们能够产生斜率或敏感度不同的分区，就意味着这些分区中的实体对处理的反应不同。

![img](./images/18/elast-split.png)

换言之，你想做的是不再预测 $Y$ 的原始值，而是开始为每个单位预测 $Y$ 对 $T$ 的导数，即 $\frac{\delta Y}{ \delta T}$。例如，假设 $Y$ 是冰淇淋销售量，$T$ 是冰淇淋价格，每个单位 $i$ 是一天。撇开道德问题不谈，假设你可以每天改变冰淇淋的价格。如果你能找出那些 $\frac{\delta Sales}{ \delta Price}$ **很低**的日子，那么在那些日子里你就可以**提高价格**而不会损失太多销量。也许你已经在这么做，比如在假日季提价。关键是，按价格敏感度区分不同的日子很有用，因为它为你如何设定最优价格提供了依据。

你可能会说，这有点棘手。如果我看不到 $\frac{\delta Sales}{ \delta Price}$，如何预测敏感度？这是个很好的问题。敏感度在单个单位层面上实际上是不可观察的。不仅如此，这还是一个陌生的概念。我们更习惯于按原始数量思考，而不是按这些数量的变化率来思考。为了更好地概念化敏感度，这里有个小技巧。你可以把每个个体看成既有一个 $Y_i$ 值（在我们的例子中是销售量），又有一个个体敏感度 $\frac{\delta Y_i}{\delta T_i}$。敏感度就是 $Y$ 随 $T$ 的变化量，所以你可以把每个个体看成还有一个斜率系数 $\frac{\delta Y}{ \delta T}_i$。在我们的例子中，我们会说每一天都有一个价格对销量的斜率系数。

![img](./images/18/elasticity.png)

当然，我们无法看到这些个体的斜率系数。若要看到个体的斜率，我们必须观察每一天在两个不同价格下的销售量，并计算销售量在这两个价格下的变化：

$$
\frac{\delta Y_i}{ \delta T_i} \approx \frac{Y(T_i) - Y(T_i + \epsilon)}{T_i - (T_i + \epsilon)}
$$

这正是因果推断的基本问题的重现：我们永远无法看到同一单位在不同处理条件下的表现。那么，我们该怎么办呢？


## 预测敏感度

我们又遇到了一个棘手问题。我们已经同意需要预测 $\frac{\delta Y_i}{ \delta T_i}$，然而遗憾的是它不可观测。因此我们无法将其作为目标直接交给机器学习算法去学习。但也许，预测它并不需要直接观测 $\frac{\delta Y_i}{ \delta T_i}$。

这里有一个想法：假如我们使用线性回归呢？

![img](./images/18/linear-fix.png)

假设你对数据拟合如下线性模型：

$$
y_i = \beta_0 + \beta_1 t_i + \beta_2 X_i + e_i
$$

如果对该模型对处理变量求导，将会得到

$$
\frac{\delta y_i}{\delta t_i} = \beta_1 
$$

既然我们可以估计上述模型并得到 $\hat{\beta_1}$，我们甚至可以大胆地说，**即使不能观察敏感度，也能预测它**。在上述情形中，预测非常简单——我们为每个人预测相同的常数值 $\hat{\beta_1}$。这当然是有用的，但还不是我们想要的。这只是平均处理效应，而不是条件平均处理效应。由于每个人得到的敏感度预测相同，这并不利于按照对处理的响应程度来对个体进行分组。为了解决这一点，我们可以做如下简单的修改：

$$
y_i = \beta_0 + \beta_1 t_i + \beta_2 X_i + \beta_3 \, t_i X_i \, + e_i
$$

这将产生如下的敏感度预测：

$$
\widehat{\frac{\delta y_i}{\delta t_i}} = \hat{\beta_1} + \hat{\beta_3}X_i
$$

其中 $\beta_3$ 是 $X$ 中特征的向量系数。

现在，由不同的 $X_i$ 所定义的每个实体都会有不同的敏感度预测。换言之，敏感度预测随着 $X$ 的变化而变化。因此，回归为我们提供了一种估计 CATE $E[y'(t)|X]$ 的方法。

我们终于走出了一步。上述模型使得我们能够为每个实体做出敏感度预测。有了这些预测，我们就可以构建更有用的分组：把预测敏感度高的单位归为一组，把预测敏感度低的单位归为另一组。总之，凭借敏感度预测，我们可以按照我们认为各实体对处理的响应程度来对其进行分组。

理论讲得够多了。现在是时候通过一个例子来展示如何构建这类敏感度模型。考虑我们的冰淇淋例子。每个单位 $i$ 是一天。对于每一天，我们知道它是否是工作日、制作冰淇淋的成本（你可以把成本视为质量的一个代理），以及当天的平均温度。这些构成了我们的特征空间 $X$。然后我们有处理变量——价格，以及结果变量——售出的冰淇淋数量。在此例中，我们假定处理是随机的，这样暂且不用担心偏倚。


```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt

import seaborn as sns
import statsmodels.formula.api as smf
import statsmodels.api as sm

from sklearn.ensemble import GradientBoostingRegressor
from sklearn.model_selection import train_test_split
```

```python
prices_rnd = pd.read_csv("./data/ice_cream_sales_rnd.csv")
print(prices_rnd.shape)
prices_rnd.head()
```

请记住我们的目标：根据每日的特征 `temp`（温度）、`weekday`（是否工作日）和 `cost`（成本）决定何时提高价格、何时降低价格。如果这就是目标，那么处理效应异质性模型必须按照其在实现该目标中的效用来评估。我们马上会讨论这一点（并将在下一章详细展开）。眼下，我们先将数据集划分为训练集和测试集。


```python
np.random.seed(123)
train, test = train_test_split(prices_rnd)
```

现在我们有了训练数据，需要构建一个模型，使我们能够区分对价格高度敏感的日子和对价格不敏感的日子。我们的做法很简单：直接预测价格敏感度。具体怎么做呢？首先，让我们考虑使用如下线性模型：

$$
sales_i = \beta_0 + \beta_1 price_i + \pmb{\beta_2} X_i + e_i
$$

通过查看该模型的参数，我们可以看到预测的敏感度会是什么样子。


```python
m1 = smf.ols("sales ~ price + temp+C(weekday)+cost", data=train).fit()
m1.summary().tables[1]
```

对于模型 $m1$，预测的价格敏感度 $\widehat{\dfrac{\delta y_i}{\delta t_i}}$ 等于 $\hat{\beta_1}$，在我们的例子中为 -2.75。这意味着冰淇淋价格每上涨 1 巴西雷亚尔，预计销量将下降约 3 单位。

注意，$m1$ 为每个人预测的敏感度完全相同。因此，如果我们想知道哪些日子人们对冰淇淋价格不那么敏感，它就不是一个很好的模型。它估计的是平均处理效应，而我们这里需要的是条件平均处理效应。记住，我们的目标是将实体划分成不同的组，以便为每个分组个性化并优化我们的处理（价格）。如果所有预测都是一样的，就没有办法划分。我们没有区分出敏感和不敏感的单位。为了解决这个问题，考虑第二个模型：

$$
sales_i = \beta_0 + \beta_1 price_i + \beta_2 \, price_i \times temp_i \, + \pmb{\beta_3} X_i + e_i
$$

这个第二个模型包括价格与温度之间的**交互项**，这意味着它允许敏感度在不同温度下有所不同。我们实际上是在说，人们对价格上涨的敏感程度取决于温度的高低。


```python
m2 = smf.ols("sales ~ price*temp + C(weekday) + cost", data=train).fit()
m2.summary().tables[1]
```

一旦我们估计了该模型，预测的敏感度为

$$
\widehat{\frac{\delta sales_i}{\delta price_i}} = \hat{\beta_1} + \hat{\beta_2} \, temp_i
$$

注意，$\hat{\beta_2}$ 为正值 0.03，而基线敏感度 $\beta_1$（在 0 摄氏度的敏感度）是 -3.6。这意味着平均而言，当价格上升时，销量会下降，这是合理的。这也意味着温度每上升一度，人们对冰淇淋价格上涨的敏感度会降低（虽然幅度不大）。例如，在 25 摄氏度时，冰淇淋价格每上涨 1 巴西雷亚尔，销量下降 2.8 单位 $(-3.6 + (0.03 \times 25))$；而在 35 摄氏度时，价格每上涨 1 雷亚尔，销量只下降 2.5 单位 $(-3.6 + (0.03 \times 35))$。这也算是直观的：天气越热，人们越愿意为冰淇淋支付更高的价格。

我们还可以更进一步。下一个模型在整个特征空间上加入了交互项。这意味着敏感度会随着温度、星期几以及成本而变化。

$$
sales_i = \beta_0 + \beta_1 price_i + \pmb{\beta_2 X_i} \times price_i + \pmb{\beta_3} X_i + e_i
$$


```python
m3 = smf.ols("sales ~ price*cost + price*C(weekday) + price*temp", data=train).fit()
```

根据上述模型，单位层面的敏感度，也即 CATE，可以表示为

$$
\frac{\delta Sales}{\delta Price} = \beta_1 + \pmb{\beta_2 X_i}
$$

其中 $\beta_1$ 是价格系数，$\pmb{\beta_2}$ 是交互项系数的向量。

最后，让我们看看如何实际做出这些敏感度预测。一种方法是直接从模型中提取敏感度参数并使用上面的公式。然而，我们将采用一种更通用的近似方法。既然敏感度不过是结果对处理的导数，我们可以利用导数的定义：

$$
\frac{\delta y}{\delta t} = \dfrac{y(t+\epsilon) - y(t)}{ (t + \epsilon) - t }
$$

其中 $\epsilon$ 趋于零。我们可以用 $\epsilon = 1$ 来近似这个定义：

$$
\frac{\delta y}{\delta t} \approx \hat{y}(t+1) - \hat{y}(t)
$$

这里的 $\hat{y}$ 是模型的预测值。换句话说，我将用模型做两次预测：一次使用原始数据，另一次使用将处理变量增加一个单位的原始数据。这两个预测的差就是我的 CATE 预测。

下面可以看到一个实现这一操作的函数。由于我们已经使用训练集估计了模型，现在将在测试集上做预测。首先让我们使用第一个平均处理效应模型 $m1$。


```python
def pred_sensitivity(m, df, t="price"):
    return df.assign(**{
        "pred_sens": m.predict(df.assign(**{t:df[t]+1})) - m.predict(df)
    })

pred_sensitivity(m1, test).head()
```

使用模型 $m1$ 来预测敏感度毫无趣味。我们可以看到它对所有天都预测完全相同的值。这是因为该模型没有任何交互项。然而，如果我们使用模型 $m3$ 进行预测，它会为每一天输出不同的敏感度预测。这是因为此时敏感度或处理效应取决于当天的特征。


```python
pred_sens3 = pred_sensitivity(m3, test)

np.random.seed(1)
pred_sens3.sample(5)
```

注意这些预测的数值大约在 -9 到 1 之间。它们并不是对销售量列的预测，后者的数量级在数百。相反，**它们是对将价格提高一个单位时销售量将变化多少的预测**。一开始我们会看到一些奇怪的数字。例如，看看第 4764 天，它预测了正的敏感度。换句话说，我们预测如果提高冰淇淋价格，销量会增加。这与经济常识不符，很可能是模型在该点进行了奇怪的外推。幸运的是，你不必过于担心这一点。记得我们的最终目标是按单位对处理的敏感程度进行分组，**并不是**要得到最准确的敏感度预测。为达成我们的主要目标，只要敏感度预测能按照敏感度大小对单位排序即可。换句话说，即使像 1.1 或 0.5 这样的正敏感度预测看起来没有意义，我们所需要的只是排序正确，即预测值为 1.1 的单位对价格上涨的影响小于预测值为 0.5 的单位。

好的，我们有了敏感度或 CATE 模型。但仍然有一个潜在的问题：它们与传统的机器学习预测模型相比如何？现在让我们试一下。我们将使用一种机器学习算法，把价格、温度、是否工作日以及成本作为特征 $X$，并尝试预测冰淇淋的销量。


```python
X = ["temp", "weekday", "cost", "price"]
y = "sales"
ml = GradientBoostingRegressor()
ml.fit(train[X], train[y])

# make sure the model is not overfiting.
ml.score(test[X], test[y])
```

该模型可以预测每天的销售量。但是它适合我们的真正需求吗？换言之，这个模型能区分哪些天人们对冰淇淋价格更敏感吗？它能帮助我们根据价格敏感度决定收取多少费用吗？

为了看哪种模型更有用，让我们尝试利用它们来分割单位。对于每个模型，我们都将把单位划分为两组。我们希望其中一组对价格上涨反应强烈，另一组反应不那么明显。如果的确如此，我们就可以围绕这些分组来安排业务：对于落在高反应性组的日子，最好不要把价格定得太高；而对低反应性组，我们可以提高价格，而不会过多损失销量。


```python
bands_df = pred_sens3.assign(
    sens_band = pd.qcut(pred_sens3["pred_sens"], 2), # create two groups based on sensitivity predictions 
    pred_sales = ml.predict(pred_sens3[X]),
    pred_band = pd.qcut(ml.predict(pred_sens3[X]), 2), # create two groups based on sales predictions
)

bands_df.head()
```

接下来，我们需要比较这两种分割方案中哪一种更好。我现在可能有点超前了，因为我们只有在下一章才会详细讨论 CATE 模型的评估。不过我想给你提前看看它的样子。一个非常简单的办法来检验这些分区方案的效果——这里的“好”指的是实用——就是在每个分区中绘制价格对销量的回归线。我们可以利用 Seaborn 的 `regplot` 加上 `FacetGrid` 来轻松实现。

下面，我们可以看到利用敏感度预测所做的分区。请记住，所有这些都在测试集上完成。


```python
g = sns.FacetGrid(bands_df, col="sens_band")
g.map_dataframe(sns.regplot, x="price", y="sales")
g.set_titles(col_template="Sens. Band {col_name}");
```

正如我们所看到的，这种划分方案似乎是有用的。在第一组中，价格敏感度很高。随着价格的上涨，销量大幅下降。然而在第二组中，随着价格上涨，销量大致保持不变。实际上，销量看起来甚至会随着价格的提高而上升，但这很可能是噪声。

对比用机器学习预测模型做出的分区：


```python
g = sns.FacetGrid(bands_df, col="pred_band")
g.map_dataframe(sns.regplot, x="price", y="sales")
g.set_titles(col_template="Pred. Band {col_name}");
```

我非常喜欢这幅图，因为它传达了一个重要的观点。如你所见，预测模型的分区是在 y 轴上分割单位。在第一组那样的日子，我们卖出的冰淇淋不多，而在第二组那样的日子，我们卖得更多。我觉得这很神奇，因为预测模型正是在做它应该做的事情：预测销售量。它能区分冰淇淋销量低的日子和销量高的日子。

唯一的问题是这种预测在这里并不是特别有用。最终我们想知道什么时候可以提价，什么时候不能。但是当我们查看预测模型分区中回归线的斜率时，会发现它们几乎没有变化。换句话说，由预测模型所定义的两个分区对价格上涨的响应程度几乎相同。这并不能为我们提供多少关于哪些日子可以提高价格的洞见，因为看起来价格对销量根本没有影响。

## 关键观点

我们终于正式提出了条件平均处理效应的概念，以及它如何在个性化中发挥作用。也就是说，如果我们能够理解每个单位如何对处理作出反应，也即理解处理效应的异质性，那么我们就可以根据单位的个体特征给予最佳的处理。

我们还将这一目标与预测模型的目标进行了对比。我们正在重新思考估计任务，从预测 $Y$ 的原始形式转变为预测 $Y$ 随 $T$ 的变化率 $\frac{\delta y}{\delta t}$。

遗憾的是，如何为此构建模型并不明显。由于我们无法直接观察敏感度，很难建立一个能够预测它的模型。但线性回归来帮忙了。通过拟合一个预测 $Y$ 的回归模型，我们找到了一种方法来同时预测 $\frac{\delta y}{\delta t}$。我们还必须加入处理与特征的交互项。这样一来，我们的敏感度预测对每个顾客都不同。换句话说，我们实际上是在估计 $E[Y'(t) \,|\, X]$。然后利用这些敏感度预测，我们将单位分成对处理更敏感和不那么敏感的两类，最终帮助我们为每个分组决定合适的处理水平。

![img](./images/18/economists.png)

由此产生的一个自然问题是，我们是否可以用一种通用的机器学习模型替代线性回归，用它来预测敏感度？答案是可以的，但需要注意一些细节。本章使用了一个非常简单的 CATE 模型，因为我认为用线性回归更容易理解其背后的概念。不过别担心，在接下来的章节中我们会看到一些更复杂的模型。但在那之前，我需要先讲解一个非常重要的话题，即如何比较两个 CATE 模型并决定哪个更好。

## 参考说明

本章内容主要基于作者个人的经验总结，许多观点和方法源自实践中的体会。因此，无法提供严格意义上的学术参考文献。与正规科研工作不同，这些内容没有经过系统性的学术审查，也未经过同行评议与理论论证。读者或许会注意到，本文更多讨论的是“实践中有效的方法”，而非详细阐述其背后的理论基础。这种方法可以被视为“来自一线的经验科学”（a sort of science from the streets）。尽管如此，既然这些内容已公开发布，也非常欢迎批评与反馈——如果读者发现其中有明显错误或不妥之处，欢迎提出 issue，作者会尽力回应。


## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。


---

# 19 - 评估因果模型

在绝大多数关于因果性的资料中，研究者使用合成数据来检验他们的方法是否有效。就像我们在《When Prediction Fails》那一章中所做的那样，他们同时生成 $Y_{0i}$ 和 $Y_{1i}$ 的数据，以便检查他们的模型是否正确捕捉到了处理效应 $Y_{1i} - Y_{0i}$。这在学术研究中完全可以，但在现实世界中我们没有这样的奢侈。当在工业界应用这些技术时，我们会一次又一次地被要求证明为什么我们的模型更好、为什么它应该替换生产环境中的现有模型，或者为什么它不会惨败。这一点如此关键，以至于我难以理解为什么几乎看不到任何材料解释我们应该如何用真实数据评估因果推断模型。

其结果是，想要应用因果推断模型的数据科学家很难说服管理层信任他们。他们采取的方式是展示理论多么严谨，以及在训练模型时多么谨慎。不幸的是，在以训练/测试划分范式为常态的世界里，这样的方式是远远不够的。模型的质量必须基于比精美理论更为具体的东西。想想看，机器学习之所以取得巨大成功，正是因为预测模型的验证非常直接。看到预测与实际发生的情况相符令人安心。

然而，在因果推断中，要实现类似训练/测试范式并不明显。这是因为因果推断关注的是估计一个不可观察的量，$\frac{\delta y}{ \delta t}$。既然我们看不见它，我们怎么知道模型在估计它方面是否表现良好？请记住，这就好像每个实体都有一个内在的响应度，由从处理到结果的直线斜率表示，但我们无法测量它。

![img](./images/19/sneak.png)

这一点非常非常难以理解，我花了多年才找到一种接近答案的东西。它不是终极答案，但在实践中有效，并且具有那种具体性，我希望它能把因果推断带向类似于我们在机器学习中拥有的训练/测试范式。这一技巧是使用敏感度的聚合测度。即使你不能逐一估计敏感度，你也可以对一组单位估计它，这正是我们将在这里利用的。


```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
from toolz import curry

import statsmodels.formula.api as smf
import statsmodels.api as sm

from sklearn.ensemble import GradientBoostingRegressor

import warnings
warnings.filterwarnings("ignore")
```

在本章中，我们将使用非随机数据来估计我们的因果模型，并用随机数据来评估它。我们仍然讨论价格如何影响冰淇淋销量。正如我们将看到的，随机数据对于评估非常宝贵。然而在现实中，收集随机数据往往代价高昂（如果你知道某些价格并不好，只会让你亏钱，为什么还要随机定价呢？）。结果就是我们经常拥有大量处理**非随机**的数据，而随机数据则少之又少。如果有任何随机数据，由于使用非随机数据来评估模型极其棘手，我们通常会把这些随机数据留作评估用途。

如果你忘记了，下面是数据的样子。


```python
prices = pd.read_csv("./data/ice_cream_sales.csv") # loads non-random data
prices_rnd = pd.read_csv("./data/ice_cream_sales_rnd.csv") # loads random data
print(prices_rnd.shape)
prices.head()
```

为了有可比较的对象，让我们训练两个模型。第一个将是带有交互项的线性回归，这样就允许不同单位之间的敏感度变化。

$$
sales_i = \beta_0 + \beta_1 price_i + \pmb{\beta_2 X}_i + \pmb{\beta_3 X}_i \, price_i + e_i
$$

一旦我们拟合了这个模型，我们就能够做出敏感度预测

$$
\widehat{\frac{\delta sales}{ \delta price}} = \hat{\beta_1} + \pmb{\hat{\beta_3} X}_i
$$


```python
m1 = smf.ols("sales ~ price*cost + price*C(weekday) + price*temp", data=prices).fit()
```

第二个模型将是完全非参数的机器学习预测模型

$$
sales_i = G(X_i, price_i) + e_i
$$


```python
X = ["temp", "weekday", "cost", "price"]
y = "sales"

np.random.seed(1)
m2 = GradientBoostingRegressor()
m2.fit(prices[X], prices[y]);
```

为了确保模型没有严重过拟合，我们可以检查在训练数据和新的未见数据上的 $R^2$。（对于那些熟悉机器学习的人，请注意，由于存在概念漂移，性能下降是预期的。模型是在价格非随机的数据上训练的，但测试集只包含随机化价格。）


```python
print("Train Score:", m2.score(prices[X], prices[y]))
print("Test Score:", m2.score(prices_rnd[X], prices_rnd[y]))
```

在训练好我们的模型之后，我们将从回归模型中得到敏感度。同样，我们将采用数值近似

$$
\frac{\delta y(t)}{\delta t} \approx \frac{y(t+h) - y(t)}{h}
$$

我们的模型是在非随机数据上训练的。现在我们转向随机数据进行预测。为了方便，我们将机器学习模型的预测和因果模型的敏感度预测添加到一个单一的数据框 `prices_rnd_pred` 中。

此外，让我们还包括一个随机模型。这个模型的思想是它仅输出随机数作为预测。它显然不是很有用，但可以作为一个基准。当我们谈论新的评估方法时，我总喜欢想一想一个随机（无用）模型的表现。如果随机模型在评估指标上表现良好，这说明评估方法本身的有效性值得怀疑。


```python
def predict_sensitivity(model, price_df, h=0.01):
    return (model.predict(price_df.assign(price=price_df["price"]+h))
            - model.predict(price_df)) / h

np.random.seed(123)
prices_rnd_pred = prices_rnd.assign(**{
    "sensitivity_m_pred": predict_sensitivity(m1, prices_rnd), ## sensitivity model
    "pred_m_pred": m2.predict(prices_rnd[X]), ## predictive model
    "rand_m_pred": np.random.uniform(size=prices_rnd.shape[0]), ## random model
})

prices_rnd_pred.head()
```

## 按模型分段的敏感度

现在我们已经有了预测，需要评估它们有多好。请记住，我们无法观察敏感度，因此没有简单的真实值可以比较。相反，让我们回想一下我们期望从敏感度模型中获得什么。也许这能为我们提供关于如何评估它们的一些洞见。

构建处理敏感度模型的想法源于寻找哪些单位对处理更敏感、哪些单位对处理不那么敏感的需求。这源于个性化的愿望。也许某个营销活动只对某一个人口细分极为有效；也许折扣只对某类客户奏效。一个好的因果模型应该帮助我们发现哪些客户对拟议的处理反应更好，哪些更差。它们应能根据单位对处理的弹性或敏感度将其分开。在我们的冰淇淋例子中，该模型应当能够判断哪些天人们愿意在冰淇淋上花更多钱，或者说哪些天价格敏感度不那么负。

如果这是我们的目标，那么如果我们能以某种方式按敏感度高到低对单位排序将非常有用。既然我们有预测的敏感度，我们可以用该预测对单位进行排序，并希望它也按照真实敏感度排序。遗憾的是，我们不能在单个单位层面评估这种排序。但如果我们不需要这样做呢？如果我们转而评估由排序定义的群组呢？如果我们的处理是随机分配的（这就是随机性的作用所在），估计一组单位的敏感度就很容易。我们所需要的只是比较处理组和未处理组之间的结果。

为了更好地理解这一点，把情形想象成二元处理案例更容易。继续用定价的例子，不过现在处理是折扣。换句话说，价格要么高（未处理），要么低（处理）。我们在 Y 轴上绘制销售量，在 X 轴上绘制我们的每个模型，并用颜色表示价格。然后，我们可以在模型轴上将数据分成三个等大小的组。**如果处理是随机分配的**，我们可以很容易地为每个组估计平均处理效应 $E[Y|T=1] - E[Y|T=0]$。

![img](./images/19/ate_bins.png)

在图中，我们可以看到第一个模型在预测销售方面还不错（与销售高度相关），但它产生的分组具有大致相同的处理效应，如底部的图所示。三个分段中的两个具有相同的敏感度，只有最后一个不同，敏感度较低。

另一方面，第二个模型产生的每个组都有不同的因果效应。这表明该模型的确可以用于个性化。最后，随机模型产生的组具有完全相同的敏感度。这并不十分有用，但也是预料之中的。如果模型是随机的，它产生的每个分段都将是数据的随机代表性样本。所以其组内的敏感度应该和整个数据集的平均处理效应大致相同。

仅通过观察这些图，你就可以对哪个模型更好形成直观印象。敏感度看起来越有序、不同分段之间差异越大，模型越好。在这里，模型2可能优于模型1，而模型1又优于随机模型。

为了推广到连续处理的情况，我们可以使用单变量线性回归模型来估计敏感度：

$$
y_i = \beta_0 + \beta_1 t_i + e_i
$$

如果我们在某个组的样本上运行该模型，我们将估计该组内的敏感度。

根据简单线性回归理论，我们知道

$$
\hat{\beta_1}=\dfrac{\sum (t_i - \bar{t}) (y_i - \bar{y})}{\sum(t_i - \bar{t})^2}
$$

其中 $\bar{t}$ 是处理的样本均值，$\bar{y}$ 是结果的样本均值。下面是其在代码中的样子


```python
@curry
def sensitivity(data, y, t):
        # line coeficient for the one variable linear regression 
        return (np.sum((data[t] - data[t].mean())*(data[y] - data[y].mean())) /
                np.sum((data[t] - data[t].mean())**2))
```

现在让我们将这应用于我们的冰淇淋价格数据。为此，我们还需要一个函数将数据集划分为等大小的区间，并对每个区间应用敏感度函数。下面的代码可以处理这一点。


```python
def sensitivity_by_band(df, pred, y, t, bands=10):
    return (df
            .assign(**{f"{pred}_band":pd.qcut(df[pred], q=bands)}) # makes quantile partitions
            .groupby(f"{pred}_band")
            .apply(sensitivity(y=y, t=t))) # estimate the sensitivity on each partition
```

最后，让我们使用之前的预测绘制按区间划分的敏感度。这里，我们将使用每个模型来构建分区，然后在每个分区上估计敏感度。


```python
fig, axs = plt.subplots(1, 3, sharey=True, figsize=(10, 4))
for m, ax in zip(["sensitivity_m_pred", "pred_m_pred", "rand_m_pred"], axs):
    sensitivity_by_band(prices_rnd_pred, m, "sales", "price").plot.bar(ax=ax)

```

首先看随机模型（`rand_m`）。在它的每个分区中，估计的敏感度大致相同。从图上我们已经可以看到，它对个性化没有太大帮助，因为它无法区分高价格敏感日和低价格敏感日。接下来考虑预测模型 `pred_m`。这个模型实际上很有希望！它能构建一些敏感度高的组和一些敏感度低的组。这正是我们需要的。

最后，因果模型 `sensitivity_m` 看起来有些奇怪。它识别出一些极低敏感度的组，这里的“低”实际上意味着高价格敏感度（当我们提高价格时销售会大幅下降）。识别这些高价格敏感的日子对我们非常有用。如果我们知道它们是哪些日子，就可以避免在这类日子上调价。该因果模型也能识别出一些较不敏感的区域，因此它可以成功地区分高敏感和低敏感，但其排序并不如预测模型那样好。

那么我们应该如何决定？哪个更有用？预测模型还是因果模型？预测模型的排序更好，但因果模型可以更好地识别极端情况。按区间绘制敏感度是一个好的初步检查，但它无法精确回答哪个模型更好。我们需要转向更复杂的东西。

## 累积敏感度曲线

再次考虑将价格转换为二元处理的示例。我们从之前停下的地方开始，因此我们得到了按区间的处理敏感度。接下来我们可以做的是根据区间的敏感度对它们重新排序。也就是说，我们把最敏感的组放在第一位，第二敏感的放在第二位，如此类推。对于模型1和3，不需要重新排序，因为它们已经是按序排列的。对于模型2，我们需要反向排序。

一旦我们有了这些有序的分组，我们就可以构建所谓的累积敏感度曲线。我们首先计算第一组的敏感度；然后计算第一组和第二组的合并敏感度，以此类推，直到包含所有组。最终，我们将得到整个数据集的敏感度。下面是在示例中的样子。

![img](./images/19/cumm_elast.png)

注意，累积敏感度曲线的第一个区间只是该模型认为最敏感组的平均处理效应。同时，对于所有模型，累积敏感度最终都会收敛到同一点，即整个数据集的平均处理效应。

在数学上，我们可以将累积敏感度定义为到第 $k$ 个单位为止估计的敏感度：

$$
\widehat{y'(t)}_k = \hat{\beta_1}_k=\dfrac{\sum_{i=1}^k (t_i - \bar{t})(y_i - \bar{y})}{\sum_{i=1}^k (t_i - \bar{t})^2}
$$

为了绘制累积敏感度曲线，我们在数据集上迭代运行上述函数，以产生以下序列：

$$
(\widehat{y'(t)}_1, \widehat{y'(t)}_2, \widehat{y'(t)}_3,..., \widehat{y'(t)}_N)
$$

这一序列在模型评估方面非常有趣，因为我们可以对其做出偏好判断。首先，一个模型越好，满足

$\hat{y}'(t)_k > \hat{y}'(t)_{k+a}$

的程度越高，对于任意的 $k$ 和 $a>0$。用通俗的话说，如果一个模型善于对敏感度排序，那么在前 $k$ 个样本中观察到的敏感度应高于在前 $k+a$ 个样本中观察到的敏感度。换句话说，如果我看排名靠前的单位，它们应该比下面的单位有更高的敏感度。

其次，一个模型越好，满足

$\hat{y}'(t)_k - \hat{y}'(t)_{k+a}$

尽可能大的程度越高，对于任意 $k$ 和 $a>0$。直觉是，我们不仅希望前 $k$ 个单位的敏感度高于它们下面的单位，而且希望这种差异尽可能大。

为了使这更具体，下面用代码表示这一思想。


```python
def cumulative_sensitivity_curve(dataset, prediction, y, t, min_periods=30, steps=100):
    size = dataset.shape[0]
    
    # orders the dataset by the `prediction` column
    ordered_df = dataset.sort_values(prediction, ascending=False).reset_index(drop=True)
    
    # create a sequence of row numbers that will define our Ks
    # The last item is the sequence is all the rows (the size of the dataset)
    n_rows = list(range(min_periods, size, size // steps)) + [size]
    
    # cumulative computes the sensitivity. First for the top min_periods units.
    # then for the top (min_periods + step*1), then (min_periods + step*2) and so on
    return np.array([sensitivity(ordered_df.head(rows), y, t) for rows in n_rows])
```

关于这个函数，需要注意几点。它假设用于排序敏感度的指标存储在 `prediction` 参数所指定的列中。此外，第一个分组包含 `min_periods` 个单位，因此它可以与其他分组不同。原因是由于样本量小，曲线开头的敏感度可能太噪声。为了解决这个问题，我们可以选择一个足够大的初始分组。最后，`steps` 参数定义了每个后续分组中包含的额外单位数量。

有了这个函数，我们就可以根据每个模型产生的排序绘制累积敏感度曲线。


```python
plt.figure(figsize=(10,6))

for m in ["sensitivity_m_pred", "pred_m_pred", "rand_m_pred"]:
    cumu_sens = cumulative_sensitivity_curve(prices_rnd_pred, m, "sales", "price", min_periods=100, steps=100)
    x = np.array(range(len(cumu_sens)))
    plt.plot(x/x.max(), cumu_sens, label=m)

plt.hlines(sensitivity(prices_rnd_pred, "sales", "price"), 0, 1, linestyles="--", color="black", label="Avg. Sens.")
plt.xlabel("% of Top Sensitivity. Days")
plt.ylabel("Cumulative Sensitivity")
plt.title("Cumulative Sensitivity Curve")
plt.legend();
```

解释累积敏感度曲线可能有点挑战，不过我是这样理解的。再次说，考虑二元处理的情况可能更容易。曲线的 X 轴表示我们处理的样本比例。这里，我把这一轴规范化为数据集的比例，所以 0.4 表示我们正在处理 40% 的样本。Y 轴是我们在这么多样本上应期望的敏感度。因此，如果曲线在 40% 处的值为 -1，意味着排名前 40% 单位的敏感度是 -1。理想情况下，我们希望在最大的样本比例上具有最高的敏感度。于是，理想的曲线应从 Y 轴高处开始，并非常缓慢地下降到平均敏感度，表示我们可以处理很大比例的单位，同时仍保持高于平均的敏感度。

不用说，我们的任何模型都没有接近理想的敏感度曲线。随机模型 `rand_m` 在平均敏感度附近震荡，且从不远离它。这意味着该模型无法找到敏感度不同于平均值的组。至于预测模型 `pred_m`，它似乎以相反的顺序排列敏感度，因为曲线从低于平均敏感度开始。不仅如此，它还很快（大约在50%的样本处）收敛到平均敏感度。最后，因果模型 `sensitivity_m` 看起来更有趣。起初它有一种奇怪的行为，累积敏感度先远离平均值增加，但随后达到一个点，我们可以处理大约 75% 的单位，同时保持相当不错的敏感度（接近 0）。这很可能是因为该模型能够识别出非常低敏感度（高价格敏感度）的日子。因此，只要我们在这些日子不提价，我们就可以在大多数样本（约 75%）中提价，同时仍保持低价格敏感度。

在模型评估方面，累积敏感度曲线已经比单纯的按区间敏感度概念好得多。在这里，我们能够更精确地给出对模型的偏好陈述。但这仍然是一条难以理解的曲线。因此，我们可以做进一步的改进。


## 累积增益曲线

接下来的想法是在累积敏感度之上进行一种非常简单但强大的改进。我们将累积敏感度乘以样本比例。例如，如果在 40% 的位置上累积敏感度是 -0.5，那么在那里我们会得到 -0.2（-0.5 * 0.4）。然后，我们将其与随机模型产生的理论曲线进行比较。这条曲线实际上是从 0 到平均处理效应的一条直线。可以这样理解：随机模型的每一个累积敏感度点都是平均处理效应，因为模型只是产生数据的随机代表性分割。如果我们沿 (0,1) 线的每个点将平均处理效应乘以该点，我们就会得到一条从零到平均处理效应的直线。

![img](./images/19/cumm_gain.png)

一旦我们有了理论随机曲线，我们就可以将其作为基准，与其他模型进行比较。所有曲线都从同一点开始并结束于同一点。然而，模型越善于排序敏感度，其曲线在 0 到 1 之间的部分就会越偏离随机直线。例如，在上图中，模型 M2 比 M1 更好，因为它在接近平均处理效应的终点之前偏离得更多。对于熟悉 ROC 曲线的人来说，可以把累积增益看作因果模型的 ROC。

从数学上说，

$$
\widehat{F(t)}_k = \hat{\beta_1}_k \times \frac{k}{N} = \dfrac{\sum_{i=1}^k (t_i - \bar{t})(y_i - \bar{y})}{\sum_{i=1}^k (t_i - \bar{t})^2} \times \frac{k}{N}
$$

在代码中实现它，我们所需做的只是加入样本比例的归一化。


```python
def cumulative_gain(dataset, prediction, y, t, min_periods=30, steps=100):
    size = dataset.shape[0]
    ordered_df = dataset.sort_values(prediction, ascending=False).reset_index(drop=True)
    n_rows = list(range(min_periods, size, size // steps)) + [size]
    
    ## add (rows/size) as a normalizer. 
    return np.array([sensitivity(ordered_df.head(rows), y, t) * (rows/size) for rows in n_rows])
```

对于我们的冰淇淋数据，我们将得到以下曲线。


```python
plt.figure(figsize=(10,6))

for m in ["sensitivity_m_pred", "pred_m_pred", "rand_m_pred"]:
    cumu_gain = cumulative_gain(prices_rnd_pred, m, "sales", "price", min_periods=50, steps=100)
    x = np.array(range(len(cumu_gain)))
    plt.plot(x/x.max(), cumu_gain, label=m)
    
plt.plot([0, 1], [0, sensitivity(prices_rnd_pred, "sales", "price")], linestyle="--", label="Random Model", color="black")

plt.xlabel("% of Top Sensitivity Days")
plt.ylabel("Cumulative Gain")
plt.title("Cumulative Gain")
plt.legend();
```

现在很明显，因果模型（`sensitivity_m`）比另外两个模型好得多。它与随机曲线的偏离程度远大于 `rand_m` 和 `pred_m`。还要注意实际的随机模型是如何非常贴近理论随机模型的，它们之间的差异很可能只是随机噪声。

至此，我们涵盖了一些关于如何评估因果模型的很好的想法。仅此一点就意义重大。我们成功地评估了模型在排序敏感度方面的优劣，即便没有真实的敏感度可供比较。还有唯一缺失的一点是，在这些测量周围加上置信区间。毕竟，我们总不能这么野蛮吧？

![img](./images/19/uncivilised.png)

## 考虑方差

当我们处理敏感度曲线时，不考虑方差显得有些不对劲。尤其是因为所有这些曲线都使用了线性回归理论，因此在它们周围加入置信区间应该相当容易。

为此，我们首先创建一个函数，用于返回线性回归参数的置信区间。我这里使用的是简单线性回归的公式，但你完全可以按你喜欢的方式计算置信区间。

$$
s_{\hat\beta_1}=\sqrt{\frac{\sum_i\hat\epsilon_i^2}{(n-2)\sum_i(t_i-\bar t)^2}}
$$


```python
def sensitivity_ci(df, y, t, z=1.96):
    n = df.shape[0]
    t_bar = df[t].mean()
    beta1 = sensitivity(df, y, t)
    beta0 = df[y].mean() - beta1 * t_bar
    e = df[y] - (beta0 + beta1*df[t])
    se = np.sqrt(((1/(n-2))*np.sum(e**2))/np.sum((df[t]-t_bar)**2))
    return np.array([beta1 - z*se, beta1 + z*se])
```

通过对我们的 `cumulative_sensitivity_curve` 函数做一些小的修改，我们可以输出敏感度的置信区间。


```python
def cumulative_sensitivity_curve_ci(dataset, prediction, y, t, min_periods=30, steps=100):
    size = dataset.shape[0]
    ordered_df = dataset.sort_values(prediction, ascending=False).reset_index(drop=True)
    n_rows = list(range(min_periods, size, size // steps)) + [size]
    
    # just replacing a call to `sensitivity` by a call to `sensitivity_ci`
    return np.array([sensitivity_ci(ordered_df.head(rows), y, t)  for rows in n_rows])
```

最后，这里是因果模型的 95% 置信区间的累积敏感度曲线。


```python
plt.figure(figsize=(10,6))

cumu_gain_ci = cumulative_sensitivity_curve_ci(prices_rnd_pred, "sensitivity_m_pred", "sales", "price", min_periods=50, steps=200)
x = np.array(range(len(cumu_gain_ci)))
plt.plot(x/x.max(), cumu_gain_ci, color="C0")

plt.hlines(sensitivity(prices_rnd_pred, "sales", "price"), 0, 1, linestyles="--", color="black", label="Avg. Sens.")

plt.xlabel("% of Top Sens. Days")
plt.ylabel("Cumulative Sensitivity")
plt.title("Cumulative Sensitivity for sensitivity_m_pred with 95% CI")
plt.legend();
```

注意，随着我们累积更多的数据集，置信区间变得越来越小。这是因为样本量在增加。

至于累积增益曲线，得到其置信区间也同样简单。同样，我们只需将调用 `sensitivity` 函数替换为调用 `sensitivity_ci` 函数即可。


```python
def cumulative_gain_ci(dataset, prediction, y, t, min_periods=30, steps=100):
    size = dataset.shape[0]
    ordered_df = dataset.sort_values(prediction, ascending=False).reset_index(drop=True)
    n_rows = list(range(min_periods, size, size // steps)) + [size]
    return np.array([sensitivity_ci(ordered_df.head(rows), y, t) * (rows/size) for rows in n_rows])
```

下面是因果模型的情况。请注意，现在即便在曲线开始时样本量较小，置信区间也从较小值开始。这是因为归一化因子 $\frac{k}{N}$ 会缩小平均处理效应及其置信区间。由于这条曲线用于比较模型，这不应成为问题，因为这一缩小因子将同等地作用于所有被评估的模型。


```python
plt.figure(figsize=(10,6))

cumu_gain = cumulative_gain_ci(prices_rnd_pred, "sensitivity_m_pred", "sales", "price", min_periods=50, steps=200)
x = np.array(range(len(cumu_gain)))
plt.plot(x/x.max(), cumu_gain, color="C0")

plt.plot([0, 1], [0, sensitivity(prices_rnd_pred, "sales", "price")], linestyle="--", label="Random Model", color="black")

plt.xlabel("% of Top Sensitivity Days")
plt.ylabel("Cumulative Gain")
plt.title("Cumulative Gain for sensitivity_m_pred with 95% CI")
plt.legend();
```

## 关键观点

在这里我们看到了三种检查模型在排序敏感度方面优劣的方法。我们利用这些方法来比较具有因果目的的模型并进行选择。这是一件大事。即使看不到真正的敏感度，我们也设法评估了模型在识别不同敏感度群体方面的好坏！

在这里，我们高度依赖随机数据。我们在非随机数据上训练模型，但所有评估都是在处理已被随机化的样本上完成的。这是因为我们需要某种可信赖的方法来估计敏感度。如果没有随机数据，我们这里使用的简单公式将不起作用。我们现在很清楚，简单线性回归在存在混杂变量时会有遗漏变量偏差。

不过，如果我们能拿到一些随机数据，我们已经知道如何比较随机模型。在下一章中，我们将解决非随机数据的问题，但在此之前，我想在模型评估方面再说几句。

让我们重申可信模型评估的重要性。有了累积增益曲线，我们终于有了一个很好的方法来比较用于因果推断的模型。我们现在可以决定哪个模型更适合用于处理个性化。这是一个重大进步。你会发现，大多数关于因果推断的资料并没有提供一种好的模型评估方法。在我看来，这是让因果推断像机器学习一样流行所缺失的那一块。有了好的评估，我们可以让因果推断更接近已经在预测模型中如此有用的训练/测试范式。这是一个大胆的说法。我在说这话时很谨慎，但目前为止我还没有发现对它的有力批评。如果你有，请告知。

## 参考说明

我在本章中的一些想法来自 Pierre Gutierrez 和 Jean-Yves Gérardy 的文章《Causal Inference and Uplift Modeling: A Review of the Literature》。作者在文中解释了 Qini 曲线（Qini curve）的概念。如果你搜索这个术语，会发现它是一种用于提升建模（uplift modeling）的方法，而提升建模可以被看作是针对二元处理（binary treatment）情况下的因果推断。在这里，我借鉴了 Qini 曲线的思想，并将其推广到了连续处理变量（continuous treatment）的情形。我认为这里提出的方法适用于连续和二元处理两种情况，但话说回来，我从未在其他地方见过类似的做法，所以请读者自行斟酌。

我也强烈推荐阅读 Leo Breiman（2001）关于训练-测试范式的文章：《Statistical Modeling: The Two Cultures》。如果你想理解一个统计技术为何能取得成功，这篇文章是非常有价值的参考资料。

本章内容主要基于作者个人的经验总结，许多观点和方法源自实践中的体会。因此，无法提供严格意义上的学术参考文献。与正规科研工作不同，这些内容没有经过系统性的学术审查，也未经过同行评议与理论论证。读者或许会注意到，本文更多讨论的是“实践中有效的方法”，而非详细阐述其背后的理论基础。这种方法可以被视为“来自一线的经验科学”（a sort of science from the streets）。尽管如此，既然这些内容已公开发布，也非常欢迎批评与反馈——如果读者发现其中有明显错误或不妥之处，欢迎提出 issue，作者会尽力回应。

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。


---

# 20 - 即插即用估计器

到目前为止，我们已经讨论了在处理不是随机分配的情况下如何校正数据，这种情况会引入混杂偏差。这有助于解决因果推断中的识别问题。换句话说，一旦单位是可交换的，或者满足 $ Y(0), Y(1) \perp X$ 的条件，我们就可以识别处理效应。但这还远未结束。

识别意味着我们能够得到平均处理效应。也就是说，我们知道某个处理在平均意义下的效果。这固然有用，可以帮助我们决定是否推出某项处理。但我们想要的不止于此，我们希望知道是否存在对处理反应更好或更差的亚群体，以便只对那些能够受益的个体实施处理，从而制定更优的政策。

## 问题设定

让我们回顾感兴趣的设定。根据潜在结果，我们可以定义个体处理效应为两个潜在结果之差：

$
\tau_i = Y_i(1) − Y_i(0),
$

对于连续处理的情况，定义 $ \tau_i = \partial Y(t)$，其中 $t$ 为处理变量。当然，我们永远无法观察到个体处理效应，因为我们只能看到潜在结果中的一个：

$$
Y^{obs}_i(t)=
\begin{cases}
Y_i(1), & \text{if } t=1\\
Y_i(0), & \text{if } t=0
\end{cases}
$$

平均处理效应（ATE）定义为

$
\tau = E[Y_i(1) − Y_i(0)] = E[\tau_i]
$

条件平均处理效应（CATE）定义为

$
\tau(x) = E[Y_i(1) − Y_i(0)|X] = E[\tau_i|X]
$

在本书的第一部分，我们主要关注 ATE。现在我们关注的是 CATE。CATE 对于个性化决策非常有用。例如，如果某种药物是我们的处理 $t$，我们想知道哪类患者对该药物反应更敏感（较高的 CATE），以及是否存在某些患者的反应为负（CATE < 0）。

我们已经看到如何通过在线性回归模型中加入处理与特征之间的交互项来估计 CATE：

$
y_i = \beta_0 + \beta_1 t_i + \beta_2 X_i + \beta_3 t_i X_i + e_i.
$

如果估计该模型，我们可以得到 $\tau(x)$ 的估计：

$
\hat{\tau}(x) = \hat{\beta}_1 + \hat{\beta}_3 X_i
$

然而，线性模型仍然有一些缺陷，主要是对特征 $X$ 做出线性假设。注意在这个模型中 $\beta_2$ 并不重要。但如果特征 $X$ 与结果之间的关系不是线性的，那么因果参数 $\beta_1$ 和 $\beta_3$ 的估计将会偏离真实值。

如果我们能用更灵活的机器学习模型替代线性模型就好了。我们甚至可以将处理作为特征输入到机器学习模型，例如提升树或神经网络：

$
y_i = M(X_i, T_i) + e_i
$

但是从这种模型中我们无法直接得到处理效应的估计，因为该模型输出的是 $\hat{y}$ 的预测，而不是 $\hat{\tau(x)}$ 的预测。理想情况下，我们希望使用一种机器学习回归模型，它不是最小化结果的均方误差 (MSE)：

$
E[(Y_i - \hat{Y}_i)^2]
$

而是最小化处理效应的均方误差：

$
E[(\tau(x)_i - \hat{\tau}(x)_i)^2] = E[(Y_i(1) - Y_i(0) - \hat{\tau}(x)_i)^2].
$

然而，这样的目标函数是不可行的。问题在于 $\tau(x)_i$ 是不可观测的，因此我们无法直接针对它优化。这让我们陷入了困境……让我们尝试简化问题，也许能想到一些办法。

![img](./images/20/infeasible.png)


## 目标变换

假设我们的处理是二元的。例如，你是一家投资公司，正在测试发送金融教育邮件的效果。你希望邮件能使人们投资更多。假设你开展了一项随机实验，50% 的客户收到了邮件，另 50% 的客户没有收到。

下面有一个大胆的想法：通过处理变量来变换结果变量。

$
Y^*_i = 2 Y_i * T_i - 2 Y_i*(1-T_i)
$

因此，如果某个个体接受了处理，我们将其结果乘以 2；如果没有接受处理，则将结果乘以 -2。例如，某位客户投资了 2000 巴西雷亚尔并收到了邮件，变换后的目标是 4000；如果他没有收到邮件，则变换后的目标是 -4000。

这听起来有些奇怪，好像在说邮件的效果可能是负数，但请耐心看下去。通过一些数学推导我们会发现，这个变换后的目标的期望等于处理效应。这非常惊人，也就是说，通过这种看似古怪的变换，我们能够估计一些连观察都无法观察到的东西。

为了理解这一点，我们需要一些数学推导。由于随机分配，我们有 $T \perp Y(0), Y(1)$，也就是熟悉的无混淆性，这意味着 $E[T\,Y(t)] = E[T]\cdot E[Y(t)]$，即独立性的定义。

此外，我们知道

$
Y_i T_i = Y(1)_i T_i \text{ 且 }  Y_i (1-T_i) = Y(0)_i (1-T_i)
$

因为处理决定了哪个潜在结果被实现。有了这些，我们可以计算 $Y^*_i$ 的条件期望：

$
\begin{align}
E[Y^*_i|X_i=x] &= E[2 Y(1)_i * T_i - 2 Y(0)_i*(1-T_i)|X_i=x] \\
&= 2E[Y(1)_i * T_i | X_i=x] - 2E[Y(0)_i*(1-T_i)|X_i=x]\\
&= 2E[Y(1)_i| X_i=x] * E[ T_i | X_i=x] - 2E[Y(0)_i| X_i=x]*E[(1-T_i)|X_i=x] \\
&= 2E[Y(1)_i| X_i=x] * 0.5 - 2E[Y(0)_i| X_i=x]*0.5 \\
&= E[Y(1)_i| X_i=x] - E[Y(0)_i| X_i=x] \\
&= \tau(x)_i
\end{align}
$

因此，这个看似疯狂的想法最终产生了个体处理效应 $\tau(x)_i$ 的无偏估计。现在，我们可以用以下准则替换之前不可实现的优化准则：

$
E[(Y^*_i - \hat{\tau}(x)_i)^2] 
$

简单来说，只需用任何回归机器学习模型预测 $Y^*_i$，模型输出的就是处理效应的预测。

我们已经解决了简单的情况，那么更复杂的情况呢？例如处理分配不是五五开，甚至不是随机的？答案稍微复杂一些，但并不难。首先，如果没有随机分配，我们至少需要满足条件独立性 $T \perp Y(1), Y(0) | X$。也就是说，在控制 $X$ 后，处理可以视为随机。在这种情况下，我们将变换后的目标推广为

$
Y^*_i = Y_i * \dfrac{T_i - e(X_i)}{e(X_i)(1-e(X_i))}
$

其中 $e(X_i)$ 是倾向得分。如果处理不是 50% 的概率分配，而是按不同的概率 $p$ 随机分配，只需把上式中的倾向得分换成 $p$。如果处理并非随机分配，则必须使用倾向得分，无论是记录下来的还是估计得到的。

如果计算这个变换后的目标的期望，你会发现它同样等于处理效应。证明虽然有点繁琐，但如下所示（可以跳过）：

$
\begin{align}
E[Y^*_i|X_i=x] &= E\big[Y_i * \dfrac{T_i - e(X_i)}{e(X_i)(1-e(X_i))}|X_i=x\big] \\
&= E\big[Y_i T_i * \dfrac{T_i - e(X_i)}{e(X_i)(1-e(X_i))} + Y_i (1-T_i) * \dfrac{T_i - e(X_i)}{e(X_i)(1-e(X_i))}|X_i=x\big]\\
&= E\big[Y(1)_i * \dfrac{T_i(1 - e(X_i))}{e(X_i)(1-e(X_i))} | X_i=x\big] - E\big[Y(0)_i * \dfrac{(1-T_i)e(X_i)}{e(X_i)(1-e(X_i))}|X_i=x\big]\\
&= \dfrac{1}{e(X_i)} E[Y(1)_i * T_i|X_i=x] - \dfrac{1}{1-e(X_i)} E[Y(0)_i * (1-T_i)| X_i=x]\\
&= \dfrac{1}{e(X_i)} E[Y(1)_i|X_i=x] * E[T_i|X_i=x] - \dfrac{1}{1-e(X_i)} E[Y(0)_i|X_i=x] * E[(1-T_i)| X_i=x]\\
&= E[Y(1)_i|X_i=x] - E[Y(0)_i|X_i=x]\\
&= \tau(x)_i
\end{align}
$

像往常一样，我认为通过一个例子会更直观。再次考虑我们发送投资邮件，让人们多投资的例子。结果变量是一个二元变量（是否投资），记作 `converted`。


```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
from nb21 import cumulative_gain, elast
```

```python
email = pd.read_csv("./data/invest_email_rnd.csv")
email.head()
```

我们的目标是进行个性化决策。让我们专注于 email-1，希望只将它发送给那些会对其反应更好的客户。

换言之，我们希望估计 email-1 的条件平均处理效应：

$
E[Converted(1)_i - Converted(0)_i|X_i=x] = \tau(x)_i
$

这样我们就可以针对那些 CATE 较高、对邮件反应最好的客户。

不过首先，我们要将数据集划分为训练集和验证集。我们将在一部分数据上估计 $\tau(x)_i$，然后在另一部分数据上评估这些估计。


```python
from sklearn.model_selection import train_test_split

np.random.seed(123)
train, test = train_test_split(email, test_size=0.4)
print(train.shape, test.shape)
```

现在我们将应用刚才学到的目标变换。由于邮件是随机分配的（尽管比例不是五五开），我们不需要考虑倾向得分，它只是一个常数，等于处理的分配概率。

```python
y = "converted"
T = "em1"
X = ["age", "income", "insurance", "invested"]

ps = train[T].mean()

y_star_train = train[y] * (train[T] - ps)/(ps*(1-ps))
```

有了变换后的目标，我们可以选择任意机器学习回归算法来预测它。这里我们使用提升树（boosted trees）。

```python
from lightgbm import LGBMRegressor

np.random.seed(123)

cate_learner = LGBMRegressor(max_depth=3, min_child_samples=300, num_leaves=5, force_col_wise=True, verbose=-1)

cate_learner.fit(train[X], y_star_train);
```

现在这个模型可以估计 $\tau(x)_i$，也就是说，它输出的是 $\hat{\tau}(x)_i$。在测试集上做预测时，会发现一些个体的 CATE 高于其他个体。例如，客户 6958 的 CATE 为 0.1，意味着如果向该客户发送邮件，他购买投资产品的概率预测会增加 0.1。相比之下，客户 3903 的概率只增加 0.04。

```python
test_pred = test.assign(cate=cate_learner.predict(test[X]))
test_pred.head()
```

为了评估这个模型的效果，我们可以绘制训练集和测试集的累积增益曲线。

```python
gain_curve_test = cumulative_gain(test_pred, "cate", y="converted", t="em1")
gain_curve_train = cumulative_gain(train.assign(cate=cate_learner.predict(train[X])), "cate", y="converted", t="em1")
plt.plot(gain_curve_test, color="C0", label="Test")
plt.plot(gain_curve_train, color="C1", label="Train")
plt.plot([0, 100], [0, elast(test, "converted", "em1")], linestyle="--", color="black", label="Baseline")
plt.legend();
```

正如我们所见，在测试集上，这个即插即用估计器比随机策略要好。然而，它似乎存在严重的过拟合问题，因为训练集上的表现远好于测试集。

这实际上是这种目标变换方法最大的缺点之一。通过这种目标变换，你获得了极大的简便性，只需变换目标就可以使用任何机器学习估计器来预测异质处理效应。然而代价是估计的方差很大。原因在于，这个变换后的目标是个体处理效应的极其嘈杂的估计，噪声会传导到你的估计中。如果样本量不大，这将是一个大问题；但在百万级以上样本量的大数据应用中，这个问题会小一些。


## 连续处理的情况

![img](./images/20/second-estimator.png)

目标变换方法的另一个明显缺点是它只适用于离散或二元的处理。这在因果推断的文献中非常常见：大多数研究都针对二元处理，而很少涉及连续处理。这让我十分困扰，因为在实际行业中，连续处理无处不在，通常表现为需要优化的价格。因此，尽管我找不到关于连续处理的目标变换的任何资料，我还是提出了一个在实践中有效的方法。但请记住，这背后没有非常坚实的计量经济学理论支撑。

为了说明这一点，让我们回到冰淇淋销售的例子。当时我们要做的是估计需求对价格的弹性，从而更好地设置冰淇淋的价格以优化收入。请记得，数据集中的观测单位是一天，我们希望知道什么时候人们对价格上涨不那么敏感。还要记得价格在这个数据集是随机设定的，因此我们不用担心混杂偏差。


```python
prices_rnd = pd.read_csv("./data/ice_cream_sales_rnd.csv")
prices_rnd.head()
```

和之前一样，先将数据分成训练集和测试集。

```python
np.random.seed(123)
train, test = train_test_split(prices_rnd, test_size=0.3)
train.shape, test.shape
```

现在我们需要一点创造力。对于离散情况，条件平均处理效应表示在给定单位特征 $X$ 的情况下，从未接受处理到接受处理时结果的变化：

$
\tau(x) = E[Y_i(1) − Y_i(0)|X] = E[\tau_i|X]
$

用通俗的语言说，这就是估计处理对不同单位特征组合的影响，其中特征组合由特征 $X$ 定义。对于连续处理，我们没有这种开关。所有单位都接受了处理，只是强度不同。因此我们不能谈论给予处理与否，而要讨论增加处理的效应。换言之，我们想知道如果将处理量增加一些，结果会怎样变化。这相当于估计结果函数 $Y$ 对处理 $t$ 的偏导数。由于我们希望在每个组别（CATE，而非 ATE）的条件下了解这一点，我们要对特征 $X$ 进行条件：

$
\tau(x) = E[\partial Y_i(t)|X] = E[\tau_i|X]
$

我们如何估计这一点呢？首先考虑简单的情形，即结果对处理是线性的。假设你有两类天气：热天（黄色）和冷天（蓝色）。在冷天，人们对价格上涨更敏感。同时，随着价格上涨，需求呈线性下降。

![img](./images/20/linear-case.png)

在这种情况下，CATE 就是每条需求曲线的斜率。这些斜率告诉我们若价格增加任意金额，需求将下降多少。如果这种关系确实是线性的，我们可以分别在热天和冷天通过简单线性回归的系数来估计这些弹性。

$$
\hat{\tau(x)} = Cov(Y_i, T_i)/Var(T_i) = \dfrac{\sum(Y_i- \bar{Y})(T_i - \bar{T})}{\sum (T_i - \bar{T})^2}
$$

我们可以从这个估计量得到启发，思考如果将它应用到单个单位会怎么样。换句话说，如果我们在每一天都定义类似的东西，它看起来会是这样：


$
Y^*_i = (Y_i- \bar{Y})\dfrac{(T_i - \bar{T})}{\sigma^2_T}
$

通俗地说，就是将原始的目标减去其平均值，再乘以处理变量（同样要减去平均值），最后除以处理的方差。如此一来，我们就得到了连续处理情况下的目标变换。

![img](./images/20/genious.jpeg)

现在的问题是：这真的有效吗？事实上它确实有效，我们可以给出类似二元处理情形的证明。首先，令

$
V_i = \dfrac{(T_i - \bar{T})}{\sigma^2_T}
$

注意 $E[V_i|X_i=x]=0$，因为在随机分配下 $E[T_i|X_i=x]=\bar{T}$，也就是说每个 $X$ 区域都有 $E[T_i]=\bar{T}$。还要注意 $E[T_i V_i | X_i=x]=1$，因为 $E[T_i(T_i - \bar{T})|X_i=x] = E[(T_i - \bar{T})^2|X_i=x]$，这正是处理的方差。最后，在条件独立（随机分配下自然满足）情况下，有 $E[T_i e_i | X_i=x] = E[T_i | X_i=x] E[e_i | X_i=x]$。

为了证明该目标变换有效，我们需要记住，我们要估计的其实是局部线性模型的参数：

$
Y_i = \alpha + \beta T_i + e_i \mid X_i=x
$

在我们的例子中，这些线性模型对应于冷热天的需求曲线。这里我们关注的是 $\beta$ 参数，即条件弹性或 CATE。通过一些推导，我们可以证明：

$
\begin{align}
E[Y^*_i|X_i=x] &= E[(Y_i-\bar{Y})V_i | X_i=x] \\
&= E[(\alpha + \beta T_i + e_i - \bar{Y})V_i | X_i=x] \\
&= \alpha E[V_i | X_i=x] + \beta E[T_i V_i | X_i=x] + E[e_i V_i | X_i=x] \\
&= \beta + E[e_i V_i | X_i=x] \\
&= \beta = \tau(x)
\end{align}
$

请注意，这只适用于随机分配的处理。如果处理不是随机的，我们必须用一个模型 $M(X_i)$ 来估计 $E[T_i|X_i=x]$，用它替代 $\bar{T}$：

$
Y^*_i = (Y_i- \bar{Y})\dfrac{(T_i - M(T_i))}{(T_i - M(T_i))^2}
$

这样可以确保上式推导中第三行的 $\alpha E[V_i | X_i=x]$ 项消失，且 $E[T_i V_i | X_i=x]$ 等于 1。如果你只关心对单位按处理效应排序，而不需要准确的数量，那么不必令 $E[T_i V_i | X_i=x]$ 等于 1；换言之，如果你只想知道在哪些天需求对价格上涨更敏感，而不关心差异有多大，估计的 $\beta$ 是否缩放并不重要。在这种情况下，可以省略分母：

$
Y^*_i = (Y_i- \bar{Y})(T_i - M(T_i))
$

如果这些数学推导让你感到疲惫，不必担心，代码其实很简单。我们再次用上述公式对训练集的目标进行变换。这里处理是随机分配的，因而不需要训练一个预测价格的模型。我也省略了分母，因为此处我只关心对处理效应的排序。


```python
y_star_cont = (train["price"] - train["price"].mean()) * (train["sales"] - train["sales"].mean())
```

然后，和之前一样，我们用一个机器学习回归模型来预测这个变换后的目标。

```python
cate_learner = LGBMRegressor(max_depth=3, min_child_samples=300, num_leaves=5)

np.random.seed(123)
cate_learner.fit(train[["temp", "weekday", "cost"]], y_star_cont)

cate_test_transf_y = cate_learner.predict(test[["temp", "weekday", "cost"]])

test_pred = test.assign(cate=cate_test_transf_y)
test_pred.sample(5)
```

这一次，CATE 的解释不太直观。由于在目标变换中去掉了分母，所看到的 CATE 被 $Var(X)$ 缩放了。然而，这样的预测仍然可以很好地对处理效应进行排序。为了验证这一点，我们可以像之前一样使用累积增益曲线。

```python
gain_curve_test = cumulative_gain(test.assign(cate=cate_test_transf_y),
                                "cate", y="sales", t="price")

gain_curve_train = cumulative_gain(train.assign(cate=cate_learner.predict(train[["temp", "weekday", "cost"]])),
                                   "cate", y="sales", t="price")


plt.plot(gain_curve_test, label="Test")
plt.plot(gain_curve_train, label="Train")
plt.plot([0, 100], [0, elast(test, "sales", "price")], linestyle="--", color="black", label="Baseline")
plt.legend();
```

对于这份数据，使用变换后的目标的模型明显优于随机策略。此外，训练集和测试集的结果也比较接近，因此方差不是问题。这只是该数据集的特性；还记得在二元处理情形下情况并非如此，当时模型的表现并不理想。

### 非线性处理效应

说完了连续处理的情况，还有一个问题需要解决。我们假定处理效应是线性的，但这种假设往往不合理，处理效应通常会以某种形式呈现饱和。在我们的例子中，可以合理地认为需求在初始涨价时下降更快，随后下降速度减缓。

![img](./images/20/non-linear-case.png)

问题在于**弹性或处理效应会随处理水平而变化**。在我们的例子中，曲线开始阶段的处理效应更强，而随着价格升高，效应减小。假设你有两种天气：热天（黄色）和冷天（蓝色），我们希望通过因果模型区分它们。但在非线性情况下，如果我们在曲线的不同价格点观察，热天和冷天的弹性可能相同（见右图），这使得因果模型难以区分它们。

这个问题没有简单的解决办法，我必须承认仍在探索最优的方法。目前的做法是思考处理效应的函数形式，尝试使其线性化。例如，需求通常具有以下形式，其中更大的 $\alpha$ 表示价格每提高一个单位，需求下降得更快：

$
D_i = \dfrac{1}{P_i^{\alpha}}
$

因此，如果对需求 $Y$ 和价格 $T$ 同时做对数变换，就会得到近似线性的关系：

$
\begin{align}
\log(D)_i &= \log\bigg(\dfrac{1}{P_i^{\alpha}}\bigg) \\
&= \log(1) - \log(P_i^{\alpha}) \\
&= \log(1) - \log(P_i^{\alpha}) \\
&= - \alpha * \log(P_i) \\
\end{align}
$

线性化并不容易，需要仔细思考。但你也可以尝试不同的变换，看看哪种效果最好。通常，对数或平方根等变换会有帮助。

## 关键观点

我们正在迈向使用机器学习模型来估计条件平均处理效应。最大的挑战是如何将预测模型改造成估计因果效应的模型。换个角度看，预测模型关注的是估计结果 $Y$ 作为特征 $X$ 以及可能的处理 $T$ 的函数 $Y = M(X,T)$，而因果模型需要估计该函数对处理的偏导数 $\partial Y = \partial M(X,T)$。这并不容易，因为虽然我们能观察到结果 $Y$，却无法在个体层面观察到 $\partial Y$。因此在设计模型的目标函数时必须很有创造性。

这里我们看到了一个非常简单的目标变换技巧。其思想是将原始目标 $Y$ 与处理 $T$ 组合起来，形成一个在期望上等于 CATE 的变换后的目标。有了这个新目标，就可以将任何预测型机器学习模型用来估计它，模型输出的便是 CATE 的估计。顺便提一下，除了目标变换，这种方法也被称为 **F-Learner**。

然而这种简单性也有代价。变换后的目标是个体处理效应的一个噪声很大的估计，这种噪声会以方差形式传导到模型的估计中。因此，目标变换更适用于大数据场景，在大样本下方差问题较小。目标变换的另一个缺点是它只适用于二元或类别型处理。我们已经尽力提出了一个连续处理版本，并且似乎有效，但目前仍缺乏坚实的理论框架来支持它。

最后，我们讨论了非线性处理效应及其带来的挑战。当处理效应随处理水平而变化时，我们可能误以为不同个体具有相同的反应曲线，只是因为它们对处理的敏感度相同，而实际上它们接受的处理强度不同。

## 参考说明

本章的大部分内容借鉴自 Susan Athey 和 Guido W. Imbens 的论文《Machine Learning Methods for Estimating Heterogeneous Causal Effects》。关于目标变换（target transformation）的一些材料也可以在 Pierre Gutierrez 和 Jean-Yves Gérardy 的论文《Causal Inference and Uplift Modeling: A Review of the Literature》中找到。需要注意的是，这几篇论文只涉及二元处理（binary treatment）情形。另一篇回顾异质性处理效应（CATE）估计中因果模型的综述性文章是 Künzel 等人于 2019 年发表的《Meta-learners for Estimating Heterogeneous Treatment Effects using Machine Learning》，该文也提到了 F-Learner。


本章内容主要基于作者个人的经验总结，许多观点和方法源自实践中的体会。因此，无法提供严格意义上的学术参考文献。与正规科研工作不同，这些内容没有经过系统性的学术审查，也未经过同行评议与理论论证。读者或许会注意到，本文更多讨论的是“实践中有效的方法”，而非详细阐述其背后的理论基础。这种方法可以被视为“来自一线的经验科学”（a sort of science from the streets）。尽管如此，既然这些内容已公开发布，也非常欢迎批评与反馈——如果读者发现其中有明显错误或不妥之处，欢迎提出 issue，作者会尽力回应。


## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 21 - 元学习器

简单回顾一下，我们现在关注的是发现处理效应的异质性，也就是识别不同单位对处理的反应差异。在这个框架下，我们希望估计

$
\tau(x) = E[Y_i(1) − Y_i(0)|X] = E[\tau_i|X]
$

或者在连续处理的情况下，估计 $E[\delta Y_i(t)|X]$。换句话说，我们想知道各单位对处理的敏感程度。当我们不能对所有人都实施处理、必须对某些人进行优先排序时，这一点非常有用，例如当你想发放折扣但预算有限时。

之前，我们看到了如何通过变换结果变量 $Y$，将其输入预测模型，从而得到条件平均处理效应（CATE）的估计。那种做法的代价是增加了估计的方差。这在数据科学中很常见：不存在唯一最好的方法，每种方法都有其优点和缺点。因此，掌握多种技术并根据情境权衡使用是值得的。本章的精神就是为你提供更多工具。

![img](./images/21/learned-new-move.png)

元学习器是一种利用现成的预测性机器学习方法来解决同样问题——估计 CATE——的简单方式。同样地，没有一种方法是万能的，每一种都有其薄弱之处。我会尝试逐一介绍这些方法，但请记住，这些方法高度依赖于具体背景。此外，元学习器用到的预测性机器学习模型可以是线性回归、提升树、神经网络或高斯过程，成功与否很大程度上取决于所用的机器学习基学习器。通常你需要尝试多种不同的组合才能找到最佳方案。

```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
from nb21 import cumulative_gain, elast
```

这里，我们继续使用之前的投资广告邮件数据。目标仍然是弄清楚谁对邮件反应更好。不过这次有一个小变化：我们将用非随机的数据来训练模型，用随机的数据来验证模型。处理非随机数据要困难得多，因为元学习器既需要对数据进行去偏处理，又需要估计 CATE。

```python
test = pd.read_csv("./data/invest_email_rnd.csv")
train = pd.read_csv("./data/invest_email_biased.csv")
train.head()
```

我们的结果变量是 `conversion`，处理变量是 `email-1`。让我们创建一些变量来存储这二者，以及我们用来寻找处理效应异质性的特征 $X$。

```python
y = "converted"
T = "em1"
X = ["age", "income", "insurance", "invested"]
```

## S-学习器（又称 “Go-Horse” 学习器）

第一个使用的学习器是 S-学习器。这是我们能够想到的最简单的学习器。我们将使用一个单一的（因此称 S）机器学习模型 $M_s$ 来估计

$
\mu(x) = E[Y| T, X]
$

为此，只需在预测结果 $Y$ 的模型中将处理作为一个特征加入即可。

```note
虽然我将使用一个回归模型来估计 E[Y| T, X]，但由于结果变量是二元的，你也可以使用分类器。只需要调整代码，使模型输出概率而不是二元类别 0、1。
```


```python
from lightgbm import LGBMRegressor

np.random.seed(123)
s_learner = LGBMRegressor(max_depth=3, min_child_samples=30, force_col_wise=True, verbose=-1)
s_learner.fit(train[X+[T]], train[y]);
```

然后，我们可以在不同的处理状态下做出预测。预测值之间的差异就是我们的 CATE 估计

$
\hat{\tau}(x)_i = M_s(X_i, T=1) - M_s(X_i, T=0)
$

如果用图示表示，大致如下：

![img](./images/21/s-learner.png)

现在，让我们看看如何在代码中实现这种学习器。

```python
s_learner_cate_train = (s_learner.predict(train[X].assign(**{T: 1})) -
                        s_learner.predict(train[X].assign(**{T: 0})))

s_learner_cate_test = test.assign(
    cate=(s_learner.predict(test[X].assign(**{T: 1})) - # predict under treatment
          s_learner.predict(test[X].assign(**{T: 0}))) # predict under control
)
```

为了评估这个模型，我们将查看测试集上的累积增益曲线。我还绘制了训练集上的增益曲线。由于训练集存在偏差，这条曲线不能说明模型是否良好，但它可以提示我们是否在训练集上过拟合。当发生过拟合时，训练集的曲线会异常之高。如果想看看这种情况的样子，可以把 `max_depth` 参数从 3 调整到 20。

```python
gain_curve_test = cumulative_gain(s_learner_cate_test, "cate", y="converted", t="em1")
gain_curve_train = cumulative_gain(train.assign(cate=s_learner_cate_train), "cate", y="converted", t="em1")
plt.plot(gain_curve_test, color="C0", label="Test")
plt.plot(gain_curve_train, color="C1", label="Train")
plt.plot([0, 100], [0, elast(test, "converted", "em1")], linestyle="--", color="black", label="Baseline")
plt.legend()
plt.title("S-Learner");
```

![img](./images/21/not-great-not-terrible.jpeg)

从累积增益曲线可以看出，虽然简单，S-学习器在这个数据集上表现还可以。需要记住的是，这种表现高度依赖于数据集的特点。根据你手头数据的类型，S-学习器可能表现更好或更差。实践中，我发现 S-学习器由于简单，是任何因果问题的一个良好出发点。不仅如此，S-学习器既能处理连续处理，也能处理离散处理，而本章的其他学习器只能处理离散处理。

S-学习器的主要缺点是倾向于将处理效应偏向零。由于 S-学习器通常使用正则化的机器学习模型，正则化会限制估计的处理效应。Chernozhukov 等 (2016) 用模拟数据说明了这个问题：

![img](./images/21/zero-bias-s-learner.png)

他们绘制了真实因果效应（红色外框）与估计的因果效应之差 $\tau - \hat{\tau}$。可以看出估计的因果效应偏差很大。

更糟的是，如果处理变量相对于其他协变量对结果的影响非常弱，S-学习器可能完全丢弃处理变量。请注意，这与所选择的机器学习模型密切相关。正则化越强，这个问题越严重。下一节将介绍一种尝试解决这一问题的学习器。

## T-学习器

T-学习器试图通过强制学习器首先根据处理进行分割来解决完全忽略处理的问题。与使用单个模型不同，我们将为每个处理取值估计一个模型。在二元处理的情况下，我们只需要估计两个模型（因此名为 T）：

$
\mu_0(x) = E[Y| T=0, X]
$

$
\mu_1(x) = E[Y| T=1, X]
$

然后，在预测时，我们可以对每个处理水平进行反事实预测并得到 CATE：

$
\hat{\tau}(x)_i = M_1(X_i) - M_0(X_i)
$

下面是该学习器的示意图：

![img](./images/21/t-learner.png)

现在，关于理论就这些。让我们动手编写代码。

```python
np.random.seed(123)

m0 = LGBMRegressor(max_depth=2, min_child_samples=60, force_col_wise=True, verbose=-1)
m1 = LGBMRegressor(max_depth=2, min_child_samples=60, force_col_wise=True, verbose=-1)

m0.fit(train.query(f"{T}==0")[X], train.query(f"{T}==0")[y])
m1.fit(train.query(f"{T}==1")[X], train.query(f"{T}==1")[y])

# estimate the CATE
t_learner_cate_train = m1.predict(train[X]) - m0.predict(train[X])
t_learner_cate_test = test.assign(cate=m1.predict(test[X]) - m0.predict(test[X]))
```

```python
gain_curve_test = cumulative_gain(t_learner_cate_test, "cate", y="converted", t="em1")
gain_curve_train = cumulative_gain(train.assign(cate=t_learner_cate_train), "cate", y="converted", t="em1")
plt.plot(gain_curve_test, color="C0", label="Test")
plt.plot(gain_curve_train, color="C1", label="Train")
plt.plot([0, 100], [0, elast(test, "converted", "em1")], linestyle="--", color="black", label="Baseline")
plt.legend();
plt.title("T-Learner");
```

T-学习器在这个数据集上的表现也不错。测试集的表现与 S-学习器差别不大，可能是因为处理本身并不弱。同时我们看到训练集的表现远高于测试集，这表明模型过拟合。这是因为我们为每个处理水平只在子样本上拟合模型，数据量减少时模型可能学到噪声。

T-学习器避免了对弱处理变量视而不见的问题，但仍然可能受到正则化偏差的影响。考虑 Kunzel 等人 (2019) 中的以下情形：你有大量未处理的数据，却只有很少的处理过的数据，这是许多应用中的常见情况，因为处理往往很昂贵。假设结果 $Y$ 存在某些非线性，但**处理效应是常数**。可以在下图中看到会发生什么：

![img](./images/21/t-learner-problem.png)

在这里，由于处理观测很少，为了避免过拟合，$M_1$ 会非常简单（在图中是线性的）。$M_0$ 会更复杂，但由于数据充足，不会过拟合。这在机器学习的角度看都是合理的。然而，如果我们用这些模型计算 CATE $\hat{\tau} = M_1(X) - M_0(X)$，那么线性的 $M_1(X)$ 减去非线性的 $M_0(X)$ 会得到一个非线性的 CATE（蓝线减红线），这显然是错误的，因为此例中的真实 CATE 是常数并等于 1。

问题在于，未处理组的模型可以捕捉非线性，而处理组模型由于用正则化应对样本小而无法捕捉。如果减少正则化以捕捉非线性，那么样本小又会导致过拟合。这似乎进退维谷。为解决这个问题，我们可以使用 X-学习器，这正是 Kunzel 等人论文中提出的方法。

## X-学习器

X-学习器比前两个学习器解释起来复杂得多，但其实现其实很简单，所以不用担心。X-学习器有两个阶段，并会用到一个倾向得分模型。第一阶段与 T-学习器完全相同：我们将样本按是否接受处理分为两组，并为处理组和控制组分别拟合一个机器学习模型。

$
\hat{M}_0(X) \approx E[Y| T=0, X]
$

$
\hat{M}_1(X) \approx E[Y| T=1, X]
$

现在事情开始有点不同。第二阶段，我们使用上述模型来填补控制组和处理组的处理效应：

$
\hat{\tau}(X, T=0) = \hat{M}_1(X, T=0) - Y_{T=0}
$

$
\hat{\tau}(X, T=1) = Y_{T=1} - \hat{M}_0(X, T=1)
$

然后，我们再拟合两个模型来预测这些效应：

$
\hat{M}_{\tau 0}(X) \approx E[\hat{\tau}(X)|T=0]
$

$
\hat{M}_{\tau 1}(X) \approx E[\hat{\tau}(X)|T=1]
$

如果我们将其应用到前面显示的图像中，$\hat{\tau}(X, T=0)$，即对未处理单位填补的处理效应，是图中的红色叉号，红色虚线则表示 $\hat{M}_{\tau 0}(X)$。注意这个模型是错误的，因为 $\hat{\tau}(X, T=0)$ 是用简单的、正则化的模型 $\hat{M}_1$（在处理组上估计）得到的，它未能捕捉 $Y$ 中的非线性，导致估计的处理效应具有非线性。

相反，蓝点是对处理组填补的处理效应 $\hat{\tau}(X, T=1)$。这些效应是用正确的模型 $M_0$（在未处理的大样本上训练）估计的。因此，由于填补的处理效应是正确的，我们能够训练出正确的第二阶段模型 $\hat{M}_{\tau 1}(X)$，如蓝线所示。

![img](./images/21/second-stage-x.png)

因此我们有一个模型是错误的，因为我们填补的处理效应是错误的，另一个模型是正确的，因为我们填补的效应是正确的。现在，我们需要一种方法来组合这两个模型，使正确模型的权重更高。这就需要倾向得分模型。设 $\hat{e}(x)$ 为倾向得分模型，我们可以如下组合两个第二阶段模型：

$
\hat{\tau}(x) = \hat{M}_{\tau 0}(X)\hat{e}(x) + \hat{M}_{\tau 1}(X)(1 - \hat{e}(x))
$

由于处理单位很少，$\hat{e}(x)$ 很小。这会给错误模型 $\hat{M}_{\tau 0}(X)$ 极小的权重。

相反，$1 - \hat{e}(x)$ 接近 1，因此我们会给予正确模型 $\hat{M}_{\tau 1}(X)$ 较大的权重。更一般地，使用倾向得分的加权平均确保我们给予在处理赋值较为可能的区域训练得到的 CATE 模型更多权重。换句话说，我们会偏好使用更多数据训练出的模型。下图展示了 X-学习器和 T-学习器估计的 CATE。

![img](./images/21/t-vs-x-learner.png)

如图所示，与 T-学习器相比，X-学习器在非线性区域纠正错误的 CATE 估计方面效果更好。一般而言，当一个处理组远大于另一个时，X-学习器表现更佳。

我知道这听起来信息很多，但在实现时希望会更清晰。为了总结一切，下面是该学习器的示意图。

![img](./images/21/x-learner.png)

最后让我们看代码实现。首先是第一阶段，这与 T-学习器完全一样。

```python
from sklearn.linear_model import LogisticRegression

np.random.seed(123)

# first stage models
m0 = LGBMRegressor(max_depth=2, min_child_samples=30, force_col_wise=True, verbose=-1)
m1 = LGBMRegressor(max_depth=2, min_child_samples=30, force_col_wise=True, verbose=-1)

# propensity score model
g = LogisticRegression(solver="lbfgs", penalty=None) 

m0.fit(train.query(f"{T}==0")[X], train.query(f"{T}==0")[y])
m1.fit(train.query(f"{T}==1")[X], train.query(f"{T}==1")[y])
                       
g.fit(train[X], train[T]);
```

现在，我们根据填补的处理效应来拟合第二阶段模型。

```python
d_train = np.where(train[T]==0,
                   m1.predict(train[X]) - train[y],
                   train[y] - m0.predict(train[X]))

# second stage
mx0 = LGBMRegressor(max_depth=2, min_child_samples=30, force_col_wise=True, verbose=-1)
mx1 = LGBMRegressor(max_depth=2, min_child_samples=30, force_col_wise=True, verbose=-1)

mx0.fit(train.query(f"{T}==0")[X], d_train[train[T]==0])
mx1.fit(train.query(f"{T}==1")[X], d_train[train[T]==1]);
```

最后，我们使用倾向得分模型对预测进行校正。

```python
def ps_predict(df, t): 
    return g.predict_proba(df[X])[:, t]
    
    
x_cate_train = (ps_predict(train,1)*mx0.predict(train[X]) +
                ps_predict(train,0)*mx1.predict(train[X]))

x_cate_test = test.assign(cate=(ps_predict(test,1)*mx0.predict(test[X]) +
                                ps_predict(test,0)*mx1.predict(test[X])))
```

让我们看看 X-学习器在测试集上的表现。同样，我们绘制累积增益曲线。

```python
gain_curve_test = cumulative_gain(x_cate_test, "cate", y="converted", t="em1")
gain_curve_train = cumulative_gain(train.assign(cate=x_cate_train), "cate", y="converted", t="em1")
plt.plot(gain_curve_test, color="C0", label="Test")
plt.plot(gain_curve_train, color="C1", label="Train")
plt.plot([0, 100], [0, elast(test, "converted", "em1")], linestyle="--", color="black", label="Baseline")
plt.legend();
plt.title("X-Learner");
```

再一次，在这个数据集上我们的模型表现尚可。这里 S、T、X 学习器的表现似乎非常相似。不过，我仍然认为了解所有这些元学习器很有价值，这样你就可以选择最适合自己的方法。请记住，模型的表现也高度依赖于所选的基础机器学习模型。本章中我们使用的是梯度提升树，但也许其他模型或相同模型使用不同的超参数会更好。

## 关键观点

最简单的做法是使用单一模型的 S-学习器，将处理变量作为一个特征。这在处理变量不是弱预测因子时往往效果不错。但如果不是这种情况，S-学习器的估计会偏向零，甚至可能完全忽略处理变量。稍微复杂一些，我们可以使用 T-学习器，强制学习器分开拟合每个处理水平的模型。当每个处理水平都有足够样本时，这种方法很好用；但当某个处理水平样本很少时，模型会被迫强烈正则化而产生偏差。为了解决这个问题，我们可以再增加一个层次的复杂性，使用 X-学习器：它有两阶段拟合，并使用倾向得分模型来纠正由于样本过少导致的潜在错误。

这些学习器的一个大问题是（除了 S-学习器之外）都假设处理是二元或类别型的。还有一种更一般的学习器——R-学习器——我们尚未介绍。但不用担心，后面会有专门的一章讨论它。

## 参考说明

在撰写本章时，我主要参考了 Uber 的 `causalml` 库以及其关于元学习器（meta learners）的官方文档。同时，我还借用了大量来自 Künzel 等人（2019）发表的论文《Meta-learners for Estimating Heterogeneous Treatment Effects using Machine Learning》中的图片和概念。最后，关于 S-Learner 倾向于将效应估计偏向零的讨论，来源于 Chernozhukov 等人（2017）的论文《Double/Debiased Machine Learning for Treatment and Causal Parameters》。

本章内容主要基于作者个人的经验总结，许多观点和方法源自实践中的体会。因此，无法提供严格意义上的学术参考文献。与正规科研工作不同，这些内容没有经过系统性的学术审查，也未经过同行评议与理论论证。读者或许会注意到，本文更多讨论的是“实践中有效的方法”，而非详细阐述其背后的理论基础。这种方法可以被视为“来自一线的经验科学”（a sort of science from the streets）。尽管如此，既然这些内容已公开发布，也非常欢迎批评与反馈——如果读者发现其中有明显错误或不妥之处，欢迎提出 issue，作者会尽力回应。


## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 22 - 去偏/正交机器学习

我们接下来要介绍的这种元学习器其实早在“元学习器”这个称谓出现之前就被提出。据我所知，它源自一篇优秀的 2016 年论文，这篇论文为因果推断文献开辟了一个丰饶的领域。这篇论文名为《双重机器学习用于处理和因果参数》（Double Machine Learning for Treatment and Causal Parameters），作者阵容强大：Victor Chernozhukov、Denis Chetverikov、Mert Demirer、Esther Duflo（顺便说一下，她与 Abhijit Banerjee 和 Michael Kremer 因“通过实验方法缓解全球贫困”共同获得了 2019 年诺贝尔经济学奖）、Christian Hansen、Whitney Newey 和 James Robins。难怪这篇论文如此精彩，我甚至借用了 Paul Goldsmith-Pinkham 的创意，将作者们画成复仇者联盟，以示敬意。

![img](./images/22/avengers.png)

唯一的问题是，这篇论文非常难读（这很正常，因为它是计量经济学论文）。既然这本书旨在让因果推断走向大众，我们就来尝试让去偏/正交机器学习的思想变得直观易懂。

为什么值得单独用一章来讲述它，而不是把它和其他元学习器放在一起？吸引我注意的一点是这种去偏/正交机器学习的理论基础非常充分。我们之前见到的 T-learner、S-learner 和 X-learner 都有点类似巧妙的小技巧。我们可以给出它们为何有效的直观解释，但它们似乎不够一般。而去偏/正交机器学习则提供了一个既直观又严谨的通用框架，而且适用于连续和离散两类处理，这是 T-learner 和 X-learner 无法做到的。更不用说这些论文对这种估计量的渐近性质做了极好的分析。因此，让我们开始吧。

作为激励例子，我们再次回到冰淇淋销售数据。提醒一下，我们试图找到价格对销售影响的异质性。我们的测试集中的价格是随机分配的，但训练数据中的价格只是观测到的，可能带有偏差。

```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
from nb21 import cumulative_gain, elast
import statsmodels.formula.api as smf
from matplotlib import style
style.use("ggplot")
```

```python
test = pd.read_csv("./data/ice_cream_sales_rnd.csv")
train = pd.read_csv("./data/ice_cream_sales.csv")
train.head()
```

```python
np.random.seed(123)
sns.scatterplot(data=train.sample(1000), x="price", y="sales", hue="weekday")
```

一个明显的偏差来源很容易看出来。正如图中所示，周末（星期 1 和 7）的价格要高得多，但我们还可能有其他混杂因素，如温度和成本。因此，如果我们想做任何因果推断，就需要校正这种偏差。

## 对骚扰参数使用机器学习

一种尝试消除这种偏差的方法是使用线性模型来估计价格对销售的处理效应，同时控制混杂变量。

$$
Sales_i = \alpha + \tau \; price_i + \beta_1 \; temp_i + \beta_2 \; cost_i + \pmb{\beta_3} \; Weekday_i + e_i
$$

其中 $\pmb{\beta_3}$ 是与每个工作日虚拟变量相关联的参数向量。

注意我们只对参数 $\tau$ 感兴趣，因为那是我们的处理效应。其他参数称为干扰（nuisance）参数，因为我们并不关心它们。不过，事实证明，即使我们不关心它们，也必须正确估计它们，否则处理效应就会偏离真实值，这真是有点烦人。

例如，从直觉上看，`temp` 与销售之间的关系可能不是线性的。随着气温升高，人们会去海滩并购买更多冰淇淋，销售额会上升。但在某个温度之后太热了，人们选择呆在家里，这时销售会下降。`temp` 与销售的关系可能先上升后下降，也就是说，上面的模型可能是错的，它应该包括一个二次项：

$$
Sales_i = \alpha + \tau \; price_i + \beta_1 \; temp_i + \beta_2 \; temp_i^2 + \beta_3 \; cost_i + \pmb{\beta_4} \; Weekday_i + e_i
$$

![img](./images/22/non-linear.png)

思考如何为干扰参数建模已经很枯燥了，何况如果我们有几十甚至上百个协变量？在现代数据集中，这很常见。我们能做什么呢？答案在于计量经济学中最酷的定理之一。

### Frisch-Waugh-Lovell 定理

Frisch、Waugh 和 Lovell 是 20 世纪的计量经济学家，他们发现了线性回归中一个非常酷的性质。这对你来说并不陌生，我们在谈到回归残差和固定效应时已经提到过。但由于这个定理是理解正交机器学习的关键，非常值得回顾。

假设你有一个线性回归模型，包含一组特征 $X_1$ 和另一组特征 $X_2$。估计该模型的参数

$
\hat{Y} = \hat{\beta}_1 X_1 + \hat{\beta}_2 X_2
$

其中 $X_1$ 和 $X_2$ 是特征矩阵（每列是一个特征，每行是一条观测），$\hat{\beta}_1$ 和 $\hat{\beta}_2$ 是行向量。你可以通过以下步骤得到与直接回归相同的 $\hat{\beta}_1$：

1. 用第二组特征回归结果 $y$ 得到 $\hat{y}^* = \hat{\gamma}_1 X_2$。
2. 用第二组特征回归第一组特征 $X_1$ 得到 $\hat{X}_1 = \hat{\gamma}_2 X_2$。
3. 取残差 $\tilde{X}_1 = X_1 - \hat{X}_1$ 和 $\tilde{y}_1 = y - \hat{y}^*$。
4. 用特征残差回归结果残差 $\tilde{y}_1 = \hat{\beta}_1 \, \tilde{X}_1$。

这太酷了。这里有一个通用的表示，但注意其中一组特征可以只是处理变量。这意味着你可以单独估计所有干扰参数。先用特征回归结果得到结果残差，再用特征回归处理得到处理残差，最后用处理残差回归结果残差。这将给出与直接在结果上回归特征和处理得到的估计完全相同的 $\hat{\beta}_1$。

不要只是听我说。任何对因果推断感兴趣的人都应该至少做一次 FWL 定理练习。下面的例子中，我们首先估计协变量对结果（销售）和处理（价格）的影响，然后用 FWL 步骤估计处理效应。

```python
my = smf.ols("sales~temp+C(weekday)+cost", data=train).fit()
mt = smf.ols("price~temp+C(weekday)+cost", data=train).fit()
```

然后，利用这些残差，我们估计价格对销售的平均处理效应（ATE）。

```python
smf.ols("sales_res~price_res", 
        data=train.assign(sales_res=my.resid, # sales residuals
                          price_res=mt.resid) # price residuals
       ).fit().summary().tables[1]
```

我们估计出的 ATE 为 -4，这意味着价格每提高一个单位，销售将减少 4 单位。

现在，让我们在同一个模型中同时包含处理和协变量，估计同一个参数。

```python
smf.ols("sales~price+temp+C(weekday)+cost", data=train).fit().params["price"]
```

如你所见，得到的数字完全一致！这说明，无论是一次性估计处理效应，还是按照 FWL 步骤分开估计，从数学上来说是相同的。

换句话说，处理效应可以通过**残差回归**来得到，其中我们先将 $Y$ 对 $X$ 回归得到残差，再将残差回归于将 $T$ 对 $X$ 回归得到的残差。用符号表示为：

$
(Y - (Y \sim X)) \sim (T - (T \sim X))
$

这本质上是在下面的模型中估计因果参数 $\tau$：

$
Y_i - E[Y_i | X_i] = \tau \cdot (T_i - E[T_i | X_i]) + \epsilon_i
$

FWL 的妙处在于，它让我们可以将因果参数的估计过程与干扰参数的估计过程分离。但我们还没有回答最初的问题：怎样避免为干扰参数指定正确的函数形式？换句话说，如何只关注因果参数，而不必担心干扰参数？这正是机器学习发挥作用的地方。

### 加强版 FWL：Double/Debiased ML

Double/Debiased ML 可以看作是 FWL 定理的升级版。思路很简单：在构建结果和处理残差时使用机器学习模型：

$
Y_i - \hat{M}_y(X_i) = \tau \cdot (T_i - \hat{M}_t(X_i)) + \epsilon_i
$

其中 $\hat{M}_y(X_i)$ 估计 $E[Y|X]$，$\hat{M}_t(X_i)$ 估计 $E[T|X]$。由于 ML 模型非常灵活，它们可以捕捉 $Y$ 与 $T$ 对 $X$ 之间的复杂关系，同时保持 FWL 式的正交化。这意味着我们不必对协变量 $X$ 与结果 $Y$ 或处理之间的关系作任何参数假设，就能得到正确的处理效应。在不存在未观测混杂的情况下，我们可以通过以下正交化步骤恢复 ATE：

1. 使用灵活的 ML 回归模型 $M_y$ 用特征 $X$ 预测结果 $Y$。
2. 使用灵活的 ML 回归模型 $M_t$ 用特征 $X$ 预测处理 $T$。
3. 计算残差 $\tilde{Y} = Y - M_y(X)$ 和 $\tilde{T} = T - M_t(X)$。
4. 将结果残差回归于处理残差：$\tilde{Y} = \alpha + \tau \tilde{T}$。

这里的 $\tau$ 就是 ATE，可以用普通最小二乘等方法估计。

利用机器学习的强大之处在于灵活性，可以在干扰关系中捕捉复杂函数形式。但这种灵活性也带来麻烦，因为模型可能会过拟合。这就是我们需要引入交叉预测和折外残差的原因：我们将数据分成 K 份，在 K-1 份上训练模型，在剩下一份上预测残差，这样即便模型过拟合，也不会把残差压到零。最后合并所有部分的预测，用这些残差来估计最终的因果模型 $\tilde{Y} = \alpha + \tau \tilde{T}$。

接下来我们通过一步一步的实现来展示 Double/Debiased ML，在实践中解释每一步的作用。我们首先用 ML 模型估计干扰关系，从处理模型 $M_t$ 开始；我们将用 LGBM 模型通过 `temp`、`weekday` 和 `cost` 预测价格，这些预测将用 `sklearn` 的 `cross_val_predict` 函数得到折外预测。我还为可视化添加了平均值 $\hat{\mu_t}$ 到残差中。

```python
from lightgbm import LGBMRegressor
from sklearn.model_selection import cross_val_predict

y = "sales"
T = "price"
X = ["temp", "weekday", "cost"]

debias_m = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)

train_pred = train.assign(price_res =  train[T] -
                          cross_val_predict(debias_m, train[X], train[T], cv=5)
                          + train[T].mean()) # add mu_t for visualization. 
```

注意我将 $M_t$ 称为去偏模型，这是因为在 Double/Debias ML 中，它的作用是为处理去偏。残差 $\tilde{T} = T - M_t(X)$ 可以被看作是处理的一个版本，其中所有来自 $X$ 的混杂偏差都被模型剔除了。换句话说，$\tilde{T}$ 与 $X$ 正交。直观地讲，$\tilde{T}$ 不再能用 $X$ 解释，因为它已经被解释掉了。

为了看这一点，我们可以绘制与之前相同的图，但将价格替换为价格残差。还记得之前周末价格更高吗？现在这种偏差消失了。所有工作日的价格残差分布都相同。

```python
np.random.seed(123)
sns.scatterplot(data=train_pred.sample(1000), x="price_res", y="sales", hue="weekday");
```

$M_t$ 的作用是对处理去偏，那么 $M_y$ 的作用是什么呢？它的作用是消除 $Y$ 中的方差，因此我称之为降噪模型（denoising model）。直观地说，$M_y$ 创建了一个结果的版本，其中来自 $X$ 的所有方差都被解释掉了。结果是，在 $\tilde{Y}$ 中做因果估计更容易。由于噪声更少，因果关系更容易看清。

```python
denoise_m = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)

train_pred = train_pred.assign(sales_res =  train[y] -
                               cross_val_predict(denoise_m, train[X], train[y], cv=5)
                               + train[y].mean())
```

如果我们绘制与之前相同的图，但将销售额替换为销售残差，可以看到 $Y$ 的方差比之前小得多。

```python
np.random.seed(123)
sns.scatterplot(data=train_pred.sample(1000), x="price_res", y="sales_res", hue="weekday");
```

现在很容易看出价格与销售之间的负相关关系。

最后，为了估计这种因果关系，我们可以对残差运行回归。

```python
final_model = smf.ols(formula='sales_res ~ price_res', data=train_pred).fit()
final_model.summary().tables[1]
```

正如我们所看到的，当我们使用正交化后的销售和价格时，我们可以非常确信价格和销售之间是负相关的，这很合乎情理。随着价格上涨，冰淇淋的需求应该下降。

但如果看未经残差处理的原始价格与销售关系，由于偏差，我们会发现它们呈正相关。这是因为商家在预期销售高峰时提高价格。

```python
final_model = smf.ols(formula='sales ~ price', data=train_pred).fit()
final_model.summary().tables[1]
```

### 使用 Double-ML 估计 CATE

到目前为止，我们已经看到 Double/Debiased ML 使我们能够专注于估计平均处理效应 (ATE)。它同样可以用来估计处理效应异质性，即条件平均处理效应 (CATE)。本质上，我们现在认为因果参数 $\tau$ 会随着单位的协变量而变化。

$
Y_i - {M}_y(X_i)
= \tau(X_i) \cdot (T_i - {M}_t(X_i)) + \epsilon_i
$

为了估计这个模型，我们将使用同样的价格和销售残差，但现在我们将价格残差与其他协变量相互作用。然后，我们可以拟合一个线性的 CATE 模型

$
\tilde{Y}_i = \alpha + \beta_1 \tilde{T}_i + \pmb{\beta}_2 \pmb{X}_i \tilde{T}_i + \epsilon_i
$

估计完该模型后，为了预测 CATE，我们将使用随机的测试集。由于最终模型是线性的，我们可以机械地计算 CATE：

$
\hat{\mu}(\partial Sales_i, X_i) = M(Price=1, X_i) - M(Price=0, X_i)
$

其中 $M$ 是我们的最终线性模型。

```python
final_model_cate = smf.ols(formula='sales_res ~ price_res * (temp + C(weekday) + cost)', data=train_pred).fit()

cate_test = test.assign(cate=final_model_cate.predict(test.assign(price_res=1))
                        - final_model_cate.predict(test.assign(price_res=0)))
```

为了检验该模型在区分高价格敏感性和低价格敏感性单位方面表现如何，我们将使用累积弹性曲线。

```python
gain_curve_test = cumulative_gain(cate_test, "cate", y=y, t=T)
plt.plot(gain_curve_test, color="C0", label="Test")
plt.plot([0, 100], [0, elast(test, y, T)], linestyle="--", color="black", label="Baseline")
plt.legend();
plt.title("R-Learner");
```

使用最终线性模型的 Double/Debiased ML 程序已经非常不错，如上图所示。但也许我们能做得更好。事实上，这是一种非常通用的程序，我们可以像元学习器一样理解它。Nie 和 Wager 将其称为 R-Learner，以纪念 Robinson (1988) 的工作，并强调残差化的重要性。

这种推广来自于认识到 Double/Debiased ML 程序定义了一个新的损失函数，我们可以用任何方法来最小化它。接下来，我们将看到如何以类似于目标变换方法或 F-learner 的方式实现这一点。

## 非参数 Double/Debiased ML

Double-ML 的好处在于它让我们摆脱了在因果模型中学习干扰参数的烦恼。这样，我们可以将所有注意力集中在学习感兴趣的因果参数上，无论是 ATE 还是 CATE。然而，在上述设定下，我们在 ML 残差化之后仍然使用线性模型作为最终的因果模型。在我们的例子中，这意味着我们假设价格对销售的影响是线性的。这在价格变化范围较小时可能还好，但微观经济理论告诉我们情况不一定如此。例如，在低价区间，价格每升高一个单位需求可能下降 2 单位；而在高价区间，价格每升高一个单位需求可能只下降 1 单位。这显然不是线性关系。

我们可以利用微观经济理论来推测结果对处理的函数形式，但也许我们可以把这一任务交给机器学习模型。换句话说，让机器自己学习那种复杂的函数形式。事实证明，只要对原来的 Double/Debiased ML 算法做些调整，这完全可行。

首先，与之前完全相同，我们用 ML 模型和交叉预测对处理和结果进行正交化。

```python
y = "sales"
T = "price"
X = ["temp", "weekday", "cost"]

debias_m = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)
denoise_m = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)

train_pred = train.assign(price_res =  train[T] - cross_val_predict(debias_m, train[X], train[T], cv=5),
                          sales_res =  train[y] - cross_val_predict(denoise_m, train[X], train[y], cv=5))
```

到目前为止还没有什么不同之处。现在事情开始有趣了。回想一下 Double/Debiased-ML 将数据建模为：

$
Y_i = \hat{M}_y(X_i) + \tau(X_i)\big(T_i - \hat{M}_t(X_i)\big) + \hat{\epsilon}_i
$

其中 $\hat{M}_y$ 和 $\hat{M}_t$ 分别是用特征预测结果和处理的模型。若重新排列上述表达式，可以隔离误差项：

$
\hat{\epsilon}_i = \big(Y_i - \hat{M}_y(X_i)\big) - \tau(X_i)\big(T_i - \hat{M}_t(X_i)\big)
$

这非常棒，因为我们可以将其称为**因果损失函数**。这意味着，如果我们最小化该损失的平方，我们将估计 $\tau(X_i)$ 的期望，即 CATE。

$
\hat{L}_n(\tau(x)) = \frac{1}{n} \sum_{i=1}^n \Big( (Y_i - \hat{M}_y(X_i)) - \tau(X_i)(T_i - \hat{M}_t(X_i)) \Big)^2
$

这个损失也称为 **R-Loss**，这是 R-learner 最小化的目标。如何最小化它呢？有多种方法，这里我们介绍最简单的一种。为了简化符号，我们用残差化后的处理和结果来重写上述损失函数：

$
\hat{L}_n(\tau(x)) = \frac{1}{n} \sum_{i=1}^n \big( \tilde{Y}_i - \tau(X_i) \tilde{T}_i \big)^2
$

通过代数变换，可以把 $\tilde{T}_i$ 提到括号外，将 $\tau(X_i)$ 单独放在括号内部：

$$
\hat{L}_n(\tau(x)) = \frac{1}{n} \sum_{i=1}^n \tilde{T}_i^2 \left(\frac{\tilde{Y}_i}{\tilde{T}_i} - \tau(X_i)\right)^2
$$

最小化上述损失等价于最小化括号内的表达式，同时用 $\tilde{T}_i^2$ 作为权重。这一技巧称为加权技巧（weight trick），用以构建非参数因果损失。注意这与我们之前看到的目标变换思想非常相似——确实，这也是一种目标变换，但加入了权重。

总结一下，在得到了干扰模型和残差化的处理与结果后，我们将：
1. 构建权重 $\tilde{T}_i^2$；
2. 构建目标 $\tilde{Y}_i / \tilde{T}_i$；
3. 使用任意预测方法在权重下预测目标 (2)。

下面就是对应的代码。

```python
model_final = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)
 
# create the weights
w = train_pred["price_res"] ** 2 
 
# create the transformed target
y_star = (train_pred["sales_res"] / train_pred["price_res"])
 
# use a weighted regression ML model to predict the target with the weights.
model_final.fit(X=train[X], y=y_star, sample_weight=w);
```

The above ML model, even though it is an off-the-shelf predictive model, is estimating the CATE. That's the power of the non-parametric Double-ML. Before, we were using a linear regression as the final model for the CATE estimation. Now, since we defined a generic loss, we can use any predictive model at our disposal as the final model. 
 
Let's now use the test set to compare this non-parametric version with the linear version we had before. 
 
First, we estimate the individual treatment effect.

```python
cate_test_non_param = test.assign(cate=model_final.predict(test[X]))
```

Next, we can plot the non-parametric cumulative elasticity curve side by side with the one we got from the parametric  (linear) version of Double/Orthogonal-ML.

```python
gain_curve_test_non_param = cumulative_gain(cate_test_non_param, "cate", y=y, t=T)
plt.plot(gain_curve_test_non_param, color="C0", label="Non-Parametric")
plt.plot(gain_curve_test, color="C1", label="Parametric")
plt.plot([0, 100], [0, elast(test, y, T)], linestyle="--", color="black", label="Baseline")
plt.legend();
plt.title("R-Learner");
```

这里改进不大，但多少有点进步。此外，不必事先指定处理函数的形式本身就是一个巨大的优势。

### 什么是“非参数”？

在继续之前，我想强调一个常见的误解。当我们想到用非参数 Double-ML 模型来估计 CATE 时，好像意味着我们会得到非线性的处理效应。比如，假设一个非常简单的数据生成过程（DGP），其中折扣以平方根形式非线性地影响销售：

$
Sales_i = 20 + 10\sqrt{Discount_i} + e_i
$

处理效应是销售函数对处理的导数：

$
\dfrac{\partial Sales_i}{\partial Discount_i} = \frac{10}{2\sqrt{Discount_i}}
$

可以看到，处理效应并不是线性的。随着处理增加，效应实际上减弱。这对这个 DGP 很合理：一开始少量折扣会显著增加销售，但折扣给多了之后，额外的折扣对销售的影响会越来越小，因为人们不会无限购买，所以折扣只有在他们尚未满足之前才有效。

那么问题来了，非参数 ML 能否捕捉这种饱和行为的处理效应？能否从小的折扣水平外推得知，如果折扣更高，处理效应会更低？答案是……某种程度上可以。为了更好地理解这一点，我们生成与上述 DGP 类似的数据。

```python
np.random.seed(321)
n=5000
discount = np.random.gamma(2,10, n).reshape(-1,1)
discount.sort(axis=0) # for better ploting
sales = np.random.normal(20+10*np.sqrt(discount), 1)
```

如果我们绘制这个数据生成过程，可以看到变量之间的平方根关系。

```python
plt.plot(discount, 20 + 10*np.sqrt(discount))
plt.ylabel("Sales")
plt.xlabel("Discount");
```

现在，让我们把非参数 Double/Debias ML 应用于这份数据。

```python
debias_m = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)
denoise_m = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)

# orthogonalising step
discount_res =  discount.ravel() - cross_val_predict(debias_m, np.ones(discount.shape), discount.ravel(), cv=5)
sales_res =  sales.ravel() - cross_val_predict(denoise_m, np.ones(sales.shape), sales.ravel(), cv=5)

# final, non parametric causal model
non_param = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)
w = discount_res ** 2 
y_star = sales_res / discount_res

non_param.fit(X=discount_res.reshape(-1,1), y=y_star.ravel(), sample_weight=w.ravel());
```

利用上述模型，我们可以得到 CATE 估计。问题在于，CATE 不是线性的；随着处理增加，CATE 应当下降。我们想知道非参数模型是否能够捕捉这种非线性。

要正确回答这个问题，让我们回忆 Double/Debiased ML 对数据生成过程的基本假设。这些假设可由之前的等式表示：

$
\tilde{Y}_i = \tau(X_i) \tilde{T}_i + e_i
$

用语言来说，这意味着残差化后的结果等于残差化后的处理乘以条件处理效应。这说明**处理对结果的影响是线性的**。这里没有非线性。该模型暗示，当我们把处理从 1 增加到 10，或从 100 增加到 110，结果都会增加一个固定的量 $\tau(X_i)$，只是简单的乘法。

那这是否意味着非参数模型不能捕捉处理效应的非线性？也不是……实际上，Double/ML 所做的是**寻找非线性 CATE 的局部线性近似**。换句话说，它找到结果相对于处理在某个处理水平处的导数。这等价于找到在处理点附近与结果函数相切的直线的斜率。

![img](./images/22/linear-aprox.png)

这意味着，非参数 Double-ML 确实能够意识到随着处理增加，处理效应会降低，但它提供的是局部斜率而不是全局非线性函数。

```python
cate = non_param.predict(X=discount)

plt.figure(figsize=(15,5))
plt.subplot(1,2,1)
plt.scatter(discount, sales)
plt.plot(discount, 20 + 10*np.sqrt(discount), label="Ground Truth", c="C1")
plt.title("Sales by Discount")
plt.xlabel("Discount")
plt.legend()

plt.subplot(1,2,2)
plt.scatter(discount, cate, label="$\hat{\\tau}(x)$", c="C4")
plt.plot(discount, 5/np.sqrt(discount), label="Ground Truth", c="C2")
plt.title("CATE ($\partial$Sales) by Discount")
plt.xlabel("Discount")
plt.legend();
```

这听起来像是技术细节，但有非常实际的后果。例如，假设在上述例子中你发现某位客户的处理效应为 2，也就是说如果将折扣提高 1 个单位，该客户的销售额会增加 2 个单位。你可能会想：“太好了！我要给这个人更多折扣！毕竟每 1 单位折扣能带来 2 单位销售。”然而这是错误的结论。处理效应为 2 只是在那一折扣水平下的局部结果。一旦提高折扣，效应就会下降。例如，这个假想客户只有 5 的折扣，因此处理效应很高。如果你看到这个巨大的处理效应并用它来证明应该给这个客户 20 的折扣，那么当你这么做时，效应可能从 2 降至 0.5。以 2 的处理效应计算看似合理的 20 的折扣，在处理效应只有 0.5 时可能不再盈利。

这意味着在将非线性的处理效应外推到新的处理水平时必须格外小心。否则你可能做出非常不划算的决定。换句话说，当处理效应不是线性时，即使是非参数 Double/Debiased-ML 也**难以进行反事实结果预测**。它会试图将低处理水平下的处理效应线性外推到高处理水平，反之亦然，由于非线性，这种外推可能会偏离实际。

为解决这个问题，还有一个最后的思路……

```python
from sklearn.model_selection import KFold

def cv_estimate(train_data, n_splits, model, model_params, X, y):
    cv = KFold(n_splits=n_splits)    
    models = []
    cv_pred = pd.Series(np.nan, index=train_data.index)
    for train, test in cv.split(train_data):
        m = model(**model_params)
        m.fit(train_data[X].iloc[train], train_data[y].iloc[train])
        cv_pred.iloc[test] = m.predict(train_data[X].iloc[test])
        models += [m]
    
    return cv_pred, models

```

现在我们有了自己的交叉预测函数，并且得到了模型，我们可以继续执行正交化步骤。

```python
y = "sales"
T = "price"
X = ["temp", "weekday", "cost"]

debias_m = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)
denoise_m = LGBMRegressor(max_depth=3, force_col_wise=True, verbose=-1)

y_hat, models_y = cv_estimate(train, 5, LGBMRegressor, dict(max_depth=3, force_col_wise=True, verbose=-1), X, y)
t_hat, models_t = cv_estimate(train, 5, LGBMRegressor, dict(max_depth=3, force_col_wise=True, verbose=-1), X, T)

y_res = train[y] - y_hat
t_res = train[T] - t_hat
```

正交化之后，我们将 $\tilde{T}$ 与 $X$ 一起输入机器学习模型，用它来预测 $\tilde{Y}$。这里我使用的是 LGBM 模型，但你可以选择任何机器学习模型。LGBM 的一个好处是可以设置单调约束。基于我们对价格的了解，销售应随着价格的上升而下降。我们可以将这一点纳入约束，让 LGBM 模型在价格上升时**不要增加**其预测值。

```python
# -1 on price saying that the predictions should not increase as price increases
monotone_constraints = [-1 if col == T else 0 for col in X+[T]]
 
model_final = LGBMRegressor(max_depth=3, monotone_constraints=monotone_constraints, force_col_wise=True, verbose=-1)
model_final = model_final.fit(X=train[X].assign(**{T: t_res}), y=y_res)
```

现在事情有点奇怪。如果仔细想想，这个最终的 ML 模型实际上是在估计以下的 $\tau$ 函数：

$
\tilde{Y}_i = \tau(X_i, \tilde{T}_i) + e_i
$

但从这个函数中并没有明确的方式提取处理效应。因此，我们不再尝试直接提取处理效应，而是像前面图示那样输入反事实预测。我们将为每个单位模拟不同的价格水平，并使用 Double-ML 模型预测在这些不同价格水平下的销售量。

为此，我们将 1) 将测试集与包含所有模拟价格的价格表进行笛卡尔连接，得到如下结果……

```python
pred_test = (test
             .rename(columns={"price":"factual_price"})
             .assign(jk = 1)
             .reset_index() # create day ID
             .merge(pd.DataFrame(dict(jk=1, price=np.linspace(3, 10, 9))), on="jk")
             .drop(columns=["jk"]))

pred_test.query("index==0")
```

注意我们只展示了索引为 1 的那一天，即单个单位。在那一天，该单位的实际价格是 7。但我们模拟了从 3 到 10 的不同反事实价格。现在，我们将把所有这些反事实价格输入因果模型，该模型会根据模拟的价格给出反事实销售预测。

由于我们的模型形式为：

$
\widehat{Price_i} = \hat{\tau}(X_i, \tilde{T}_i)
$

在进行反事实预测之前，我们需要得到 $\tilde{T}_i$，即价格残差。我们将首先用所有的处理模型进行预测（记住训练时我们用了 5 折交叉预测），然后将五个模型的预测平均为一个单一预测，最后用这个集成模型的预测减去我们之前生成的反事实价格，以获得残差。

```python
def ensamble_pred(df, models, X):
    return np.mean([m.predict(df[X]) for m in models], axis=0)

t_res_test = pred_test[T] - ensamble_pred(pred_test, models_t, X)

pred_test[f"{y}_pred"] = model_final.predict(X=pred_test[X].assign(**{T: t_res_test}))

pred_test.query("index==0")
```

可以看到，我们现在对每个模拟价格都有销售预测。价格越低，销售越高。有趣的是，这些预测的水平偏离实际，例如从约 24 到约 -24。这是因为模型预测的是残差化后的结果，其均值约为零。如果你只想得到销售曲线的斜率，即价格的处理效应，这没有问题。另外，如果你想修正预测水平，只需将降噪模型 $M_y$ 的预测加回来即可。

```python
y_hat_test = ensamble_pred(pred_test, models_y, X)
pred_test[f"{y}_pred"] = (y_hat_test + 
                          model_final.predict(X=pred_test[X].assign(**{T: t_res_test})))

pred_test.query("index==0")
```

我们还可以绘制单位级别的销售曲线。让我们抽样十个单位，看看它们在不同价格下的表现。

```python
np.random.seed(1)
sample_ids = np.random.choice(pred_test["index"].unique(), 10)

sns.lineplot(data=pred_test.query("index in @sample_ids"),
             x="price", y="sales_pred", hue="index");
```

有趣的是，一些单位对价格上涨非常敏感。在某些情况下，当价格从 3 增加到 10 时，我们预测销售会从 250 降至将近 200。另一方面，一些单位的价格弹性非常小：当价格从 3 增至 10 时，我们预计销售仅从大约 195 降到大约 185。

这些价格敏感性的差异很难直接看出，所以我喜欢做的是让所有曲线从同一个起点（此处为平均销售量）开始。这能让我们更容易看到一些单位在价格提高时销售急剧下降，而其他单位下降不那么明显。

```python
np.random.seed(1)
sample_ids = np.random.choice(pred_test["index"].unique(), 10)

sns.lineplot(data=(pred_test
                   .query("index in @sample_ids")
                   .assign(max_sales = lambda d: d.groupby("index")[["sales_pred"]].transform("max"))
                   .assign(sales_pred = lambda d: d["sales_pred"] - d["max_sales"] + d["sales_pred"].mean())),
             x="price", y="sales_pred", hue="index");
```

### 可能需要更多计量经济学！

![img](./images/22/more-metrics.png)

我想以一则提醒结束这一“非科学”的 Double-ML 部分。我称这种方法为非科学并非无故。它有点像一种获取非线性反事实预测的巧妙办法。既然是个巧妙的方法，我觉得有必要谈谈它的潜在弊端。

首先，它有所有机器学习方法在因果推断中未经调整时面临的同样问题：偏差。由于最终模型是一个正则化的 ML 模型，这种正则化可能会把因果估计偏向零。

第二个问题与您选择的 ML 算法有关。这里我们选择的是提升树。树不擅长生成平滑的预测，因此预测曲线可能会出现不连续的跳变，这在上面的图中可以看到：某些地方出现阶梯状行为。此外，树不擅长外推，因此该模型可能会对训练集中未出现过的价格输出奇怪的预测值。

```python
pred_test = (test
             .rename(columns={"price":"factual_price"})
             .assign(jk = 1)
             .reset_index() # create day ID
             .merge(pd.DataFrame(dict(jk=1, price=np.linspace(3, 30, 30))), on="jk")
             .drop(columns=["jk"]))

t_res_test = pred_test[T] - ensamble_pred(pred_test, models_t, X)

y_hat_test = ensamble_pred(pred_test, models_y, X)
pred_test[f"{y}_pred"] = model_final.predict(X=pred_test[X].assign(**{T: t_res_test})) + y_hat_test

np.random.seed(1)
sample_ids = np.random.choice(pred_test["index"].unique(), 10)

sns.lineplot(data=(pred_test
                   .query("index in @sample_ids")
                   .assign(max_sales = lambda d: d.groupby("index")[["sales_pred"]].transform("max"))
                   .assign(sales_pred = lambda d: d["sales_pred"] - d["max_sales"] + d["sales_pred"].mean())),
             x="price", y="sales_pred", hue="index");
```

总结来说，这种方法高度依赖于最终的 ML 模型。如果正则化太强，因果估计将会偏向零；使用某个具体的 ML 算法，也会将其所有局限性带入最终的反事实预测。尽管如此，如果你认为这种方法值得尝试，不妨一试！只要别忘了我在这里指出的缺点。

## 关键观点

Double/Debiased/Orthogonal ML 提供了一种估计干扰参数的方法，使我们能够将注意力集中在感兴趣的因果参数上。它首先采用两步正交化过程：

1. 拟合一个模型 $M_t(X)$ 用协变量 $X$ 预测处理 $T$，并获得折外残差 $\tilde{t} = t - M_t(X)$。我们称之为去偏模型，因为残差 $\tilde{t}$ 按定义与用于构建它的特征正交。
2. 拟合一个模型 $M_y(X)$ 用协变量 $X$ 预测结果 $Y$，并获得折外残差 $\tilde{y} = y - M_y(X)$。我们称之为降噪模型，因为残差 $\tilde{y}$ 可以看作是消除了特征方差的结果版本。

一旦得到了这些残差，在不存在未观察混杂的情况下，我们可以将 $\tilde{y}$ 回归于 $\tilde{t}$ 得到 ATE 的线性近似。我们也可以将 $\tilde{t}$ 与协变量相互作用来估计 CATE，或使用加权技巧让任何通用 ML 模型成为最终的 CATE 模型。

![img](./images/22/diagram.png)

最后，我认为正交化步骤是一种促进因果学习的通用工具。本着这种精神，我们尝试将处理和结果残差输入类似 S-learner 的 ML 算法。这样，我们便可以从模拟的处理中获取反事实预测。的确，正交机器学习在许多因果推断应用中充当预处理步骤。

## 参考说明

本章内容主要基于作者个人的经验总结，许多观点和方法源自实践中的体会。因此，无法提供严格意义上的学术参考文献。与正规科研工作不同，这些内容没有经过系统性的学术审查，也未经过同行评议与理论论证。读者或许会注意到，本文更多讨论的是“实践中有效的方法”，而非详细阐述其背后的理论基础。这种方法可以被视为“来自一线的经验科学”（a sort of science from the streets）。尽管如此，既然这些内容已公开发布，也非常欢迎批评与反馈——如果读者发现其中有明显错误或不妥之处，欢迎提出 issue，作者会尽力回应。

撰写本章时，作者参考了 Chernozhukov 等 (2016) 《Double/Debiased Machine Learning for Treatment and Causal Parameters》、D. Foster 和 V. Syrgkanis (2019) 《Orthogonal Statistical Learning》以及 *econml* 库的文档。正交 ML 近期备受关注，因此有许多其他参考文献。例如，Nie 和 Wager (2020) 的草稿中对 R-loss 进行了讨论，Athey 等 (2019) 在因果决策树的背景下讨论了它，Chernozhukov 及其合作者还有许多后续论文进一步发展了该主题。

另外，作者从 [Pedro Sant'Anna 的幻灯片](https://pedrohcgs.github.io/files/Callaway_SantAnna_2020_slides.pdf) 中借用了一张图。

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 23 - 处理效应异质性和非线性的挑战

由于缺乏真实的反事实，预测单个单位的处理效应极其困难。因为我们只能观察到一个潜在结果 $T(t)$，无法直接估计个体处理效应。相反，我们只能依靠目标变换（也可视为巧妙设计的损失函数）来在期望意义下估计条件处理效应。然而，这并不是唯一的挑战。由于处理效应本身非常难以捉摸，其估计量通常非常嘈杂。这对需要根据处理效应对个体进行分段的应用有很大的现实影响，比如我们希望进行个性化处理分配时。

我们接下来会看到，在某些情况下，如果我们不直接去估计 CATE，而是关注另一个方差更小的代理目标，我们可以获得更好的处理效应分段。这种情况常见于结局变量 $Y$ 为二元变量时。

## 二元结局上的处理效应


```python
import pandas as pd
import numpy as np
import seaborn as sns
import statsmodels.formula.api as smf
from matplotlib import pyplot as plt
from matplotlib import style
style.use("ggplot")

```

```python
from typing import List

import numpy as np
import pandas as pd
from toolz import curry, partial

@curry
def avg_treatment_effect(df, treatment, outcome):
    return df.loc[df[treatment] == 1][outcome].mean() - df.loc[df[treatment] == 0][outcome].mean()
    
    

@curry
def cumulative_effect_curve(df: pd.DataFrame,
                            treatment: str,
                            outcome: str,
                            prediction: str,
                            min_rows: int = 30,
                            steps: int = 100,
                            effect_fn = avg_treatment_effect) -> np.ndarray:
    
    size = df.shape[0]
    ordered_df = df.sort_values(prediction, ascending=False).reset_index(drop=True)
    n_rows = list(range(min_rows, size, size // steps)) + [size]
    return np.array([effect_fn(ordered_df.head(rows), treatment, outcome) for rows in n_rows])


@curry
def cumulative_gain_curve(df: pd.DataFrame,
                          treatment: str,
                          outcome: str,
                          prediction: str,
                          min_rows: int = 30,
                          steps: int = 100,
                          effect_fn = avg_treatment_effect) -> np.ndarray:
    

    size = df.shape[0]
    n_rows = list(range(min_rows, size, size // steps)) + [size]

    cum_effect = cumulative_effect_curve(df=df, treatment=treatment, outcome=outcome, prediction=prediction,
                                         min_rows=min_rows, steps=steps, effect_fn=effect_fn)

    return np.array([effect * (rows / size) for rows, effect in zip(n_rows, cum_effect)])

```

这里有一个你在科技公司工作时极有可能遇到的常见问题：管理层希望通过某种“助推”来提高客户对产品的转化率。例如，他们可能通过向客户提供 10 巴西雷亚尔的代金券来促进应用安装后的内购，或在客户第一次使用叫车应用时提供一次免费乘车，或者在投资平台的前 3 个月降低交易费用。由于助推通常成本较高，他们不愿意对每个人都这样做。更理想的是，我们能把助推仅用于那些对其最敏感的客户。

用因果推断的术语来说，这类业务问题属于“处理效应异质性”（TEH）范畴。具体地，你有一个昂贵的助推作为处理 $T$，转化率作为二元结局 $Y$，以及客户特定的处理前特征 $X$。你可以像使用 Double/Debiased ML 那样估计条件平均处理效应 $E[Y_1 - Y_0 \mid X]$（如果处理是连续的，则是 $E[Y'(T)\mid X]$），最终只对估计的处理效应最高的客户实施助推。用业务术语说，你就是在个性化你的转化策略，找到那些转化增量高的客户群体，并只在他们身上使用助推。

然而，这里有一个使 TEH 方法不那么显而易见的复杂因素——结局是二元的。这大大增加了难度。因为这一点有些违反直觉，所以我先展示会发生什么，再解释其原因。

## 模拟一些数据

让我们保持简单，但仍然贴近实际。我们将模拟完全随机的处理 `nudge`，它服从参数 $p=0.5$ 的伯努利分布。这意味着助推的分配就像抛硬币一样，而且不存在需要关注的混杂因素。

$ nudge \sim \mathcal{B}(0.5) $

接下来，我们模拟客户的协变量 `age` 和 `income`，它们分别服从伽马分布。这些是你对客户已知的信息，因此你希望根据这些变量进行个性化。换言之，你希望根据年龄和收入将客户分组，使得其中一组对 `nudge` 处理高度响应。

$ age \sim G(10, 4) $

$ income \sim G(20, 2) $

最后，我们模拟转化率。为此，我们首先**构建一个服从线性模型的潜在变量**并加入随机噪声。重要的是要注意，`income` 对 $Y_{latent}$ 有很高的预测力，但**它并不改变处理效应**。简单来说，助推对所有收入水平的 $Y_{latent}$ 的影响是相同的。相反，`age` 只通过与 `nudge` 的交互影响 $Y_{latent}$。

$Y_{latent} \sim N(-4.5 + 0.001 \ income + nudge + 0.01 \ nudge \ age, 1)$

一旦我们有了 $Y_{latent}$，我们通过设置 $Y_{latent} > x$ 来模拟 `conversion`。首先令 `x=0`，这样转化率大约是 50%。即平均有 50% 的客户会选择我们的产品。

$conversion = 1\{Y_{latent} > 0\}$


```python
np.random.seed(123)

n = 100000
nudge = np.random.binomial(1, 0.5, n)
age = np.random.gamma(10, 4, n)
estimated_income = np.random.gamma(20, 2, n)*100

latent_outcome = np.random.normal(-4.5 + estimated_income*0.001 + nudge + nudge*age*0.01)
conversion = (latent_outcome > .1).astype(int)
```

我们还将所有数据存入一个 DataFrame 以便处理。同时检查一下平均转化率确实接近 50%。

```python
df = pd.DataFrame(dict(conversion=conversion,
                       nudge=nudge,
                       age=age,
                       estimated_income=estimated_income,
                       latent_outcome=latent_outcome))

df.mean()
```

至于平均处理效应，由于处理是随机的，我们可以将其估计为处理组和对照组的平均值差：$E[Y\mid T=1] - E[Y\mid T=0]$。让我们看看这些处理组平均值是什么样的。我们将同时考察潜在结果和实际转化两个角度。有一些重要的现象值得注意。

```python
df.groupby("nudge")[["latent_outcome", "conversion"]].mean()
```

```python
avg_treatment_effect(df, "nudge", "latent_outcome")
```

```python
avg_treatment_effect(df, "nudge", "conversion")
```

对潜在结果而言，ATE 很容易计算。根据我们的数据生成模型，该效应应为 `1 + avg(age)*0.01`。由于平均年龄约为 40，这给我们一个大约为 1.4 的平均处理效应。事情在转化率的 ATE 上变得更有趣（也更复杂）。**由于转化率被限制在 0 到 1 之间，其 ATE 将不是线性的**。因此，我们不能像对潜在结果那样通过简单的公式推断它（虽然有公式，但非常复杂）。我们姑且认为该效应较小。这是有道理的，对吧？处理不可能将转化率提高 1.4 个百分点，因为转化率不能超过 100%。我希望你记住这一点，这对理解我们接下来将看到的内容至关重要。

现在来谈谈条件平均处理效应 (CATE)。观察我们的数据生成过程，我们知道 `estimated_income` 可以预测转化率，但并不改变助推对转化的影响。因此，基于 `estimated_income` 对客户进行分段将产生具有相同处理效应的组。相反，`age` 只通过与助推的交互影响转化。不同的年龄分段对处理的反应会非常不同，而不同的收入分段不会。换句话说，`estimated_income` 不是一个好的个性化变量，而 `age` 是。

一个认识这一点的方法是通过累积效应曲线。`age` 的曲线应该从远离 ATE 的地方开始，并缓慢趋近于它，而 `estimated_income` 的曲线应该只是围绕 ATE 上下波动。对助推作用在潜在结果上的累积效应曲线，正是我们看到的这种情形。


```python
cumulative_effect_fn = cumulative_effect_curve(df, "nudge", "latent_outcome", min_rows=500)

age_cumm_effect_latent = cumulative_effect_fn(prediction="age")
inc_cumm_effect_latent = cumulative_effect_fn(prediction="estimated_income")

plt.plot(age_cumm_effect_latent, label="age")
plt.plot(inc_cumm_effect_latent, label="est. income")
plt.legend()
plt.xlabel("Percentile")
plt.ylabel("Effect on Latet Outcome");
```

再次强调，潜在结果的情况非常理想。由于其线性特性，我们的预期与现实非常吻合。但在现实生活中，我们既不关心也无法观测潜在结果。我们拥有的只有转化率。而对于转化率来说，情形要复杂得多。如果我们绘制累积效应曲线，`age` 仍然展示出一些处理效应异质性：曲线从高于 ATE 的位置开始，并缓慢收敛到 ATE。这意味着年龄越大，处理效应越高。目前为止，一切如我们所预期。


```python
cumulative_effect_fn = cumulative_effect_curve(df, "nudge", "conversion", min_rows=500)

age_cumm_effect_latent = cumulative_effect_fn(prediction="age")
inc_cumm_effect_latent = cumulative_effect_fn(prediction="estimated_income")

plt.plot(age_cumm_effect_latent, label="age")
plt.plot(inc_cumm_effect_latent, label="est. income")
plt.legend()
plt.xlabel("Percentile")
plt.ylabel("Effect on Conversion");
```

然而，`estimated_income` 展现出了非常大的处理效应异质性。收入较高的客户具有更低的处理效应，这使得累积效应曲线从零开始，然后超过 ATE，最后才收敛到 ATE。这告诉我们，就个性化而言，按 `estimated_income` 分组所产生的处理效应异质性比按 `age` 分组更大。

这是不是很令人困惑？为什么我们明知 `age` 会驱动效应异质性，却发现在个性化上，按 `age` 分组竟然不如按 `estimated_income` 分组有效？答案在于**结局函数的非线性**。虽然 `estimated_income` 不改变助推对潜在结果的影响，但在将潜在结果转换为转化率时（至少间接地）它会改变处理效应。转化率不是线性的，这意味着**它的导数取决于你所在的位置**。由于转化率上界为 1，如果已经很高，再想提升就很难。换句话说，高转化率区间的导数非常小。低端同样如此：由于转化率有下界 0，如果已经很低，其导数也很小。转化率呈 S 形曲线，在两端导数都很低，我们可以通过绘制按 `estimated_income` 分箱（箱宽 100）的平均转化率来看出这一点。


```python
(df
 .assign(estimated_income_bins=(df["estimated_income"]/100).astype(int)*100)
 .groupby("estimated_income_bins")
 [["conversion"]]
 .mean()
 .plot()
);
```

注意，当转化率很高时，这条曲线的斜率（导数）非常小。当转化率很低时，斜率也很小（虽然由于这一区域样本较少，这一点不易观察）。有了这条信息，我们可以解释为什么 `estimated_income` 会产生高的处理效应异质性。

由于 `estimated_income` 对转化率的预测力很强，我们可以说不同 `estimated_income` 的客户位于 S 形转化曲线的不同位置。`estimated_income` 非常高或非常低的客户落在曲线的两端，在那里导数较小，这意味着提高转化率更加困难，也意味着处理效应可能较小。另一方面，中等收入范围的客户位于转化曲线的中间地带，在那里导数较大，因此处理效应可能也较大。我说“可能”是因为，理论上，一个变量可能有如此强的效应修改力量，以至于它主导了我们沿着转换曲线看到的导数变化。然而，至少从我的经验来看，S 形转化曲率往往主导其他任何效应修改。

当然，这不仅是我的观点。下面这张图来自 Susan Athey 在哥伦比亚数据科学研究院的演讲幻灯片。她在讨论通过助推让学生申请联邦助学金以支付大学费用的效果。这也是一个转化问题。她发现最佳策略是针对那些已经有较高转换可能性的学生。她还表示，针对那些转换概率低的学生通常不是好主意。

![img](./images/23/slide-susan-athey.png)

等等！这与你上面说的矛盾！你不是说在曲线的低端和高端，处理效应都很小吗？

没错。然而在现实中，转化率很少覆盖整个 S 形曲线。通常所有人都挤在曲线的一端。在商业实践中，你的平均转化率很少是 50%，更常见的是 70% 到 90%，或者 1% 到 20% 这样的水平。在这些更常见的情形下，针对高基线概率人群可能是好主意，也可能不是。

我的意思是：我们沿用之前的潜在结果，但现在生成一个平均转化率较低的情形，将转化率设定为 `latent_outcome > 2`；接着，再构造一个平均转化率较高的情形，将转化率设定为 `latent_outcome > -2`。


```python
df["conversion_low"] = conversion = (latent_outcome > 2).astype(int)
df["conversion_high"] = conversion = (latent_outcome > -2).astype(int)

print("Avg. Low Conversion: ", df["conversion_low"].mean())
print("Avg. High Conversion: ", df["conversion_high"].mean())
```

根据我们对转化率非线性的理解，我们已经可以预测接下来会发生什么。在低转化率的情况下，针对那些具有高基准转化率（高 `estimated_income`）的群体将更为有效。这是因为我们处于S形转化曲线的左侧，在这个区域，基准转化率越低，导数会越小。在这个区域，**高基准转化率将转化为更高的处理效应**。因此，我们应该对高基准转化率的群体进行干预，而这些群体的特征就是拥有更高的 `estimated_income`。

```python
cumulative_effect_fn = cumulative_effect_curve(df, "nudge", "conversion_low", min_rows=500)

age_cumm_effect_latent = cumulative_effect_fn(prediction="age")
inc_cumm_effect_latent = cumulative_effect_fn(prediction="estimated_income")

plt.plot(age_cumm_effect_latent, label="age")
plt.plot(inc_cumm_effect_latent, label="est. income")
plt.xlabel("Percentile")
plt.ylabel("Effect on Conversion");
plt.legend();
```

正如我们所预测的，具有高 `estimated_income`（即高基准转化率）的群体，确实表现出了更高的处理效应。

现在，对于转化率较高的另一种情况，平均而言，**高基准转化率的群体会有较低的处理效应**。因此，针对那些具有高 `estimated_income` 的群体并不是一个好主意。我们可以通过倒置的累计效应曲线看到这一点，曲线显示了高 `estimated_income` 群体的处理效应较低。

```python
cumulative_effect_fn = cumulative_effect_curve(df, "nudge", "conversion_high", min_rows=500)

age_cumm_effect_latent = cumulative_effect_fn(prediction="age")
inc_cumm_effect_latent = cumulative_effect_fn(prediction="estimated_income")

plt.plot(age_cumm_effect_latent, label="age")
plt.plot(inc_cumm_effect_latent, label="est. income")
plt.xlabel("Percentile")
plt.ylabel("Effect on Conversion")
plt.legend();
```

总结一下，我们看到当结局是二元变量时，处理效应往往受到 S 形函数曲率（导数）的支配。

![img](./images/23/logistic.png)

例如，在我们的转化问题中，如果**平均转化率较低**，我们位于逻辑曲线的左侧，**处理效应会在高基线转化率时更大**。这意味着助推策略应该优先针对那些已有较高转化概率的客户。另一方面，如果**平均转化率较高**，我们位于逻辑曲线的右侧，此时导数（因此处理效应）会**在较低基线转化率的客户身上更大**。

这确实有点复杂，但可以简化为：**就去处理那些基线转化率接近 50% 的人**。数学上的依据很可靠：逻辑函数的导数在 50% 时达到峰值，所以只需针对那些接近这一点的单位。

更妙的是，这是为数不多的经验法则与数学一致的情况。在营销领域，转化问题非常常见，人们普遍认为不应该将资源浪费在“必输无疑”的客户（转化概率非常低）或“稳操胜券”的客户（转化概率非常高）身上，而应集中于中间地带。这很有意思，因为这与我们通过更正式的因果推理得出的结论完全一致。


# 连续处理与非线性

我们详细探讨了一个二元结局使处理效应异质性分析更加困难的例子。但这种现象远不止营销中的转化问题。例如，在 2021 年，全球首次向公众大规模提供获批的 COVID‑19 疫苗。当时，一个关键问题是谁应该优先接种疫苗。这显然也是一个处理效应异质性问题。决策者希望首先给那些受益最大的人接种。在这种情况下，处理效应是预防死亡或住院。那么，接种后谁的死亡或住院风险下降最多？在大多数国家，是老年人和有基础疾病（共病）的群体。这些人**感染 COVID‑19 时死亡的可能性更高**。另外，COVID 的死亡率（谢天谢地！）远低于 50%，这使得我们处于逻辑函数的左侧。在这一区域，按照我们在营销案例中所用的同一理由，优先接种那些基线死亡概率高的人是有道理的，这正是上述群体。是否只是巧合？也许吧。请记住我不是卫生专家，所以可能理解有误，但这个逻辑对我来说很有道理。

在营销助推和 COVID‑19 疫苗这两个例子中，**导致处理效应异质性复杂化的关键因素是结局函数 $Y(0)$ 的非线性**。这种非线性意味着当我们从 $Y(0)$ 转移到 $Y(1)$ 时，结局的增加主要是由于结局函数的曲率。我们看到在二元结局中 $E[Y\mid X]$ 遵循逻辑曲线，这种非线性造成了问题。但这种现象更具普遍性。事实上，它在商业中反复出现，尤其是当处理是连续变量时。为了更清楚地说明这一点，让我们再看最后一个例子。

考虑一个经典的定价问题。你为一家流媒体公司如 Netflix 或 HBO 工作。公司想知道对客户应收取什么价格。为回答这一问题，他们设计了一项实验，随机将客户分配到不同价格的套餐：5 巴西雷亚尔/月、10 巴西雷亚尔/月、15 巴西雷亚尔/月或 20 巴西雷亚尔/月。这样做不仅希望了解客户对价格上涨的敏感性，还希望了解不同类型的客户是否有不同的敏感程度。下图显示了该实验按两个客户群的结果：`A` 是收入估计较高的客户，`B` 是收入估计较低的客户。


```python
data = pd.DataFrame(dict(
    segment= ["b", "b", "b", "b",  "a", "a", "a", "a",],
    price=[5, 10, 15, 20, ] * 2,
    sales=[5100, 5000, 4500, 3000,  5350, 5300, 5000, 4500]
))

plt.figure(figsize=(8,4))
sns.lineplot(data=data, x="price", y="sales", hue="segment")
plt.title("Avg. Sales by Price (%) by Customer Segment");
```

有了这份数据，公司希望回答如下问题：谁对折扣更敏感？换句话说，我们如何**根据价格敏感度（销售对价格的弹性）对客户进行排序**？观察曲线，我们感觉 `A` 组整体上对折扣不那么敏感，尽管它产生的收入更高。不过，我们也注意到曲线存在一定的弯曲。如果考虑这种曲率，处理效应的排序不仅仅是 `A` 与 `B` 之间的比较。处理效应还取决于他们在处理曲线上的位置。例如，对于 `A` 组客户，从 15 巴西雷亚尔降到 10 巴西雷亚尔的处理效应要高于 `B` 组客户从 5 巴西雷亚尔升至 10 巴西雷亚尔的处理效应：

$$
E[Y(10) - Y(5) \mid Seg=B] < E[Y(15) - Y(10) \mid Seg=A]
$$

如果我们对这些处理效应进行排序，结果大概如下：


```python
plt.figure(figsize=(8,4))
sns.lineplot(data=data, x="price", y="sales", hue="segment")

plt.annotate("1", (8, 5350), bbox=dict(boxstyle="round", fc="1"))
plt.annotate("2", (8, 5000), bbox=dict(boxstyle="round", fc="1"))
plt.annotate("3", (13, 5100), bbox=dict(boxstyle="round", fc="1"))
plt.annotate("4", (13, 4700), bbox=dict(boxstyle="round", fc="1"))
plt.annotate("4", (17, 4800), bbox=dict(boxstyle="round", fc="1"))
plt.annotate("5", (17, 3900), bbox=dict(boxstyle="round", fc="1"))

plt.title("Ordering of the Effect of Increasing Price");
```

正如结局为二元变量时的情形一样，在这个例子中，**处理效应与结果相关**。销售越高（价格越低），绝对处理效应越小；销售越低（价格越高），绝对处理效应越小。但在这里，情况更复杂，因为**效应不仅与结果相关，还与处理水平相关**。这使得回答反事实问题更加棘手。例如，假设你的实验数据实际长成了下面的样子：对于 `A` 组（富裕人群），你测试了较高的价格，而对于 `B` 组，你只测试了较低的价格。这种情况非常常见，因为公司通常只想在他们认为合理的处理范围内进行实验。


```python
data = pd.DataFrame(dict(
    segment= ["b", "b", "b", "b",  "a", "a", "a", "a",],
    price=[5, 10, 15, 20, ] * 2,
    sales=[5100, 5000, 4500, 3000,  5350, 5300, 5000, 4500]
))

plt.figure(figsize=(8,4))
sns.lineplot(data=data.loc[lambda d: (d["segment"] == "a") | (d["price"] < 12) ], x="price", y="sales", hue="segment")
plt.title("Avg. Sales by Price (%) by Customer Segment");
```

如果你天真地将处理效应结果简单汇总，可能会得出 `A` 组对提价的价格弹性远高于 `B` 组的结论。但那只是因为对于 `B` 组，你只探索了低处理效应区域。

那么，当处理效应随着处理水平和结果水平而变化时，你可以做什么？说实话，这仍是一个活跃的研究领域。在实践中，你能做的最好的事情是在回答“哪类客户对处理更敏感”时**非常谨慎**。确保比较的客户类型有相同的处理分布。如果没有，就要对外推处理效应持高度怀疑态度。例如，在上面的例子中，尽管 `B` 组客户对提价似乎不那么敏感，但如果你对该组客户实行高于 10 巴西雷亚尔的价格，你并不知道这一结论是否仍然成立。

你还可以尝试做的一件事是对反应曲线进行线性化。这里的想法是，通过变换处理或结局（或两者），使它们之间的关系看起来像一条直线。由于直线的导数是恒定的，这可以消除处理效应随曲线位置而变化的问题。例如，如果我们将价格变量取负，对其进行四次方运算然后再反转符号，我们得到的是一种大致线性的关系。在这份变换后的数据中，`A` 对提价的敏感度低于 `B` 的说法更加合理，因为它不再取决于我们处于曲线的哪一部分。


```python

plt.figure(figsize=(8,4))
sns.lineplot(data=data.assign(price = lambda d: -1*(-d["price"]**4)),
             x="price", y="sales", hue="segment")
plt.title("Avg. Sales by -(-price^4)");
```

然而，这种做法有很多缺点。首先，并非总能线性化一条曲线。在我们的例子中，你可以清楚地看到这种线性化并不完美。但更重要的是，有时放弃曲率在业务上毫无意义。在我们的定价例子中，完全有可能我们认为以 15 巴西雷亚尔的价格对 `A` 组客户提价比对 5 巴西雷亚尔的 `B` 组客户提价更为敏感。这会导致一个合理的决策，即将 `A` 组客户的价格从 15 调整到 10，但不对 `B` 组的价格做任何改变。


## 关键观点

我意识到我提出的问题可能比提供的答案还多。不过，有时对一个问题最好的做法就是充分意识到它的存在。在本章中，我希望我能让你意识到，当我们关心的结局是非线性时会出现的各种复杂性。

对于二元结局，这是一个常见且研究较多的问题。在这种情况下，处理效应在平均结局接近 0.5 时往往较高。由于结局被限制在 0 和 1 之间，如果我们太接近 0 或 1，效应往往非常小。

当非线性出现在连续结局中时，情况就更为复杂了。在这种情况下，你能做的最好事情就是非常仔细地思考问题。试着回答，你究竟更关心不受基线影响的处理效应，还是基线本身也很重要。仅仅这个问题就能为你提供宝贵的指导原则。

## 参考说明

这里写的大部分内容都来自作者对这个问题的个人经验。不过，作者确实找到了一篇涉及这个主题的学术文章：《Causal Classification: Treatment Effect Estimation vs. Outcome Prediction》由 Fernández‑Loría 和 Provost 撰写，其中讨论了处理效应与结果变量相关的情况。

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

```python
from toolz import *

import pandas as pd
import numpy as np
from scipy.special import expit

from linearmodels.panel import PanelOLS
import statsmodels.formula.api as smf

import seaborn as sns
from matplotlib import pyplot as plt
from matplotlib import style

style.use("ggplot")
```

# 24 - 双重差分传奇

在讨论完处理效应异质性之后，我们现在回到平均处理效应。接下来的几章，我们将介绍面板数据方法的一些最新进展。面板是一种按时间重复观察同一个单位的数据结构。由于我们观察同一单位多个时间段的情况，可以查看处理发生前后的变化。这使面板数据成为在无法随机化时识别因果效应的有希望的替代方案。

为了激励面板数据的使用，我们将主要讨论营销领域的因果推断应用。营销特别有趣，因为在这个领域进行随机实验非常困难。在营销中，我们往往无法控制谁接受处理，即谁看到我们的广告。当一个新用户访问我们的网站或下载我们的应用时，我们无法很好地知道他是否看过我们的营销活动，还是由于其他因素而来。

（注意：对于熟悉营销归因的人来说，我知道有许多归因工具旨在解决这个问题。但我也知道它们有许多局限性。）

离线营销的问题更大。你如何知道一场电视广告的价值是否超过其成本？因此，在营销中常见的做法是地理实验（Geo-Experiments）：我们在某些地理区域投放营销活动而不在其他区域投放，然后进行比较。在这种设计中，面板数据方法尤其有趣：我们可以收集整个地理区域（单位）在多个时间段的数据。为了理解这种数据并识别因果效应，或许最流行的方法是双重差分（DiD）家族。

2020 年和 2021 年对我们大多数人来说都不容易。但对 DiD 来说尤其难。大量的最新研究凸显了这些方法的一些严重缺陷，这些缺陷在过去并不为人所知。因此，尽管我们已经在第一部分中有一章介绍 DiD，但那里的内容比较入门。它没有涵盖围绕面板数据方法的新发现和激烈讨论。现在我们应该更彻底地了解它们，从 DiD 开始。在这一章中，我将尝试总结最近发现的 DiD 问题，并展示如何解决它们。本章分为三个部分：

1. **诞生**：回顾为什么面板数据对因果推断如此有吸引力，以及 DiD 和双向固定效应（TWFE）如何利用时间结构。
2. **死亡**：消化 DiD 和 TWFE 模型隐含的一个关键假设，该假设一直被忽视。理解该假设何时及如何会失败。
3. **启蒙**：在了解了 DiD 和 TWFE 的问题后，我们可以思考解决方案。本节展示了对第二节问题的一个简单变通方法。

让我们直接进入正题！

## 1) 诞生：面板数据的承诺

![img](./images/24/promise.png)

正如我所说，面板数据是我们在多个时间段观察多个单位 `i` 的数据。想想美国的政策评估场景，你想考察大麻合法化对犯罪率的影响。你拥有多个州 `i` 在多个时间段 `t` 的犯罪率数据。你还观察到每个州何时采取了支持大麻合法化的立法。我希望你能看出这对因果推断而言何等强大。将大麻合法化视为处理 `D`（因为 `T` 用来表示时间）。我们可以追踪最终会被处理的某个州的犯罪率趋势，看看处理时间是否带来了趋势的变化。从某种程度上说，一个州在前后对比中充当了自己的控制单位。此外，由于我们有多个州，我们还可以比较处理州和控制州。当我们把这两种比较放在一起——处理与控制以及处理前与处理后——我们最终得到了一个极其强大的工具去推断反事实，从而获得因果效应。

面板数据方法经常用于政府政策评估，但我们可以轻松论证它们对（科技）行业也非常有用。公司通常会在多个时间段跟踪用户数据，这导致了丰富的面板数据结构。不仅如此，有时实验不可行，因此我们必须依赖其他识别策略。为了进一步探索这一想法，我们考虑一个假想例子：一家年轻的科技公司在多个城市跟踪其应用的安装人数。在 2021 年的某个时候，这家公司在其应用中推出了一个新功能。它现在想知道这个功能为公司带来了多少新增用户。推出是渐进的。有些城市在 `2021-06-01` 得到新功能，另一些在 `2021-07-15`。其余城市的全面推出要到 2022 年才进行。由于我们的数据只到 `2021-07-31`，最后这组城市可视为对照组。用因果推断的术语说，推出这一功能可以看作处理，安装量可以看作结局变量。我们想知道处理对结果的效应，也就是新功能对安装量的影响。

请注意，这家科技公司无法在此做实验。他们不能控制哪个人知道自己有新功能。我们说他们对处理分配的控制有限。这是因为分析单位是**尚未成为其客户的人**。他们想知道能通过安装应用将多少人转化为客户。当然，他们无法对这些人随机化。因此，相反，他们将分析单位改为城市。在一个城市而非另一个城市推出功能是他们可以控制的，而对个人的推出则不行。

在某一特定时间得到功能（被处理）的一组城市称为 cohort（队列）。在我们的例子中，我们有三个 cohort：一个在 `2021-06-01` 被处理，另一个在 `2021-07-15` 被处理，还有一个控制 cohort，在我们的数据结束后才会被处理。为了了解这份数据是什么样子，让我们绘制按 cohort 分组的日均安装量。


```python
date = pd.date_range("2021-05-01", "2021-07-31", freq="D")
cohorts = pd.to_datetime(["2021-06-01", "2021-07-15", "2022-01-01"]).date
units = range(1, 100+1)

np.random.seed(1)

df = pd.DataFrame(dict(
    date = np.tile(date, len(units)),
    unit = np.repeat(units, len(date)),
    cohort = np.repeat(np.random.choice(cohorts, len(units)), len(date)),
    unit_fe = np.repeat(np.random.normal(0, 5, size=len(units)), len(date)),
    time_fe = np.tile(np.random.normal(size=len(date)), len(units)),
    week_day = np.tile(date.weekday, len(units)),
    w_seas = np.tile(abs(5-date.weekday) % 7, len(units)),
)).assign(
    trend = lambda d: (d["date"] - d["date"].min()).dt.days/70,
    day = lambda d: (d["date"] - d["date"].min()).dt.days,
    treat = lambda d: (d["date"] >= d["cohort"]).astype(int),
).assign(
    y0 = lambda d: 10 + d["trend"] + d["unit_fe"] + 0.1*d["time_fe"] + d["w_seas"]/10,
).assign(
    y1 = lambda d: d["y0"] + 1
).assign(
    tau = lambda d: d["y1"] - d["y0"],
    installs = lambda d: np.where(d["treat"] == 1, d["y1"], d["y0"])
)

```

```python
plt.figure(figsize=(10,4))
[plt.vlines(x=cohort, ymin=9, ymax=15, color=color, ls="dashed") for color, cohort in zip(["C0", "C1"], cohorts[:-1])]
sns.lineplot(
    data=(df
          .groupby(["cohort", "date"])["installs"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs",
    hue="cohort",
);
```

虚线标记了各 cohort 获得处理（特性推出）的时刻。花些时间欣赏上图数据的丰富性。首先，我们可以看到每个 cohort 都有自己的基线水平。这只是因为不同城市人口规模不同，导致安装量因城市规模而异。例如，看起来第一批 cohort（6 月 1 日被处理的城市）的基线更高，而对照 cohort 的安装基线更低。这意味着仅仅比较处理 cohort 和对照 cohort 会得到偏差的结果，因为控制组的 $Y_0$ 低于处理组，即 $Y_{0}|G=Control < Y_{0}|G=Treated$，其中我们用 $G$ 表示 cohort。幸运的是，这不会成为问题。面板数据允许我们跨城市**和**时间进行比较，从而调整不同的基线。

说到时间，可以看到有一个总体上升趋势，并伴有一些波动（看起来像每周季节性）。关注对照 cohort，可以看到日安装量从 5 月的约 10 个增加到 6 月的约 11 个，增长了约 1 个。用技术术语来说，后期时间段的 $Y_0$ 高于早期时间段。这意味着仅仅比较同一城市跨时间的变化也会产生偏差。再次庆幸的是，面板数据结构允许我们不仅跨时间比较，还能跨城市比较，从而调整趋势。

理想情况下，为了推断该新功能上线对安装量的影响，我们想知道那些获得功能的 cohort 如果没有得到它会发生什么。我们想估计处理组在处理之后时期的反事实结果 $Y(0)$。如果我们用 cohort 获得处理的时间 `g` 来标记每个 cohort（记住 cohort 只是同一时间被处理的一组城市），我们可以将该反事实写为 $E[Y_0\mid t \geq g]$，然后把 cohort `g` 的处理效应（ATT）定义为：

$$
E[Y_1\mid t \geq g] - E[Y_0\mid t\geq g]
$$

接下来的问题是如何从我们拥有的数据中估计这一点。一种方法是利用面板数据结构的力量来估计这些反事实。例如，我们可以使用线性回归和双重差分公式得到双向固定效应模型。假设每个城市 `i` 有一个基础安装水平 \(\gamma_i\)。这与我们之前所见联系起来：也许一个城市有更多的安装是因为它人口更大，或者因为其文化更符合我们公司的产品。无论原因是什么，即使我们不知道，我们说这些单位特异性可以通过一个**单位固定参数** \(\gamma_i\) 捕捉。同样，我们可以说每个时间 `t` 有一个基线安装水平，我们可以用一个**时间固定参数** \(\theta_t\) 来捕捉。如果是这样，一个好的建模方式是说安装量取决于城市（单位）效应 \(\gamma\) 和时间效应 \(\theta\)，再加上一些随机噪声。

$$
Installs_{it} = \gamma_i + \theta_t + e_{it}
$$

为了把处理纳入这个模型，我们定义变量 \(D_{it}\)，当单位被处理时取 1，否则取 0。在我们的例子中，这个变量对于永不被处理的 cohort 始终为零；对于其他 cohort，在其处理日期之前为零，之后为一。


```python
formula = f"""installs ~ treat + C(unit) + C(date)"""

twfe_model = smf.ols(formula, data=df).fit()

twfe_model.params["treat"]
```

由于我在上面模拟了数据，我确切地知道真实的个体处理效应，它存储在 `tau` 列中。既然 TWFE 应该能恢复处理组的处理效应，我们可以验证真实的 ATT 是否与上述估计匹配。我们所要做的只是筛选出被处理的单位和时期（`treat==1`），然后计算 `tau` 列的平均值。

```python
df.query("treat==1")["tau"].mean()
```

在有人指出给每个单位生成一个虚拟变量会在大数据中不切实际之前，我先承认，确实如此。但有一个简单的解决办法。我们可以用 FWL 定理将那一个回归分成两个。事实上，上述模型在数值上等价于估计下面这个模型

$$
\tilde{Installs}_{it} = \tau \tilde D_{it} + e_{it}
$$

其中

$$
\tilde{Installs}_{it} = Installs_{it} - \underbrace{\frac{1}{T}\sum_{t=0}^T Installs_{it}}_\text{时间平均} - \underbrace{\frac{1}{N}\sum_{i=0}^N Installs_{it}}_\text{单位平均}
$$

和

$$
\tilde{D}_{it} = D_{it} - \frac{1}{T}\sum_{t=0}^T D_{it} - \frac{1}{N}\sum_{i=0}^N D_{it}
$$

用文字来说，如果数学公式太拥挤，我们从处理指标和结果变量中分别减去跨时间的单位平均值（第一项）和跨单位的时间平均值（第二项）以构造残差。这个过程通常称为去均值（de-meaning），因为我们从结果和处理中减去了平均值。最后，这段代码就是同样的东西，只不过用代码实现：


```python
@curry
def demean(df, col_to_demean):
    return df.assign(**{col_to_demean: (df[col_to_demean]
                                        - df.groupby("unit")[col_to_demean].transform("mean")
                                        - df.groupby("date")[col_to_demean].transform("mean"))})


formula = f"""installs ~ treat"""
mod = smf.ols(formula,
              data=df
              .pipe(demean(col_to_demean="treat"))
              .pipe(demean(col_to_demean="installs")))

result = mod.fit()

result.summary().tables[1]
```

我们还可以做另一件事来理解 TWFE 模型在做什么，那就是绘制反事实预测 \(\hat{Y_0}|t \geq g\)。这很有帮助，因为我们的模型将处理效应 \(\hat{\tau}\) 看作是估计的差值 $Y_1 - \hat{Y_0}$。从这个显式差值出发可以帮助我们理解模型在做什么。在下面的图中，我们正是看到了这个，\(\hat{Y_0}\) 用虚线表示。

```python
df_pred = df.assign(**{"installs_hat_0": twfe_model.predict(df.assign(**{"treat":0}))})
          

plt.figure(figsize=(10,4))
[plt.vlines(x=cohort, ymin=9, ymax=15, color=color, ls="dashed") for color, cohort in zip(["C0", "C1"], cohorts[:-1])]
sns.lineplot(
    data=(df_pred
          .groupby(["cohort", "date"])["installs_hat_0"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs_hat_0",
    hue="cohort",
    alpha=0.7,
    ls="dotted",
    legend=None
)
sns.lineplot(
    data=(df_pred
          .groupby(["cohort", "date"])["installs"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs",
    hue="cohort",
);
```

该图展示了 TWFE 如何将它在控制单位中看到的趋势投影到被处理单位，并调整基准水平。例如，如果我们看红色队列的线，反事实 $Y_0$ 是来自蓝色和紫色队列的平均趋势（趋势投影），但水平上移到红色队列的水平（水平调整）。这就是我们把 TWFE 看成双重差分方法的原因。它既做了趋势投影和水平调整，又适用于多个时间段和多个单位（在两个单位和两个时间段的情况中，TWFE 与 DiD 等价）。

## 2) 死亡：效应异质性下的失败

正如我们刚才看到的，DiD 和 TWFE 有其优点。它可以很好地估计反事实，既适应时间又适应单位差异，这使其成为一种强大的因果推断技术。但如果事情仅止于此，我们就无需这一章了，因为这在本书的第一部分已有涵盖。最近发生的事情是，许多学者注意到将 2×2 的 DiD 扩展到多期的 TWFE 并不像我们最初想象的那样简单。事实上，在许多现实应用中，**TWFE 在其通常的形式下被证明是有偏的**。这一发现导致经济学中依赖这种技术的多项研究被重新审视。要理解这一切，最好的起点是阐明模型背后的假设。

![img](./images/24/death.png)

为简便起见，让我们考虑不含时间效应的固定效应模型：

$$
y_{it} = \tau D_{it} + \gamma_i + e_{it}
$$

我们可以将该模型的假设分成两组：

1. **函数形式假设：**
    * 时间上无异质效应（效应恒定）；
    * 协变量与结果的线性关系；
    * 加性固定效应。
2. **严格外生性：**
    * 平行趋势；
    * 无预期（处理前无影响）；
    * 无未观测的随时间变化的混杂因素；
    * 过去的处理不影响当前结果（无延滞效应）；
    * 过去的结果不影响当前处理（无反馈）。

在这里，我们将坚持讨论函数形式假设。协变量的线性假设众所周知，适用于所有线性回归模型。但正如我们在“去偏/正交机器学习”一章中看到的，我们可以用机器学习模型轻松放松这一假设。这意味着如果愿意，我们可以放松这一假设。至于加性固定效应，这并不是一个过于限制性的假设，因此问题不大。我想重点关注的（并引起广泛讨论）的，是时间上无异质效应这个假设。

### 时间上的处理效应异质性

如果你曾在营销或科技行业工作过，你就知道事情需要时间才能成熟。如果你推出一个新功能，用户需要时间去适应。同样，开展营销活动时，该活动的影响不会立刻显现。它会随着时间成熟，甚至在活动结束后仍能吸引新用户。这**不是**我们之前安装数据中看到的模式。那里面，安装量在队列被处理的时刻立刻跃升。如果我们将其改成更符合现实的情况会怎样？具体来说，我们仍然让 ATT 为 1，但现在它需要 10 天才能成熟（因此在处理的第一天效应为 0.1，第二天为 0.2，以此类推，直到第 10 天达到 1）。另外，我会减少时间和单位效应的大小，这样总体趋势更容易看清。


```python
date = pd.date_range("2021-05-01", "2021-07-31", freq="D")
cohorts = pd.to_datetime(["2021-06-01", "2021-07-15", "2022-01-01"])

units = range(1, 100+1)

np.random.seed(1)

df_heter = pd.DataFrame(dict(
    date = np.tile(date, len(units)), 
    unit = np.repeat(units, len(date)),
    cohort = np.repeat(np.random.choice(cohorts, len(units)), len(date)),
    unit_fe = np.repeat(np.random.normal(0, 5, size=len(units)), len(date)),
    time_fe = np.tile(np.random.normal(size=len(date)), len(units)),
    week_day = np.tile(date.weekday, len(units)),
    w_seas = np.tile(abs(5-date.weekday) % 7, len(units)),
)).assign(
    trend = lambda d: (d["date"] - d["date"].min()).dt.days / 70,
    day = lambda d: (d["date"] - d["date"].min()).dt.days,
    treat = lambda d: (d["date"] >= d["cohort"]).astype(int),
).assign(
    y0 = lambda d: 10 + d["trend"] + 0.2 * d["unit_fe"] + 0.05 * d["time_fe"] + d["w_seas"] / 50,
).assign(
    y1 = lambda d: d["y0"] + np.minimum(0.1 * (np.maximum(0, (d["date"] - d["cohort"]).dt.days)), 1)
).assign(
    tau = lambda d: d["y1"] - d["y0"],
    installs = lambda d: np.where(d["treat"] == 1, d["y1"], d["y0"])
)
```

```python
plt.figure(figsize=(10,4))
[plt.vlines(x=cohort, ymin=9, ymax=15, color=color, ls="dashed") for color, cohort in zip(["C0", "C1"], cohorts)]
sns.lineplot(
    data=(df_heter
          .groupby(["cohort", "date"])["installs"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs",
    hue="cohort",
);
```

我们看到安装量最终仍然达到之前的水平，但需要一段时间（10 天）。这似乎合理吧？现实生活中的大多数数据都是这样，效应需要一些时间才能成熟。好的，让我们在这些数据上运行 TWFE 模型，看看会发生什么。

```python
formula = f"""installs ~ treat + C(date) + C(unit)"""

twfe_model = smf.ols(formula, data=df_heter).fit()

print("Estimated Effect: ", twfe_model.params["treat"])
print("True Effect: ", df_heter.query("treat==1")["tau"].mean())
```

首先要注意，真实的 ATT 不再是 1，因为在最初的几个时期它会较小。其次，更重要的是，我们看到**TWFE 的估计 ATT 不再恢复真实的 ATT**。简单地说：TWFE 有偏。但为什么呢？在这里我们有平行趋势、无预期以及所有其他严格外生性假设。那到底发生了什么？

理解这一问题的第一步是意识到 TWFE 实际上可以分解为多个 2×2 的 DiD。在我们的例子中，这将包括：比较早期处理与从未处理、比较后期处理与从未处理、比较早期处理与后期处理（以后期处理作为对照）以及比较后期处理与早期处理（以早期处理作为对照）这四种比较：


```python
# 译者注： 原始代码因调色板未涵盖全部 cohort 值而报错。此处已修正为自动匹配调色板，并单独绘制排除组以实现正确对比效果。
g_plot_data = (
    df_heter
    .groupby(["cohort", "date"])["installs"]
    .mean()
    .reset_index()
    .astype({"cohort": str})
)


unique_cohorts = sorted(g_plot_data["cohort"].unique())
palette = dict(zip(unique_cohorts, sns.color_palette(n_colors=len(unique_cohorts))))


fig, axs = plt.subplots(2, 2, figsize=(15, 8), sharex=True, sharey=True)


def plot_comp(df, ax, exclude_cohort, name):
    exclude_cohort_str = str(pd.to_datetime(exclude_cohort).date())


    df_main = df[df["cohort"] != exclude_cohort_str]
    df_excl = df[df["cohort"] == exclude_cohort_str]


    sns.lineplot(
        data=df_main,
        x="date",
        y="installs",
        hue="cohort",
        palette=palette,
        legend=None,
        ax=ax
    )


    color = palette.get(exclude_cohort_str, "gray")
    sns.lineplot(
        data=df_excl,
        x="date",
        y="installs",
        color=color,
        alpha=0.2,
        legend=None,
        ax=ax
    )

    ax.set_title(name)


plot_comp(g_plot_data, axs[0, 0], cohorts[1], "Early vs Never")
plot_comp(g_plot_data, axs[0, 1], cohorts[0], "Late vs Never")
plot_comp(g_plot_data[g_plot_data["date"] <= pd.to_datetime(cohorts[1])], axs[1, 0], cohorts[-1], "Early vs Late")
plot_comp(g_plot_data[g_plot_data["date"] > pd.to_datetime(cohorts[0])], axs[1, 1], cohorts[-1], "Late vs Early")


plt.tight_layout()
```

前三个比较没有什么可担心的，主要是因为它们使用的控制组行为良好。然而，第四个比较，即“晚期 vs 早期”是有问题的。注意这个比较使用早期处理组作为控制。同时注意早期处理组有奇怪的行为：它在开始时急剧上升。这反映了我们的 ATT 并非即时，而是需要 10 天才能成熟。从直觉上看，我们可以看到这会扰乱 DiD 中反事实趋势的估计，使其比实际的更陡峭。为了直观展示这一点，让我们绘制上述第四组中晚期处理的反事实 $Y_0$。


```python
late_vs_early = (df_heter
                 [df_heter["date"].astype(str)>="2021-06-01"]
                 [lambda d: d["cohort"].astype(str)<="2021-08-01"])


formula = f"""installs ~ treat + C(date) + C(unit)"""

twfe_model = smf.ols(formula, data=late_vs_early).fit()

late_vs_early_pred = (late_vs_early
                      .assign(**{"installs_hat_0": twfe_model.predict(late_vs_early.assign(**{"treat":0}))})
                      .groupby(["cohort", "date"])
                      [["installs", "installs_hat_0"]]
                      .mean()
                      .reset_index())


plt.figure(figsize=(10,4))
plt.title("Late vs Early Counterfactuals")
sns.lineplot(
    data=late_vs_early_pred,
    x="date",
    y = "installs",
    hue="cohort",
    legend=None
)

sns.lineplot(
    data=(late_vs_early_pred
          [late_vs_early_pred["cohort"].astype(str) == "2021-07-15"]
          [lambda d: d["date"].astype(str) >= "2021-07-15"]
         ),
    x="date",
    y ="installs_hat_0",
    alpha=0.7,
    color="C0",
    ls="dotted",
    label="counterfactual"
);
```

正如我们所说的，反事实的趋势比它应该有的要陡得多。它捕捉了早期处理开始时的快速增长，并将这种趋势投射到了后期处理组上。

从更技术的角度看，可以证明（Goodman-Bacon, 2019）即便在严格外生（平行趋势、无预期…）的情况下，如果各个队列规模相同，TWFE 估计量收敛于

$$
plim_{x \to \infty} \hat{\tau}^{TWFE} = VWATT - \Delta ATT
$$

第一个项是来自多个 DiD 比较（如我们先前看到的）的方差加权 ATT，这正是我们想要的。然而，这里还有一个额外的 $\Delta ATT$ 项，它表示 ATT 随时间变化的幅度，这正是导致估计偏差的原因。观察这个项，我们可以看到：如果效应随时间增大（如我们的例子），估计将向下偏；如果效应随时间减小，则向上偏。

在上述例子中，我们看到 TWFE 的估计效应小于真实的 ATT。但情况可能更极端。我认为值得再看一个例子，以说明这种偏差会强大到甚至颠倒真实 ATT 的符号。让我们考虑一个非常简单的过程，只有两个队列。在这个例子中，处理效应是负的，并且每天减少 0.1。我还去掉了所有时间固定效应和趋势，这样我们可以真正看清发生了什么。


```python
date = pd.date_range("2021-05-15", "2021-07-01", freq="D")
cohorts = pd.to_datetime(["2021-06-01", "2021-06-15"])
units = range(1, 100+1)

np.random.seed(1)

df_min = pd.DataFrame(dict(
    date = np.tile(date, len(units)),
    unit = np.repeat(units, len(date)),
    cohort = np.repeat(np.random.choice(cohorts, len(units)), len(date)),
    unit_fe = np.repeat(np.random.normal(0, 5, size=len(units)), len(date)),
)).assign(
    trend = 0,
    day = lambda d: (d["date"] - d["date"].min()).dt.days,
    treat = lambda d: (d["date"] >= d["cohort"]).astype(int),
).assign(
    y0 = lambda d: 10 - d["trend"] + 0.1*d["unit_fe"]
).assign(
    y1 = lambda d: d["y0"] - 0.1*(np.maximum(0, (d["date"] - d["cohort"]).dt.days))
).assign(
    tau = lambda d: d["y1"] - d["y0"],
    installs = lambda d: np.where(d["treat"] == 1, d["y1"], d["y0"])
)

```

```python
plt.figure(figsize=(10,4))
[plt.vlines(x=cohort, ymin=7, ymax=11, color=color, ls="dashed") for color, cohort in zip(["C0", "C1"], cohorts)]
sns.lineplot(
    data=(df_min
          .groupby(["cohort", "date"])["installs"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs",
    hue="cohort",
);
```

从上面的图可以明显看出，ATT 是负的，对吗？正确的反事实应该是一条大约在 11 附近的直线。然而，如果我们运行 TWFE 估计器，我们得到的却是正的效应！


```python
formula = f"""installs ~ treat + C(date) + C(unit)"""

twfe_model = smf.ols(formula, data=df_min).fit()

twfe_model.params["treat"]
```

再次说明，要弄清发生什么，请把注意力放在用早期处理队列作为控制的比较上。记住，与 DiD 类似，TWFE 会将控制组的趋势调整到处理组的水平，因此反事实应该反映这一点。


```python
df_pred = df_min.assign(**{"installs_hat_0": twfe_model.predict(df_min.assign(**{"treat":0}))})
          
plt.figure(figsize=(10,4))
[plt.vlines(x=cohort, ymin=7, ymax=11, color=color, ls="dashed") for color, cohort in zip(["C0", "C1"], cohorts)]
sns.lineplot(
    data=(df_pred
          [(df_pred["cohort"].astype(str) > "2021-06-01") & (df_pred["date"].astype(str) >= "2021-06-15")]
          .groupby(["cohort", "date"])["installs_hat_0"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs_hat_0",
    alpha=0.7,
    ls="dotted",
    color="C0",
    label="counterfactual",
)
sns.lineplot(
    data=(df_pred
          .groupby(["cohort", "date"])["installs"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs",
    hue="cohort",
    legend=None
)
plt.ylabel("Installs");
```

注意反事实的水平被压低了。由于早期处理组的效应在下降，这将反事实水平从 10 推低到约 9.5。不仅如此，反事实还调整出一个本不该存在的下降趋势。从图中可以明显看出，正确的反事实应该是一条水平线在 10，但却成了一条向下倾斜的线……


```python
df_min_rel = (df_min
              .assign(relative_days = (df_min["date"] - df_min["cohort"]).dt.days))

df_min_rel.head()
```

Then, we can pass that column as a category so our model will estimate the expected number of installs for each period relative to the treatment. We can then define the effect as the extra expected number of installs compared to relative day -1, which is the last day prior to the treatment.
 
We might think that this formulation would capture the time heterogeneity in the ATT and solve all our issues. Unfortunately, that is not the case. If we try it out and plot the counterfactuals, we can see that they are far from where they should intuitively be (the horizontal line at 11).

```python
# remove the intercept, otherwise effects will be relative to relative day -30
formula = f"installs ~ -1 + C(relative_days) + C(date) + C(unit)"

twfe_model = smf.ols(formula, data=df_min_rel).fit()
```

```python
df_pred = df_min_rel.assign(
    installs_hat_0=twfe_model.predict(df_min_rel.assign(relative_days=-1))
) 

plt.figure(figsize=(10,4))
[plt.vlines(x=cohort, ymin=7, ymax=11, color=color, ls="dashed") for color, cohort in zip(["C0", "C1"], cohorts)]
sns.lineplot(
    data=(df_pred
          [(df_pred["cohort"].astype(str) > "2021-06-01") & (df_pred["date"].astype(str) >= "2021-06-15")]
          .groupby(["cohort", "date"])["installs_hat_0"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs_hat_0",
    alpha=0.7,
    ls="dotted",
    color="C0",
    label="counterfactual",
)
sns.lineplot(
    data=(df_pred
          .groupby(["cohort", "date"])["installs"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs",
    hue="cohort",
    legend=None
)
plt.ylabel("Installs");
```

![img](./images/24/awful.jpeg)

不过这些反事实略好一些。我们可以看到它们位于实际的 $Y_1$ 之上。因此，我们至少会估计出一个负效应，这是应该的。为了看到这一点，我们可以通过先提取与每个虚拟变量相关联的参数，再从这些参数中减去相对第 -1 天（基线）对应的参数，来绘制估计的效应。


```python
effects = (twfe_model.params[twfe_model.params.index.str.contains("relative_days")]
           .reset_index()
           .rename(columns={0:"effect"})
           .assign(relative_day=lambda d: d["index"].str.extract(r'\[(.*)\]').astype(int))
           # set the baseline to period -1
           .assign(effect = lambda d: d["effect"] - d.query("relative_day==-1")["effect"].iloc[0]))

# effects
effects.plot(x="relative_day", y="effect", figsize=(10,4))
plt.ylabel("Estimated Effect")
plt.xlabel("Time Relative to Treatment");
```

我们可以看到它稍微好了一些，因为至少在处理之后估计的效应 1）大多为负，2）总体呈下降趋势。但仍然出现了一些奇怪的尖峰，以及类似于正的处理前效应的东西，这显然不合理。

问题依旧是我们一直讨论的：由于处理的时间不同，早期处理组被用作晚期处理组的控制，这导致模型估计出非常奇怪的反事实趋势。结论是，仅仅在模型中加入相对于处理时间的虚拟变量并不能解决问题。那么，到底应该怎么做呢？

## 3) 启蒙：更灵活的函数形式

有好消息也有坏消息。先说好消息：我们已经把问题归结为函数形式方面，因此可以通过修正函数形式来解决。具体而言，我们反复提到 TWFE 的这种特定偏差源自时间异质的效应。这种情况会发生，原因之一是处理效应需要时间才能成熟（例如，一场营销活动可能需要 10 天才能达到完全效果）。换句话说，传统 TWFE 的函数形式不够灵活，无法捕捉这种异质性，导致了我们讨论的偏差。大多数情况下，认识到问题本身就已在寻找解决方案的路上走了很远。

在上一节末尾，我们看到仅仅允许与处理相对时间的每个时期有不同的效应（事件研究设计）是不够的。尽管那样做提高了模型的灵活性，但并没有真正解决问题。我们需要想办法让模型比这更灵活。


```python
formula = f"""installs ~ treat:C(cohort):C(date) + C(unit) + C(date)"""

# for nicer plots latter on
df_heter_str = df_heter.astype({"cohort": str, "date":str})

twfe_model = smf.ols(formula, data=df_heter_str).fit()
```

为此，让我们回到最初的例子，即我们试图建模推出一个新功能（处理）带来的新增安装数量。我们看到简单的 TWFE 模型在这里不起作用：

$$
Installs_{it} = \tau D_{it} + \gamma_i + \theta_t + e_{it}
$$

不仅如此，我们知道它不起作用是因为它太过限制性。它强迫效应对于所有时间和单位都是相同的，即 \(\tau_{it} = \tau \ \forall i, t\)，也就是说它强迫时间同质。如果这是问题所在，一个简单的解决办法是允许每个时间和每个单位有不同的效应：

$$
Installs_{it} = \sum_{i=0}^N \sum_{t=0}^T \tau_{it} D_{it} + \gamma_i + \theta_t + e_{it}
$$

这等价于运行下面的公式：

```
installs ~ treat:C(unit):C(date) + C(unit) + C(date)
```

不幸的是，我们不能拟合这个模型。这会使参数数量超过数据点数量。因为我们对日期和单位进行了交互，我们将拥有每个单位每个时间段的一个处理效应参数，共 \(T*N\) 个参数，但这恰好是我们的样本数量！OLS 无法运行。

好吧，现在我们需要减少模型的处理效应参数数量。为此，我们可以考虑某种方式对单位进行分组。如果稍微思考一下，我们可以看到一种很自然的分组方式：按 cohort（队列）分！我们知道整个队列的效应随时间遵循同一模式。因此，对上面那个不切实际模型的一个自然改进是允许效应随 cohort 而不是单位变化：

$$
Installs_{it} = \sum_{g=0}^G \sum_{t=0}^T \tau_{gt} D_{it} + \gamma_i + \theta_t + e_{it}
$$

其中 `G` 是 cohort 的总数，`g` 标记每个具体 cohort。该模型的处理效应参数数量更为合理（\(T*G\)），因为 \(G\) 通常远小于 \(N\)。现在，我们终于可以运行它。


```python
df_pred = (df_heter_str
           .assign(**{"installs_hat_0": twfe_model.predict(df_heter_str.assign(**{"treat":0}))})
           .assign(**{"effect_hat": lambda d: d["installs"] - d["installs_hat_0"]}))

print("Number of param.:", len(twfe_model.params))
print("True Effect: ", df_pred.query("treat==1")["tau"].mean())
print("Pred. Effect: ", df_pred.query("treat==1")["effect_hat"].mean())
```

为了检验这个模型是否有效，我们可以像之前那样把 `treat` 设为零以获取 $Y_0$ 的反事实预测。然后，我们将观察到的处理结果 $Y_1$ 减去 $\hat{Y}_0$ 来估计效应。让我们看看这是否与真实的 ATT 相匹配。


```python
effects = (twfe_model.params[twfe_model.params.index.str.contains("treat")]
           .reset_index()
           .rename(columns={0:"param"})
           .assign(cohort=lambda d: d["index"].str.extract(r'C\(cohort\)\[(.*)\]:'))
           .assign(date=lambda d: d["index"].str.extract(r':C\(date\)\[(.*)\]'))
           .assign(date=lambda d: pd.to_datetime(d["date"]), cohort=lambda d: pd.to_datetime(d["cohort"])))

plt.figure(figsize=(10,4))
sns.lineplot(data=effects, x="date", y="param", hue="cohort")
plt.xticks(rotation=45)
plt.ylabel("Estimated Effect");
```

确实如此！我们终于建立了一个足够灵活的模型来捕捉时间异质性，从而能够估计正确的处理效应！我们还可以做另一件有趣的事情：按时间和 cohort 提取估计的效应并绘制它们。在这种情况下，由于我们知道数据是如何生成的，我们知道应该期待什么。也就是说，对每个 cohort 而言，在处理前效应应为零，处理后第 10 天效应应为 1，并且在处理后到第 10 天之间应是一条从 0 升到 1 的直线。

再一次，这张图与我们对效应的预期相匹配。它们遵循我们前面描述的确切模式。

这已经非常好，但我们还能做得更好。首先注意，这个模型有大量参数。由于我们有 100 个单位和约 92 天的数据，我们知道其中 192 个参数是单位和时间固定效应。即便如此，仍然剩下 250 多个处理效应参数。

如果我们假定在处理前效应为零（没有预期效应），我们可以通过从交互项中删去处理前的日期来减少参数数量。

$$
Installs_{it} = \sum_{g=0}^G \sum_{t=g}^T \tau_{gt} D_{it} + \gamma_i + \theta_t + e_{it}
$$

此外，我们可以从交互项中删去控制 cohort，因为其在处理前的效应始终为零

$$
Installs_{it} = \sum_{G=q}^g \sum_{t=g}^T \tau_{gt} D_{it} + \gamma_i + \theta_t + e_{it}
$$

其中 $g<q$ 的 cohort 被视为控制 cohort。

不过，要用公式实现这一点很棘手，所以我们需要先做一些特征工程。也就是说，我们将手动创建 cohort 虚拟变量，生成一列在 cohort 为 `2021-06-01` 时取 1 否则取 0，另一列在 cohort 为 `2021-07-15` 时取 1 否则取 0。此外，我们还会创建一个日期列，用于 `2021-06-01` cohort，将该 cohort 日期之前的所有日期折叠到一个 `control` 类别。对于 `2021-07-15` cohort 的日期，我们可以做类似的处理。下面是代码示例。


```python
def feature_eng(df):
    return (
        df
        .assign(date_0601 = np.where(df["date"]>="2021-06-01", df["date"], "control"),
                date_0715 = np.where(df["date"]>="2021-07-15", df["date"], "control"),)
        .assign(cohort_0601 = (df["cohort"]=="2021-06-01").astype(float),
                cohort_0715 = (df["cohort"]=="2021-07-15").astype(float))
    )

formula = f"""installs ~ treat:cohort_0601:C(date_0601) 
                       + treat:cohort_0715:C(date_0715) 
                       + C(unit) + C(date)"""

twfe_model = smf.ols(formula, data=df_heter_str.pipe(feature_eng)).fit()
```

如果我们现在像之前那样进行反事实预测，可以看到估计的效应仍然完全吻合真实效应。这里的好处是我们有一个简单得多的模型，只有大约 80 个处理效应参数（记住，其中 192 个参数是时间和单位固定效应）。


```python
df_pred = (df_heter
           .assign(**{"installs_hat_0": twfe_model.predict(df_heter_str
                                                           .pipe(feature_eng)
                                                           .assign(**{"treat":0}))})
           .assign(**{"effect_hat": lambda d: d["installs"] - d["installs_hat_0"]}))


print(len(twfe_model.params))
print("True Effect: ", df_pred.query("treat==1")["tau"].mean())
print("Pred Effect: ", df_pred.query("treat==1")["effect_hat"].mean())
```

绘制这些处理效应参数，可以看到我们已经移除了控制 cohort 的那些以及 cohort 被处理前日期的那些。


```python
effects = (twfe_model.params[twfe_model.params.index.str.contains("treat")]
           .reset_index()
           .rename(columns={0:"param"})
           .assign(cohort=lambda d: d["index"].str.extract(r':cohort_(.*):'),
                   date_0601=lambda d: d["index"].str.extract(r':C\(date_0601\)\[(.*)\]'),
                   date_0715=lambda d: d["index"].str.extract(r':C\(date_0715\)\[(.*)\]'))
           .assign(date=lambda d: pd.to_datetime(d["date_0601"].combine_first(d["date_0715"]), errors="coerce")))

           
plt.figure(figsize=(10,4))
sns.lineplot(data=effects.dropna(subset=["date"]), x="date", y="param", hue="cohort")
plt.xticks(rotation=45);
```

注意我们还可以进一步简化，因为两个 cohort 的效应遵循相同的形状。具体而言，我们可以限制模型让两个 cohort 的效应相同，只随时间变化。为此，我们需要创建一列表示距离处理后的天数，就像事件研究设计那样：

```
days_after_treat=1(date>cohort)*(date - cohort)
```

然后将其与处理指示变量交互：

```
installs ~ treat:C(days_after_treat) + C(unit) + C(date)
```

不过，我认为我们可以到此为止。通常不允许 cohort 之间的异质性是一个坏主意，因为处理效应倾向于随着日历时间变化，而不仅仅是随着距离处理的时间变化。例如，可能一段时间后，竞争对手复制了我们的功能，使其不再像以前那样具有吸引力。在这种情况下，新功能对安装量的效应会随着时间减弱。

除了展示随时间变化的效应，我们还应该做的一件事是绘制反事实，以判断它们是否在合理的位置。我知道这不是对模型特别科学的验证，但相信我，它很有帮助。下面是结果。


```python
twfe_model_wrong = smf.ols("installs ~ treat + C(date) + C(unit)",
                           data=df_pred).fit()


df_pred = (df_pred
           .assign(**{"installs_hat_0_wrong": twfe_model_wrong.predict(df_pred.assign(**{"treat":0}))}))


plt.figure(figsize=(10,4))
sns.lineplot(
    data=(df_pred
          [(df_pred["cohort"].astype(str) > "2021-06-01") & (df_pred["date"].astype(str) >= "2021-06-01")]
          .groupby(["date"])["installs_hat_0"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs_hat_0",
    ls="dotted",
    color="C3",
    label="counterfactual",
)

sns.lineplot(
    data=(df_pred
          .groupby(["cohort", "date"])["installs"]
          .mean()
          .reset_index()),
    x="date",
    y = "installs",
    hue="cohort",
    legend=None
)

plt.ylabel("Installs");
```

正如我们所看到的，反事实 $Y_0$ 的预测正好落在我们认为应该落的地方，即非常接近控制 cohort。这令人欣慰。我们知道 TWFE 模型通过比较处理 cohort 的结果与该反事实来估计处理效应，即 $Y - \hat{Y_0}$。既然反事实看起来没问题，我们可以放心处理效应也大概率没问题。

这是好消息，但别以为我忘了之前答应你的坏消息。尽管我们解决了 TWFE 的函数形式问题，但 DiD 和 TWFE 还有一个更大的问题，那就是其独立性假设。

在使用 DiD 和 TWFE 时，我们经常引入平行趋势假设，却不真正思考该假设究竟意味着什么。遗憾的是，平行趋势假设比大多数人意识到的要严格得多、可信度也低得多。但由于这一章已经非常长，我觉得可以在此打住，让我们享受一下 DiD 的小胜利。

## 关键观点

我想可以肯定地说，我们不仅理解了 TWFE 的函数形式问题，还成功地修正了它。我们追溯问题的根源（时间异质性），并通过提供更多的灵活性加以修复。现在我们可以去喝一杯放松一下，因为 TWFE 再次看起来可以安全使用。或者……真的可以吗？

![img](./images/24/twfeworking.png)

我们千万不要忘记，TWFE（更广义上的 DiD）混合了**函数形式假设和独立性假设**。在本章中，我们仅解决了函数形式的问题，但屋子里仍有一个大象：平行趋势假设。平行趋势是 DiD 所做的独立性假设。这很有名。但我觉得我们并不真正理解这一假设的含义。我们只是凭空假设它成立，好像这样就能让它成为现实。不幸的是，平行趋势假设要求的条件远比大多数人意识到的多。在接下来的章节中，我们将看到为什么会这样，以及我们是否能对此有所作为。

## 参考说明

这一章写了很久。最近的计量经济学文献涌现了许多关于 DiD 问题及其解决方法的新思想和见解。我们可以从多个角度来看这些问题，从而可以利用多种方法来解决它们。请做好准备，因为这里的参考文献列表将会很长（而且可能组织得并不那么好）。

首先，我大量参考了 Andrew Goodman‑Bacon 的《Difference‑in‑Differences with Variation in Treatment Timing》。他对问题的诊断非常巧妙直观，而且文中有一些很棒的图帮助我清楚理解 DiD 发生了什么。本章中的一些图片几乎直接复制自 Goodman‑Bacon 的作品。

第二个主要灵感来源是 Pedro H.C. Sant'Anna 和 Brantly Callaway 的《Difference‑in‑Differences with Multiple Time Periods》。注意 Callaway 和 Sant'Anna 对这一问题的解决思路与我们采取的路线不同。不过，他们的解决方案为理解 DiD 问题提供了很多启示，使之容易理解。除此之外，Pedro 有一篇很好的博客文章展示 TWFE 的问题。那篇博客中的数据生成过程深深启发了我在这里使用的示例。我基本上是把 Pedro 的代码从 R 翻译成 Python。Pedro 还非常友好地帮我解答了关于 DiD 假设的一些问题。他还有另一篇很有趣的文章讨论了在 DiD 模型中加入协变量的问题，这里没有涉及，因为章节已经很长了。这篇文章名为《Doubly Robust Difference‑in‑Differences Estimators》，如果你打算在模型中加入协变量，我强烈建议你阅读。

最后，但绝不是最不重要的，我们在这里使用的函数形式修正灵感来自 Sun 与 Abraham 的《Estimating dynamic treatment effects in event studies with heterogeneous treatment effects》以及 Jeffrey Wooldridge 的《Two‑Way Fixed Effects, the Two‑Way Mundlak Regression, and Difference‑in‑Differences Estimators》。虽然我很喜欢 Callaway 和 Sant'Anna 在他们的论文中所做的工作，但他们的解决方案实现起来略为复杂。相比之下，Sun、Abraham 和 Wooldridge 的解决方案只需要在 TWFE 回归模型中巧妙地处理交互项，用 `statsmodels` 和一些公式就能轻松实现。

除了上述论文，我还参考了 Taylor Wright 在 YouTube 上组织的 DiD Study Group 中的演示。在听作者亲自讲解这些文章之后再去阅读文章，会容易理解得多。我还非常感谢徐亦青教授，他在 YouTube 上的“Panel Data 的因果推断”课程中将这一切串联起来。

最后，请记住我也在学习。因此，如果你发现任何荒谬的地方，请提出 issue，我会尽力处理。

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 25 - 合成双重差分

在前面的章节里，我们已经讨论了双重差分（Difference‑in‑Differences，DiD）和合成控制（Synthetic Control）两种方法，用面板数据（即在多个时间段观察多个单位的数据）识别处理效应。事实证明，我们可以将这两种方法合并为一个新的估计器。这种新的**合成双重差分**估计程序同时利用两种方法的优点，并提高了处理效应估计的精度（缩小误差区间）。

本章主要讨论**块式处理分配**的情况。这意味着我们在多个时间段观察多个单位，并且**在同一时间**只有一部分单位接受处理，而另一部分单位保持未处理。可以将这种分配表示为一个处理指示矩阵 $D$，其中**矩阵的列对应单位**，**矩阵的行对应时间段**：

$$
D = \begin{bmatrix}
    0 & 0 & 0 & \dots & 0 & 0 \\
    0 & 0 & 0 & \dots & 0 & 0 \\
    \vdots \\
    0 & 0 & 0 & \dots & 1 & 1 \\
    0 & 0 & 0 & \dots & 1 & 1 \\
\end{bmatrix}
$$

为了更具体一些，我们继续以加利福尼亚州通过 **Proposition 99** 对香烟消费的影响为例。在这个例子中，我们只有一个被处理的单位——加利福尼亚州——它在某个时间（1988 年 11 月）通过了提案。如果我们把加州作为矩阵的最后一列，那么矩阵变成：

$$
D = \begin{bmatrix}
    0 & 0 & 0 & \dots & 0 & 0 \\
    0 & 0 & 0 & \dots & 0 & 0 \\
    \vdots \\
    0 & 0 & 0 & \dots & 0 & 1 \\
    0 & 0 & 0 & \dots & 0 & 1 \\
\end{bmatrix}
$$

这里我们只讨论所有被处理单位在**同一时间**接受处理的情况。最后，我们还将讨论如何处理**分批（staggered）采用的处理分配**，即处理逐步推广至各单位，导致它们在不同时间受到处理。此设计唯一要求的是：一旦一个单位接受处理，它就不会回到未处理状态。

回到简单情况，即所有单位在同一时间接受处理，我们可以把处理指示矩阵简化为四个块，每个块对应一个矩阵。一般来说，矩阵向下表示时间向后，我们也把被处理的单位分组放在矩阵右边。这样，矩阵左上角（第一块）对应处理前的控制单位；右上角（第二块）对应处理前的被处理单位；左下角（第三块）包含处理后的控制单位；右下角（第四块）是处理后的被处理单位。处理指示在所有位置为零，除了“处理后且被处理”这一块：

$$
D = \begin{bmatrix}
    \pmb{0} & \pmb{0} \\
    \pmb{0} & \pmb{1} \\
\end{bmatrix}
$$

这种分配矩阵会导致下列结果矩阵：

$$
Y = \begin{bmatrix}
    \pmb{Y}_{pre, co} & \pmb{Y}_{pre, tr} \\
    \pmb{Y}_{post, co} & \pmb{Y}_{post, tr} \\
\end{bmatrix}
$$

再次注意，处理后期在矩阵的下方，被处理单位在右侧。

在分析 Proposition 99 的效应时，结果 $Y$ 是香烟销售量。我们用 $pre$ 和 $post$ 分别表示处理前和处理后时期，用 $co$ 和 $tr$ 分别表示控制单位和被处理单位。

在估计合成控制权重时我们将使用上述矩阵表示，但还有另一种数据表示方式也很有用，特别是在讨论双重差分时。这种表示是一个包含 5 列的表：一列表示单位，一列表示时间段，一列是结果变量，还有两列布尔变量分别标记“该单位是否属于处理组”和“该时期是否属于处理后”。该表的行数等于单位数 $N$ 乘以时期数 $T$。对于 Proposition 99 的数据，它看起来是这样的：


```python
import numpy as np
import pandas as pd
from toolz import curry, partial
import seaborn as sns
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf
import cvxpy as cp

import warnings
warnings.filterwarnings('ignore')

from matplotlib import style
style.use("ggplot")

pd.set_option('display.max_columns', 10)
```

```python
data = (pd.read_csv("data/smoking.csv")[["state", "year", "cigsale", "california", "after_treatment"]]
        .rename(columns={"california": "treated"})
        .replace({"state": {3: "california"}}))

data.head()
```

```python
data.query("state=='california'").query("year.between(1986, 1990)")
```

如果想从这种表格形式回到我们之前讨论的矩阵表示，只需按时间（年份）和单位（州）进行透视。我们将在这两种表示之间来回转换，因为一种表示更适用于双重差分，另一种更适用于合成控制的估计。


```python
data_piv = data.pivot(index="year", columns="state", values="cigsale")
data_piv = data_piv.rename(columns={c: f"state_{c}" for c in data_piv.columns if c != "california"})
data_piv.head()[["state_1", "state_2", "state_4", "state_38", "state_39", "california"]].round()
```

从潜在结果的角度，我们可以回到结果矩阵，以重新审视我们的因果推断目标。由于处理只在处理后时期施加于被处理单位，我们在整个矩阵中都观察到潜在结果 $Y_0$，除了右下角那一块。

$$
Y = \begin{bmatrix}
    \pmb{Y}(0)_{pre, co} & \pmb{Y}(0)_{pre, tr} \\
    \pmb{Y}(0)_{post, co} & \pmb{Y}(1)_{post, tr} \\
\end{bmatrix}
$$

我们的目标是估计 $ATT =  \pmb{Y}(1)_{post, tr} -  \pmb{Y}(0)_{post, tr}$。为此，我们需要以某种方式估计缺失的潜在结果 \(\pmb{Y}(0)_{post, tr}\)。换句话说，我们要知道：在处理后时期，如果该单位没有接受处理，其结果会是多少。考虑到这一点，一个很好的起点是回顾双重差分和合成控制。这两种方法乍看起来采取了截然不同的方式来估计这一缺失的潜在结果，将它们结合起来似乎很奇怪。不过，它们之间的共同之处要比你想象的多。


## 双重差分回顾

在“双重差分”章节中，我们通过估计下面的线性模型来得到处理效应：

$$
Y_{it} = \beta_0 + \beta_1 Post_t + \beta_2 Treated_i + \beta_3 Treated_i\, Post_t + e_{it}
$$

其中 `post` 是一个时间虚拟变量，表示该时期是在处理之后，`treated` 是一个单位虚拟变量，用来标记该单位是否属于处理组。在加州的例子中估计该模型，我们得到 $ATT=-27.34$，表明 Proposition 99 对香烟消费的影响是显著负的。这意味着人均香烟消费减少了约 27 包。


```python
did_model = smf.ols("cigsale ~ after_treatment*treated", data=data).fit()
att = did_model.params["after_treatment[T.True]:treated[T.True]"]
print("DiD ATT: ", att.round(3))
```

```python
pre_year = data.query("~after_treatment")["year"].mean()
post_year = data.query("after_treatment")["year"].mean()

pre_control_y = did_model.params["Intercept"]
post_control_y = did_model.params["Intercept"] + did_model.params["after_treatment[T.True]"]

pre_treat_y = did_model.params["Intercept"] + did_model.params["treated[T.True]"]

post_treat_y0 = post_control_y + did_model.params["treated[T.True]"]

post_treat_y1 = post_treat_y0 + did_model.params["after_treatment[T.True]:treated[T.True]"]

plt.plot([pre_year, post_year], [pre_control_y, post_control_y], color="C0", label="Control")
plt.plot([pre_year, post_year], [pre_treat_y, post_treat_y0], color="C1", ls="dashed")
plt.plot([pre_year, post_year], [pre_treat_y, post_treat_y1], color="C1", label="California")

plt.vlines(x=1988, ymin=40, ymax=140, linestyle=":", lw=2, label="Proposition 99", color="black")
plt.title("DiD Estimation")
plt.ylabel("Cigarette Sales")
plt.legend();
```

不过，这个估计值要谨慎看待。我们知道，双重差分假设在没有处理的情况下，控制组的趋势应该与处理组相同。形式上是 $E[Y(0)_{post, co} - Y(0)_{pre, co}] = E[Y(0)_{post, tr} - Y(0)_{pre, tr}]$。这是一个无法检验的假设，但通过查看加州（被处理单位）和其他州的处理前趋势，我们可以判断其合理性。具体地，我们会发现加州的 `cigsale` 下降速度快于其他州的平均水平，至少在处理前时期就是如此。如果这种趋势在处理后继续存在，那么双重差分估计器将会向下偏，意味着真实的效应其实没有我们上面估计的那么极端。


```python
plt.figure(figsize=(10,5))
plt.plot(data_piv.drop(columns=["california"]), color="C1", alpha=0.3)
plt.plot(data_piv.drop(columns=["california"]).mean(axis=1), lw=3, color="C1", ls="dashed", label="Control Avg.")
plt.plot(data_piv["california"], color="C0", label="California")
plt.vlines(x=1988, ymin=40, ymax=300, linestyle=":", lw=2, label="Proposition 99", color="black")
plt.legend()
plt.ylabel("Cigarette Sales")
plt.title("Non-Parallel Trends");
```

非平行趋势的问题正是合成控制在合成双重差分模型中发挥作用的地方。不过我们先不要跑得太快。无论上面的数据是否适合用双重差分建模，令人感兴趣的是，我们可以把 DiD 重写成双向固定效应（Two‑Way Fixed Effects，TWFE）的形式。为了这样表述 DiD，我们会拟合单位效应（$\alpha_i$）和时间效应（$\beta_t$），再加上处理指示变量：

$$
\hat{\tau}^{did} = \underset{\mu, \alpha, \beta, \tau}{\arg\min} \bigg\{ \sum_{i=1}^N \sum_{t=1}^T \big(Y_{it} - (\mu + \alpha_i + \beta_t + \tau D_{it}\big)^2 \bigg\}
$$

在这种表述中，单位效应捕捉各单位截距的差异，而时间效应捕捉处理组和控制组共同的整体趋势。实现这一点，我们可以在回归中加入时间和单位的虚拟变量，也可以将数据去均值。在去均值过程中，我们同时从结果和处理变量中减去跨时间和跨单位的平均值：

$$
\ddot{Y}_{it} = Y_{it} - \bar{Y}_i  - \bar{Y}_t\\
\ddot{D}_{it} = D_{it} - \bar{D}_i - \bar{D}_t
$$

其中，$\bar{X}_i$ 是单位 $i$ 在所有时间段的平均值，$\bar{X}_t$ 是时间 $t$ 在所有单位的平均值：

$$
\ddot{Y}_{it} = Y_{it} - T^{-1}\sum_{t=0}^{T} Y_{it}  - N^{-1}\sum_{i=0}^{N} Y_{it}\\
\ddot{D}_{it} = D_{it} - T^{-1}\sum_{t=0}^{T} D_{it} - N^{-1}\sum_{i=0}^{N} D_{it}
$$

在去均值之后，只需用 `treat*post` 回归结果变量，就可以得到双重差分估计量。


```python
@curry
def demean(df, col_to_demean):
    return df.assign(**{col_to_demean: (df[col_to_demean]
                                        - df.groupby("state")[col_to_demean].transform("mean")
                                        - df.groupby("year")[col_to_demean].transform("mean"))})

formula = f"""cigsale ~ treat"""
mod = smf.ols(formula,
              data=data
              .assign(treat = data["after_treatment"]*data["treated"])
              .pipe(demean(col_to_demean="treat"))
              .pipe(demean(col_to_demean="cigsale")))

mod.fit().summary().tables[1]
```

正如你所见，我们得到的参数与之前完全相同。毕竟，这两种做法只是看待同一双重差分估计器的不同方式。然而，这种表述之所以更有趣，是因为它让我们看到 DiD 实际上与合成控制非常相似。仔细观察上面的 TWFE 表述，它是一个同时包含时间效应和单位效应的回归问题。但请注意，其优化目标里没有任何权重。这正是双重差分与合成控制的主要区别，稍后我们会看到。

## 合成控制回顾

在经典的合成控制估计器中，我们寻找单位（州）权重，使得被处理单位在处理前的结果与控制单位的加权平均结果之间的差异最小（在没有协变量的情况下）。同时要求这些权重为正且和为 1。为找到这些权重，我们求解以下优化问题：

$$
\hat{w}^{sc} = \underset{w}{\mathrm{argmin}}\; \|\pmb{\bar{y}}_{pre, tr} - \pmb{Y}_{pre, co} \pmb{w}_{co}\|^2_2 \\
\text{s.t. } \sum w_i = 1 \text{ 且 } w_i > 0 \; \forall i
$$

其中，结果矩阵 \(\pmb{Y}_{pre, co}\) 是一个 \(T_{pre}\times N_{co}\) 的矩阵，列对应单位，行对应时间段；\(\pmb{w}_{co}\) 是一个 \(N_{co}\times 1\) 的列向量，每个元素对应一个单位。最后，\(\pmb{\bar{y}}_{pre, tr}\) 是一个 \(T_{pre}\times 1\) 的列向量，其中的每个元素都是被处理单位在处理前时期的时间平均。这就是我们有时将合成控制称为“横向回归”的原因：在大多数回归中，单位是矩阵的行，但在这里单位是列。我们将被处理单位的平均结果回归到控制单位上。

一旦找到了满足上述优化问题的权重，就可以在所有时间段上将它们与控制单位相乘，得到被处理单位的合成控制：

$$
\pmb{y}_{sc} = \pmb{Y}_{co}\hat{\pmb{w}}^{sc}
$$

此时认为 \(\pmb{y}_{post, sc}\) 是缺失潜在结果 \(Y(0)_{post, tr}\) 的良好估计。如果情况如此，ATT 就是处理后时期被处理单位的平均结果减去合成控制的平均结果：

$$
\hat{\tau} =  \bar{y}_{post, tr} - \bar{y}_{post, sc}
$$


```python
from sc import SyntheticControl


sc_model = SyntheticControl()

y_co_pre = data.query("~after_treatment & ~treated").pivot(index="year", columns="state", values="cigsale")
y_tr_pre = data.query("~after_treatment & treated").set_index("year")["cigsale"]

sc_model.fit(y_co_pre, y_tr_pre)

sc_weights = pd.Series(sc_model.w_, index=y_co_pre.columns, name="sc_w")
sc = data.query("~treated").pivot(index="year", columns="state", values="cigsale").dot(sc_weights)

att = data.query("treated").set_index("year")["cigsale"][sc.index > 1988].mean() - sc[sc.index > 1988].mean()

print("SC ATT: ", att.round(4))
```

这个估计值要比我们用双重差分得到的要小得多。合成控制能够更好地处理处理前的非平行趋势，因此不会像双重差分那样受到同样的偏误。实际上，构造合成控制的过程在处理前时期强制让趋势平行。结果是，我们得到的估计更小，也更合理。

我们可以通过绘制加州的实际结果与合成控制的结果来直观展示这种估计过程。我们还用虚线绘制了干预后加州和合成控制的平均值。这两条水平线之间的差异即为估计的 $ATT$。


```python
plt.plot(sc, label="Synthetic Control")
plt.plot(sc.index, data.query("treated")["cigsale"], label="California", color="C1")

calif_avg = data.query("treated")["cigsale"][sc.index > 1988].mean()
sc_avg = sc[sc.index > 1988].mean()

plt.hlines(calif_avg, 1988, 2000, color="C1", ls="dashed")
plt.hlines(sc_avg, 1988, 2000, color="C0", ls="dashed")

plt.title("SC Estimation")
plt.ylabel("Cigarette Sales")
plt.vlines(x=1988, ymin=40, ymax=140, linestyle=":", lw=2, label="Proposition 99", color="black")
plt.legend();
```

有意思的是，我们还可以将合成控制估计器重写为如下的优化问题，这与我们用在双重差分中的双向固定效应形式非常相似：

$$
\hat{\tau}^{sc} = \underset{\beta, \tau}{\arg\min} \bigg\{ \sum_{i=1}^N \sum_{t=1}^T \big(Y_{it} - \beta_t - \tau D_{it}\big)^2 \hat{w}^{sc}_i \bigg\}
$$

其中，控制单位的权重 $\hat{w}^{sc}_i$ 来自之前的优化问题。对于被处理单位，其权重简单地设为 $1/N_{tr}$（均匀权重）。

请注意这里合成控制和双重差分的区别。首先，合成控制在优化目标中引入了单位权重 $\hat{w}^{sc}_i$；其次，它包含时间固定效应 $\beta_t$，但没有单位固定效应 $\alpha_i$，也没有总体截距……


```python
@curry
def demean_time(df, col_to_demean):
    return df.assign(**{col_to_demean: (df[col_to_demean]
                                        - df.groupby("year")[col_to_demean].transform("mean"))})

data_w_cs_weights = data.set_index("state").join(sc_weights).fillna(1/len(sc_weights))

formula = f"""cigsale ~ -1 + treat"""

mod = smf.wls(formula,
              data=data_w_cs_weights
              .assign(treat = data_w_cs_weights["after_treatment"]*data_w_cs_weights["treated"])
              .pipe(demean_time(col_to_demean="treat"))
              .pipe(demean_time(col_to_demean="cigsale")),
              weights=data_w_cs_weights["sc_w"]+1e-10)

mod.fit().summary().tables[1]
```

我们刚刚看到，SC 和 DiD 这两种方法其实关系密切。现在我们准备介绍合成双重差分（Synthetic Diff‑in‑Diff）。顾名思义，我们会在 DiD 估计器中加入权重，或者在合成控制估计器中加入单位固定效应。

![img](./images/25/both-pills.png)

## 合成双重差分

在正式介绍合成双重差分估计器之前，我们先把前面 SC 和 DiD 的方程列出来，便于比较。

$$
\hat{\tau}^{sc} = \underset{\beta, \tau}{\arg\min}  \bigg\{ \sum_{i=1}^N \sum_{t=1}^T \big(Y_{it} - \beta_t - \tau D_{it}\big)^2 \hat{w}^{sc}_i \bigg\}
$$

$$
\hat{\tau}^{did} = \underset{\mu, \alpha, \beta, \tau}{\arg\min} \bigg\{ \sum_{i=1}^N \sum_{t=1}^T \big(Y_{it} - (\mu + \alpha_i + \beta_t + \tau D_{it}\big)^2 \bigg\}
$$

接下来，正如我所承诺的，我们可以轻松将上述方程合并为一个，同时包含两者的元素：

$$
\hat{\tau}^{sdid} = \underset{\mu, \alpha, \beta, \tau}{\arg\min}  \bigg\{ \sum_{i=1}^N \sum_{t=1}^T \big(Y_{it} - (\mu + \alpha_i + \beta_t + \tau D_{it}\big)^2 \hat{w}^{sdid}_i \hat{\lambda}^{sdid}_t \bigg\}
$$

如你所见，我们把单位固定效应 \(\alpha_i\) 加回来了，同时保留了单位权重 \(\hat{w}_i\)。但这里还有一个新的东西，即时间权重 \(\hat{\lambda}_t\)。别担心，它们并不复杂。回忆一下单位权重 \(w_i\) 是如何使控制组的结果与被处理组的平均结果之间的差异最小化的；也就是说，我们用它们来匹配处理前的趋势。时间权重做的事情类似，只不过它匹配的是处理前和处理后时期之间的差异，它使控制组的处理前期和处理后期的平均值尽可能接近。

$$
\hat{\lambda}^{sdid} = \underset{\lambda}{\mathrm{argmin}} \; \|\bar{\pmb{y}}_{post, co} - (\pmb{\lambda}_{pre} \pmb{Y}_{pre, co} +  \lambda_0)\|^2_2 \\
\text{s.t. } \sum \lambda_t = 1 \text{ 且 } \lambda_t > 0 \; \forall t
$$

这里，\(\pmb{Y}_{pre, co}\) 是一个 \(T_{pre}\times N_{co}\) 的结果矩阵，行代表时间段，列代表单位。而 \(\bar{\pmb{y}}_{post, co}\) 是一个 \(1\times N_{co}\) 的行向量，其每个元素是对应控制单位在处理后时期的时间平均结果。\(\pmb{\lambda}_{pre}\) 则是一个 \(1\times T_{pre}\) 的行向量，每个元素对应一个处理前时期。换个角度看，单位权重 \(w\) 是右乘在结果矩阵上的，即 \(\pmb{Y}_{pre, co} \pmb{w}_{co}\)。这意味着我们将被处理单位在每个时间段的平均结果回归到控制单位的结果上。而现在，我们把问题翻转过来，将控制组**每个处理后时期**的平均结果回归到同一控制组在处理前的结果上。

至于处理后的时间权重，我们简单地设为 $1/T_{post}$（即均匀权重）。注意还有一个截距 \(\lambda_0\)。我们这样做是为了允许处理后时期的平均水平高于或低于所有处理前时期，这在许多具有明显趋势的应用中是常见的。

如果这些仍然有些抽象，那么接下来的代码会帮助你理解正在发生的事情。


```python
def fit_time_weights(data, outcome_col, year_col, state_col, treat_col, post_col):
        
        control = data.query(f"~{treat_col}")
        
        # pivot the data to the (T_pre, N_co) matrix representation
        y_pre = (control
                 .query(f"~{post_col}")
                 .pivot(index=year_col, columns=state_col, values=outcome_col))
        
        # group post-treatment time period by units to have a (1, N_co) vector.
        y_post_mean = (control
                       .query(f"{post_col}")
                       .groupby(state_col)
                       [outcome_col]
                       .mean()
                       .loc[y_pre.columns]  # 保证顺序一致
                       .values)
        
        # add a (1, N_co) vector of 1 to the top of the matrix, to serve as the intercept.
        X = np.concatenate([np.ones((1, y_pre.shape[1])), y_pre.values], axis=0)
        
        # estimate time weights
        w = cp.Variable(X.shape[0])
        objective = cp.Minimize(cp.sum_squares(w@X - y_post_mean))
        constraints = [cp.sum(w[1:]) == 1, w[1:] >= 0]
        problem = cp.Problem(objective, constraints)
        problem.solve(verbose=False)
        
        # print("Intercept: ", w.value[0])
        return pd.Series(w.value[1:], # remove intercept
                         name="time_weights",
                         index=y_pre.index)
```

在代码中，我们首先过滤掉被处理组。然后对处理前数据进行透视，得到矩阵 \(\pmb{Y}_{pre,co}\)。接下来，我们对处理后数据按单位分组，计算每个控制单位在处理后时期的平均结果。然后，在 \(\pmb{Y}_{pre,co}\) 的顶部添加一行全为 1 的行，这行将用作截距。最后，我们将 \(\bar{\pmb{y}}_{post, co}\) 回归到这些处理前时期（\(\pmb{Y}_{pre,co}\) 的行）上，以得到时间权重 \(\lambda_t\)。注意我们在回归时加入了权重和为 1 且非负的约束。最后，我们将截距丢弃，并将时间权重存储到一个序列中。

下面是运行上述代码在 Proposition 99 问题中得到的时间权重的结果。


```python
time_weights = fit_time_weights(data,
                                outcome_col="cigsale",
                                year_col="year",
                                state_col="state",
                                treat_col="treated",
                                post_col="after_treatment")

time_weights.round(3).tail()
```

为了更好地理解这些权重的作用，我们可以绘制 \(\hat{\pmb{\lambda}}_{pre}\, \pmb{Y}_{pre, co} +  \hat{\lambda}_0\) 作为处理前时期的一条水平线（不会被归零的部分）。在它旁边，我们绘制处理后时期的平均结果。请注意，这两条线完全对齐。我们还在次坐标轴上用红色条形展示估计的时间权重。


```python
fig = plt.figure()
ax = fig.add_subplot(111)

ax.plot(data.query("~treated").query("~after_treatment").groupby("year")["cigsale"].mean())
ax.plot(data.query("~treated").query("after_treatment").groupby("year")["cigsale"].mean())

intercept = -15.023877689807628
ax.hlines((data.query("~treated").query("~after_treatment").groupby("year")["cigsale"].mean() * time_weights).sum() - 15, 1986, 1988,
          color="C0", ls="dashed", label=""" $\lambda_{pre} Y_{pre, co} + \lambda_0$""")
ax.hlines(data.query("~treated").query("after_treatment").groupby("year")["cigsale"].mean().mean(), 1988, 2000,
          color="C1", ls="dashed", label="""Avg  $Y_{post, co}$""")
ax.vlines(x=1988, ymin=90, ymax=140, linestyle=":", lw=2, label="Proposition 99", color="black")
plt.legend()

plt.title("Time Period Balancing")
plt.ylabel("Cigarette Sales");

ax2 = ax.twinx()
ax2.bar(time_weights.index, time_weights, label="$\lambda$")
ax2.set_ylim(0,10)
ax2.set_ylabel("Time Weights");
```

在了解了合成双重差分估计器中的时间权重 \(\lambda_t\) 及其估计方法之后，现在让我们把注意力转向单位权重 \(w_i\)。它们并不完全像传统合成控制中的权重。第一点不同是我们允许存在一个截距 $w_0$。这样做是因为我们不再要求被处理单位与合成控制在同一水平上。由于我们要与双重差分结合，只需要合成控制与被处理单位具有平行趋势即可。

第二点不同是我们在权重中加入了一个 $L_2$ 正则化项。这样可以使非零权重更均匀地分散到各控制单位，而不是只有少数几个单位对合成控制起作用。


```python
def calculate_regularization(data, outcome_col, year_col, state_col, treat_col, post_col):
    
    n_treated_post = data.query(post_col).query(treat_col).shape[0]
    
    first_diff_std = (data
                      .query(f"~{post_col}")
                      .query(f"~{treat_col}")
                      .sort_values(year_col)
                      .groupby(state_col)
                      [outcome_col]
                      .diff()
                      .std())
    
    return n_treated_post**(1/4) * first_diff_std
```

至于单位权重，其估计并没有太多新的东西。我们可以复用用于估计时间权重的函数代码，只需注意维度，因为这个问题现在上下颠倒。


```python
def fit_unit_weights(data, outcome_col, year_col, state_col, treat_col, post_col):
    
    zeta = calculate_regularization(data, outcome_col, year_col, state_col, treat_col, post_col)
    pre_data = data.query(f"~{post_col}")
    
    # pivot the data to the (T_pre, N_co) matrix representation
    y_pre_control = (pre_data
                     .query(f"~{treat_col}")
                     .pivot(index=year_col, columns=state_col, values=outcome_col))
    
    # group treated units by time periods to have a (T_pre, 1) vector.
    y_pre_treat_mean = (pre_data
                        .query(f"{treat_col}")
                        .groupby(year_col)
                        [outcome_col]
                        .mean())
    
    # add a (T_pre, 1) column to the begining of the (T_pre, N_co) matrix to serve as intercept
    T_pre = y_pre_control.shape[0]
    X = np.concatenate([np.ones((T_pre, 1)), y_pre_control.values], axis=1) 
    
    # estimate unit weights. Notice the L2 penalty using zeta
    w = cp.Variable(X.shape[1])
    objective = cp.Minimize(cp.sum_squares(X@w - y_pre_treat_mean.values) + T_pre*zeta**2 * cp.sum_squares(w[1:]))
    constraints = [cp.sum(w[1:]) == 1, w[1:] >= 0]
    
    problem = cp.Problem(objective, constraints)
    problem.solve(verbose=False)
    
    # print("Intercept:", w.value[0])
    return pd.Series(w.value[1:], # remove intercept
                     name="unit_weights",
                     index=y_pre_control.columns)

```

首先，我们用前面定义的函数计算 \(\zeta\)，并过滤掉处理后时期。接下来，将处理前的数据透视成 \(\bar{\pmb{y}}_{pre, tr}\) 的结果矩阵。然后，在 \(\bar{\pmb{y}}_{pre, tr}\) 的开头加上一列全为 1 的列，这一列使我们能够估计截距。有了这些，我们便可以定义包含权重 $L_2$ 正则化的优化目标。最后，我们丢弃截距，把估计得到的权重存储在一个序列中。

如果用这段代码在 Proposition 99 问题中估计单位权重，下面是前 5 个州得到的结果：


```python
unit_weights = fit_unit_weights(data,
                                outcome_col="cigsale",
                                year_col="year",
                                state_col="state",
                                treat_col="treated",
                                post_col="after_treatment")

unit_weights.round(3).head()
```

这些单位权重也定义了一个合成控制，我们可以将其与加州的实际结果一起绘制。我们还将把之前估计的传统合成控制与刚刚估计的（加上截距）合成控制一起绘制。这将帮助我们理解其背后的直觉以及它与传统合成控制之间的差别。


```python
intercept = -24.75035353644767
sc_did = data_piv.drop(columns="california").values @ unit_weights.values

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(18,5))

ax1.plot(data_piv.index, sc_did, label="Synthetic Control (SDID)", color="C0", alpha=.8)
ax1.plot(data_piv["california"], label="California", color="C1")
ax1.vlines(x=1988, ymin=40, ymax=160, linestyle=":", lw=2, label="Proposition 99", color="black")

ax1.legend()
ax1.set_title("SDID Synthetic Control")
ax1.set_ylabel("Cigarette Sales");

ax2.plot(data_piv.index, sc_did+intercept, label="Synthetic Control (SDID) + $w_0$", color="C0", alpha=.8)
ax2.plot(data_piv.index, sc, label="Traditional SC", color="C0", ls="dashed")
ax2.plot(data_piv["california"], label="California", color="C1")
ax2.vlines(x=1988, ymin=40, ymax=160, linestyle=":", lw=2, label="Proposition 99", color="black")
ax2.legend()
ax2.set_title("SDID and Traditional SCs");
```

如我们在第一张图中看到的，最明显的区别是新的合成控制不再与加州的水平一致。这是因为我们加入了一个截距，它允许被处理单位与其合成控制处于完全不同的水平。新的合成控制方法旨在让处理前的趋势与被处理单位一致，但不要求水平相同。

在第二张图中，我们通过重新加上之前移除的截距来平移这个新的合成控制，使其与加州重合。为了比较，我们用红色虚线展示了之前拟合的传统合成控制。注意它们并不相同。这种差异既来自我们允许存在一个截距，又来自 $L_2$ 正则化，它使权重更加分散。


```python
def join_weights(data, unit_w, time_w, year_col, state_col, treat_col, post_col):
    return (
        data
        .set_index([year_col, state_col])
        .join(time_w)
        .join(unit_w)
        .reset_index()
        .fillna({time_w.name: 1 / len(pd.unique(data.query(f"{post_col}")[year_col])),
                 unit_w.name: 1 / len(pd.unique(data.query(f"{treat_col}")[state_col]))})
        .assign(**{"weights": lambda d: (d[time_w.name] * d[unit_w.name]).round(10)})
        .astype({treat_col: int, post_col: int}))
```

这一拼接过程会使被处理组的单位权重和处理后时期的时间权重变成 `null`。幸运的是，由于我们在这两种情况下都使用均匀权重，很容易填补这些 `null`。对于时间权重，用处理后虚拟变量的平均值填补，即 $1/T_{post}$；对于单位权重，用被处理虚拟变量的平均值填补，即 $1/N_{tr}$。最后，将两类权重相乘即可。

下面是在 Proposition 99 数据上运行该代码得到的结果：


```python
did_data = join_weights(data, unit_weights, time_weights,
                        year_col="year",
                        state_col="state",
                        treat_col="treated",
                        post_col="after_treatment")

did_data.head()
```

```python
data["after_treatment"].mean()
```

```python
1/len(data.query("after_treatment==1")["year"].unique())
```

最后，只需使用我们刚刚定义的权重来估计一个加权的双重差分模型。与处理后时期和处理组虚拟变量相互作用项相关联的参数估计就是合成双重差分估计的 $ATT$。


```python
did_model = smf.wls("cigsale ~ after_treatment*treated",
                    data=did_data,
                    weights=did_data["weights"]+1e-10).fit()

did_model.summary().tables[1]
```

这个估计值要比我们用普通双重差分得到的结果小得多，但这并不奇怪。正如我们已经讨论过的，在这里双重差分估计器很可能有偏，因为我们有充分理由质疑平行趋势假设。也许不那么明显的是，为什么 SDID 的估计比传统合成控制的估计要小。如果回头看 SC 的图，会发现早在 Proposition 99 之前，加州的香烟销售就开始低于其合成控制。这可能是因为传统合成控制需要在整个处理前时期匹配处理组和控制组，结果可能会忽略某一年或另一年。而在 SDID 中，这个问题较小，因为时间权重使我们可以仅关注那些真正重要的时期。


```python
avg_pre_period = (time_weights * time_weights.index).sum()
avg_post_period = 1989 + (2000 - 1989) / 2

fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(15,8), sharex=True, gridspec_kw={'height_ratios': [3, 1]})

ax1.plot(data_piv.index, sc_did, label="California")
ax1.plot(data_piv.index, data_piv["california"], label="Synthetic Control (SDID)")
ax1.vlines(1989, data_piv["california"].min(), sc_did.max(), color="black", ls="dotted", label="Prop. 99")

pre_sc = did_model.params["Intercept"]
post_sc = pre_sc + did_model.params["after_treatment"]
pre_treat = pre_sc + did_model.params["treated"]
post_treat = post_sc + did_model.params["treated"] + did_model.params["after_treatment:treated"]

sc_did_y0 = pre_treat + (post_sc - pre_sc)

ax1.plot([avg_pre_period, avg_post_period], [pre_sc, post_sc], color="C2")
ax1.plot([avg_pre_period, avg_post_period], [pre_treat, post_treat], color="C2", ls="dashed")
ax1.plot([avg_pre_period, avg_post_period], [pre_treat, sc_did_y0], color="C2")

ax1.annotate('ATT', xy=(1995, 69), xytext=(1996, 66.5), 
            fontsize=12, ha='center', va='bottom',
            bbox=dict(boxstyle='square', fc='white', color='k'),
            arrowprops=dict(arrowstyle='-[, widthB=1.5, lengthB=0.5', lw=2.0, color='k'))

ax1.legend()
ax1.set_title("Synthetic Diff-in-Diff")
ax1.set_ylabel("Cigarette Sales")

ax2.bar(time_weights.index, time_weights)
ax2.vlines(1989, 0, 1, color="black", ls="dotted")
ax2.set_ylabel("Time Weights")
ax2.set_xlabel("Years");
```

上述估计器估计的是 $ATT$，即 Proposition 99 对加州的平均效应（在所有处理后时期平均）。但从上面的图看，这个效应随着时间推移而增加。如果我们想考虑这种变化怎么办？幸运的是，这非常容易实现。

在继续之前，需要提醒一句：不要相信我们刚刚回归得到的标准误和置信区间。它们没有反映估计权重时的方差。我们稍后会简要讨论如何进行正确的推断，但首先，让我们看看如何处理随时间异质的效应。

## 时间效应异质性与分批采用

幸运的是，我们可以为每一个处理后时期单独估计一个效应……


```python
def synthetic_diff_in_diff(data, outcome_col, year_col, state_col, treat_col, post_col):
    
    # find the unit weights
    unit_weights = fit_unit_weights(data,
                                    outcome_col=outcome_col,
                                    year_col=year_col,
                                    state_col=state_col,
                                    treat_col=treat_col,
                                    post_col=post_col)
    
    # find the time weights
    time_weights = fit_time_weights(data,
                                    outcome_col=outcome_col,
                                    year_col=year_col,
                                    state_col=state_col,
                                    treat_col=treat_col,
                                    post_col=post_col)

    # join weights into DiD Data
    did_data = join_weights(data, unit_weights, time_weights,
                            year_col=year_col,
                            state_col=state_col,
                            treat_col=treat_col,
                            post_col=post_col)
    
    # run DiD
    formula = f"{outcome_col} ~ {post_col}*{treat_col}"
    did_model = smf.wls(formula, data=did_data, weights=did_data["weights"]+1e-10).fit()
    
    return did_model.params[f"{post_col}:{treat_col}"]


synthetic_diff_in_diff(data, 
                       outcome_col="cigsale",
                       year_col="year",
                       state_col="state",
                       treat_col="treated",
                       post_col="after_treatment")
```

现在我们已经可以轻松地运行 SDID，我们可以多次运行它，每次都只保留一个指定的处理后时期，以获得该时期的效应。


```python
effects = {year: synthetic_diff_in_diff(data.query(f"~after_treatment|(year=={year})"), 
                                        outcome_col="cigsale",
                                        year_col="year",
                                        state_col="state",
                                        treat_col="treated",
                                        post_col="after_treatment")
           for year in range(1989, 2001)}

effects = pd.Series(effects)
```

```python
plt.plot(effects);
plt.ylabel("Effect in Cigarette Sales")
plt.title("SDID Effect Estimate by Year");
```

正如预期的那样，随着时间推移，效应不断增大。它起初很小，但逐渐增加，到 2020 年似乎相当于人均减少 25 包香烟。

方便的是，多次运行 SDID 在处理分批采用情形时也非常重要。在分批采用设计中，我们有多个被处理单位，它们在不同的时间接受处理。例如，回到我们非常简单的指派矩阵，假设有 3 个单位和 4 个时间段，单位 1 从未接受处理，单位 2 在第 4 个时间段接受处理，单位 3 在第 3 个时间段接受处理。这样会得到以下矩阵

$$
D = \begin{bmatrix}
    0_1 & 0_1 & 0_1 \\
    0_2 & 0_2 & 0_2 \\
    0_3 & 0_3 & 1_3 \\
    0_4 & 1_4 & 1_4 \\
\end{bmatrix}
$$


```python
np.random.seed(1)
n = 3
tr_state = (data
            .query(f"state.isin({list(np.random.choice(data['state'].unique(), n))})")
            .assign(**{
                "treated": True,
                "state": lambda d: "new_" + d["state"].astype(str),
                "after_treatment": lambda d: d["year"] > 1992
            })
            # effect of 3% / year
            .assign(**{"cigsale": lambda d: np.random.normal(d["cigsale"] - 
                                                             d["cigsale"]*(0.03*(d["year"] - 1992))*d["after_treatment"], 1)}))

new_data = pd.concat([data, tr_state]).assign(**{"after_treatment": lambda d: np.where(d["treated"], d["after_treatment"], False)})
```

```python
new_data_piv = new_data.pivot(index="year", columns="state", values="cigsale")

new_tr_states = list(filter(lambda c: str(c).startswith("new"), new_data_piv.columns))

plt.figure(figsize=(10,5))
plt.plot(new_data_piv.drop(columns=["california"]+new_tr_states), color="C1", alpha=0.3)
plt.plot(new_data_piv.drop(columns=["california"]+new_tr_states).mean(axis=1), lw=3, color="C1", ls="dashed", label="Control Avg.")

plt.plot(new_data_piv["california"], color="C0", label="California")
plt.plot(new_data_piv[new_tr_states].mean(axis=1), color="C4", label="New Tr State")

plt.vlines(x=1988, ymin=40, ymax=300, linestyle=":", lw=2, label="Proposition 99", color="black")
plt.vlines(x=1992, ymin=40, ymax=300, linestyle="dashed", lw=2, label="New State Tr.", color="black")
plt.legend()
plt.ylabel("Cigarette Sales")
plt.title("Two Treatment Groups");
```

我们终于得到了这份分批采用的数据。现在，我们需要想办法过滤掉一些州，以便将问题分解为多个块式指派案例。首先，我们可以按各州通过法律的时间进行分组。下面的代码就做了这件事。


```python
assignment_blocks = (new_data.query("treated & after_treatment")
                     .groupby("state")["year"].min()
                     .reset_index()
                     .groupby("year")["state"].apply(list).to_dict())

assignment_blocks
```

正如你所见，我们有两组州。一组只有加州，它从 1989 年开始受到处理；另一组是我们创建的三个位于 1993 年开始受处理的新州。现在，我们需要分别对这两组运行 SDID。我们只需保留控制单位和其中一组即可轻松做到这一点。不过有一个小问题：`after_treatment` 列的含义将根据所看的组而不同。如果看的是只含加州的那组，那么 `after_treatment` 应该定义为 `year >= 1989`；如果看的是新州那一组，那么 `after_treatment` 应该定义为 `year >= 1993`。幸运的是，这很容易处理，只需要在每次迭代中重新生成 `after_treatment`。


```python
staggered_effects = {year: synthetic_diff_in_diff(new_data
                                                   .query(f"~treated|(state.isin({states}))")
                                                   .assign(**{"after_treatment": lambda d: d["year"] >= year}),
                                                  outcome_col="cigsale",
                                                  year_col="year",
                                                  state_col="state",
                                                  treat_col="treated",
                                                  post_col="after_treatment")
                     for year, states in assignment_blocks.items()}

staggered_effects
```

毫不意外，第一组（只包含加州）的 $ATT$ 估计与之前看到的一模一样。另一组的 $ATT$ 是我们针对新州得到的。我们需要将它们合并为一个总体 $ATT$。这可以用之前解释过的加权平均完成。

首先，计算每个块中处理实例 (`after_treatment \& treated`) 的数量。然后使用这些权重将两个 $ATT$ 组合起来。


```python
weights = {year: sum((new_data["year"] >= year) & (new_data["state"].isin(states)))
           for year, states in assignment_blocks.items()}

att = sum([effect*weights[year]/sum(weights.values()) for year, effect in staggered_effects.items()])

print("weights: ", weights)
print("ATT: ", att)
```

这里，我们总共有 36 个处理实例：加州在处理后时期有 12 个观察值，再加上我们引入的三个新州在 1993–2000 年各有 8 个处理时期。考虑这一点，第一个 $ATT$ 的权重是 $12/36$，第二个 $ATT$ 的权重是 $24/36$，组合起来就得到上面的结果。

## 安慰剂方差估计

本章已经有点长了，但还有一个承诺没有兑现。记得开头我们说过，与合成控制相比，SDID 的精度更高（误差条更小）吗？原因是 SDID 中的时间和单位固定效应捕捉了结果的大部分变异，从而降低了估计量的方差。

当然，我不会让你只凭我的话信服，所以接下来我们将展示如何给 SDID 估计值构建一个置信区间。事实上，有许多方法可以解决这个问题，但在只有一个被处理单位（我们这里就是这种情况，因为只有加州被处理）的情况下，只有一种方法适用。其思路是运行一系列安慰剂检验：假装从控制池中抽取的某个单位被处理，实际上它并没有被处理。然后，我们用 SDID 来估计这个安慰剂检验的 $ATT$ 并保存结果。重复这个步骤多次，每次随机抽取一个控制单位。最后，我们将得到一组安慰剂 $ATT$ 值。这组值的方差就是 SDID 效果估计的安慰剂方差，我们可以用它来构建置信区间。

$$
\hat{V}^{placebo}_{\tau} = B^{-1}\sum_{b=1}^B\bigg(\hat{\tau}^{(b)} - \bar{\hat{\tau}}\bigg)^2
$$

$$
\tau \in \hat{\tau}^{sdid} \pm \mathcal{z}_{\alpha/2} \sqrt{\hat{V}_{\tau}}
$$


```python
def make_random_placebo(data, state_col, treat_col):
    control = data.query(f"~{treat_col}")
    states = control[state_col].unique()
    placebo_state = np.random.choice(states)
    return control.assign(**{treat_col: control[state_col] == placebo_state})
```

```python
np.random.seed(1)
placebo_data = make_random_placebo(data, state_col="state", treat_col="treated")

placebo_data.query("treated").tail()
```

在上面的例子中，我们随机抽取了州 39，并假装它被处理了。注意 `treated` 列被翻转为 `True`。

下一步是用这个安慰剂数据计算 SDID 估计，并重复多次。下面的函数就是这么做的：它调用 `synthetic_diff_in_diff` 函数来获得 SDID 估计，但不是传入真实的数据，而是传入 `make_random_placebo` 的结果。重复这个过程多次，得到一个 SDID 估计的数组，最后计算该数组方差的平方根，也就是标准差。


```python
from joblib import Parallel, delayed # for parallel processing


def estimate_se(data, outcome_col, year_col, state_col, treat_col, post_col, bootstrap_rounds=400, seed=0, njobs=4):
    np.random.seed(seed=seed)
    
    sdid_fn = partial(synthetic_diff_in_diff,
                      outcome_col=outcome_col,
                      year_col=year_col,
                      state_col=state_col,
                      treat_col=treat_col,
                      post_col=post_col)
    
    effects = Parallel(n_jobs=njobs)(delayed(sdid_fn)(make_random_placebo(data, state_col=state_col, treat_col=treat_col))
                                     for _ in range(bootstrap_rounds))
    
    return np.std(effects, axis=0)

```

```python
effect = synthetic_diff_in_diff(data,
                                outcome_col="cigsale",
                                year_col="year",
                                state_col="state",
                                treat_col="treated",
                                post_col="after_treatment")


se = estimate_se(data,
                 outcome_col="cigsale",
                 year_col="year",
                 state_col="state",
                 treat_col="treated",
                 post_col="after_treatment")
```

然后，我们可以用这个标准差来构建置信区间，就像上面的公式描述的那样。


```python
print(f"Effect: {effect}")
print(f"Standard Error: {se}")
print(f"90% CI: ({effect-1.65*se}, {effect+1.65*se})")
```

注意，在这个例子中 $ATT$ 并不显著，但更有趣的是比较 SDID 估计的标准误与传统合成控制的标准误。


```python
def synthetic_control(data, outcome_col, year_col, state_col, treat_col, post_col):
    
    x_pre_control = (data
                     .query(f"~{treat_col}")
                     .query(f"~{post_col}")
                     .pivot(index=year_col, columns=state_col, values=outcome_col)
                     .values)
    
    y_pre_treat_mean = (data
                        .query(f"~{post_col}")
                        .query(f"{treat_col}")
                        .groupby(year_col)
                        [outcome_col]
                        .mean())
    
    w = cp.Variable(x_pre_control.shape[1])
    objective = cp.Minimize(cp.sum_squares(x_pre_control @ w - y_pre_treat_mean.values))
    constraints = [cp.sum(w) == 1, w >= 0]
    
    problem = cp.Problem(objective, constraints)
    problem.solve(verbose=False)
    
    sc = (data
          .query(f"~{treat_col}")
          .pivot(index=year_col, columns=state_col, values=outcome_col)
          .values) @ w.value
    
    y1 = data.query(f"{treat_col}").query(f"{post_col}")[outcome_col]
    att = np.mean(y1 - sc[-len(y1):])
    
    return att

def estimate_se_sc(data, outcome_col, year_col, state_col, treat_col, post_col, bootstrap_rounds=400, seed=0):
    np.random.seed(seed=seed)
    effects = [synthetic_control(make_random_placebo(data, state_col=state_col, treat_col=treat_col), 
                                 outcome_col=outcome_col,
                                 year_col=year_col,
                                 state_col=state_col,
                                 treat_col=treat_col,
                                 post_col=post_col)
              for _ in range(bootstrap_rounds)]
    
    return np.std(effects, axis=0)


effect_sc = synthetic_control(data,
                              outcome_col="cigsale",
                              year_col="year",
                              state_col="state",
                              treat_col="treated",
                              post_col="after_treatment")


se_sc = estimate_se_sc(data,
                       outcome_col="cigsale",
                       year_col="year",
                       state_col="state",
                       treat_col="treated",
                       post_col="after_treatment")
```

```python
print(f"Effect: {effect_sc}")
print(f"Standard Error: {se_sc}")
print(f"90% CI: ({effect_sc-1.65*se_sc}, {effect_sc+1.65*se_sc})")
```

可以看到，合成控制的误差高于 SDID。再次强调，这是因为 SDID 通过时间和单位固定效应捕捉了结果的大部分变异。至此，我们兑现了先前的承诺。但在结束之前，值得一提的是，我们还可以用同样的程序来估计每个处理后时期效应的方差，从而为其构建置信区间。只需对每个时间段运行一次上述代码即可。请记住，即便采用了并行化，这个过程也可能需要一些时间。


```python
standard_errors = {year: estimate_se(data.query(f"~after_treatment|(year=={year})"), 
                                     outcome_col="cigsale",
                                     year_col="year",
                                     state_col="state",
                                     treat_col="treated",
                                     post_col="after_treatment")
                   for year in range(1989, 2001)}

standard_errors = pd.Series(standard_errors)
```

```python
plt.figure(figsize=(15,6))

plt.plot(effects, color="C0")
plt.fill_between(effects.index, effects-1.65*standard_errors, effects+1.65*standard_errors, alpha=0.2,  color="C0")

plt.ylabel("Effect in Cigarette Sales")
plt.xlabel("Year")
plt.title("Synthetic DiD 90% Conf. Interval")
plt.xticks(rotation=45);
```

## 关键观点

**合成双重差分（SDID）** 从双重差分和合成控制两种方法中汲取灵感，结合了两者的优点。与 SC 一样，当处理前趋势不平行时，SDID 仍然适用于多期面板。然而，与 SC 不同，SDID 会估计单位权重来构建控制单位，使其与处理组具有平行趋势（而不必匹配其水平）。从 DID 方面来看，SDID 利用了时间和单位固定效应，这有助于解释结果的大部分变异，从而降低 SDID 估计量的方差。合成双重差分还引入了一些自己的新思想：第一，在单位权重的优化中加入了额外的 $L2$ 惩罚，使权重更加分散到各控制单位；第二，在构建权重时允许一个截距（因此允许外推）；第三，引入了时间权重，这是 DID 和 SC 中都没有的。正因如此，SDID 并不仅仅是 SC 和 DID 的简单叠加；它是在两者启发下构建出的新方法。我也不会说 SDID 一定优于传统的合成控制或劣于它们。它们各自具有不同的特性，适用与否取决于具体情形。例如，在某些情况下，允许 SDID 的外推可能很危险，此时 SC 可能是一个更好的选择。

## 参考文献

本章基本上是对 Dmitry Arkhangelsky、Susan Athey、David A. Hirshberg、Guido W. Imbens 和 Stefan Wager 于 2019 年发表的《Synthetic Difference in Differences》一文的解读。此外，我要感谢 Masa Asami 在 Python 中实现 SDID（pysynthdid）。他的代码帮助我确保自己的实现没有错误，这非常有帮助。

## 参与贡献

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。

---

# 附录


---

# Debiasing with Orthogonalization

Previously, we saw how to evaluate a causal model. By itself, that's a huge deed. Causal models estimates the elasticity $\frac{\delta y}{\delta t}$, which is an unseen quantity. Hence, since we can't see the ground truth of what our model is estimating, we had to be very creative in how we would go about evaluating them. 

The technique shown on the previous chapter relied heavily on data where the treatment was randomly assigned. The idea was to estimate the elasticity $\frac{\delta y}{\delta t}$ as the coefficient of a single variable linear regression of `y ~ t`. However, this only works if the treatment is randomly assigned. If it isn't, we get into trouble due to omitted variable bias. 

To workaround this, we need to make the data look as if the treatment is randomly assigned. I would say there are two main techniques to do this. One is using propensity score and the other using orthogonalization. We will cover the latter in this chapter.

One final word of caution before we continue. I would argue that probably the safest way out of non random data is to go out and do some sort of experiment to gather random data. I myself don't trust very much on debiasing techniques because you can never know if you've accounted for every confounder. Having said that, orthogonalization is still very much worth learning. It's an incredibly powerful technique that will be the foundation of many causal models to come. 

## Linear Regression Reborn

The idea of orthogonalization is based on a theorem designed by three econometricians in 1933, Ragnar Frisch, Frederick V. Waugh, and Michael C. Lovell. Simply put, it states that you can decompose any multivariable linear regression model into three stages or models. Let's say that your features are in an $X$ matrix. Now, you partition that matrix in such a way that you get one part, $X_1$, with some of the features and another part, $X_2$, with the rest of the features. 

In the first stage, we take the first set of features and estimate the following linear regression model 

$$
y_i = \theta_0 + \pmb{\theta_1 X}_{1i} + e_i
$$

where $\pmb{\theta_1}$ is a vector of parameters. We then take the residuals of that model

$$
y^* = y_i - (\hat{\theta}_0 + \pmb{\hat{\theta}_1 X}_{1i})
$$

On the second stage, we take the first set of features again, but now we run a model where we estimate the second set of features

$$
\pmb{X}_{2i} = \gamma_0 + \pmb{\gamma_1 X}_{1i} + e_i
$$

Here, we are using the first set of features to predict the second set of features. Finally, we also take the residuals for this second stage.

$$
\pmb{X}^*_{2i} = \pmb{X}_{2i} - (\hat{\gamma}_0 + \pmb{\hat{\gamma}_1 X}_{1i})
$$


Lastly, we take the residuals from the first and second stage, and estimate the following model

$$
y_i^* = \beta_0 + \pmb{\beta_2 X}^*_{2i} + e_i
$$

The Frisch–Waugh–Lovell theorem states that the parameter estimate $\pmb{\hat{\beta}_2}$ from estimating this model is equivalent to the one we get by running the full regression, with all the features:

$$
y_i = \beta_0 + \pmb{\beta_1 X}_{1i} + \pmb{\beta_2 X}_{2i} + e_i
$$

![img](./images/appendix/nazare-confusa.jpg)

OK. Let's unpack this a bit further. We know that regression is a very special model. Each of its parameters has the interpretation of a partial derivative: how much would $Y$ increase if I increase one feature **while holding all the others fixed**. This is very nice for causal inference, because it means we can control for variables in the analysis, even if those same variables have not been held fixed during the collection of the data.

We also know that if we omit variables from the regression, we get bias. Specifically, omitted variable bias (or confounding bias). Still, the Frisch–Waugh–Lovell is saying that I can break my regression model into two parts, neither of them containing the full feature set, and still get the same estimate I would get by running the entire regression. Not only that, this theorem also provides some insight into what linear regression is doing. To get the coefficient of one variable $X_k$, regression first uses all the other variables to predict $X_k$ and takes the residuals. This "cleans"  $X_k$ of any influence from those variables. That way, when we try to understand $X_k$'s impact on $Y$, it will be free from omitted variable bias. Second, regression uses all the other variables to predict $Y$ and takes the residuals. This "cleans" $Y$ from any influence from those variables, reducing the variance of $Y$ so that it is easier to see how $X_k$ impacts $Y$. 

I know it can be hard to appreciate how awesome this is. But remember what linear regression is doing. It's estimating the impact of $X_2$ on $y$ while accounting for $X_1$. This is incredibly powerful for causal inference. It says that I can build a model that predicts my treatment $t$ using my features $X$, a model that predicts the outcome $y$ using the same features, take the residuals from both models and run a model that estimates how the residual of $t$ affects the residual of $y$. This last model will tell me how $t$ affects $y$ while controlling for $X$. In other words, the first two models are controlling for the confounding variables. They are generating data which is as good as random. This is debiasing my data. That's what we use in the final model to estimate the elasticity.

There is a (not so complicated) mathematical proof for why that is the case, but I think the intuition behind this theorem is so straightforward we can go directly into it.

## The Intuition Behind Orthogonalization

```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split

import statsmodels.formula.api as smf
import statsmodels.api as sm

from nb21 import cumulative_elast_curve_ci, elast, cumulative_gain_ci
```

Let's take our price data once again. But now, we will only take the sample where prices where **not** randomly assigned. Once again, we separate them into a training and a test set. Since we will use the test set to evaluate our causal model, let's see how we can use orthogonalization to debias it.

```python
prices = pd.read_csv("./data/ice_cream_sales.csv")

train, test = train_test_split(prices, test_size=0.5)
train.shape, test.shape
```

If we show the correlations on the test set, we can see that price is positively correlated with sales, meaning that sales should go up as we increase prices. This is obviously nonsense. People don't buy more if ice cream is expensive. We probably have some sort of bias here. 

```python
test.corr()
```

If we plot our data, we can see why this is happening. Weekends (Saturday and Sunday) have higher price but also higher sales. We can see that this is the case because the weekend cloud of points seems to be to the upper right part of the plot.

Weekend is probably playing an important role in the bias here. On the weekends, there are more ice cream sales because there is more demand. In response to that demand, prices go up. So it is not that the increase in price causes sales to go up. It is just that both sales and prices are high on weekends. 

```python
np.random.seed(123)
sns.scatterplot(data=test.sample(1000), x="price", y="sales", hue="weekday");
```

To debias this dataset we will need two models. The first model, let's call it $M_t(X)$, predicts the treatment (price, in our case) using the confounders. It's the one of the stages we've seen above, on the Frisch–Waugh–Lovell theorem.

```python
m_t = smf.ols("price ~ cost + C(weekday) + temp", data=test).fit()
debiased_test = test.assign(**{"price-Mt(X)":test["price"] - m_t.predict(test)})
```

Once we have this model, we will construct the residuals

$$
\hat{t}_i = t_i - M_t(X_i)
$$

You can think of this residual as a version of the treatment that is unbiased or, better yet, that is impossible to predict from the confounders $X$. Since the confounders were already used to predict $t$, the residual is by definition, unpredictable with $X$. Another way of saying this is that the bias has been explained away by the model $M_t(X_i)$, producing $\hat{t}_i$ which is as good as randomly assigned. Of course this only works if we have in $X$ all the confounders that cause both $T$ and $Y$.

We can also plot this data to see what it looks like.

```python
np.random.seed(123)
sns.scatterplot(data=debiased_test.sample(1000), x="price-Mt(X)", y="sales", hue="weekday")
plt.vlines(0, debiased_test["sales"].min(), debiased_test["sales"].max(), linestyles='--', color="black");
```

We can see that the weekends are no longer to the upper right corner. They got pushed to the center. Moreover, we can no longer differentiate between different price levels (the treatment) using the weekdays. We can say that the residual $price-M_t(X)$, plotted on the x-axis, is a "random" or debiased version of the original treatment.

This alone is sufficient to debias the dataset. This new treatment we've created is as good as randomly assigned. But we can still do one other thing to make the debiased dataset even better. Namely, we can also construct residuals for the outcome.

$$
\hat{y}_i = y_i - M_y(X_i)
$$

This is another stage from the Frisch–Waugh–Lovell theorem. It doesn't make the set less biased, but it makes it easier to estimate the elasticity by reducing the variance in $y$. Once again, you can think about $\hat{y}_i$ as a version of $y_i$ that is unpredictable from $X$ or that had all its variances due to $X$ explained away. Think about it. We've already used $X$ to predict $y$ with $M_y(X_i)$. And $\hat{y}_i$ is the error of this prediction. So, by definition, it's not possible to predict it from $X$. All the information in $X$ to predict $y$ has already been used. If that is the case, the only thing left to explain $\hat{y}_i$ is something we didn't used to construct it (not included in $X$), which is only the treatment (again, assuming no unmeasured confounders). 

```python
m_y = smf.ols("sales ~ cost + C(weekday) + temp", data=test).fit()

debiased_test = test.assign(**{"price-Mt(X)":test["price"] - m_t.predict(test),
                               "sales-My(X)":test["sales"] - m_y.predict(test)})
```

Once we do both transformations, not only does weekdays not predict the price residuals, but it also can't predict the residual of sales $\hat{y}$. The only thing left to predict these residuals is the treatment. Also, notice something interesting. In the plot above, it was hard to know the direction of the price elasticity. It looked like sales decreased as prices went up, but there was such a large variance in sales that it was hard to say that for sure. 

Now, when we plot the two residuals, it becomes much clear that sales indeed causes prices to go down.

```python
np.random.seed(123)
sns.scatterplot(data=debiased_test.sample(1000), x="price-Mt(X)", y="sales-My(X)", hue="weekday")
plt.vlines(0, debiased_test["sales-My(X)"].min(), debiased_test["sales-My(X)"].max(), linestyles='--', color="black");
```

One small disadvantage of this debiased data is that the residuals have been shifted to a different scale. As a result, it's hard to interpret what they mean (what is a price residual of -3?). Still, I think this is a small price to pay for the convenience of building random data from data that was not initially random. 

To summarize, by predicting the treatment, we've constructed $\hat{t}$ which works as an unbiased version of the treatment; by predicting the outcome, we've constructed $\hat{y}$ which is a version of the outcome that can only be further explained if we use the treatment. This data, where we replace $y$ by $\hat{y}$ and $t$ by $\hat{t}$ is the debiased data we wanted. We can use it to evaluate our causal model just like we deed previously using random data.

To see this, let's once again build a causal model for price elasticity using the training data.

```python
m3 = smf.ols(f"sales ~ price*cost + price*C(weekday) + price*temp", data=train).fit()
```

Then, we'll make elasticity predictions on the debiased test set.

```python
def predict_elast(model, price_df, h=0.01):
    return (model.predict(price_df.assign(price=price_df["price"]+h))
            - model.predict(price_df)) / h

debiased_test_pred = debiased_test.assign(**{
    "m3_pred": predict_elast(m3, debiased_test),
})

debiased_test_pred.head()
```

Now, when it comes to plotting the cumulative elasticity, we still order the dataset by the predictive elasticity, but now we use the debiased versions of the treatment and outcome to get this elasticity. This is equivalent to estimating $\beta_1$ in the following regression model

$$
\hat{y}_i = \beta_0 + \beta_1 \hat{t}_i + e_i
$$

where the residuals are like we've described before.

```python
plt.figure(figsize=(10,6))

cumm_elast = cumulative_elast_curve_ci(debiased_test_pred, "m3_pred", "sales-My(X)", "price-Mt(X)", min_periods=50, steps=200)
x = np.array(range(len(cumm_elast)))
plt.plot(x/x.max(), cumm_elast, color="C0")

plt.hlines(elast(debiased_test_pred, "sales-My(X)", "price-Mt(X)"), 0, 1, linestyles="--", color="black", label="Avg. Elast.")
plt.xlabel("% of Top Elast. Customers")
plt.ylabel("Elasticity of Top %")
plt.title("Cumulative Elasticity")
plt.legend();
```

We can do the same thing for the cumulative gain curve, of course.

```python
plt.figure(figsize=(10,6))

cumm_gain = cumulative_gain_ci(debiased_test_pred, "m3_pred", "sales-My(X)", "price-Mt(X)", min_periods=50, steps=200)
x = np.array(range(len(cumm_gain)))
plt.plot(x/x.max(), cumm_gain, color="C1")

plt.plot([0, 1], [0, elast(debiased_test_pred, "sales-My(X)", "price-Mt(X)")], linestyle="--", label="Random Model", color="black")

plt.xlabel("% of Top Elast. Customers")
plt.ylabel("Cumulative Gain")
plt.title("Cumulative Gain on Debiased Sample")
plt.legend();
```

Notice how similar these plots are to the ones in the previous chapter. This is some indication that the debiasing worked wonders here. 

In contrast, let's see what the cumulative gain plot would look like if we used the original, biased data.

```python
plt.figure(figsize=(10,6))

cumm_gain = cumulative_gain_ci(debiased_test_pred, "m3_pred", "sales", "price", min_periods=50, steps=200)
x = np.array(range(len(cumm_gain)))
plt.plot(x/x.max(), cumm_gain, color="C1")

plt.plot([0, 1], [0, elast(debiased_test_pred, "sales", "price")], linestyle="--", label="Random Model", color="black")

plt.xlabel("% of Top Elast. Customers")
plt.title("Cumulative Gains on Biased Sample")
plt.ylabel("Cumulative Gains")
plt.legend();
```

First thing you should notice is that the average elasticity goes up, instead of down. We've seen this before. In the biased data, it looks like sales goes up as price increases. As a result, the final point in the cumulative gain plot is positive. This makes little sense, since we now people don't buy more as we increase ice cream prices. If the average price elasticity is already messed up, any ordering in it also makes little sense. The bottom line being that this data should not be used for model evaluation.

## Orthogonalization with Machine Learning

In a 2016 paper, Victor Chernozhukov *et all* showed that you can also do orthogonalization with machine learning models. This is obviously very recent science and we still have much to discover on what we can and can't do with ML models. Still, it's a very interesting idea to know about.

The nuts and bolts are pretty much the same to what we've already covered. The only difference is that now, we use machine learning models for the debiasing. 

$$
\begin{align}
\hat{y}_i &= y_i - M_y(X_i) \\
\hat{t}_i &= t_i - M_t(X_i)
\end{align}
$$

There is a catch, though. As we know very well, machine learning models are so powerful that they can fit the data perfectly, or rather, overfit. Just by looking at the equations above, we can know what will happen in that case. If $M_y$ somehow overfits, the residuals will all be very close to zero. If that happens, it will be hard to find how $t$ affects it. Similarly, if $M_t$ somehow overfits, its residuals will also be close to zero. Hence, there won't be variation in the treatment residual to see how it can impact the outcome. 

To account for that, we need to do sample splitting. That is, we estimate the model with one part of the dataset and we make predictions in the other part. The simplest way to do this is to split the test sample in half, make two models  in such a way that each one is estimated in one half of the dataset and makes predictions in the other half. 

A slightly more elegant implementation uses K-fold cross validation. The advantage being that we can train all the models on a sample which is bigger than half the test set.

![img](./images/appendix/kfold-cv.png)

Fortunately, this sort of cross prediction is very easy to implement using Sklearn's `cross_val_predict` function.

```python
from sklearn.model_selection import cross_val_predict
from sklearn.ensemble import RandomForestRegressor

X = ["cost", "weekday", "temp"]
t = "price"
y = "sales"

folds = 5

np.random.seed(123)
m_t = RandomForestRegressor(n_estimators=100)
t_res = test[t] - cross_val_predict(m_t, test[X], test[t], cv=folds)

m_y = RandomForestRegressor(n_estimators=100)
y_res = test[y] - cross_val_predict(m_y, test[X], test[y], cv=folds)
```

Now that we have the residuals, let's store them as columns on a new dataset. 

```python
ml_debiased_test = test.assign(**{
    "sales-ML_y(X)": y_res,
    "price-ML_t(X)": t_res,
})
ml_debiased_test.head()
```

Finally, we can plot the debiased dataset. 

```python
np.random.seed(123)
sns.scatterplot(data=ml_debiased_test.sample(1000),
                x="price-ML_t(X)", y="sales-ML_y(X)", hue="weekday");
```

## Contribute

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。


---

# Debiasing with Propensity Score
 
Previously, we saw how to go from a biased dataset to one where the treatment looked as good as randomly assigned. We used orthogonalization for that. That technique was based on predicting the treatment and the outcome and then replacing both with their predictions' residuals. 
 
$
t^* = t - M_t(X)
$
 
$
y^* = y - M_y(X)
$
 
That alone is a powerful technique. It works both when the treatment is continuous or binary. Now, we'll take a look at another method which is based on the propensity score. Since the propensity score is best defined when the treatment is binary or categorical, this debiasing technique will also work only for categorical  or binary treatments. Still, in some situations, it can be more reliable than orthogonalization and it is very much worth learning. 
 
But first, we need to change context a little bit. We were talking about ice cream prices, which is a continuous treatment. Now, we will look into marketing emails, a binary treatment. And just as a side note, it is totally fair play to discretize a continuous treatment into buckets so it looks categorial (for example, you can take price, which is continuous, and discretize it in bins of R$ 2.00 like [2.00, 4.00, 6.00, 8.00]). Back to the emails now.
 
The situation goes like this. You work at a financial company (I'm not very creative with my examples, sorry) that supplies, not surprisingly, financial products such as life insurance, savings account, investment account and so on. The company is coming up with a new financial consulting service and wants to market it to its customers. To do so, the marketing team tried out three different emails: `em1`, `em2` and `em3`. 
 
Because the marketing team is very well educated in statistics and experimental design, they don't simply send the email to everyone and see what happend. Instead, they design an experiment where each customer has a probability of receiving the email. This probability is based on their business intuition to who will be more responsive to the email. For example, `em1` is targeted to a mass market audience, that don't invest much and that don't have a high income. These people will have a higher probability of receiving `em1`. Finally, after having defined that probability, we randomize the assignment so that customers receive the email according to their assigned probabilities.
 
![img](./images/appendix/ps-experiment.png)
 
As a side note, this is the best way I know of that uses business expertise to target an audience while still allowing you to make valid inference about how effective the marketing strategy is. The marketing team not only assigned the email according to this probability function, but they actually stored each customers' probability, which will be very handy later on. 
 
Here is what the data from this experiment looks like. We have data on four customer attributes: `age`, `income`, how much they have on life `insurance` and how much they have `invested`. These are the attributes which the marketing team used to assign the probability of receiving each email. Those probabilities are stored in columns `em1_ps`, `em2_ps` and `em3_ps`. Then, we have information on whether the customer actually received the email on columns `em1`, `em2` and `em3`. These are our treatment variables. Finally, we have the outcome, `converted`, flags if the customer contracted the financial advisory service.

```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import seaborn as sns
```

```python
email = pd.read_csv("./data/invest_email.csv")
email.head()
```

As we can see, there are customers with very low probability of receiving `em1`. This is the case of customer 2, which has a probability of only 0.062. Also notice that this customer did not receive `em1`. This is not surprising, given that he had a very low probability of receiving this email. 
 
Now, contrast this with the following customers. All of them had a very high chance of getting `em1`, but none of them did.

```python
email.query("em1 == 0").query("em1_ps>0.9")
```

## Inverse Probability of Treatment Weighting
 
![img](./images/appendix/again.png)
 
Just to recap the idea behind propensity score debiasing (we've already seen it when we discussed propensity score), we are very interested in customers like the ones above. They look a lot like the ones that get `em1`, after all they had a high probability of getting that email. However, they didn't get it. This makes them awesome candidates to estimate the counterfactual outcome $Y_0|em1=1$. 
 
Propensity score debiasing works by encoding this intuitive importance we want to give to customers that look like the treated, but didn't get the treatment. It will also assign a high importance for those that look like the untreated, but got the treatment. The idea is pretty simple: just **weight every unit by the inverse probability of the treatment they got**. So, if a unit got the treatment, weigh it by $1/P(treatment=1)$. If a unit did not get the treatment, weigh it by $1/P(treatment=0)$ or $1/P(control)$. In the case of binary treatment, this is simply
 
$$
W_i = \dfrac{T}{P(T=1|X)} + \dfrac{1-T}{1-P(T=1|X)}
$$
 
For multiple treatments, we can generalize it to
 
$$
W_i = \sum^K_{k=0} \dfrac{\mathcal{1}_{K=k} }{P(T=k|X)}
$$
 
Now that we had an idea about how to use the propensity score to do debiasing, let's check how this works in practice. But first, let's see what happens when we don't debias our dataset.
 
Like I've said before, the treatment is confounded by the customer's characteristics: age, income, how much they have on insurance and how muc they have on hinvestment. 

```python
confounders = ["age", "income", "insurance", "invested"]
```

If we look at their correlation structure, we can see that these variables are indeed confounders. They are correlated both with the treatment `em1` and the outcome `converted`. 


```python
email[confounders + ["em1", "converted"]].corr()[["em1", "converted"]]
```

If we fail to account for this confounding bias, our causal estimates will be wrong. To give an example, consider the `invested` variable. Those that invest **less** are **more** likely to convert **and** also **more** likely to receive the email. Hence, if we don't control for investments, it will look like `em1` increases the chance of conversion. But that could be just due to correlation, not causation, as lower levels of investments leads to both higher conversion and a higher chance of getting `em1`. 
 
Besides looking at the correlation with the confounders, we can also check how the distribution looks like for those that did and didn't get the email `em1`.

```python
plt_df = pd.melt(email[confounders + ["em1"]], ["em1"], confounders)

g = sns.FacetGrid(plt_df, col="variable", hue="em1", col_wrap=4, sharey=False, sharex=False)

for i, ax in enumerate(g.axes):
    iter_df = plt_df.loc[lambda df: df["variable"] == confounders[i]]
    sns.kdeplot(x="value", hue="em1", data=iter_df, ax=ax, fill=True)
    ax.set_xlabel(confounders[i])

plt.show() 
```

 
### Stored Propensity Score
 
Now that we've confirmed that the assignment of `em1` is indeed biased, we can work on debiasing it with the propensity score. Now it's a good time to answer a question that might be in your head: why can't I just use orthogonalization? Or, more elegantly, when should I use the propensity score instead of orthogonalization. That's a good question and one I confess I don't have all the answers for. However, there is one clear case when there is a strong argument for the propensity score.
 
When you've stored the probabilities of receiving the treatment while conducting your experiment, **propensity score debiasing can be done without having to estimate a model**. If the assignment of the emails was done in a probabilistic way and we've stored those probabilities, then we don't have to rely on models. That's a huge advantage, because models are never perfect, which means that debiasing with them is also never perfect. Here, we have this situation where we've stored the probabilities of the treatments in the columns `em1_ps`, `em2_ps` and `em3_ps`. 
 
Since we are dealing only with `em1`, we only need the probability in `em1_ps` to do the debiasing. Here is what we'll do. First, we will generate the debiasing weights by using the formula we've seen above. Then, we will resample with replacement from this dataset, using the newly created weights. This means a unit with weight 2 will be resampled twice as often as a unit with weight 1. 

```python
np.random.seed(123)
em1_rnd = email.assign(
    em1_w = email["em1"]/email["em1_ps"] + (1-email["em1"])/(1-email["em1_ps"])
).sample(10000, replace=True, weights="em1_w")
```

```python
np.random.seed(5)
em1_rnd.sample(5)
```

This resampling should make a new dataset that is debiased. It should have oversampled units that looked like the treated (high `em1_ps`) but did not get the treatment and those that looked like the control (low `em1_ps`), but got the treatment. 
 
If we look at correlations between the treatment and the confounders, we can see that they essentially vanished.

```python
em1_rnd[confounders + ["em1", "converted"]].corr()[["em1", "converted"]]
```

Moreover, if we look at the confounders distributions by treatment assignment, we can see how nicely they align. This is not 100% proof that the debiasing worked, but it's good evidence of it. 

```python
plt_df = pd.melt(em1_rnd[confounders + ["em1"]], ["em1"], confounders)

g = sns.FacetGrid(plt_df, col="variable", hue="em1", col_wrap=4, sharey=False, sharex=False)

for i, ax in enumerate(g.axes):
    iter_df = plt_df.loc[lambda df: df["variable"] == confounders[i]]
    sns.kdeplot(x="value", hue="em1", data=iter_df, ax=ax, fill=True)
    ax.set_xlabel(confounders[i])

plt.show() 
```

This new dataset we've created is now debiased. We can use it to do model evaluation or any other analys that requires the treatment to be randomly assigned. There is only one thing you need to watch out for. Notice how I've sampled 10000 points but the original dataset had only 5000? With this resampling method, I can make a debiased dataset as big as I wish. This means that confidence intervals that are computed in it are not valid, as they don't take into account the fact that the sample size can be artificially inflated. 
 
Ok, so this technique was super effective because we had the stored probabilities in the first place, but what if we don't? What if we only have access to the confounders and the treatment that got assigned, but not to how likely those units were to get the treatment?
 
### Estimated Propensity Score
 
If we don't have the propensity score stored, we will have to estimate them. In this situation, it becomes less clear when you should use propensity score or orthogonalisation for debiasing.
 
Since we don't have the propensity score, we will use a machine learning model to estimate it. The propensity score is closely related to the probability of treatment, so this ML model must be calibrated to output a probability. Not only that, we need to do cross prediction to work around any sort of bias we might have due to overfitting. 

```python
from sklearn.model_selection import cross_val_predict
from sklearn.ensemble import RandomForestClassifier
from sklearn.calibration import CalibratedClassifierCV

t = "em1"

folds = 5

np.random.seed(123)

# makes calibrated Random Forest. 
m_t = CalibratedClassifierCV(
    RandomForestClassifier(n_estimators=100, min_samples_leaf=40, max_depth=3),
    cv=3
)

# estimate PS with cross prediction. 
ps_score_m1 = cross_val_predict(m_t, email[confounders], email[t],
                                cv=folds, method="predict_proba")[:, 1]
```

```python
email.assign(ps_score_m1_est = ps_score_m1).head()
```

Just out of curiosity, notice that the estimated propensity score, `ps_score_m1_est`, is close to the true one `em1_ps`, but not identical. Those errors in the estimation process will affect the final debiasing, but we hope it won't affect much. We can also check how well calibrated our score is. For that, we can plot the mean score against the mean `em1`. If the score is well calibrated, 20% of those with a score of 0.2 should have received email-1, 30% of those with a score of 0.3 should have received email-1 and so on.

```python
from sklearn.calibration import calibration_curve

prob_true, prob_pred = calibration_curve(email["em1"], ps_score_m1, n_bins=3)
plt.plot(prob_pred, prob_true, label="Calibrated RF")
plt.plot([.1,.8], [.1, .8], color="grey", linestyle="dashed", label="Perfectly Calibrated")
plt.ylabel("Fraction of Positives")
plt.xlabel("Average Prediction")
plt.legend();
```

From here, we can proceed as if we had the true propensity score.


```python
np.random.seed(123)
em1_rnd_est = email.assign(
    em1_w = email["em1"]/ps_score_m1 + (1-email["em1"])/(1-ps_score_m1)
).sample(10000, replace=True, weights="em1_w")
```

If we check the correlation structure, we will see that there are still some strong correlations between the treatment and the confounders, even after the debiasing process. For instance, income has a correlation of `-0.18` this is lower than the correlation in the unbiased dataset (`-0.3`), but much higher than what we got in the dataset that was debiased with the original propensity score (`0.01`). The same is true for the `invested` variable, which still shows some correlation. 

```python
em1_rnd_est[confounders + ["em1"]].corr()["em1"]
```

```python
plt_df = pd.melt(em1_rnd_est[confounders + ["em1"]], ["em1"], confounders)

g = sns.FacetGrid(plt_df, col="variable", hue="em1", col_wrap=4, sharey=False, sharex=False)

for i, ax in enumerate(g.axes):
    iter_df = plt_df.loc[lambda df: df["variable"] == confounders[i]]
    sns.kdeplot(x="value", hue="em1", data=iter_df, ax=ax, fill=True)
    ax.set_xlabel(confounders[i])

plt.show() 
```

As for the distributions, we can see that they don't align that well as before, specially for the variables `invested` and `income`
 
## The Weakness of Propensity Score
 
We've already talked a lot about this on the propensity score chapter, so I won't spend much time on it, but it is recalling one of the main weaknesses with the propensity score debiasing. It has to do with propensities scores that are two high or too low. 

![img](./images/appendix/fear-no-man.png)
 
To understand this, take a look at the following unit in our original dataset.

```python
email.loc[[1014]]
```

This unit has a propensity score of 0.027 for `em1`. This means it's weight will be around 37 (1/0.027). This unit will be sampled almost twice as much as a treated unit with a propensity score of 0.05 (weight of 20), which is already a very low propensity score. This unit appears 38 times in the dataset we've resampled using the stored (not estimated) propensity score.

```python
em1_rnd.loc[[1014]].shape
```

That's a problem, because the debiased dataset is overpopulated with only one unit. If this unit wasn't in the original dataset, the debiased dataset could look totally different. So, removing a single unit can affect a lot how a debiased dataset looks like. This is a problem of high variance.
 
If we plot the number of replications for each unit in the debiased dataset, we see that a bunch of them appear more than 10 times. Those are treated units with low propensity score or untreated units with high propensity score. 

```python
plt.figure(figsize=(10,5))
sns.scatterplot(
    data=em1_rnd.assign(count=1).groupby(em1_rnd.index).agg({"count":"count", "em1_ps": "mean", "em1": "mean"}),
    x="em1_ps",
    y="count",
    hue="em1",
    alpha=0.2
)
plt.title("Replications on Debiased Data");
```

To avoid having overly important units, some scientists like to clip the weights, forcing them to be, let's say, at most 20. It is true that this technique will remove some of the variance, but it will add bias back. Since the whole point of this was to remove bias, I feel like clipping the weights is not a good idea.
 
In my opinion, the best thing you can do is at the experimentation phase, not at the analysis phase. You should make sure none of the units have a weight that is too high. In other words, as much as possible, try to give the treatment with equal probabilities to everyone. 
 
To be fair to the propensity weighting method, all causal inference methods will suffer whenever the probability of receiving either the treatment or the control is too low, for the entire sample or for a subpopulation. Intuitively, this means some units will almost never receive the treatment or the control, which will make it difficult to estimate counterfactuals for those units. This problem can be seen as almost violating the common support assumption, which we will explore next. 

## Positivity or Common Support
 
Besides high variance, we can also have problems with positivity. Positivity, or common support is a causal inference assumption which states that there must be sufficient overlap between the characteristics of the treated and the control units. Or, in other words, that everyone has a non zero probability of getting the treatment or the control. If this doesn't happen, we won't be able to estimate a causal effect that is valid for the entire population, only for those we have common support.
 
To be clear, positivity issues are problems of the data itself, not a problem with the propensity score method. Propensity score method just makes it very clear when positivity problems exist. In that sense, this is an argument in favor of the propensity score. While most methods will not warn you when there are positivity problems, propensity score will show it to your face. All you have to do is plot the distribution of the propensity score for the treated and for the untreated.

```python
sns.displot(data=email, x="em1_ps", hue="em1")
plt.title("Positivity Check");
```

If they have a nice overlap, like in the plot above, then you have common support. 
 
Up until now, we've only looked at email-1 (`em1`). Let's look at email-3 now. 

```python
email.head()
```

First thing that jumps the eye is that there are units with zero probability, which already indicates a violation to the positivity assumption. Now, let's look at the features distributions by `em3`

```python
sns.pairplot(email.sample(1000)[confounders + ["em3"]], hue="em3", plot_kws=dict(alpha=0.3));
```

It looks like `em3` was only sent to customers that are older than 40 years. Thats a huge problem. If the control has younger folks, but the treatment doesn't, there is no way we can estimate the counterfactual $Y_0|T=1, age<40$. Simply because we have no idea how younger customers will respond to this email. 
 
Just so you know what will happen, we can try to make the propensity score debiasing.

```python
em3_weight = (email
              # using a different implementation to avoid division by zero
              .assign(em3_w = np.where(email["em3"].astype(bool), 1/email["em3_ps"], 1/(1-email["em3_ps"])))
              .sample(10000, replace=True, weights="em3_w"))
```

```python
em3_weight[confounders + ["em3"]].corr()["em3"]
```

As we can see, there is still a huge correlation between age and the treatment. Not having treated samples that are younger than 40 years made it so that we couldn't remove the bias due to age.
 
We can also run the positivity diagnostic, plotting the propensity score distribution by treatment. 

```python
sns.displot(data=email, x="em3_ps", hue="em3")
plt.title("Positivity Check");
```

Notice how poor the overlap is here. Units with propensity score below 0.4 almost never get the treatment. Not to mention that huge peak at zero. 
 
### Positivity In Practice
 
Issues in positivity are very serious in the academic world because they treat the generalization of a conclusion or theory. If you only have treated individuals that are older, you have no way of generalizing whatever treatment effect that you find to a younger population. But in the industry, violations of the positivity assumption might not be so problematic. 
 
To give an example, if you are a lender wishing to estimate the elasticity of loan amount on probability of default, you will probably not give high loans to people with very low credit scores. Sure, this will violate the positivity assumption, but you are not very interested in estimating the loan amount elasticity for risky customers because you are not intending to give them loans anyway. Or, for a somewhat more exaggerated example, if you want to estimate the price elasticity of some product, you will probably not test prices from 0 to 1000000. That's because you have some intuition on where the prices normally are. You will often test around that region. 
 
The point here is that you can (and probably should) use your intuition to exclude some part of the population from receiving the treatment (or the control). Doing random experiments is expensive, so you should focus your efforts on where things are more promising. 
 
In our email example, probably the marketing team though that email-3 should never be sent to younger people. Maybe it contains text explicitly discussing something that only older people can relate to. Regardless of the reason, we can only estimate the effect of email-3 on the older, age>40, population. And that is totally fine. We just shouldn't expect to generalize whatever findings we have to the younger population.
 
With that in mind, let's debias email-3. Of course, we will remove the younger population from the sample. 

```python
em3_weight.query("age>40")[confounders + ["em3"]].corr()["em3"]
```

Once we do that, notice how the correlation between the treatment and the confounders goes away. Remember that in the entire sample, the correlation with age was more than 0.2. Now, it is probably indistinguishable from zero.
 
We can also explore the distribution of treated and non treated in this filtered sample.

```python
sns.pairplot(em3_weight.query("age>40").sample(1000)[confounders + ["em3"]], hue="em3", plot_kws=dict(alpha=0.3))
```

## Contribute

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。


---

# When Prediction Fails
 
## When all you have is a Hammer...
Between 2015 and 2020, Machine Learning went through a massive surge. Its proven usefulness in the fields of computer vision and natural language understanding, coupled with an initial lack of professionals in the area, provided the perfect opportunity for a machine learning teaching industry. Figures like Andrew Ng and Sebastian Thrun managed to teach machine learning to the world at rock bottom prices. At the same time, on the software side, it became increasingly easier to fit a complex machine learning model (as you've already seen by the very few lines of code it took us to write an ML in the previous chapter). Tutorials about how to make intelligent systems sprung all over the internet. The cost of entry in ML plummeted.

![img](./images/appendix/ml-in-5.png)
 
Building ML became so simple that you didn't even need to know how to code very well (and I'm living evidence of that), nor the math behind the algorithms. In fact, you could build wonders with the following 5 lines of Python.
 
```python
X_train, y_train, X_test, y_test =  train_test_split(X, y)
 
## instantiate the machine learning model
model = MachineLearningModel()
 
## Fit the ML model
model.fit(X_train, y_train)
 
# Make predictions on unseen data
y_pred = model.predict(X_test)
 
# Evaluate the quality of predictions
print("Performance", metric(y_test, y_pred))
```
 
For the most part, this is an amazing thing! I'm all in for taking valuable content and making it available. However, there is also a dark side to all of this. This new wave of data scientists were trained mostly in predictive modeling, since that is what ML primarily focuses on solving. As a result, whenever those data scientists encountered a business problem, they tried to tackle it with, not surprisingly, predictive models. When they were indeed prediction problems, like the one we saw in the previous chapter, the data scientist usually succeeded and everyone got happy. However, there is an entire class of problems that are simply not solvable with prediction techniques. And when those appeared, the data scientists usually failed miserably. These are problems that are framed like "how much can I increase Y by changing X".
 
From my experience, this other type of problem is what management usually cares the most about. They often want to know how to increase sales, decrease cost or bring in more customers. Needless to say, they are not very happy when a data scientist comes up with an answer to how to predict sales instead of how to increase it. Sadly, when everything the data scientist knows is predictive models, this tends to happen a lot. As a boss of mine once told me: "when all you have is a hammer, everything starts to look like a thumb". 
 
Like I've said, I'm all in for lowering the cost of knowledge, but the current Data Scientist curriculum has a huge gap. I think that my job here is to fill in that gap. Is to equip you with tools to solve this other class of problems, which are causal in nature. 

What you are trying to do is estimate how something you can control (advertisement, price, customer service) affects or causes something you want to change, but can't control directly (sales, number of customers, PNL). But ,before showing you how to solve these problems, I want to show you what happens when you treat them like prediction tasks and try to solve them with the traditional ML toolkit. The reason for it is that data scientists often come to me and say "OK, but although tackling causal problems with prediction tools is not the best idea, it surely helps something, no? Imean, it couldn't hurt...". Well, as it turns out, it can. And you better understand this before you go on hammering your own thumb.

![img](./images/appendix/horse-meme.png)


```python
import pandas as pd
import numpy as np
from sklearn import ensemble
from sklearn.model_selection import train_test_split, cross_val_predict
from sklearn.ensemble import GradientBoostingRegressor 
from sklearn.metrics import r2_score
import seaborn as sns
from matplotlib import pyplot as plt
from matplotlib import style
style.use("ggplot")

# helper functions for this notebook
from nb18 import ltv_with_coupons
```

## Who Wants a Coupon?
 
To make matters more relatable, let's continue with the example we used in the previous chapter, but with a little twist to it. Before, we were trying to distinguish the profitable from the non profitable customers. We framed that as a prediction problem: predicting customer profitability. We could then build a machine learning model for this task and use it to choose who we would do business with: only the customers we predicted to be profitable. In other words, our goal was to separate the profitable from the non-profitable, which we could do with a predictive model.
 
Now, you have a new task. You suspect that giving coupons to new customers increases their engagement with your business and makes them more profitable in the long run. That is, they spend more and for a longer period. Your new assignment is to figure out how much the coupon value should be (zero included). Notice that, with coupons, you are essentially giving away money for people to spend on your business. For this reason, they enter as a cost in your book account. Notice that if the coupon value is too high, you will probably lose money, since customers will buy all they need using only the coupons. That's another way of saying that they will get your product for free. On the flip side, if coupon value is too low (or zero), you are not even giving coupons. This could be a valid answer, but it could also be that some discounts upfront, in the form of coupons, will be more profitable in the long run. 
 
For reasons you will see later, we will use a data generating function instead of loading a static dataset. The function `ltv_with_coupons` generates transaction data for us. As you can see, they have the same format as the one we saw previously, with one row per customer, a column for the cost of acquisition and columns for the transactions between day 1 and 30. 

```python
transactions, customer_features = ltv_with_coupons()

print(transactions.shape)
transactions.head()
```

As for the other parts of the data, again, we have a customer identifier, the region the customer lives, the customer income and the customer age. In addition, we now have a variable that is `coupons`, which tells us how much we've given in coupons for that customer.

```python
print(customer_features.shape)
customer_features.head()
```

To process this data to a single dataframe, we will sum all the columns in the first table (that is, summing `CACQ` with the transactions).This will give us the `net_value` as it was computed in the previous chapter. After that, we will join in the features data and update the `net_value` to include the coupon cost.

```python
def process_data(transactions, customer_data):

    profitable = (transactions[["customer_id"]]
                  .assign(net_value = transactions
                          .drop(columns="customer_id")
                          .sum(axis=1)))

    return (customer_data
            # join net_value and features
            .merge(profitable, on="customer_id")
            # include the coupons cost
            .assign(net_value = lambda d: d["net_value"] - d["coupons"]))

customer_features = process_data(transactions, customer_features)
customer_features.head()
```

This processed data frame has all that we need. It has our target variable `net_value`, it has our customer features `region`, `income` and `age`, and it has the lever or treatment we want to optimise for: coupons. Just to begin understanding how coupons can increase `net_value`, let's look at how they were given away.

```python
customer_features.groupby("coupons")["customer_id"].count()
```

We can see that most of the coupons that were handed out had a value of 5 BRL, followed by the coupons with 10 BRL in value. We gave very few 15 BRL coupons or no coupons at all (zero value). This is indicative that they were **NOT** given randomly. To check that, let's see the correlation between the other variable and coupons.

```python
customer_features.corr()[["coupons"]]
```

That's interesting. It looks like the older the person, the higher the probability he or she will receive a coupon. This is some indication of bias in our data. We can also see a negative correlation between coupons and `net_value`: the more coupons we give, the smaller the net_value. This is hardly causal, since we already know that coupons were not randomly distributed. It could be that, say, older people spend less on our products and also receive higher coupon values, confounding the relationship between coupons and `net_value` to the point of making it negative.

The point being, we know there is bias. However, since there is already too much packed into this chapter, I'll ignore it for now (actually, I'll bypass the problem with an artifact you will see in just a moment). Just keep in mind it's something we will have to address sometime in the future.

At this point in the analysis, **if this was a prediction problem**, we would probably split the dataset into a training and a test set to build and evaluate some policies, respectively. But this is NOT a prediction problem. The final goal here is not to get a good prediction on customer profitability. Instead, it's to figure out the optimal coupon strategy. To evaluate this optimization, we would have to know how things would have played out if we have given different coupons than the ones that were given. This is the sort of counterfactual "what if'' question we've been studying under causality. Cross validation won't help us here because we simply can't observe counterfactuals. We can only see what happened for the coupons that were actually given, but we can't know what would have happened if customers received a different coupon value. Unless, we have simulated data!

If our data is simulated, we can generate the exact same data, only changing the coupon value parameters. This will allow us to see how `net_value` changes under different coupons strategies. We will then be able to calculate the treatment effect between different strategies $NetValue_{t=a} - NetValue_{t=b}$. With the power of simulated data, understanding this chapter will be much easier. Oh yes, and this will also render the bias problem irrelevant, because we will observe the causal effect directly. 

Nevertheless, always remember that this is a pedagogical artifact. In the real world, you don't have simulated data and you certainly can't see what would have happened under different treatment strategies. Individual causal effects remain hidden as they always have been. This poses an interesting problem. How can we evaluate our strategies for identifying causal effect if we can never see the actual causal effect? The real answer is very involved and so important that it deserves its own chapter. Rest assured that we will tackle it. For now, just enjoy the simplicity of simulated data. And speaking of simplicity...


## Simple Policy

As always, the first thing we should do whenever we encounter a new data problem is to ask ourselves "what is the simplest thing I can do that will already bring value?". For this specific case, the simplest thing is to look back on the data that we have and estimate the `net_value` for each coupon value. Then, check which coupon value is generating the highest `net_value` and give only that coupon value for every customer. 

```python
sns.barplot(data=customer_features, x="coupons", y="net_value")
plt.title("Net Value by Coupon Value");
```

Doing that analysis, we can see that, on average, we lose money when the coupon value is 0 or 15 and we gain money for coupons of 5 and 10 BRL. The highest average `net_income` appears when we have 5 BRL coupons, yielding us about 250 BRL in `net_value` per customer. Naturally then, the simplest thing we can try is to give everyone 5 BRL in coupons and see how that would play out. This completely disregards the possibility of bias but hey, we are talking simplicity here!

To do evaluate that policy, the function `ltv_with_coupons` accepts as argument a 10000 array that contains the desired coupon for each of the 10000 customers on our database. To create this array, we will generate an array of ones with `np.ones` the size of our `coupons` array (10000) and multiply it by 5. Then, we will pass this array to the `ltv_with_coupons`. This will generate a new dataset exactly like the one we had previously, but with every coupon value set to 5. We then process this data to get the net value under this newly proposed policy.

```python
simple_policy = 5 * np.ones(customer_features["coupons"].shape)

transactions_simple_policy, customer_features_simple_policy = ltv_with_coupons(simple_policy)
customer_features_simple_policy = process_data(transactions_simple_policy, customer_features_simple_policy)

customer_features_simple_policy.head()
```

Just as a sanity check, let's see if the features are indeed unchanged, considering the first few customers. Take the third one, for example (`customer_id` 2). For this customer, the region is 35, income is 2034 and the age is 33. If we scroll up a bit, we can see that it matches what we had before, so we are good here. Also, we can check that all the coupons are indeed 5 BRL. Finally, the `net_value` changes as expected. One reason for this is that the cost associated with coupons will change. For example, that customer had 15 BRL in coupons, but now it's 5. This would decrease the cost from 15 to 5 units. But notice that the `net_value` goes from -23 to 63, a 86 BRL increase in `net_value`. This is much larger than the 10 cost difference. Here giving less in the coupons made this particular customer much more profitable than he or she was before. Finally, to evaluate the policy, we can simply take the average `net_value`.

```python
simple_policy_gain = customer_features_simple_policy["net_value"].mean()
simple_policy_gain
```

As we can see, this simple policy is telling us we can get, on average, 253 BRL for each customer if we give them all a 5 BRL coupon. This is massive! But can we do better? What if we use our shiny machine learning hammer on this problem? Let's try this next.


## Policy With Model

To use ML, we will adapt what we did in the previous chapter. The idea is to build a ML model that predicts `net_value`, just like before, take those predictions and bins them into a defined number of bands. Then, we will partition the data into those bands. Essentially, we are splitting the customer by their predicted `net_value`. Customers that we think will generate roughly the same `net_value` will end up in the same bin or group. Finally, for each group, we will see which coupon value yields the maximum `net_value`. We are doing the same thing as in the simple policy, but now within the groups defined by a prediction band.

![img](./images/appendix/partitions.png)

The intuition behind this is the following: we know that, on average, 5 BRL coupons performed better. However, it is possible that for some group of customers, another value is even better than 5 BRL. Maybe 5 BRL is the optimal strategy for most of the customers, but not for all of them. 

![img](./images/appendix/personalise.png)

If we can identify the ones where the optimal value is different, we can build a coupon strategy better than the simple one we did above.

This is what we call a personalisation problem. We can leverage personalisation when we have more than one strategy to choose from and at least one of them is not the overall best strategy, but it is the best in a subset of the targeted population. This definition is a bit convoluted, but the intuition is simple. If you have only one strategy, you are not personalising. You are doing the same thing for every customer. If you have more than one strategy, but one of them is better for every single customer, why will you personalise? You could just do that one best thing. You will only do personalisation if you have one strategy that works better at one subset of the population and another strategy that works best in another subset of the population.

But back to the example. The first thing we need is a function that fits our predictive model and also splits the predictions into the prediction bands. This function will return another function, a prediction function that will take a dataframe and add both predictions and band columns.


```python
def model_bands(train_set, features, target, model_params, n_bands, seed=1):
    
    np.random.seed(seed)
    
    # train the ML model
    reg = ensemble.GradientBoostingRegressor(**model_params)
    reg.fit(train_set[features], train_set[target])
    
    # fit the bands
    bands = pd.qcut(reg.predict(train_set[features]), q=n_bands, retbins=True)[1]
    
    def predict(test_set):
        # make predictions with trained model
        predictions = reg.predict(test_set[features])
        
        # discretize predictions into bands.
        pred_bands = np.digitize(predictions, bands, right=False) 
        return test_set.assign(predictions=predictions,
                               # cliping avoid creating new upper bands
                               pred_bands=np.clip(pred_bands, 1, n_bands))
    
    return predict
```

To evaluate the quality of our predictions, we will split the dataset into a training and a testing set. Notice here that we are evaluating the quality of the prediction, NOT of the policy. This is just to see if our model is any good at doing what is supposed to do. 

```python
train, test = train_test_split(customer_features, test_size=0.3, random_state=1)
```

Now, let's train our model and make 10 bands with its predictions.

```python
model_params = {'n_estimators': 150,
                'max_depth': 4,
                'min_samples_split': 10,
                'learning_rate': 0.01,
                'loss': 'squared_error'}

features = ["region", "income", "age"]
target = "net_value"

np.random.seed(1)
model = model_bands(train, features, target, model_params, n_bands=10)
```

After training our model, we can use it to make predictions, passing it a dataframe. The result will also be a dataframe with 2 new columns: `predictions` and `pred_bands`.

```python
model(train).head()
```

To see the predictive power of our model, we can look at the $R^2$ for both training and test sets.

```python
print("Train Score:, ", r2_score(train["net_value"], model(train)["predictions"]))
print("Test Score:, ", r2_score(test["net_value"], model(test)["predictions"]))
```

Remember that this performance is only the predictive performance. What we really want to know is if this model can make us some money. Let's make a policy! The idea here is very similar to what we saw in the previous chapter. We will group the customers by model band. Then, for each type of customer (where type is defined by the bands) we'll see which decision - coupon value in our case - is the best one. To do so, we can group our data by prediction band and coupon value and plot the `net_value`.

```python
plt.figure(figsize=(12,6))
sns.barplot(data=model(customer_features), x="pred_bands", y="net_value", hue="coupons")
plt.title("Net Value by Coupon Value");
```

This plot is very interesting. Notice how the optimal decision changes across prediction bands. For instance, on bands like 1, 7 and 8, the best thing to do is to give 10 BRL in coupons. For bands like 3, 5 and 10, the best thing is 5 BRL in coupons. This means that this policy is very much like the simple one, except for the last band. This is evidence that personalisation might be possible, since the optimal decision changes across subpopulations.

We can code that policy with a couple of `if ... then ...` statements, but I'll show a more general approach that leverages dataframe operations.

![img](./images/appendix/pandas-magic.png)

First, we will group our customers by band and coupon value and take the average `net_value` for each group, much like the plot above.

```python
pred_bands = (model(customer_features)
              .groupby(["pred_bands", "coupons"])
              [["net_value"]].mean()
              .reset_index())

pred_bands.head(7)
```

Then, we will group by band and take the `net_value` rank for each row. This will order the rows according to the average `net_value`, where 1 is the best `net_value` in that band.

```python
pred_bands["max_net"] = (pred_bands
                         .groupby(['pred_bands'])
                         [["net_value"]]
                         .rank(ascending=False))


pred_bands.head(7)
```

For example, for band one, the best coupon strategy is 10 BRL. Next, we will keep only the greatest `net_value` per band.

```python
best_coupons_per_band = pred_bands.query("max_net==1")[["pred_bands", "coupons"]]

best_coupons_per_band
```

To build our policy, we will take that small table above and join it back on the original table using the band as the key. This will pair each row in the original dataset with what we think is optimal coupon value, according to this policy. Then, we sort the rows according to the `customer_id` so that we keep the same ordering we had previously. This is important for evaluation, since `ltv_with_coupons` takes as argument the coupon value in the order of the original dataframe.

```python
coupons_per_id = (model(customer_features)
                 .drop(columns=["coupons"])
                 .merge(best_coupons_per_band, on="pred_bands")
                 [["customer_id", "coupons"]]
                 .sort_values('customer_id'))

coupons_per_id.head()
```

Finally, to evaluate the policy, we pass the `coupons` column as the coupon array to the `ltv_with_coupons` function. This will regenerate the data, now assuming the coupons were given as we defined by this policy. 

```python
transactions_policy_w_model, customer_features_policy_w_model = ltv_with_coupons(
    coupons_per_id[["coupons"]].values.flatten()
)

customer_features_policy_w_model = process_data(transactions_policy_w_model, customer_features_policy_w_model)

customer_features_policy_w_model.head()
```

Just doing a sanity check again, we can see that the third customer is still the one with region 35, income 2034 and age 33. It also has a coupon value of 5 BRL, just like we've established by our policy. 

To check how much money this policy is making us, we can compute the average `net_value` mean for this new dataset.

```python
policy_w_model_gain = customer_features_policy_w_model["net_value"].mean()
policy_w_model_gain
```

Not bad! We can expect to get about 230 BRL per customer with this model policy. But wait a second! You remember how much we were making with the simple policy? Let's compare both of them side by side.

```python
plt.figure(figsize=(10,6))
sns.histplot(data=customer_features_policy_w_model, bins=40,
             x="net_value", label="Policy W/ Model", color="C0")
sns.histplot(data=customer_features_simple_policy, bins=40,
             x="net_value", label="Simple Policy", color="C1")
plt.legend()
plt.title(f"Simple Policy Gain: {simple_policy_gain}; Policy w/ Model Gain: {policy_w_model_gain};");
```

Here is where most Data Scientists fall off their chair. The policy with the model has an average `net_income` which is 20 BRL worse than a very simple policy. How can a model that is good at predicting `net_value` not be good for a strategy that aims to maximize `net_value`? Surely, there must be some bug in the code. You are clearly mistaken. This cannot be! Well, as it turns out, there is a perfectly reasonable and simple explanation for it. However, the answer to this question is so important that I think it's worth going a bit deeper into it.

## Hammering Your Thumb with Predictions

The short answer lies in understanding what we want with this policy, namely, to optimize `net_value` by playing with the coupon values. If we were to put it in an image, it's not crazy to think that `net_value` will have a quadratic shape on coupons: as we increase the coupon value `net_value` first increases, then it reaches a maximum point. After that, any additional coupon value will cost more than the value it brings.

![img](./images/appendix/opt-deriv.png)

Finding the optimal coupon value is then equivalent to finding the maximum of the `net_value` function. We can do this by differentiating the function and setting it to zero (second plot). Economists might recognize this as a pricing problem.  The way they (we) would tackle this problem would be to assume a functional form for `net_value`, differentiate it and optimize it. 

There are some great merits to this approach, but I feel is not that general and it requires a great deal of hypothesizing. Sadly, real world data doesn't come with an underlying function we can differentiate, so guesswork is often involved here. A more practical approach (and perhaps less elegant) is to test multiple coupon values and see which one yields the best `net_value`. This is exactly what our simple policy does. It looks at what happened in the past and repeats what the treatment that was shown to be the most promising one. 

Contrast this to what the model based policy does. First, the model based policy fits a machine learning model to predict `net_value`. Then, it partitions the customer space according to the predictions. If the model is good, this is approximately equal to partitioning the space by `net_value` itself, just like in the following plot.

![img](./images/appendix/model-opt.png)

The better the prediction, the more this partitioning of the space approaches partitioning on the target variable, `net_income`. Pay very close attention to what happens when you do that. Essentially, you are splitting the customer into sets where `net_value`, the thing you've predicted, doesn't change! And that makes total sense from a predictive standpoint. If your model is good at predicting, groups of points that have the same prediction will also have the same `net_value`. 

So far, so good, but look at what this does to the perceived function of `net_value` on coupons (green lines). It flattens them to have no slope at all. From the predictive point of view, this is awesome. It means that your model has captured all the variation in `net_income`. However, from the policy perspective, this is terrible, because there is no variance left in `net_income` for us to see how it would change given different coupon values. Without this variation in `net_income`, it would look like changing the coupon values has no effect on `net_income` at all, leaving us no room for optimization. By the way, this is a general phenomenon that has nothing to do with the specific quadratic shape we are using here. I'm only using an example to make things more concrete.

![img](./images/appendix/flat-curves.png)

To summarize it, whenever we want to optimise some $Y$ variable using some $T$ variable, predicting $Y$ will not only not help, it will hurt our policy, since data partitions defined by the prediction will have limited $Y$ variance, hindering our capacity to estimate how $T$ changes $Y$, that is, the elasticity $\frac{\delta Y}{\delta T} $. This is the most important paragraph of this chapter. All of it is summarized here, so reread it if you didn't understood at first. 

The key to fixing this mistake lies in adjusting our objective to what we really want. Instead of estimating $Y$ out of $X$, which is what prediction does, we need to estimate $\frac{\delta Y}{\delta T} $ out of $X$. Easier said than done. As you might have guessed already, this precisely what causal inference is all about. And, as it is natural of causal problems, we can't observe our quantity of interest $\frac{\delta Y}{\delta T} $. You simply cannot observe how `net_income` would change if we changed the coupon value because we only observe one instance of a coupon per customer. We can never know what would have happened if some different coupon value had taken place (unless we use simulated data, of course. But that's only useful for teaching purposes).

This characteristic of causal problems leads to further questions: how can I know if my model is any good if I can't see what it is supposedly estimating? How can I validate a model like that? Besides, what do we do when data is not random? How can we estimate the best policy under biased data? Those are fair questions and we shall answer them in time. Meanwhile, keep in mind that, as we change our focus from estimating $Y$ to estimating $\frac{\delta Y}{\delta T}$, lots of things will have to change accordingly. The traditional ML toolkit will need some adaptation.


## When Might Prediction Helps

All this mess with machine learning hindering our ability to infer causal effects comes from mistaking the real goal. It comes from estimating $Y$ instead of $\frac{\delta Y}{\delta T} $. But sometimes, you might actually get away when using a prediction model to achieve a causal inference goal. But for this to happen, $Y$ and $\frac{\delta Y}{\delta T} $ must be somehow correlated. For example, consider the cases on the image below.


![img](./images/appendix/waiting-time.png)

The first one is the problem we've seen before of understanding how coupons impact profitability. First, as we increase coupon value, profitability increases. If they go up together, we can see that they are positively correlated. Moreover, in the derivative plot, at the beginning, as we increase the coupon value, the elasticity $\frac{\delta Y}{\delta T} $ decreases. If we put both of them together, in the first region of the plot, as net value increases, elasticity will decrease. This means that they are negatively correlated.

But that's only at the beginning. If we keep increasing the coupon value, profitability will decrease and elasticity profitability will also decrease. This means that $\frac{\delta Y}{\delta T} $ and $Y$ are now positively correlated. So, while outcome and elasticity are negatively correlated at the beginning, this correlation reverses as we increase coupon values. This means that a prediction model won't help us here because there isn't a direct relationship between outcome and elasticity of the outcome. 

Another way of thinking about this is that prediction models make slices of the data on the $Y$ axis. If we do those sorts of slices, we will mix units that are both in the positive elasticity region and in the negative elasticity region. 

![img](./images/appendix/slice-1.png)

Now, consider a second case where we want to see how waiting in line to get answered by customers services impact customer satisfaction. In this case, we can see that customer satisfaction drops pretty fast in the first few minutes of waiting time. Customers get really pissed off when they go from not having to wait much to having to wait just a little bit. However, as waiting time increases, customer satisfaction is already so low it doesn't drop much afterwards. It sorts of saturates at a lower level. 

This case is interesting because the relationship between $\frac{\delta Y}{\delta T} $ and $Y$ doesn't change much. It is always negatively correlated. As waiting time increases, satisfaction decreases and satisfaction elasticity increases. Or, as $T$ increases, $\frac{\delta Y}{\delta T} $ also increases and  $Y$ decreases. In this case, a prediction model can be useful. The reason being that, now, if we slice on the $Y$ axis, we will group units that have similar elasticities.  

![img](./images/appendix/slice-2.png)

More generally, whenever we have these sorts of functional forms where elasticity changes ruthly in the same direction as the outcome, you might get away with prediction models. Now, be careful here. This doesn't mean they are your best bet nor that you should use them for causal problems. I'm simply stating this here so that when you come across a prediction model that works for a causal goal, you can understand what is happening. 

## A Graphical Explanation

The fact that prediction models can be harmful for personalisation is so counterintuitive and strange that I feel I must tackle the problem from multiple perspectives. The following graphical explanation is thus a way of understanding what is happening from a different angle. 

Consider the causal leftmost graph on the image below. It's a typical confounding graph, where we have features $X$ causing both the outcome $Y$ and the treatment $T$. What happens if you build a predictive model and use it to segment the units by its predictions?

![img](./images/appendix/graph-1.png)

You end up getting something like the graphs on the left (technically, they are not causal anymore, because the model obviously doesn't cause the outcome. It just learns the mapping function from features to outcomes, but that doesn't invalidate the points I'm about to make). Segmenting on this model is equivalent to conditioning on the model node. You are holding $M(T, X)$ fixed on each segment. If you do that and try to learn the relationship between $T$ and $Y$ while conditioning on $M(T, X)$, you will end up with the situation we've discussed in part I, about bad controls. And in case you don't remember, if you control for anything in the path between the treatment and the outcome, you block a path that the treatment is using to affect the outcome. As a consequence, the perceived effect ends up looking smaller than the actual effect. So there you have it. We used another explanation to get to the exact same conclusion as before: segmenting units by a predictive model hinders our ability to identify the causal effect. 

Now, if you are smart, you will probably object with the following idea. What if we simply remove $T$ from the model? That's a really good argument. In fact, it's so good it took me years to properly articulate why just trying to fool your model like that is probably not a smart thing to do. In fact, if you look back on the example we've explored in this chapter, notice that I didn't even give the predictive model the `coupon` treatment. That didn't solve the problem that the predictive model partitioning was squashing our measured elasticity to a horizontal line. The answer to why lies in the following image.

![img](./images/appendix/graph-2.png)

Excluding the treatment $T$ from your model $M(X)$ is not enough to make the model ignore the relationship between $T$ and $Y$. Because $X$ causes $T$, the model can indirectly learn $T$ through $X$. Why will it do that? Because if $T$ causes $Y$ and $M$ wants to predict $Y$ as much as possible,  it will forcibly explore the pattern in $X$ that links $T$ to $Y$. In other words, the information of how the treatment impacts the outcomes flows backwards to our model through the features. To give an example, consider that you only give discounts ($T$) to customers that live in a certain area of town ($X$). For the sake of the argument, suppose that the area doesn't cause sales ($Y$) in any way. Even if that is the case, the city area still has predictive power because it causes $T$, which in turns causes $Y$. Hence, the model will explore that fact in order to predict $Y$. 

To be fair to this argument, you actually can fool your model a little bit by not giving it $T$. To see that, I recommend you rerun the code on our examples but this time include `coupons` in the list of features to the predictive model. You will see that the Model Policy will perform even worse. This means that NOT giving $T$ to our model is doing some good, although not much. 

Finally, there is one way out of this, which is when $T$ is random. In that very particular case, a predictive model can actually help you with causal inference. But first, let's understand why it doesn't hurt. When $T$ is random, this excludes any confounding that comes from $X$. In this case, $X$ have no information about how $T$ comes to be.

![img](./images/appendix/graph-3.png)

When that happens, even if you control for $M(X)$, you are still allowing all the effect from $T$ to flow onto $Y$. In other words, even if you look inside segments where $M(X)$ is fixed, $Y$ will still vary due to $T$. $M(X)$ can no longer control that variability because it has no way of learning $T$ anymore. Not only that, this sort of condition will actually help you figure out the Average Treatment Effect. We also saw that on Part I. If we control for variables that are good predictors of $Y$ but that don't cause $T$, we will reduce the variance of our causal estimates. The intuitive reason is that a lot of the variation on $Y$ that is due to $X$ is removed by conditioning on the model. Since $X$ is then explained away, the reason why $Y$ is still changing must be mostly due to $T$ (or other unmeasured things, but you get the point that it helps). 

Again, this isn't an argument in favor of using predictive models when the goal is causal inference. In fact, segments defined by a predictive model will help you identify the ATE, but that doesn't mean each partition will have a different ATE (or elasticity). Remember that not only do you want to estimate the elasticity, but you also want to find segments where it is higher or lower than average so that you can personalise the treatment. 

## Contribute

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。


---

```python
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf
from toolz import *
```

# Why Prediction Metrics are Dangerous For Causal Models
 
A common misconception I often hear is that, if we have random data, to evaluate a causal model we could just evaluate the predictive performance of such model on the random dataset, using a metric like $R^2$. Unfortunately things are not that simple and I'll try to explain why. 
 
Generally speaking, we can say any outcome is a function of the treatment and covariates
 
$$
Y = F(x, t)
$$
 
Let's say we can decompose this function into two additive pieces. One piece that doesn't depend on the treatment and another that depends only on the treatment and possible interactions.
 
$$
Y = g(x) + f(t,x)
$$
 
This additive structure places some restriction on the functional form but not much, so we can argue it is a pretty general way of describing a Data Generating Process (DGP).
 
The point is that if the treatment effect is weaker than the covariates effect, then, **even if we have random data**, we can have a model which has higher predictive power but is bad for causal inference. All that model has to do is approximate $g(x)$ while disregarding $f(t,x)$.
 
In other words, **predictive performance on a random dataset does not translate our preference for how good a model is for causal inference**.
 
To show that, let's use some simulated data.
 
## Simulating Data
 
In the following DGP, we have covariates $X$ that have a high predictive power but don't interact with the treatment. In other words, they don't dictate the treatment effect heterogeneity. We also have features $W$ that only impact the outcome through the treatment (are not confounders). Since the treatment has low predictive power, $W$ also doesn't have much predictive power.
 
$$
Y_i = g(X_i) + f(T_i,W_i) + e_i
$$

```python
n = 100000
n_features = 20
n_heter = 10

np.random.seed(12321)

X = np.random.normal(1, 10, (n, n_features))
nuisance = np.random.uniform(-1,1, (n_features, 1))

W = np.random.normal(1, 10, (n, n_heter))
heter_y = np.random.uniform(-1,1, (n_heter, 1))

T = np.random.normal(10, 2, (n, 1)) # T is random!
Y = np.random.normal(T + T*W.dot(heter_y) + 20*X.dot(nuisance), 0.1)

df = pd.concat([
    pd.DataFrame(X, columns=[f"f{f}" for f in range(n_features)]),
    pd.DataFrame(W, columns=[f"w{f}" for f in range(n_heter)])
], axis=1).assign(T=T, Y=Y)
```

Now, let's break that dataset into a training and a test set

```python
from sklearn.model_selection import train_test_split

train, test = train_test_split(df, test_size=0.5)
train.shape, test.shape
```

and train two models for treatment effect heterogeneity, $M1$ and $M2$. $M1$ will include the highly predictive features that don't affect the treatment heterogeneity and $M2$ will include the low predictive features that do affect treatment heterogeneity.

```python
m1 = smf.ols("Y~T*(" + "+".join([f"f{f}" for f in range(n_features)])+")", data=df).fit()
m2 = smf.ols("Y~T*(" + "+".join([f"w{f}" for f in range(n_heter)])+")", data=df).fit()
```

If we look at the predictive power of both models using the $R^2$, indeed, $M1$ is much better than $M2$. 

```python
from sklearn.metrics import r2_score

print("M1:", r2_score(test["Y"], m1.predict(test)))
print("M2:", r2_score(test["Y"], m2.predict(test)))
```

Now, let's calculate the cumulative elasticity curve for both models. For that, we will need Conditional Average Treatment Effect predictions. Since all the models here are linear, we can use the following formula to get CATE predictions
 
$$
\hat{CATE_i} = M(X, W, t) - M(X, W, t-1)
$$

```python
@curry
def elast(data, y, t):
        return (np.sum((data[t] - data[t].mean())*(data[y] - data[y].mean())) /
                np.sum((data[t] - data[t].mean())**2))
    

def cumulative_gain(dataset, prediction, y, t, min_periods=30, steps=100):
    size = dataset.shape[0]
    ordered_df = dataset.sort_values(prediction, ascending=False).reset_index(drop=True)
    n_rows = list(range(min_periods, size, size // steps)) + [size]
    return np.array([elast(ordered_df.head(rows), y, t) * (rows/size) for rows in n_rows])

```

```python
test_pred = test.assign(
    cate_1 = m1.predict(test) - m1.predict(test.assign(T=test["T"]-1)),
    cate_2 = m2.predict(test) - m2.predict(test.assign(T=test["T"]-1))
)
```

Once we have those CATE predictions, we can evaluate them using the cumulative elasticity curve

```python
cumelast_1 = cumulative_gain(test_pred, "cate_1", "Y", "T", steps=100)
cumelast_2 = cumulative_gain(test_pred, "cate_2", "Y", "T", steps=100)
```

```python
plt.plot(range(len(cumelast_1)), cumelast_1, label="M1")
plt.plot(range(len(cumelast_2)), cumelast_2, label="M2")
plt.plot([0, 100], [0, elast(test_pred, "Y", "T")], linestyle="--", label="Random Model", color="black")
plt.legend()
plt.ylabel("Cumulative Elasticity");
```

As we can see, now $M1$ is much worse than $M2$, even though it has a higher $R^2$ on the test set. This shows that predictive power doesn't translate directly to a good causal model, even if we use random data.

## Predictive Metric For Causal Inference
 
This doesn't mean we can't come up with a way to correctly evaluate a causal model using predictive metrics. In order to do so, let's go back to our additive assumption about de DGP.
 
$$
Y = g(x) + f(t,x)
$$
 
To use a predictive metric, we need to somehow transform the outcome in order to remove the $g(x)$ component from it. That way, all the remaining predictive power will necessarily be used to learn the causal relationship.
 
$$
\tilde{Y} = Y - g(x) = f(t,x)
$$
 
One way of doing that is by orthogonalizing $Y$. We can use any ML model to estimate $g$ and get out of fold residuals
 
$$
\tilde{Y} = Y - \hat{g}(x)
$$

```python
denoise_m = smf.ols("Y~"+
                    "+".join([f"w{f}" for f in range(n_heter)])+
                    "+"+
                    "+".join([f"f{f}" for f in range(n_features)]), data=train).fit()

test_res = test.assign(Y_res = test["Y"] - denoise_m.predict(test) + test["Y"].mean())
```

Once we do that, we can evaluate the power of each model in predicting the residualized outcome $\tilde{Y}$. The predictive performance on this new outcome will directly translate to a better causal model.

```python
print("M1:", r2_score(test_res["Y_res"], m1.predict(test_res)))
print("M2:", r2_score(test_res["Y_res"], m2.predict(test_res)))
```

The downside of this approach is that it depends on how well you can estimate $g(x)$.

---

```python
import numpy as np
import pandas as pd
from toolz import curry
import seaborn as sns
from matplotlib import pyplot as plt
import statsmodels.formula.api as smf
import cvxpy as cp

import toolz as f

from sklearn.linear_model import Lasso
import warnings
warnings.filterwarnings('ignore')

from matplotlib import style
style.use("ggplot")
```

# Conformal Inference for Synthetic Controls
 
## Synthetic Control Refresher
 
Synthetic Control (SC) is a particularly useful causal inference technique for when you have a single treatment unit and very few control units, but you have repeated observation of each unit through time (although there are plenty of SC extensions in the Big Data world). The canonical use case is when you want to know the impact of the treatment in one geography (like a state) and you use the other untreated states as controls. In our Synthetic Control chapter, we've motivated the technique by trying to estimate the effect of Proposition 99 (a bill passed in 1988 that increased cigarette tax in California) in cigarette sales. 
 
In order to do that, we have to estimate what would have happened to California, had it not passed Proposition 99. This boils down to estimating the counterfactual $Y_{t}(0)$ so that we can compare it to the observed outcome in the post intervention periods:
 
$$
ATT = Y_{t}(1) - Y_{t}(0) = Y_{t} - Y_{t}(0)  \text{ for } t \geq 1988
$$
 
There are many methods to do that, among which, we have Synthetic Controls. Synthetic Controls tries to model $Y(0)$ for the treated unit by combining multiple control units in such a way that they mimic the pre-treatment behavior of the treated unit. In our case, this means finding a combination of states that, together,  approximate the cigarette sales trend in California prior to Proposition 99. This is done because we rarely have a control unit that follows the same pattern as the treatment unit. We can see that by plotting the cigarette sales trend for multiple states. Notice none of them have a trend that closely resembles that of California. 

```python
data = pd.read_csv("data/smoking.csv")

data = data.pivot(index="year", columns="state", values="cigsale")
data = data.rename(columns={c: f"state_{c}" for c in data.columns}).rename(columns={"state_3": "california"})
data.shape
```

```python
plt.figure(figsize=(10,5))
plt.plot(data.drop(columns=["california"]), color="C1", alpha=0.5)
plt.plot(data["california"], color="C0", label="California")
plt.vlines(x=1988, ymin=40, ymax=300, linestyle=":", lw=2, label="Proposition 99", color="black")
plt.legend()
plt.ylabel("Cigarette Sales")
```

That is why we combine multiple treated units. The goal is, if we don't have a good enough control, we can craft a synthetic one that resembles the treated unit the way we want. 
 
In order to find the combination of states that better approximate the pretreatment trend of California, the Synthetic Control method runs a horizontal regression, where the rows are the time periods and the columns are the states. It tries to find the weights that, when multiplied by the control states, better approximate the treated state
 
![img](./images/appendix/regr_space.png)
 
Since we have more states (39, some were discarded from the analysis) than time periods, an unconstrained regression would simply overfit, which is why Synthetic Control imposes two restrictions:
 
1. Weights must sum to 1;
2. Weights must be non-negative;
 
Or, in mathematical terms, let $\pmb{y}$ be the vector of outcomes for the treated state in the pre-treated periods, $\pmb{X}$ the $J$ by $T0$ matrix, where each column is a state $j$ and each row is a period $t$ prior to the intervention period, $T1 = T0 + 1$
 
$$
\underset{w}{\mathrm{argmin}} \ ||\pmb{y} - \pmb{X} \pmb{w}|| \\
\text{s.t } \ \sum w_j = 1 \text{ and } \ w_j > 0 \ \forall \ j
$$
 
Combined, these constraints means we are defining the synthetic control as a convex combination of the control units. It also means we are not doing any dangerous extrapolation and that our synthetic control will use only a small subset of control units. 
 
![img](./images/appendix/extrapolation.png)
 
Here is what this looks like in code, as an Sklearn estimator:

```python
from sklearn.base import BaseEstimator, RegressorMixin
from sklearn.utils.validation import check_X_y, check_array, check_is_fitted
import cvxpy as cp

class SyntheticControl(BaseEstimator, RegressorMixin):

    def __init__(self,):
        pass

    def fit(self, X, y):

        X, y = check_X_y(X, y)
    
        w = cp.Variable(X.shape[1])
        objective = cp.Minimize(cp.sum_squares(X@w - y))
        
        constraints = [cp.sum(w) == 1, w >= 0]
        
        problem = cp.Problem(objective, constraints)
        problem.solve(verbose=False)
        
        self.X_ = X
        self.y_ = y
        self.w_ = w.value
        
        self.is_fitted_ = True
        return self
        
        
    def predict(self, X):

        check_is_fitted(self)
        X = check_array(X)
        
        return X @ self.w_
```

Let's apply this method to our data, fitting it in the pre-intervention period (prior to 1988).

```python
model = SyntheticControl()

train = data[data.index < 1988]

model.fit(train.drop(columns=["california"]), train["california"]);
```

We can now plot, side by side the trend for California and for the synthetic control we've just created. The difference between these two lines is the estimated effect of Proposition 99 in California.

```python
plt.plot(data["california"], label="California")
plt.plot(data["california"].index, model.predict(data.drop(columns=["california"])), label="SC")
plt.vlines(x=1988, ymin=40, ymax=120, linestyle=":", lw=2, label="Proposition 99", color="black")

plt.legend();
```

From the look of this plot, it looks like Proposition 99 had a pretty big effect on the reduction of cigarette sales.

```python
pred_data = data.assign(**{"residuals": data["california"] - model.predict(data.drop(columns=["california"]))})

plt.plot(pred_data["california"].index, pred_data["residuals"], label="Estimated Effect")
plt.hlines(y=0, xmin=1970, xmax=2000, lw=2, color="Black")
plt.vlines(x=1988, ymin=5, ymax=-25, linestyle=":", lw=2, label="Proposition 99", color="Black")

plt.legend();
```

## Inference for Grown Ups
 
In the Synthetic Control chapter, we showed an inference procedure where we've permuted units, pretending control units where treated. This is also referred to as a placebo test, where we check the effect of units that haven't gone through the treatment. If the estimated effect in the treated unit is bigger than most of the placebo effects, we say that this effect estimate is significant.

```python
plt.figure(figsize=(10,5))
for state in data.columns:
    
    model_ier = SyntheticControl()
    train_iter = data[data.index < 1988]
    model_ier.fit(train_iter.drop(columns=[state]), train_iter[state])
    
    effect = data[state] - model_ier.predict(data.drop(columns=[state]))
    
    is_california = state == "california"
    
    plt.plot(effect,
             color="C0" if is_california else "C1",
             alpha=1 if is_california else 0.5,
             label="California" if is_california else None)

plt.hlines(y=0, xmin=1970, xmax=2000, lw=2, color="Black")
plt.vlines(x=1988, ymin=-50, ymax=100, linestyle=":", lw=2, label="Proposition 99", color="Black")
plt.ylabel("Effect Estimate")
plt.legend();
```

In our example, we can see that the post-treatment difference for California is quite extreme, when compared to the other states. However, there are also some states with terrible pre-treatment fit, which then translates to a huge error in the post-intervention period. The guideline here is to remove units with high pretreatment error, but how high is a bit more complicated. Not only that, this procedure assumes a random assignment of the intervention, which is hard to believe for this kind of policy intervention (see Abadie, 2021)
 
One alternative method for inference is to recast the problem of effect estimation as counterfactual prediction. If you think about it, all we are trying to do is predict the counterfactual $Y_{i, t}(0)$ where $i$ is the treated unit and $t \geq T1$, that is, in the post intervention period. If we do that, we can leverage the literature on **Conformal Prediction for inference**. Interestingly enough, this method is quite general and applies to other models of $Y_{i, t}(0)$ but let's focus just on Synthetic Controls here.
 
To understand this procedure, let's first look at how we would do Hypothesis Tests and get P-Values.

### Hypothesis Test and P-Values
 
Let's say we are interested in testing the Hypothesis about the trajectory of effects in the post treatment period $\theta = (\theta_{T0+1}, ..., \theta_{T})$
 
$$
H_0 : \theta = \theta^0
$$
 
For instance, if we wish to test for no effect whatsoever, we can set $\theta^0 = (0, ..., 0)$. Notice that this hypothesis fully determines the counterfactual outcome in the absence of treatment:
 
$$
Y_t(0) = Y_t(1) - \theta_t = Y_t - \theta_t
$$
 
The key idea is to then generate data following the null hypothesis we want to test and check the residuals of a model for $Y(0)$ in this generated data. If the residuals are too extreme, we say that the data is unlikely to have come from the null hypothesis we've postulated. If this whole procedure sounds obscure at first, don't worry, it will become clear as we walk through a step by step implementation of it. 
 
The first step is to generate data under the null hypothesis. This is achieved by simply subtracting the postulated null from the outcome of the treated unit, just like in the equation above. Here is the code to do that.

```python
def with_effect(df, state, null_hypothesis, start_at, window):
    window_mask = (df.index >= start_at) & (df.index < (start_at +window))
    
    y = np.where(window_mask, df[state] - null_hypothesis, df[state])
    
    return df.assign(**{state: y})
```

```python
plt.plot(with_effect(data, "california", 0, 1988, 2000-1988+1)["california"], label="H0: 0")
plt.plot(with_effect(data, "california", -4, 1988, 2000-1988+1)["california"], label="H0: -4")

plt.ylabel("Y0 Under the Null")
plt.legend();
```

If we postulate the null of no effect, the data under that null means that $Y(0) = Y(1) = Y$, which is just the trajectory of observed outcome we see for the treated state of California. Now, if we postulate that the null is -4, that is, Proposition 99 decreases cigarette sales by 4 packs, then $Y(0) = Y(1) - (-4)$, which shifts the trajectory of the post treatment outcomes by +4. This is very intuitive. If we think the bill decreases cigarette sales, then, in the absence of it, we should see higher levels of cigarette sales than the one we have in our observed data. 
 
The next part of the inference procedure is to fit a model for the counterfactual $Y(0)$ (which we get with the function we just created) in the entire data, pre **and** post-treatment period. This is an important distinction between how we usually fit synthetic controls. The idea here is that the model must be estimated with the entire data, under the postulated null hypothesis, to avoid huge post intervention residuals. With this model, we then compute the residuals $\hat{u_t} = Y_t - \hat{Y}_t(0)$ for all time periods $t$.
 
The function to do that first uses the `with_effect` function we created earlier to generate data under then null. Then, it fits the model in this data under the null. Next, we estimate $Y(0)$ by making predictions with the recently fit model. Finally, we compute the residuals $\hat{u}_t$ and stores everything in a dataframe.

```python
@curry
def residuals(df, state, null, intervention_start, window, model):
    
    null_data = with_effect(df, state, null, intervention_start, window)
            
    model.fit(null_data.drop(columns=[state]), null_data[state])
    
    y0_est = pd.Series(model.predict(null_data.drop(columns=[state])), index=null_data.index)
    
    residuals = null_data[state] - y0_est
    
    test_mask = (null_data.index >= intervention_start) & (null_data.index < (intervention_start + window))
    
    return pd.DataFrame({
        "y0": null_data[state],
        "y0_est": y0_est,
        "residuals": residuals,
        "post_intervention": test_mask
    })[lambda d: d.index < (intervention_start + window)]  # just discard  points after the defined effect window
```

With our data, to get the residuals for $H_0 : 0$, meaning Proposition 99 had no effect, we can simply pass 0 as the null for our function. 

```python
model = SyntheticControl()

residuals_df = residuals(data,
                         "california",
                         null=0.0,
                         intervention_start=1988,
                         window=2000-1988+1,
                         model=model)

residuals_df.head()
```

The result is a dataframe containing the estimated residuals for each time period, something we will use going forward. Remember that the idea here is to see if that residual, in the post intervention period, is too high. If it is, the data is unlikely to have come from this null, where the effect is zero. To get a visual idea of what we are talking about, we can inspect the error of our model in the post intervention period.

```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 4))
residuals_df[["y0", "y0_est"]].plot(ax=ax1)
ax1.set_title("Y0 under H0: 0");
residuals_df[["residuals"]].plot(ax=ax2);
ax2.set_title("Residuals under H0: 0");
```

We can already see that the model fitted under $H_0: 0$ yields quite large and negative residuals, which is some evidence we might want to reject this null of no effect. 

#### Test Statistic
 
This visual evidence is interesting for our own understanding, but we need to be a bit more precise here. This is done by the definition of a **Test Statistic S**, which summarizes how big are the residuals and hence, how unikly is the data we saw, under the null. 
 
$$
S(\hat{u})_q = \bigg(\sum_{t=T0 + 1}^{T} |u_t|^q \bigg) ^{1/q}
$$
 
Here, we focus on $q=1$, which gives us $S(\hat{u}) = \sum_{t=T0 + 1}^{T} |u_t|$.
 
Notice that this statistic is computed using only the post-intervention period, with $t \geq T0 + 1$. So, although we use all the data to fit our model for the counterfactual $Y(0)$, we check the residuals only for the outcome which concerns the formulated null hypothesis, that is, the post-intervention period. 

```python
def test_statistic(u_hat, q=1, axis=0):
    return (np.abs(u_hat) ** q).mean(axis=axis) ** (1/q)
```

```python
print("H0:0 ", test_statistic(residuals_df.query("post_intervention")["residuals"]))
```

High values of this test statistic indicate poor post intervention fit and, hence rejection of the null. However, we could have pretty big test statistics in the post-intervention period if our model is poorly fitted, even if $H_0$ is true. This means we can't define high in absolute terms. Rather, we have to think about how high are the post intervention residuals - and test statistics - in comparison to the pre-intervention residuals. 
 
#### P-Value
 
To compute the P-value, we block-permute the residuals, calculating the test statistic in each permutation. This procedure is better understood by the following picture
 
![img](./images/appendix/block-perm.png)
 
Once we do that, we will end up with $T$ test statistics, one for each of the block permutations.
 
Let $\Pi$ be the set of all block permutations, by the definition of P-value 
 
$$
\text{P-value} = \frac{1}{|\Pi|}\sum_{\pi \in \Pi} \mathcal{1}\{S(\hat{u}_{\pi_0}) \leq S(\hat{u}_{\pi})\}
$$
 
and $\hat{u}_{\pi_0}$ is the original (unpermuted) vector or residuals. In plain terms, we are simply finding the proportion of times that the unpermuted test statistic is higher (more extreme) than the test statistics obtained by all possible block permutations. 
 
To implement this, we will make use of the `np.roll` function, which takes an array and circles it, mujustch like we've represented in the image above.

```python
def p_value(resid_df, q=1):
    
    u = resid_df["residuals"].values
    post_intervention = resid_df["post_intervention"].values
    
    block_permutations = np.stack([np.roll(u, permutation, axis=0)[post_intervention]
                                   for permutation in range(len(u))])
    
    statistics = test_statistic(block_permutations, q=1, axis=1)
    
    p_val = np.mean(statistics >= statistics[0])

    return p_val
```

We can now compute the P-value for $H_0: 0$. As we can see, it is a low P-value, but not extremely low. At $\alpha=0.1$, we would not reject this null of no effect. 

```python
p_value(residuals_df)
```

Remember, this is the P-value for the null hypothesis which states that the effect in all time periods is zero: $\theta = (\theta_{T0+1}=0, ..., \theta_{T}=0)$. From our effect plot from the Synthetic Control, we get the feeling that the effect of Proposition 99 is not a fixed number. We can see that it starts small, around -5, but gradually increases to -25. For this reason, it might be interesting to plot the confidence interval for effect each post treatment period individually, rather than just testing a null hypothesis about an entire affect trajectory.

### Confidence Intervals
 
To understand how we can place a confidence interval around the effect of each post-treatment period, let's first try to understand how we would define the confidence interval for a single time period. If we have a single period, then $H_0$ is defined in terms of a scalar value, rather than a trajectory vector $\theta$. This means we can generate a fine line of $H_0s$ and compute the P-value associated with each null. For example, Let's say we think the effect of Proposition 99 in the year 1988 (the year it passed) is somewhere between -20 and 20. We can then build a table containing a bunch of $H_0$, from -20 to 20, and each associated P-value:
 
```
P-value(H_0: -20) = 0.01
P-value(H_0: -19) = 0.01
P-value(H_0: -18) = 0.02
...
P-value(H_0: 18) = 0.03
P-value(H_0: 19) = 0.03
P-value(H_0: 20) = 0.02
```
 
With the functions we've defined, this can be achieved by first appending the period of interest (1988 in this example) at the end of the pre-intervention period, creating what is called an augmented dataset. Then, we iterate over the fine line of nulls, computing the p-value of a post-intervention window of size 1, which starts at the period of interest

```python
def p_val_grid(df, state, nulls, intervention_start, period, model):
    
    df_aug = pd.concat([df[df.index < intervention_start], df.loc[[period]]])
    
    p_vals =  {null: p_value(residuals(df_aug,
                                       state,
                                       null=null,
                                       intervention_start=period,
                                       window=1,
                                       model=model)) for null in nulls}        
        
    return pd.DataFrame(p_vals, index=[period]).T
```

```python
model = SyntheticControl()

nulls = np.linspace(-20, 20, 100)

p_values_df = p_val_grid(
    data,
    "california",
    nulls=nulls,
    intervention_start=1988,
    period=1988,
    model=model
)

p_values_df
```

As you can see, the result is a table where the row index is the null hypothesis and the row values are the p-values.
 
To build the confidence interval, all we need to do is filter out the $H_0$s that gave us a low P-value. Remember that low p-value means that the data we have is unlikely to have come from that null. For instance, if we define the significant level $\alpha$ to be 0.1, we remove $H_0$s that have P-value lower than 0.1. 

```python
def confidence_interval_from_p_values(p_values, alpha=0.1):
    big_p_values = p_values[p_values.values >= alpha]
    return pd.DataFrame({
        f"{int(100-alpha*100)}_ci_lower": big_p_values.index.min(),
        f"{int(100-alpha*100)}_ci_upper": big_p_values.index.max(),
    }, index=[p_values.columns[0]])
```

```python
confidence_interval_from_p_values(p_values_df)
```

This gives us the confidence interval for the effect in 1988.
 
We can also plot the $H_0$ by P-value to better understand how this confidence interval was obtained. In the figure below, the dashed line is the 0.1 line, which is the $\alpha$ we've specified. The blue lines mark the confidence intervals. $H_0$ outside these lines have a P-value lower than 0.1.

```python
plt.plot(p_values_df[1988], p_values_df.index)
plt.xlabel("P-Value")
plt.ylabel("H0")
plt.vlines(0.1, nulls.min(), nulls.max(), color="black", ls="dotted", label="0.1")

plt.hlines(confidence_interval_from_p_values(p_values_df)["90_ci_upper"], 0, 1, color="C1", ls="dashed")
plt.hlines(confidence_interval_from_p_values(p_values_df)["90_ci_lower"], 0, 1, color="C1", ls="dashed", label="90% CI")

plt.legend()
plt.title("Confidence Interval for the Effect in 1988");
```

All there's left to do is repeat the procedure above for each time period. This means that, for each post intervention year, appending it to the end of the pre-intervention period to create the augmented dataset and then computing the confidence interval just like we've done above.
 
![img](./images/appendix/aug-data.png)

```python
def compute_period_ci(df, state, nulls, intervention_start, period, model, alpha=0.1):
    p_vals = p_val_grid(df=df,
                        state=state,
                        nulls=nulls,
                        intervention_start=intervention_start,
                        period=period,
                        model=model)
    
    return confidence_interval_from_p_values(p_vals, alpha=alpha)


def confidence_interval(df, state, nulls, intervention_start, window, model, alpha=0.1, jobs=4):    
    return pd.concat([compute_period_ci(df, state, nulls, intervention_start, period, model, alpha)
                     for period in range(intervention_start, intervention_start+window)])
```

We are now ready to compute the confidence interval for all the post-intervention periods

```python
model = SyntheticControl()

nulls = np.linspace(-60, 20, 100)

ci_df = confidence_interval(
    data,
    "california",
    nulls=nulls,
    intervention_start=1988,
    window=2000 - 1988 + 1,
    model=model
)

ci_df
```

```python
plt.figure(figsize=(10,5))
plt.fill_between(ci_df.index, ci_df["90_ci_lower"], ci_df["90_ci_upper"], alpha=0.2,  color="C1")
plt.plot(pred_data["california"].index, pred_data["residuals"], label="California", color="C1")
plt.hlines(y=0, xmin=1970, xmax=2000, lw=2, color="Black")
plt.vlines(x=1988, ymin=10, ymax=-50, linestyle=":", color="Black", lw=2, label="Proposition 99")
plt.legend()
plt.ylabel("Gap in per-capita cigarette sales (in packs)");
```

## Contribute

**《Causal Inference for the Brave and True》** 是一本关于因果推断的开源教材，致力于以经济上可负担、认知上可理解的方式，普及这门“科学的统计基础”。全书基于 Python，仅使用自由开源软件编写，原始英文版本由 [Matheus Facure](https://github.com/matheusfacure) 编写与维护。

本书的中文版由黄文喆与许文立助理教授合作翻译，并托管在 [GitHub 中文主页](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。希望本地化的内容能帮助更多中文读者学习和掌握因果推断方法。

如果你觉得这本书对你有帮助，并希望支持该项目，可以前往 [Patreon](https://www.patreon.com/causal_inference_for_the_brave_and_true) 支持原作者。

如果你暂时不方便进行经济支持，也可以通过以下方式参与贡献：

* 修正错别字
* 提出翻译或表达建议
* 反馈你未能理解的部分内容

欢迎前往英文版或中文版仓库点击 [issues 区](https://github.com/matheusfacure/python-causality-handbook/issues) 或 [中文版 issues 区](https://github.com/Wenzhe-Huang/python-causality-handbook-zh/issues) 提出反馈。

最后，如果你喜欢这本书的内容，也请将其分享给可能感兴趣的朋友，并为项目在 GitHub 上点亮一颗星：[英文版仓库](https://github.com/matheusfacure/python-causality-handbook) / [中文版仓库](https://github.com/Wenzhe-Huang/python-causality-handbook-zh)。
