## 统计学习概论

### 方法

* 监督学习（supervised learning）
* 非监督学习（unsupervised learning）
* 半监督学习（semi-supervised learning）
* 强化学习（reinforcement learning）

### 步骤

1. 得到一个有限的训练数据集合；
2. 确定包含所有可能的模型的假设空间，即学习模型的集合；
3. 确定模型选择的准则，即学习的策略，
4. 实现求解最优模型的算法，即学习的算法；
5. 通过学习方法选择最优模型；
6. 利用学习的最优模型对新数据进行预测或分析。

### 三要素

1. 模型（model）
2. 策略（strategy）
3. 算法（algorithm）

方法 = 模型 + 策略 + 算法

#### 模型

* 决策函数的集合：$$ \mathcal{F}=\{f|Y=f(X)\} $$，参数空间：$$ \mathcal{F}=\{f|Y=f_\theta(X),\theta\in{\mathbf{R}^n}\} $$
* 条件概率的集合：$$ \mathcal{F}=\{P|P(Y|X)\} $$，参数空间：$$ \mathcal{F}=\{f|P_\theta(Y|X),\theta\in{\mathbf{R}^n}\} $$

#### 策略

* 损失函数：一次预测的好坏
* 风险函数：平均意义下模型预测的好坏

##### 常用的损失函数

1. 0-1损失函数：