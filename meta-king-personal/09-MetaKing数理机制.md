---
tags: [meta-king, mechanism, math]
version: 1.0.0
created: 2026-07-16
replaces: 09-数理机制背书.md
---

# 09 MetaKing 数理机制

> MetaKing 设计决策的数学、物理、逻辑原理。每条机制都有一个形式化定义。

## 与老 KING 数理机制的对照

老 KING v3.0 的 7 条机制(M1-M7)有 4 条被**完全删除**:

- M1 引力盆路由 → 删除(无 Cursor 等价物)
- M2 权重最小路径选择 → 删除(改由 Planner 听证三步替代)
- M3 语义去重 (cosine > 0.92) → 删除(改由 Thinker 需求-环境对账替代)
- M4 strength 计数器 → 删除(改由 PatternBank PROMOTE_THRESHOLD 替代)

3 条**保留并升级**:

- M5 因果链追溯 → 升级为新 M1(详见 [[10-因果链追溯]])
- M6 RRF 融合排序 → 升级为新 M6(详见下)
- M7 衰减机制 → 升级为新 M7(详见下)

新 MetaKing 7 条机制如下:

---

## 新 M1 — Planner 听证完备性

**原理**:对任意非平凡任务,Planner 至少收集五维听证回答的前三维(scope/format/constraint)才能进入拆解阶段。

**形式化**:

```
∀ task ∈ non_trivial:
    has_hearing(task) := {scope, format, constraint} ⊆ answers(task)
    require(has_hearing(task)) before decomposing(task)
```

**应用**:Planner 强制反问;用户已在消息中明确给出则跳过对应问题(详见 [[02-Planner三步听证]])。

---

## 新 M2 — Thinker 四类缺口分类

**原理**:Thinker 探测后产生的每个缺口,必属四类之一,且每类有明确的解决方归属。

**形式化**:

```
gap ∈ {mismatch, knowledge_gap, action_gap, infra_gap}
resolver(gap) := {
    mismatch      → ThinkerRepair,
    knowledge_gap → Searcher,
    action_gap    → user,
    infra_gap     → infrastructure
}
```

**应用**:Planner 不直接处理缺口,ThinkerRepair 不处理 knowledge_gap,Searcher 不处理 action_gap — **职责严格分离**(详见 [[05-Thinker环境探测]])。

---

## 新 M3 — ThinkerRepair 三闸门完备性

**原理**:任何自动执行的自修命令,必须依次通过 Gate A(安全分类)、Gate B(执行上限)、Gate C(验证)。

**形式化**:

```
auto_repair(cmd) := (
    gate_a_classify(cmd) = safe_pip ∨ safe_system
) ∧ (
    count(repairs) ≤ max_auto_repairs
) ∧ (
    verify_installation(pkg)
)
```

**应用**:manual_only 类(`sudo`/`docker`/`kubectl`/`systemd`)即使满足 Gate B/C 也**必须**通知用户(详见 [[06-ThinkerRepair自修闸门]])。

---

## 新 M4 — Auditor 四维独立性

**原理**:Auditor 的安全、合规、质量、恢复四个维度互不替代,每个维度独立打分。

**形式化**:

```
verdict ∈ {pass, warn, fail}
audit(task) := (security, compliance, quality, recovery)
            where each ∈ verdict

required: security = pass ∧ compliance = pass
threshold: quality.score ≥ 0.7
trigger:   recovery = fail → rollback(task)
```

**应用**:Gate C(安全边界)是硬约束,通过 [[01-MetaKing调度中枢]] 的 Gate 三步强制;质量阈值 0.7 与老 KING D2 验收一致(详见 [[07-Auditor四维审查]])。

---

## 新 M5 — 因果链追溯

**原理**:每次决策写入因果链,保证可追溯。

**形式化**:

```
C = { (input, action, output, timestamp) | action ∈ Actions }
```

**应用**:调试、审计、回滚;详见 [[10-因果链追溯]]。

---

## 新 M6 — RRF 融合排序

**原理**:多路检索结果融合。

**形式化**:

```
score(d) = Σ_{r ∈ R} 1 / (k + rank_r(d)), where k = 60
```

**应用**:三路混合检索(rg+FTS+vector)融合。**从老 KING 继承,但作用域扩展到 PatternBank 候选池的多次验证融合**。

---

## 新 M7 — 衰减机制

**原理**:30d 未命中降级,90d 清除。

**形式化**:

```
if last_hit(t) + 30d < now → demote(t)
if last_hit(t) + 90d < now → delete(t)
```

**应用**:PatternBank 候选池与正式规则的双重维护(详见 [[03-PatternBank与记忆]])。失败 ≥ 3 次立即 demote(不等 30d)。

---

## 存储结构

```sql
-- PostgreSQL schema
CREATE TABLE mechanism_endorsements (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  formal_definition TEXT,
  application TEXT,
  source VARCHAR(50) DEFAULT 'meta-king',  -- 改 source
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE pattern_bank_candidates (
  id SERIAL PRIMARY KEY,
  rule_json JSONB NOT NULL,
  success_count INT DEFAULT 0,
  failure_count INT DEFAULT 0,
  last_hit_at TIMESTAMP,
  status VARCHAR(20) DEFAULT 'candidate',  -- candidate / promoted / deprecated / deleted
  created_at TIMESTAMP DEFAULT NOW()
);
```

## 关联

- 上游: [[01-MetaKing调度中枢]]
- 同级: [[10-因果链追溯]] (M5 详细展开)
- 下游: [[11-部署拓扑总览]]