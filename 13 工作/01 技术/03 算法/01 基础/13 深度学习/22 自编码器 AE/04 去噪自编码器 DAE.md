


# 去噪自编码器 DAE

（没有明白）

去噪自编码器：

- 损坏数据作为输入，未被损坏数据作为输出的自编码器。

介绍：

- 如图：

<p align="center">
    <img width="30%" height="70%" src="http://images.iterate.site/blog/image/20190718/TRBAfaJNvPtp.png?imageslim">
</p>

- 说明：
  - 去噪自编码器代价函数的计算图。
  - $C(\tilde{\mathbf x} \mid \mathbf x)$ 为损坏过程 ，这个条件分布代表给定数据样本 $\mathbf x$ 产生损坏样本 $\tilde \mathbf x$ 的概率。
  - 去噪自编码器被训练为从损坏的版本 $\tilde \boldsymbol x$ 重构干净数据点 $\boldsymbol x$。
  - 这可以通过最小化损失 $L = - \log p_{\text{decoder}} (\boldsymbol x \mid \boldsymbol h = f(\tilde \boldsymbol x))$ 实现
    - 其中 $\tilde \boldsymbol x$ 是样本 $\boldsymbol x$ 经过损坏过程 $C (\tilde \boldsymbol x \mid \boldsymbol x)$ 后得到的损坏版本。
    - 通常， 分布 $p_{\text{decoder}}$ 是因子的分布（平均参数由前馈网络 $g$ 给出）。（什么是因子分布？）

训练：

- 自编码器则根据以下过程，从训练数据对 $(\boldsymbol x, \tilde \boldsymbol x)$ 中学习**重构分布**(reconstruction distribution) $p_{\text{reconstruct}} (\mathbf x \mid \tilde \mathbf x)$：
  - 从训练数据中采一个训练样本 $\boldsymbol x$。
  - 从 $C(\tilde{\mathbf x} \mid \mathbf x=\boldsymbol x)$ 采一个损坏样本 $\tilde \boldsymbol x$。
  - 将 $(\boldsymbol x, \tilde \boldsymbol x)$ 作为训练样本来估计自编码器的重构分布 
    $$p_{\text{reconstruct}} (\boldsymbol x \mid \tilde \boldsymbol x) = p_{\text{decoder}}(\boldsymbol x \mid\boldsymbol h)$$
    
    - 其中
      - $\boldsymbol h$ 是编码器 $f(\tilde \boldsymbol x)$ 的输出
      - $p_{\text{decoder}}$ 根据解码函数 $g(\boldsymbol h)$ 定义。

- 通常我们可以简单地对负对数似然 $-\log p_{\text{decoder}} (\boldsymbol x \mid \boldsymbol h)$ 进行基于梯度法（如小批量梯度下降）的近似最小化。只要编码器是确定性的，去噪自编码器就是一个前馈网络，并且可以使用与其他前馈网络完全相同的方式进行训练。

理解：

- 可以认为 DAE 是在以下期望下进行随机梯度下降：

    $$\begin{aligned}
    - \mathbb E_{\mathbf x \sim \hat{p}_{\text{data}}(\mathbf x)} \mathbb E_{\tilde{\mathbf x} \sim C(\tilde{\mathbf x}\mid\boldsymbol x)} \log p_{\text{decoder}}(\boldsymbol x \mid \boldsymbol h = f(\tilde{\boldsymbol x})),
    \end{aligned}$$

- 其中
  - $\hat{p}_{\text{data}}(\boldsymbol x)$ 是训练数据的分布。



## 得分估计

得分匹配：

- 得分匹配是最大似然的代替。
- 它提供了概率分布的一致估计，促使模型在各个数据点 $\boldsymbol x$ 上获得与数据分布相同的得分。在这种情况下，得分是一个特定的梯度场：


$$\begin{aligned}
 \nabla_{\boldsymbol x} \log p(\boldsymbol x) .
\end{aligned}$$

对于自编码器：

- 可以认为学习 $\log p_{\text{data}}$ 的梯度场就是学习 $p_{\text{data}}$ 结构的一种方式。
- DAE 的训练准则（条件高斯 $p(\boldsymbol x \mid \boldsymbol h)$）能让自编码器学到能估计数据分布得分的向量场 $(g(f(\boldsymbol x))-\boldsymbol x)$ ，这是 DAE 的一个重要特性。
- 如图：



<p align="center">
    <img width="70%" height="70%" src="http://images.iterate.site/blog/image/20190718/Yjig5xixzupc.png?imageslim">
</p>

- 说明：
  - 去噪自编码器被训练为将损坏的数据点 $\tilde \boldsymbol x$ 映射回原始数据点 $\boldsymbol x$。
  - 我们将训练样本 $\boldsymbol x$ 表示为位于低维流形（粗黑线）附近的红叉。
  - 我们用灰色圆圈表示等概率的损坏过程 $C(\tilde \boldsymbol x \mid \boldsymbol x)$。灰色箭头演示了如何将一个训练样本转换为经过此损坏过程的样本。
  - 当训练去噪自编码器最小化平方误差 $\| g(f(\tilde \boldsymbol x)) - \boldsymbol x \|^2$ 的平均值时，重构 $g(f(\tilde \boldsymbol x))$ 估计 $\mathbb E_{\mathbf x, \tilde{\mathbf x} \sim p_{\text{data}}(\mathbf x) C(\tilde{\mathbf x} \mid \mathbf x)}[\mathbf x \mid \tilde{\boldsymbol x}]$。$g(f(\tilde \boldsymbol x))$ 对可能产生 $\tilde \boldsymbol x$ 的原始点 $\boldsymbol x$ 的质心进行估计，所以向量 $g(f(\tilde \boldsymbol x)) - \tilde \boldsymbol x$ 近似指向流形上最近的点。
  - 因此自编码器可以学习由绿色箭头表示的向量场 $g(f( \boldsymbol x)) -  \boldsymbol x$。该向量场将得分 $\nabla_{\boldsymbol x} \log p_{\text{data}}(\boldsymbol x)$ 估计为一个乘性因子，即重构误差均方根的平均。

理解：

- 对一类采用高斯噪声和均方误差作为重构误差的特定去噪自编码器（具有 sigmoid 隐藏单元和线性重构单元）的去噪训练过程，与训练一类特定的被称为 RBM 的无向概率模型是等价的。
- 这类模型将在 Gaussian-Bernoulli RBM 给出更详细的介绍；
- 对于现在的讨论，我们只需知道这个模型能显式的给出 $p_{\text{model}}(\boldsymbol x; \boldsymbol \theta)$。
- 当 RBM 使用去噪得分匹配算法 训练时，它的学习算法与训练对应的去噪自编码器是等价的。
  - 在一个确定的噪声水平下，正则化的得分匹配不是一致估计量；相反它会恢复分布的一个模糊版本。
  - 然而，当噪声水平趋向于 0 且训练样本数趋向于无穷时，一致性就会恢复。我们将会在 去噪得分匹配 中更详细地讨论去噪得分匹配。
- 自编码器和 RBM 还存在其他联系。在 RBM 上应用得分匹配后，其代价函数将等价于重构误差结合类似 CAE 惩罚的正则项。自编码器的梯度是对 RBM 对比散度训练的近似。
- 对于连续的 $\boldsymbol x$，高斯损坏和重构分布的去噪准则得到的得分估计适用于一般编码器和解码器的参数化。这意味着一个使用平方误差准则
    $$\begin{aligned}
    \| g(f(\tilde \boldsymbol x)) - \boldsymbol x \|^2
    \end{aligned}$$
- 和噪声方差为 $\sigma^2$ 的损坏
    $$\begin{aligned}
    C(\tilde x = \tilde \boldsymbol x \mid \boldsymbol x) = N(\tilde \boldsymbol x; \mu=\boldsymbol x, \Sigma = \sigma^2 I)
    \end{aligned}$$
- 的通用编码器-解码器架构可以用来训练估计得分。
- 下图展示其中的工作原理。
    <p align="center">
        <img width="70%" height="70%" src="http://images.iterate.site/blog/image/20190718/hIh8UihJMU72.png?imageslim">
    </p>
- 说明：
  - 由去噪自编码器围绕 $1$ 维弯曲流形学习的向量场，其中数据集中在 $2$ 维空间中。
  - 每个箭头与重构向量减去自编码器的输入向量后的向量成比例，并且根据隐式估计的概率分布指向较高的概率。
  - 向量场在估计的密度函数的最大值处（在数据流形上）和密度函数的最小值处都为零。
  - 例如，螺旋臂形成局部最大值彼此连接的 $1$ 维流形。局部最小值出现在两个臂间隙的中间附近。
  - 当重构误差的范数（由箭头的长度示出）很大时，在箭头的方向上移动可以显著增加概率，并且在低概率的地方大多也是如此。自编码器将这些低概率点映射到较高的概率重构。在概率最大的情况下，重构变得更准确，因此箭头会收缩。

发展：

- 一般情况下，不能保证重构函数 $g(f(\boldsymbol x))$ 减去输入 $\boldsymbol x$ 后对应于某个函数的梯度，更不用说得分 。这是早期工作 专用于特定参数化的原因（其中 $g(f(\boldsymbol x)) - \boldsymbol x$ 能通过另一个函数的导数获得）。通过标识一类特殊的浅层自编码器家族，使 $g(f(\boldsymbol x)) - \boldsymbol x$ 对应于这个家族所有成员的一个得分，以此推广。
- 目前为止我们所讨论的仅限于去噪自编码器如何学习表示一个概率分布。更一般的，我们可能希望使用自编码器作为生成模型，并从其分布中进行采样。这将在 从自编码器采样 中讨论。


