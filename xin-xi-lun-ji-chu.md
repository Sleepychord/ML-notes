# 信息论基础

## Kraft-McMillan theorem
该定律是讲如果我们要给S个事件指定唯一的可解码编码，设字符集大小为r，那么存在这S个事件编码的长度分别为$${l_1,l_2,...,l_n}$$的充分必要条件是$$\sum r^{l_i}\leq 1$$。
证明很直观，考虑编码树每次有r个分支即可。
## 熵
考虑随机事件S有概率分布p，用最优编码来对事件编码，最理想情况（非整幂次TODO）长度为$$-log_r \ p_i$$。不妨设r=2，那么描述事件用的消息长度期望、得知随机事件结果的价值、通信传输的期望长度……就是
$$I(S)=\mathbb{E}(-\log  p_i)=-\sum p_i\log p_i$$
## 交叉熵（cross-entropy）
假设我们认为随机事件S的概率分布是q，但实际上是p。我们按照q给事件设计了最优编码，那么消息的期望长度$$H(p,q)=\sum -p_i\log q_i$$。
经常用来作为loss function，以0、1分布为例，我们预测的概率为$$y'$$，label为$$y$$（通常为0、1）。
$$loss = -\sum p_i\log q_i = -(p_0\log q_0 + p_1\log q_1) = -[(1-y)\log (1-y') + y\log y']$$
## KL divergence（相对熵）
$$D_{KL}(p||q)=\sum\limits_i p_i \log \frac{p_i}{q_i} = H(p,q) - H(p)$$
是一种使用真实分布p代替分布q的信息增益（实际上减少浪费）。经常用来度量分布的相似性。
由于真实分布p往往固定，优化KL散度和优化交叉熵等价。
$$D_{KL} \geq 0$$
可用琴生不等式证明：对于(下)凸函数
$$f(\sum a_ix_i)\leq \sum a_if(x_i), \sum a_i = 1$$
令$$x_i = \frac{q_i}{p_i}$$，上式$$\geq 0$$
## 条件熵
设随机变量X、Y之间有关联，那么条件熵即$$H(X|Y) = \mathbb{E}_{y\sim p_Y}H(X|y)=\sum\limits_{x,y}p_{x,y}\log \frac{p_{x,y}}{p_y}$$
## 互信息
$$I(X;Y) = H(X) - H(X|Y)$$ 是知道Y后的信息增益。如果XY独立，那么I(X;Y)为0。如果存在Y到X的函数，那么将得到最大值H(x)。
###  Variational Information Maximization
计算互信息中条件熵需要后验分布，这点是及其困难的，虽然我们通过蒙特卡洛方法可以采样，但是内层的概率很难估计导致结果不准确。
由于上面分析过$$D_{KL}\geq 0$$我们可以使用任意一种自己模型分布q使用H(p,q)代替H(p)得到一个上界。条件熵的内层可以同样处理。
$$H(X|Y) \leq \mathbb{E}_{y\sim Y}[\mathbb{E}_{x\sim p(x|y)}-\log q(x|y)]$$