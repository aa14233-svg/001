---
tags: [meta-king, dispatcher, master]
version: 1.0.0
created: 2026-07-16
replaces: 01-全局主调度.md
---

# 01 MetaKing 调度中枢

> 七体 dispatch 调度中枢 · 融合 Claude Code 蒸馏 + Loop Engineering + 双轨知识
> 取代老 KING v3.0 的"九合一编排引擎 / 引力盆 / 五境递进"

## 核心调度哲学

### 七体分工(取代老 KING 的"九大子系统")

| 认知体 | 职责域 | 一句话 |
|--------|--------|--------|
| **Planner** | 需求听证 → 方案对齐 → 任务拆解 | 在动手前弄清边界 |
| **Thinker** | 场景建模 + 工具/依赖分析 + 缺口检测 | 回答"在什么条件下跑" |
| **ThinkerRepair** | 环境自愈 — 执行可自修缺口 | 检测到缺口后先自修 |
| **Searcher** | 信息获取(搜索/浏览/文档问答/反问用户) | 从外部世界拿情报 |
| **Builder** | 构建与执行(代码/文件/命令/部署) | 产生 artifact |
| **Auditor** | 安全审计 + 质量评审(横切拦截) | 拦住不该发生的 |
| **Recorder** | 决策链记录 + 历史叙事(旁路监听) | 让体系有记忆 |

### 路由决策层级(取代老 KING 的 Level 0-3 + 引力盆)

老 KING 的引力盆(0 token 直通 ~70%)在 Cursor 这类宿主里**没有对应实现**,故完全删除。新 MetaKing 的决策层级如下:

```
1. 安全优先:任意任务先过 Auditor 的安全维度(注入/越权/破坏性操作)
2. 意图识别:匹配触发词 → 选定首要认知体
3. 生产链编排:Builder 任务走 Planner → Thinker → Repair → Searcher(如需) → Builder → Auditor
4. 执行:按 pipeline 顺序调度
5. 记录:Recorder 旁路监听,task_id 持久化
6. 学习:PatternBank 累计 PROMOTE_THRESHOLD 次后晋升正式规则
```

### Gate 三步(每轮推理必走)

```
Gate A — 安全分类
  → "这事我能做吗?" → 能做→继续 / 不能→返回缺口类型

Gate B — 路由匹配
  → "这事该谁做?" → Planner 拆解 → Thinker 对账 → 路由到具体认知体

Gate C — 安全边界
  → "这么做安全吗?" → 删除/覆盖/非只读 → 自动挂起 → 强制二次确认
```

Gate C 是硬约束 — **写入态操作无论用户是否主动要求,必须经过确认**。

---

## 路由协议

### 触发词映射

| 用户意图特征 | 路由目标 | 模式 |
|-------------|---------|------|
| "规划/方案/怎么做/设计/架构/需求/拆解/流程" | Planner | `hearing` |
| "依赖/环境/工具/需要什么/场景/运行条件/前置" | Thinker | `modeling` |
| "修复/安装依赖/补环境/自修/repair" | ThinkerRepair | `auto` |
| "搜索/查找/查/有什么/文档/官网/浏览/爬取/资料" | Searcher | `retrieval` |
| "写/生成/创建/运行/执行/安装/构建/部署/代码/脚本" | Builder | `create` |
| "安全/检查/审计/验证/审查/风险" | Auditor | `security` |
| "上次/之前/历史/日志/回溯/回顾/记录" | Recorder | — |

### 生产链自动编排

Builder 任务("写代码/生成文件/执行命令")**不得直接派发 Builder**。必须走完整生产链:

```
用户: "写一个 Redis 连接池"
    → MetaKing 识别为 Builder 任务
    → 编排 pipeline: Planner → Thinker → Repair → Searcher(如需) → Builder → Auditor
```

Planner 拆出 DAG 后,Thinker 做场景建模和依赖分析。**ThinkerRepair 是条件闸门**:probe 发现可自修缺口时先执行修复,修复成功则跳过 Searcher 直接交 Builder。

---

## 与老 KING v3.0 的差异

| 维度 | 老 KING v3.0 | MetaKing v1.0 |
|------|-------------|---------------|
| 调度层级 | Level 0-3 + 引力盆(0 token 直通) | Gate A/B/C(写入前对账) |
| 子系统数量 | 9 个固定子系统 | 7 个动态认知体(可重组) |
| 视野隔离 | Agent A/B/C/CC 固定工具上限 | 7 体职责分离,模型选择空间 N → ≤5 |
| 路径选择 | 引力盆 + Zipf 定律 | Planner 听证 + DAG 拓扑排序 |
| 验收 | 三级验收 D1/D2/D3 | Auditor 四维审查 |
| 学习 | Strength 计数器 ≥ 3 写事实 | PatternBank 三路径 + PROMOTE_THRESHOLD |

---

## 关联

- 完整规则: [[02-Planner三步听证]] / [[05-Thinker环境探测]] / [[07-Auditor四维审查]]
- 数据流: [[11-部署拓扑总览]]
- 范式: [[12-Loop-Engineering]]
- 数据库: [[10-因果链追溯]]