# System Prompt 引用与依赖图

> 所有 System Prompt 文件的完整引用关系、依赖链路、版本映射。

## 一、System Prompt 文件清单

### 核心 Prompt 文件

| 文件 | 路径 | 类型 | 角色 |
|------|------|------|------|
| `system-prompt.md` | 01-trae-intelligence/ | 基础定义 | 全局角色、安全约束 |
| `system-prompt-enhanced.md` | 01-trae-intelligence/ | 增强版 | 全能力叠加 |
| `MASTER_AGENT_ARCHITECTURE.md` | 01-trae-intelligence/ | 架构概述 | 架构总览 |
| `TRAE_ENHANCED_SUMMARY.md` | 01-trae-intelligence/ | 增强摘要 | 能力摘要 |
| `CALL_SCENARIOS.md` | 01-trae-intelligence/ | 调用场景 | 场景映射 |
| `system-validation.md` | 01-trae-intelligence/ | 验证报告 | 131 测试验证 |

### 规则文件

| 文件 | 路径 | 类型 | 角色 |
|------|------|------|------|
| `agent-rules.md` | 01-trae-intelligence/rules/ | Agent 规则 | 行为约束 |
| `core-rules.md` | 01-trae-intelligence/rules/ | 核心规则 | 安全边界 |
| `decode-rules.md` | 01-trae-intelligence/rules/ | 解码规则 | 格式规范 |

### 路由文件

| 文件 | 路径 | 类型 | 角色 |
|------|------|------|------|
| `dispatch-hub.md` | 01-trae-intelligence/ | 分发枢纽 | 任务分发 |
| `hook-loop.md` | 01-trae-intelligence/ | Hook 循环 | 工程约束 |
| `respiratory-system.md` | 01-trae-intelligence/ | 呼吸管道 | 生命周期 |

### Agent 定义

| 文件 | 路径 | 类型 | 角色 |
|------|------|------|------|
| `agent-list.md` | 01-trae-intelligence/agents/ | Agent 列表 | 20+ 定义 |
| `agent-definitions.md` | 01-trae-intelligence/agents/ | Agent 定义 | 详细规范 |
| `integrated-agent.md` | 01-trae-intelligence/agents/ | 集成 Agent | 协同模型 |

### Skill 定义

| 文件 | 路径 | 类型 | 角色 |
|------|------|------|------|
| `skill-hub.md` | 01-trae-intelligence/skills/ | 技能中心 | 路由入口 |
| `skill-registry.md` | 01-trae-intelligence/skills/ | 技能注册 | 注册中心 |
| `skills-index.md` | 01-trae-intelligence/skills/ | 技能索引 | 索引一览 |

### SKILL 定义文件（8 个核心）

| 文件 | 路径 | 工具数 | 路由 |
|------|------|--------|------|
| `cc-switch-SKILL.md` | 02-skills-definitions/ | 10 | B/C |
| `token-stats-SKILL.md` | 02-skills-definitions/ | 6 | B/C |
| `master-engineer-hub-SKILL.md` | 02-skills-definitions/ | 6 | 网关 |
| `lso-os-SKILL.md` | 02-skills-definitions/ | — | 基础设施 |
| `omnipotent-engineer-SKILL.md` | 02-skills-definitions/ | — | 应用层 |
| `路由skill-SKILL.md` | 02-skills-definitions/ | — | 路由引擎 |
| `ai-coding-optimizer-SKILL.md` | 02-skills-definitions/ | 5 | 工具层 |
| `trae-auto-collaboration-SKILL.md` | 02-skills-definitions/ | 5 | 桥接 |

## 二、完整依赖关系图

```
system-prompt.md（基础）
  │
  ├── [direct] → core-rules.md（安全边界）
  ├── [direct] → agent-rules.md（Agent 行为）
  ├── [direct] → decode-rules.md（格式规范）
  │
  ├── [enhance] → system-prompt-enhanced.md（增强版）
  │    │
  │    ├── [direct] → MASTER_AGENT_ARCHITECTURE.md
  │    │    ├── [direct] → dispatch-hub.md（分发枢纽）
  │    │    ├── [direct] → hook-loop.md（Hook 循环）
  │    │    └── [direct] → respiratory-system.md（呼吸管道）
  │    │
  │    ├── [direct] → CALL_SCENARIOS.md（调用场景）
  │    │
  │    ├── [direct] → system-validation.md（验证报告）
  │    │
  │    ├── [direct] → skill-hub.md（技能中心）
  │    │    ├── [direct] → skill-registry.md（技能注册）
  │    │    │    ├── [direct] → skills-index.md（技能索引）
  │    │    │    │
  │    │    │    ├── [load] → cc-switch-SKILL.md
  │    │    │    │    ├── [tool] → ccs_provider_list
  │    │    │    │    ├── [tool] → ccs_provider_switch
  │    │    │    │    ├── [tool] → ccs_mcp_sync
  │    │    │    │    └── [tool] → ccs_skills_sync
  │    │    │    │
  │    │    │    ├── [load] → token-stats-SKILL.md
  │    │    │    │    ├── [tool] → token_stats_query
  │    │    │    │    ├── [tool] → token_stats_compare
  │    │    │    │    ├── [tool] → token_stats_budget
  │    │    │    │    └── [tool] → token_stats_audit
  │    │    │    │
  │    │    │    ├── [load] → master-engineer-hub-SKILL.md
  │    │    │    │    ├── [gateway] → 路由决策
  │    │    │    │    ├── [gateway] → Agent 视野隔离
  │    │    │    │    └── [gateway] → 假借修真
  │    │    │    │
  │    │    │    ├── [load] → lso-os-SKILL.md
  │    │    │    │    ├── [infra] → 五境递进
  │    │    │    │    └── [infra] → 20+ Agent 集群
  │    │    │    │
  │    │    │    ├── [load] → omnipotent-engineer-SKILL.md
  │    │    │    │    └── [app] → 七维全栈开发
  │    │    │    │
  │    │    │    └── [load] → 路由skill-SKILL.md
  │    │    │         └── [router] → 三体合一
  │    │    │
  │    │    ├── [direct] → agent-list.md（Agent 列表）
  │    │    ├── [direct] → agent-definitions.md（Agent 定义）
  │    │    └── [direct] → integrated-agent.md（集成 Agent）
  │    │
  │    └── [direct] → SYSTEM_ARCHITECTURE.md（系统架构）
  │
  └── [direct] → MCP 配置（03-mcp-configs/）
       └── [load] → project-mcp-config.json
            ├── cc-switch MCP Server
            ├── token-stats MCP Server
            └── 其他 MCP Server
```

## 三、system-prompt-enhanced.md 的完整依赖栈

```
system-prompt-enhanced.md 依赖栈（按加载顺序）
  │
  1. system-prompt.md（基础）
  2. core-rules.md（安全规则）
  3. agent-rules.md（Agent 规则）
  4. MASTER_AGENT_ARCHITECTURE.md（主架构）
  5. dispatch-hub.md（分发枢纽）
  6. hook-loop.md（Hook 循环）
  7. respiratory-system.md（呼吸管道）
  8. CALL_SCENARIOS.md（调用场景）
  9. system-validation.md（验证报告）
  10. skill-hub.md（技能中心）
  11. skill-registry.md（技能注册）
  12. skills-index.md（技能索引）
  13. agent-list.md（Agent 列表）
  14. agent-definitions.md（Agent 定义）
  15. integrated-agent.md（集成 Agent）
  16. SYSTEM_ARCHITECTURE.md（系统架构）
  ─────────────────────────────────
  运行时按需加载：
  17. cc-switch-SKILL.md（当需要 Provider 切换时）
  18. token-stats-SKILL.md（当需要 Token 审计时）
  19. master-engineer-hub-SKILL.md（当需要路由决策时）
  20. lso-os-SKILL.md（当需要五境执行时）
  21. omnipotent-engineer-SKILL.md（当需要开发时）
  22. 路由skill-SKILL.md（当需要三体路由时）
```

## 四、引用统计

| 文件 | 被引用次数 | 引用者 |
|------|-----------|--------|
| system-prompt.md | 3 | enhanced, MASTER, SYSTEM_ARCHITECTURE |
| system-prompt-enhanced.md | 2 | MASTER, CALL_SCENARIOS |
| skill-hub.md | 2 | enhanced, skills-index |
| skill-registry.md | 3 | skill-hub, skills-index, SYSTEM_ARCHITECTURE |
| cc-switch-SKILL.md | 2 | skill-registry, master-engineer-hub |
| token-stats-SKILL.md | 2 | skill-registry, master-engineer-hub |
| master-engineer-hub-SKILL.md | 2 | skill-registry, skills-index |
| lso-os-SKILL.md | 2 | skill-registry, skills-index |
| hook-loop.md | 2 | enhanced, MASTER |
| dispatch-hub.md | 2 | enhanced, MASTER |
| agent-rules.md | 2 | system-prompt, enhanced |
| core-rules.md | 2 | system-prompt, enhanced |