# SDE 前向扩散公式推导

本文从最基础的概率论出发，推导 score-based diffusion models 的前向扩散 SDE 公式。

---

## 1. 目标

我们希望把真实数据 \(x_0 \sim p_{\text{data}}(x)\) 逐步加噪，最终得到标准高斯分布：
\[
x(T) \sim N(0, I)
\]

每一步的操作：
1. 数据衰减（drift）
2. 加随机噪声（diffusion）

---

## 2. 确定性衰减（Drift）

设当前时间点的数据为 \(x(t)\)，向 0 衰减：
\[
x(t+\Delta t) - x(t) \approx -k x(t) \Delta t
\]

- \(k = \beta(t)/2\)，随时间可变化
- 概率论解释：这是期望值方向的变化
\[
\mathbb{E}[x(t+\Delta t)] = x(t) - k x(t) \Delta t
\]

---

## 3. 随机扰动（Diffusion）

加上随机噪声：
\[
\Delta x_{\text{stochastic}} = \sqrt{\beta(t) \Delta t} \, \epsilon, \quad \epsilon \sim N(0, I)
\]

- 对应布朗运动（Wiener process）：
\[
dw \sim N(0, dt)
\]

---

## 4. 合并漂移与随机性

离散形式：
\[
x(t+\Delta t) - x(t) = -\frac{\beta(t)}{2} x(t) \Delta t + \sqrt{\beta(t) \Delta t} \, \epsilon
\]

当 \(\Delta t \to 0\) 时，连续形式：
\[
dx = -\frac{\beta(t)}{2} x \, dt + \sqrt{\beta(t)} \, dw
\]

---

## 5. 理解每一项

1. **漂移项** \(-\frac{\beta(t)}{2} x dt\)  
   - 数据衰减，让 \(x\) 越来越接近 0  
   - 确定性方向

2. **扩散项** \(\sqrt{\beta(t)} dw\)  
   - 随机噪声  
   - 服从正态分布，方差随时间变化  
   - 布朗运动的增量特性

---

## 6. 概率论基础

1. **布朗运动（Wiener process）**：
   - \(w(0) = 0\)  
   - 增量独立且服从正态分布：
   \[
   w(t+\Delta t)-w(t) \sim N(0, \Delta t)
   \]

2. **随机微分形式**：
   \[
   dx = f(x) dt + g(x) dw
   \]

- 离散化近似：
\[
\Delta x \approx f(x) \Delta t + g(x) \sqrt{\Delta t} \, \epsilon
\]

---

## 7. 前向扩散 SDE 总结

公式：
\[
\boxed{dx = -\frac{\beta(t)}{2} x \, dt + \sqrt{\beta(t)} \, dw}
\]

- 漂移项：控制数据衰减  
- 扩散项：控制随机噪声  
- 目标：将复杂数据分布逐步变为标准高斯

