# A Genetic Algorithm Based on Auxiliary-Individual-Directed Crossover for Internet-of-Things Applications

本文章主要是提出了遗传算法的新的交叉策略 auxiliary-individual-directed crossover（AIDX），是一种辅助个体的有向交叉算子。并在确定城市管网中的阻力系数上进行了应用。

作者的思路主要来源与KBS与Gaussian AX。

#### 1.1不等位交叉KBS：（二进制编码与实数编码均适用）

**目的**：

- 是实现了不同位上的基因交叉

**计算方式**：

对于*N*维优化问题，设父代个体$X=(X_1, X_2, …,X_N)^T$ 、$Y=(Y_1, Y_2, …,Y_N)^T$ ，子代个体为$x=(x_1, x_2,…, x_N)^T$ 、$y=(y_1, y_2,…, y_N)^T$，随机选择两个父代个体中的*N/2*个基因位（出现小数时向上取整），对每个被选中的基因位进行以下操作：
$$
\begin{equation}
\begin{aligned}
&x_{i}=X_{i} \alpha+Y_{j}(1-\alpha) \\
&y_{j}=Y_{j} \alpha+X_{i}(1-\alpha)
\end{aligned}
\end{equation}
$$
其中*i*为*X*被选中的基因位，*j*为*Y*被选中的基因位，*α*为服从均匀分布的随机数，即*α~U*(0, 0.4)。

**意义**：

1. 突破了基因信息基因位的限制，使得其他基因位上的信息都有概率传播到其他基因位上。
2. 提高了基因的多样性。

**局限性**：

1. 没有考虑不同维度上可行域可能存在差异。（这个问题可以通过对所有维度进行归一化在进行处理）
2. 对不同函数之间优化性能差异较大：当个体的不同基因位相似时，交叉子代会趋向于各个变量都相等的直线如（X=Y)

#### 1.2 AIDX

**目的**：

用辅助个体指导多维函数优化过程中子代落点控制，减少无意义探索。

**计算方式**：

1. 辅助个体的选择：
   $$
   \begin{equation}
   A=\arg \left\{\max \left[f\left(x_{1}\right), f\left(x_{2}\right), \ldots, f\left(x_{k}\right)\right]\right\}
   \end{equation}
   $$

2. 辅助个体和高斯分布的交叉：

   辅助个体会与父代中距离近的进行交叉，产生两个子代，并与两个父代高斯交叉产生的子代进行对比。

3. 在进入KBX进行交叉：防止种群过早收敛

   

