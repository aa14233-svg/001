---
tags: [meta-king, pattern-bank, recorder]
version: 1.0.0
created: 2026-07-16
replaces: 03-记忆系统.md
---

# 03 PatternBank 与记忆

> MetaKing 不会只"执行"然后就忘了。它有一个 **PatternBank**,三路径积累调度经验。

## 与老 KING 记忆系统的差异

老 KING v3.0 的记忆系统是 **MEMORY.md + autoDream + 团队记忆** 三件套,核心机制是 **strength 计数器**(count × recency_weight ≥ 3 → 写入事实) 和 **cosine 相似度去重**(> 0.92 合并)。

MetaKing 把这套**全部删除**,原因是:

1. **strength 计数器是"频次幻觉"**:相同查询触发 N 次不一定意味着"规则正确",只意味着"用户重复"。
2. **cosine 去重阈值(0.92)是经验值**,没有跨领域可迁移性。
3. **MEMORY.md 作为单文件索引**在团队规模下会变成瓶颈。

**MetaKing 用 PatternBank 替代**,三路径学习,候选模式累计 PROMOTE_THRESHOLD(默认 3)次成功验证后自动升级为正式规则。

## PatternBank 三路径

```
路径 A(自动提取):
  _reconcile_node → 满足度高 → _record_success_pattern → 候选池
  → 累计 PROMOTE_THRESHOLD 次 → _promote_single → 正式规则

路径 B(自主播种):
  Auditor 放入 inspiration_pool 的成功案例
  → hook_complete → _seed_from_inspiration_pool → 候选池
  → 同路径 A 累积升级

路径 C(手动追加):
  learn_inference_rule(task_id, keywords, agent, mode, success_needs)
  → 直接添加为正式规则(跳过高频去重)
```

## 关键参数

| 参数 | 默认值 | 含义 |
|------|--------|------|
| `PROMOTE_THRESHOLD` | 3 | 候选模式被成功验证多少次后自动升级 |
| 去重重叠阈值 | 70% | 与已有规则关键词重叠超过此比例则拒绝添加 |
| 自动提取满足度阈值 | 60% | reconciliation 中 satisfied 占比达到此值触发提取 |

## 反路径(降级与衰减)

老 KING 只定义了"晋升",没有"降级"。MetaKing 补齐:

- **降级**:若一个 Pattern 失败 ≥ 3 次,标记 `deprecated`,不再使用。
- **衰减**:30 天未命中降级,90 天清除(与老 KING 7 条机制的 M7 一致,但作用域从引力盆条目改为 PatternBank 条目)。

## 历史叙事(Recorder 的元能力)

Recorder 不是独立文件系统,而是 MetaKing 内嵌方法:

- 每次任务完成后记录:`意图 → 路由决策 → 执行结果 → 异常`
- 按 `task_id` 分组,挂载在对应分支的 history 上持久化
- 可被查询: "上次怎么处理的 X?"

```python
mk.pattern_bank_stats()   # → {candidates, total_rules, static_rules, learned_rules, ...}
mk.list_inference_rules() # → [{id, source, keywords, agent, mode, needs}]
```

## 关联

- 上游: [[07-Auditor四维审查]] 把成功案例送入 inspiration_pool
- 下游: [[09-MetaKing数理机制]] 第 5/7 条
- 旁路: [[10-因果链追溯]] 是 Recorder 的物理存储