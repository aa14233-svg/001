---
tags: [meta-king, recorder, history]
version: 1.0.0
created: 2026-07-16
replaces: 08-技能优化.md
---

# 08 Recorder 与决策链

> Recorder 不是独立文件系统,而是 MetaKing 内嵌方法。让体系有记忆。

## Recorder 的最小契约

| 字段 | 类型 | 说明 |
|------|------|------|
| `task_id` | str | 任务唯一标识 |
| `intent` | str | 用户意图 |
| `routing_decision` | dict | 路由决策(Planner 输出) |
| `gap_analysis` | dict | Thinker 对账表 |
| `repair_log` | list | ThinkerRepair 闸门记录 |
| `execution_result` | dict | Builder 执行结果 |
| `auditor_verdict` | dict | Auditor 四维审查结论 |
| `anomalies` | list | 异常列表 |
| `timestamp_start` | ISO 8601 | 开始时间 |
| `timestamp_end` | ISO 8601 | 结束时间 |
| `duration_ms` | int | 总耗时 |

## 持久化路径

按 `task_id` 分组,挂载在对应分支的 history 上持久化:

```
storage/
├── branches/
│   ├── main/
│   │   ├── history/
│   │   │   ├── 2026-07-16/
│   │   │   │   ├── task_001.json     # task_id 1
│   │   │   │   ├── task_002.json     # task_id 2
│   │   │   │   └── ...
│   │   │   └── ...
│   │   └── ...
│   └── feature-x/
│       └── history/...
```

## 查询接口

可被查询:"上次怎么处理的 X?"

```python
mk.recorder_query(task_id=None, intent_pattern=None, since=None, until=None)
# → {task_id, intent, routing_decision, execution_result, anomalies, ...}
```

## 与 PatternBank 的关系

Recorder 把每次成功案例送到 PatternBank 的 inspiration_pool(路径 B)。失败案例送到降级队列(详见 [[03-PatternBank与记忆]] 的"反路径")。

```
Recorder 完成一次任务
    │
    ├─ 成功 + 满足度高 → inspiration_pool (路径 B)
    │
    ├─ 失败 → anomalies 表 + 降级判定
    │
    └─ 任意情况 → causal_chains 表 (因果链追溯,详见 [[10-因果链追溯]])
```

## 与老 KING 技能优化的对照

| 老 KING v3.0 (技能优化) | MetaKing v1.0 (Recorder + PatternBank) |
|------------------------|---------------------------------------|
| SkillOpt Lite(Rollout→Reflect→Aggregate→Update→Validate→Save) | Recorder 持久化 + PatternBank 三路径 |
| 静态规则文件 + 自动演化管道 | 候选池 → PROMOTE_THRESHOLD → 正式规则 |
| Strength 计数器(频次幻觉) | 累计成功验证次数(明确语义) |

老 KING 的 SkillOpt 强调"自动演化",但**演化方向不可控**(频次高的不一定是好规则)。MetaKing 的 PatternBank **必须有 Auditor 把关**(通过路径 B 进入候选池),演化方向由质量维度控制。

## 关联

- 上游: [[07-Auditor四维审查]]
- 下游: [[03-PatternBank与记忆]] [[10-因果链追溯]]
- 同级: [[04-Builder任务生产链]]