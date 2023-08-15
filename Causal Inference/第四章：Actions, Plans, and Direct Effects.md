
这一章会对之前把变量变成常量的do算子做一些扩展；
行动对于观测的状态，因果模型的主要作用——找到预料之外的结果；
拓展do算子——引入概率和条件；
intervention calculus给出可识别的semi-Markovian models的表征
评估sequential plans结果（时变动作）；
区分直接和非直接效应

# 4.1引入
——介绍什么是actions，actions分析的作用action和反事实

## 1. Actions, Acts, and Probabilities
对action的解释：反应性（A->B->C)和审视性解释(A?B?)；actions->act
一个关于行动和行为 的悖论（do——act和condition——action）；Evidential decision theory
*行动不是概率论的一部分：probabilities capture normal relationships in the world, whereas actions represent interventions that perturb those relationships

*By analogy, the additional information required for describing the transformation from P(s) to PA(s) should identify those elements of the world that remain invariant under the action do(A)
这部分工作交给了do算子，下一个部分 是它和标准方法的比较

## 2.Actions in Decision Analysis
传统的方法是把这些差异归因到seeing和doing的差异；
意义在于把行动者当成了变量，建立增广函数和决策变量；
同时把操作主体是做自由的实验者——所有决策变量外生
The ability to predict the effect of interventions without enumerating those interventions in advance
困难：我们需要事先预测并明确表示我们需要评估的所有行动
优点：predict the effect of interventions without enumerating those interventions in advance

## 3.Actions and Counterfactuals
bayes之外的一种概率转换形式：image——第七章详细说


# 4.2 CONDITIONAL ACTIONS AND STOCHASTIC POLICIES
考虑更复杂的intervention情况
公式***assumption：Z cannot be a descendant of X;
$$P(y|do(X=g(z))=E_z[P(y|\hat{x},z)|_{x=g(z)}]$$
*干预的函数可识别那么干预一定可识别；但是反之不成立；

随机政策——引入新的分布P*
公式（直接给定）直观上理解是某种期望的意思
![[Pasted image 20230602153622.png]]
# 4.3 Action的效应什么时候是可以识别的？
讨论更一般的情况下因果效应P什么时候是可以识别的
## 1. Graphical Conditions for Identification
对可识别性定义——semi-Markovian model
单节点情况下：
+ **TH：**Let X and Y denote two singleton variables in a semi-Markovian model characterized by graph G. A sufficient condition for the identifiability of $P(y|\hat{x})$ is that G satisfy one of the following four conditions.
![[Pasted image 20230602153750.png]]
+ **TH：必要性** At least one of the four conditions of Theorem 4.3.1 is necessary for identifiability. That is, if all four conditions of Theorem 4.3.1 fail in a graph G, then there exists no finite sequence of inference rules that reduces to a hat-free expression.
## 2. Remarks on Efficiency
上面的定理第三个条件需要暴力搜索，有没有什么方法需要提高效率？
**TH：**
![[Pasted image 20230602153909.png]]
->一个最小可辨识，其他最小可辨识

例子：知道分布 和 但是不知道联合分布.
+ **Lemma：**
![[Pasted image 20230602154007.png]]
+ **TH：什么情况下那个是可识别的**
![[Pasted image 20230602154018.png]]
+ **TH：condition4的“遗传”**
![[Pasted image 20230602154029.png]]

## 3. Deriving a Closed-Form Expression for Control Queries
a closed-form expression for$P(Y|\hat{X})$
算法
![[Pasted image 20230602154155.png]]
![[Pasted image 20230602154207.png]]
## 4.summary



# 4.4 动态规划的识别——THE IDENTIFICATION OF DYNAMIC PLANS
这一节包括了一些不可测得的变量，为什么不可测？包含同时或者序列的动作并且会被祖先影响
目的：建立识别准则
## 1. Motivation
一个例子：我们为什么要处理这种情况
第三章我们处理不能观测的变量的思路是控制confounders；但是我们目前的情况是confounder被控制变量影响
这个例子里面的问题：两个控制变量不能看作整体也不能拆开——都不是identifiable
那怎么办？注意到X1满足后门准则，把联合概率写成可识别的形式
更特殊的模式呢？比如函数

任务：in general, by graphical means, whether a proposed plan can be evaluated from the joint distribution on the observables and, if so, to identify which covariates should be measured and how they should be adjusted.
## 2. 记号和假设
control problems;
plan;
conditional plan->不包含任何X的后代

A control problem is identifiable whenever the distribution is identifiable.
$P(y |\hat{x}, z)$


## 3.Plan Identification: The Sequential Back-Door Criterion
+ **TH:The Sequential Back-Door Criterion**
![[Pasted image 20230602154357.png]]
![[Pasted image 20230602154412.png]]
Proof:
...

有没有可能不用穷举Z呢？后面再说

+ **def:Admissible Sequence and G-Identifiability**
![[Pasted image 20230602154439.png]]
+ Corollary 4.4.3:A control problem is G-identifiable if and only if it has an admissible sequence.
控制问题中G-识别和admissible sequence.的等价性

新问题：G-可识别是充分的

## 4.Plan Identification: A Procedure
上一个定理找Z的过程太麻烦
例子
避免坏的选择：坚持Z的最小
问题：最小的Z不唯一

+ **TH：安全性定理** 
![[Pasted image 20230602154533.png]]
+ **推论：算法**
![[Pasted image 20230602154539.png]]
也就是说我们可以把上面的定理的admissible sequence的Z们写成更简单的W
+ **TH：**
![[Pasted image 20230602154603.png]]
问题：The implication of this order sensitivity is that, whenever G permits several orderings of the control variables, all orderings need be examined before we can be sure that a plan is not G-identifiable.



# 4.5 DIRECT AND INDIRECT EFFECTS
## 1. 区分直接效应和全部效应
例子；一些计算直接效应的直观想法：控制中间变量
问题：需要剥离直接效A，虚假关联的问题；

## 2. 直接效应的定义和识别

用do算子估计非实验数据

+ **def：直接效应**
![[Pasted image 20230602154639.png]]
如果我们知道了变量的结构我们没有必要hold所有的变量

+ **推论：**
![[Pasted image 20230602154650.png]]

Corollary 4.5.2 implies that the direct effect of X on Y is identifiable whenever is identifiable
we conclude that a direct effect is identifiable whenever the effect of the corresponding parents’ plan is identifiable.

引入图：
+ **TH:**
![[Pasted image 20230602154700.png]]
推论：什么时候显然不可识别
![[Pasted image 20230602154727.png]]

## 3.例子
## 4.自然直接效应
线性模型里面我们的直接效应对输出节点的值不敏感，但是非线性不是
+ **natural direct effect：概率差的期望**
![[Pasted image 20230602154819.png]]
一般来说不一定可识别
在没有混淆的情况下：
TH：（用后门准则）
![[Pasted image 20230602155131.png]]
*TH在Markovian models中有效


## 5.间接效应
+ **def：indirect effect** 
![[Pasted image 20230602154921.png]]
*注意区别。谁是常量
def：总效应TE
![[Pasted image 20230602155210.png]]
在一个特殊的情况：（没有混淆）Mediation Formula
![[Pasted image 20230602155219.png]]
>path-specific effects df 

