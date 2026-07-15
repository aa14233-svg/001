---
tags: [meta-king, distillation, claude-code]
version: 1.0.0
created: 2026-07-16
replaces: 13-Claude-Code-蒸馏.md
---

# 13 Claude Code 蒸馏 · MetaKing 视角

> Claude Code 512K 行源码的蒸馏,改从 MetaKing 七体视角重新解读。

## 老 KING 的蒸馏视角 vs MetaKing 的蒸馏视角

| 老 KING v3.0 | MetaKing v1.0 |
|-------------|---------------|
| QueryEngine.ts → Agent Loop 引擎 | QueryEngine.ts → Planner + Builder 协同 |
| MEMORY.md + autoDream → 记忆系统 | MEMORY.md + autoDream → PatternBank 三路径 |
| Coordinator Mode → 多 Agent 编排 | Coordinator Mode → Planner DAG 拓扑排序 |
| System Prompt 模板 → 提示词引擎 | System Prompt 模板 → **删除**(改 Thinker 需求-环境对账) |
| 87 Feature Flags → 功能开关 | 87 Feature Flags → **保留**(作为兼容层) |
| 四级权限 + 注入检测 → 安全系统 | 四级权限 + 注入检测 → Auditor 安全维度 |
| SkillOpt Lite → 技能自动演化 | SkillOpt Lite → PatternBank 三路径 |
| Claude Code Provider → cc-switch | Claude Code Provider → cc-switch (保留) |
| BudgetManager → token-stats | BudgetManager → token-stats (保留) |

## 关键蒸馏要点

### 1. QueryEngine.ts 的核心循环 → Planner + Builder 协同

Claude Code 的 QueryEngine 核心是一个 while 循环:发请求 → 收响应 → 解析工具调用 → 执行 → 回到发请求。MetaKing 把这个循环拆成两个认知体:

- **Planner**: 解析用户意图,产出 DAG
- **Builder**: 串行执行 DAG 中的每个节点

**好处**: 调试时可以分别检查 Planner 的输出和 Builder 的执行,而不是纠缠在一个 while 循环里。

### 2. MEMORY.md 的层次 → PatternBank + Recorder

Claude Code 的 MEMORY.md 是单文件索引,老 KING 把它升级为三层压缩(autoDream 四阶段)。MetaKing 进一步解耦:

- **PatternBank**: 学习正式规则(PROMOTE_THRESHOLD 控制晋升)
- **Recorder**: 持久化决策链(task_id 关联)
- **causal_chains**: 因果链,详见 [[10-因果链追溯]]

**好处**: 三个存储职责分离,各自可独立备份/归档/查询。

### 3. Coordinator Mode → Planner DAG 拓扑排序

Claude Code 的 Coordinator 模式是"派发子任务 + 协调结果"。MetaKing 用 Planner 的 DAG 替代:

```python
# 老 KING 风格
coordinator.dispatch(task, sub_tasks, parallelism='auto')

# MetaKing 风格
dag = Planner.decompose(task)  # 含 depends 和 mode
for layer in topo_sort(dag):
    for node in layer:
        if node.mode == 'create': Builder.create(node)
        elif node.mode == 'execute': Builder.execute(node)
        elif node.mode == 'retrieval': Searcher.retrieve(node)
```

**好处**: 并发是显式的(`parallel=true` 标注),默认串行(节省 token)。

### 4. 87 Feature Flags → 兼容层保留

老 KING 的 87 Feature Flags 在 MetaKing 中**保留作为兼容层**。原因是这些 flag 已经在 Claude Code 生产环境验证,迁移成本高。MetaKing 在 [[01-MetaKing调度中枢]] 的启动序列中读取这些 flag,但决策层级使用自己的 Gate 三步。

### 5. 四级权限 → Auditor 安全维度

Claude Code 的四级权限(L0 只读 / L1 写工作区 / L2 写系统配置 / L3 网络)被 Auditor 的"安全"维度吸收,但 **MetaKing 拒绝"分级权限"思维**,改为"白名单+对账":

- 默认所有写入都需 Gate C 二次确认
- 不存在"L1 写工作区可自动执行"这种默认放行

## 与老 KING Claude Code 蒸馏的差异

老 KING 把 Claude Code 的子系统**一对一映射**到自己的子系统(9 个子系统对应 9 个蒸馏来源)。MetaKing 不做一对一映射,而是按"职责"重新组织 — **有些 Claude Code 子系统被合并(如提示词引擎被删除),有些被拆解(如 MEMORY.md 拆为 PatternBank + Recorder)**。

## 关联

- 上游: [[12-Loop-Engineering]]
- 下游: [[14-反蒸馏防御]]
- 同级: [[01-MetaKing调度中枢]] [[04-Builder任务生产链]]