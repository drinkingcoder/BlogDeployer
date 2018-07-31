# Optimizer

## Basic Knowledge

### Optimization Framework

Definiton:

​	$w$ - parameters

​	$f(w)$ - objective function

​	$\alpha$ - initial learning rate

Pipeline:

​	1.	Figure out gradients: $g_t=\nabla f(w_t)$

​	2.	Figure out first-order and second-order momentum:
$$
m_t=\phi(g_1,g_2,g_3,...,g_t)\\
V_t=\psi(g_1,g_2,g_3,...,g_t)
$$
​	3.	Figure out descent gradients:$\eta_t=\alpha m_t/\sqrt{V_t}$

​	4.	Update parameters: $w_{t+1}=w_t-\eta_t$

2. ​

##SGD

1. Fitting the framework: $m_t=g_t;V_t=I$, so$\eta_t=\alpha g_t$
2. batch size对这个的影响

##Adam

1. 自适应，基本算法
2. 坏处

##AdamW

1. weight decay的改进

## 调参手法

1. batch size越小，variance越大，越难收敛；反过来batch size越大，越容易overfit，且掉到鞍点or局部最优上出不来。一般来说如果SGD，开始的时候batch size小一点，后面大一点会比较好。
2. 用Adam时别上L2正则，直接上weight decay
3. ​

## Reference

[1]Dominic Masters, Carlo Luschi,[Revisiting Small Batch Training for Deep Neural Networks](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1804.07612), arXiv:1804.07612v1