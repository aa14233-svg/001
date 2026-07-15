# Meta King Skill — 七体 Dispatch Pipeline

> King 调度准则：规则文件 + 理论背景 + 演进脉络

---

## 概述

`meta-king-skill.md` 是 King-Skill 系统的最高层调度准则文件，定义了 King 主智能体如何将用户任务路由到七个子智能体（Planner / Thinker / ThinkerRepair / Searcher / Builder / Auditor / Recorder），并通过严格的 pipeline 编排确保执行质量。

---

## 文件清单

| 文件 | 用途 |
|------|------|
| `meta-king-skill.md` | 调度准则（操作规则 + 格式定义） |
| `README.md` | 本文件（架构背景 + 理论溯源 + 使用指引） |

---

## 架构全景

```
用户输入
    │
    ▼
┌──────────────────────────────────────────────┐
│                   King                       │
│  (裁决、编排、回退)                            │
└──────────────────────────────────────────────┘
    │
    │ 按触发词 + 生产链规则路由
    ▼
┌──────────┐ ┌──────────┐ ┌───────────────┐
│ Planner  │→│ Thinker  │→│ ThinkerRepair │
│ 听证+拆解│ │ 环境对账  │ │ 自修闸门       │
└──────────┘ └──────────┘ └───────────────┘
                                    │
                        ┌───────────┴───────────┐
                        ▼                       ▼
                 ┌──────────┐            ┌──────────┐
                 │ Searcher │            │ Builder  │
                 │ 信息获取  │            │ 构建执行  │
                 └──────────┘            └──────────┘
                                              │
                    ┌──────────────────────────┘
                    ▼
              ┌──────────┐     ┌──────────┐
              │ Auditor  │     │ Recorder │
              │ 横切审查  │     │ 旁路记录  │
              └──────────┘     └──────────┘
```

## 七体职责

| 子智能体 | 职责域 | 定位 |
|----------|--------|------|
| **Planner** | 需求听证 → 方案对齐 → 任务拆解 | 动手前弄清边界 |
| **Thinker** | 场景建模 + 依赖分析 + 缺口检测 | 回答"在什么条件下跑" |
| **ThinkerRepair** | 环境自愈（三级闸门） | 缺口自修 |
| **Searcher** | 信息获取（搜索/浏览/文档问答） | 从外部拿情报 |
| **Builder** | 构建与执行（代码/文件/命令/部署） | 产生 artifact |
| **Auditor** | 安全审计 + 质量评审（横切拦截） | 拦住不该发生的 |
| **Recorder** | 决策链记录 + 历史叙事（旁路监听） | 让体系有记忆 |

## 核心设计原则

1. **需求听证优先**：Planner 不可猜测用户意图，必须反问题目边界
2. **生产链强制编排**：Builder 任务不得直接派发，必须走 Planner → Thinker → Repair → Searcher(如需) → Builder → Auditor
3. **缺口分级处理**：四类缺口（mismatch / knowledge_gap / action_gap / infra_gap）各走不同解决路径
4. **自修优先于外部查**：ThinkerRepair 能解决的环境缺口先修，修不掉才交人或走 Searcher
5. **安全横切**：Auditor 在所有节点前后拦截，安全/合规未通过则停止执行
6. **记忆积累**：Recorder 记录每次决策链路，PatternBank 从成功案例中自动学习推断规则

---

## 理论背景

### VIGIL: Stage-Gated Pipeline

Meta King 的 pipeline 编排（Planner → Thinker → Repair → Searcher → Builder → Auditor）是 VIGIL 论文中 stage-gated pipeline 思想的直接实现。每关有明确的准入/准出条件，上一关未通过则无法进入下一关。

- ThinkerRepair Gate A/B/C 三级闸门对应 VIGIL 的 security check → execution cap → verification 三段校验
- 生产链规则（Builder 不得直接派发）对应 VIGIL 的 guard-before-action 策略

### SetupBench: 环境故障注入

Thinker 的四类缺口分类（mismatch / knowledge_gap / action_gap / infra_gap）借鉴了 SetupBench 的环境故障注入方法论。SetupBench 指出 agent 最常见的失败模式不是模型不够聪明，而是环境和任务需求之间存在未被检测到的缺口——这正是 Thinker 存在的意义。

- `mismatch` 对应 SetupBench 的 Software Dependency Failure
- `action_gap` 对应 Environment Variable & Config Failure
- `infra_gap` 对应 Infrastructure & Service Failure
- Gate C 的安装后验证对应 SetupBench 建议的 post-setup verification

### PatternBank: 在线学习

PatternBank 的三条学习路径（自动提取 / 自主播种 / 手动追加）和"候选→累积验证→升级正式规则"机制，借鉴了 LLM Agent 在线学习（Online Learning from Interaction）的研究方向，目标是让规则系统从静态走向自适应。

---

## 与原有 King-Skill Hook+Loop 的关系

Meta King 是 King-Skill 系统的**调度层升级**，不替代原有的 Hook+Loop 体系。

| 维度 | 原有 Hook+Loop | Meta King 调度准则 |
|------|---------------|-------------------|
| 调度粒度 | 任务级（创建 → 执行 → 回收） | 子步骤级（听证 → 对账 → 闸门 → 审查） |
| 环境感知 | 无 | Thinker 主动探测 |
| 缺口处理 | 运行时失败 | 事前检测 + 自动修复 |
| 质量保障 | 事后校验 | Auditor 横切拦截 |
| 学习能力 | 无 | PatternBank 自动学习 |

原有 Hook+Loop 的生命周期管理（初始化 → 注册 → 执行 → 回收）继续运作，Meta King 在其之上叠加了更细粒度的调度准则。

---

## 使用方式

### 加载 Skill

```python
from king_skill import SkillHub

hub = SkillHub()
hub.load_skill("meta-king")
```

King 主程序在 `hook_dispatch` 节点读取 `meta-king-skill.md` 中的路由表、生产链规则、格式定义，按规则执行任务调度。

### 查询 PatternBank

```python
mk.pattern_bank_stats()      # → {candidates, total_rules, ...}
mk.list_inference_rules()    # → [{id, source, keywords, agent, ...}]
```

### 手动追加推断规则

```python
mk.learn_inference_rule(
    task_id="task_abc",
    keywords=["redis", "连接池"],
    agent="Builder",
    mode="create",
    success_needs=["redis-py>=5.0", "connection_pool"]
)
```

---

## 版本记录

| 日期 | 变更 |
|------|------|
| 2026-07-15 | 初始版本：七体分工、路由协议、生产链规则、Planner 听证三步、Thinker 对账、ThinkerRepair 闸门、Searcher 路由、Builder 路由、Auditor 矩阵、Recorder 规则、PatternBank |
