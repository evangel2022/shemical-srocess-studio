# Chemical Process Studio

面向化工类专业本科教学的轻量级稳态流程仿真工作台。使用拖拽式 PFD 画布搭建教学流程，编辑进料和单元操作参数，运行独立的 TypeScript 计算 Engine，并查看物料/能量平衡、结果图表、计算 Trace 和两次工况对比。

> **重要声明**
>
> 本项目是教学和基础流程计算学习工具，**不是 Aspen Plus 等工业流程模拟软件的替代品**。
>
> 计算采用假想二元教学组分 A/B 和常数 Cp 教学物性，**不构成真实化学物性、工业设计或工艺预测结果**。

---

## 产品截图

以下截图来自当前版本的 Engine 实际运行结果。

### 默认教学流程

![默认教学流程](docs/screenshots/default.png)

### 结果图表

![结果图表](docs/screenshots/charts.png)

### 计算 Trace

![计算 Trace](docs/screenshots/trace.png)

### 两次工况对比

![两次工况对比](docs/screenshots/compare.png)

### Unsupported 设备标识

![Unsupported 设备标识](docs/screenshots/unsupported.png)

---

## 环境要求

| 项目 | 要求 |
|---|---|
| 操作系统 | Windows 11 / Windows 10（桌面浏览器） |
| Node.js | **推荐 Node.js 24 LTS** |
| Node.js 支持范围 | `^22.12.0 || ^24.0.0 || ^26.0.0` |
| npm | 随 Node.js 安装，建议 `>=10` |
| 浏览器 | 当前 Chrome / Edge / Firefox 桌面版 |
| 后端服务 | 不需要 |
| 环境变量 | 不需要 |

推荐使用 Node.js 24 LTS x64 安装包；Node.js 22 LTS（22.12 及以上）也可运行。

项目声明支持 Node.js 22、24、26 主版本系列；不使用未验证的 Node.js 23、25 等奇数次版本。

本次干净安装和浏览器验证使用 Node.js 24.22.3 LTS。Node.js 22/26 的 engines 兼容范围已核对，但未在各自独立的干净环境中逐一验证。

可以从 [nodejs.org](https://nodejs.org/) 下载对应版本的 Windows 安装包。

---

## Windows 11 安装步骤

### 1. 安装 Node.js

安装 Node.js 24 LTS，然后在 PowerShell 中确认：

```powershell
node -v
npm -v
```

`node -v` 应显示 `v24.x.x`（或满足上方支持范围的版本）。

### 2. 获取项目

```powershell
git clone https://github.com/evangel2022/shemical-srocess-studio.git
cd shemical-srocess-studio
```

如果没有 Git，也可以从仓库页面下载 ZIP 后解压。

### 3. 安装依赖

```powershell
npm ci
```

`npm ci` 会严格按照 `package-lock.json` 安装依赖，不会意外升级版本。

首次安装需要网络连接；项目运行时不依赖网络或后端服务。

### 4. 启动开发工作台

```powershell
npm run dev
```

打开终端显示的本地地址，通常为：

```text
http://localhost:5173
```

### 5. 生产构建与本地预览（可选）

```powershell
npm run build
npm run preview
```

生产预览通常位于：

```text
http://localhost:4173
```

---

## 常用命令

| 命令 | 用途 |
|---|---|
| `npm ci` | 从锁文件安装依赖 |
| `npm run dev` | 启动 Vite 开发服务器 |
| `npm run test` | 运行 Vitest 测试 |
| `npm run lint` | 运行 Oxlint 静态检查 |
| `npm run build` | TypeScript 编译 + Vite 生产构建 |
| `npm run preview` | 预览生产构建 |

---

## 当前支持范围

### 已支持

- **Feed**：进料流量、温度、压力、A/B 组成
- **Mixer**：多股进料混合、质量/组分守恒、绝热混合出口温度
- **Heater**：目标出口温度、常数 Cp 显热负荷
- **Cooler**：目标出口温度、常数 Cp 冷却负荷
- **Intermediate**：中间物流 pass-through
- **Product**：产品边界物流
- 基础稳态物料平衡
- 基础能量平衡
- 假想二元组分 A/B
- 常数 Cp 教学物性（2.2 kJ/(kg·K)）
- 计算 Trace
- 结果图表（温度/流量）
- 两次真实 Engine 运行的工况对比
- 本地保存与恢复
- stale / failed / partial / unsupported 状态管理
- 空流程、非法输入、断开的物流、循环连接等错误诊断

### 暂不支持

- **Flash Drum 数值计算**：尚未实现闪蒸及汽液相平衡计算
- **Separator 数值计算**：尚未实现分离计算
- **Pump 数值计算**：尚未实现泵功率及压力提升计算
- 真实工业物性数据库
- 汽液相平衡（VLE）
- 精馏
- 反应器动力学
- 动态仿真
- 复杂压降和水力计算
- 工业级工艺预测

Flash Drum、Separator、Pump 可以在设备库中拖入画布，仅用于流程布局和教学演示；它们会被明确标注为 **UNSUPPORTED / 未支持**，不会产生虚构的数值结果，也不参与 Engine 求解。

---

## 默认教学流程

```text
Feed A ──┐
         ├── Mixer ── Heater ── Product
Feed B ──┘
```

默认结果（Engine 计算）：

```text
总进料流量    12 t/h
产品流量      12 t/h
进料加权温度  45°C
产品温度      85.0°C
净热负荷      293.3 kW
质量闭合      100.0%
```

设备参数：

| 设备 | 参数 |
|---|---|
| Feed A | 6 t/h, 25°C, 2.8 bar, A=95 wt% |
| Feed B | 6 t/h, 65°C, 2.6 bar, A=15 wt% |
| Mixer | 绝热混合，出口压力取入口最小值 |
| Heater | 目标出口温度 85°C |
| Product | 12 t/h, 85°C |

---

## Engine 计算模型

计算核心位于 `src/engine/`，不依赖 React、DOM、ECharts 或浏览器存储。

### 物料衡算

- Mixer 满足总质量守恒与组分质量守恒。
- 产品物流来自上游已计算物流。
- 组分使用假想的 A/B 二元教学物料。

### 能量衡算

- 使用常数 Cp：`Cp = 2.2 kJ/(kg·K)`。
- Mixer：绝热混合，出口温度按入口流量和 Cp 加权平均。
- Heater/Cooler：按目标出口温度计算显热负荷：

```text
Q = ṁ · Cp · (T_out - T_in)
```

- 当前教学模型不考虑压力对液体显热的影响，也不计算复杂压降。

### 求解方式

- 仅支持有向无环图（DAG）。
- 循环连接会被诊断并拒绝，不做单次通过近似。
- 每个设备具有输入/输出连接约束。
- Engine 在内部使用 SI 单位，UI Adapter 负责 t/h、°C、bar 等单位转换。

### 结果解释

- **计算成功**：完整流程计算成功。
- **部分结果**：部分分支成功，部分设备或物流被阻塞。
- **计算失败**：本次没有产生可展示的成功结果。
- **UNSUPPORTED**：设备尚未实现计算能力。
- **STALE / 上次结果**：流程已修改，但尚未重新运行。

---

## 结果、Trace 与工况对比

### Results Panel

- **概要**：总进料、产品温度、净热负荷、质量闭合
- **物料平衡**：各物流流量、温度、压力、A/B 组成与状态
- **能量平衡**：各加热/冷却设备的负荷与能量构成
- **图表**：物流温度分布和流量分布
- **对比**：最近两次真实 Engine 运行的总进料、Mixer 出口温度、Heater 热负荷、Product 流量/温度、物料平衡
- **计算详情**：每个计算步骤的公式、输入、输出、假设和原始字段

### Trace 单位显示

Engine Trace 保留 SI 原始字段，UI 同时显示易读单位：

| Engine 原始单位 | UI 展示单位 |
|---|---|
| kg/s | t/h |
| K | °C |
| Pa | bar |
| W | kW |
| J/(kg·K) | kJ/(kg·K) |

转换仅发生在 UI 展示层，不修改 Engine 原始计算数据。

### 工况对比

修改参数并再次点击“运行仿真”后，可以比较两次真实 Engine 运行。

只修改流程但没有重新运行时，不会生成虚假的新对比结果。

---

## 本地保存与恢复

- 保存 Key：`chemical-process-studio.flowsheet.v1`
- 保存内容：流程节点、物流、节点位置、参数、保存时间。
- 页面刷新后自动恢复最近一次保存的流程，并重新运行 Engine。
- 如果本地数据损坏或无效，会回退到默认教学流程。

工作台会显示：

- 尚未保存
- 有未保存更改（脏状态）
- 已保存时间

---

## 已知限制

- 仅支持稳态、单相液体、常数 Cp、假想二元组分。
- 不支持真实物性、相平衡、精馏、反应、动态、复杂压降。
- 循环连接会被拒绝，不做循环收敛计算。
- Flash Drum、Separator、Pump 仅作为占位设备，不具备计算能力。
- Engine 结果不持久化；刷新页面后会重新计算。
- 对比历史保存在当前页面内存中，刷新后清空。
- 计算结果仅用于教学，不可用于工程设计、安全分析或工业预测。

---

## 项目结构

```text
src/
  engine/               # 独立 TypeScript 计算 Engine
    domain/             # 域模型、诊断、结果类型
    operations/         # Mixer、Heater/Cooler 计算
    properties/         # 假想教学二元物性
    solver/             # 图构建、DAG 求解、运行入口
    units/              # SI 单位转换
    adapters/           # UI 流程 → Engine 域模型
    __tests__/          # Engine 测试
  simulation/           # Engine 结果 → UI View Model
  components/           # 画布、设备库、属性检查器、结果面板
  hooks/                # 流程历史、保存、Toast
  data/                 # 教学示例、设备定义
  types/                # UI 类型
  utils/                # 格式化、存储、节点工厂
```

详细 Engine 说明见 [src/engine/README.md](src/engine/README.md)。

---

## 测试

```powershell
npm run test
```

当前测试覆盖：

- Engine 单位转换、Mixer、Heater/Cooler、DAG/循环、错误诊断、数值边界
- UI Adapter 单位映射
- Simulation View Model 的 success / partial / failed / unavailable
- Charts 数据只来自已计算物流
- Trace 单位转换与原始数据保持
- stale 状态、失败历史、两次运行对比

---

## 第三方依赖

完整依赖树的许可证审查见 [docs/THIRD_PARTY_LICENSES.md](docs/THIRD_PARTY_LICENSES.md)。

本项目自身的 MIT License **不改变**任何第三方依赖的独立版权、许可证或 NOTICE 保留义务。

---

## 许可证

本项目采用 **MIT License**。

```text
Copyright (c) 2026 Lei Meng
```

完整授权文本见仓库根目录的 [LICENSE](LICENSE)。

本项目自身采用 MIT License；第三方依赖仍分别遵循其原有许可证。项目许可证不覆盖、替代或改变第三方依赖自身的版权和许可义务。

许可证决策记录见 [docs/LICENSE_DECISION.md](docs/LICENSE_DECISION.md)。

---

## 非工业预测声明

本项目使用假想教学组分、常数 Cp 和简化稳态模型。界面中的流量、温度、压力、热负荷、组成等结果仅用于化工本科教学和基础流程计算学习。

**这些结果不代表真实化学物质性质、工业装置行为或工程设计结论。**

---

## 贡献

当前阶段以教学 MVP 和发布准备为主。

项目采用 MIT License；提交贡献即表示同意以该许可证授权相应贡献，并保留第三方依赖原有的许可和 NOTICE 义务。
