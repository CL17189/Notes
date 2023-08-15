# 后验分布

## 信息
+ 总体
+ 样本
+ 先验

## 后验分布公式

+ 记号
> 样本信息：$P(X|\theta)=\prod_{i=1}^{n} P(x_i|\theta)$
> 先验分布：$\pi(\theta)$
> 后验分布：$\pi(\theta|X)$

+ 后验概率公式
$$\pi(\theta | X) = \frac{P(X | \theta) \cdot \pi(\theta)}{\int P(X | \theta) \cdot \pi(\theta) \, d\theta}$$
->因此是三种信息的综合
->eg：后验分布是结合样本对先验分布的“调整”

## 共轭先验分布
+ 共轭先验分布：指当似然函数与先验分布属于某个特定的概率分布族时，后验分布也属于该分布族。
+ “核” 方法
+ 优点
+ ![[Pasted image 20230721173630.png]]

## 超参数确定

+ 先验矩和先验分位数确定


## 多个参数的情况
+ 首先获得

$$\pi(\theta_1,\theta_2 | X) \propto P(X | \theta_1,\theta_2) \cdot \pi(\theta_1,\theta_2) $$
+ 之后有$$\pi(\theta_1 | X) = \int \pi(X | \theta1,\theta_2)  d\theta_2 $$
## 充分统计量
在贝叶斯统计中，充分统计量和经典统计中的充分统计量概念基本平行。并且依据因子分解定理，有类似的结论
+ TH：设X是来自密度函数$p(x|\theta)$的一个样本,T=T(x)是统计量，它的密度函数为 $p(t|\theta)$,又设H是$\theta$的某个先验分布族，则T(x)为$\theta$的充分统计量的充要条件是对任一先验分布$\pi(\theta)$有 $\pi(\theta | T(X))=\pi(\theta | X)$ 

即用样本分布 p(x)算得的后验分作与统计T(x)算得的后验分布是相同的。

# Bayes 推断
## 思想：条件方法
+ 观点：只考虑已出现的数据（样本），认为未出现的数据和推断无关

## Bayes估计
### 估计量
+ 三种估计量：对于后验分布$\pi(\theta|X)$，取满足不同条件的$\hat{\theta}$.最大后验估计、后验中位数估计、后验期望估计
+ 二项分布场合的两个结论
	+ 最大后验估计和极大似然估计等价
		+ 缺点：未证明贝叶斯估计和经典估计一一对应
	+ 后验期望估计比最大后验估计更有解释性
### 误差
+ 后验均方差：$MSE(\hat{\theta}|X)=E[\hat{\theta}-\theta]^2$
+ 平方和分解公式：$MSE(\hat{\theta}|X)= Var(\theta|X)+(\hat{\theta}-\theta)^2$

## 区间估计
### 可信区间
+ 贝叶斯统计中的可信区间定义为后验分布的一个区间， 一个置信度为 $(1 - \alpha)$ 的可信区间可以表示为 $[\theta_{\text{下界}}, \theta_{\text{上界}}]$，其中 $\theta_{\text{下界}}$ 和 $\theta_{\text{上界}}$
 是使得： $$ \int_{\theta_{\text{下界}}}^{\theta_{\text{上界}}} P(\theta | X) \, d\theta = 1 - \alpha $$成立的 $\theta$ 值。换句话说，$\theta_{\text{下界}}$ 和 $\theta_{\text{上界}}$ 是后验分布累积概率分别为 $\frac{\alpha}{2}$ 和 $1 - \frac{\alpha}{2}$ 的两个值。
### HPD可信区间
+ 给定$\pi(\theta|X),1 - \alpha$,对于直线上的子集C满足：$P(C|X)=1 - \alpha$ ; 任给的$\theta_1 \in C , \theta_2 \notin C$  , $\pi(\theta_1|X)>\pi(\theta_2|X)$
+ 这个定义是针对多峰的密度函数
## 假设检验
+ 后验概率：$\alpha_i=P(\theta_i|X)$
+ 先验概率：$\pi_i=\pi(\theta_i)$
+ 计算$\alpha_0/\alpha_1$
### 后验机会比
+ $\alpha_0/\alpha_1$
### Bayes因子
+ $B^{\pi}(x)=\frac{\alpha_0/\alpha_1}{\pi_0/\pi_1}$
### 三种假设检验的场合
+ 简单：参数空间退化为一个点
+ 简单假设对简单假设
	+ 后验机会比和$B^{\pi}(x)$ 均为显式，$B^{\pi}(x)$只和似然比有关
+ 简单假设对复杂假设
	+ 特殊处理：在H0的地方给一个正概率$\pi_0$,使得$\pi(\theta)=\pi_0I_{\theta_0}(\theta)+\pi_1g_1(\theta)$
+ 复杂假设对复杂假设
	+  后验机会比和$B^{\pi}(x)$ 均为显式积分形式
	+ 这时候极大似然估计是贝叶斯因子的特殊情况


## 预测
+ 预测方法
	+ 先验预测分布：m(x)
	+ 后验预测分布：$m(z|X)=\int _{\theta} g(z|\theta)\pi(\theta|x)d\theta$

## 似然原理
### 似然原理
+1.有了观测值x之后,在做关于$\theta$的推断和决策时，所有与试验有关的信息均被包含在似然函数 L($\theta$)之中。
2.如果有两个似然函数是成比例的，比例常数与$\theta$ 无关，则它们关于$\theta$含有相同的信息。
### 例子


# 确定先验分布
## 主观概率方法
+ 经验
+ 修正
+ 满足概率公理

## 利用先验信息
前提：历史经验数据足够多

+ 直方图
+ 选定先验分布形式再估计超参数
+ 定分度法和变分度法


## 利用边缘分布
### 边缘分布
+形式 $m(x)=\int_{\theta} p(x|\theta)\pi(\theta)d\theta$
### 混合分布
+ 从不同总体中取出来的东西汇集成一个新的总体
### ML-II
+ 对于已有观察值找$m^{\pi_1}(x)>m^{\pi_2}(x)$
### 矩方法
+ 当先验密度函数的形式已知，要估计超参数


## 无信息先验分布
### Bayes假设
+ 无信息是指除了参数的范围和参数在总体分布的地位之外，再也不包含任何关于这个参数的信息
+ 换句话说就是对这个参数在其范围内均等的无知，没有偏好
+ 因此会采用均匀分布
+ 无穷的情况
	+ 广义先验密度
+ 不满足变换下的不变性
### 两种参数的无信息先验
#### 位置参数
+ 位置参数 $p(x-\theta)$
+ 结论：$\pi(\theta)=1$
#### 尺度参数
+ 位置参数 $\frac{1}{\sigma}p(\frac{x}{\sigma})$
+ 结论：$\pi(c)=\pi(1)/c$

### Fisher信息阵确定

## 多层先验
+ 参数的参数的...
+ 多个参数的情况
	+ 等高线法



# 决策：收益、损失、效用
+ 决策和推断侧差别在于是否涉及后果
## 决策问题三要素
+ 状态集
+ 行动集
+ 收益函数
## 决策准则
+ 行动$a_i$是容许的，当不存在
	+ 1. $Q(a_j,\theta)>Q(a_i,\theta)$
	+ 2. 至少有一个$\theta$让上式严格成立
### 决策准则
+ 悲观准则
	+ $\max_a \min_{\theta} Q(a,\theta)$ 
+ 乐观准则
	+ $\max_a \max_{\theta} Q(a,\theta)$ 
+ 折中准则
	+  $\max_a [\alpha \max_{\theta} Q(a,\theta)+(1-\alpha)\min_{\theta} Q(a,\theta)],\alpha \in (0,1)$ 
## 先验期望准则
+ 已知$\pi(\theta)$
+ $E_{\theta}Q(a,\theta),Var_{\theta}Q(a,\theta)$
+ 先验期望准则下最优

性质：
+ 1. 收益函数的线性变换不改变先验期望准则下最优
+ 2. 状态空间侧某个子集上收益函数的加上一个常数不改变先验期望准则下最优

## 损失函数
+ 是定义在行动集合A和状态集合$\Theta$ 的笛卡尔积 $A * \Theta$ 中的二元函数$L(a,\theta)$
+  损失函数的类型
	+ 平方
	+ 线性
	+ ...
### 损失函数下的准则
+ 悲观准则
	+ $\min_{\theta}\max_a  L(a,\theta)$ 
+ 先验期望准则
	+ $E_{\theta}L(a,\theta),Var_{\theta}L(a,\theta)$
## 效用函数
+ U(m),m是收益


# Bayes决策
## Bayes决策问题
### 问题分类
+ 仅先验信息——无数据决策（上一章节讨论）
+ 仅抽样信息——统计决策
+ 先验+抽样——Bayes决策（本章）
### Bayes决策问题
+ 已知随机变量X，密度函数$p(x|\theta)$,参数空间$\Theta$
+ 先验分布 $\pi(\theta)$
+ 行动集A
+ 损失函数

## 后验风险准则
### 后验风险
+ $R(a|x)=E^{\theta|x}[L(\theta,a)]$
### 决策函数
+ 样本空间X到行动空间A的映射$\delta(x)$
### 后验风险准则
+  $R(\delta(x)|x)=E^{\theta|x}[L(\theta,\delta(x))]$
+ 贝叶斯解：$\delta(x)=augminR(\delta(x)|x)$
+ 贝叶斯估计
### 常用损失函数下的贝叶斯估计
#### 平方损失函数
+ 平方损失函数：$L(\theta,\delta)=(\theta-\delta)^2$
	+ 加权平方损失函数：$L(\theta,\delta)=\lambda(\theta)(\theta-\delta)^2$
+ 结论：平方损失函数$\theta$的贝叶斯估计是后验均值$\delta(x)=E(\theta|x)$
+ 推广1：加权平方损失函数$\theta$的贝叶斯估计是$\delta(x)=E(\frac{\lambda(\theta)\theta|x}{\lambda(\theta)|x})$
+ 推广2：多元成立（均值向量）
#### 线性损失函数
+ 绝对值线性损失函数：$L(\theta,\delta)=|\theta-\delta|$
+ 线性损失函数：$L(\theta,\delta)=k_i|\theta-\delta|$
+ 结论：绝对值线性损失函数$\theta$的贝叶斯估计是后验分布的中位数$\pi_{0.5}(\theta|x)$
+ 推广1：线性损失函数$\theta$的贝叶斯估计是$\frac{k_0}{k_0+k_1}分位数$



## 抽样信息期望值
+ 对抽样的费用和获得信息带来的收益的衡量

### 完全理想信息期望值（理想状态）
+ EVPI：完全信息带给决策者的收益（平均意义下）
+  $EVPI=E[max(Q(\theta_i,a_j)]-max(E[Q(\theta,a)]$
+ 先验EVPI：在先验最优行动准则$a'$下，$E^{\theta}[L(\theta,a')]$
### 抽样信息期望值
+ 后验EVPI: 用后验分布确定贝叶斯函数$\delta(x)$, $E^{\theta|X}[L(\theta,\delta(x))]$
+ 后验EVPI期望（应对没有样本X的情况）$E^XE^{\theta|X}[L(\theta,\delta(x))]$
+ EVSI=先验EVPI-后验EVPI
### 样本量确定
+ 抽样净益：ENGS(n)=EVSI-C(n)
	+ $C(n)=C_f+C_v*n$表示抽样成本
+ 最佳样本量
	+ $n*=augmax ENGS$
+ 样本量上界
	+ C(n*)<=EVSN(n*)<=先验EVPI ，第一个等号取等
### 两行动线性决策的EVPI
+ ...

# 统计决策理论
## 统计决策问题
+ 风险函数
	+ 在仅适用抽样信息的统计决策问题中，风险函数定义为$R(\delta(x),\theta)=E^{\theta|x}[L(\theta,\delta(x))]$
	+ 和Bayes统计的不同在于不是后验的
+ 决策函数的最优性
	+ def:一致最小风险决策函数:在参数空间$\Theta$上，决策函数类D中存在$\delta(x)*$,对任意的$\delta(x)$总有：$R(\theta,\delta*)<=R(\theta,\delta),\forall \theta$ 
+ 点估计
	+ 利用样本得到的参数估计$\hat{\theta(X)}$ 就是一个决策函数
	+ 可以由此计算损失
		+ UMVUE
		+ Pitan意义下的最优估计
+ 区间估计
	+ 决策函数是一个区间函数$\delta(x)=(d_1(x),d_2(x))$
	+ 行动集合A是直线上的有界区间类
+ 假设检验
	+ 行动集只包含接受和拒绝
	+ 损失函数经常采取0-1损失函数
		+ 此时风险函数中如果$\theta \in \theta_0$,风险函数是第一类错误概率
		+ 如果$\theta \in \theta_1$,风险函数是第二类错误概率
	+ Neyman-Pearson



## 容许性
+ 原因：一致最优很难达到，因此降低要求
+ 容许性：
	+ 非容许：有统计决策问题和决策函数类D，决策函数$\delta_1(x)$是非容许的，当：存在$\delta_2(x)$满足
		+ $R(\theta,\delta_2)<=R(\theta,\delta_1), \forall \theta$
		+ 在$\Theta 中至少存在一个\theta_0,R(\theta,\delta_2)<R(\theta,\delta_1)$
+ 和一致最优比，它放松了‘所有情况’
### Stein邻近
+ 多元二次损失函数下，p>=3时，多元正态分布的样本均值向量是正态均值向量的非容许估计

## Minimax

### Minimax准则
+ $\widetilde{R}=Min_{a \in A}Max_{\theta \in \Theta}R(\theta,\delta)$ 

### Minimax准则的容许性
+ Th1: 在一个统计决策问题中，参数的最大最小估计存在且唯一，那么它就是容许的
+ Th2: 在一个统计决策问题中，参数的容许估计是最大最小估计的条件是估计在参数空间上的风险是常数


## Bayes风险
### Bayes风险准则
+ 贝叶斯风险：风险函数对先验分布的期望：$R(\delta)=E^{\theta}[R(\theta,\delta)]$
+ 贝叶斯风险准则下最优决策函数：找到最小
### 和后验风险的等价性
+ Th：如果贝叶斯风险是有限的，那么贝叶斯风险准则和后验风险准则等价
（R和E[R]上面时候等价)



## Bayee估计的性质
+ 讨论其的容许性以及和Minimax的关系
	+ 容许性：参数的贝叶斯估计的风险函数是关于参数连续的，并且参数的贝叶斯有限，那么参数的贝叶斯估计是容许的
	+ 最大最小：参数的贝叶斯估计的风险函数是常数，则参数的贝叶斯估计是最大最小估计
		+ 结论1：在给定的贝叶斯决策问题中，设$\{\pi_k,k>=1\}$是$\Theta$的一个先验分布列,相应的贝叶斯估计列和贝叶斯风险列分别记为$\{\delta_k\},\{R(delta_k)\}$。假如$\delta_0$是$\theta$的一个估计,，且它的风险函数满足$Max_{\theta \in \Theta}R(\theta,\delta_0)<=lim_{k->\infty}R(\delta_k(x)$,则$\delta_0$是$\theta$的最小最大估计。
		+ 结论2：在给定的贝叶斯决策问题中,若$\theta$的一个估计$\delta_0$的风险函数 $R(\theta,\delta_0)$在$\Theta$ 上为常数c ,且存在一个先验分布列$\{\pi_k,k>=1\}$，使得相应贝叶斯估计 $\delta_k$的贝叶斯风险满足$lim_{k->\infty}R_k(\delta_k(x)$，则$\delta_0$是$\theta$的最小最大估计。