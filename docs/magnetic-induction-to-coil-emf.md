# 从磁感应强度到整个线圈感应电动势的推导

> 文件编码：UTF-8  
> 适用场景：无线充电线圈原理学习、参数估算、仿真结果校验  
> 说明：本文推导的是 RX 线圈内部产生的感应电动势，不等同于整流后的 5V 输出电压。

---

## 1. 总体推理链

从 TX 电流到 RX 整个线圈感应电动势，完整过程为：

```text
TX 线圈电流 i_TX(t)
        |
        v
空间磁感应强度 B(r,t)
        |
        v
RX 每一匝的磁通量 Phi_k(t)
        |
        v
RX 整个线圈的磁链 lambda(t)
        |
        v
RX 整个线圈的感应电动势 e_RX(t)
```

数学表达为：

$$
i_{TX}(t)
\rightarrow
\vec B(\vec r,t)
\rightarrow
\Phi_k(t)
\rightarrow
\lambda(t)
\rightarrow
e_{RX}(t)
$$

最一般的最终公式是：

$$
\boxed{
e_{RX}(t)=
-\frac{d}{dt}
\left[
\sum_{k=1}^{N_{RX}}
\int_{S_k}
\vec B(\vec r,t)\cdot d\vec S
\right]
}
$$

---

## 2. 第一步：由 TX 电流计算磁感应强度

### 2.1 单匝圆形线圈轴线上的磁场

对于半径为 $R$ 的单匝圆形线圈，线圈中流过电流 $i(t)$，在线圈中心轴线上、距离线圈平面 $z$ 处，磁感应强度为：

$$
B_z(z,t)=
\frac{\mu_0 i(t)R^2}
{2(R^2+z^2)^{3/2}}
$$

其中：

- $B_z$：轴向磁感应强度，单位 T；
- $\mu_0=4\pi\times10^{-7}\ \mathrm{H/m}$：真空磁导率；
- $i(t)$：TX 线圈的瞬时电流；
- $R$：TX 线圈半径；
- $z$：观察点到 TX 线圈平面的轴向距离。

当观察点位于线圈中心，即 $z=0$ 时：

$$
B(0,t)=\frac{\mu_0 i(t)}{2R}
$$

### 2.2 多匝、等半径 TX 线圈

如果 TX 线圈有 $N_{TX}$ 匝，并且每一匝半径近似相同，则各匝产生的磁场叠加：

$$
B_z(z,t)=
\frac{\mu_0 N_{TX}i_{TX}(t)R_{TX}^2}
{2(R_{TX}^2+z^2)^{3/2}}
$$

这里已经包含 TX 匝数 $N_{TX}$。后续计算 RX 电动势时，只需再考虑 RX 匝数，不能重复乘以 TX 匝数。

### 2.3 平面螺旋 TX 线圈

实际无线充电线圈通常是平面螺旋线圈，每一匝半径不同：

```text
R1 < R2 < R3 < ... < RN
```

在中心轴线上，可以将每一匝产生的磁场相加：

$$
B_z(z,t)=
\sum_{m=1}^{N_{TX}}
\frac{\mu_0 i_{TX}(t)R_m^2}
{2(R_m^2+z^2)^{3/2}}
$$

其中 $R_m$ 是 TX 第 $m$ 匝的半径。

该公式仍然只描述中心轴线上的磁场。RX 线圈具有一定面积，因此后续还需要对整个 RX 面积积分。

---

## 3. 第二步：计算 RX 一匝线圈的磁通量

磁通量定义为：

$$
\boxed{
\Phi=\int_S\vec B\cdot d\vec S
}
$$

也可以写成：

$$
\Phi=\int_S B_n\,dS
$$

其中：

- $\Phi$：穿过一匝线圈的磁通量，单位 Wb；
- $S$：这一匝线圈围成的面积；
- $B_n$：垂直于线圈平面的磁感应强度分量。

只有穿过线圈平面的法向磁场分量才能形成有效磁通。

### 3.1 均匀磁场近似

如果磁场在整个 RX 线圈面积内近似均匀，磁场方向与线圈平面法线之间的夹角为 $\theta$，则：

$$
\Phi(t)=B(t)A\cos\theta
$$

其中 $A$ 是一匝 RX 线圈包围的面积。

当 TX 与 RX 完全平行且对准时：

$$
\theta=0
$$

因此：

$$
\Phi(t)=B(t)A
$$

如果 RX 是半径为 $R_{RX}$ 的圆形线圈：

$$
A=\pi R_{RX}^2
$$

所以：

$$
\Phi(t)=B(t)\pi R_{RX}^2
$$

### 3.2 RX 线圈倾斜

如果 RX 线圈相对磁场倾斜，则：

$$
\Phi(t)=B(t)A\cos\theta
$$

在理想均匀场模型下：

| 倾斜角 $\theta$ | $\cos\theta$ | 剩余磁通比例 |
|---:|---:|---:|
| $0^\circ$ | 1.000 | 100% |
| $30^\circ$ | 0.866 | 86.6% |
| $60^\circ$ | 0.500 | 50% |
| $90^\circ$ | 0 | 0% |

实际无线充电磁场并不均匀，因此倾斜损失不能只用 $\cos\theta$ 精确描述。

### 3.3 非均匀磁场

如果磁场在 RX 面积内不均匀，应执行面积积分。

对于轴对称、同轴线圈：

$$
\Phi(t)=
\int_0^{R_{RX}}
B_z(\rho,z,t)\,2\pi\rho\,d\rho
$$

其中：

- $\rho$：RX 平面内到中心的径向距离；
- $B_z(\rho,z,t)$：该位置的轴向磁感应强度；
- $2\pi\rho\,d\rho$：一个微小圆环的面积。

离散计算时，可以把 RX 面积分成很多小区域：

$$
\Phi(t)\approx
\sum_{j=1}^{n}
B_{n,j}(t)\Delta S_j
$$

当存在横向偏移、磁铁、铁氧体、电池或复杂外壳时，通常采用以下方法：

1. 有限元仿真；
2. 数值积分；
3. 直接测量互感。

---

## 4. 第三步：由每匝磁通计算整个 RX 线圈的磁链

### 4.1 磁通与磁链的区别

一匝线圈中的磁通量为：

$$
\Phi
$$

多匝线圈的总磁链为：

$$
\boxed{
\lambda(t)=
\sum_{k=1}^{N_{RX}}
\Phi_k(t)
}
$$

其中：

- $\lambda$：整个 RX 线圈的磁链，单位 Wb·turn；
- $\Phi_k$：穿过 RX 第 $k$ 匝的磁通量；
- $N_{RX}$：RX 线圈匝数。

### 4.2 每匝磁通近似相同

如果每一匝：

- 面积相近；
- 位置相近；
- 穿过的磁场近似相同；

则可以近似认为：

$$
\Phi_1=\Phi_2=\cdots=\Phi_{N_{RX}}=\Phi
$$

于是：

$$
\lambda=N_{RX}\Phi
$$

代入均匀磁场公式：

$$
\lambda(t)=N_{RX}B(t)A\cos\theta
$$

### 4.3 平面螺旋 RX 线圈

平面螺旋线圈中每一匝的面积不同：

$$
A_1\ne A_2\ne\cdots\ne A_{N_{RX}}
$$

因此，每一匝应分别计算：

$$
\Phi_k(t)=
\int_{S_k}
\vec B(\vec r,t)\cdot d\vec S
$$

整个线圈的精确磁链为：

$$
\boxed{
\lambda(t)=
\sum_{k=1}^{N_{RX}}
\int_{S_k}
\vec B(\vec r,t)\cdot d\vec S
}
$$

---

## 5. 第四步：由磁链变化计算整个线圈的电动势

根据法拉第电磁感应定律：

$$
\boxed{
e(t)=-\frac{d\lambda(t)}{dt}
}
$$

代入多匝线圈的磁链：

$$
\boxed{
e(t)=
-\frac{d}{dt}
\left[
\sum_{k=1}^{N_{RX}}
\int_{S_k}
\vec B(\vec r,t)\cdot d\vec S
\right]
}
$$

负号来自楞次定律，表示感应电流产生的磁场总是阻碍原磁通的变化。

如果只计算电动势大小，可以写成：

$$
|e(t)|=
\left|\frac{d\lambda(t)}{dt}\right|
$$

### 5.1 均匀磁场、线圈位置固定

如果：

- RX 线圈不移动；
- 面积不变化；
- 角度不变化；
- 磁场在 RX 面积内均匀；

则：

$$
\lambda(t)=N_{RX}AB(t)\cos\theta
$$

因此：

$$
\boxed{
e(t)=
-N_{RX}A\cos\theta
\frac{dB(t)}{dt}
}
$$

当 TX 与 RX 完全对准时：

$$
\boxed{
e(t)=
-N_{RX}A\frac{dB(t)}{dt}
}
$$

这说明感应电动势取决于磁感应强度的变化速度，而不只是磁感应强度的大小。

如果磁场恒定：

$$
\frac{dB}{dt}=0
$$

则：

$$
e=0
$$

因此，静止的永久磁铁不能持续向 RX 线圈传输电能。

---

## 6. 正弦磁场下的电动势

假设 TX 在 RX 位置产生的磁场近似为正弦波：

$$
B(t)=\hat B\sin(\omega t)
$$

其中：

- $\hat B$：磁感应强度峰值；
- $\omega=2\pi f$：角频率；
- $f$：工作频率。

对时间求导：

$$
\frac{dB}{dt}=
\omega\hat B\cos(\omega t)
$$

代入法拉第定律：

$$
e(t)=
-N_{RX}A\omega\hat B
\cos(\omega t)\cos\theta
$$

电动势峰值为：

$$
\boxed{
\hat E=
\omega N_{RX}A\hat B\cos\theta
}
$$

又因为：

$$
\omega=2\pi f
$$

所以：

$$
\boxed{
\hat E=
2\pi fN_{RX}A\hat B\cos\theta
}
$$

### 6.1 电动势有效值

正弦波有效值为峰值除以 $\sqrt{2}$：

$$
E_{RMS}=\frac{\hat E}{\sqrt{2}}
$$

因此：

$$
E_{RMS}=
\frac{2\pi}{\sqrt{2}}
fN_{RX}A\hat B\cos\theta
$$

由于：

$$
\frac{2\pi}{\sqrt{2}}\approx4.44
$$

得到：

$$
\boxed{
E_{RMS}=
4.44fN_{RX}A\hat B\cos\theta
}
$$

如果定义单匝磁通峰值：

$$
\hat\Phi=\hat B A\cos\theta
$$

则：

$$
\boxed{
E_{RMS}=4.44fN_{RX}\hat\Phi
}
$$

这就是常见的 4.44 公式。

> 4.44 公式只适用于近似正弦磁通。如果磁通波形不是正弦波，应对真实磁链波形直接求导。

### 6.2 使用磁感应强度有效值

因为：

$$
B_{RMS}=\frac{\hat B}{\sqrt{2}}
$$

所以也可以写成：

$$
\boxed{
E_{RMS}=
2\pi fN_{RX}A B_{RMS}\cos\theta
}
$$

使用公式时必须确认 $B$ 是峰值、峰峰值还是有效值。

---

## 7. 把 TX 电流直接代入

多匝、等半径 TX 线圈在中心轴线上的磁场为：

$$
B_z(z,t)=
\frac{\mu_0N_{TX}i_{TX}(t)R_{TX}^2}
{2(R_{TX}^2+z^2)^{3/2}}
$$

假设：

- RX 面积内磁场近似均匀；
- TX 与 RX 完全平行；
- RX 一匝面积为 $A_{RX}$；

则 RX 总磁链为：

$$
\lambda(t)=
N_{RX}A_{RX}
\frac{\mu_0N_{TX}i_{TX}(t)R_{TX}^2}
{2(R_{TX}^2+z^2)^{3/2}}
$$

对时间求导：

$$
\boxed{
e_{RX}(t)=
-
\frac{
\mu_0N_{TX}N_{RX}A_{RX}R_{TX}^2
}{
2(R_{TX}^2+z^2)^{3/2}
}
\frac{di_{TX}(t)}{dt}
}
$$

如果 TX 电流为：

$$
i_{TX}(t)=\hat I_{TX}\sin(\omega t)
$$

则：

$$
\frac{di_{TX}}{dt}=
\omega\hat I_{TX}\cos(\omega t)
$$

RX 电动势峰值为：

$$
\boxed{
\hat E_{RX}=
\frac{
\mu_0N_{TX}N_{RX}A_{RX}R_{TX}^2
}{
2(R_{TX}^2+z^2)^{3/2}
}
\omega\hat I_{TX}
}
$$

该公式仍然属于简化模型，因为它把 RX 整个面积都近似成 TX 中心轴处的磁场。

---

## 8. 使用互感表示

工程中通常使用互感 $M$，避免逐点计算磁场。

如果 TX 电流产生的磁场与电流近似成正比：

$$
\vec B(\vec r,t)=\vec b(\vec r)i_{TX}(t)
$$

那么 RX 总磁链也与 TX 电流成正比：

$$
\lambda_{RX}(t)=Mi_{TX}(t)
$$

互感定义为：

$$
\boxed{
M=\frac{\lambda_{RX}}{i_{TX}}
}
$$

代入法拉第定律：

$$
e_{RX}(t)=
-\frac{d}{dt}\left[Mi_{TX}(t)\right]
$$

如果线圈位置固定、互感不随时间变化：

$$
\boxed{
e_{RX}(t)=
-M\frac{di_{TX}(t)}{dt}
}
$$

### 8.1 正弦电流

假设：

$$
i_{TX}(t)=\hat I_{TX}\sin(\omega t)
$$

则：

$$
e_{RX}(t)=
-\omega M\hat I_{TX}\cos(\omega t)
$$

峰值为：

$$
\boxed{
\hat E_{RX}=\omega M\hat I_{TX}
}
$$

有效值为：

$$
\boxed{
E_{RX,RMS}=\omega M I_{TX,RMS}
}
$$

即：

$$
\boxed{
E_{RX,RMS}=2\pi fM I_{TX,RMS}
}
$$

这是无线充电项目中最实用的感应电压估算公式之一。

---

## 9. 互感、耦合系数与自感

互感可以写成：

$$
\boxed{
M=k\sqrt{L_{TX}L_{RX}}
}
$$

其中：

- $L_{TX}$：TX 线圈自感；
- $L_{RX}$：RX 线圈自感；
- $k$：耦合系数；
- $0\le k\le1$。

代入感应电压公式：

$$
\boxed{
E_{RX,RMS}=
2\pi f
k\sqrt{L_{TX}L_{RX}}
I_{TX,RMS}
}
$$

该公式反映：

- 频率越高，感应电压倾向于越高；
- TX 电流越大，感应电压越高；
- 线圈互感越大，感应电压越高；
- 线圈完全对准时，$k$ 较大；
- 距离、偏移和倾斜增加时，$k$ 降低。

频率和电流不能无限提高，因为还会增加：

- MOSFET 开关损耗；
- 线圈交流损耗；
- 涡流损耗；
- 电磁干扰；
- 温升。

---

## 10. 简化计算示例

假设：

| 参数 | 数值 |
|---|---:|
| TX 匝数 $N_{TX}$ | 10 |
| TX 平均半径 $R_{TX}$ | 20mm |
| TX 电流峰值 $\hat I_{TX}$ | 2A |
| TX/RX 轴向距离 $z$ | 3mm |
| RX 半径 $R_{RX}$ | 15mm |
| RX 匝数 $N_{RX}$ | 12 |
| 工作频率 $f$ | 150kHz |
| 倾斜角 $\theta$ | $0^\circ$ |

暂时假设 RX 面积内磁场都等于 RX 中心处的磁场。

### 10.1 TX 在 RX 中心处的磁感应强度峰值

$$
\hat B=
\frac{
\mu_0N_{TX}\hat I_{TX}R_{TX}^2
}{
2(R_{TX}^2+z^2)^{3/2}
}
$$

代入：

$$
\hat B=
\frac{
4\pi\times10^{-7}
\times10
\times2
\times0.020^2
}{
2(0.020^2+0.003^2)^{3/2}
}
$$

得到：

$$
\boxed{
\hat B\approx0.61\ \mathrm{mT}
}
$$

### 10.2 RX 一匝面积

$$
A_{RX}=\pi R_{RX}^2
$$

$$
A_{RX}=\pi\times0.015^2
$$

$$
\boxed{
A_{RX}\approx7.07\times10^{-4}\ \mathrm{m^2}
}
$$

### 10.3 RX 一匝磁通峰值

$$
\hat\Phi=\hat B A_{RX}
$$

$$
\hat\Phi=
0.61\times10^{-3}
\times7.07\times10^{-4}
$$

$$
\boxed{
\hat\Phi\approx4.31\times10^{-7}\ \mathrm{Wb}
}
$$

### 10.4 整个 12 匝 RX 线圈的磁链峰值

$$
\hat\lambda=N_{RX}\hat\Phi
$$

$$
\hat\lambda=
12\times4.31\times10^{-7}
$$

$$
\boxed{
\hat\lambda\approx5.17\times10^{-6}\ \mathrm{Wb\cdot turn}
}
$$

### 10.5 感应电动势有效值

$$
E_{RMS}=4.44f\hat\lambda
$$

$$
E_{RMS}=
4.44
\times150000
\times5.17\times10^{-6}
$$

得到：

$$
\boxed{
E_{RMS}\approx3.44\ \mathrm{V}
}
$$

该结果表示在简化条件下，RX 整个串联线圈的开路感应电动势有效值约为 3.44V。

### 10.6 该结果的局限性

上述计算只是粗略估算，因为：

- RX 边缘位置的磁场不等于中心磁场；
- TX 和 RX 每一匝的半径不同；
- 铁氧体会改变磁场分布；
- 电池、PCB、磁铁和金属件会改变磁路；
- RX 加载后会反向影响 TX；
- 谐振网络会改变线圈电流与端口电压。

最终结果需要通过有限元仿真或互感实测验证。

---

## 11. 多匝平面线圈的精确互感思路

如果 TX 有 $N_{TX}$ 匝、RX 有 $N_{RX}$ 匝，可以将每一对匝之间的互感相加：

$$
\boxed{
M_{total}=
\sum_{m=1}^{N_{TX}}
\sum_{n=1}^{N_{RX}}
M_{mn}
}
$$

其中 $M_{mn}$ 是 TX 第 $m$ 匝与 RX 第 $n$ 匝之间的互感。

整个 RX 线圈的感应电动势为：

$$
\boxed{
e_{RX}(t)=
-M_{total}\frac{di_{TX}(t)}{dt}
}
$$

对于同轴圆形单匝线圈，$M_{mn}$ 可以使用椭圆积分计算。实际工程中通常使用：

- 电磁有限元软件；
- 专用线圈计算工具；
- 实际互感测量。

---

## 12. 互感的实测方法

### 12.1 串联同向与串联反向测量

测量：

- 两线圈串联同向电感 $L_{aiding}$；
- 两线圈串联反向电感 $L_{opposing}$。

有：

$$
L_{aiding}=L_{TX}+L_{RX}+2M
$$

$$
L_{opposing}=L_{TX}+L_{RX}-2M
$$

两式相减：

$$
L_{aiding}-L_{opposing}=4M
$$

因此：

$$
\boxed{
M=
\frac{L_{aiding}-L_{opposing}}{4}
}
$$

耦合系数为：

$$
\boxed{
k=\frac{M}{\sqrt{L_{TX}L_{RX}}}
}
$$

测量时应保持：

- 最终线圈距离；
- 最终横向位置；
- 最终铁氧体；
- 最终磁铁；
- 尽可能接近最终外壳结构。

### 12.2 通过开路感应电压计算互感

让 TX 流过已知正弦电流 $I_{TX,RMS}$，RX 保持开路，测量 RX 感应电压：

$$
E_{RX,RMS}=2\pi fM I_{TX,RMS}
$$

因此：

$$
\boxed{
M=
\frac{E_{RX,RMS}}
{2\pi fI_{TX,RMS}}
}
$$

例如：

- $f=150\ \mathrm{kHz}$；
- $I_{TX,RMS}=2\ \mathrm{A}$；
- $E_{RX,RMS}=3.77\ \mathrm{V}$。

则：

$$
M=
\frac{3.77}
{2\pi\times150000\times2}
$$

得到：

$$
\boxed{
M\approx2.0\ \mu\mathrm{H}
}
$$

---

## 13. 感应电动势不等于最终输出电压

法拉第定律计算出的 $E_{RX}$ 是 RX 线圈内部的等效感应电动势。

RX 线圈可以等效为：

```text
感应电压源 E_RX
      |
      +-- 线圈电阻 R_RX
      +-- 线圈自感 L_RX
      +-- 谐振电容 C1/C2
      +-- BQ51013B 等效输入
      +-- 整流后的实际负载
```

交流稳态下，感应电动势相量为：

$$
\underline E_{RX}=
j\omega M\underline I_{TX}
$$

RX 电流近似为：

$$
\underline I_{RX}=
\frac{
\underline E_{RX}
}{
Z_{coil}+Z_{comp}+Z_{load}
}
$$

接入负载后还会发生：

- 线圈电阻压降；
- 线圈电抗压降；
- 谐振补偿；
- RX 反向磁场对 TX 的影响；
- TX 控制器自动调频或调功率；
- BQ51013B 同步整流和输出调节。

因此，不能因为开路感应电动势计算为 5V，就直接判断整流后一定能稳定输出 5V/1A。

---

## 14. 串联匝与并联绕组

如果 RX 的 $N$ 匝全部串联，则各匝感应电动势相加：

$$
E_{total}=E_1+E_2+\cdots+E_N
$$

每匝电动势近似相同时：

$$
E_{total}\approx NE_{turn}
$$

如果两组线圈并联：

- 并联后的电压不会相加；
- 理想情况下电流承载能力可以提高；
- 两组绕组的电压、相位和耦合必须一致；
- 不能把并联的两组线圈继续当作双倍串联匝数计算电压。

---

## 15. 单位检查

### 15.1 磁感应强度

$$
[B]=\mathrm{T}
$$

### 15.2 面积

$$
[A]=\mathrm{m^2}
$$

### 15.3 磁通量

$$
[\Phi]
=
\mathrm{T\cdot m^2}
=
\mathrm{Wb}
$$

### 15.4 磁链

$$
[\lambda]
=
\mathrm{Wb\cdot turn}
$$

### 15.5 电动势

$$
[e]
=
\frac{\mathrm{Wb}}{\mathrm{s}}
=
\mathrm{V}
$$

### 15.6 互感公式

$$
\left[M\frac{di}{dt}\right]
=
\mathrm{H}\cdot\frac{\mathrm{A}}{\mathrm{s}}
=
\frac{\mathrm{Wb}}{\mathrm{A}}
\cdot
\frac{\mathrm{A}}{\mathrm{s}}
=
\mathrm{V}
$$

---

## 16. 常见计算错误

### 错误 1：把静态磁感应强度直接乘匝数当成电压

错误写法：

$$
E=NB
$$

正确关系必须包含面积和时间变化率：

$$
e=-NA\frac{dB}{dt}
$$

### 错误 2：只计算磁场大小，不考虑方向

必须使用法向分量：

$$
\Phi=BA\cos\theta
$$

### 错误 3：TX 匝数重复计算

如果 $B$ 的公式已经包含 $N_{TX}$，后续只应乘 RX 匝数 $N_{RX}$。

### 错误 4：把中心磁场当作整个 RX 面积的磁场

中心磁场只能用于初步估算。精确计算需要面积积分。

### 错误 5：混淆峰值与有效值

正弦波满足：

$$
X_{RMS}=\frac{\hat X}{\sqrt{2}}
$$

4.44 公式中的磁通必须使用峰值。

### 错误 6：把开路感应电动势当作带载输出电压

实际输出还取决于：

- 线圈阻抗；
- 谐振电容；
- 互感反射；
- 整流器；
- 负载；
- TX 闭环控制。

### 错误 7：把并联绕组按串联匝数计算

并联绕组电压不相加。

### 错误 8：直接使用自由空间电感设计最终谐振参数

铁氧体、磁铁、电池、外壳和对侧线圈都会改变电感和互感。最终参数应在实际结构下测量。

---

## 17. 最终公式汇总

### 17.1 最一般形式

RX 第 $k$ 匝磁通：

$$
\Phi_k(t)=
\int_{S_k}
\vec B(\vec r,t)\cdot d\vec S
$$

整个 RX 线圈磁链：

$$
\lambda(t)=
\sum_{k=1}^{N_{RX}}
\Phi_k(t)
$$

整个 RX 线圈电动势：

$$
\boxed{
e_{RX}(t)=
-\frac{d}{dt}
\left[
\sum_{k=1}^{N_{RX}}
\int_{S_k}
\vec B(\vec r,t)\cdot d\vec S
\right]
}
$$

### 17.2 均匀磁场近似

$$
\boxed{
e(t)=
-N_{RX}A\cos\theta
\frac{dB(t)}{dt}
}
$$

### 17.3 均匀正弦磁场

使用磁感应强度峰值：

$$
\boxed{
E_{RMS}=
4.44fN_{RX}A\hat B\cos\theta
}
$$

使用磁感应强度有效值：

$$
\boxed{
E_{RMS}=
2\pi fN_{RX}A B_{RMS}\cos\theta
}
$$

### 17.4 互感形式

瞬时形式：

$$
\boxed{
e_{RX}(t)=
-M\frac{di_{TX}(t)}{dt}
}
$$

正弦稳态有效值：

$$
\boxed{
E_{RX,RMS}=
2\pi fM I_{TX,RMS}
}
$$

### 17.5 使用耦合系数

$$
M=k\sqrt{L_{TX}L_{RX}}
$$

所以：

$$
\boxed{
E_{RX,RMS}=
2\pi f
k\sqrt{L_{TX}L_{RX}}
I_{TX,RMS}
}
$$

---

## 18. 工程应用建议

项目不同阶段建议采用不同计算方法：

| 阶段 | 推荐方法 | 用途 |
|---|---|---|
| 基础学习 | $e=-N\,d\Phi/dt$ | 理解电磁感应 |
| 初步估算 | $E=4.44fN\hat\Phi$ | 估算开路电压 |
| 参数估算 | $E=2\pi fMI$ | 利用互感估算电压 |
| 几何设计 | 磁场面积积分 | 比较线圈尺寸和距离 |
| 结构优化 | 有限元仿真 | 分析铁氧体、磁铁和偏移 |
| 最终验证 | 实测 $M$、$k$ 和开路电压 | 校验实际装配性能 |

在本项目中，最终设计不应只依赖中心磁场公式，而应采用以下闭环流程：

```text
确定 TX/RX 线圈候选
        |
        v
测量 L_TX、L_RX、M 和 k
        |
        v
计算开路感应电动势
        |
        v
配置谐振电容
        |
        v
连接 BQ51013B 和电子负载
        |
        v
测试输出功率、效率和温升
        |
        v
加入铁氧体、磁铁、外壳和电池
        |
        v
重新测量并修正参数
```
