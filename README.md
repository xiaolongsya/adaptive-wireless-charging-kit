# Adaptive Wireless Sealed-Battery Charging Kit

一个面向密封电池设备与桌面机器人场景的 5W 无线充电方案设计研究。项目拆分为无线发射端（TX）、无线接收与电池管理端（RX），重点讨论器件选型、热与 EMC 风险、手工焊接可行性和整体 BOM 取舍。

> 当前仓库以方案与设计文档为主，不包含可直接投产的 PCB、Gerber 或固件。所有硬件设计在用于真实电池前都必须重新完成原理图审查、PCB 检查、热测试与安全验证。

## 设计目标

- 5W 无线功率传输
- USB-C 供电
- 密封、非金属外壳
- 支持 14500 / 18650 3.7V 锂电池
- 适合低成本小批量与手工焊接
- 明确 FOD、过温、过充和过放风险边界

## 架构

```text
USB-C 5V
   │
   ▼
┌──────────────── TX ────────────────┐
│ TVS / fuse / input filtering       │
│ IP6805U integrated 5W transmitter  │
│ resonant network + A11 coil        │
└──────────────────┬─────────────────┘
                   │ magnetic coupling
┌──────────────────▼─────────────────┐
│ RX coil + rectification            │
│ battery detection / CC-CV charging │
│ protection + gauge                 │
└──────────────────┬─────────────────┘
                   ▼
          14500 / 18650 Li-ion
```

## 关键设计取舍

| 版本 | 方案 | 结论 |
| --- | --- | --- |
| V1 | BQ500100 + 分立 MOS 驱动 | 外围复杂、BOM 高，废弃 |
| V2 | IP6806 QFN-32 高集成方案 | 功能完整，但 0.5 mm 钢网间距不适合手工焊接 |
| V3 | IP6805U eSOP-8 | 外围最简、手工焊接可行性最好，作为当前方案 |

## 主要风险

- 无线耦合效率与发热
- FOD 异物检测与温升保护
- 密封外壳下的散热
- 锂电池过充、过放和短路保护
- EMC、线圈定位偏差与不同电池包兼容性

## 文档

- [V3 架构](docs/architecture.md)
- [TX 模块设计](hardware/TX/README.md)
- [RX 模块设计](hardware/RX/README.md)
- [无线耦合原理推导](docs/magnetic-induction-to-coil-emf.md)
- [V1 电路原理存档](docs/circuit-principle_V1_deprecated.md)
- [V2 架构存档](docs/architecture_V2_deprecated.md)

## 项目状态

方案研究完成，当前仓库用于记录设计思路和历史迭代。PCB、机械结构和固件尚未形成完整可复现发布物。
