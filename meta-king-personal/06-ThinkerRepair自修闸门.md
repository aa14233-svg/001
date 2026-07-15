---
tags: [meta-king, repair, gates]
version: 1.0.0
created: 2026-07-16
replaces: 06-功能开关.md
---

# 06 ThinkerRepair 自修闸门

> Thinker 检测到环境缺口后,Builder 获得蓝图前,先过三级闸门。

## Gate A — 安全分类

| 修复类型 | 判定条件 | 处理 |
|----------|---------|------|
| `safe_pip` | `pip install` 不含 sudo | 自动执行 |
| `safe_system` | `yum/apt` 不含 sudo | 自动执行 |
| `manual_only` | 含 `sudo` / `docker` / `kubectl` / `systemd` | 跳过 → 通知用户 |

## Gate B — 执行上限

单次最多执行 `max_auto_repairs` 条修复(默认 5 条)。超出部分自动降级为手动操作项。

## Gate C — 验证

修复执行后,重新读取 `importlib.metadata` 确认包已安装。

## 输出示例

```json
{
  "summary": {"total": 3, "executed": 2, "skipped": 0, "failed": 1},
  "gates": {
    "a_classified": "safe=3 manual=0",
    "b_capped": "3/3 auto-queued",
    "c_verified": "2/2 confirmed"
  },
  "repairs": [
    {"tool": "redis-py", "command": "pip install redis", "status": "executed", "verified": true},
    {"tool": "uvicorn",  "command": "pip install uvicorn", "status": "executed", "verified": true},
    {"tool": "numpy",    "command": "pip install numpy",   "status": "failed",  "hint": "检查包名或网络连接"}
  ],
  "instructions_for_user": []
}
```

## 完整数据流

```
Planner DAG
    │
    ▼
┌──────────────────────┐
│  MetaKing 执行环境探测 │  → env_snapshot
│  (shell 命令)         │
└──────────────────────┘
    │
    ▼
Thinker.probe(dag, env_snapshot, hearing_answers)
    │
    ├─ 产出 enhance_dag(含对账表)
    │
    ├─ 有 self_repairs → ThinkerRepair 执行自修闸门 (Gate A/B/C)
    │       │
    │       ├─ 修复成功 → 跳过 Searcher,直接交 Builder
    │       │
    │       └─ 修复失败 → 记录需手动操作项,交用户
    │
    ├─ 有 search_needs → MetaKing 路由 Searcher 补查 → 追加到 DAG
    │
    └─ 无 search_needs → 直接交给 Builder
```

## 与老 KING 功能开关的对照

| 老 KING v3.0 (功能开关) | MetaKing v1.0 (ThinkerRepair) |
|------------------------|-------------------------------|
| 三层次 Feature Flags(static / dynamic / runtime) | 三闸门(safe/manual_only + execution cap + verification) |
| 87 个静态开关(由开发者维护) | 动态缺口列表(由 Thinker 探测产出) |
| 不涉及系统调用 | Gate A 分类决定能否自动执行系统命令 |

老 KING 的 Feature Flags 是"配置管理",MetaKing 的 ThinkerRepair 是"自愈执行" — **目标不同,不互斥**。可在 MetaKing 之上层叠老 KING 的开关机制(详见 [[13-Claude-Code-蒸馏]] 中的兼容层设计)。

## 关联

- 上游: [[05-Thinker环境探测]]
- 下游: [[04-Builder任务生产链]]
- 同级: [[07-Auditor四维审查]]