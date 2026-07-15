---
tags: [meta-king, builder, pipeline]
version: 1.0.0
created: 2026-07-16
replaces: 04-多Agent编排.md
---

# 04 Builder 任务生产链

> Builder 任务("写代码/生成文件/执行命令")**不得直接派发 Builder**。必须走完整生产链。

## 生产链定义

```
用户: "写一个 Redis 连接池"
    → MetaKing 识别为 Builder 任务
    → 编排 pipeline:
         Planner → Thinker → Repair → Searcher(如需) → Builder → Auditor
    → Recorder 旁路监听,持久化决策链
    → PatternBank 累计学习
```

## 每一步的最小契约

### Planner 输出 → DAG

```json
{
  "id": "impl_redis_pool",
  "name": "实现 Redis 连接池",
  "depends": ["env_check"],
  "agent": "Builder",
  "mode": "create"
}
```

详见 [[02-Planner三步听证]]。

### Thinker 输出 → 需求-环境对账表

```json
{
  "node_id": "impl_redis_pool",
  "match": {
    "satisfied":     ["Python 3.11 ✓ (≥3.x)", "fastapi ✓"],
    "mismatch":      [{"need": "redis-py", "have": "未安装", "fix": "pip install redis"}],
    "knowledge_gap": [],
    "action_gap":    [],
    "infra_gap":     []
  },
  "blocking_risk": "低 — 仅 1 个 mismatch,可自修"
}
```

详见 [[05-Thinker环境探测]]。

### ThinkerRepair 闸门(条件触发)

只在 Thinker 报告有可自修 mismatch / infra_gap 时执行。详见 [[06-ThinkerRepair自修闸门]]。

### Searcher 旁路(条件触发)

只在 Thinker 报告有 knowledge_gap 时执行。**action_gap 和 infra_gap 不浪费 Searcher 配额**。

### Builder 执行

按 DAG 顺序执行 create/execute/retrieval 模式。**输入必须来自 Planner 的拆解输出或 MetaKing 的明确规格说明,不得凭空执行**。

### Auditor 拦截

四维审查横切所有写入操作,详见 [[07-Auditor四维审查]]。

### Recorder 落盘

每个 task_id 关联一次完整链路记录,详见 [[08-Recorder与决策链]]。

## 与老 KING 多 Agent 编排的对照

| 老 KING v3.0 | MetaKing v1.0 |
|-------------|---------------|
| Sub-agent Forking + Coordinator(并发) | Planner DAG(拓扑排序,显式依赖) |
| orch_fork / orch_coordinate / orch_batch_fork 三个工具 | Builder 一个执行点,通过 mode 区分 create/execute/retrieval |
| 拓扑排序并行 | 串行 + 条件闸门(Repair) + 旁路(Searcher) |

**关键差异**:老 KING 强调"多 Agent 并发",MetaKing 强调"七体串行 + 闸门" — **并发在 LLM 时代的成本是 token 翻倍,MetaKing 默认拒绝并发,只对 Planner 明确标注 `parallel=true` 的节点才并行**。

## 关联

- 上游: [[01-MetaKing调度中枢]]
- 同级: [[05-Thinker环境探测]] [[06-ThinkerRepair自修闸门]] [[07-Auditor四维审查]] [[08-Recorder与决策链]]
- 下游: [[11-部署拓扑总览]]