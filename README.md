# APEX-FORGE · 旗舰算法工具库

> **从零造 AI，并造尺子证明它没骗你** · 一套开源的"AI 验伪"方法与实物
> 舰队总览：[apex-flagship](https://github.com/CJX0712/apex-flagship) · 作者：晨星

> 站在世界顶级成果之上锻造的世界级 AI 基础算法集。
> **零重写 · 集大成 · 可验证 · 闭环开源。**

作者：晨星（GitHub: [CJX0712](https://github.com/CJX0712)） · 许可：MIT

---

## 一、旗舰原则（贯穿每一行代码）

| 原则 | 含义 | 在本库的体现 |
|------|------|--------------|
| **零重写** | 禁止从零造轮子；任何需求先检索世界已有的最优解 | 全部模块复用权威论文/数值实践，未重新发明算法 |
| **集大成** | 每一行都必须站在世界顶级成果之上 | 每个算法标注权威出处，可直接溯源 |
| **唯一目标** | 锻造世界顶级 AI | 数学不变量交叉验证，正确性可被独立复现 |
| **闭环交付** | 完整开源、可运行、可复现、可验证 | 单文件零依赖，Node 8/8 全绿 + 浏览器一键自检 |
| **自治执行** | 自主安装/配置/诊断/修复 | 零外部依赖，零构建，开箱即跑 |

---

## 二、自检结果（8/8 全绿）

```
=== APEX-FORGE 自检 ===
✅ 矩阵乘法 matmul            | C=[[19,22],[43,50]] 精确命中
✅ Softmax 数值稳定性          | 平移 1e6 最大差=0.00e+0
✅ Softmax 归一 Σ=1         | Σ=1.000000000000000
✅ LayerNorm μ≈0,σ²≈var/(var+ε) | μ=1.73e-17 σ²=0.9999862605 理论=0.9999862605
✅ 注意力 Σw=1 + 因果          | Σw=1.000000000000 w[pos0→3]=0
✅ Adam 收敛性               | loss 2.498→2.39e-14 (↓>$3数量级)
✅ Xavier 方差              | 实测σ²=0.06670 目标=0.06667
✅ 梯度检验 ∂L/∂logits        | maxRelErr=2.54e-10
```

每条自检都是一条**可被独立实现交叉验证的不变量**（解析解 vs 数值解、对数似然单调、梯度检验……），而非“看起来对”。

---

## 三、已集成的世界顶级算法

| 模块 | 算法 | 权威出处 |
|------|------|----------|
| `LA` | 矩阵乘法（深度学习基石） | 标准线性代数 |
| `Act` | 数值稳定 Softmax（log-sum-exp）、GELU、SiLU | Hendrycks & Gimpel 2016 (1606.08415)；数值稳定实践 |
| `Norm` | LayerNorm、RMSNorm | Ba et al. 2016 (1607.06450)；Zhang & Sennrich 2019 (1910.07467) |
| `Attention` | 缩放点积注意力、多头注意力（含因果掩码） | Vaswani et al. 2017 (1706.03762) |
| `Adam` / `AdamW` / `RMSProp` | 现代优化器（AdamW 解耦权重衰减） | Kingma & Ba 2015 (1412.6980)；Loshchilov & Hutter 2019 (1904.09237) |
| `Init` | Xavier / Kaiming 初始化 | Glorot & Bengio 2010；He et al. 2015 (1502.01852) |
| `Sample` | Top-k / Top-p（核）采样 | Fan et al. 2018 (1805.04833)；Holtzman et al. 2020 (1904.09751) |
| `Loss` | 数值稳定交叉熵、MSE、解析梯度 | 标准监督学习 |

---

## 四、快速开始

### 环境要求
- **零依赖**：仅需 Node.js（无头自检）或任意现代浏览器（可视化自检）。
- **零构建**：直接运行，无需 `npm install`。

### 无头运行自检
```bash
node apex-forge.js
# 输出 8/8 全绿
```

### 浏览器可视化自检
直接用浏览器打开 `index.html`，即可看到 8 项不变量自检卡片与总分。

### 作为模块引用
```js
const { Attention, Adam, Norm, Act, Init, Sample, Loss, LA } = APEX;

// 多头注意力
const out = Attention.multiHead(Q, K, V, 8, causalMask);

// 优化一步（AdamW，解耦权重衰减）
const opt = new APEX.AdamW(1e-3, 0.9, 0.999, 1e-8, 0.01);
const wNext = opt.step([w], [grad]);

// 数值稳定 softmax
const p = Act.softmax(logits);

// Top-p 核采样
const masked = Sample.topP(logits, 0.9);
```

---

## 五、为什么可信（认知反缴械）

本库拒绝“看起来很完整”的幻觉。每一项能力都附带一条**数学不变量**作为硬金标准：
- 矩阵乘法与手算小例精确一致；
- Softmax 平移 1e6 不变（log-sum-exp 防溢出）；
- 注意力每行权重和严格为 1，因果掩码下位置 0 看不到未来；
- Adam 在凸二次损失上 300 步收敛 3+ 数量级；
- Xavier 实测方差与理论 `2/(fanIn+fanOut)` 误差 < 0.05%；
- 梯度检验最大相对误差 `2.54e-10`（中心差分 vs 解析梯度）。

---

## 六、目录结构

```
flagship-algo-forge/
├── apex-forge.js   # 核心：全部算法 + 8/8 内置自检（零依赖）
├── index.html      # 浏览器可视化自检页（单文件内联）
└── README.md       # 本文档
```

> 下一步：以本工具库为地基，组合出端到端 AI 系统（RAG / 推理引擎）与十层架构参考实现 + benchmark 对照——让“最先进”有对手、有打分。
