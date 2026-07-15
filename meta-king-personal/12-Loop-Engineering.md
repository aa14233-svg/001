---
tags: [meta-king, paradigm, loop-engineering]
version: 1.0.0
created: 2026-07-16
replaces: 12-Loop-Engineering.md
---

# 12 Loop Engineering · 从 Prompt 到 Loop 的范式转移

> 老 KING v3.0 和 MetaKing v1.0 在范式层面共享同一根基。

## 范式转移的历史

```
Prompt Engineering (2023)        Loop Engineering (2024+)
        │                                  │
        ▼                                  ▼
  "如何写好 prompt"                    "如何设计持续运行的 loop"
        │                                  │
        ▼                                  ▼
  一次性 prompt 优化                  持续迭代 + 经验积累
  静态规则                           动态学习(PatternBank)
  无状态                             有状态(Recorder)
```

## Loop 的四要素

| 要素 | 老 KING v3.0 | MetaKing v1.0 |
|------|-------------|---------------|
| **Trigger** | KAIROS 三闸触发器(时间/会话数/锁) | Planner 听证触发(用户意图) |
| **Actions** | 7 个固定工具(agent_run / kairos_status / ...) | 7 个认知体(动态路由) |
| **Termination** | 预算控制(token/时间/调用次数上限) | Auditor 四维审查(质量 ≥ 0.7 通过) |
| **Verification** | 三级验收 D1/D2/D3 | Recorder 持久化 + 因果链追溯 |

## MetaKing 的 Loop 形态

MetaKing 的 Loop **不是定时器驱动的后台 loop**,而是**任务驱动的同步 loop**:

```
loop(task):
    dag = Planner.hear(task)
    gaps = Thinker.probe(dag, env_snapshot)
    if gaps.mismatch: repaired = ThinkerRepair.gate(gaps.mismatch)
    if gaps.knowledge_gap: facts = Searcher.retrieve(gaps.knowledge_gap)
    artifact = Builder.execute(dag, facts)
    verdict = Auditor.audit(artifact)
    Recorder.persist(task_id, dag, gaps, artifact, verdict)
    PatternBank.maybe_promote(task_id, verdict)
    return verdict.passed ? artifact : None
```

**关键差异**: 老 KING 的 KAIROS loop 是后台定时器(每 5 分钟检查),MetaKing 的 loop 是前台任务驱动(用户来一次跑一次)。前者适合"持续维护"场景(如 autoDream),后者适合"实时响应"场景(如用户提问)。

**两者可以共存**: 详见 [[13-Claude-Code-蒸馏]] 中关于 KAIROS 在 MetaKing 上的兼容层设计。

## 关联

- 上游: [[11-部署拓扑总览]]
- 下游: [[13-Claude-Code-蒸馏]]
- 同级: [[01-MetaKing调度中枢]]