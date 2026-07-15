---
tags: [meta-king, thinker, environment, gap-analysis]
version: 1.0.0
created: 2026-07-16
replaces: 05-提示词引擎.md
---

# 05 Thinker 环境探测

> Thinker 接在 Planner 拆解之后。Planner 给出了"做什么"(DAG),Thinker 回答"能不能做、缺什么"。

## Planner vs Thinker

| 维度 | Planner | Thinker |
|------|---------|---------|
| 交互对象 | 用户(对话) | 系统(探测) |
| 输入 | 用户意图 | Planner DAG + 环境快照 |
| 输出 | 任务 DAG | 需求-环境对账表 + 缺口清单 |

## 环境快照(env_snapshot)

MetaKing 在调用 Thinker 前执行系统探测,产出环境快照:

```python
env_snapshot = {
    "os": "TencentOS Server 4.6",
    "python": {"version": "3.11.6", "path": "/usr/bin/python3.11"},
    "shell_tools": {"git": "2.43.0", "docker": "26.1.3(需 sudo)", "kubectl": None},
    "python_packages": ["fastapi==0.115.4", "sqlalchemy==2.0.36", "redis-py==5.2.1"],
    "env_vars": {"DB_HOST": "已设置", "REDIS_URL": "未设置", "API_KEY": "已设置"},
    "network": {"redis:6379": "可达", "postgres:5432": "未测试"},
}
```

## 需求对账逻辑

Thinker 拿到 Plan DAG + env_snapshot + Planner 听证全量回答后,对每个节点做:

| 步骤 | 说明 |
|------|------|
| 需求推断 | 听证约束 > scope 排除 > DAG 关键词 > 默认最小集 |
| 环境比对 | n+1 维对账:shell_tools / runtime_deps(含 Python 语义版本比对) / external_services / credentials |
| 缺口分类 | 四类按解决方分工(见下) |
| Searcher 触发 | `knowledge_gap` 自动生成 `search_needs`,`action_gap` 和 `infra_gap` 不浪费 Searcher 配额 |

**听证穿透**:`hearing_answers` 传入完整 5 维回答(scope/format/constraint/done_criteria/stakeholders),Thinker 读取 scope 中的排除项("不需要 Docker"/"不含前端")滤除无关工具,从 constraint 提取显式需求,再叠加 DAG 节点关键词规则。

## 四类缺口

| 缺口类型 | 解决方 | 是否生成 search_needs | 示例 |
|----------|--------|----------------------|------|
| **mismatch** | ThinkerRepair 自修 | 否 | `pip install redis` |
| **knowledge_gap** | Searcher 查 | **是** | `"PostgreSQL 15 installation guide TencentOS"` |
| **action_gap** | 用户动手 | 否 | `export DB_URL=<value>` |
| **infra_gap** | 基础设施 | 否 | `Python>=3.10 缺失(当前 3.8)`、Redis 未运行 |

对账输出 JSON 模板:

```json
{
  "node_id": "impl_core",
  "match": {
    "satisfied":    ["Python 3.11 ✓ (≥3.x)", "fastapi ✓", "pytest ✓"],
    "mismatch":     [{"need": "redis-py", "have": "未安装", "fix": "pip install redis"}],
    "knowledge_gap":[{"need": "安装 PostgreSQL", "have": "需部署", "fix_hint": "Searcher 查找安装指南"}],
    "action_gap":   [{"need": "DB_URL", "have": "未设置", "fix_hint": "export DB_URL=<value>"}],
    "infra_gap":    [{"need": "Redis", "have": "未运行或不可达"}]
  },
  "blocking_risk": "高 — 2 个基础设施缺口;2 个需用户操作",
  "search_needs": ["PostgreSQL 15 installation guide TencentOS Server 4.6"]
}
```

## Python 版本真比对

`runtime_deps` 中的 Python 需求不再无条件标记 satisfied。Thinker 从 env_snapshot 读取实际版本,与约束中的 `>=X.Y` 做语义版本比较;低于要求时归入 `infra_gap`。

## 与老 KING 提示词引擎的对照

| 老 KING v3.0 (提示词引擎) | MetaKing v1.0 (Thinker) |
|--------------------------|--------------------------|
| XML 模板引擎 + 条件渲染 + 版本管理 | 需求-环境对账,产出缺口清单 |
| 静态模板,提前渲染 | 动态探测,运行时对账 |
| 不与系统环境交互 | env_snapshot 是必填输入 |

老 KING 的"提示词引擎"被 MetaKing 完全删除 — **在 LLM 时代,XML 模板的条件渲染已经被结构化输出(JSON schema)和 function calling 取代**。

## 关联

- 上游: [[02-Planner三步听证]] (DAG 输出)
- 下游: [[06-ThinkerRepair自修闸门]] [[07-Auditor四维审查]]
- 同级: [[04-Builder任务生产链]]